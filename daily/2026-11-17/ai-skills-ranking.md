# AI 独立 Skill 热度双榜｜2026年11月17日

**样本总星榜前十不变；近周观察净增榜Taste升至第三，Hallmark降至第四。** 本期公开仓库详情API部分返回403，因此统一改用公开GitHub搜索API的精确仓库查询取数。搜索索引可能滞后，以下是带时间的观察值，不是GitHub实时原子计数，微小差距不宜解读。

## 范围与统计口径

批次请求在北京时间 **09:37:27.446–09:37:27.456** 发起，随后数秒陆续返回。总星全部采用此批次，不混入先前详情API的数字。12个仓库ID与历史一致；重新读取12项README和主SKILL.md。目录树仅3项成功读取，其余返回403，独立Skill边界沿用此前完整目录核验，不能声称今天全面排除了新增子技能。

补搜 `topic:agent-skills stars:>50000` 与 `skill in:name stars:>50000`，分别返回20与15项，存在重叠。框架、市场、awesome清单、多技能合集及完整软件附带的技能不计；不把大仓库星数分配给子目录。**这是已检索样本榜，不是GitHub全网穷尽前十**，高星门槛可能漏掉低星快涨项目。

## 一、总 Stars 前十

以下支持平台来自项目声明，未实机验证。

