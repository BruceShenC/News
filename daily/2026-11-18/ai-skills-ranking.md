# AI 独立 Skill 热度双榜｜2026年11月18日

**总星榜前十不变；近周净增榜Taste升至第二，Last30Days升至第八，Book to Skill进入第十。** 本期公开仓库详情API恢复可读；昨天使用搜索API，短窗对比需保留索引滞后限制。

## 范围与口径

总星批次请求于北京时间 **09:32:30.874–09:32:30.886** 发起，随后约8秒内陆续返回，非原子同时计数。12个仓库ID与历史一致；重新读取12项README、主SKILL.md、未截断目录树及Release记录。仅统计独立分发的单一Skill，宿主副本、测试夹具及贡献者内部技能不另计产品；Archify仍按其对外分发主Skill边界计入。

补搜公开API的`topic:agent-skills stars:>50000`与`skill in:name stars:>50000`，返回20、15项，存在重叠，未将稍后搜索星数覆盖本批次。排除通用框架、市场、awesome清单、多技能合集及完整软件内嵌技能；不把大仓库星数分配给某个子目录。**这是已检索样本榜，不是GitHub全网穷尽前十**，关键词和高星筛选可能漏掉其他项目及低星快涨项。

## 一、总 Stars 前十


以下支持平台来自项目声明，未实机验证。

