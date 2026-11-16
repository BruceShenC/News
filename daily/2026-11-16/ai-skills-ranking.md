# AI 独立 Skill 热度双榜｜2026年11月16日

**总星榜前十顺序不变；近周净增榜出现两组换位。** Impeccable以 **1星**优势超过Hallmark，暂列第二；Last30Days超过I Have ADHD，升至第九。前一组差距小于本期采样期间可能发生的波动，不宜解读为稳定领先。

## 统计范围与口径

本期观察锚点为北京时间 **11月16日09:31:05.610**。12个仓库的公开GitHub API请求在09:31:05.609–09:31:05.623启动，约3秒内完成，并非原子同时计数。以下总Stars固定使用该批次，不被随后搜索的数字覆盖。

重新读取12个跟踪候选的README、主SKILL.md及未截断递归文件树；仓库数字ID与历史记录一致。另补搜公开API中的`topic:agent-skills stars:>50000`和`skill in:name stars:>50000`，分别返回20和15项，有重叠。**这是已检索、通过独立Skill核验的样本榜，不是GitHub全网穷尽前十。** 高星门槛可能漏掉低星快涨项目，不使用相关关键词的项目也可能遗漏。

只计独立分发的单一Skill。框架、市场、awesome清单、多技能合集及完整软件附带的技能不计；平台副本和测试夹具不重复计数。Archify的贡献者辅助技能不视为对外分发的技能产品。沿用此前产品边界，不把大仓库总星分配给某个子目录。

## 一、总 Stars 前十

以下平台支持是项目声明，未实机验证。

