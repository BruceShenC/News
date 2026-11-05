**AI 独立 Skill 热度双榜｜2026 年 11 月 5 日**

今天总 Stars 前十没有换位。共同近 7 日净增仍由 Impeccable、Cloudflare Security Audit 领跑；Hallmark 今天首次取得同一周基线，以 +7,201 排第三。值得重点看的是 Taste：今天北京时间 07:41 正式发布 v2.5.0。

**统计口径**
- 总 Stars：北京时间 **11 月 5 日 09:31:34.329–09:31:36.847**，重新请求公开 GitHub 仓库 API，禁用抓取缓存。这是采集批次区间，不是完全同时的服务器快照。
- 增长榜：统一比较 **10 月 29 日 09:34:05.427 → 11 月 5 日 09:31:34.329**，共 **6 天 23 小时 57 分 28.902 秒**。11 个仓库均按数字 ID 匹配；起点批次耗时约 4.6 秒，终点约 2.5 秒。称“近 7 日”，不冒充精确 168 小时。
- 全部 11 个既有候选重读 README、主 SKILL.md 和完整文件树；Taste、Impeccable、Hallmark 的树有变化，其余 8 个与昨日一致。补搜高星仓库、Agent Skills 主题及公开网页。**这是已核验候选池榜单，不是 GitHub 全网穷尽排名。**
- 框架、市场、合集和内含 Skill 的综合软件不参加。再次核实 Caveman、Ponytail、UI UX Pro Max 分发多个技能；Open Code Review 是更广的 CLI 产品，也含多个技能。Impeccable 的宿主副本、Archify 的贡献者内部辅助技能，不算额外对用户分发的产品技能。

**一、总 Stars 前十**

