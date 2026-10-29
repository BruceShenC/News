# AI 独立 Skill 热度双榜｜2026年10月25日·首期

日期：2026-10-25（Asia/Shanghai，北京时间 UTC+8）

归档说明：本文保留 2026-10-25 当期调研内容、来源和统计口径，不代表当前状态。仅将个性化称谓改为通用表述；未重新研究或执行项目。

后续更正（2026-10-28）：UI UX Pro Max 实际分发 7 个 Skill，不符合独立单 Skill 口径，已移出后续榜单。这是分类纠正，不是掉粉；以下保留历史原文及当时星数，请结合[10 月 28 日更正](../2026-10-28/ai-skills-ranking.md)阅读。

总Star采集时间：北京时间09:29。本期从公开Skill目录和GitHub仓库发现候选，再核验README与技能定义。以下是已核验候选池前十，不宣称穷尽GitHub全站；所有数字均为仓库总Star，不是安装量或增长量。

一、总Star榜

1. [Karpathy Guidelines](https://github.com/multica-ai/andrej-karpathy-skills)｜223,410
将明确假设、最小实现、局部修改、可验证目标打包为一个编程行为Skill。适合研发、QA减少Agent过度设计；支持Claude Code、Cursor。
注意：README声明MIT，API未识别许可；最近推送4月20日，热度不等于近期活跃。

2. [UI UX Pro Max](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill)｜139,946
UI风格、配色、字体及设计系统检索，适合前端原型和设计规范落地；支持Claude Code、Codex、Cursor等。MIT，10月23日有推送。
注意：开源版与商业Premium能力需分开。

3. [Taste Skill](https://github.com/Leonxlnx/taste-skill)｜102,034
结合艺术方向、素材和浏览器检查改善界面，适合前端及产品设计；支持Claude Code、Codex及Skills CLI宿主。MIT，10月24日有推送。
注意：10月19日已迁出其他旧技能，目前保留单一主Skill；需要Node 22+、浏览器，素材检索需联网。

4. [Impeccable](https://github.com/pbakaus/impeccable)｜86,437
当前为一个Skill、24个子命令，覆盖设计审查、排版、响应式、无障碍与浏览器迭代。适合前端质量、设计系统团队；支持Claude Code、Codex、Cursor等。Apache-2.0，10月24日有推送。
注意：确定性检测不等于完整UX正确性；首次运行可能下载引擎，应先审查依赖和权限。

5. [Last30Days](https://github.com/mvanhorn/last30days-skill)｜65,326
汇集近期社区、网页信号做主题研究，适合资讯调研与工具比较；支持Claude Code、Codex、Cursor、Copilot、Gemini CLI等。MIT，10月23日有推送。
注意：部分来源依赖独立凭据或付费服务，讨论量不能证明信息真实。

6. [Diagram Design](https://github.com/cathrynlavery/diagram-design)｜60,745
将架构、流程、用户旅程整理为HTML/SVG图，适合技术方案和质量体系汇报；支持Claude Code、Codex、Cursor、Copilot等。MIT，10月24日有推送。
注意：图画得清晰不代表关系正确，需核对边界、依赖方向和事实。

7. [Humanizer](https://github.com/blader/humanizer)｜60,273
改善模板化措辞、冗余表达与文字自然度，适合文档和工作沟通；支持Claude Code、Codex等Skill宿主。MIT，最近推送10月16日。
注意：目标是可读性，不是绕过AI检测；改写后仍需检查数字、引用与原意。

8. [I Have ADHD](https://github.com/ayghri/i-have-adhd)｜58,605
让编程助手先给行动、编号步骤、减少岔题，适合偏好简洁可执行输出的人；有Claude Code、Codex、Cursor等适配。MIT，最近推送10月19日。
注意：是输出组织技能，不是诊断或治疗工具；精简不能省略必要风险与上下文。

9. [Cloudflare Security Audit](https://github.com/cloudflare/security-audit-skill)｜44,611
编排侦察、覆盖分析、候选验证与独立复核，输出机器可读安全发现。适合安全和平台QA，需要支持工具调用、并行子Agent的编码助手。MIT，最近推送9月14日。
注意：执行目标代码需要操作系统级隔离、网络和资源约束，Skill文本本身不是沙箱。

10. [Book to Skill](https://github.com/virgiliojr94/book-to-skill)｜35,982
将技术书、文档或多份资料转换为按需加载的知识Skill，适合知识库及团队规范复用；支持Claude Code、Codex、Copilot CLI、Amp等。MIT，最近推送10月18日。
注意：仍可能遗漏、误解来源；需检查版权、敏感资料及可选发布动作，不能照搬“无幻觉”宣传。

二、Star飙升榜

首期暂不列未经复核的前十。今天已建立精确总Star基线，但还没有同口径24小时或7日差分，未知增长不会记成零。

[第三方Skill榜](https://www.skillleaderboard.com/)提供截至10月24日的7日增长圆整值，但本次未取得对应原始快照与精确起止采样时间，且混合了单技能、合集和工具。因此仅作候选线索，不与本次API数据拼成精确增长排名。[方法说明](https://www.skillleaderboard.com/methodology)

后续使用同一时间窗的净增量比较；有多少可靠增长项列多少，不强凑十个。

本期观察与建议
- 头部既有轻量行为规范，也有脚本、浏览器驱动的复杂Skill，权限风险和使用成本差异很大。
- 不能只按仓库名字分类：Taste、Impeccable当前收敛为单Skill；Caveman已扩展为多部件套装，本期排除。
- 首期无前期可比排名，不声称任何项目“上升几位”。

优先看Cloudflare Security Audit的“发现→验证→覆盖记录”流程，与相关问题发现方法最贴近；其次看Impeccable如何结合确定性检测与模型审查，可用非敏感测试页面做小范围对照。

热度不等于成熟度、安全性或生产效果。
