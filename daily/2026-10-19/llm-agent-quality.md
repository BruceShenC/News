# 10月19日 · AI 测试与推理／训练 QA 简报

日期：2026-10-19（Asia/Shanghai，北京时间 UTC+8）

归档说明：本文保留 2026-10-19 当期调研内容、来源和统计口径，不代表当前状态。仅将个性化称谓改为通用表述；未重新研究或执行项目。

今天重点是静默错误：接口正常返回、前向计算正常，甚至测试全绿，都可能掩盖质量问题。前5项包含过去24小时的新报告或修复PR，尚未合并；后2项补充近7天ASE会议进展。以下实验数字均来自作者，本次未独立复现。

测试 AI 系统：推理与训练

1. vLLM：MoE热更新可能将正确权重写入错误布局
10月19日02:11北京时间创建的[#62332](https://github.com/vllm-project/vllm/pull/62332)，仍为草稿PR。FlashInfer BF16 MoE加载后会重排专家权重；再次直接写入checkpoint布局，可能静默破坏计算。
作者在GB300、Qwen3-Omni-30B-A3B、TRT-LLM、TP=1配置上，用1,319道GSM8K、5-shot评测：冷启动85.44%，直接回写同一checkpoint后为0%，候选补丁约保持85%。
可采用：把“同权重热更新应等价于冷启动”纳入回归，覆盖完整、部分专家和稀疏更新，检查权重指纹、输出及后处理幂等性。补丁仍有DBRX直接写参数、IPC权重缓存等限制。

2. SGLang：精确答案探针定位调度与缓存错误
两份issue均于10月18日21:13北京时间创建，仍开放，尚无独立复现确认。作者使用与Qwen3-0.6B同形状、答案可精确计算的合成权重：
- FP8 Marlin、RTX3090、SGLang0.5.22：192请求负载的部分轮次有1–5条错误请求，关闭overlap的5轮均正确；独立GEMM测试未发现错误。
- NGRAM推测解码：共享前缀、stop-token结束的192请求中有3条错误，关闭overlap后为0；普通EOS结束尚未直接测试。
可采用：“量化×调度×缓存×结束方式”组合测试，记录错误请求率、首个错误token位置和跨请求传播。真实模型输出不同本身不足以判定引擎损坏。[FP8报告](https://github.com/sgl-project/sglang/issues/45000) · [NGRAM报告](https://github.com/sgl-project/sglang/issues/44997)

3. vLLM：空元数据块会污染流式性能测量
10月19日04:22北京时间创建的[#62341](https://github.com/vllm-project/vllm/pull/62341)，开放PR。chat benchmark把开头仅含role的空块当作输出，使TTFT计到元数据到达，并多记一个ITL间隔。作者600次请求中发现208次间隔数量异常，首个伪间隔中位数仅4微秒。
价值：压测客户端本身也需要正确性测试。这是测量修复，实际引擎速度未变；作者称常见汇总指标变化小于轮间波动。
可采用：回放role-only、reasoning、tool_calls、中途空块及空流，核对首个真实输出时间和间隔数量，保留原始SSE时间戳。

4. Transformers：专家并行前向正确，反向可能没有梯度
问题10月17日提出，修复[#49710](https://github.com/huggingface/transformers/pull/49710)于10月18日17:24北京时间创建，仍开放，CI等待维护者批准。
作者在Transformers5.20.1、两进程CPU/Gloo、四专家/top-2配置下报告：torch2.10前向与未分片参考一致，但专家参数及hidden states梯度为None，且无警告；2.14.1对照正常。PR指向旧版all-to-all缺少autograd支持，不能扩大成所有旧版本均已实测。
可采用：不同rank使用不同token数量，断言梯度存在、有限，比较梯度最大误差和一次optimizer step后的权重变化。[问题与对照](https://github.com/huggingface/transformers/issues/49702)

5. LeRobot：复用采集缓冲区可能改写历史训练帧
10月19日04:02北京时间创建的[#5009](https://github.com/huggingface/lerobot/pull/5009)，开放PR。add_frame按引用保存数值数组，录制时复用NumPy或CPU tensor，会使后续修改污染历史帧和归一化统计。作者三帧示例预期0、1、2，实际全部变成2；候选修复改为录入时复制。
可采用：复用同一缓冲区后保存、重新读取，核对逐帧值及min/max/mean/std。作者两组定向测试分别57、25项通过，未跑全库测试，也没有真实机器人训练收益数据。

AI 帮助测试

6. DCAware：测试通过率之外，还要检查断言强度
10月13日ASE安排报告；论文8月6日首发、10月9日更新，并非本周首次发布。研究结合静态代码上下文与按需运行时状态，生成Java测试的缺失断言。
作者在38个项目、500个单断言任务上报告：GPT-5-mini通过率88.2%，ChatAssert为73.2%；共同通过的183题上，Shared KG为92.83%与86.31%。
可采用：同时衡量通过率、变异体检杀能力、断言削弱比例和成本，避免只优化“修绿”。Shared KG是相对人工断言的变异体检杀指标，不是真实缺陷发现率；以当前实现为回归预期也不能证明需求正确。[论文及指标定义](https://arxiv.org/html/2608.05917v2) · [ASE议程](https://conf.researchr.org/track/ase-2026/ase-2026-research-track)

7. PropGen：从GUI操作生成移动App性质测试
10月14日ASE安排报告；论文4月15日首发、10月8日更新。方法从GUI提出功能假设，执行验证，再生成“前提—操作—结果”的性质。
作者用GPT-5.2测试12个Android App，每App分配3小时生成、6小时查错：985条性质中912条有效，人工确认25个此前未报告的功能错误，论文记载5个已修复。
可采用：选择有状态业务流，记录有效性质率、误报率、去重确认缺陷数及每个确认缺陷耗时。其他工具发现了它漏掉的10个错误，仍需人工确认业务意图；上述不是上周新增缺陷数。[论文](https://arxiv.org/html/2604.13463v2) · [报告安排](https://conf.researchr.org/details/ase-2026/ase-2026-research-track/240/PropGen-Automated-Property-Generation-for-Property-Based-Testing-of-Mobile-Apps)

建议优先做两个小验证
- 推理侧：用20条固定输入比较冷启动与同权重热更新，再加入共享前缀及stop-token结束，记录权重指纹、首个输出分歧和错误请求率；精确探针与真实模型分开判定。
- AI测试侧：选20个已有测试前缀，对比直接生成、迭代修复、补充上下文三组。通过率之外加入变异体检杀与断言削弱检查，防止测试越来越容易通过，却越来越难发现问题。
