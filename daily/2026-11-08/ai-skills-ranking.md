**AI 独立 Skill 热度双榜｜2026年11月8日**

今天的变化：Cloudflare Security Audit总星升至第7，超过Diagram Design 84 Stars，并以近周净增10,411继续领跑增长榜；Hallmark在本系列快照中首次超过5万星。Taste已发布v2.5.1，值得关注新增的结束检查、文案和移动端检查。

**统计范围与时间**

本期总星统一采自公开GitHub仓库API，北京时间 **11月8日09:32:02.133–09:32:03.057**。11个既有候选的数字ID全部匹配，逐一读取官方README、主Skill定义和完整递归文件树；仅Taste的文件树较昨日变化，其余十项不变。稍后搜索结果中的星数不替换本批快照。

补搜[agent-skills主题](https://github.com/search?q=topic%3Aagent-skills+stars%3A%3E50000&type=repositories)、[名称含skill](https://github.com/search?q=skill+in%3Aname+stars%3A%3E50000&type=repositories)、[描述含skill](https://github.com/search?q=skill+in%3Adescription+stars%3A%3E50000&type=repositories)且超过50,000 Stars的仓库，并结合网页发现与补充候选复核。**这是已检索且通过资格核验的样本榜，不是GitHub全网穷尽榜**；低星但快速增长、未使用相关关键词的项目可能遗漏。

只纳入独立分发的单一Skill。框架、市场、awesome清单、多技能合集，以及仅内嵌Skill的完整软件产品不纳入。相同Skill的平台副本、测试样例和仅供贡献者使用的内部辅助技能不重复计数。

本期复核的排除例：[Ponytail](https://github.com/DietrichGebert/ponytail/tree/main/skills)虽把核心称为“一个提示”，实际公开分发6个不同Skill；[Academic Research Skills](https://github.com/Imbad0202/academic-research-skills#individual-skills)是多技能研究套件；[Nature Skills](https://github.com/Yuan1z0825/nature-skills#6-技能索引)有23项技能及共享支持包；[Obsidian Skills](https://github.com/kepano/obsidian-skills#skills)分发6项技能。UI UX Pro Max的七技能合集、Graphify综合软件继续按原口径排除。不能把这些整仓Stars分配给某个子Skill，也不能把排除解释为热度下降。

**一、总 Stars 前十**

1. [Karpathy Guidelines](https://github.com/multica-ai/andrej-karpathy-skills)｜**230,631**
强调明确假设、最小改动和可验证目标，适合研发与QA。支持Claude Code，提供Cursor规则。README与Skill声明MIT，API仍未识别许可证；最近推送4月20日。提示规则不能代替测试和代码审查。

2. [Taste](https://github.com/Leonxlnx/taste-skill)｜**111,673**
指导公司网站的美术方向、真实素材、文案和浏览器检查，适合前端及设计工程师。提供Claude Code、Codex等接入方式；MIT。需Node 22+与Chrome、Edge或Chromium；脚本、hooks、联网取素材和素材许可需分别审查。

3. [Impeccable](https://github.com/pbakaus/impeccable)｜**105,566**
把UI设计、审查、润色和规则检测组织成一个Skill，适合前端、设计与界面质量团队。支持Claude Code、Codex、Cursor、Copilot、Gemini CLI等；Apache-2.0。含执行引擎与hooks，首次运行可能下载二进制。尤其注意README披露：live copy的Apply在没有活跃聊天会话时，可能启动关闭审批与沙箱的CLI回退，不宜未经审查启用。

4. [Archify](https://github.com/tt-a1i/archify)｜**94,480**
把架构、流程和代码关系转成可交互HTML图解，也能制作带来源的一页解释，适合架构师和技术文档作者。支持Claude Code、Codex、Cursor、OpenCode；Apache-2.0。结构校验不能证明业务语义正确，第三方组件许可需另查。

5. [Last30Days](https://github.com/mvanhorn/last30days-skill)｜**66,943**
汇总近期多平台讨论并保留来源，适合研究、产品与内容团队。支持Claude Code、Codex、Cursor、Copilot、Gemini CLI等；MIT。部分数据源涉及API密钥、登录会话或付费额度，热度不能替代事实核验。

6. [Humanizer](https://github.com/blader/humanizer)｜**66,004**
减少机械化写作表达并保留原意，适合编辑和写作者。支持Claude Code、Codex及兼容Agent Skills的宿主；MIT。文风改善不等于事实准确，也不保证任何AI检测结果。

7. [Cloudflare Security Audit](https://github.com/cloudflare/security-audit-skill)｜**65,772**
组织多Agent安全审查、独立验证和结构化报告，适合安全、平台及QA团队。宿主需支持工具调用与并行子Agent，校验器需Node.js；MIT，最近推送仍为9月14日。执行目标代码须有操作系统级隔离、网络和资源限制，不可直接运行不可信项目。

8. [Diagram Design](https://github.com/cathrynlavery/diagram-design)｜**65,688**
生成自包含HTML/SVG图解，适合技术文档、方案评审和汇报。支持Claude Code、Codex、Cursor、Copilot、Factory Droid、Pi等；MIT。默认外载Google字体，可选系统字体；连线、数值、图意及离线可用性仍需检查。

9. [I Have ADHD](https://github.com/ayghri/i-have-adhd)｜**60,437**
让回答行动优先、步骤明确，适合偏好较低阅读负担的用户。提供Claude Code插件和Cursor副本，也可用于兼容Skill的宿主；MIT，最近推送10月19日。这是输出风格指导，不是诊断或治疗工具。

10. [Hallmark](https://github.com/Nutlope/hallmark)｜**50,299**
提供品牌页面设计、审计和明确调用后的改版流程，适合前端及设计团队。支持Claude Code、Codex、Cursor、OpenCode；MIT。脚本需Node 18+，完整渲染检查另需Chrome与puppeteer-core，照片遵循各自许可及署名要求。

**二、近7日 Star 净增前十**

统一窗口：北京时间 **11月1日09:33:02.736 → 11月8日09:32:02.133**，实际 **6天23小时58分59.397秒**，不是严格168小时。

使用同一组11个仓库的两次公开API总星快照相减，按净增排序，括号为相对起点增长率。起点采样批次结束于09:33:03.865，终点批次结束于09:32:03.057，存在秒级请求先后；不是GitHub Trending的“today/this week”，也不是按当前保留Star的创建时间倒推历史净增。[11月1日历史观察记录](https://github.com/BruceShenC/News/blob/main/daily/2026-11-01/ai-skills-ranking.md)

1. Cloudflare Security Audit：**+10,411（18.81%）**
2. Impeccable：**+7,255（7.38%）**
3. Hallmark：**+6,351（14.45%）**
4. Taste：**+4,610（4.31%）**
5. Karpathy Guidelines：**+3,115（1.37%）**
6. Humanizer：**+3,016（4.79%）**
7. Archify：**+2,323（2.52%）**
8. Diagram Design：**+2,196（3.46%）**
9. I Have ADHD：**+839（1.41%）**
10. Last30Days：**+819（1.24%）**

用途、平台和风险见总榜。第11个持续跟踪候选[Book to Skill](https://github.com/virgiliojr94/book-to-skill)总星 **37,217**，同窗口净增 **560（1.53%）**，未进入两榜前十。它把书籍内容整理为可追溯技能材料，适合知识整理，提供标准Skill；MIT覆盖项目代码，输入书籍与输出材料的版权需另行确认。

补充候选[归藏PPT Skill](https://github.com/op7418/guizang-ppt-skill)在北京时间 **09:33:51.601–09:33:52.596** 观察到 **29,433 Stars**。已复核根目录唯一SKILL.md，生成单文件HTML演示稿，适合分享、产品发布与排练；支持Claude Code、Codex及具备文件和命令能力的宿主。AGPL-3.0，当前交付主线是HTML而非原生PPTX，修改及网络服务分发义务、字体图片许可需分别审查。低于总榜门槛，且没有匹配的11月1日端点，近周增长未知，不填零、不混入增长榜。

**变化与值得细读的两项**

- 相比[昨日](https://github.com/BruceShenC/News/blob/main/daily/2026-11-07/ai-skills-ranking.md)，总榜仅Security Audit与Diagram Design交换第7、第8位。两者星数均增加，换位不代表Diagram Design掉星。Hallmark从49,644到50,299，是本系列快照首次观察到超过5万，不是已知的准确破关时刻。
- 近周增长榜前十顺序未变。另看相邻观察，窗口为 **11月7日09:34:45.277 → 11月8日09:32:02.133，共23小时57分16.856秒**：Security Audit +1,706，Impeccable +744，Hallmark +655，Taste +438，Humanizer +346，Karpathy Guidelines +334，Archify +274，Diagram Design +183，Last30Days +88，I Have ADHD +82，Book to Skill +61。此短窗仅供辅助，不与近周榜混排，也不把滚动净增变化直接称作增长加速。
- **Taste v2.5.1：适合前端与QA一起读新版检查设计。** 已核验正式Release于北京时间11月7日20:34:31发布，主分支合并在20:33:21。结束检查结合布局、浏览器与文案检查，最多三轮、五项附理由的豁免；还增加移动端碰撞、触控目标、320像素视口、可访问名称与表单检查。可先审阅其“通过、失败、轮次耗尽、设置异常”的区分，再决定是否在隔离项目评估。作者报告的44站点测试和模板通过情况是项目方结果，本期未复现；Stop hook与可联网取图也应纳入权限审查。[正式发布](https://github.com/Leonxlnx/taste-skill/releases/tag/v2.5.1) · [变更记录](https://github.com/Leonxlnx/taste-skill/blob/main/CHANGELOG.md) · [功能提交](https://github.com/Leonxlnx/taste-skill/commit/b900a8075fd7a2449bd6f414a2ae79657e95363c)
- **Security Audit：适合先读“发现与验证分离”的流程。** 其[技能定义](https://github.com/cloudflare/security-audit-skill/blob/main/skills/security-audit/SKILL.md)将审查、独立复核及结构化输出串联，值得用来检查现有安全评审是否要求可复现证据。它本期没有文件树变化，不因星数上升宣称新增功能；采用前须独立审查隔离方式、误报处理和报告质量。
- 其余十个候选主分支文件树与昨日一致。部分仓库的pushed_at更晚，并不证明默认分支已更新或发布新版本，不据此编写功能新闻，也不推断Stars上涨原因。

**来源与局限**

总星来自各仓库公开API，例如[Security Audit元数据](https://api.github.com/repos/cloudflare/security-audit-skill)；用途和平台以当日官方README与SKILL.md为依据，适配声明未经本期实机测试。GitHub仓库Stars是关注信号，不是安装量、活跃用户数或效果测评。许可证字段不是完整法律审查，脚本、模型服务、素材与第三方组件可能另有要求。

本期只读研究公开资料，未登录、安装或运行任何候选项目；仅将本篇及归档索引按授权同步到News仓库。**热度不等于成熟度、安全性或实际效果。**

[本期归档](https://github.com/BruceShenC/News/blob/main/daily/2026-11-08/ai-skills-ranking.md)
