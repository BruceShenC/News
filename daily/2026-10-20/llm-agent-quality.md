# 10月20日 · AI 测试与推理／训练 QA 简报

日期：2026-10-20（Asia/Shanghai，北京时间 UTC+8）

归档说明：本文保留 2026-10-20 当期调研内容、来源和统计口径，不代表当前状态。仅将个性化称谓改为通用表述；未重新研究或执行项目。

今天重点是权重同步的静默损坏、恢复训练测试的假通过，以及工具调用与JSON Schema的组合回归。以下6项均为新提交、尚未合并的PR，其中1项仍为草稿；数字来自作者验证，未在目标环境复测。

测试 AI 系统

1. vLLM：packed NCCL同步可能静默损坏MoE权重
10月19日，开放PR #62555。layerwise reload跨多个pack时，其他stream仍在读取的接收缓冲区可能被复用。作者真实NCCL NET传输实验中，20次复现由20/20损坏降至0/20；相关套件报告249通过、36跳过。
可采用：覆盖单层跨多个pack、NET/NVLink和不同缓冲区数量；同步后逐tensor核对权重，再比较副本输出。仅在NVLink上通过可能漏掉该竞态。[PR](https://github.com/vllm-project/vllm/pull/62555) · [复现条件](https://github.com/vllm-project/vllm/issues/62554)

2. Transformers：恢复测试可能从头训练，却仍然通过
10月19日，开放PR #49723。分布式测试worker没把checkpoint参数传给trainer.train；从头训练也可能产生相同学习率序列，造成假通过。补强后，作者在双RTX4090上报告DDP、FSDP1/2、DeepSpeed ZeRO2/3共6项通过。
可采用：断言恢复起始step、实际执行步数及新生成checkpoint。完整恢复还应比较optimizer、RNG和数据迭代状态，不能只看loss或学习率。[PR与CPU最小复现](https://github.com/huggingface/transformers/pull/49723)

3. Agent输出契约：携带tools时，显式JSON Schema可能被放宽
10月19日，vLLM开放PR #62499。GLM工具解析器的默认严格级别，可能将“非strict工具＋auto tool choice＋response_format”组合成允许工具调用的语法，让原本要求输出JSON的回合继续调用工具。作者报告282项解析器测试通过，尚不等于完整模型服务验证通过。
可采用：交叉覆盖tools有无、strict开关、auto/none和流式输出；统计schema违例、意外工具调用及token预算耗尽率。[PR](https://github.com/vllm-project/vllm/pull/62499) · [原始问题](https://github.com/vllm-project/vllm/issues/62483)

4. 量化性能：Triton升级引出特定大批次prefill退化
10月19日，vLLM开放PR #62565，已有批准评审。Kimi-K2.6 W4A16、TP8、MI350/355的特定MoE tile在Triton3.8上退化；调整配置后，作者报告并发32的TTFT从1981ms降至829ms、吞吐提高6.2%。
限制：主要命中约6k token以上的批次，单请求没有相应收益。应按编译器版本、批次token边界和并发分桶，同时保留数值一致性检查；这些性能数字没有证明精度不变。[PR与分场景数据](https://github.com/vllm-project/vllm/pull/62565)

5. vLLM MRV2：容量测试需要覆盖负载爬坡
10月19日，开放PR #62560。graph capture之后缺少最大批次采样器预热，可能使服务随批次增大不断保留新显存块。结合依赖PR #57926，作者报告Qwen3-4B负载期显存增长从1.77GB降至0.11GB，吞吐基本持平。
限制：验证来自AMD R9700，未测NVIDIA，收益不能全部归因于这一项改动。可记录预热后显存增长曲线、峰值、可用KV token及OOM，避免只看启动成功和短时tok/s。[PR及依赖](https://github.com/vllm-project/vllm/pull/62560)

6. SGLang DSpark：重复输出加速，仍需独立验证质量
10月20日，PR #45270当前为Draft。作者在固定2048-token周期性输出上保持token IDs相同，将verify次数从2047降至285；单次预热后测量为14.38秒→2.38秒。
重要限制：空白／重复输出的根因仍在调查。普通数值配置控制组只有42/48精确匹配，另有确定性配置下的零KL检查。应同时报告输出一致性、任务成功率和接受率，避免把更快生成重复内容算成质量改善。[草稿PR及限制](https://github.com/sgl-project/sglang/pull/45270)

AI 帮助测试
本次未核实到过去24小时内有明确首发日期的重要GUI/API用例生成或自愈更新，不重复此前内容。

建议小范围验证两项
- 恢复训练反证：在独立目录从checkpoint-2恢复到step4，检查是否只新生成checkpoint-4；再次生成checkpoint-2可暴露从头重跑。这只验证恢复路径确实执行，不能替代完整状态一致性检查。
- Schema×tools探针：使用含固定const字段的JSON Schema，对比无tools、非strict tools＋auto、显式none三组，记录schema合规率、意外tool_calls及输出token数，并设置严格token上限控制异常循环。

昨天详细讨论的SGLang #45049暂未核到实质新进展，本次不重复。
