# Agent 与大模型技术热点｜2026-11-26

**观察窗口：北京时间 11 月 25 日 09:30 至 26 日 09:00。** 这是简报提前半小时后的首期；研究条目标明原始提交或近期公开日期。

## 头部 AI 公司重大变化

1. **GitHub Copilot PR 审查：12 月 30 日起，符合条件的审查默认改由仓库所属组织付费。** 11 月 25 日公告，尚未生效。适用于使用 Copilot Business／Enterprise、且没明确选择付款方的持牌成员审查；没有开启 AI Credits 付费用量、存在计费锁或没有可计费客户时，仍设为 Member。已有明确选择和无许可证成员的单独规则都不变。改的是付款方，不是审查权限或合并规则；团队应同时看预算和最新 PR 版本的实际审查覆盖率。[GitHub 公告](https://github.blog/changelog/2026-11-25-copilot-code-review-billing-will-default-to-the-organization/) · [配置文档](https://docs.github.com/en/copilot/how-tos/copilot-on-github/set-up-copilot/configure-code-review)
2. **Anthropic：MCP 服务能随工具分发 Skill，也能接手异步任务。** Claude Code 2.1.332 于 11 月 25 日 09:32（北京时间）发布，MCP 技能在斜杠菜单显示为 `server:skill`。Skill 是公开 Beta，Pro／Max／Ultra 默认可用，Team 需管理员启用，Enterprise 尚待开放；Tasks 与 Elicitation 已向前三类套餐正式开放，并向 Team／Enterprise 推送。价值在于工具与操作指引可以一起更新，长任务不用堵住当前对话；但需要兼容的服务端和会话，一次性 `claude -p` 不接收 MCP Tasks。[Anthropic 公告](https://claude.com/resources/articles/skills-over-mcp-tasks-and-more) · [版本说明](https://github.com/anthropics/claude-code/releases/tag/v2.1.332) · [会话限制](https://code.claude.com/docs/en/mcp)
3. **阿里通义：Token Plan 的 Qwen4-Max 预览退场，旧 ID 仍能调用，但计费已变。** 11 月 25 日 10:00（北京时间）生效。旧 `qwen4-max-preview` 请求自动转到正式 `qwen4-max`，按正式模型抵扣 Credits、统计用量和计费，预览折扣结束。迁移别只看请求是否成功，还要复核实际路由、长输出和用量变化；这是 Token Plan 公告，不是所有产品入口下线。[阿里云产品日志](https://help.aliyun.com/zh/model-studio/token-plan-product-updates)

## Agent 与模型研究

4. **Delegent：主模型留住最终决策，小模型接手查代码、跑测试和搜索。** 11 月 25 日北京时间提交的预印本。在作者的 DeepSWE 和搜索样本中，27B FP8 子模型分别降低主模型开销 67% 和 59%，完成表现未下降。不是路由整条任务，也不是整体成本降了同样比例；延迟会增加，还应计入小模型算力、重试和复核。[论文与代码入口](https://arxiv.org/abs/2611.33098)
5. **腾讯 OmniEVIE-4B：检索结果可定位到页面区域或视频片段。** 11 月 24 日提交预印本，权重和代码公开。把每条记录压成最多 128 个低维 int8 向量，走三级检索，再用向量匹配定位证据。作者报告百万记录的单线程搜索为 54.2 ms，不含文档编码，也不是知识库端到端延迟。值得对比的是：在相同召回率下，能保留多少证据位置、节省多少索引空间。[论文](https://arxiv.org/abs/2611.32203) · [模型卡](https://huggingface.co/tencent/OmniEVIE-4B)
6. **AIChipBench：让编码 Agent 找到“第一个错误状态”，比只告知输出错更有效。** 11 月 25 日北京时间提交的预印本。量化解码器的 RTL 任务既检验 token，也对照整数参考实现的持久状态；作者在 12 组配对实验中报告，通过率比只给输出反馈高 25.0–46.9 个百分点。结论限于该硬件任务；可以迁移的是独立状态断言和留出序列，不是模型榜单。[论文](https://arxiv.org/abs/2611.33093)

**两点值得跟进：** 长任务把“任务结束”和“结果有足够证据”分开验收；分层 Agent 不只看主模型账单，按每个验收通过的任务算总成本与等待时间。仅核对公开资料，未运行。
