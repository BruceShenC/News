**AI 独立 Skill 热度双榜｜2026年11月11日**

本期变化：Humanizer超过Last30Days，升至样本总星榜第6。Security Audit继续领跑近周净增；值得注意的是Humanizer 4.0.0允许补充原文没有的细节，升级前应核对自己的事实保真要求。

**范围与口径**

总星观察：北京时间11月11日09:35:40.860–09:35:42.669，来自公开GitHub API实时抓取。复核12个跟踪仓库的README、主SKILL.md及未截断的完整目录树；统计独立分发的单一Skill，多宿主副本、测试夹具与仓库内部开发辅助技能不另算产品。

补搜[agent-skills主题且超过3万星](https://github.com/search?q=topic%3Aagent-skills+stars%3A%3E30000&type=repositories)、[名称含skill且超过3万星](https://github.com/search?q=skill+in%3Aname+stars%3A%3E30000&type=repositories)、[描述含skill且超过5万星](https://github.com/search?q=skill+in%3Adescription+stars%3A%3E50000&type=repositories)，各按总星取前30项；返回29、23、30项，存在重叠。排除框架、清单、市场和多技能合集，Ponytail与Reverse Skill本次目录复核仍含多个独立技能。Nuwa包含主技能和多个人物技能示例，本期不将其整仓热度归给单一技能。其他沿用已核实的排除口径。

这是**已检索且核验的样本榜，不是GitHub全网穷尽前十**；缺少关键词的仓库和低星快速增长项目可能遗漏。

**一、总 Stars 前十**

1. [Karpathy Guidelines](https://github.com/multica-ai/andrej-karpathy-skills)｜**231,884 Stars**
约束编码代理的假设、改动范围与验证目标，适合研发、QA和审查者；Claude Code、Cursor及兼容宿主。Skill声明MIT，仓库许可证字段为空；最近推送4月20日。提示规范不能代替测试。

2. [Taste](https://github.com/Leonxlnx/taste-skill)｜**113,015 Stars**
网站设计、素材选择和浏览器检查，适合前端与设计工程师；Claude Code、Codex等。MIT，Skill 2.5.1；完整检查依赖Node 22+和浏览器，需审查hooks、联网素材及图片许可。

3. [Impeccable](https://github.com/pbakaus/impeccable)｜**107,619 Stars**
界面设计、审查、润色和规则检测，适合前端与界面质量团队；Claude Code、Codex、Cursor、Copilot、Gemini CLI等。Apache-2.0；含引擎二进制、hooks与宿主配置修改，并非只有文本提示。

4. [Archify](https://github.com/tt-a1i/archify)｜**95,390 Stars**
把架构、流程和代码证据变成可交互HTML图解，适合架构师和技术文档作者；Claude Code、Codex、Cursor、OpenCode。Apache-2.0；结构校验不能保证业务语义正确，分享前检查私有架构信息。

5. [Cloudflare Security Audit](https://github.com/cloudflare/security-audit-skill)｜**69,369 Stars**
覆盖驱动的安全审查、独立验证与结构化发现，适合安全、平台及QA团队；需要工具调用和子Agent的编码宿主，校验器需Node.js。MIT，最近推送仍为9月14日；热度不代表新功能，审计目标须获授权。

6. [Humanizer](https://github.com/blader/humanizer)｜**67,578 Stars**
编辑机械化表达，适合写作者与编辑；Claude Code、Codex、Pi及兼容宿主。MIT，当前4.0.0已改变事实补充边界，详见下文；不能保证绕过AI检测或事实正确。

7. [Last30Days](https://github.com/mvanhorn/last30days-skill)｜**67,243 Stars**
聚合近期多平台讨论并保留来源，适合研究、产品及内容团队；Claude Code、Codex、Cursor及Agent Skills宿主。MIT，当前3.34.1；需Node和Python 3.12+，部分来源需要密钥、会话或付费额度，讨论不等于事实。

8. [Diagram Design](https://github.com/cathrynlavery/diagram-design)｜**66,329 Stars**
生成品牌化HTML/SVG图表，适合方案、文档与汇报；Claude Code、Codex、Cursor、Copilot、Qoder、Pi等。MIT；45类图形是维护者声明，仍需核对数据、连线、可见性及字体依赖。

9. [I Have ADHD](https://github.com/ayghri/i-have-adhd)｜**60,775 Stars**
行动优先、编号步骤和跨轮状态提示，适合希望降低阅读负担的人；Claude Code、Cursor及兼容宿主。MIT，最近推送10月19日；属于输出风格指导，不是诊断或治疗。

10. [Hallmark](https://github.com/Nutlope/hallmark)｜**52,639 Stars**
新页面设计、审计和明确调用后的改版，适合前端与设计团队；Claude Code、Codex、Cursor、OpenCode。MIT，Skill 2.0.0；Node18+，完整渲染检查另需浏览器，不能替代用户研究与无障碍实测。

**二、近7日 Star 净增前十**

共同起点：[11月4日公开快照](https://github.com/BruceShenC/News/blob/main/daily/2026-11-04/ai-skills-ranking.md)，北京时间09:29:13.855；终点为本期09:35:40.860。实际窗口**7天6分27.005秒**（605,187.005秒），不是严格168小时。各项目使用同一批次端点，起点批次跨度1.516秒、终点1.809秒；仓库不是原子同步计数。按净增降序，括号是相对起点的增长率，不是日均速度。

1. Cloudflare Security Audit：58,222 → 69,369，**+11,147（19.15%）**。
2. Impeccable：101,565 → 107,619，**+6,054（5.96%）**。
3. Hallmark：47,008 → 52,639，**+5,631（11.98%）**。
4. Taste：109,513 → 113,015，**+3,502（3.20%）**。
5. Humanizer：64,423 → 67,578，**+3,155（4.90%）**。
6. Karpathy Guidelines：229,107 → 231,884，**+2,777（1.21%）**。
7. Archify：93,141 → 95,390，**+2,249（2.41%）**。
8. Diagram Design：64,673 → 66,329，**+1,656（2.56%）**。
9. I Have ADHD：59,955 → 60,775，**+820（1.37%）**。
10. Last30Days：66,491 → 67,243，**+752（1.13%）**。

以上项目的用途、平台与风险见总榜。[Book to Skill](https://github.com/virgiliojr94/book-to-skill)为37,547星，同窗+672（1.82%），在11个有匹配端点的候选中列第11；把书籍文档转为可追溯技能，支持Claude Code、Codex、Copilot、Amp、Hermes、OpenCode、OpenClaw等。MIT仅覆盖代码，输入内容版权需另查。

[归藏PPT Skill](https://github.com/op7418/guizang-ppt-skill)为29,597星；根目录唯一SKILL.md，支持Claude Code、Codex等，输出单文件HTML演示稿而非原生PPTX。AGPL-3.0；无匹配11月4日起点，近周增长未知，不写零，不参加增长榜。

**变化与短窗辅助观察**

相比[昨日](https://github.com/BruceShenC/News/blob/main/daily/2026-11-10/ai-skills-ranking.md)，Humanizer与Last30Days交换第6、第7，领先335星；其他总榜名次不变。近周净增前十顺序不变，但昨天实际窗口只有6天12小时49分，不能用两期净增大小直接断言加速。

相邻快照窗口为11月10日09:34:28.126至本期09:35:40.860，**24小时1分12.734秒**：Security Audit +1,265、Hallmark +825、Impeccable +710、Karpathy Guidelines +507、Taste +474、Humanizer +452、Archify +323、Diagram Design +233、I Have ADHD +131、Last30Days +110、归藏PPT +67、Book to Skill +63。这是单独的短窗观察，不与近周榜混排。

所有差值均为总星快照净增，包含新增与取消星；**不是Trending的today/this week，不是安装量，也不是总星除以仓龄**。

**值得深入阅读的两项**

1. **Humanizer 4.0.0：升级会改变“只改表达”的边界。**
[正式Release](https://github.com/blader/humanizer/releases/tag/v4.0.0)于北京时间11月10日09:35:15发布，刚好在昨日快照结束后。它允许在原文泛泛而谈时补充模型认为真实的具体细节，明确提示依赖旧规则者保留3.1.0。同时要求核对丢失或变化的事实、文件模式完整读取，并将文本中的伪系统指令视为材料。
建议先对带数字、引用和不确定性表述的短文做隔离对照，分别标记原文保留、事实变化和新增事实，再人工验证新增内容。本期未执行，该建议不等于已验证效果。

2. **Last30Days 3.34.1：看来源统计与结果表达是否对得上。**
[3.34.0](https://github.com/mvanhorn/last30days-skill/releases/tag/v3.34.0)于北京时间11月11日08:08:13发布，调整标题、引文和首行，并修正实际发现数量、市场名和保存链接；[3.34.1](https://github.com/mvanhorn/last30days-skill/releases/tag/v3.34.1)于09:25:43发布，维护者称修复真实运行发现的5项缺陷。这里只引用发布说明，不展开质量简报式回归细节。
可先阅读来源健康检查与引用保存流程，使用允许公开检索的话题验证“报告声称找到多少”是否对应实际来源。不要把未经成功执行的来源或模型补写当真实搜索结果，也不要为验证默认提供私人会话或密钥。

**来源与限制**

总星取自公开仓库API，例如[Security Audit元数据](https://api.github.com/repos/cloudflare/security-audit-skill)。定义可见[Humanizer SKILL.md](https://github.com/blader/humanizer/blob/main/SKILL.md)、[Last30Days SKILL.md](https://github.com/mvanhorn/last30days-skill/blob/main/skills/last30days/SKILL.md)，其余主技能已在本次读取核验。动态页面今后可能改变，以上以观察时刻为准。

本期仅公开只读研究，未登录、安装或执行候选项目；没有把推送时间当新功能发布日期，没有把排名变化归因于版本发布。公开观察快照已保存供后续同口径比较。**热度不等于成熟度、安全性、兼容性或实际效果。**

