# Agent 与大模型技术热点｜2026-11-28

观察窗口：北京时间 11 月 27 日 09:00 至 11 月 28 日 09:00，重要背景回看近 7 天。已公布但尚未生效的变更单独标注。

## 头部 AI 公司重大变化

1. **OpenAI：12 月起，ChatGPT 与 Codex 将共用订阅额度。** 11 月 27 日核对的官方说明明确：Plus、Pro、Business 按 token 共用额度，**Instant 聊天不扣这份额度**，少量账号可能提前灰度。余额与刷新时间看「设置 → 用量」。这是订阅政策，和昨天报道的 GPT-6.2 Codex 扣减修正是两件事；官方说明未给出各档的新额度数值，不能据此判断额度增减。[官方说明](https://help.openai.com/en/articles/20001625-understanding-chatgpt-and-codex-usage-limits)

2. **Anthropic：Claude Artifacts 可以安装成桌面应用。** 11 月 24 日起在 Mac、Windows 逐步开放 Beta。应用可调用 Claude 和连接器，随原 Artifact 更新；运行仍需联网、登录，模型调用计入原套餐。适合把反复用的内部小工具从聊天里拿出来。Team 默认开放安装但关闭完全访问，Enterprise 默认关闭安装；试用先保留默认权限，尤其是包含外部代码的应用，因为外部代码变化未必再次触发授权。[发布公告](https://claude.com/resources/articles/install-as-app) · [权限和版本要求](https://support.claude.com/en/articles/18516354-install-an-artifact-as-an-app)

3. **OpenAI：托管 Evals、Agent Builder 和可复用 Prompt 对象计划 11 月 30 日停用。** 这是六月公告的临近期限，不是本周新发布；截至本期，官方已把托管 Evals 标为只读，ChatKit 保留，微调的评分器另有时间表。若团队仍依赖托管 Evals，历史运行结果和可重跑的输入、提示词、评分规则要分开留存；迁移后先用固定输出回放一个已知通过、一个已知失败的样本，再把新评分接回回归。官方提供手工重建到 Promptfoo 的指南，不能假设所有配置可一键导出。[停用表](https://developers.openai.com/api/docs/deprecations#feature-and-endpoint-deprecations) · [迁移指南](https://developers.openai.com/cookbook/examples/evaluation/moving-from-openai-evals-to-promptfoo)

## Agent 与模型技术

4. **微软 Foundry IQ：知识库可以为不同调用方声明具名检索规则。** 11 月文档中的公开预览支持把数据源、过滤条件和默认值封装成可经 MCP 发现的工具。请求可以覆盖普通默认值，但对兼容索引源只能叠加过滤，不能扩大数据源；显式传未知工具名会报错。另一个 11 月 25 日更新的 Retriever 1 指南给出实际边界：配置了该预览模型时，Minimal 请求有多个语义意图会回退普通检索，返回 206 和活动警告，即使还能看到结果。两个预览的适用条件不同，不宜假设可任意组合。[具名工具与过滤](https://learn.microsoft.com/en-us/azure/search/agentic-retrieval-how-to-configure-tools#apply-defaults-and-intersect-filters) · [Retriever 1 回退规则](https://learn.microsoft.com/en-us/azure/search/agentic-retrieval-how-to-use-microsoft-retriever#troubleshooting)

5. **TASK Memory：检索到正确记忆，模型仍可能用错。** 11 月 26 日 arXiv 公告的预印本把文本记忆、可移除的知识编辑和激活引导结合起来，按查询决定是否采用检索到的事实。作者在受控任务中报告，300 条记忆时，CounterFact 的综合分比 BM25 RAG 高 16.4 个百分点。方法需要能访问模型内部，指标也不代表生产 Agent；可借鉴的是把「没取到最新记录」「取到了却沿用旧值」「用到了错误对象」分开记录，而不是只看检索命中率。[论文与实验设置](https://arxiv.org/html/2611.34762v1#S4)

6. **DRBench Pro：把深度研究的覆盖面和深度分开算。** 11 月 26 日 arXiv 公告的预印本以 Wikipedia 知识树生成问题和证据评分项。144 个案例覆盖 24 个领域，作者测试的 16 个研究 Agent 中，Claude Opus 5 以 57.04% 的综合覆盖率居首（广度 53.09%，深度 61.91%）。它衡量的是该知识树定义的覆盖，不是报告的事实正确率，也不直接代表企业私有资料或实时研究。代码和冻结数据已公开，仓库提供不调用模型的合成分数样例。生成端只看问题，评分端才看证据树，可避免把答案提示混进测试输入。[论文](https://arxiv.org/html/2611.34852v1) · [代码与数据](https://github.com/ZJU-REAL/DRBenchPro#-try-the-offline-example)

**值得跟进：** 知识网关先把硬约束和排序分开：版本、数据源和访问范围先过滤，再谈相关性和新鲜度。研究 Agent 则分别看证据是否找齐、模型是否正确采用；两个环节的失败要能单独定位。
