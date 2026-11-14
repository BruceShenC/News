# AI 独立 Skill 热度双榜｜2026年11月14日

总星榜和近周净增榜前十顺序均未变化。Security Audit仍领跑近周净增；Hallmark与Impeccable的近周净增差缩至86星。今天更值得关注的是Impeccable新版本的联网保存边界，而不是把星数上涨当作技术突破。

## 范围与统计口径

总星观察：北京时间11月14日09:26:30.253–09:26:31.353，公开GitHub仓库API新抓取。逐项复核12个跟踪候选的README、主SKILL.md和未截断递归目录树。多宿主副本、测试夹具不重复计数；Archify的内部开发辅助技能不视作对外产品合集。

补搜[agent-skills主题且超过3万星](https://github.com/search?q=topic%3Aagent-skills+stars%3A%3E30000&type=repositories)、[名称含skill且超过3万星](https://github.com/search?q=skill+in%3Aname+stars%3A%3E30000&type=repositories)、[描述含skill且超过5万星](https://github.com/search?q=skill+in%3Adescription+stars%3A%3E50000&type=repositories)，各取Stars降序前30项，分别返回30、23、30项，有重叠。检索结果的星数仅供发现，排名统一用上述批次快照。

**这是已检索并核验的样本前十，不是GitHub全网穷尽前十。**门槛与结果上限可能漏掉低星快涨项目；支持平台是项目声明，未实际运行。排除框架、awesome清单、市场和多技能合集；例如[UI UX Pro Max](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill)当前树中分发brand、slides、banner-design等多项技能，[Ponytail](https://github.com/DietrichGebert/ponytail)分发audit、debt、gain等技能，不把仓库总星归给其中一项。[Caveman](https://github.com/JuliusBrussee/caveman)包含代理/路由等更广产品，[Graphify](https://github.com/Graphify-Labs/graphify/tree/v8)包含可独立使用的知识图谱CLI和MCP能力，本期仍不把整仓库关注量算成Skill自身热度。此为保守产品边界，不是对这些项目质量的否定。

## 一、总 Stars 前十

1. [Karpathy Guidelines](https://github.com/multica-ai/andrej-karpathy-skills)｜**232,884 Stars**
约束编码代理的假设、改动范围与验证目标，适合研发、QA和审查者；Claude Code、Cursor及兼容宿主。Skill声明MIT，仓库API许可证字段为空；最近推送4月20日。提示规范不能代替测试。
[技能定义](https://raw.githubusercontent.com/multica-ai/andrej-karpathy-skills/main/skills/karpathy-guidelines/SKILL.md)。

2. [Taste](https://github.com/Leonxlnx/taste-skill)｜**115,065 Stars**
网站设计、素材选择和浏览器检查，适合前端与设计工程师；Claude Code、Codex等。MIT，Skill 2.5.1；完整检查依赖Node 22+和浏览器，需审查hooks、联网素材及图片许可。
[技能定义](https://raw.githubusercontent.com/Leonxlnx/taste-skill/main/skills/taste-skill/SKILL.md)。

3. [Impeccable](https://github.com/pbakaus/impeccable)｜**109,627 Stars**
界面设计、审查、润色和规则检测，适合前端与界面质量团队；Claude Code、Codex、Cursor、Copilot、Gemini CLI等。Apache-2.0，Skill 4.9.1；包含引擎、hooks及宿主配置修改，尤其需要先读live模式的CLI权限边界，不能把安装Skill等同于只加载提示词。
[技能定义](https://raw.githubusercontent.com/pbakaus/impeccable/main/plugin/skills/impeccable/SKILL.md)。

4. [Archify](https://github.com/tt-a1i/archify)｜**96,229 Stars**
把架构、流程和代码证据变成可交互HTML图解，适合架构师和技术文档作者；Claude Code、Codex、Cursor、OpenCode。Apache-2.0；结构校验不能保证业务语义正确，分享前检查私有架构信息。
[技能定义](https://raw.githubusercontent.com/tt-a1i/archify/main/archify/SKILL.md)。

5. [Cloudflare Security Audit](https://github.com/cloudflare/security-audit-skill)｜**71,942 Stars**
覆盖驱动的安全审查、独立验证与结构化发现，适合安全、平台及QA团队；完整流程需要工具调用、子Agent及Node.js校验器。MIT，最近推送仍为9月14日；热度不代表新功能，审计目标须获授权。
[技能定义](https://raw.githubusercontent.com/cloudflare/security-audit-skill/main/skills/security-audit/SKILL.md)。

6. [Humanizer](https://github.com/blader/humanizer)｜**68,831 Stars**
编辑机械化表达，适合写作者与编辑；Claude Code、Codex、Pi及兼容宿主。MIT，Skill 4.0.0；改写后应核对原意和新添细节，不能把自然文风当作事实准确性，也不能保证绕过AI检测。
[技能定义](https://raw.githubusercontent.com/blader/humanizer/main/SKILL.md)。

7. [Last30Days](https://github.com/mvanhorn/last30days-skill)｜**67,564 Stars**
聚合近期多平台讨论并保留来源，适合研究、产品及内容团队；Claude Code、Codex、Cursor、Copilot、Gemini CLI及兼容Agent Skills宿主。MIT，Skill已到3.39.0；需Node和Python 3.12+，部分来源需要密钥、会话或付费额度，讨论不等于事实。
[技能定义](https://raw.githubusercontent.com/mvanhorn/last30days-skill/main/skills/last30days/SKILL.md)。

8. [Diagram Design](https://github.com/cathrynlavery/diagram-design)｜**66,897 Stars**
生成HTML/SVG图表，适合方案、文档与汇报；Claude Code、Codex、Cursor、Copilot、Qoder、Pi等。MIT；README声明支持50类图形，仍需核对数据、连线、可见性及字体依赖。
[技能定义](https://raw.githubusercontent.com/cathrynlavery/diagram-design/main/skills/diagram-design/SKILL.md)。

9. [I Have ADHD](https://github.com/ayghri/i-have-adhd)｜**61,123 Stars**
行动优先、编号步骤和跨轮状态提示，适合希望降低阅读负担的人；Claude Code、Cursor及兼容宿主。MIT，最近推送10月19日；属于输出风格指导，不是诊断或治疗。
[技能定义](https://raw.githubusercontent.com/ayghri/i-have-adhd/main/skills/i-have-adhd/SKILL.md)。

10. [Hallmark](https://github.com/Nutlope/hallmark)｜**54,535 Stars**
新页面设计、审计和明确调用后的改版，适合前端与设计团队；Claude Code、Codex、Cursor、OpenCode。MIT，Skill 2.0.0；Node18+，完整渲染检查另需浏览器，不能替代用户研究与无障碍实测。
[技能定义](https://raw.githubusercontent.com/Nutlope/hallmark/main/skills/hallmark/SKILL.md)。


## 二、近7日 Star 净增前十

起点为[11月7日快照](https://github.com/BruceShenC/News/blob/main/daily/2026-11-07/ai-skills-ranking.md)北京时间09:34:45.277–09:34:46.641；终点为本期09:26:30.253–09:26:31.353。按批次起点，统一窗口为**6天23小时51分44.976秒**（604,304.976秒），不是精确168小时。单项读取存在约1秒批次偏移，非原子同时计数。11个仓库ID匹配后计算总星净差，按净增降序，括号为相对起点增长率。

1. Cloudflare Security Audit：64,066 → 71,942，**+7,876（12.29%）**。
2. Hallmark：49,644 → 54,535，**+4,891（9.85%）**。
3. Impeccable：104,822 → 109,627，**+4,805（4.58%）**。
4. Taste：111,235 → 115,065，**+3,830（3.44%）**。
5. Humanizer：65,658 → 68,831，**+3,173（4.83%）**。
6. Karpathy Guidelines：230,297 → 232,884，**+2,587（1.12%）**。
7. Archify：94,206 → 96,229，**+2,023（2.15%）**。
8. Diagram Design：65,505 → 66,897，**+1,392（2.13%）**。
9. I Have ADHD：60,355 → 61,123，**+768（1.27%）**。
10. Last30Days：66,855 → 67,564，**+709（1.06%）**。

用途、平台、风险见总榜。同窗第11为[Book to Skill](https://github.com/virgiliojr94/book-to-skill)：37,156 → 37,780，**+624（1.68%）**。它把书籍、文档转为可追溯技能，适合知识整理者；支持Claude Code、Codex、Copilot、Amp、Hermes、OpenCode、OpenClaw。MIT只覆盖代码，不解决输入书籍版权。[技能定义](https://github.com/virgiliojr94/book-to-skill/blob/master/SKILL.md)。

[归藏PPT Skill](https://github.com/op7418/guizang-ppt-skill)：**29,758星**，缺11月7日匹配快照，近周增长未知，不按零处理、不参排。适合演讲汇报作者，支持Claude Code、Codex等；生成单文件HTML而非原生PPTX，AGPL-3.0。[技能定义](https://github.com/op7418/guizang-ppt-skill/blob/main/SKILL.md)。

## 榜单变化与短窗观察

与[昨日](https://github.com/BruceShenC/News/blob/main/daily/2026-11-13/ai-skills-ranking.md)相比，两榜前十顺序不变。Humanizer领先Last30Days由978星扩大到1,267星；Hallmark与Impeccable近周净增差由192缩至86。滚动窗口起点改变，不能仅凭两期周净增大小判断增长加速或减速。

独立短窗为11月13日09:35:36.034至11月14日09:26:30.253，共**23小时50分54.219秒**，不是严格24小时。净增：Taste +740、Security Audit +714、Impeccable +619、Hallmark +510、Humanizer +408、Karpathy Guidelines +327、Archify +287、Diagram Design +165、Last30Days +119、I Have ADHD +101、Book to Skill +83、归藏PPT +43。短窗只辅助观察，不与近周数据混排。

以上均为定时总星快照净差，包含新增和取消星，不是Trending的today/this week、安装量或总星除以仓龄。没有独立证据把某次发布归因成Star增长。

## 值得深入阅读或隔离验证的两项

### 1. Impeccable：新功能之外，先读清上传边界

[正式Skill 4.9.0](https://github.com/pbakaus/impeccable/releases/tag/skill-v4.9.0)发布于北京时间11月13日14:18:33。发布说明明确：CLI登录后，完整detect会保存设计健康统计；DESIGN.md和保存的critique报告会全文上传，各有关闭开关。不能将“代码和文件名不上传”理解为“没有项目内容上传”。另外，Codex钩子改为扫描会话开始以来改变的UI文件，以覆盖shell写入。

[正式Skill 4.9.1](https://github.com/pbakaus/impeccable/releases/tag/skill-v4.9.1)发布于11月14日08:06:45，增加从world写入DESIGN.md、录屏生成walkthrough、从本地文件检查18项上线基础中的16项等。world需要登录；无法确认的项目应保持未勾选。以上是维护者说明，未自行验证。

适合前端平台团队先阅读权限、上传和覆盖范围，再在无私有信息的临时项目里验证：shell改动是否被扫描、已有DESIGN.md是否保留、缺少构建产物时是否错误显示通过。不要直接在生产仓库启用登录同步。本期只读资料，未登录或执行。

### 2. Last30Days：合并内容时保留证据，不只保留热度

[正式3.39.0](https://github.com/mvanhorn/last30days-skill/releases/tag/v3.39.0)发布于北京时间11月14日07:03:32。发布说明列出X文本反转义、短视频副本合并时保留转录，以及公司调研的Patent Secret Signals。相比昨天的Digg改动，今天可迁移的阅读重点是去重过程中是否丢掉原始文本证据。

建议研究团队先用公开、可人工核对的重复短视频样本，检查合并后转录、来源链接和文本字符是否保留。专利信号只能作为调研线索，不等于产品已落地或商业效果已证实。此为建议方案，不是我们执行过的测试；部分数据源可能需要密钥、登录或费用，不建议直接授予私人会话访问。

## 来源与局限

仓库、技能定义与release均为一手来源。Stars来自公开GitHub API的stargazers_count，例如[Security Audit元数据](https://api.github.com/repos/cloudflare/security-audit-skill)，以本期观察时间为准。所有跟踪候选当前均未归档，但未归档、有推送和高Stars不代表维护充分。

已保存带时间、仓库ID、两端总星的公开数据观察记录，供后续同口径比较。没有合适周起点的候选保持未知。仅公开只读研究，未登录、star、fork、安装、执行候选项目或付费。**热度不等于成熟度、安全性、兼容性或实际效果。**

