**AI 独立 Skill 热度双榜｜2026 年 11 月 4 日**

**本期结论**
已核验样本的总 Stars 前十顺序与 11 月 3 日晚间观察一致。共同近 7 日窗口内，Impeccable 净增 10,990 居首，Cloudflare Security Audit 净增 7,996 次之。Hallmark 也增长较快，但历史采样起点不同，单独披露，不挤入同窗排名。

**观察时间与口径**
- 总 Stars：北京时间 11 月 4 日 09:29:13.855—09:29:15.371，公开 GitHub 仓库 API 实时请求（maxAge=0）。这是请求批次区间，不是所有仓库完全同时的服务器快照。
- 主增长榜：10 月 28 日 09:28:29.801 → 11 月 4 日 09:29:13.855，共 **7 天 44.054 秒**。10 个仓库沿用同一批次起点，按仓库数字 ID 对齐后计算净增。称“近 7 日”，不冒充精确 168 小时。
- 11 月 3 日基线实际采于 20:45:06.247，至本期只有 **12 小时 44 分 7.608 秒**。本期不把这个短窗叫“24 小时增长”。
- 净增=本期总 Stars−起点总 Stars；增长率=净增÷起点总 Stars。下列日均值按实际经过天数折算，不是每天都达到的速度。本期不用 GitHub Trending 的 today/this week 数字，也不混入第三方榜单估值、安装量或仓龄均值。

**一、总 Stars 榜：已核验独立 Skill 样本前十**
以下总量均对应上面的同一观察批次；名称链接直达仓库，另附技能定义。