1. [Karpathy Guidelines](https://github.com/multica-ai/andrej-karpathy-skills)：**229,518**
明确假设、减少过度工程、限制修改范围并定义验证目标，适合研发与 QA；Claude Code、Cursor 等。Skill 声明 MIT，GitHub API 许可证字段仍为空；最近推送 4 月 20 日，热度不代表近期维护，提示词也不等于质量门禁。

2. [Taste](https://github.com/Leonxlnx/taste-skill)：**110,093**
公司与产品网站的设计、素材组织和真实浏览器检查，适合前端及设计工程师；Claude Code、Codex 等 Agent Skills 宿主。MIT；需 Node 22+、Chrome/Edge/Chromium，新增照片、字体与品牌图标涉及联网及各自许可。当前仍只分发一个 Skill，旧技能已迁到其他仓库。

3. [Impeccable](https://github.com/pbakaus/impeccable)：**102,846**
界面设计、审查、润色及自动规则检测，适合前端和 GUI 质量团队；Claude Code、Codex、Cursor、Copilot、Hermes 等。Apache-2.0；含引擎、hooks 与 live 模式。**README 明示 live-copy 的命令行回退可关闭审批和沙箱，并继承环境变量**，不能把它当作纯提示词直接装入敏感工作区。

4. [Archify](https://github.com/tt-a1i/archify)：**93,480**
将代码架构、流程或解释内容做成交互式 HTML 图解，适合架构评审与技术文档；Claude Code、Codex、Cursor、OpenCode 等。Apache-2.0；结构校验不保证业务语义正确，图中的关系仍要回查代码证据。

5. [Last30Days](https://github.com/mvanhorn/last30days-skill)：**66,621**
汇总近期多平台讨论及公开信息，适合研究、产品和内容团队；Claude Code、Codex、Cursor、Copilot、Gemini CLI 等。MIT；部分来源要 API 密钥、浏览器会话或付费服务，应分别审查权限和费用，互动量不是事实可靠性。

6. [Diagram Design](https://github.com/cathrynlavery/diagram-design)：**65,024**
生成带品牌风格的 HTML/SVG 图解，支持架构、流程、数据等多类图表，适合技术沟通与方案评审；Claude Code、Codex、Cursor、Copilot 等。MIT；需核对连线含义、数值、引用及品牌适配。

7. [Humanizer](https://github.com/blader/humanizer)：**64,861**
识别并改写机械化表达，适合编辑与写作者；Claude Code、Codex 及兼容 Agent Skills 的宿主。MIT；改写后仍要核对事实和原意，文风改善不保证真实性或规避 AI 检测。

8. [I Have ADHD](https://github.com/ayghri/i-have-adhd)：**60,083**
让回答行动优先、步骤简短、减少阅读负担，适合偏好清晰行动说明的人；Claude Code、Codex、Cursor 等。MIT；它是输出风格技能，不是诊断或治疗工具，具体时间估计也需要证据支持。

9. [Cloudflare Security Audit](https://github.com/cloudflare/security-audit-skill)：**59,441**
编排代码安全审查、候选漏洞验证、独立证据复核和报告，适合安全与平台 QA；宿主需支持工具调用、子 Agent 及相关校验脚本。MIT；目标代码运行需要 OS 级隔离，无法隔离时应保留待验证状态。最近推送仍为 9 月 14 日。

10. [Hallmark](https://github.com/Nutlope/hallmark)：**47,965**
以主题、设计约束和浏览器检查改善网站页面，提供明确调用的改版与审计模式，适合前端和设计团队；Claude Code、Codex、Cursor、OpenCode。MIT；基础脚本需 Node 18+，浏览器检查另需 Chrome 和 puppeteer-core；照片遵循各自许可，不应默认改写已有设计体系。

**二、近 7 日 Star 净增前十**

按上述统一批次窗口的总 Stars 相减，括号为相对起点的增长率，不与 GitHub Trending 的 today/this week 混排。

1. Impeccable：**+10,089（10.88%）**，92,757 → 102,846
2. Cloudflare Security Audit：**+7,731（14.95%）**，51,710 → 59,441
3. Hallmark：**+7,201（17.67%）**，40,764 → 47,965
4. Taste：**+5,582（5.34%）**，104,511 → 110,093
5. Karpathy Guidelines：**+3,514（1.55%）**，226,004 → 229,518
6. Humanizer：**+2,902（4.68%）**，61,959 → 64,861
7. Diagram Design：**+2,559（4.10%）**，62,465 → 65,024
8. Archify：**+2,471（2.72%）**，91,009 → 93,480
9. I Have ADHD：**+851（1.44%）**，59,232 → 60,083
10. Last30Days：**+827（1.26%）**，65,794 → 66,621

用途、宿主及风险见总榜。候选 [Book to Skill](https://github.com/virgiliojr94/book-to-skill) 为 **36,970 Stars**，同窗口 **+543（1.49%）**，未入两榜前十。它把书籍或文档转为可加载的知识技能，适合知识管理；MIT 不覆盖输入资料的版权，敏感文档也不宜随意公开。

本期 11 个候选都有可比较历史，没有用零补缺失数据。历史起点见[10 月 29 日记录](https://github.com/BruceShenC/News/blob/main/daily/2026-10-29/ai-skills-ranking.md)，对比口径见[昨日记录](https://github.com/BruceShenC/News/blob/main/daily/2026-11-04/ai-skills-ranking.md)。今日带时间的精确快照已保存，供后续比较。

**变化与值得读的更新**
- **Hallmark 的第三名来自首次取得共同周基线**。昨天因起点不同而单列，今天能正式并排比较，不是从零突然增长。Book to Skill 移出近 7 日前十是候选可比范围变化，并非掉星。
- 总榜仍未换位。Cloudflare 与 I Have ADHD 的差距由昨日 1,733 缩至 **642**；Humanizer 与 Diagram Design 的差距由 250 缩至 **163**。这是观察到的差距，不能据此承诺反超。
- 作为短期参照，昨日到今天的窗口为 **24 小时 2 分 20.474 秒**：Impeccable +1,281、Cloudflare +1,219、Hallmark +957 居前三。昨日那期短窗口只有约 12 小时 44 分，不能直接拿两期净增比较“加速”。
- **Taste v2.5.0 正式发布**：北京时间 11 月 5 日 07:41:47，重构网站设计流程，提供真实产品 UI 素材和多宽度浏览器检查。发布说明称有 100 多项检查，这是项目自述，未在本次独立运行验证。发布之后 main 又补入逐个字体加载告警、页面服务根目录和分页面报告目录修复，须区分 release 与后续 main。[正式发布](https://github.com/Leonxlnx/taste-skill/releases/tag/v2.5.0)、[字体告警提交](https://github.com/Leonxlnx/taste-skill/commit/9b4bf937a1f69348126959c15d2f99a73e65699b)
- **Impeccable 的 Vue 兼容修复**：11 月 4 日 main 调整 live 预览 CSS 的注入形式，并保护用户已有 style 块。最新可见 Skill 正式版仍是 10 月 26 日 v4.8.0，不将 main 修复称为新发布。[修复提交](https://github.com/pbakaus/impeccable/commit/8d0229af28b400f3c9be7923179ff06bb977f357)、[正式版本](https://github.com/pbakaus/impeccable/releases/tag/skill-v4.8.0)
- **Hallmark 补充 1440 宽度检查**：11 月 4 日 main 合入机械布局检查扩展，适合关注桌面断点覆盖的团队阅读，不代表所有实际页面已通过测试。[提交](https://github.com/Nutlope/hallmark/commit/de4999e0cd7a7a41df0b4e6903fa02fe9f1492a8)

**建议优先看两项**
- **Taste**：用不含敏感信息的示例页，比较 2.5 的多宽度检查能否发现字体加载失败、横向溢出、空素材等问题；固定版本后再评估误报与设计自由度。
- **Cloudflare Security Audit**：重点读“候选发现 → 独立验证 → 结构化记录”的边界，适合借鉴到 QA 证据复核流程。先评估文档与示例，不把热度当成审计效果。

热度不等于成熟度、安全性或生产效果。本期只读公开资料，未安装、运行项目、登录或进行外部写入。
