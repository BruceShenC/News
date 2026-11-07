**大模型与 Agent 质量简报｜2026-11-07**

观察截止：北京时间2026-11-07 09:34。主要窗口为11月6日09:34至11月7日09:34，另收录两项明确标注的近7日补充。本期7条：前6条聚焦AI系统的具体缺陷、复现判定与回归测试，最后1条讨论AI辅助生成测试。

全部运行数字来自原作者公开报告。本简报只核查公开来源，未安装、运行相关项目，也未自行复现。已与[11月6日质量简报](https://github.com/BruceShenC/News/blob/main/daily/2026-11-06/llm-agent-quality.md)去重。下文issue/PR时间均为北京时间；论文日期按来源注明。候选补丁、作者实测、上游CI通过与合并发布分别标记，不互相替代。

**1. SGLang：NVFP4专家错误共用gate/up量化尺度，首版修复还漏掉非零EP rank**

问题与修复PR均于11月7日00:55公开。涉及ModelOpt NVFP4 MoE的Marlin路径，main 0afb736及v0.5.23；作者在H20-3e/SM90、CUDA 13.0、PyTorch 2.14.1上验证。

- **触发与判定**：checkpoint分别保存gate和up的全局scale，但该路径保留gate值并丢掉up值。作者构造4个expert，使输出可反推出实际使用的scale；输入gate/up对为(1,2)、(2,1)、(1,1)、(1,4)，main的up实际值为1、2、1、1，应为2、1、1、4。这是有明确数值答案的复现，不依赖生成文本的主观质量。
- **修复与回归**：PR #48931在GEMM1后、激活前，用fp32将up半部乘以up/gate比例。首个提交把已经本地化的expert id再次映射，令非零EP rank漏修；第二个提交修正，并补rank 1、3测试。作者报告4项测试、687个子测试通过，覆盖scale比例0.1至10及不同token数。
- **实模证据**：8×H20、EP8的同一GLM-5.3-Flash NVFP4模型，WikiText-2困惑度2.47→2.35，GSM8K 88.8%→90.9%；与vLLM Humming的top-1一致率91.6%→96.4%。这些是作者特定环境实测，不能推广为所有量化模型的收益。
- **状态与边界**：PR仍开放未合并；上游Base工作流停在run-ci标签门禁，测试随后跳过，不是GPU测试已通过。LoRA MoE路径仍只警告，compressed-tensors路径仍拒绝不等scale，未给出完整速度回归。
- **可复用方法**：用可解析的权重与输入核对量化元数据，并让分布式回归覆盖非零rank；rank 0的global/local id相同会掩盖修复本身的缺陷。

来源：[原始复现](https://github.com/sgl-project/sglang/issues/48930)；[补丁、测试及实模对照](https://github.com/sgl-project/sglang/pull/48931)；[上游CI运行](https://github.com/sgl-project/sglang/actions/runs/42287326313)。

**2. TorchTitan：FSDP拼接轴不一致，量化前的权重已被静默打乱**

近7日补充：PR #5874于11月6日08:20公开，略早于本期24小时窗口，且昨日未收录。涉及MXFP8Linear/NVFP4Linear的stacked w13权重，形状[2,F,D]、Shard(1)、至少2个FSDP rank。

- **发现与复现**：pre-all-gather将非零分片维移到dim 0，却返回普通tensor；FSDP按参数的分片维拼接，而post-hook假定按dim 0拼接。作者在双rank CPU和2×H100上验证：F=4时，[2,4,3]权重的24个元素中12个错位，且不报错；F=5时触发尺寸异常。
- **修复与判定**：非零分片维返回AllGatherInput(source, dim=0)，明确拼接轴。新增TestFullyShardStackedWeight直接逐位比较all-gather后的逻辑权重，main失败、补丁通过；CPU单测套件报告2项通过。
- **状态与边界**：PR仍为Draft、未合并。上游CPU与GPU单测工作流均跳过，只有Lint成功。H100不能验证所需SM100量化核，未做MXFP8/NVFP4训练loss对照；非均匀Shard(1)还依赖PyTorch #203465，不能称补丁单独覆盖全部情况。
- **可复用方法**：在量化之前核对分布式重组的逻辑权重，既测均匀分片也测非均匀分片。shape正确、loss有限都不足以证明权重没有错序。

来源：[原始发现与前后对照](https://github.com/pytorch/torchtitan/pull/5874)；[回归测试代码](https://github.com/pytorch/torchtitan/pull/5874/files)；[GPU工作流跳过状态](https://github.com/pytorch/torchtitan/actions/runs/42136160675)。

**3. Megatron：RL已更新高精度权重，CUDA graph却继续使用旧量化缓存**

PR #9076于11月6日23:55公开，无独立issue。涉及local CUDA graph、bf16主权重、FP8/MXFP8/NVFP4计算，以及生成之间原地更新权重的场景。

- **作者发现**：NVFP4 RL设置中，93%的采样token不同于相同权重trainer的argmax，generator/trainer KL为0.005至0.011，作者预期接近0。即使设置disable_parameter_transpose_cache仍出现旧输出。
- **定位与修复**：graph路径没有沿用eager的刷新策略；Transformer Engine的更新标志是全局的，但“上次写入值”却按runner保存，多batch-size graph会互相覆盖状态。补丁统一刷新策略，并按全局flag tensor追踪状态。
- **回归判定**：GB200、Transformer Engine 2.13上，构造两个batch-size graph，进行三次原地权重更新，与使用新权重的eager输出逐位比较。新增FP8/MXFP8参数化回归共4例，main全部失败、补丁全部通过。原始症状含NVFP4，但新增4例不能算NVFP4回归已覆盖。
- **状态与边界**：PR仍为Draft、未合并；官方GPU CI尚无通过证据，NVIDIA runner准入待验证。完整测试文件还有main与补丁共同存在的4项配置校验失败。原生量化主权重、capture后不再变化的权重不属作者列出的受影响范围。
- **可复用方法**：RL权重refit应连续跨多个graph尺寸检查“新权重真的生效”，使用新权重eager输出作参考；只检查参数checksum会漏掉执行图里的旧缓存。

来源：[原始症状、根因分析与4例回归](https://github.com/NVIDIA/Megatron-LM/pull/9076)；[测试代码](https://github.com/NVIDIA/Megatron-LM/pull/9076/files)；[官方runner准入说明](https://github.com/NVIDIA/Megatron-LM/pull/9076#issuecomment-6689121854)。

**4. SGLang/ROCm Triton：不等长MLA batch触发NaN、有限错值或非法访问**

问题于11月7日05:15公开，规避补丁于08:12提交。实测硬件为Radeon 8060S/gfx1151，ROCm 7.13 nightly、PyTorch 2.10、Triton 3.6；MLA维度Lk=576、Lv=512。

- **触发与复现**：长度[3,700,9000,64,1500,20]重复4次，构成不等长batch，多个序列到达64个KV split上限。作者对照fp32参考，5次运行共40个seed-run全部错误，有限结果误差约3.7至4.8，远超约0.01的bf16舍入量级；同输入的坏行会随运行变化。
- **定位证据**：错误来自stage 1 partials，换成Torch做stage 2仍错。脱离SGLang的独立Triton脚本中，4 warps/1 stage为10/10坏，改2 stages为0/10坏且逐位一致；gathered KV load触发，连续KV读取对照不触发。AMD codegen问题仍属根因推断，未获上游确认。
- **补丁与回归**：PR #48987仅对gfx115x且Lk=576将stage 1的num_stages改成2。典型不等长batch从15/15坏变为0/15；128例正确性扫描中127例逐位不变，剩余1例从NaN变为0.0042误差。新增测试对fp32要求atol=0.02，main失败、补丁连续三轮通过。
- **状态与边界**：PR仍开放未合并，Base CI止于标签门禁；新测试没有对应gfx115x CI runner，在其他硬件上跳过。仅gfx1151实测，不能宣称同门控下gfx1150/1152也已验证。服务GSM8K 58.68%→58.61%的对照没有触发原故障，只能作为无明显回归旁证。
- **可复用方法**：测试batch内长度混合、split上限、间接读取及同输入重复执行。平均任务分数正常，不足以排除局部内核错误。

来源：[原始脚本与对照](https://github.com/sgl-project/sglang/issues/48967)；[独立Triton复现](https://github.com/ROCm/triton/issues/989)；[规避补丁及回归](https://github.com/sgl-project/sglang/pull/48987)；[上游CI运行](https://github.com/sgl-project/sglang/actions/runs/42316851972)。

**5. vLLM × Triton 3.9：block-FP8 MoE把标量scale当二维张量索引，服务启动失败**

问题于11月7日08:09公开，修复PR于08:58提交。环境为官方cu134-nightly a2bcb5657、vLLM 0.33.0rc1.dev806、PyTorch 2.15、Triton 3.9，Qwen3.8-Flash-Next-FP8、TP4。

- **复现与定位**：4×GB200与4×VR200均在profile/dummy run报“rank-0 tensor索引过多”，引擎无法初始化。作者固定容器和依赖，仅替换逐提交wheel：父提交f9b40a5d通过，#64760的42c043c6失败。同一a2bcb5657在PyTorch 2.14/Triton 3.8容器通过，清楚分离了提交与编译器版本的交互。
- **错误判定**：优化在特定block shape下把b_scale变为标量，却继续使用二维切片；Triton 3.9拒绝这种索引。实际观察到的是[32,32] block配置，更广形状范围来自源码推断。
- **修复证据**：PR #66202令scalar分支直接广播、vector分支不变。问题报告者在同一4×GB200环境应用补丁，确认仍走TRITON FP8 MoE路径，启动及8k输入/1k输出benchmark在并发1、64、256均完成。VR200修复后尚未复测。
- **状态与边界**：PR开放未合并；PR作者只有CPU，新测试仅完成collection后因无CUDA而跳过。测试目前只断言shape，不是数值正确性检查；上游pre-commit工作流停在作者/标签资格检查，不能写成代码检查或GPU CI已通过。
- **可复用方法**：建立“代码提交×编译器版本×block shape”的兼容矩阵；把编译/启动冒烟通过与数值回归通过分开验收。

来源：[原始复现、固定依赖二分及报告者验证](https://github.com/vllm-project/vllm/issues/66195)；[候选修复](https://github.com/vllm-project/vllm/pull/66202)；[上游门禁运行](https://github.com/vllm-project/vllm/actions/runs/42321482734)。

**6. TorchTitan：Muse Spark的loss可以继续计算，TP下embedding却每步没有梯度**

Issue #5915于11月7日08:05公开；相关PR #5802最初于11月4日创建，11月7日08:10更新并进入ready for review，尚未合并。

- **复现与判定**：main 9f70e5597、muse_spark_debugmodel、TP2+EP2、seed 42下，tok_embeddings.embedding.weight.grad每步为None，其余参数都有梯度。这是逐参数检查发现的问题，不能用总loss或总grad_norm替代。
- **候选修复**：将embedding reduction改为显式同步执行，作者报告step 1/2梯度norm恢复为0.0590/0.0378；TP1首步参考为0.0564，其余参数梯度与main逐位一致。async redistribution进入compiled RMSNorm只是怀疑，最小例子未复现，根因仍未确定。
- **回归与CI**：作者对多模型TP/SP/CP/EP配置做10步loss和grad_norm对照，并做full-model逐参数梯度检查。HF Transformers backend的8-GPU集成CI确实通过，但它不能证明Muse Spark专门缺梯度用例已被覆盖；“TP下每个可训练参数都有梯度”的专门CI仍待补充。
- **可复用方法**：对预期参与训练的每个参数断言grad存在，另外明确eager与compiled路径覆盖。某个集成工作流变绿，不代表原始缺陷的判定条件已进入CI。

来源：[缺梯度报告](https://github.com/pytorch/torchtitan/issues/5915)；[候选修改及验证范围](https://github.com/pytorch/torchtitan/pull/5802)；[已通过的8-GPU集成CI](https://github.com/pytorch/torchtitan/actions/runs/42316587012)。

**7. AI辅助测试：给Agent更好的代码地图，生成有效测试的提升仍小于额外成本**

近7日研究补充：York University的“How Should Agents See Code?”于11月5日提交预印本。它固定Claude Code/Haiku 4.5，对比原始文件/搜索工具与AST、调用图、依赖图、文档及混合代码视图。

- **实验与判定**：测试生成部分包含161个SWT-bench任务。成功必须是生成的测试在缺陷版本失败、在已知修复版本通过，而不是仅看测试能运行或覆盖率增加。
- **作者实测**：Hybrid复现测试率47.8%，Docs为47.2%，原始工具基线44.7%，分别高3.1和2.5个百分点；Callgraph反而降至37.9%。全部代码表示方案增加token和耗时，Hybrid平均耗时192.5秒，对比基线161.4秒。
- **边界与状态**：每个任务、每种方案仅运行一次，差距可能包含单次生成波动，不能宣称稳定提升或统计显著。实验同时改变表示内容、工具可用性与使用引导，不能把全部差异归因于数据结构。论文和实验工具已公开，但这是基准研究，未展示生产CI推广效果，本简报未复跑。
- **可复用方法**：评估AI生成测试时，坚持“缺陷版失败、修复版通过”的双向判定，同时统计有效检出、成本与耗时；不要把更丰富的检索上下文直接等同于更好的测试断言。

来源：[预印本](https://arxiv.org/abs/2611.07286)；[完整实验、表V与局限](https://arxiv.org/html/2611.07286v1)；[官方实验仓库](https://github.com/MelikaSepidband/How-Should-Agents-See-Code)。

**本期最值得落地的检查**

1. 在“能跑通”之外加上具体正确性判定：量化scale的解析答案、all-gather权重逐位一致、更新后graph与eager一致、逐参数grad非空。
2. 让回归覆盖容易漏测的组合：非零EP rank、不均匀分片、多个graph尺寸、不等长batch和编译器版本切换。
3. 保存修复前失败、修复后通过的证据，并单独记录作者运行、CI执行、skip、门禁与合并状态。AI生成的测试也接受同一标准。

归档：[本期完整简报](https://github.com/BruceShenC/News/blob/main/daily/2026-11-07/llm-agent-quality.md)。
