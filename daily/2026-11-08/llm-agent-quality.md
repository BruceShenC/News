**大模型与 Agent 质量简报｜2026-11-08**

观察截止：北京时间2026-11-08 09:39。主要窗口为11月7日09:39至11月8日09:39，另补充两项窗口前、近7日内且昨日未收录的案例。本期6条，重点是“能够加载、输出有限、测试变绿”仍可能掩盖的错误。

全部运行数字来自作者公开报告。本简报核查了公开issue、PR、复现材料及可见CI状态，未安装运行相关项目，未自行复现。已与[11月7日简报](https://github.com/BruceShenC/News/blob/main/daily/2026-11-07/llm-agent-quality.md)去重。以下日期均为北京时间；作者实测、候选补丁、CI执行与合并发布分别说明。

**1. vLLM：TP切开FP4量化组，服务正常启动，GSM8K却降至0.30%**

问题于11月7日10:15公开。涉及compressed-tensors NVFP4/MXFP4的row-parallel层及MoE w2；每个rank的列数不能整除16/32的scale group时，codes按元素切分、scales按向下取整的组数切分，造成错配或尾部缺scale。

- **环境与复现**：实模评测用8×H20-3e、vLLM 0.33.0rc1.dev848+g14d147a7a、PyTorch 2.14.0+cu130，模型RedHatAI/gemma-4-26B-A4B-it-NVFP4，revision 5557756b。层级复现用main 8176238c9，公开torchrun双rank脚本无需下载模型。
- **正确性判定**：构造所有FP4 code为+1、各组scale为2的周期幂，输入6倍单位矩阵，逐列反读实际权重，与解析答案精确比较。45组使每个rank分到22.5组，46组为合法对照。
- **作者结果**：NVFP4每rank 360列中，rank 0错8列、rank 1错184列；MXFP4分别错16/720和368/720。完整GSM8K共1,319题、5-shot、greedy、启用chat template时，TP8为0.30%，TP4四次为91.51%、92.80%、92.19%、92.42%，TP2为91.89%；实验性group-aligned TP8为92.34%。原issue的约24%→1.44%来自未用chat template的另一设置，不能混用。
- **修复与CI**：PR #66213是加载时拒绝非法切分的最小安全修复，不是已支持正确的TP8。新增6例从6失败变为6通过，46组对照保持正确；pre-commit成功。测试目录已纳入H200 Quantized Models步骤，但尚未核到该GPU CI执行结果，多卡复现仍是手工运行。PR为Draft、未合并。
- **限制与迁移测试**：PR不修改ModelOpt NVFP4 MoE路径，实模TP8拒绝也未由PR作者在八卡上复测。应把“合法分片必须数值正确”和“非法分片必须明确拒绝”分开验收，覆盖rank 0及非零rank，再用统一prompt/template的任务评测量化影响。

来源：[原始复现](https://github.com/vllm-project/vllm/issues/66211)；[chat-template完整评测](https://github.com/vllm-project/vllm/issues/66211#issuecomment-6700915434)；[候选修复与前后测试](https://github.com/vllm-project/vllm/pull/66213)；[pre-commit运行](https://github.com/vllm-project/vllm/actions/runs/42328449455)。

**2. vLLM：all-reduce没有NaN、也不崩溃，却把四卡通信算成两卡子组**

问题及PR于11月8日03:56公开。启用VLLM_USE_NCCL_SYMM_MEM后，进程级custom op闭包保存了首次注册的PyNCCL communicator，后续调用缺少实际通信组身份，ETP可能误用TP组。

- **环境与复现**：4×RTX 6000D、vLLM f98575ec官方CUDA wheel、PyTorch 2.14.0+cu130、NCCL 2.30.7。TP2、DP2、Engram ETP4，测试本地生成tiny Qwen4Exp checkpoint，实际执行eager LLM.generate，并非替换collective后自测。
- **正确性判定**：为每次观测独立计算ETP4和TP2参考，并要求至少一项能区分两组；每行最多一个非零shard，使求和可精确比较。四卡NCCL window求和及12个有向P2P copy另作控制。
- **作者结果**：基线两次观测均为有限值，却与ETP4不同、与所属TP2完全一致，45.13秒失败；同节点加补丁后43.53秒、1 passed。独立window控制在四个rank上均正确返回15。
- **修复与CI**：PR #66304让op显式接收group identity并在dispatch时解析。pre-commit成功；四卡H100 nightly命令已加入，但执行仍pending。普通两卡及非CUDA路径会skip。PR开放未合并；公开GPU运行在随后新增非CUDA guard和类型转换之前完成，不能把最终head的所有变化都算作同轮GPU验证。
- **限制与迁移测试**：没有模型准确率评测，synthetic checkpoint也没有业务准确率基线。通信回归应使用能区分process group的数值oracle，而不是只断言有限值、shape或完成请求。

来源：[问题报告](https://github.com/vllm-project/vllm/issues/66303)；[完整复现材料](https://gist.github.com/jackLei0901/9885b172cf0465111bc6c28efc461181)；[修复与验证范围](https://github.com/vllm-project/vllm/pull/66304)；[pre-commit运行](https://github.com/vllm-project/vllm/actions/runs/42408673466)。

**3. SGLang：量化排除名单没跟随权重改名，文本正常而视觉失效**

问题于11月7日17:16公开。Gemma3把out_proj加载为proj，但quantization ignore仍用旧名，27个本应BF16的视觉投影被建成FP8；Mistral3同时缺少前缀、o_proj及fused mapping，Pixtral tower被建成NVFP4，BF16权重被静默跳过。

- **环境与复现**：1×H20-3e/SM90、CUDA 13.0；服务运行SGLang 0.5.23，配置复现用main 754185a。公开repro.py只读取两个checkpoint的config.json，无需GPU，沿真实loader逻辑构建quant config并检查should_ignore_layer。
- **正确性判定**：第一层检查配置名称是否命中排除名单；第二层加载真实vision layer 0，与BF16参考输出比较；第三层用两张COCO图片进行greedy、64-token描述，并以20条文本prompt作对照。
- **作者结果**：Gemma3原层出现928个inf，不同图片均被描述为蘑菇；Mistral3视觉权重未加载，图片输出乱码。补丁后层相对误差约0.0016–0.0017，与BF16舍入量级一致，两模型都能描述象群及餐桌人物。文本对照未识别出补丁造成的退化，但Gemma3基线自身存在运行间变化。
- **修复与CI**：PR #49062补齐名称映射与packed_modules_mapping，新增CPU回归。作者报告基线16 failures、1 error，补丁后5 tests OK。Lint成功；Base、Extra、AMD工作流在“Require run-ci label”门禁失败，后续测试跳过，不是数值测试失败或GPU CI通过。PR开放未合并。
- **限制与迁移测试**：两张图片只能验证具体症状消失，不能等同于完整多模态评测。权重名称、量化排除名单、融合层映射应共享一致转换，并把配置断言、层级数值对照及跨模态端到端测试连起来。

来源：[配置脚本与错误输出](https://github.com/sgl-project/sglang/issues/49061)；[修复、层级及服务对照](https://github.com/sgl-project/sglang/pull/49062)；[Lint](https://github.com/sgl-project/sglang/actions/runs/42359237592)；[Base门禁](https://github.com/sgl-project/sglang/actions/runs/42359237670)。

**4. DTensor：分片规则依赖标量条件，官方验证器仍报告0条错误**

近7日补充：issue于11月7日08:16公开，早于本期主窗口，昨日未收录。报告者称在Lean 4中检查pointwise规则时发现，Partial(max/min)跨函数传播要求单调性，而threshold只有在value≤threshold时满足此条件。

- **环境与复现**：CPU、fake process group、LocalTensorMode、2 ranks；报告未填写PyTorch版本，不能补写版本。将[[-1,3],[1,-2]]按行切分，先amax，再threshold(0,5)。
- **正确性判定与结果**：完整张量eager参考应为[1,3]，DTensor却返回[5,5]且没有警告；value=0的默认对照正确。作者运行strategy_validation报告0条错误，即便OpInfo四个样例有三个满足value>threshold。未见可独立核查的Lean证明仓库，形式化发现过程仅按作者披露记录。
- **修复与回归**：PR #203886按标量条件决定策略，不单调时先转Replicate；将相关标量纳入策略缓存键。新增max/min×两种标量关系的四分支测试，同时断言placement及eager数值，作者报告CPU通过。
- **CI与限制**：PR开放、处于pre-review；5个工作流待批准，DTensor/Inductor/TorchTitan相关CI尚未触发，不能写成CI通过。也不能由这个反例推断验证器对所有规则都无效。
- **迁移测试**：分布式算子既检查输出，也检查合法placement；标量会改变传播策略时，连续切换参数验证缓存没有复用旧策略。默认参数测试很容易错过这个边界。

来源：[最小复现与验证器漏检](https://github.com/pytorch/pytorch/issues/203879)；[候选修复与测试计划](https://github.com/pytorch/pytorch/pull/203886)；[回归代码](https://github.com/pytorch/pytorch/pull/203886/files)。

**5. AArch64低精度算子：穷举暴露“更快”路径的跨平台错误，作者披露使用Claude Code**

问题及PR于11月8日03:53公开。环境为GH200的Neoverse V2 CPU、Ubuntu 24.04、PyTorch 2.16.0.dev20261030+cu130，CPU源码构建d60701e也复现。这是CPU算子问题，不是GH200 GPU核缺陷。

- **复现与正确性判定**：BF16输入2.5，SVE128 reciprocal返回0.3984375，同dtype的tensor(1.)/x为0.400390625；rsqrt(1.5)返回0.8125，DEFAULT为0.81640625。通过精确结果ULP、等价表达式、不同CPU capability及全输入输出hash交叉比较，65,280个有限BF16输入中17,252个的两种倒数表达式不一致。
- **定位与修复**：未经细化的倒数估计只有约8位精度，低精度sqrt后再reciprocal又造成双重舍入。PR #203940改为FP32计算后舍入一次，新增ReciprocalReducedFloat穷举所有正常输入，以0.501 ULP为阈值；作者记录修前失败、修后通过，相关最大误差降至0.50 ULP。
- **代价与覆盖**：SVE128 BF16 rsqrt由0.337升至0.519 ns/元素，正确性提升有吞吐代价。SVE256未验证且被排除出新测试，x86 DEFAULT的rsqrt仍未修；完整测试套件还存在修前修后共有的其他失败。PR未合并，Dr.CI“尚无失败”不等于所有CI已经完成并通过。
- **AI辅助测试证据**：issue明确说明测量与初稿由Claude Code辅助完成，PR进一步披露代码、测试及列出的运行也使用了它。这是公开披露的AI辅助测试个案，并有修前/修后及穷举oracle；没有对照实验能证明AI独立发现、比人工更高效，或已在生产CI稳定落地。
- **迁移测试**：低精度有限输入域可穷举时，应优先覆盖完整域，并将正常数、subnormal、特殊值及各dispatch路径分开记录。更快的候选近似即使正常输入通过，也可能在极小subnormal上产生错误符号的inf。

来源：[原始数值、跨平台对照与AI披露](https://github.com/pytorch/pytorch/issues/203939)；[修复、穷举回归、性能与覆盖限制](https://github.com/pytorch/pytorch/pull/203940)。

**6. DTensor checkpoint：非零storage_offset让异步快照准备阶段失败**

近7日补充：下游于11月4日16:36报告，上游PyTorch #203866于11月7日05:40公开，早于本期主窗口，昨日未收录。维护者确认带offset的本地view是合法用例。

- **环境与复现**：下游torch_checkpointing ca688ec44cd55863198ad8400aae917cd63c8703、PyTorch 2.15.0+cpu、Python 3.12.10、2-rank Gloo，pin_memory和share_memory均关闭。脚本分别测试普通Tensor view、零offset DTensor和DTensor.from_local(base[2:6])，上游另给出单进程CPU复现。
- **正确性判定与结果**：要求stage→save→load完整成功，并以rtol=atol=0比较本地值。普通view和零offset DTensor通过，非零offset DTensor在stage即失败。上游进一步展示同步dcp.save可成功、async_save失败，ForkingPickler还可能段错误。
- **根因**：wrapper没有可复制的真实storage，但data_ptr包含offset，示例返回8而非0；调用方用data_ptr≠0判断真实storage存在，因而走错复制/序列化路径。
- **修复与CI**：维护者计划下游窄范围workaround及上游不会抛异常的storage查询接口；本次核查页面未见已关联的修复PR或CI通过证据。不能把“正在修”写成已发布。
- **限制与迁移测试**：已证实的是快照准备失败，不是静默checkpoint损坏，也不能外推所有FSDP/TP任务或其他checkpoint后端。checkpoint测试矩阵应加入非零offset view、同步/异步、不同stager，保存完成后再精确回读，不能只看文件是否生成。

来源：[下游三组对照脚本](https://github.com/meta-pytorch/torch_checkpointing/issues/11)；[维护者确认与修复计划](https://github.com/meta-pytorch/torch_checkpointing/issues/11#issuecomment-6696681775)；[上游诊断与独立CPU复现](https://github.com/pytorch/pytorch/issues/203866)。

**本期可直接迁移的检查**

1. 将“有限值/能启动”升级为可区分错误机制的oracle：通信组参考、量化组解析权重、BF16层级参考、完整张量eager参考。
2. 特别覆盖元数据边界：非零rank、切开的量化组、改名后的ignore、改变策略的标量、非零storage_offset。
3. 保存修复前失败和修复后通过两份证据，分开记录作者运行、CI门禁、skip、pending、全套通过与合并状态。本期所有候选修复均未合并。
4. AI辅助测试本期有明确披露的工程个案：除第5条外，vLLM #66304也说明AI参与实现、测试及验证工具。但未检得足以支持普遍效率提升的新对照研究，不重复昨日论文，也不把“用了AI”当作测试质量证明。

归档：[本期完整简报](https://github.com/BruceShenC/News/blob/main/daily/2026-11-08/llm-agent-quality.md)。
