# AI 独立 Skill 热度双榜｜2026-11-21

**总星榜前十不变；Impeccable 近周净增 +4,433 升至第一，仅领先 Security Audit 63 星。** 本期优先看 Last30Days 4.4.0 的证据与保存边界，以及 Archify 对真实发布包的安装验证。

北京时间 **09:29:06.515–09:29:07.319** 批量读取 12 个持续跟踪仓库的公开 GitHub 详情 API，核对仓库 ID、README、主 SKILL.md、目录树及近期 Releases。补搜 [agent-skills 主题](https://github.com/search?q=topic%3Aagent-skills+stars%3A%3E30000&type=repositories)、[名称含 skill](https://github.com/search?q=skill+in%3Aname+stars%3A%3E30000&type=repositories)、[描述含 skill](https://github.com/search?q=skill+in%3Adescription+stars%3A%3E50000&type=repositories)，分别审阅 30、25、30 项，结果有重叠。仅计独立分发的单一 Skill，排除框架、目录、市场、多技能合集及综合软件附带的 Skill。例如 UI UX Pro Max 分发多项技能，Graphify 包含独立 CLI/MCP。**这是已核验样本榜，不是全网穷尽排名**；搜索门槛可能漏掉低星快涨项。

## 一、总 Stars 前十

1. [Karpathy Guidelines](https://github.com/multica-ai/andrej-karpathy-skills) **234,756**：让编码代理明确假设、限制改动、定义可验证目标。适合研发、QA 和代码审查者。支持 Claude Code、提供 Cursor 规则；Skill 声明 MIT，仓库 API 许可字段为空。提示规范不能替代测试。[技能定义](https://github.com/multica-ai/andrej-karpathy-skills/blob/main/skills/karpathy-guidelines/SKILL.md)
2. [Taste](https://github.com/Leonxlnx/taste-skill) **119,349**：网站设计、素材选择和浏览器检查，适合前端与设计工程师。支持 Claude Code、Codex 等；MIT，2.5.1。启用前检查 hooks 和联网素材授权。[技能定义](https://github.com/Leonxlnx/taste-skill/blob/main/skills/taste-skill/SKILL.md)
3. [Impeccable](https://github.com/pbakaus/impeccable) **114,060**：设计、评审和润色界面，适合前端与界面 QA。支持 Claude Code、Codex、Cursor、Copilot、Gemini CLI 等；Apache-2.0，Skill 4.10.0，勿混同引擎 Release 0.1.28。未经审批与隔离评审，勿启用可直接修改界面的 Live Apply。[技能定义](https://github.com/pbakaus/impeccable/blob/main/plugin/skills/impeccable/SKILL.md)
4. [Archify](https://github.com/tt-a1i/archify) **98,349**：将需求和代码证据转成可交互 HTML 架构图，适合架构、文档和 QA。支持 Claude Code、Codex、Cursor、OpenCode；Apache-2.0，核心稳定版 3.2.0。结构正确不等于业务语义正确，分享前检查私有信息。[技能定义](https://github.com/tt-a1i/archify/blob/main/archify/SKILL.md)
5. [Cloudflare Security Audit](https://github.com/cloudflare/security-audit-skill) **76,312**：面向安全和平台 QA 的代码审查记录、复核与结构化报告。完整流程需支持工具和子 Agent 的宿主；MIT。只讨论授权代码的防御性审查，热度上涨不是新功能。
6. [Humanizer](https://github.com/blader/humanizer) **71,676**：减少机械化表达，适合作者和编辑。支持 Claude Code、Codex 等；MIT，4.0.0。改写后的事实，尤其新增细节，应人工核对。[技能定义](https://github.com/blader/humanizer/blob/main/SKILL.md)
7. [Last30Days](https://github.com/mvanhorn/last30days-skill) **71,459**：聚合近期多平台讨论与引用，适合研究、产品和内容团队。支持 Claude Code、Codex、Cursor、Copilot、Gemini CLI 等；MIT，4.4.0。部分来源需登录或付费，热度不等于可信。[技能定义](https://github.com/mvanhorn/last30days-skill/blob/main/skills/last30days/SKILL.md)
8. [Diagram Design](https://github.com/cathrynlavery/diagram-design) **69,292**：输出单文件 HTML/SVG 图解，适合方案、文档和汇报。支持 Claude Code、Codex、Cursor、Copilot、Qoder 等；MIT，README 声明 51 类图形。要单独核对节点、方向、图例和源数据。[技能定义](https://github.com/cathrynlavery/diagram-design/blob/main/skills/diagram-design/SKILL.md)
9. [I Have ADHD](https://github.com/ayghri/i-have-adhd) **61,814**：用编号步骤和跨轮状态提示降低阅读负担。提供 Claude Code 插件、Cursor 副本；MIT，Release 0.4.1。是表达方式指导，不作诊断或治疗。[技能定义](https://github.com/ayghri/i-have-adhd/blob/main/skills/i-have-adhd/SKILL.md)
10. [Hallmark](https://github.com/Nutlope/hallmark) **58,602**：新站设计、审查及明确调用后的改版，适合前端与设计团队。支持 Claude Code、Codex、Cursor、OpenCode；MIT，2.0.0。自动检查不能替代无障碍实测。[技能定义](https://github.com/Nutlope/hallmark/blob/main/skills/hallmark/SKILL.md)

## 二、近 7 日 Star 净增前十

两端均为公开详情 API、同一仓库 ID：[11 月 14 日快照](https://github.com/BruceShenC/News/blob/main/daily/2026-11-14/ai-skills-ranking.md) 北京时间 09:26:30.253 至本期 09:29:06.515，实际 **7 天 2 分 36.262 秒**。按 Star 净差排序，包含取消 Star；括号为相对起点增幅，不等于 Trending 榜或安装量。

1. Impeccable：109,627 → 114,060，**+4,433（4.04%）**
2. Security Audit：71,942 → 76,312，**+4,370（6.07%）**
3. Taste：115,065 → 119,349，**+4,284（3.72%）**
4. Hallmark：54,535 → 58,602，**+4,067（7.46%）**
5. Last30Days：67,564 → 71,459，**+3,895（5.76%）**
6. Humanizer：68,831 → 71,676，**+2,845（4.13%）**
7. Diagram Design：66,897 → 69,292，**+2,395（3.58%）**
8. Archify：96,229 → 98,349，**+2,120（2.20%）**
9. Karpathy Guidelines：232,884 → 234,756，**+1,872（0.80%）**
10. [Book to Skill](https://github.com/virgiliojr94/book-to-skill)：37,780 → 38,583，**+803（2.13%）**。面向知识整理，支持 Claude Code、Codex、Copilot、Amp、Hermes、OpenCode、OpenClaw；MIT，2.0.1，只用有处理权的材料。[技能定义](https://github.com/virgiliojr94/book-to-skill/blob/master/SKILL.md)

I Have ADHD 同窗 +691，列第11；[归藏PPT](https://github.com/op7418/guizang-ppt-skill) 30,190 星、同窗 +432，列第12；支持 Claude Code、Codex，AGPL-3.0，产物是网页幻灯片，不是原生 PPTX。[女娲 Skill](https://github.com/alchaincyf/nuwa-skill/blob/main/SKILL.md) 09:30:23 补查为 38,703 星，未达总星榜第十；缺 11 月 14 日同口径起点，不进净增榜。该 Skill 声明支持 Claude Code、Codex、Cursor 等，MIT；生成观点不能当成人物原话。

## 三、较昨日变化与两项建议

- 对比[昨日](https://github.com/BruceShenC/News/blob/main/daily/2026-11-20/ai-skills-ranking.md)，Impeccable 增长榜第 3→1，Security Audit 第 1→2，Taste 第 2→3，Diagram Design 第 9→7。窗口起点不同，小差距不代表持续加速。两期采样相距 **23 小时 56 分 43.238 秒**，短窗前三是 Diagram Design +645、Last30Days +643、Impeccable +565，不是严格 24 小时净增。
- **先读 Last30Days 4.4.0。** 11 月 20 日 13:46:34 发布。可把三条版说明做成独立验收：GitHub 证据只来自公开仓库；设置不保存时不生成本地产物；私有追问上下文不进入可公开简报。未实测，也无法把 Star 上涨归因于发版。[发布说明](https://github.com/mvanhorn/last30days-skill/releases/tag/v4.4.0)
- **再看 Archify 的 Windows 安装验收。** 主分支调整真实 Skill ZIP 的 Windows 解包以兼容不同 PowerShell 环境，核心稳定版未随之更新。建议把发布包解压、安装路径、主 Skill 可发现性都纳入发布门禁，而不只验证源码树。[测试提交](https://github.com/tt-a1i/archify/commit/f06c31f74cbf48144e6de9618433d2999c0dbcd9)

公开观察记录（按总星榜顺序，末尾为 Book to Skill、归藏PPT），仓库 ID:Stars：1142983825:234756、1162099055:119349、1097346685:114060、1211139949:98349、1273427276:76312、1136666433:71676、1140843380:71459、1212769188:69292、1238120057:61814、1222917575:58602、1226123343:38583、1219042200:30190。[API 例](https://api.github.com/repos/pbakaus/impeccable)

**项目仓库是否被维护者设为只读归档状态：跟踪的 12 项和补充 1 项均为否（GitHub archived=false）。** 这是项目仓库属性，不是简报归档状态，也不表示维护活跃。平台兼容性采用公开声明；本期未安装或运行项目。
