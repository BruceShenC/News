# Agent 与大模型技术热点｜2026-11-27

观察窗口：11 月 26 日 09:00 至 11 月 27 日 09:00（北京时间）；窗口外事项注明日期和入选理由。

## 头部 AI 公司重大变化

1. **OpenAI：修正 Codex 的 GPT-6.2 订阅额度少扣。** 11 月 26 日，Codex 负责人称，修正后相同工作会用掉更多订阅额度；相较上周，Astra 与 Sol 分别约快 40% 和 25% 触及上限。其他模型不受影响，历史用量不追扣。Go、Plus、Pro、Business 另获一次可手动使用的重置，负责人同日称已上线，具体以用量页为准。这里改的是订阅用量扣减，不是 API 单价，也不能直接换算成任务数减少多少。[公告原帖](https://x.com/thsottiaux/status/2125784147889393806) · [重置状态](https://quota-reset.com/history/2026-11-26)

2. **OpenAI Private Inference：API Alpha 开始小范围测试。** 近期补充，11 月 26 日凌晨北京时段，Sam Altman 表示正在与少量客户测试。官网将其标为 API 预览，通过机密计算保护模型**处理过程中**的提示词和响应；这与零数据保留（ZDR）管**处理后**是否留存是两件事。支持模型、地区、定价和可供客户核验的接口未见公开说明，不能当成现有 API 默认能力。[产品页](https://openai.com/business/private-intelligence/) · [小范围测试原帖](https://x.com/sama/status/2125638790593274259)

3. **11 月 30 日迁移提醒：Claude Sonnet 4.5 按接入方判定，不是所有云同天停。** Anthropic 9 月 30 日发的公告临近生效：Claude API 的 `claude-sonnet-4-5-20250929` 和 `claude-sonnet-4-5` 都将停用，Messages API 届时返回 404；Google Cloud 同日停用，Bedrock 常规地区列的是 2027 年 4 月 8 日。若主 Agent 或子 Agent 还固定旧 ID，先查生效的接入地址和模型，再回归工具实际执行、固定 thinking budget、助手末轮预填；后两项在 Sonnet 5.5 可直接返回 400。[Anthropic 停用表](https://platform.claude.com/docs/en/about-claude/model-deprecations) · [迁移指南](https://platform.claude.com/docs/en/models/sonnet-5-5/migration-guide) · [Google Cloud](https://docs.cloud.google.com/gemini-enterprise-agent-platform/models/deprecations/partner-models?hl=en) · [Bedrock](https://docs.aws.amazon.com/bedrock/latest/userguide/model-card-anthropic-claude-sonnet-4-5.html)

## Agent 与推理研究

4. **NVIDIA NOOA：让模型选择何时压缩、压缩哪段上下文。** 11 月 26 日 arXiv 公告批次，25 日投稿。作者报告在 OfficeQA Pro v2 的 32K 预算下，GPT-6.1 得分 21% → 56%；哪个模型会主动用压缩工具、何时触发，随模型和提示词变化。另一个 SGLang 原型尝试复用未改后缀的 KV，单一模型实验最多减少 15% 单请求预填充计算量；没有证明线上端到端延迟也降 15%。[论文与实验限制](https://arxiv.org/html/2611.34147v1)

5. **MeterRouter：把每个请求的美元预算纳入多 Agent 编排。** 同属 11 月 26 日 arXiv 公告批次。路由器逐步选角色、模型、连接和结束时机；作者以 5 个基准、3 档模型池评估，报告达到与最强多 Agent 基线相当的准确度时，成本比所选最强路由基线低 82%。这是作者对比结果；“很少超预算”不等于硬上限。上生产仍要在派发工具、重试和升级模型前检查剩余额度。[论文](https://arxiv.org/abs/2611.34327)

6. **FBQ：一次前向估计该保留哪些 KV。** 11 月 26 日 arXiv 公告批次预印本。方法用首个生成 token 及学习得到的位置偏移，近似后续注意力查询，免去先逐步生成八个草稿 token。在同一缓存预算下，作者报告 LongBench 结果接近八步前瞻基线。这是单次请求内的缓存选择，不是跨请求前缀复用，不能据此推断线上尾延迟。[论文](https://arxiv.org/abs/2611.33679)

**值得跟进：** 一是让模型参与上下文压缩，验证省下 token 后关键约束是否还能保留；二是把 Agent 成本从事后账单变成执行前约束，按成功交付计算总成本，把失败重试也记进去。
