**大模型与 Agent 质量简报｜2026-11-09**

观察窗口：北京时间2026-11-08 09:32至2026-11-09 09:32；归档前再次核对了各项issue/PR的开放与合并状态。本期6条，均在窗口内公开，已与[11月8日简报](https://github.com/BruceShenC/News/blob/main/daily/2026-11-08/llm-agent-quality.md)去重。重点是量化加载、后端语义、适配器转换、分布式算子、损失边界及新硬件dispatch的可验证测试。

全部运行数字来自作者公开报告。本简报核查公开issue、PR、源码/补丁、复现材料及可见CI状态，未安装运行相关项目，未自行复现。以下日期均为北京时间；作者本地测试、CI执行、合并和发布分别说明。

**1. SGLang：量化MoE router被静默漏载，GSM8K从正常水平降至0%**

问题于11月9日00:42公开。Qwen3-MoE仅为ModelSlim router传入量化配置，其他配置按普通Linear创建；W4A16 checkpoint的weight_packed、weight_scale、weight_shape找不到对应参数后被直接跳过，启动仍然成功。FP8 router还可能只复制codes而漏掉scale。修复范围随后扩展至Mixtral与Llama 4。

- **环境与复现**：SGLang v0.5.23，main 4003bf1存在同路径；1×H20-3e/SM90、CUDA 13.0、PyTorch 2.14.1。模型为RedHatAI/Qwen3-30B-A3B-Instruct-2507-quantized.w4a16。原帖提供完整启动命令、curl请求及无需GPU的8个checkpoint索引检查脚本；简单首都问题只返回反斜杠或换行。
- **正确性判定与结果**：先检查checkpoint实际tensor格式，再通过真实loader加载layer 0 router，与checkpoint反量化权重的参考输出比较，最后做任务评测。基线128个router输出均NaN；修复后的层相对误差为0.0056。GSM8K共1,319题、5-shot、greedy：原版0.0%，作者修复85.7%，vLLM 0.33.0对照85.4%。
- **修复与反例**：PR #49187按quant config创建router，并在实际权重为BF16/FP16/FP32时重建普通router。这个回退不可省略：第一版只信config，使实际存BF16的AngelSlim checkpoint产生NaN；第二版恢复至0.0017相对误差。作者报告7项CPU单测通过，并提供10个checkpoint的层级对照。
- **CI与状态**：PR开放、未合并，head 3291e4e。标题检查与Lint通过；Base及Extra因缺少run-ci标签在门禁失败，后续计算测试跳过。PR正文的沙漏状态已落后于实际日志，不能写成“GPU CI已通过”或“仍在运行”。
- **限制与迁移测试**：10个checkpoint的层级检查不等于10个完整模型评测；不支持的量化格式可能从静默错误变为显式拒绝。应交叉覆盖“配置声明”与“实际tensor格式”，将加载完整性、层级数值和统一设置的端到端准确率连起来，不能只验服务启动与健康检查。

来源：[原始复现](https://github.com/sgl-project/sglang/issues/49185)；[修复、反例与准确率](https://github.com/sgl-project/sglang/pull/49187)；[router构造代码](https://github.com/sgl-project/sglang/blob/4003bf1077debcca3fccfa6e0d7b56ed584aa727/python/sglang/srt/models/qwen3_moe.py#L268-L284)；[静默跳过路径](https://github.com/sgl-project/sglang/blob/4003bf1077debcca3fccfa6e0d7b56ed584aa727/python/sglang/srt/models/qwen3_moe.py#L1246-L1256)；[CI门禁日志](https://github.com/sgl-project/sglang/actions/runs/42543881185/job/128924724105)。

**2. vLLM：BatchedTritonExperts吞掉两处bias，正确输出1却返回0**

问题于11月9日00:54公开。后端选择器接受has_bias=True，但batched路径的两次GEMM没有消费w1_bias/w2_bias。用零矩阵隔离bias贡献，即可把这一静默错误与普通矩阵乘法误差分开。

- **环境与复现**：main 83630ab85eb27e456d99008cc0a27e025e73c0b4的Python源码覆盖已有native扩展；RTX 4090/SM89、BF16权重与激活、PyTorch 2.14.0+cu130、Python 3.12。公开Gist含repro_batched_bias.py、baseline.json、fixed.json及候选补丁。脚本调用真实GPU kernel和本地BatchedPrepareAndFinalize，无需下载模型，不是mock kernel。
- **正确性判定与结果**：相同输入与routing weights分别送普通TritonExperts和batched路径，并与解析答案比较。零权重、down-bias=1且路由权重和为1时，预期1，普通Triton为1，batched为0；零W1、up-bias=1、单位W2时，BF16 SiLU预期0.73046875，batched仍为0。零bias对照正常。
- **修复与测试**：本地候选补丁把bias及stride送入GEMM epilogue，在输出cast前相加。新增三组preserve_bias测试，包括跨expert、跨输出列变化的bias：原版2失败/1通过，补丁3通过；作者报告完整test_batched_moe.py为477通过、508跳过，Ruff及格式检查通过。跳过项不能算覆盖成功。
- **CI与限制**：issue仍开放，本次未检得对应上游PR；只有公开本地补丁与运行结果，没有上游CI通过证据。完整模型服务、多rank EP、XPU和性能均未验证。
- **迁移测试**：对每个宣称支持bias的后端加入零权重、独立up/down bias及异构expert bias的解析oracle。只用随机权重或零bias，容易掩盖后端间语义缺项。

来源：[问题、环境与修前修后结果](https://github.com/vllm-project/vllm/issues/66404)；[完整复现、结果JSON与候选补丁](https://gist.github.com/SyaOtiLan/d021c2340b17c65e4fe7562c4eebe230)；[原版GEMM调用](https://github.com/vllm-project/vllm/blob/83630ab85eb27e456d99008cc0a27e025e73c0b4/vllm/model_executor/layers/fused_moe/experts/fused_batched_moe.py#L1128-L1191)。

**3. llama.cpp：rsLoRA转GGUF遗漏缩放，r=64只保留12.5%的适配器强度**

issue于11月8日17:08公开，PR于17:20提交。rsLoRA的有效缩放应为alpha/√r；转换器却把alpha原样写入GGUF，而运行时按alpha/r应用，因此适配器更新被静默弱化。

- **环境与复现**：Linux、master d6be065；问题在Python转换器，无特定GPU要求。公开脚本无需下载模型，创建微型Llama配置及r=16、alpha=16的rsLoRA，运行convert_lora_to_gguf.py后读取GGUF元数据。
- **正确性判定与结果**：直接比较GGUF中的alpha/r与PEFT规范的alpha/√r。r=16时应为4，实际1；r=32仅约17.7%的预期强度，r=64仅12.5%。文件能生成、能加载并不能验证有效更新量。
- **质量影响的证据边界**：作者以SmolLM2-135M-Instruct、Alpaca训练200步、3个seed，在PEFT内比较正确与错误缩放。r=16的held-out loss为1.543对1.719，r=64为1.539对1.801。这是PEFT模拟错误公式的实验，不能写成llama.cpp端到端评测。
- **修复与回归**：PR #31882在use_rslora=true时写入alpha×√r，普通LoRA不变。新测试转换合成adapter并回读GGUF：原版普通LoRA通过、rsLoRA失败，补丁两项通过。若rank_pattern包含混合rank，单一alpha无法精确覆盖所有模块，目前仅警告，不能视为完整支持。
- **CI与状态**：PR开放、未合并，head 14bbef2。两个标签管理任务成功，但Python类型检查、flake8、Behavior Check等6个workflow为action_required；不能把管理任务成功算成功能CI通过。
- **迁移测试**：跨框架转换应验证实际公式和有效更新量，同时保留普通LoRA对照，分别覆盖统一rank与rank_pattern；不要只比较张量shape和输出文件存在。

来源：[最小复现与质量实验](https://github.com/ggml-org/llama.cpp/issues/31879)；[候选修复与回归](https://github.com/ggml-org/llama.cpp/pull/31882)；[转换器](https://github.com/ggml-org/llama.cpp/blob/d6be065572cdb7bf64014c8c5771e210c9570e44/convert_lora_to_gguf.py#L531-L542)；[运行时缩放公式](https://github.com/ggml-org/llama.cpp/blob/d6be065572cdb7bf64014c8c5771e210c9570e44/src/llama-adapter.h#L52-L57)；[类型检查状态](https://github.com/ggml-org/llama.cpp/actions/runs/42466757603)。

**4. DTensor：先位重解释、再归约，两个rank可以一致地算错**

问题于11月9日01:58公开。普通分片矩阵乘法自然产生Partial(sum)，随后view(int32)错误保留待求和状态；物化全局结果时，相加的是各rank的整数位模式，而非先求出全局浮点值再重解释。

- **环境与复现**：torch 2.16.0.dev20261108+cpu；Windows 11 Home 10.0.26300、Python 3.14.3、Intel Core Ultra 9 288V；两CPU进程、Gloo、FileStore。原帖完整repro.py可用python repro.py运行。a为1×4全1并按列分片，b=[0.5,0.5,1,1]ᵀ并按行分片，无需人为构造Partial。
- **正确性判定与结果**：以“先full_tensor、再view(int32)”为参考，对照“先view(int32)、再full_tensor”。全局浮点结果应为3.0，其int32位模式为1077936128；实际两rank都得到2139095040，即本地1.0与2.0的整数位模式相加。不是浮点容差，也不是rank间不一致。
- **定位与临时规避**：aten.view.dtype复用propagate_single_input_single_dim_strategy，允许Partial(sum)直接传播，但位重解释不与求和交换。显式redistribute至Replicate后再view得到正确值。
- **修复、CI与限制**：作者称候选strategy修改和两个回归测试在已安装nightly上通过，但未做完整源码构建；本次未见公开关联PR或CI。隐式重分布虽可修数值，却可能改变view的本地storage别名语义，需维护者决定是重分布还是拒绝不兼容输入。
- **迁移测试**：分布式算子应以全局eager语义作oracle，同时检查placement和别名。仅断言各rank一致会漏报；对不能与归约交换的操作，应明确验证先后顺序。

来源：[完整CPU复现、数值、候选方案及别名限制](https://github.com/pytorch/pytorch/issues/203981)。

**5. KL loss：masked类别把数学上为0的KL变成NaN，CI成功也不等于可合并**

issue于11月8日19:37公开，修复PR于11月9日04:50提交。零概率target与-inf log-input相遇时，非log-target分支的target×input产生0×(-inf)；log-target分支也存在exp(-inf)乘无穷差值的问题。

- **环境与复现**：Windows 11、torch 2.16.0.dev20261104+cpu、commit 7f24a19c0967db04f2d5ba44c1e56eca489932ec、Python 3.14.7。作者还报告20261102 CPU nightly和Tesla T4复现，但CUDA软件版本不完整，不能当成完整环境矩阵。
- **正确性判定与结果**：logits=[-inf,0,-inf]、target=[0,1,0]，先log_softmax再计算KL。相同退化分布的KL应为0；实际reduction=none得到[NaN,0,NaN]，sum、mean、batchmean均为NaN；有限log-input对照为0。nn.KLDivLoss及log_target=True亦报告同类问题。
- **修复与回归**：PR #203987在Loss.cpp中先mask乘法操作数，再计算，而不是事后把NaN替换为零。common_nn新增masked_zero和masked_log_target两例，以zeros_like为参考，覆盖两条分支。新测试明确使用reduction=none且关闭forward AD支持，不能声称新增用例已覆盖全部归约或全部自动微分路径。
- **CI与状态**：PR开放、非Draft、未合并，head 0720cd49。151项check-runs中147 success、3 skipped、1 neutral；pull、Lint及BC Lint成功，可见CUDA构建成功，但本次未核实专门GPU执行该回归测试的日志。另有独立commit status因缺少CLA授权而failure，也仍缺actionable issue/维护者支持，不能概括为“全部检查通过、可合并”。
- **限制与迁移测试**：作者未在本地完成完整native构建/测试。蒸馏与掩码损失的回归应覆盖零概率、-inf、逐元素值、各归约及梯度；本期已有证据支持前向边界修复候选，不等于所有梯度边界已获验证。

来源：[原始脚本与环境](https://github.com/pytorch/pytorch/issues/203967)；[修复PR](https://github.com/pytorch/pytorch/pull/203987)；[实际补丁与测试范围](https://github.com/pytorch/pytorch/pull/203987/files)；[pull工作流](https://github.com/pytorch/pytorch/actions/runs/42522151852)；[Lint](https://github.com/pytorch/pytorch/actions/runs/42522151882)；[CLA阻塞](https://github.com/pytorch/pytorch/pull/203987#issuecomment-6726948274)。

**6. RMSNorm：B300能运行，却没走已调优的backward与weight-only分流**

issue与PR于11月8日22:29公开。架构门控只接受sm_90/sm_100，sm_103因此落回quack默认grid，weight-only也总走native kernel。这里发现的是硬件dispatch与性能覆盖缺口，不能写成数值错误。

- **环境与方法**：torch 2.16.0.dev20261102+cu130、CUDA 13.0、NVIDIA B300/sm_103。作者用profiler扫N∈{128,512,1024,2048,4096,8192}、M=256…65536、FP16/BF16/FP32及两种梯度需求，每种操作162格。主线与PR结果来自同GPU、同nightly的两个session。
- **判定依据与结果**：用实测native/ATen交叉点决定分流，并检查真正执行的kernel。BF16 256×1024 weight-only由4.36降至2.51µs，8192×128由9.20降至5.38µs。weight-only有125/162格快于3%，但2格变慢；input+weight有61/162格改善、6格变慢，不能写成普遍加速或零回归。
- **修复与回归**：PR #203972将门控集中为TUNED_ARCHS并加入(10,3)，设置B300专用阈值表。不能直接沿用sm_100：例如N=128时，B300的native胜出阈值为1024行，sm_100为8192行。新增11个阈值边界case×3 dtype，以profiler核对实际dispatch。
- **作者本地测试**：test_rmsnorm_cutedsl.py为192通过、28跳过，主线为159通过、28跳过；另有routing/numerics测试78通过。前一组使用TORCH_DISABLE_NATIVE_AOT=1和TORCH_NATIVE_SKIP_VERSION_CHECK=1。未跑sm_90/sm_100，也未跑spin fixlint；issue正文未附完整基准脚本，复现材料不如前几例完整。
- **CI与状态**：PR为Draft、未合并，head f2bf4d42。Dr.CI列出6个workflow等待批准，包括native-AOT kernel CI、pull和Lint；h100/b200 ciflow尚未触发。“270通过”是两组作者本地测试之和，不是CI结果。
- **迁移测试**：新GPU验收应核对compute capability对应的真实dispatch，在阈值前后设置边界用例，并保留负向性能格。相邻架构不等于相同最佳阈值；“kernel能跑”也不等于调优路径生效。

来源：[问题与环境](https://github.com/pytorch/pytorch/issues/203971)；[基准、负向格及本地测试](https://github.com/pytorch/pytorch/pull/203972)；[实际补丁](https://github.com/pytorch/pytorch/pull/203972/files)；[CI待批准状态](https://github.com/pytorch/pytorch/pull/203972#issuecomment-6722722955)。

**本期可直接迁移的检查**

1. 把可执行的oracle放在回归核心：零权重隔离bias、转换前后有效缩放、先全局归约再操作、零概率KL、实际dispatch与性能交叉点。输出有限、shape正确、各rank一致都不充分。
2. 分开记录配置与真实数据：量化config和checkpoint tensor格式、统一rank与混合rank、后端声明的能力与kernel实际消费的参数，都应做交叉测试。
3. CI报告至少区分作者本地通过、跳过、门禁失败、待批准、正式计算测试成功及独立CLA阻塞。本期所有列出的候选PR均未合并。
4. **AI辅助测试有具体工程披露，但没有效率因果证明**：llama.cpp PR说明AI助手编写测试并运行flake8/类型检查，作者逐行复核并执行修前/修后测试；vLLM issue披露AI参与调查、原型和测试执行；RMSNorm PR披露Claude Code参与代码、测试与所列运行。可借鉴的是最小反例、解析oracle、修前失败/修后通过和负向结果保留，不能仅因“使用AI”就认定质量更高、效率提升或已稳定落地生产CI。

归档：[本期完整简报](https://github.com/BruceShenC/News/blob/main/daily/2026-11-09/llm-agent-quality.md)；[按主题查看历史](https://github.com/BruceShenC/News/blob/main/topics/llm-agent-quality/README.md)。
