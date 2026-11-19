# AI 独立 Skill 热度双榜｜2026年11月19日

**总星榜前十顺序不变；近周净增榜 Last30Days 从第八升至第六，Impeccable 发布 Skill 4.10.0。**

## 范围与口径

北京时间 **09:26:57.120–09:26:57.602** 批次读取12个公开仓库详情API，非原子同时计数；统一以前者作观察锚点，仓库ID均与历史匹配。重新读取全部README、主SKILL.md、未截断目录树及最新Release记录。

补搜 `topic:agent-skills stars:>50000`、`skill in:name stars:>50000`，分别返回20、15项，存在重叠。保留独立分发的单一Skill；排除框架、市场、awesome清单、多技能合集及软件内嵌技能。UI UX Pro Max的README明确安装包含多个技能，本期不将整仓星数归给单个Skill；Archify的贡献者内部辅助技能、宿主副本与测试夹具不另计产品。**这是已检索样本榜，并非GitHub全网穷尽前十**；高星筛选会遗漏低星快涨项。搜索结果只用于发现，不覆盖本批次星数。

## 一、总 Stars 前十

以下平台兼容性是项目声明，不是实机测试结果；观察时间均为上述批次。

1. **[Karpathy Guidelines](https://github.com/multica-ai/andrej-karpathy-skills)｜234,269 Stars**  
明确假设、最小改动和可验证目标，适合研发、QA及代码审查者。支持Claude Code，提供Cursor规则。Skill声明MIT；本期API许可证字段仍为空，最近推送4月20日。提示规范不能替代测试。[技能定义](https://github.com/multica-ai/andrej-karpathy-skills/blob/main/skills/karpathy-guidelines/SKILL.md)。

2. **[Taste](https://github.com/Leonxlnx/taste-skill)｜118,440 Stars**  
指导网站设计、素材选择和浏览器完成检查，适合前端及设计工程师。支持Claude Code、Codex等宿主；MIT，主Skill仍为2.5.1。完整检查依赖Node 22+和Chromium类浏览器，需审查hooks、联网取素材及素材许可。[技能定义](https://github.com/Leonxlnx/taste-skill/blob/main/skills/taste-skill/SKILL.md)。

3. **[Impeccable](https://github.com/pbakaus/impeccable)｜112,917 Stars**  
组织UI设计、审查、润色和规则检测，适合前端及界面质量团队。支持Claude Code、Codex、Cursor、Copilot、Gemini CLI等；Apache-2.0。主分支Skill与最新Skill Release均为4.10.0，北京时间今日06:26:52发布，不与引擎或CLI版本混同。README仍披露live模式Apply的CLI回退可能关闭审批、权限检查与沙箱，不宜未经审查启用。[技能定义](https://github.com/pbakaus/impeccable/blob/main/plugin/skills/impeccable/SKILL.md)。

4. **[Archify](https://github.com/tt-a1i/archify)｜97,755 Stars**  
把架构、流程及代码证据转成可交互HTML图解，适合架构师和技术文档作者。支持Claude Code、Codex、Cursor、OpenCode；Apache-2.0。结构验证不保证业务语义正确；分享前检查源码和私有架构信息。[技能定义](https://github.com/tt-a1i/archify/blob/main/archify/SKILL.md)。

5. **[Cloudflare Security Audit](https://github.com/cloudflare/security-audit-skill)｜75,387 Stars**  
覆盖驱动的安全审查、独立验证与结构化报告，适合安全、平台及QA团队。完整流程需要工具调用、子Agent和Node.js校验器；MIT，最近推送仍为9月14日。执行目标代码前须有操作系统级隔离、网络和资源限制。[技能定义](https://github.com/cloudflare/security-audit-skill/blob/main/skills/security-audit/SKILL.md)。

6. **[Humanizer](https://github.com/blader/humanizer)｜70,835 Stars**  
编辑机械化表达并尽量保留原意，适合写作者和编辑。支持Claude Code、Codex及兼容宿主；MIT，主Skill 4.0.0。4.0.0允许补充原文没有的具体细节，需逐项核对事实；自然文风不保证事实准确或任何AI检测结果。[技能定义](https://github.com/blader/humanizer/blob/main/SKILL.md)。

7. **[Last30Days](https://github.com/mvanhorn/last30days-skill)｜70,035 Stars**  
汇总近期多平台讨论与来源，适合研究、产品和内容团队。支持Claude Code、Codex、Cursor、Copilot、Gemini CLI等；MIT，主Skill及最新正式Release为4.3.2，详见下文。部分来源涉及密钥、登录会话或付费额度，互动量不等于事实可靠性。[技能定义](https://github.com/mvanhorn/last30days-skill/blob/main/skills/last30days/SKILL.md)。

8. **[Diagram Design](https://github.com/cathrynlavery/diagram-design)｜68,217 Stars**  
生成HTML/SVG图解，适合方案、技术文档和汇报。支持Claude Code、Codex、Cursor、Copilot、Qoder等；MIT。README现声明51类图形，仍需核对数值、连线、语义与字体网络依赖。[技能定义](https://github.com/cathrynlavery/diagram-design/blob/main/skills/diagram-design/SKILL.md)。

9. **[I Have ADHD](https://github.com/ayghri/i-have-adhd)｜61,636 Stars**  
强调行动优先、编号步骤和跨轮状态提示，适合偏好较低阅读负担的人。提供Claude Code插件与Cursor副本，也可用于兼容Skill的宿主；MIT；最近推送仍为10月19日。它是表达方式指导，不是诊断或治疗工具。[技能定义](https://github.com/ayghri/i-have-adhd/blob/main/skills/i-have-adhd/SKILL.md)。

10. **[Hallmark](https://github.com/Nutlope/hallmark)｜57,544 Stars**  
提供新页面设计、审计和明确调用后的改版流程，适合前端及设计团队。支持Claude Code、Codex、Cursor、OpenCode；MIT，主Skill 2.0.0。脚本和渲染检查有环境要求，照片遵循各自许可；自动检查不能替代无障碍实测。[技能定义](https://github.com/Nutlope/hallmark/blob/main/skills/hallmark/SKILL.md)。


## 二、近7日 Star 净增前十

起点为[11月12日快照](https://github.com/BruceShenC/News/blob/main/daily/2026-11-12/ai-skills-ranking.md)北京时间09:31:07.980，终点为本次09:26:57.120，实际 **6天23小时55分49.140秒**，不是严格168小时。两端均为仓库详情API；按总星净差排序，包含取消Star，括号为相对起点增长率，不是日均速度。

1. [Security Audit](https://github.com/cloudflare/security-audit-skill)：70,391 → 75,387，**+4,996（7.10%）**
2. [Taste](https://github.com/Leonxlnx/taste-skill)：113,590 → 118,440，**+4,850（4.27%）**
3. [Impeccable](https://github.com/pbakaus/impeccable)：108,323 → 112,917，**+4,594（4.24%）**
4. [Hallmark](https://github.com/Nutlope/hallmark)：53,429 → 57,544，**+4,115（7.70%）**
5. [Humanizer](https://github.com/blader/humanizer)：67,992 → 70,835，**+2,843（4.18%）**
6. [Last30Days](https://github.com/mvanhorn/last30days-skill)：67,346 → 70,035，**+2,689（3.99%）**
7. [Archify](https://github.com/tt-a1i/archify)：95,652 → 97,755，**+2,103（2.20%）**
8. [Karpathy Guidelines](https://github.com/multica-ai/andrej-karpathy-skills)：232,233 → 234,269，**+2,036（0.88%）**
9. [Diagram Design](https://github.com/cathrynlavery/diagram-design)：66,539 → 68,217，**+1,678（2.52%）**
10. [Book to Skill](https://github.com/virgiliojr94/book-to-skill)：37,614 → 38,405，**+791（2.10%）**

重复项目的用途、平台及风险见总榜。[Book to Skill](https://github.com/virgiliojr94/book-to-skill)现38,405星，把书籍文档转为可追溯技能，适合知识整理者；支持Claude Code、Codex、Copilot、Amp、Hermes、OpenCode、OpenClaw。MIT只覆盖代码，不解决输入作品版权。[技能定义](https://github.com/virgiliojr94/book-to-skill/blob/master/SKILL.md)。

I Have ADHD同窗+742（1.22%），列第11，距第十49星。[归藏PPT](https://github.com/op7418/guizang-ppt-skill)30,061星，同窗+403（1.36%），列第12；适合演讲作者，支持Claude Code、Codex，AGPL-3.0，生成单文件HTML而非原生PPTX。

## 榜单变化与短窗观察

相对11月18日保存快照，总星榜不变；增长榜Last30Days由第八升第六，Karpathy Guidelines由第六降第八，其他名次不变。滚动起点改变，不能仅用相邻两期近周差值判断加速。

11月18日09:32:30.874至本次09:26:57.120，约 **23小时54分26.246秒**，两端均为仓库详情API。以下是近24小时观察差值，不是精确24小时：

Karpathy Guidelines +251；Taste +680；Impeccable +697；Archify +266；Last30Days +938；Security Audit +571；Humanizer +450；Diagram Design +303；I Have ADHD +127；Hallmark +636；Book to Skill +110；归藏PPT +77。

本期数字不是GitHub Trending的today/this week，不是安装量，未以总星除以仓龄估算速度。

## 值得深入阅读的两项

**1. Impeccable 4.10.0：把设计规范压缩成可检索地图，并让动画检查有证据。** 北京时间11月19日06:26:52发布的Skill版，重点包括长DESIGN.md按需定位、指出代码与规范的具体冲突，以及捕获动画效果后审查。适合前端QA用非敏感页面检验“规范偏移是否真能定位、动画证据是否覆盖关键状态”。这是发布说明，不是本期实测；主Skill、引擎与CLI版本不能混为一谈。README仍提示Live Apply回退会关闭审批与沙箱，hooks也可能在模型审批之外执行，不能因有检查器就默认安全。[发布说明](https://github.com/pbakaus/impeccable/releases/tag/skill-v4.10.0)。

**2. Last30Days 4.3.2：关注来源不完整时如何降低结论强度。** 当前版本仍是北京时间11月17日23:56:44发布，不是今天的新版本。它修正Reddit发现阶段失败仍误报完整、X Cookie缺失解释及Windows工具路径等，并补充Exa摘要与官方网站回退。适合阅读“来源状态→报告措辞”的对应方式，避免把缺失证据当作没有事件；近24小时净增938星，不能据此认定发布导致上涨。[发布说明](https://github.com/mvanhorn/last30days-skill/releases/tag/v4.3.2)。

## 公开观察记录与限制

来源为公开仓库详情API，例如[Security Audit元数据](https://api.github.com/repos/cloudflare/security-audit-skill)。以下均采用本期批次锚点，供后续同口径比较。

| 仓库 | GitHub ID | 总Stars |
|---|---:|---:|
| Karpathy Guidelines | 1142983825 | 234269 |
| Taste | 1162099055 | 118440 |
| Impeccable | 1097346685 | 112917 |
| Archify | 1211139949 | 97755 |
| Last30Days | 1140843380 | 70035 |
| Security Audit | 1273427276 | 75387 |
| Humanizer | 1136666433 | 70835 |
| Diagram Design | 1212769188 | 68217 |
| I Have ADHD | 1238120057 | 61636 |
| Hallmark | 1222917575 | 57544 |
| Book to Skill | 1226123343 | 38405 |
| 归藏PPT | 1219042200 | 30061 |

**项目仓库是否被维护者设为只读归档状态：本期12项均为否（GitHub archived=false）。** 这不等于维护活跃，更不等于成熟、安全或兼容性已验证。仅研究公开资料，未安装或运行项目、未star/fork、未付费。热度不等于实际效果。