1. **[Karpathy Guidelines](https://github.com/multica-ai/andrej-karpathy-skills)｜233,749 Stars**  
明确假设、最小改动和可验证目标，适合研发、QA及代码审查者。支持Claude Code，提供Cursor规则。Skill声明MIT；昨日API许可证字段为空、推送日期为4月20日，本期详情未成功复核。提示规范不能替代测试。[技能定义](https://github.com/multica-ai/andrej-karpathy-skills/blob/main/skills/karpathy-guidelines/SKILL.md)。

2. **[Taste](https://github.com/Leonxlnx/taste-skill)｜116,945 Stars**  
指导网站设计、素材选择和浏览器完成检查，适合前端及设计工程师。支持Claude Code、Codex等宿主；MIT，主Skill仍为2.5.1。完整检查依赖Node 22+和Chromium类浏览器，需审查hooks、联网取素材及素材许可。[技能定义](https://github.com/Leonxlnx/taste-skill/blob/main/skills/taste-skill/SKILL.md)。

3. **[Impeccable](https://github.com/pbakaus/impeccable)｜111,404 Stars**  
组织UI设计、审查、润色和规则检测，适合前端及界面质量团队。支持Claude Code、Codex、Cursor、Copilot、Gemini CLI等；Apache-2.0。主分支Skill标为4.9.2；本期Release端点读取失败，不将主分支版本当作已核实正式发布。README仍披露live模式Apply的CLI回退可能关闭审批、权限检查与沙箱，不宜未经审查启用。[技能定义](https://github.com/pbakaus/impeccable/blob/main/plugin/skills/impeccable/SKILL.md)。

4. **[Archify](https://github.com/tt-a1i/archify)｜97,176 Stars**  
把架构、流程及代码证据转成可交互HTML图解，适合架构师和技术文档作者。支持Claude Code、Codex、Cursor、OpenCode；Apache-2.0。结构验证不保证业务语义正确；分享前检查源码和私有架构信息。[技能定义](https://github.com/tt-a1i/archify/blob/main/archify/SKILL.md)。

5. **[Cloudflare Security Audit](https://github.com/cloudflare/security-audit-skill)｜73,987 Stars**  
覆盖驱动的安全审查、独立验证与结构化报告，适合安全、平台及QA团队。完整流程需要工具调用、子Agent和Node.js校验器；MIT，最近推送仍为9月14日。执行目标代码前须有操作系统级隔离、网络和资源限制。[技能定义](https://github.com/cloudflare/security-audit-skill/blob/main/skills/security-audit/SKILL.md)。

6. **[Humanizer](https://github.com/blader/humanizer)｜69,930 Stars**  
编辑机械化表达并尽量保留原意，适合写作者和编辑。支持Claude Code、Codex及兼容宿主；MIT，主Skill 4.0.0。需复核改写有没有新增事实或改变含义；自然文风不保证事实准确或任何AI检测结果。[技能定义](https://github.com/blader/humanizer/blob/main/SKILL.md)。

7. **[Last30Days](https://github.com/mvanhorn/last30days-skill)｜68,147 Stars**  
汇总近期多平台讨论与来源，适合研究、产品和内容团队。支持Claude Code、Codex、Cursor、Copilot、Gemini CLI等；MIT，主分支Skill已标4.3.0；本期未核实对应Release，不能据此确定正式发布日期。部分来源涉及密钥、登录会话或付费额度，互动量不等于事实可靠性。[技能定义](https://github.com/mvanhorn/last30days-skill/blob/main/skills/last30days/SKILL.md)。

8. **[Diagram Design](https://github.com/cathrynlavery/diagram-design)｜67,651 Stars**  
生成HTML/SVG图解，适合方案、技术文档和汇报。支持Claude Code、Codex、Cursor、Copilot、Qoder等；MIT。README声明50类图形，仍需核对数值、连线、语义与字体网络依赖。[技能定义](https://github.com/cathrynlavery/diagram-design/blob/main/skills/diagram-design/SKILL.md)。

9. **[I Have ADHD](https://github.com/ayghri/i-have-adhd)｜61,398 Stars**  
强调行动优先、编号步骤和跨轮状态提示，适合偏好较低阅读负担的人。提供Claude Code插件与Cursor副本，也可用于兼容Skill的宿主；MIT；最近推送日期沿用昨日所见10月19日，本期未重新核实。它是表达方式指导，不是诊断或治疗工具。[技能定义](https://github.com/ayghri/i-have-adhd/blob/main/skills/i-have-adhd/SKILL.md)。

10. **[Hallmark](https://github.com/Nutlope/hallmark)｜56,150 Stars**  
提供新页面设计、审计和明确调用后的改版流程，适合前端及设计团队。支持Claude Code、Codex、Cursor、OpenCode；MIT，主Skill 2.0.0。脚本和渲染检查有环境要求，照片遵循各自许可；自动检查不能替代无障碍实测。[技能定义](https://github.com/Nutlope/hallmark/blob/main/skills/hallmark/SKILL.md)。

## 二、近7日 Star 观察净增前十

起点为[11月10日快照](https://github.com/BruceShenC/News/blob/main/daily/2026-11-10/ai-skills-ranking.md)的09:34:28.126，终点为本次09:37:27.446，实际 **7天2分59.320秒**，不是严格168小时。共同窗口下按当前总星减去历史总星排序；起点是仓库详情API、终点是搜索API，字段同为stargazers_count，但搜索索引滞后可能影响差值与小差距排序，不能称精确增长速度。

1. Security Audit：68,104 → 73,987，**+5,883（8.64%）**
2. Impeccable：106,909 → 111,404，**+4,495（4.20%）**
3. Taste：112,541 → 116,945，**+4,404（3.91%）**
4. Hallmark：51,814 → 56,150，**+4,336（8.37%）**
5. Humanizer：67,126 → 69,930，**+2,804（4.18%）**
6. Karpathy Guidelines：231,377 → 233,749，**+2,372（1.03%）**
7. Archify：95,067 → 97,176，**+2,109（2.22%）**
8. Diagram Design：66,096 → 67,651，**+1,555（2.35%）**
9. Last30Days：67,133 → 68,147，**+1,014（1.51%）**
10. I Have ADHD：60,644 → 61,398，**+754（1.24%）**

用途、平台与风险见总榜。第11项[Book to Skill](https://github.com/virgiliojr94/book-to-skill)为38,137星，较37,484增加653（1.74%）；第12项[归藏PPT](https://github.com/op7418/guizang-ppt-skill)为29,896星，较29,530增加366（1.24%），两项均有本次共同起点。前者把文档整理为可追溯技能，适合知识整理者，支持Claude Code、Codex、Copilot等，MIT不覆盖输入书籍的版权；后者适合演讲作者，支持Claude Code、Codex，AGPL-3.0，输出单文件HTML而非原生PPTX。

## 榜单变化与短窗辅助观察

与[昨日](https://github.com/BruceShenC/News/blob/main/daily/2026-11-16/ai-skills-ranking.md)相比，总星前十顺序不变。近周榜Taste与Hallmark交换第三、第四，Impeccable保持第二；不能据滚动窗口净增变化直接判断加速或减速。Last30Days领先Diagram Design的观察差距从386增至496星。

昨日09:31:05.610至本次09:37:27.446，共 **24小时6分21.836秒**，不是严格24小时；同样受详情/搜索API口径切换及索引滞后影响：

Karpathy Guidelines +287；Taste +692；Impeccable +636；Archify +363；Last30Days +363；Security Audit +734；Humanizer +444；Diagram Design +253；I Have ADHD +100；Hallmark +634；Book to Skill +155；归藏PPT +60。

这不是GitHub Trending的today/this week，不是安装量，也没有用总Stars除以仓龄估算速度。

## 值得深入阅读的两项

**1. Book to Skill：生成成功不等于宿主能发现。** [主技能定义](https://github.com/virgiliojr94/book-to-skill/blob/master/SKILL.md)要求区分跨宿主目录与Claude Code私有目录，并回读符号链接状态，不能仅凭“执行了链接命令”声称可发现。适合QA借鉴“写入、发现、实际加载”三层验收；同时遵守版权与目标目录授权。本期推荐的是当前流程，不声称今天首次发布。

**2. Security Audit：把候选发现与独立确认分开。** [技能定义](https://github.com/cloudflare/security-audit-skill/blob/main/skills/security-audit/SKILL.md)区分指导模式和完整审计，疑似发现需反证及独立验证。适合用明确授权、隔离的玩具仓库检查误报和证据完整性；只加载技能不构成对目标系统的审计授权。其最近推送仍为9月14日，热度不代表近期功能更新。

以上为阅读与验证建议，未运行。Last30Days主分支版本变化值得继续核查，但本期未取得可靠Release时间与完整变更证据，不将其写成新发布新闻。

## 公开观察记录

总星来源为公开搜索API的精确仓库查询，例如[Security Audit查询](https://api.github.com/search/repositories?q=repo%3Acloudflare%2Fsecurity-audit-skill)。下表统一对应本期09:37:27.446批次锚点，保留供后续同口径比较。

| 仓库 | GitHub ID | 总Stars |
|---|---:|---:|
| Karpathy Guidelines | 1142983825 | 233749 |
| Taste | 1162099055 | 116945 |
| Impeccable | 1097346685 | 111404 |
| Archify | 1211139949 | 97176 |
| Last30Days | 1140843380 | 68147 |
| Security Audit | 1273427276 | 73987 |
| Humanizer | 1136666433 | 69930 |
| Diagram Design | 1212769188 | 67651 |
| I Have ADHD | 1238120057 | 61398 |
| Hallmark | 1222917575 | 56150 |
| Book to Skill | 1226123343 | 38137 |
| 归藏PPT | 1219042200 | 29896 |

仅研究公开资料，未登录、star、fork、安装、运行或付费。API访问限制使本期目录与发布状态复核不完整；没有把未知写成零，也未把关注变化归因于某次发布。**热度不等于成熟度、安全性或实际效果。**

## 归档

[本期完整简报](https://github.com/BruceShenC/News/blob/main/daily/2026-11-17/ai-skills-ranking.md)。共享索引本期未更新。