1. **[Karpathy Guidelines](https://github.com/multica-ai/andrej-karpathy-skills)｜229,107 Stars**
把显式假设、最小改动和可验证成功标准写进编码流程，适合开发者、代码评审与 QA。支持 Claude Code、Cursor。技能定义声明 MIT，但仓库 API 未识别许可证；最近推送仍为 4 月 20 日，不宜把高热度当近期维护活跃。 [技能定义](https://raw.githubusercontent.com/multica-ai/andrej-karpathy-skills/main/skills/karpathy-guidelines/SKILL.md)

2. **[Taste Skill](https://github.com/Leonxlnx/taste-skill)｜109,513 Stars**
给网页与应用界面提供视觉方向、真实素材和浏览器检查，适合前端工程师与设计工程师。README 明确当前只分发 design-taste-frontend，旧技能已迁出；支持 Claude Code、Codex、兼容 Agent Skills 的宿主。MIT；脚本需 Node 22+ 与 Chromium 类浏览器，联网找图须检查授权和署名。 [技能定义](https://raw.githubusercontent.com/Leonxlnx/taste-skill/main/skills/taste-skill/SKILL.md)

3. **[Impeccable](https://github.com/pbakaus/impeccable)｜101,565 Stars**
对现有界面做设计审查、可访问性和响应式检查，适合前端与设计系统团队。支持 Claude Code、Codex、Cursor、Copilot、Hermes 等。Apache-2.0；平台副本和测试夹具不是额外分发的 Skill。特别注意：README 的 Live mode 说明，无活跃聊天轮询时 Apply 可能调用本机 CLI，并关闭其权限检查和沙箱、继承环境变量。热度不能抵消此风险，优先只读审阅，勿直接接入含凭证的工作环境。 [技能定义](https://raw.githubusercontent.com/pbakaus/impeccable/main/cursor-plugin/skills/impeccable/SKILL.md)

4. **[Archify](https://github.com/tt-a1i/archify)｜93,141 Stars**
把需求、流程或代码证据转换成可探索的独立 HTML 图，适合架构师、开发者和技术文档作者。支持 Claude Code、Codex、Cursor、OpenCode。Apache-2.0；用户分发目标是 archify，仓库内其他 .agents 技能是贡献者辅助。布局检查不等于架构语义正确，源码关系仍须人工核对。 [技能定义](https://raw.githubusercontent.com/tt-a1i/archify/main/archify/SKILL.md)

5. **[Last30Days](https://github.com/mvanhorn/last30days-skill)｜66,491 Stars**
跨 Reddit、X、YouTube、Hacker News 等来源研究近期讨论，适合研究、产品与内容团队。支持 Claude Code、Codex、Cursor、Copilot、Gemini CLI 等。MIT；可涉及 API、浏览器会话和付费数据源，须单独审查访问与费用。11 月 3 日 UTC 的最新提交修复“拒绝 Cookie 后仍自动取凭证”的处理，属于维护修复，不代表全部访问风险已消除。 [技能定义](https://raw.githubusercontent.com/mvanhorn/last30days-skill/main/skills/last30days/SKILL.md)

6. **[Diagram Design](https://github.com/cathrynlavery/diagram-design)｜64,673 Stars**
输出带品牌样式的 HTML/SVG 架构、流程与数据图，适合文档、培训和技术沟通。支持 Claude Code、Codex、Cursor、Copilot 等。MIT；11 月 3 日 UTC 最新提交把插件清单版本升至 2.6.141，不能仅凭版本号推断新能力。图中数字、关系和因果仍要校对。 [技能定义](https://raw.githubusercontent.com/cathrynlavery/diagram-design/main/skills/diagram-design/SKILL.md)

7. **[Humanizer](https://github.com/blader/humanizer)｜64,423 Stars**
去掉模板化措辞并尽量保留作者声音，适合编辑与技术写作者。支持 Claude Code、Codex、Claude.ai/Desktop 及其他兼容 Skill 的宿主。MIT；最近推送 10 月 28 日。它是文风编辑工具，不是事实核查器；README 也明确不以绕过 AI 检测器为目标。 [技能定义](https://raw.githubusercontent.com/blader/humanizer/main/SKILL.md)

8. **[i-have-adhd](https://github.com/ayghri/i-have-adhd)｜59,955 Stars**
让回答以行动开头、分步呈现并减少岔题，适合希望降低阅读负担的用户。仓库提供 Claude Code 插件和 Cursor 技能副本。MIT；最近推送 10 月 19 日。属于输出风格调整，不是医疗诊断或治疗工具。 [技能定义](https://raw.githubusercontent.com/ayghri/i-have-adhd/main/skills/i-have-adhd/SKILL.md)

9. **[Cloudflare Security Audit](https://github.com/cloudflare/security-audit-skill)｜58,222 Stars**
把安全审计拆为侦察、覆盖驱动搜索、候选验证、结构化结果和独立复核，适合安全工程师与审计团队。面向能读写文件、调用工具及隔离子代理的编码 Agent；校验器需 Node.js，不能仅有聊天界面。MIT；最近推送 9 月 14 日。只应针对明确授权代码库，在隔离环境审查与验证，发现数量不等于审计质量。 [技能定义](https://raw.githubusercontent.com/cloudflare/security-audit-skill/main/skills/security-audit/SKILL.md)

10. **[Hallmark](https://github.com/Nutlope/hallmark)｜47,008 Stars**
为新网站、页面和组件提供主题、设计约束与静态/浏览器复核，适合前端原型和设计工程。支持 Claude Code、Codex、Cursor、OpenCode。MIT；Node 18+，完整浏览器检查另需 Chrome/Puppeteer；素材授权、可选联网和密钥使用应分别审查。 [技能定义](https://github.com/Nutlope/hallmark/blob/main/skills/hallmark/SKILL.md)

**二、近期飙升榜：同一近 7 日窗口的净增排序**
同上 7 天 44.054 秒窗口；前九项用途已在总榜说明，第十项补充说明。

1. [Impeccable](https://github.com/pbakaus/impeccable)：90,575 → 101,565；净增 **+10,990**，增长率 **12.13%**，日均约 **1,570 Stars/日**。
2. [Cloudflare Security Audit](https://github.com/cloudflare/security-audit-skill)：50,226 → 58,222；净增 **+7,996**，增长率 **15.92%**，日均约 **1,142 Stars/日**。
3. [Taste Skill](https://github.com/Leonxlnx/taste-skill)：103,710 → 109,513；净增 **+5,803**，增长率 **5.60%**，日均约 **829 Stars/日**。
4. [Karpathy Guidelines](https://github.com/multica-ai/andrej-karpathy-skills)：225,512 → 229,107；净增 **+3,595**，增长率 **1.59%**，日均约 **514 Stars/日**。
5. [Humanizer](https://github.com/blader/humanizer)：61,509 → 64,423；净增 **+2,914**，增长率 **4.74%**，日均约 **416 Stars/日**。
6. [Diagram Design](https://github.com/cathrynlavery/diagram-design)：61,821 → 64,673；净增 **+2,852**，增长率 **4.61%**，日均约 **407 Stars/日**。
7. [Archify](https://github.com/tt-a1i/archify)：90,473 → 93,141；净增 **+2,668**，增长率 **2.95%**，日均约 **381 Stars/日**。
8. [i-have-adhd](https://github.com/ayghri/i-have-adhd)：59,058 → 59,955；净增 **+897**，增长率 **1.52%**，日均约 **128 Stars/日**。
9. [Last30Days](https://github.com/mvanhorn/last30days-skill)：65,697 → 66,491；净增 **+794**，增长率 **1.21%**，日均约 **113 Stars/日**。
10. [Book to Skill](https://github.com/virgiliojr94/book-to-skill)：36,306 → 36,875；净增 **+569**，增长率 **1.57%**，日均约 **81 Stars/日**。

Book to Skill（总榜第 11，36,875 Stars）：把有权使用的书籍与文档抽取成可按需加载的结构化知识 Skill，适合学习者和知识工程人员。支持 Copilot CLI、Amp、Claude Code、Codex、Hermes、OpenCode 等。MIT；11 月 3 日 UTC 提交改进 PDF 目录章节范围及 CLI 入口测试。抽取质量和版权边界仍要检查，不能把文档的“无幻觉”宣传视为保证。 [技能定义](https://github.com/virgiliojr94/book-to-skill/blob/master/SKILL.md)

**Hallmark：另窗补充，不参与上述飙升排序**
10 月 28 日 09:33:35.431 → 11 月 4 日 09:29:13.855，共 **6 天 23 小时 55 分 38.424 秒**；39,051 → 47,008，净增 **+7,957**，增长率 **20.38%**。起点比主榜晚 5 分 5.630 秒，虽很接近，也不混排或推算成精确 7 日值。

**变化与维护观察**
- 总榜十个席位和顺序不变。共同近 7 日榜前三仍是 Impeccable、Cloudflare Security Audit、Taste Skill；但上期使用 10 月 27 日起点且因晚间补采更长，不能拿两期净增差直接当“加速/减速”。
- 仅在昨晚至今的共同短窗中，Hallmark +490、Impeccable +476、Cloudflare Security Audit +395；这是 12 小时 44 分的快照净增，不是日榜。
- 11 个仓库均未标记 archived。与昨晚完整树 SHA 对照，Last30Days、Diagram Design、Book to Skill 有变化，其余 8 个相同；仓库 pushed_at 变化未必表示默认分支内容变化，也不代表发布新版本。
- 可核实维护证据：[Last30Days 的 Cookie 拒绝处理修复](https://github.com/mvanhorn/last30days-skill/commit/e9c31b868ab9d284c1a988e528c3391c51ebf944)、[Diagram Design 插件清单版本提交](https://github.com/cathrynlavery/diagram-design/commit/f24d98b05e2d7a5e68c2dedfe46e5b7173f50118)、[Book to Skill 的 PDF 目录范围修复](https://github.com/virgiliojr94/book-to-skill/commit/182e1854782c2fce0e49af4f14c583e8c5a51673)。以上均为代码/文档观察，本期没有运行测试或验证修复效果。

**候选范围与限制**
复查 11 个持续跟踪候选的 README、主 SKILL.md、默认分支完整递归树，所有树均未截断；再用公开 GitHub 搜索扩展：
- [topic:agent-skills 且 Stars >45,000](https://api.github.com/search/repositories?q=topic%3Aagent-skills%20stars%3A%3E45000&sort=stars&order=desc&per_page=100)：21 个结果；
- [名称含 skill 且 Stars >45,000](https://api.github.com/search/repositories?q=skill%20in%3Aname%20stars%3A%3E45000&sort=stars&order=desc&per_page=100)：16 个结果。
两组有重叠，API 均未标记 incomplete_results；加上公开网页关键词检索，但名称/主题标签缺漏仍可能漏掉项目。这是**检索样本排名，不是 GitHub 全网穷尽前十**；增长榜也只是有同窗历史的合格样本。

排除多技能合集、清单、框架及大型产品内的技能：例如 anthropics/skills、UI UX Pro Max、[Ponytail](https://github.com/DietrichGebert/ponytail/tree/main/skills)、[Caveman](https://github.com/JuliusBrussee/caveman/tree/main/skills)、[Understand Anything](https://github.com/Egonex-AI/Understand-Anything/tree/main/understand-anything-plugin/skills)；后面三项本期再次读 README 与完整树，确认有多个对外技能。Open Code Review 是较完整的 CLI 产品，Graphify 是图谱 CLI/MCP 产品，不将整个仓库 Stars 归给其中的 Skill。第三方 Skill 榜仅用于发现候选，不采用其可能较旧或不同窗的增长数字。

当前总量的原始来源为各仓库公开 API，例如 [Karpathy](https://api.github.com/repos/multica-ai/andrej-karpathy-skills)、[Impeccable](https://api.github.com/repos/pbakaus/impeccable)、[Cloudflare Security Audit](https://api.github.com/repos/cloudflare/security-audit-skill)、[Hallmark](https://api.github.com/repos/Nutlope/hallmark)；历史端点来自当时保存的同类公开 API 观察记录。历史数值与时间已在本简报列出，本期精确快照也已保存并读回核对；API 当前页面本身不会回放旧总量。

**值得深入看**
1. **Cloudflare Security Audit**：值得阅读其“已确认／待验证／已否定”判定、覆盖台账与独立验证设计，再用明确授权的小型测试仓库隔离评估。推荐理由是证据流程，不是增长名次。
2. **Book to Skill**：PDF 目录边界与抽取来源可追溯性适合做小范围验证；选择自有或公开许可文档，人工对照章节范围与回答出处，检查抽取遗漏，不直接拿真实私密文档试用。

热度不等于成熟度或安全性。本期只读取公开资料，未登录、安装、运行项目或触发项目外部操作。

[本期归档](https://github.com/BruceShenC/News/blob/main/daily/2026-11-04/ai-skills-ranking.md)
