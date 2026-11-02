**AI 独立 Skill 热度双榜｜2026-11-02**

**北京时间观察：2026-11-02 09:33:59–09:34:00。** 总 Stars 前十顺序未变；同口径近 7 日榜仅有 **9 项**，Impeccable、Cloudflare Security Audit、Taste 领先。以下均是已检索且核验资格的样本榜，**不是 GitHub 全网穷尽前十**。

**口径与覆盖**

- 本期实时读取 11 个跟踪仓库的公开 GitHub API（禁用抓取缓存），逐项核对数值仓库 ID、README、主要 SKILL.md 与未截断的递归目录树；另检索 [agent-skills 主题且 Stars > 40,000](https://github.com/search?q=topic%3Aagent-skills+stars%3A%3E40000&type=repositories)（返回 23 项）、[名称含 skill 且 Stars > 40,000](https://github.com/search?q=skill+in%3Aname+stars%3A%3E40000&type=repositories)（返回 17 项），辅以公开网页关键词搜索。不同搜索结果有重叠，其星数不混入本期同批快照。
- 排除通用框架、市场、awesome 与多 Skill 合集，如 anthropics/skills、mattpocock/skills、marketing skills；延续排除 UI UX Pro Max 的多技能分发，以及 career-ops 综合应用。大仓库子目录 Skill 不借用母仓库 Stars。
- 同一技能的多平台复制、测试 fixture、内部贡献者辅助技能不另计独立产品：Impeccable 的多平台目录、Archify 的内部维护技能不等同多款对外技能合集。
- 搜索依赖命名、主题与索引，可能漏掉未标注或尚未索引的仓库；未核验候选不强行补位。未登录、安装或执行项目。
- 两端公开 API 总 Stars 相减为**快照净增**，包括取消 Star 的影响；不是新增 Star 事件计数，也不是 GitHub Trending 的 today／this week。本期不使用 Trending 增长数、安装量或总星数／仓龄。

**一、总 Stars 榜：已核验样本前十**

所有总量均属上述同批观察，来源为各仓库公开 API 的 stargazers_count 字段（如 [Impeccable API](https://api.github.com/repos/pbakaus/impeccable)、[Cloudflare API](https://api.github.com/repos/cloudflare/security-audit-skill)）；实时页面此后可能变化。仓库链接可看 README；“定义”链接指向本期核验的技能入口。用途仅在本节说明，后面的增长榜不重复。

1. **[Karpathy Guidelines](https://github.com/multica-ai/andrej-karpathy-skills) — 228,010 Stars**（[定义](https://raw.githubusercontent.com/multica-ai/andrej-karpathy-skills/main/skills/karpathy-guidelines/SKILL.md)）  
   把澄清假设、最小改动、可验证目标写成编码行为约束；适合开发者与代码审查者。Claude Code 插件／Cursor 规则；不能代替自动化测试。Skill 声明 MIT，但仓库 API 许可证字段为空。

2. **[Taste Skill](https://github.com/Leonxlnx/taste-skill) — 107,811 Stars**（[定义](https://raw.githubusercontent.com/Leonxlnx/taste-skill/main/skills/taste-skill/SKILL.md)）  
   让前端生成先确定视觉方向、参考图，再浏览器检查；适合前端与设计工程师。Claude Code、Cursor、Codex 等 Skill 宿主；浏览器工具需 Node.js 22+ 与本机 Chromium 系浏览器，图片搜索涉及联网。MIT。

3. **[Impeccable](https://github.com/pbakaus/impeccable) — 99,366 Stars**（[定义](https://raw.githubusercontent.com/pbakaus/impeccable/main/cursor-plugin/skills/impeccable/SKILL.md)）  
   将设计规范、界面审计与确定性检测结合；适合设计系统和前端团队。Claude Code、Cursor、Codex、Copilot、Hermes 等。Apache-2.0；引擎下载、Hooks 和项目校验脚本需要单独审查权限。

4. **[Archify](https://github.com/tt-a1i/archify) — 92,399 Stars**（[定义](https://raw.githubusercontent.com/tt-a1i/archify/main/archify/SKILL.md)）  
   输出可验证布局的架构图、流程图与说明页；适合架构师、技术文档作者。Claude Code、Codex、Cursor、OpenCode。Apache-2.0；布局／schema 校验不证明架构事实正确；可关闭可选联网更新提醒。

5. **[Last30Days](https://github.com/mvanhorn/last30days-skill) — 66,262 Stars**（[定义](https://raw.githubusercontent.com/mvanhorn/last30days-skill/main/skills/last30days/SKILL.md)）  
   聚合近期跨平台资料并交叉组织研究结果；适合研究者、产品与开发人员。Claude Code、Codex、Cursor、Copilot、Gemini CLI 等。MIT；各来源 API、浏览器会话与费用须独立审查，不能把来源互动量当事实可信度。

6. **[Diagram Design](https://github.com/cathrynlavery/diagram-design) — 63,982 Stars**（[定义](https://raw.githubusercontent.com/cathrynlavery/diagram-design/main/skills/diagram-design/SKILL.md)）  
   生成品牌化、自包含 HTML＋SVG 图解；适合技术写作者、产品与架构沟通。Claude Code、Codex、Copilot、Cursor 等。MIT；图形清晰不代表输入数据或关系正确。

7. **[Humanizer](https://github.com/blader/humanizer) — 63,497 Stars**（[定义](https://raw.githubusercontent.com/blader/humanizer/main/SKILL.md)）  
   减少模式化措辞并保留原意；适合编辑、文档作者。Claude Code、Codex 及支持 Skill 的 Agent。MIT；须人工复核事实和语气，项目明确不以绕过 AI 检测为目标。

8. **[I Have ADHD](https://github.com/ayghri/i-have-adhd) — 59,700 Stars**（[定义](https://raw.githubusercontent.com/ayghri/i-have-adhd/main/skills/i-have-adhd/SKILL.md)）  
   让 Agent 回复更简洁、可扫描、下一步明确；适合偏好低认知负担交互的用户。Claude Code、Cursor 等。MIT；属于沟通风格调整，不是医疗干预。

9. **[Security Audit Skill](https://github.com/cloudflare/security-audit-skill) — 56,407 Stars**（[定义](https://raw.githubusercontent.com/cloudflare/security-audit-skill/main/skills/security-audit/SKILL.md)）  
   将代码侦察、漏洞候选、独立验证与结构化报告组织成单一审计技能；适合安全工程师与代码审计者。支持 Skill 的编码 Agent，需子 Agent、文件／命令工具及 Node.js 校验器。MIT；运行不可信目标必须有操作系统级隔离，缺少条件应保留待验证状态。

10. **[Hallmark](https://github.com/Nutlope/hallmark) — 44,908 Stars**（[定义](https://github.com/Nutlope/hallmark/blob/main/skills/hallmark/SKILL.md)）  
   围绕真实页面任务生成、重设计或审计界面，并用浏览器检查与独立审阅把关；适合独立开发者与前端团队。Claude Code、Codex、Cursor、OpenCode。MIT；浏览器执行及图片授权需审查，不应自动改写既有设计。

**二、近期飙升榜：同窗口近 7 日，可靠项共 9 个**

**窗口：2026-10-26 01:29:57.824 → 2026-11-02 01:33:59.483 UTC**，即 7 日 4 分 1.659 秒（605,041.659 秒）。为尽量接近近 7 日而使用保存的定时快照，并非精确滚动 168 小时。各批次有秒级采集偏差；按统一批次起点算窗口。

1. [Impeccable](https://github.com/pbakaus/impeccable)：87,735 → 99,366；净增 **+11,631（13.26%）**。
2. [Security Audit Skill](https://github.com/cloudflare/security-audit-skill)：46,959 → 56,407；净增 **+9,448（20.12%）**。
3. [Taste Skill](https://github.com/Leonxlnx/taste-skill)：102,505 → 107,811；净增 **+5,306（5.18%）**。
4. [Karpathy Guidelines](https://github.com/multica-ai/andrej-karpathy-skills)：223,865 → 228,010；净增 **+4,145（1.85%）**。
5. [Diagram Design](https://github.com/cathrynlavery/diagram-design)：61,060 → 63,982；净增 **+2,922（4.79%）**。
6. [Humanizer](https://github.com/blader/humanizer)：60,596 → 63,497；净增 **+2,901（4.79%）**。
7. [I Have ADHD](https://github.com/ayghri/i-have-adhd)：58,729 → 59,700；净增 **+971（1.65%）**。
8. [Last30Days](https://github.com/mvanhorn/last30days-skill)：65,460 → 66,262；净增 **+802（1.23%）**。
9. [Book-to-Skill](https://github.com/virgiliojr94/book-to-skill)：36,087 → 36,713；净增 **+626（1.73%）**。

按净增降序，不以增长率替换排序；增长率＝净增／期初总量。未将净增除以仓龄，也不把此项称为“日均”。

**Book-to-Skill 补充说明：** 总量榜外第 11 名，36,713 Stars；将技术书、文档或资料集合提取成结构化知识 Skill，适合开发者与知识管理者；支持 Copilot CLI、Amp、Claude Code、Codex、Hermes、OpenCode 等。MIT；应核对内容版权、提取完整性与原文保真度。[技能定义](https://github.com/virgiliojr94/book-to-skill/blob/master/SKILL.md)

**不混排的独立观察：** Archify 的 10 月 26 日首个端点较晚，窗口为 **2026-10-26 01:31:07.349 → 2026-11-02 01:33:59.483 UTC**（604,972.134 秒）；89,194 → 92,399，净增 **3,205（3.59%）**。这不是与上述榜单完全等长的窗口，故单列、不塞入第十名。Hallmark 缺少对应 7 日端点，增长未知，不写 0。

**短窗补充：近 24 小时净增前十**

**独立窗口：2026-11-01 01:33:02.736 → 2026-11-02 01:33:59.483 UTC**，24 小时 56.747 秒（86,456.747 秒），与近 7 日榜分开统计，不声称精确 24 小时。

1. Impeccable：98,311 → 99,366；净增 **+1,055（1.07%）**。
2. Security Audit Skill：55,361 → 56,407；净增 **+1,046（1.89%）**。
3. Hallmark：43,948 → 44,908；净增 **+960（2.18%）**。
4. Taste Skill：107,063 → 107,811；净增 **+748（0.70%）**。
5. Humanizer：62,988 → 63,497；净增 **+509（0.81%）**。
6. Karpathy Guidelines：227,516 → 228,010；净增 **+494（0.22%）**。
7. Diagram Design：63,492 → 63,982；净增 **+490（0.77%）**。
8. Archify：92,157 → 92,399；净增 **+242（0.26%）**。
9. Last30Days：66,124 → 66,262；净增 **+138（0.21%）**。
10. I Have ADHD：59,598 → 59,700；净增 **+102（0.17%）**。

Book-to-Skill 同窗 +56，在前十之外。所有净增均来自本期与历史保存的公开总量观察；本期已保存精确数字、UTC 时间、仓库 ID 和来源，供后续同口径比较。

**榜单变化与维护观察**

- **总量榜及近 7 日共同样本顺序均未变化。** 短窗榜 Humanizer 从第 7 升至第 5、Diagram 从第 8 升至第 7；Karpathy 第 5 → 6、Archify 第 6 → 8。相邻期窗口略有不同，不据此断言“持续加速”。
- **11 个仓库均未归档；两项默认分支树发生变化。** Impeccable 在 11 月 1 日合入 [VS Code／Windows Hook 兼容处理](https://github.com/pbakaus/impeccable/commit/a30a56dfc96b7cb7d7026be7c8cbd487e8983509)及 [Angular 图片绑定识别](https://github.com/pbakaus/impeccable/commit/f5c80432f883a0ee437735305eafc2a9e1bdb7a0)。这属于主分支维护，不等于新正式版本；本次 latest release API 返回的 [CLI 4.8.0](https://github.com/pbakaus/impeccable/releases/tag/cli-v4.8.0) 仍发布于 10 月 26 日。
- Last30Days 在 11 月 1 日出现 [YouTube bot-check 后保留转录时间预算](https://github.com/mvanhorn/last30days-skill/commit/6894d44e088c449ecdd7246a18763fddba1ff805)、[stdin 研究计划输入](https://github.com/mvanhorn/last30days-skill/commit/f01a451a5aa745cd1af4497fb7d79e171917364e)等维护；[最新正式 v3.31.0](https://github.com/mvanhorn/last30days-skill/releases/tag/v3.31.0)仍为 10 月 27 日。其余 9 项树与昨日一致；push 时间变化不自动证明默认分支代码变更。
- Karpathy 最近公开 push 仍为 4 月 20 日，Cloudflare 为 9 月 14 日；本周 Star 增长不能替代维护活跃度证据。没有自行运行测试或验证作者效果主张。

**值得深入阅读／隔离验证的 2 项**

1. **Impeccable：** 同口径周窗和短窗均领先，且近期仍维护宿主兼容性。建议先读 Hook 权限说明，再在无敏感信息的小型本地 UI 项目对照检查；README 明示 Claude Code 已安装命令 Hook 可能在拒绝模型命令后仍运行，首次编辑还可能下载引擎。不要把“允许 Skill”误当“只读提示词”。
2. **Last30Days：** 排名不靠前，但跨来源研究中的超时、输入及来源失效边界持续维护，值得阅读。建议仅用公开非敏感主题，对照人工来源抽样核验日期、实体一致性及遗漏；这是建议方案，不是本期已执行结果。

**热度不等于成熟度、安全性或有效性。** 设计／审计类技能的多步骤工作流也不构成事实、漏洞或合规结论的保证。

[本期归档](https://github.com/BruceShenC/News/blob/main/daily/2026-11-02/ai-skills-ranking.md)
