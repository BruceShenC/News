**AI 独立 Skill 热度双榜｜2026年11月12日**

本期总星前十名次不变；Security Audit超过7万星，近周净增仍第一。增长榜第二、第三名Impeccable与Hallmark仅差13星，不宜把这种差距解读为显著的采用差异。新增值得核对的内容是Last30Days的视频引文处理，以及Impeccable live模式的权限边界。

**范围与口径**

总星观察：北京时间11月12日09:31:07.980–09:31:11.822，来自公开GitHub仓库API实时抓取。复核12个跟踪候选的README、主SKILL.md及完整递归目录树，均未截断。多宿主的同一技能副本、测试夹具、仓库内部开发辅助技能不另计产品。

补搜[agent-skills主题且超过3万星](https://github.com/search?q=topic%3Aagent-skills+stars%3A%3E30000&type=repositories)、[名称含skill且超过3万星](https://github.com/search?q=skill+in%3Aname+stars%3A%3E30000&type=repositories)、[描述含skill且超过5万星](https://github.com/search?q=skill+in%3Adescription+stars%3A%3E50000&type=repositories)，各取按总星排序前30项，返回30、23、30项，存在重叠。目录复核Ponytail、Caveman、UI UX Pro Max为多技能分发；Nuwa包含多个人物技能示例；REA与Graphify是更广泛的MCP/CLI系统，不把随附技能占用整仓Stars。

这是**已检索且核验的样本榜，不是GitHub全网穷尽前十**。搜索阈值、关键词和结果上限会遗漏小型快速增长项目；独立Skill的边界也需随仓库变化复查。

**一、总 Stars 前十**

1. [Karpathy Guidelines](https://github.com/multica-ai/andrej-karpathy-skills)｜**232,233 Stars**
约束编码代理的假设、改动范围与验证目标，适合研发、QA和审查者；Claude Code、Cursor及兼容宿主。Skill声明MIT，仓库许可证字段为空；最近推送4月20日。提示规范不能代替测试。

2. [Taste](https://github.com/Leonxlnx/taste-skill)｜**113,590 Stars**
网站设计、素材选择和浏览器检查，适合前端与设计工程师；Claude Code、Codex等。MIT，Skill 2.5.1；完整检查依赖Node 22+和浏览器，需审查hooks、联网素材及图片许可。

3. [Impeccable](https://github.com/pbakaus/impeccable)｜**108,323 Stars**
界面设计、审查、润色和规则检测，适合前端与界面质量团队；Claude Code、Codex、Cursor、Copilot、Gemini CLI等。Apache-2.0；含引擎、hooks及宿主配置修改；live模式回退CLI可能关闭审批与沙箱，详见下文。

4. [Archify](https://github.com/tt-a1i/archify)｜**95,652 Stars**
把架构、流程和代码证据变成可交互HTML图解，适合架构师和技术文档作者；Claude Code、Codex、Cursor、OpenCode。Apache-2.0；结构校验不能保证业务语义正确，分享前检查私有架构信息。

5. [Cloudflare Security Audit](https://github.com/cloudflare/security-audit-skill)｜**70,391 Stars**
覆盖驱动的安全审查、独立验证与结构化发现，适合安全、平台及QA团队；需要工具调用和子Agent的编码宿主，校验器需Node.js。MIT，最近推送仍为9月14日；热度不代表新功能，审计目标须获授权。

6. [Humanizer](https://github.com/blader/humanizer)｜**67,992 Stars**
编辑机械化表达，适合写作者与编辑；Claude Code、Codex、Pi及兼容宿主。MIT，当前4.0.0允许补充原文以外细节，事实需另行核对；不能保证绕过AI检测或事实正确。

7. [Last30Days](https://github.com/mvanhorn/last30days-skill)｜**67,346 Stars**
聚合近期多平台讨论并保留来源，适合研究、产品及内容团队；Claude Code、Codex、Cursor及Agent Skills宿主。MIT，当前3.36.1；需Node和Python 3.12+，部分来源需要密钥、会话或付费额度，讨论不等于事实。

8. [Diagram Design](https://github.com/cathrynlavery/diagram-design)｜**66,539 Stars**
生成品牌化HTML/SVG图表，适合方案、文档与汇报；Claude Code、Codex、Cursor、Copilot、Qoder、Pi等。MIT；45类图形是维护者声明，仍需核对数据、连线、可见性及字体依赖。

9. [I Have ADHD](https://github.com/ayghri/i-have-adhd)｜**60,894 Stars**
行动优先、编号步骤和跨轮状态提示，适合希望降低阅读负担的人；Claude Code、Cursor及兼容宿主。MIT，最近推送10月19日；属于输出风格指导，不是诊断或治疗。

10. [Hallmark](https://github.com/Nutlope/hallmark)｜**53,429 Stars**
新页面设计、审计和明确调用后的改版，适合前端与设计团队；Claude Code、Codex、Cursor、OpenCode。MIT，Skill 2.0.0；Node18+，完整渲染检查另需浏览器，不能替代用户研究与无障碍实测。

**二、近7日 Star 净增前十**

共同起点为[11月5日快照](https://github.com/BruceShenC/News/blob/main/daily/2026-11-05/ai-skills-ranking.md)北京时间09:31:34.329，终点为本期09:31:07.980。实际窗口**6天23小时59分33.651秒**（604,773.651秒），比严格7日少26.349秒。所有项目使用同一批次端点，起点批次跨度2.518秒、终点3.842秒；不是原子同步计数。按净增降序，括号为相对起点增长率，不是日均速度。

1. Cloudflare Security Audit：59,441 → 70,391，**+10,950（18.42%）**。
2. Impeccable：102,846 → 108,323，**+5,477（5.33%）**。
3. Hallmark：47,965 → 53,429，**+5,464（11.39%）**。
4. Taste：110,093 → 113,590，**+3,497（3.18%）**。
5. Humanizer：64,861 → 67,992，**+3,131（4.83%）**。
6. Karpathy Guidelines：229,518 → 232,233，**+2,715（1.18%）**。
7. Archify：93,480 → 95,652，**+2,172（2.32%）**。
8. Diagram Design：65,024 → 66,539，**+1,515（2.33%）**。
9. I Have ADHD：60,083 → 60,894，**+811（1.35%）**。
10. Last30Days：66,621 → 67,346，**+725（1.09%）**。

用途、平台与风险见总榜。[Book to Skill](https://github.com/virgiliojr94/book-to-skill)为37,614星，同窗+644（1.74%），在11个有匹配端点的候选中列第11；把书籍和文档变成可追溯技能，支持Claude Code、Codex、Copilot、Amp、Hermes、OpenCode、OpenClaw。MIT仅覆盖代码，输入内容版权需另查。

[归藏PPT Skill](https://github.com/op7418/guizang-ppt-skill)为29,658星，根目录唯一SKILL.md，支持Claude Code、Codex等，输出单文件HTML演示稿而非原生PPTX。AGPL-3.0；缺少匹配11月5日起点，近周增长未知，不写零，不参与增长榜。

**变化与短窗辅助观察**

相比[昨日](https://github.com/BruceShenC/News/blob/main/daily/2026-11-11/ai-skills-ranking.md)，两榜前十顺序均未变。Humanizer领先Last30Days由335星扩大至646星。Impeccable与Hallmark的近周净增差仅13星，远小于各自存量，不应据此推断技术优劣。

相邻快照为11月11日09:35:40.860至本期09:31:07.980，实际**23小时55分27.120秒**：Security Audit +1,022、Hallmark +790、Impeccable +704、Taste +575、Humanizer +414、Karpathy Guidelines +349、Archify +262、Diagram Design +210、I Have ADHD +119、Last30Days +103、Book to Skill +67、归藏PPT +61。这是单独的短窗净增，不标成严格24小时，不与近周榜混排。

差值是总星快照净增，包含新增及取消星；**不是GitHub Trending的today/this week，不是安装量，也不是总星除以仓龄**。本期与昨日滚动窗口长度略有不同，不能仅比较净增总数声称增长加速。

**值得深入阅读的两项**

1. **Impeccable：先读live模式的权限边界，再试设计能力。**
[当前README的Live mode说明](https://github.com/pbakaus/impeccable#live-mode)明确写出：没有活跃聊天Agent轮询时，Apply可回退到本地Codex或Claude CLI，关闭审批、权限检查和沙箱，并继承环境。其提示携带编辑内容、元素HTML、附近文字、页面URL及候选源路径。这不是建议开启该行为。

建议只在无秘密、可丢弃的本地样例中评估；先检查其支持的chat-only或关闭Apply方式，不在生产站点注入，不为它弱化CSP，也不直接使用陌生仓库的验证脚本。静态源码检测与渲染页面检测覆盖不同，需分别理解。该风险说明是本次读取发现，未确定引入日期，不包装成今日新漏洞。

版本方面，11月12日04:05:58北京时间的[loop-v0.1.1](https://github.com/pbakaus/impeccable/releases/tag/loop-v0.1.1)是Loop组件版本；主Skill仍为4.8.2。不能把同仓所有release都算成Skill升级。

2. **Last30Days 3.36.1：区分视频原话与评论。**
[3.36.1正式Release](https://github.com/mvanhorn/last30days-skill/releases/tag/v3.36.1)于北京时间11月12日08:04:16发布，说明将引文从仅引用视频评论，调整为也引用视频本身说了什么。[3.36.0](https://github.com/mvanhorn/last30days-skill/releases/tag/v3.36.0)于昨日15:12:10发布，涉及公司话题的招聘信息流程和失败来源等待处理。

可先选一个允许公开读取、同时有视频及评论的话题，核对最终引文到底来自视频内容还是评论者，并检查来源失败是否被如实呈现。这里引用维护者发布说明，未执行测试，不将其写成独立验证过的效果；也不将新版本与Star增长建立因果关系。来源可能需要凭据或付费服务，默认不给私人会话和密钥。

**来源与局限**

总星来自公开仓库API，例如[Security Audit元数据](https://api.github.com/repos/cloudflare/security-audit-skill)。技能定义已逐项读取，可见[Impeccable定义](https://github.com/pbakaus/impeccable/blob/main/plugin/skills/impeccable/SKILL.md)、[Last30Days定义](https://github.com/mvanhorn/last30days-skill/blob/main/skills/last30days/SKILL.md)。动态页面会变化，本文以标明的观察时刻为准。

仅公开只读研究，未登录、安装、运行或执行候选项目。未把推送时间当新功能发布日期；快照已保存供后续同口径比较。**热度不等于成熟度、安全性、兼容性或实际效果。**
