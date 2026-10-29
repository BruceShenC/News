# 10月17日 · 大模型与 AI 测试简报

日期：2026-10-17（Asia/Shanghai，北京时间 UTC+8）

归档说明：本文保留 2026-10-17 当期调研内容、来源和统计口径，不代表当前状态。仅将个性化称谓改为通用表述；未重新研究或执行项目。

今天优先看三类风险：数值异常、PD 链路协议回归，以及 GUI 自愈“执行成功但点错元素”。精选7条，兼顾过去24小时及近一周。下列 GitHub 日期按 UTC；修复合并不等于正式版本已发布。

测试 AI 系统

1. vLLM：Hopper 上 LoRA 的 NaN 修复，需要补回归覆盖
10月16日合并 [#61906](https://github.com/vllm-project/vllm/pull/61906)。SM90 上 fp32 输入的 LoRA expand，在 rank≥64 时可能产生垃圾值或 NaN；修复将对应路径的 num_stages 设为1。
作者在 H20、Qwen3-8B、rank64 实验中，2047个 prompt logprob 从全部 NaN 降至0；最差测试形状的单个 kernel 耗时增加约21%，不能当成整机吞吐损失。
特别注意：[最终差异](https://github.com/vllm-project/vllm/pull/61906/files)只有修复，首帖提到的回归测试后来被删除。建议补 dtype×LoRA rank×prompt长度矩阵，同时检查 NaN、参考 logprob 差异和耗时。

2. vLLM＋Mooncake：压测“空响应”可能来自 SSE 协议错误
10月16日合并 [#61218](https://github.com/vllm-project/vllm/pull/61218)，修复示例代理把流式响应误标为 application/json，以及多连接器指标导出的断言问题。
CPU 回归21项通过，覆盖流式／非流式与 completion／chat 四种组合。此前独立集成版本中，错误 header 让13个抽样预热响应全部被 AIPerf 拒绝。
可加入 PD 冒烟门禁：验 Content-Type、SSE 原始字节、指标导出和代理文件变更是否触发 CI。GPU 证据来自另一个集成提交，不能声称所有 Store 超时、空响应都已解决。

3. vLLM：长工具参数解析，可能成为 Agent 延迟瓶颈
10月16日合并 [#62035](https://github.com/vllm-project/vllm/pull/62035)，修复 MiniMax M2 流式工具参数被反复全量解析造成的平方级开销。
作者微基准中，16KB write_file 参数的 parser CPU 中位数从5.9秒降至25.6毫秒；4.4万条随机切块、截断、异常 tag 流的逐 delta 对比报告一致。
这是解析器微基准，不是端到端 Agent 提速倍数。建议容量测试加入1／4／16／32KB工具参数和不同 chunk 粒度，监测解析 CPU、事件循环阻塞及客户端尾延迟。

4. SGLang：长历史初始化要测对其他请求的影响
10月16日合并 [#44787](https://github.com/sgl-project/sglang/pull/44787)，Engram 历史初始化改为先截取需要的尾部，再异步复制到 GPU，避免约10万 token 输入的完整列表拼接与阻塞复制。
两项单测及8×H200的 DSv4 PD 定向测试通过，但未公布端到端吞吐或尾延迟收益。
适合用长短 prompt 混流验证：decode 接入停顿、历史边界和输出一致性；不能把局部测试通过等同整体 CI 全绿。

5. 训练 QA：换 GPU 数量时，数据顺序可能干扰实验结论
10月16日 PyTorch 发布 [Zephon 接入 TorchTitan 的说明](https://pytorch.org/blog/bringing-elastically-deterministic-data-loading-to-torchtitan-with-zephon/)。方法是固定不随拓扑改变的数据排列、packing 和混合顺序。
作者用1B模型、20B token、8／16／32／64卡比较，标准 loader 的评测分数跨度在 DCLM／FineWeb 分别为0.82／0.64分，Zephon 为0.05／0.011分。这是方差控制证据，不能解读成精度提升。
新事件是集成说明；包0.1.0已于10月7日发布，仍为 beta、可选参考 fork。建议恢复测试先比对全局 batch 指纹，再看 loss；顺序一致也不保证权重逐位一致。[结果与限制](https://www.datologyai.com/blog/zephon)｜[恢复测试说明](https://github.com/datologyai/torchtitan-zephon/blob/main/examples/zephon/README.md)

6. Rondo：同一输入，编译器“记得什么”也会改变正确性
10月15日提交的[研究预印本](https://arxiv.org/abs/2610.22003)，通过调用序列变异，以及 eager／清空编译状态／保留编译状态三路对照，寻找缓存和历史调用触发的缺陷。
作者报告提交39份问题报告，开发者确认35份、修复18项；不能把检测到的117项全部写成开发者确认缺陷。
对 GoldenPrompt 的启发：覆盖冷启动、预热、shape 切换、保留缓存后的同一输入。论文评估的是深度学习框架，尚未直接验证 vLLM／SGLang。

AI 帮助软件测试

7. GUI 自愈：必须把“找到了”和“找对了”分开统计
10月13日在线发表的 [JSS 论文预校样](https://www.sciencedirect.com/science/article/pii/S0164121226003900)，在 Supabase／Grafana 共133个对抗性定位器扰动上，LLM 正确恢复约55%–68%。
其中 Supabase 的53例，Claude 正确恢复33例；约26%的案例静默选中了错误元素。这个26%不能外推成整个133例或所有 GUI 自愈系统的错误率。
追加跨层上下文的配对实验从60%升至68%，但样本57、p≈0.23，尚无充分统计证据证明提升，也未证明净节省工程时间。
GUI Agent 可增加独立的元素身份与业务后置条件检查，不能把“脚本变绿”直接算自愈成功。[作者实验表](https://github.com/SuneetMalhotra/agent-harness/blob/main/paper.md)

建议优先小范围验证
- SM90＋LoRA：检查 rank64／128、fp32 中间输入的 NaN，保留 BF16 对照与修复前后耗时，避免只断言最终文本。
- GUI Agent：选20–30个历史定位失败用例，人工标记目标元素和业务结果，分别统计正确恢复、静默错定位、无法恢复及耗时，再决定自动放行范围。

以上数字来自作者或维护者公开证据，本次未自行进行 GPU 复现；相关 PR 已合并，但尚未核实哪些正式 release 包含这些修复。
