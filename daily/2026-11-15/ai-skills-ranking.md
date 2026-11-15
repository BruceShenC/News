# AI 独立 Skill 热度双榜｜2026年11月15日

**本期总星榜与近周净增榜前十顺序均未变化。** Security Audit继续领跑近周净增，Hallmark与Impeccable分别居第二、第三；没有证据将星数变化归因于某次发布。

## 统计范围与口径

总星观察时间：北京时间 **11月15日09:35:11.477–09:35:12.452**。本期重新读取12个跟踪候选的公开GitHub API、README、主SKILL.md及未截断递归文件树；读取要求即时获取，不使用搜索摘要中的星数排名。

另外补搜公开GitHub API中“`topic:agent-skills stars:>50000`”与“`skill in:name stars:>50000`”，分别返回20和15项，有重叠，并做网页关键词发现。**这是已检索、通过独立Skill资格核验的样本榜，不是GitHub全网穷尽前十。** 高星门槛可能漏掉低星快涨项目，未使用相关关键词的仓库也可能遗漏。

只计独立分发的单一Skill，排除框架、市场、awesome清单、多技能合集和仅在完整软件中附带的技能。平台副本与测试夹具不重复计数；Archify的内部开发辅助技能不作为对外技能产品。本期持续跟踪项的产品边界沿用[昨日说明](https://github.com/BruceShenC/News/blob/main/daily/2026-11-14/ai-skills-ranking.md)，没有把大仓库Stars分配给某个子目录Skill。

## 一、总 Stars 前十

以下Stars均来自上述同一采样批次；平台兼容性是项目声明，未实机验证。

1. **[Karpathy Guidelines](https://github.com/multica-ai/andrej-karpathy-skills)｜233,164 Stars**  
约束编码代理的假设、改动范围和验证目标，适合研发、QA与代码审查者。支持Claude Code，提供Cursor规则。Skill声明MIT，API许可证字段仍为空；最近推送4月20日。提示规范不能替代测试。[技能定义](https://github.com/multica-ai/andrej-karpathy-skills/blob/main/skills/karpathy-guidelines/SKILL.md)。

2. **[Taste](https://github.com/Leonxlnx/taste-skill)｜115,664 Stars**  
网站设计、素材选择与浏览器完成检查，适合前端和设计工程师。支持Claude Code、Codex等Agent Skills宿主。MIT，当前Skill 2.5.1；完整检查依赖Node 22+及Chrome、Edge或Chromium。需审查Stop hook、联网取素材和图片许可。[技能定义](https://github.com/Leonxlnx/taste-skill/blob/main/skills/taste-skill/SKILL.md)。

3. **[Impeccable](https://github.com/pbakaus/impeccable)｜110,240 Stars**  
组织UI设计、审查、润色和规则检测，适合前端及界面质量团队。支持Claude Code、Codex、Cursor、Copilot、Gemini CLI等。Apache-2.0，Skill 4.9.1；含执行引擎、hooks和宿主配置。README明确live模式的Apply可在无活跃聊天轮询时启动关闭审批与沙箱的CLI回退，不能未经审查在真实仓库启用。[技能定义](https://github.com/pbakaus/impeccable/blob/main/plugin/skills/impeccable/SKILL.md)。

4. **[Archify](https://github.com/tt-a1i/archify)｜96,451 Stars**  
将架构、流程和代码证据变成可交互HTML图解，适合架构师与技术文档作者。支持Claude Code、Codex、Cursor、OpenCode；Apache-2.0。结构校验不保证业务语义正确，分享前应检查私有架构与源码信息。[技能定义](https://github.com/tt-a1i/archify/blob/main/archify/SKILL.md)。

5. **[Cloudflare Security Audit](https://github.com/cloudflare/security-audit-skill)｜72,656 Stars**  
覆盖驱动的安全审查、独立验证与结构化发现，适合安全、平台和QA团队。完整流程需要工具调用、子Agent及Node.js校验器；MIT。最近推送仍为9月14日，热度不代表新增功能。运行目标代码前需要操作系统级隔离、禁外网和资源限制。[技能定义](https://github.com/cloudflare/security-audit-skill/blob/main/skills/security-audit/SKILL.md)。

6. **[Humanizer](https://github.com/blader/humanizer)｜69,145 Stars**  
编辑机械化表达并尽量保留原意，适合写作者与编辑。支持Claude Code、Codex和兼容宿主；MIT，Skill 4.0.0。需检查改写有没有新增事实、改变语气或原意；自然文风不等于事实准确，也不保证任何AI检测结果。[技能定义](https://github.com/blader/humanizer/blob/main/SKILL.md)。

7. **[Last30Days](https://github.com/mvanhorn/last30days-skill)｜67,678 Stars**  
汇总近期多平台讨论并保留来源，适合研究、产品及内容团队。支持Claude Code、Codex、Cursor、Copilot、Gemini CLI等；MIT，Skill 3.39.0。部分数据源涉及密钥、会话或付费额度，讨论热度不能代替事实核验。[技能定义](https://github.com/mvanhorn/last30days-skill/blob/main/skills/last30days/SKILL.md)。

8. **[Diagram Design](https://github.com/cathrynlavery/diagram-design)｜67,149 Stars**  
生成HTML/SVG图解，适合方案、技术文档和汇报。支持Claude Code、Codex、Cursor、Copilot、Qoder、Gemini CLI等；MIT。README声明50类图形，仍需核对数值、连线、含义和字体网络依赖。[技能定义](https://github.com/cathrynlavery/diagram-design/blob/main/skills/diagram-design/SKILL.md)。

9. **[I Have ADHD](https://github.com/ayghri/i-have-adhd)｜61,204 Stars**  
强调行动优先、编号步骤与跨轮状态提示，适合偏好较低阅读负担的人。提供Claude Code插件与Cursor副本，兼容可读取Skill的宿主；MIT，最近推送10月19日。它是表达方式指导，不是诊断或治疗工具。[技能定义](https://github.com/ayghri/i-have-adhd/blob/main/skills/i-have-adhd/SKILL.md)。

10. **[Hallmark](https://github.com/Nutlope/hallmark)｜55,056 Stars**  
提供新页面设计、审计及明确调用后的改版流程，适合前端和设计团队。支持Claude Code、Codex、Cursor、OpenCode；MIT，Skill 2.0.0。脚本和渲染检查有运行环境要求，照片各自保留许可；自动检查不能替代无障碍实测与用户研究。[技能定义](https://github.com/Nutlope/hallmark/blob/main/skills/hallmark/SKILL.md)。

## 二、近7日 Star 净增前十

采用[11月8日公开总星快照](https://github.com/BruceShenC/News/blob/main/daily/2026-11-08/ai-skills-ranking.md)与本期快照相减。

统一批次起点：**11月8日09:32:02.133 → 11月15日09:35:11.477**，实际 **7天3分9.344秒**，不是精确168小时。两端各仓库读取存在约1秒批次偏移，不是原子同时计数。按净增降序，括号为相对起点增长率；不使用总星除以仓龄。

1. Security Audit：65,772 → 72,656，**+6,884（10.47%）**
2. Hallmark：50,299 → 55,056，**+4,757（9.46%）**
3. Impeccable：105,566 → 110,240，**+4,674（4.43%）**
4. Taste：111,673 → 115,664，**+3,991（3.57%）**
5. Humanizer：66,004 → 69,145，**+3,141（4.76%）**
6. Karpathy Guidelines：230,631 → 233,164，**+2,533（1.10%）**
7. Archify：94,480 → 96,451，**+1,971（2.09%）**
8. Diagram Design：65,688 → 67,149，**+1,461（2.22%）**
9. I Have ADHD：60,437 → 61,204，**+767（1.27%）**
10. Last30Days：66,943 → 67,678，**+735（1.10%）**

用途、平台与风险见总榜。

同窗第11项为 **[Book to Skill](https://github.com/virgiliojr94/book-to-skill)**：37,217 → **37,904 Stars**，净增 **687（1.85%）**。将书籍、文档转换成可追溯技能材料，适合知识整理者；支持Claude Code、Codex、Copilot、Amp、Hermes、OpenCode和OpenClaw。MIT覆盖代码，不解决输入书籍及输出材料的版权问题。[技能定义](https://github.com/virgiliojr94/book-to-skill/blob/master/SKILL.md)。

**[归藏PPT Skill](https://github.com/op7418/guizang-ppt-skill)** 当前 **29,794 Stars**。11月8日观察为29,433，但该次采样比统一起点晚约109秒，**不混入本期统一窗口榜**。它生成单文件HTML演示稿而非原生PPTX，适合演讲与发布会作者；支持Claude Code、Codex等，AGPL-3.0，另需核对字体、图片许可。[技能定义](https://github.com/op7418/guizang-ppt-skill/blob/main/SKILL.md)。

## 榜单变化与近一日辅助观察

与[昨日](https://github.com/BruceShenC/News/blob/main/daily/2026-11-14/ai-skills-ranking.md)相比，两榜前十顺序不变。Hallmark与Impeccable的近周净增差由86缩至83星。Humanizer领先Last30Days的总星差由1,267扩大到1,467。

短窗单独统计：**11月14日09:26:30.253 → 11月15日09:35:11.477**，实际 **24小时8分41.224秒**，不称为严格24小时净增：

Security Audit +714；Impeccable +613；Taste +599；Hallmark +521；Humanizer +314；Karpathy Guidelines +280；Diagram Design +252；Archify +222；Book to Skill +124；Last30Days +114；I Have ADHD +81；归藏PPT +36。

这些是总星快照净差，包含新增和取消Star，不是GitHub Trending的today／this week，也不是安装量。滚动窗口起点改变，不能直接用两期近周净增大小断言增长加速或减速。

## 值得深入阅读的两项

- **Security Audit：关注“证据不足时不执行、不确认”。** 技能将指导模式与完整审计分开；运行目标代码需要明确隔离条件，缺少条件就保留待验证状态，而不是强行执行。适合平台QA先用来审阅现有安全审查的授权、证据与复核流程。此为现有设计，不是今天新功能。
- **Hallmark：关注现有页面保护与可检查规则。** Skill明确要求在用户点名时才改动既有设计，并要求焦点可见、减少动态效果的替代方式和无脚本可见内容。建议用不含真实业务信息的小页面检查这些规则是否落地，再评估设计价值。以上是建议，不是本期执行过的测试。

## 公开观察记录与局限

为后续比较保留本期仓库ID与总星，观察时间统一为上述09:35:11.477–09:35:12.452：

| 仓库简称 | GitHub ID | 总Stars |
|---|---:|---:|
| Karpathy Guidelines | 1142983825 | 233164 |
| Taste | 1162099055 | 115664 |
| Impeccable | 1097346685 | 110240 |
| Archify | 1211139949 | 96451 |
| Security Audit | 1273427276 | 72656 |
| Humanizer | 1136666433 | 69145 |
| Last30Days | 1140843380 | 67678 |
| Diagram Design | 1212769188 | 67149 |
| I Have ADHD | 1238120057 | 61204 |
| Hallmark | 1222917575 | 55056 |
| Book to Skill | 1226123343 | 37904 |
| 归藏PPT | 1219042200 | 29794 |

总星源为各仓库公开API的`stargazers_count`，例如[Security Audit元数据](https://api.github.com/repos/cloudflare/security-audit-skill)。所有12项本期均未归档，但未归档和最近有推送都不代表维护充分。

仅研究公开资料，未登录、star、fork、安装、运行候选项目或付费。**热度不等于成熟度、安全性、兼容性或实际效果。**

## 归档

[本期完整简报](https://github.com/BruceShenC/News/blob/main/daily/2026-11-15/ai-skills-ranking.md)。
根 README 与 daily/README 共享索引本期未更新，仍等待此前写入限制的确认；正文及主题索引独立归档。