1. **[Karpathy Guidelines](https://github.com/multica-ai/andrej-karpathy-skills)｜234,018 Stars**  
明确假设、最小改动和可验证目标，适合研发、QA及代码审查者。支持Claude Code，提供Cursor规则。Skill声明MIT；本期API许可证字段仍为空，最近推送4月20日。提示规范不能替代测试。[技能定义](https://github.com/multica-ai/andrej-karpathy-skills/blob/main/skills/karpathy-guidelines/SKILL.md)。

2. **[Taste](https://github.com/Leonxlnx/taste-skill)｜117,760 Stars**  
指导网站设计、素材选择和浏览器完成检查，适合前端及设计工程师。支持Claude Code、Codex等宿主；MIT，主Skill仍为2.5.1。完整检查依赖Node 22+和Chromium类浏览器，需审查hooks、联网取素材及素材许可。[技能定义](https://github.com/Leonxlnx/taste-skill/blob/main/skills/taste-skill/SKILL.md)。

3. **[Impeccable](https://github.com/pbakaus/impeccable)｜112,220 Stars**  
组织UI设计、审查、润色和规则检测，适合前端及界面质量团队。支持Claude Code、Codex、Cursor、Copilot、Gemini CLI等；Apache-2.0。主分支Skill标为4.9.2；最新可见CLI Release为11月16日的cli-v4.9.2，不与Skill版本混同。README仍披露live模式Apply的CLI回退可能关闭审批、权限检查与沙箱，不宜未经审查启用。[技能定义](https://github.com/pbakaus/impeccable/blob/main/plugin/skills/impeccable/SKILL.md)。

4. **[Archify](https://github.com/tt-a1i/archify)｜97,489 Stars**  
把架构、流程及代码证据转成可交互HTML图解，适合架构师和技术文档作者。支持Claude Code、Codex、Cursor、OpenCode；Apache-2.0。结构验证不保证业务语义正确；分享前检查源码和私有架构信息。[技能定义](https://github.com/tt-a1i/archify/blob/main/archify/SKILL.md)。

5. **[Cloudflare Security Audit](https://github.com/cloudflare/security-audit-skill)｜74,816 Stars**  
覆盖驱动的安全审查、独立验证与结构化报告，适合安全、平台及QA团队。完整流程需要工具调用、子Agent和Node.js校验器；MIT，最近推送仍为9月14日。执行目标代码前须有操作系统级隔离、网络和资源限制。[技能定义](https://github.com/cloudflare/security-audit-skill/blob/main/skills/security-audit/SKILL.md)。

6. **[Humanizer](https://github.com/blader/humanizer)｜70,385 Stars**  
编辑机械化表达并尽量保留原意，适合写作者和编辑。支持Claude Code、Codex及兼容宿主；MIT，主Skill 4.0.0。4.0.0允许补充原文没有的具体细节，需逐项核对事实；自然文风不保证事实准确或任何AI检测结果。[技能定义](https://github.com/blader/humanizer/blob/main/SKILL.md)。

7. **[Last30Days](https://github.com/mvanhorn/last30days-skill)｜69,097 Stars**  
汇总近期多平台讨论与来源，适合研究、产品和内容团队。支持Claude Code、Codex、Cursor、Copilot、Gemini CLI等；MIT，主Skill及最新正式Release为4.3.2，详见下文。部分来源涉及密钥、登录会话或付费额度，互动量不等于事实可靠性。[技能定义](https://github.com/mvanhorn/last30days-skill/blob/main/skills/last30days/SKILL.md)。

8. **[Diagram Design](https://github.com/cathrynlavery/diagram-design)｜67,914 Stars**  
生成HTML/SVG图解，适合方案、技术文档和汇报。支持Claude Code、Codex、Cursor、Copilot、Qoder等；MIT。README声明50类图形，仍需核对数值、连线、语义与字体网络依赖。[技能定义](https://github.com/cathrynlavery/diagram-design/blob/main/skills/diagram-design/SKILL.md)。

9. **[I Have ADHD](https://github.com/ayghri/i-have-adhd)｜61,509 Stars**  
强调行动优先、编号步骤和跨轮状态提示，适合偏好较低阅读负担的人。提供Claude Code插件与Cursor副本，也可用于兼容Skill的宿主；MIT；最近推送仍为10月19日。它是表达方式指导，不是诊断或治疗工具。[技能定义](https://github.com/ayghri/i-have-adhd/blob/main/skills/i-have-adhd/SKILL.md)。

10. **[Hallmark](https://github.com/Nutlope/hallmark)｜56,908 Stars**  
提供新页面设计、审计和明确调用后的改版流程，适合前端及设计团队。支持Claude Code、Codex、Cursor、OpenCode；MIT，主Skill 2.0.0。脚本和渲染检查有环境要求，照片遵循各自许可；自动检查不能替代无障碍实测。[技能定义](https://github.com/Nutlope/hallmark/blob/main/skills/hallmark/SKILL.md)。


## 二、近7日 Star 净增前十

共同起点为[11月11日快照](https://github.com/BruceShenC/News/blob/main/daily/2026-11-11/ai-skills-ranking.md)09:35:40.860，终点为本次09:32:30.874，实际 **6天23小时56分50.014秒**，不是严格168小时。两端均为公开仓库详情API；按总星快照净差排序，包含取消Star，括号为相对起点增长率，不是日均速度。

1. Security Audit：69,369 → 74,816，**+5,447（7.85%）**
2. Taste：113,015 → 117,760，**+4,745（4.20%）**
3. Impeccable：107,619 → 112,220，**+4,601（4.28%）**
4. Hallmark：52,639 → 56,908，**+4,269（8.11%）**
5. Humanizer：67,578 → 70,385，**+2,807（4.15%）**
6. Karpathy Guidelines：231,884 → 234,018，**+2,134（0.92%）**
7. Archify：95,390 → 97,489，**+2,099（2.20%）**
8. Last30Days：67,243 → 69,097，**+1,854（2.76%）**
9. Diagram Design：66,329 → 67,914，**+1,585（2.39%）**
10. Book to Skill：37,547 → 38,295，**+748（1.99%）**

用途、平台与风险见总榜。新入增长榜的[Book to Skill](https://github.com/virgiliojr94/book-to-skill)把书籍文档转为可追溯技能，适合知识整理者；支持Claude Code、Codex、Copilot、Amp、Hermes、OpenCode、OpenClaw。MIT只覆盖代码，不解决输入作品版权。[技能定义](https://github.com/virgiliojr94/book-to-skill/blob/master/SKILL.md)。

I Have ADHD同窗+734，列第11，落后Book to Skill仅14星，不宜视为稳定差距。[归藏PPT](https://github.com/op7418/guizang-ppt-skill)29,984星，同窗+387（1.31%），列第12；适合演讲作者，支持Claude Code、Codex，AGPL-3.0，生成单文件HTML而非原生PPTX。

## 排名变化与短窗观察

相对[昨日](https://github.com/BruceShenC/News/blob/main/daily/2026-11-17/ai-skills-ranking.md)：Taste与Impeccable交换增长榜第二、第三；Last30Days超过Diagram Design；Book to Skill替代I Have ADHD进入前十。总星顺序不变，Last30Days领先Diagram Design的差距由496扩大到1,183。滚动起点改变，不能仅用两期近周差值判断加速。

昨日09:37:27.446至今日09:32:30.874，共 **23小时55分3.428秒**。昨日搜索API可能滞后，下面仅为跨端点观察差值，不声称精确24小时增长：

Karpathy Guidelines +269；Taste +815；Impeccable +816；Archify +313；Last30Days +950；Security Audit +829；Humanizer +455；Diagram Design +263；I Have ADHD +111；Hallmark +758；Book to Skill +158；归藏PPT +88。

本期所有差值均不是GitHub Trending的today/this week，不是安装量，也未用总星除以仓龄估算速度。

## 值得深入阅读的两项

**1. Last30Days 4.3.2：让不完整检索显式表现为“不完整”。** 北京时间11月17日23:56:44正式发布，修正Reddit archive在发现阶段失败时仍被误报完整、X浏览器Cookie缺失说明、无效转录密钥建议，以及Windows工具路径；另增加Exa摘要和官方网站回退。适合研究团队阅读其来源状态与报告口径如何对应。本期只核实发布说明，未运行验证，不能将新增Star归因于这次发布。[Release](https://github.com/mvanhorn/last30days-skill/releases/tag/v4.3.2)。

**2. Taste：关注完成判定，而非仅看首次生成截图。** 当前正式版仍为11月7日的2.5.1，今天不是新发布。它要求结合检查器、布局检查和文案检查形成结束门禁，同时设定轮次与豁免边界。适合用非敏感小页面评估“检查通过”是否对应真实可用、是否留下误报和漏报；规则通过不能代替浏览器及无障碍实测。[Release与门禁说明](https://github.com/Leonxlnx/taste-skill/releases/tag/v2.5.1)。

## 公开观察记录与限制

总星来源为公开仓库API，例如[Security Audit元数据](https://api.github.com/repos/cloudflare/security-audit-skill)。以下统一采用上述09:32:30.874批次锚点，便于后续同口径比较。

| 仓库 | GitHub ID | 总Stars |
|---|---:|---:|
| Karpathy Guidelines | 1142983825 | 234018 |
| Taste | 1162099055 | 117760 |
| Impeccable | 1097346685 | 112220 |
| Archify | 1211139949 | 97489 |
| Last30Days | 1140843380 | 69097 |
| Security Audit | 1273427276 | 74816 |
| Humanizer | 1136666433 | 70385 |
| Diagram Design | 1212769188 | 67914 |
| I Have ADHD | 1238120057 | 61509 |
| Hallmark | 1222917575 | 56908 |
| Book to Skill | 1226123343 | 38295 |
| 归藏PPT | 1219042200 | 29984 |

12项本期均未归档，但推送与Stars不能证明维护质量。仅研究公开资料，未登录、star、fork、安装、运行项目或付费；以上建议不是实测结果。**热度不等于成熟度、安全性、兼容性或实际效果。**

## 归档

[本期完整简报](https://github.com/BruceShenC/News/blob/main/daily/2026-11-18/ai-skills-ranking.md)。正文已归档；主题和共享索引本期未更新。