1. **[Karpathy Guidelines](https://github.com/multica-ai/andrej-karpathy-skills)｜233,462 Stars**  
明确假设、最小改动和可验证目标，适合研发、QA及代码审查者。支持Claude Code，提供Cursor规则。Skill声明MIT，API许可证字段为空；最近推送仍为4月20日。提示规范不能替代测试。[技能定义](https://github.com/multica-ai/andrej-karpathy-skills/blob/main/skills/karpathy-guidelines/SKILL.md)。

2. **[Taste](https://github.com/Leonxlnx/taste-skill)｜116,253 Stars**  
指导网站设计、素材选择和浏览器完成检查，适合前端及设计工程师。支持Claude Code、Codex等宿主；MIT，主Skill仍为2.5.1。完整检查依赖Node 22+和Chromium类浏览器，需审查hooks、联网取素材及素材许可。[技能定义](https://github.com/Leonxlnx/taste-skill/blob/main/skills/taste-skill/SKILL.md)。

3. **[Impeccable](https://github.com/pbakaus/impeccable)｜110,768 Stars**  
组织UI设计、审查、润色和规则检测，适合前端及界面质量团队。支持Claude Code、Codex、Cursor、Copilot、Gemini CLI等；Apache-2.0。主分支Skill标为4.9.2，但本次未找到同名Skill Release，不能与已发布CLI 4.9.2混同。README仍披露live模式Apply的CLI回退可能关闭审批、权限检查与沙箱，不宜未经审查启用。[技能定义](https://github.com/pbakaus/impeccable/blob/main/plugin/skills/impeccable/SKILL.md)。

4. **[Archify](https://github.com/tt-a1i/archify)｜96,813 Stars**  
把架构、流程及代码证据转成可交互HTML图解，适合架构师和技术文档作者。支持Claude Code、Codex、Cursor、OpenCode；Apache-2.0。结构验证不保证业务语义正确；分享前检查源码和私有架构信息。[技能定义](https://github.com/tt-a1i/archify/blob/main/archify/SKILL.md)。

5. **[Cloudflare Security Audit](https://github.com/cloudflare/security-audit-skill)｜73,253 Stars**  
覆盖驱动的安全审查、独立验证与结构化报告，适合安全、平台及QA团队。完整流程需要工具调用、子Agent和Node.js校验器；MIT，最近推送仍为9月14日。执行目标代码前须有操作系统级隔离、网络和资源限制。[技能定义](https://github.com/cloudflare/security-audit-skill/blob/main/skills/security-audit/SKILL.md)。

6. **[Humanizer](https://github.com/blader/humanizer)｜69,486 Stars**  
编辑机械化表达并尽量保留原意，适合写作者和编辑。支持Claude Code、Codex及兼容宿主；MIT，主Skill 4.0.0。需复核改写有没有新增事实或改变含义；自然文风不保证事实准确或任何AI检测结果。[技能定义](https://github.com/blader/humanizer/blob/main/SKILL.md)。

7. **[Last30Days](https://github.com/mvanhorn/last30days-skill)｜67,784 Stars**  
汇总近期多平台讨论与来源，适合研究、产品和内容团队。支持Claude Code、Codex、Cursor、Copilot、Gemini CLI等；MIT，主Skill及正式版本均为4.0.5。部分来源涉及密钥、登录会话或付费额度，互动量不等于事实可靠性。[技能定义](https://github.com/mvanhorn/last30days-skill/blob/main/skills/last30days/SKILL.md)。

8. **[Diagram Design](https://github.com/cathrynlavery/diagram-design)｜67,398 Stars**  
生成HTML/SVG图解，适合方案、技术文档和汇报。支持Claude Code、Codex、Cursor、Copilot、Qoder等；MIT。README声明50类图形，仍需核对数值、连线、语义与字体网络依赖。[技能定义](https://github.com/cathrynlavery/diagram-design/blob/main/skills/diagram-design/SKILL.md)。

9. **[I Have ADHD](https://github.com/ayghri/i-have-adhd)｜61,298 Stars**  
强调行动优先、编号步骤和跨轮状态提示，适合偏好较低阅读负担的人。提供Claude Code插件与Cursor副本，也可用于兼容Skill的宿主；MIT，最近推送10月19日。它是表达方式指导，不是诊断或治疗工具。[技能定义](https://github.com/ayghri/i-have-adhd/blob/main/skills/i-have-adhd/SKILL.md)。

10. **[Hallmark](https://github.com/Nutlope/hallmark)｜55,516 Stars**  
提供新页面设计、审计和明确调用后的改版流程，适合前端及设计团队。支持Claude Code、Codex、Cursor、OpenCode；MIT，主Skill 2.0.0。脚本和渲染检查有环境要求，照片遵循各自许可；自动检查不能替代无障碍实测。[技能定义](https://github.com/Nutlope/hallmark/blob/main/skills/hallmark/SKILL.md)。

## 二、近7日 Star 净增前十

使用[11月9日快照](https://github.com/BruceShenC/News/blob/main/daily/2026-11-09/ai-skills-ranking.md)与本期公开API快照相减。

共同批次锚点：**11月9日09:26:23.304 → 11月16日09:31:05.610**，实际 **7天4分42.306秒**，不是精确168小时。两端均有秒级读取先后，按净增降序，括号为相对起点增长率。

1. Security Audit：66,890 → 73,253，**+6,363（9.51%）**
2. Impeccable：106,230 → 110,768，**+4,538（4.27%）**
3. Hallmark：50,979 → 55,516，**+4,537（8.90%）**
4. Taste：112,052 → 116,253，**+4,201（3.75%）**
5. Humanizer：66,522 → 69,486，**+2,964（4.46%）**
6. Karpathy Guidelines：230,958 → 233,462，**+2,504（1.08%）**
7. Archify：94,745 → 96,813，**+2,068（2.18%）**
8. Diagram Design：65,889 → 67,398，**+1,509（2.29%）**
9. Last30Days：67,024 → 67,784，**+760（1.13%）**
10. I Have ADHD：60,545 → 61,298，**+753（1.24%）**

用途、平台与风险见总榜。Impeccable与Hallmark近乎持平；这是本次快照的排序，不代表持续领先。

同窗第11项 **[Book to Skill](https://github.com/virgiliojr94/book-to-skill)**：37,357 → **37,982 Stars**，净增 **625（1.67%）**。把书籍、文档转换为可追溯技能材料，适合知识整理者；支持Claude Code、Codex、Copilot、Amp、Hermes、OpenCode和OpenClaw。MIT覆盖代码，不解决输入书籍及输出内容的版权问题。[技能定义](https://github.com/virgiliojr94/book-to-skill/blob/master/SKILL.md)。

补充 **[归藏PPT Skill](https://github.com/op7418/guizang-ppt-skill)**：当前 **29,836 Stars**。11月9日观察为29,482，但采样起点09:27:49.464比统一批次晚约86秒，**不混入统一近周榜**。它输出单文件HTML演示稿，而非原生PPTX；适合演讲与发布会作者，支持Claude Code、Codex等，AGPL-3.0，另需核对字体与图片许可。[技能定义](https://github.com/op7418/guizang-ppt-skill/blob/main/SKILL.md)。

## 榜单变化与近一日辅助观察

与[昨日](https://github.com/BruceShenC/News/blob/main/daily/2026-11-15/ai-skills-ranking.md)相比：

- 总星榜前十不变；Last30Days领先Diagram Design的差距由529缩至386星。
- 近周榜Impeccable与Hallmark交换第二、第三；Last30Days与I Have ADHD交换第九、第十。
- 不能仅因滚动近周净增变小就判断增长减速，因为起点也已改变。

短窗单独统计：**11月15日09:35:11.477 → 11月16日09:31:05.610**，实际 **23小时55分54.133秒**，不是严格24小时：

Security Audit +597；Taste +589；Impeccable +528；Hallmark +460；Archify +362；Humanizer +341；Karpathy Guidelines +298；Diagram Design +249；Last30Days +106；I Have ADHD +94；Book to Skill +78；归藏PPT +42。

以上是总Stars快照净差，包含新增和取消Star，不是GitHub Trending的today／this week，不是安装量，也不以总星除以仓龄估算近期速度。

## 值得深入阅读的两项

**1. Last30Days v4.0.5：检索排序会改变你看到的“热度”。**  
北京时间11月16日06:11:23正式发布。主题关键词搜索从X的Latest切到Top，意图覆盖整个30日窗口中有互动的内容；特定账户帖子与提及仍使用Latest。作者报告同主题对照的互动量明显增加，但这是检索结果选择变化，不是社区突然更活跃，也不证明完整覆盖或真实性。适合研究型QA审阅“时间范围、排序、分页与结果去重”如何共同影响结论。[正式发布说明](https://github.com/mvanhorn/last30days-skill/releases/tag/v4.0.5)。

**2. Impeccable：优先审阅安装及权限边界，而不是跟随星数升级。**  
北京时间11月16日08:40:46发布的是 **CLI v4.9.2**：包括Loop组件的固定校验和验证、将偏好写入Agent规则的选项，以及登录后的仓库关联行为。Loop、CLI、engine和Skill有独立版本；本期主Skill标4.9.2，但未找到对应Skill Release，不能混称。适合先阅读哪些步骤下载组件、写配置或关联账户，再在隔离、非敏感项目中评估；本期没有执行安装或测试。[CLI正式说明](https://github.com/pbakaus/impeccable/releases/tag/cli-v4.9.2) · [README权限提示](https://github.com/pbakaus/impeccable#installation)。

## 公开观察记录与限制

以下ID与总星保留供后续同口径比较，时间统一采用上述09:31:05.610批次锚点：

| 仓库简称 | GitHub ID | 总Stars |
|---|---:|---:|
| Karpathy Guidelines | 1142983825 | 233462 |
| Taste | 1162099055 | 116253 |
| Impeccable | 1097346685 | 110768 |
| Archify | 1211139949 | 96813 |
| Security Audit | 1273427276 | 73253 |
| Humanizer | 1136666433 | 69486 |
| Last30Days | 1140843380 | 67784 |
| Diagram Design | 1212769188 | 67398 |
| I Have ADHD | 1238120057 | 61298 |
| Hallmark | 1222917575 | 55516 |
| Book to Skill | 1226123343 | 37982 |
| 归藏PPT | 1219042200 | 29836 |

总星来自各仓库公开API的`stargazers_count`，例如[Security Audit元数据](https://api.github.com/repos/cloudflare/security-audit-skill)。12项仓库本期均非归档状态，但这不能证明维护充分。

仅做公开资料研究，未登录、star、fork、安装、运行候选项目或付费。没有将增长归因于特定版本发布。**热度不等于成熟度、安全性、兼容性或实际效果。**

## 归档

[本期完整简报](https://github.com/BruceShenC/News/blob/main/daily/2026-11-16/ai-skills-ranking.md)。
根 README 与 daily/README 共享索引本期未更新，仍等待此前写入限制的确认；正文及主题索引独立归档。
