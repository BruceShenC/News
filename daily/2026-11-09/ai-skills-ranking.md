**AI 独立 Skill 热度双榜｜2026年11月9日**

今天的变化：Cloudflare Security Audit 总星升至第6，超过 Humanizer 368 Stars，距离第5名 Last30Days 还有134；近周净增10,483，继续领先。Humanizer 在近周增长榜升至第5。Impeccable 已正式发布 Skill v4.8.1，Archify 则发布了单独版本的 DeepSeek Harness 适配包，两者需要区分核心技能与接入组件。

**统计范围与时间**

本期总星统一采自公开 GitHub 仓库 API，北京时间 **11月9日09:26:23.304–09:26:26.067**。11个既有候选的数字仓库 ID 全部与历史快照匹配，逐一读取官方 README、主 Skill 定义和未截断的完整递归文件树。Impeccable、Archify、Diagram Design 的主分支文件树较昨日变化，其余8项不变。稍后搜索中的星数不覆盖本批观察。

补搜[agent-skills 主题且超过3万星](https://github.com/search?q=topic%3Aagent-skills+stars%3A%3E30000&type=repositories)、[名称含 skill 且超过3万星](https://github.com/search?q=skill+in%3Aname+stars%3A%3E30000&type=repositories)、[描述含 skill 且超过5万星](https://github.com/search?q=skill+in%3Adescription+stars%3A%3E50000&type=repositories)，结合网页发现及补充候选复核。**这是已检索且通过资格核验的样本榜，不是 GitHub 全网穷尽榜**；低星但快速增长、未使用相关关键词的项目可能遗漏。

只纳入独立分发的单一 Skill。框架、市场、awesome 清单、多技能合集和仅内嵌 Skill 的完整软件产品不纳入；相同 Skill 的平台副本、测试样例和仅供贡献者使用的内部辅助技能不重复计数。

本期复核的排除例：
- [Caveman](https://github.com/JuliusBrussee/caveman)：虽可单独安装其中一个写作 Skill，当前仓库同时分发多个技能及代理、压缩和模型路由功能，不能把整仓 Stars 算给其中一个技能。
- [Reverse Skill](https://github.com/zhaoxuya520/reverse-skill)：README 明列45个核心模块，是安全技能路由包。
- [女娲 Skill](https://github.com/alchaincyf/nuwa-skill)：除生成器本体，还公开分发多个可独立安装的人物/主题 Skill，按多技能分发口径不纳入。
UI UX Pro Max 七技能合集及 Graphify 综合软件继续排除。排除属于统计资格判断，不代表热度下降。

**一、总 Stars 前十**

1. [Karpathy Guidelines](https://github.com/multica-ai/andrej-karpathy-skills)｜**230,958**
强调明确假设、最小改动和可验证目标，适合研发与 QA。支持 Claude Code，提供 Cursor 规则。README 与 Skill 声明 MIT，API 仍未识别许可证；最近推送4月20日。提示规则不能代替测试和代码审查。

2. [Taste](https://github.com/Leonxlnx/taste-skill)｜**112,052**
指导公司网站的美术方向、真实素材、文案和浏览器检查，适合前端及设计工程师。提供 Claude Code、Codex 等接入方式；MIT。需 Node 22+ 与 Chrome、Edge 或 Chromium；脚本、hooks、联网取素材和素材许可需分别审查。当前 Skill 为2.5.1，本期文件树未变。

3. [Impeccable](https://github.com/pbakaus/impeccable)｜**106,230**
把 UI 设计、审查、润色和规则检测组织成一个 Skill，适合前端、设计与界面质量团队。支持 Claude Code、Codex、Cursor、Copilot、Gemini CLI 等；Apache-2.0。含执行引擎与 hooks，首次运行可能下载二进制。尤其注意 README 披露：live copy 的 Apply 在没有活跃聊天会话时，可能启动关闭审批与沙箱的 CLI 回退，不宜未经审查启用。

4. [Archify](https://github.com/tt-a1i/archify)｜**94,745**
把架构、流程和代码关系转成可交互 HTML 图解，也能制作带来源的一页解释，适合架构师和技术文档作者。支持 Claude Code、Codex、Cursor、OpenCode；Apache-2.0。结构校验不能证明业务语义正确，第三方组件许可需另查。

5. [Last30Days](https://github.com/mvanhorn/last30days-skill)｜**67,024**
汇总近期多平台讨论并保留来源，适合研究、产品与内容团队。支持 Claude Code、Codex 等兼容宿主；MIT。部分数据源涉及 API 密钥、登录会话或付费额度，热度不能替代事实核验。

6. [Cloudflare Security Audit](https://github.com/cloudflare/security-audit-skill)｜**66,890**
组织多 Agent 安全审查、独立验证和结构化报告，适合安全、平台及 QA 团队。宿主需支持工具调用与并行子 Agent，校验器需 Node.js；MIT，最近推送仍为9月14日。执行目标代码须有操作系统级隔离、网络和资源限制，不可直接运行不可信项目。

7. [Humanizer](https://github.com/blader/humanizer)｜**66,522**
减少机械化写作表达并保留原意，适合编辑和写作者。支持 Claude Code、Codex 及兼容 Agent Skills 的宿主；MIT。文风改善不等于事实准确，也不保证任何 AI 检测结果。

8. [Diagram Design](https://github.com/cathrynlavery/diagram-design)｜**65,889**
生成自包含 HTML/SVG 图解，适合技术文档、方案评审和汇报。支持 Claude Code、Codex、Cursor、Copilot、Factory Droid、Pi 等；MIT。默认外载 Google 字体，可选系统字体；连线、数值、图意及离线可用性仍需检查。

9. [I Have ADHD](https://github.com/ayghri/i-have-adhd)｜**60,545**
让回答行动优先、步骤明确，适合偏好较低阅读负担的用户。提供 Claude Code 插件和 Cursor 副本，也可用于兼容 Skill 的宿主；MIT，最近推送10月19日。这是输出风格指导，不是诊断或治疗工具。

10. [Hallmark](https://github.com/Nutlope/hallmark)｜**50,979**
提供品牌页面设计、审计和明确调用后的改版流程，适合前端及设计团队。支持 Claude Code、Codex、Cursor、OpenCode；MIT。脚本需 Node 18+，完整渲染检查另需 Chrome 与 puppeteer-core，照片遵循各自许可及署名要求。

**二、近7日 Star 净增前十**

统一窗口：北京时间 **11月2日09:33:59.483 → 11月9日09:26:23.304**，实际 **6天23小时52分23.821秒，共604,343.821秒**，不是严格168小时。

使用同一组11个仓库的两次公开 API 总星快照相减，按净增排序，括号为相对起点增长率。起点批次结束于09:34:00.390，终点结束于09:26:26.067，存在秒级请求先后；统一时间是批次观察锚点，并非所有仓库同时变更的时刻。不是 GitHub Trending 的“today/this week”，也不按当前保留 Star 的创建时间倒推历史净增。[11月2日历史观察记录](https://github.com/BruceShenC/News/blob/main/daily/2026-11-02/ai-skills-ranking.md)

1. Cloudflare Security Audit：**+10,483（18.58%）**
2. Impeccable：**+6,864（6.91%）**
3. Hallmark：**+6,071（13.52%）**
4. Taste：**+4,241（3.93%）**
5. Humanizer：**+3,025（4.76%）**
6. Karpathy Guidelines：**+2,948（1.29%）**
7. Archify：**+2,346（2.54%）**
8. Diagram Design：**+1,907（2.98%）**
9. I Have ADHD：**+845（1.42%）**
10. Last30Days：**+762（1.15%）**

用途、平台和风险见总榜。第11个持续跟踪候选 [Book to Skill](https://github.com/virgiliojr94/book-to-skill) 总星 **37,357**，同窗口净增 **644（1.75%）**，未进入两榜前十。它把书籍或文档整理为可追溯技能材料，适合知识整理，提供标准 Skill；MIT 覆盖项目代码，输入书籍及输出材料的版权需另行确认。

补充候选 [归藏 PPT Skill](https://github.com/op7418/guizang-ppt-skill) 在北京时间 **09:27:49.464–09:27:50.519** 观察到 **29,482 Stars**。已复核根目录唯一 SKILL.md，生成单文件 HTML 演示稿，适合分享、产品发布与排练；支持 Claude Code、Codex 及具备文件和命令能力的宿主。AGPL-3.0，当前交付主线是 HTML 而非原生 PPTX，修改及网络服务分发义务、字体图片许可需分别审查。低于总榜门槛，且没有匹配的11月2日端点，近周增长未知，不填零、不混入增长榜。

**变化与值得细读的两项**

- 相比[昨日](https://github.com/BruceShenC/News/blob/main/daily/2026-11-08/ai-skills-ranking.md)，总榜只有 Security Audit 与 Humanizer 交换第6、第7位，两者星数均增加。Security Audit 现比 Last30Days 少134星，这是本次采样差值，不代表之后仍保持这一顺序。
- 近周增长榜 Humanizer 超过 Karpathy Guidelines，升至第5，其余名次不变。另看相邻观察，窗口为 **11月8日09:32:02.133 → 11月9日09:26:23.304，共23小时54分21.171秒**：Security Audit +1,118，Hallmark +680，Impeccable +664，Humanizer +518，Taste +379，Karpathy Guidelines +327，Archify +265，Diagram Design +201，Book to Skill +140，I Have ADHD +108，Last30Days +81。此短窗仅供辅助，不与近周榜混排，也不把滚动窗口净增变化直接称作增长加速。

**1. Impeccable v4.8.1：重点看检查流程与权限边界。**
[正式 Skill Release](https://github.com/pbakaus/impeccable/releases/tag/skill-v4.8.1) 发布于北京时间11月8日10:47:12，晚于昨日采样。它调整 Stop hook：逐项修复或给出跳过理由，无法判断时再询问；增加 Hermes 设计 hook，修复多宿主 hook 接线、重复报告和部分视觉误报；live 模式加入元素移除及撤销，并修复 Vue/Svelte 预览等问题。这是实际发布的 Skill 版本，不能与 CLI v4.8.1 或 engine v0.1.19 混为一项。

另一个更晚的[主分支 PR #1342](https://github.com/pbakaus/impeccable/pull/1342) 已合并，增加 hook 选项及 Codex MCP 审批配置写入/回退能力，作者报告43项测试通过。它修改的是宿主配置与审批行为，适合先读“旧值保存、漂移检测、精确撤销”的设计；不能把主分支合并写成已包含在 Skill v4.8.1 的功能，也不能仅凭测试通过就允许修改现有审批配置。上述测试是作者报告，本期未执行。

**2. Diagram Design：一个适合图表 QA 的负向测试。**
北京时间11月9日04:01:25 的[箱线图检查修复](https://github.com/cathrynlavery/diagram-design/commit/c2c0c96f088d6f7e0bcbc8044c335854ea881ad7) 针对“标签在源代码中存在，但被 CSS 隐藏，检查却通过”的问题。提交新增了 display:none、visibility:hidden、opacity:0、fill-opacity:0、父分组和嵌套 tspan 的负例，也保留不匹配选择器、非零透明度和空心箱体等应通过的对照。

可借鉴的检查顺序是：先确认数据与标签绑定，再确认最终可见性，最后查看浏览器实际渲染。当前静态检查不实现完整 CSS 层叠，因此父层隐藏、子层恢复可见的情况仍保守拒绝，不能当完整浏览器判定器。随后还修复[独立 SVG 导出中的 HTML 数字字符引用](https://github.com/cathrynlavery/diagram-design/commit/ab75153adae4335a980ca18d0de5ab0b9c06ad49)，并把插件清单更新到2.6.147；本次 GitHub Releases 列表为空，故只称主分支改动，不称正式 GitHub Release。本期仅审阅代码与测试定义，未运行复现。

**另一个需要区分版本的更新**

[Archify DSH v1.1.0](https://github.com/tt-a1i/archify/releases/tag/archify-dsh-v1.1.0) 发布于北京时间11月9日00:38:47，是 DeepSeek Harness 社区适配包，内含 Archify 3.2.0；不是核心 Skill 升到新版本。适配对象是开发预览版 @deepseek-ai/dsh@0.2.1-rc.2，要求 Node ^22.19.0 或 >=24.0.0。项目公开说明：原生 Desktop/Electron 验收未宣称完成，个人或项目中的同名 Skill 可能覆盖插件内版本，因此“插件更新成功”不等于“实际加载技能已更新”。外层适配包 MIT、内含 Skill Apache-2.0，需分别核查。[版本与加载优先级说明](https://github.com/tt-a1i/archify/blob/main/integrations/deepseek-harness/README.md)

**来源与局限**

总星来自各仓库公开 API，例如 [Security Audit 元数据](https://api.github.com/repos/cloudflare/security-audit-skill)；用途和平台以当日官方 README 与 SKILL.md 为依据，适配声明未经本期实机测试。主分支完整树核验示例：[Impeccable](https://api.github.com/repos/pbakaus/impeccable/git/trees/main?recursive=1)、[Archify](https://api.github.com/repos/tt-a1i/archify/git/trees/main?recursive=1)。动态页面之后可能变化，本文数字以所列观察批次为准。

GitHub 仓库 Stars 是关注信号，不是安装量、活跃用户数或效果测评。许可证字段不是完整法律审查，脚本、模型服务、素材与第三方组件可能另有要求。没有从推送时间推断新增功能，没有从星数变化推断增长原因。

本期只读研究公开资料，未登录、安装或运行任何候选项目；外部写入仅限获授权的简报归档。**热度不等于成熟度、安全性或实际效果。**

[本期归档](https://github.com/BruceShenC/News/blob/main/daily/2026-11-09/ai-skills-ranking.md)
