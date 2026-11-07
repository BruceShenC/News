**AI 独立 Skill 热度双榜｜2026年11月7日**

今天的变化：Humanizer总星超过Diagram Design，由第7升至第6，领先153 Stars；Cloudflare Security Audit以近周净增9,906继续领跑增长榜。增长榜前十次序与昨日相同。

**统计范围与时间**

本期总星统一采自公开GitHub仓库API，北京时间 **11月7日09:34:45.277–09:34:46.641**。11个既有候选的数字ID全部匹配，逐一复核README、主技能定义及完整文件树；仅Impeccable和Diagram Design的文件树较昨日变化，其余九项不变。搜索结果中稍后变化的星数不替换本批快照。

补搜[agent-skills主题](https://github.com/search?q=topic%3Aagent-skills+stars%3A%3E49000&type=repositories)、[名称含skill](https://github.com/search?q=skill+in%3Aname+stars%3A%3E49000&type=repositories)、[描述含skill](https://github.com/search?q=skill+in%3Adescription+stars%3A%3E49000&type=repositories)且超过49,000 Stars的仓库，并结合网页发现及昨日补充候选复核。**这是已检索并通过资格核验的样本榜，不是GitHub全网穷尽榜**；低星但快速增长、没有相关关键词的项目可能遗漏。

只纳入独立分发的单一Skill。排除框架、市场、awesome清单、多技能合集，以及仅内嵌Skill的完整软件产品。相同Skill的平台副本、测试样例和仅供贡献者使用的内部辅助技能不重复计数。

本期复核的排除例：Ponytail公开分发6个不同技能，不能把整仓Stars分给核心提示；Nature Skills为23项技能加共享支持包，Emil为17项技能；REA以MCP产品为主体，Caveman为Skill加代理等多组件产品。UI UX Pro Max的七技能合集、Graphify综合软件仍按原口径排除。这些是范围判断，不是它们热度下降。[Ponytail定义目录](https://github.com/DietrichGebert/ponytail/tree/main/skills) · [Nature说明](https://github.com/Yuan1z0825/nature-skills#6-技能索引) · [Emil定义](https://github.com/emilkowalski/skills#reference) · [REA](https://github.com/morluto/rea) · [Caveman](https://github.com/JuliusBrussee/caveman)

**一、总 Stars 前十**

1. [Karpathy Guidelines](https://github.com/multica-ai/andrej-karpathy-skills)｜**230,297**
强调先明确假设、最小改动和可验证目标，适合研发与QA。支持 Claude Code，提供 Cursor 规则。README及Skill声明MIT，API仍未识别许可证；最近推送4月20日。提示规则不能替代测试和代码审查。

2. [Taste](https://github.com/Leonxlnx/taste-skill)｜**111,235**
指导网站美术方向、真实素材和浏览器检查，适合前端及设计工程师。提供 Claude Code、Codex 等接入方式；MIT。当前分发一个Skill，旧技能已分开；脚本、浏览器权限及素材许可仍需审查。

3. [Impeccable](https://github.com/pbakaus/impeccable)｜**104,822**
将UI设计、审查、润色和规则检测组织成一个Skill，适合前端、设计和界面质量团队。支持 Claude Code、Codex、Cursor、Copilot、Gemini CLI等；Apache-2.0。包含执行引擎与hooks，部分宿主的hook会在模型命令审批之外执行，首次运行还可能下载二进制。

4. [Archify](https://github.com/tt-a1i/archify)｜**94,206**
把架构、流程和代码关系转成交互式HTML图解，适合架构师及技术文档作者。支持 Claude Code、Codex、Cursor、OpenCode；Apache-2.0。结构校验不能证明业务语义正确，历史版本及第三方组件许可需分别核对。

5. [Last30Days](https://github.com/mvanhorn/last30days-skill)｜**66,855**
汇总近期多平台讨论并保留来源，适合研究、产品及内容团队。支持 Claude Code、Codex、Cursor、Copilot、Gemini CLI等；MIT。部分数据源涉及API密钥、登录会话或付费额度，关注度不能替代事实核验。

6. [Humanizer](https://github.com/blader/humanizer)｜**65,658**
减少机械化写作表达并尽量保留原意，适合编辑和写作者。支持 Claude Code、Codex及兼容Agent Skills的宿主；MIT。文风改善不等于事实准确，也不保证任何AI检测结果。

7. [Diagram Design](https://github.com/cathrynlavery/diagram-design)｜**65,505**
生成自包含HTML/SVG图解，适合技术文档、方案评审和汇报。支持 Claude Code、Codex、Cursor、Copilot、Factory Droid、Pi等；MIT。默认外载Google字体，可选择系统字体；连线、数值、图意及离线可用性仍需检查。

8. [Cloudflare Security Audit](https://github.com/cloudflare/security-audit-skill)｜**64,066**
组织多Agent安全审查、独立验证和结构化报告，适合安全、平台及QA团队。宿主需支持工具调用与并行子Agent，校验器需Node.js；MIT，最近推送仍为9月14日。执行目标代码需操作系统级隔离、网络和资源限制，不能直接运行不可信项目。

9. [I Have ADHD](https://github.com/ayghri/i-have-adhd)｜**60,355**
让回答行动优先、步骤明确，适合偏好较低阅读负担的用户。提供Claude Code插件和Cursor副本，也可用于兼容Skill的宿主；MIT，最近推送10月19日。它是输出风格指导，不是诊断或治疗工具。

10. [Hallmark](https://github.com/Nutlope/hallmark)｜**49,644**
提供品牌页面设计、审计和明确调用后的改版工作流，适合前端及设计团队。支持 Claude Code、Codex、Cursor、OpenCode；MIT。脚本需Node 18+，完整渲染检查另需Chrome与puppeteer-core，照片遵循各自许可及署名要求。

**二、近7日 Star 净增前十**

统一窗口：北京时间 **10月31日09:27:40.989 → 11月7日09:34:45.277**，实际 **7天7分4.288秒**，不是严格168小时。

使用同一组11个仓库的两次公开API总星快照相减，按净增排序，括号为相对起点增长率。起点采样批次结束于09:27:42.958，终点批次结束于09:34:46.641，因此存在秒级请求先后；不是GitHub Trending的“today/this week”，也不是根据当前保留Star的创建时间倒推历史净增。[10月31日历史观察记录](https://github.com/BruceShenC/News/blob/main/daily/2026-10-31/ai-skills-ranking.md)

1. Cloudflare Security Audit：**+9,906（18.29%）**
2. Impeccable：**+7,726（7.96%）**
3. Hallmark：**+6,651（15.47%）**
4. Taste：**+4,874（4.58%）**
5. Karpathy Guidelines：**+3,420（1.51%）**
6. Humanizer：**+2,924（4.66%）**
7. Archify：**+2,357（2.57%）**
8. Diagram Design：**+2,240（3.54%）**
9. I Have ADHD：**+847（1.42%）**
10. Last30Days：**+840（1.27%）**

用途、平台和风险见总榜。第11个持续跟踪候选 [Book to Skill](https://github.com/virgiliojr94/book-to-skill) 总星 **37,156**，同窗口净增 **562（1.54%）**，未进入两榜前十。它把书籍内容整理为可追溯技能材料，适合知识整理，提供标准Skill；MIT仅覆盖项目代码，输入书籍和输出材料的版权需另行确认。

补充候选 [归藏PPT Skill](https://github.com/op7418/guizang-ppt-skill) 在北京时间 **09:36:43** 观察到 **29,390 Stars**。已核验一个根目录SKILL.md，生成单文件网页演示稿，适合分享、发布会及数据汇报；支持Agent Skills宿主，提供Codex等工作流。AGPL-3.0；当前主交付是HTML而非原生PPTX，修改与网络服务分发义务、字体图片许可需分别审查。低于总榜门槛，也没有匹配的10月31日端点，近周增长未知，不填零、不混入增长榜。

**变化与值得细读的两项**

- 相比[昨日](https://github.com/BruceShenC/News/blob/main/daily/2026-11-06/ai-skills-ranking.md)，总榜仅Humanizer与Diagram Design交换第6、第7位；近周增长榜顺序未变。两者总星都增加，换位不代表Diagram Design掉星。
- 另看相邻两次观察，窗口为 **11月6日09:28:44.473 → 11月7日09:34:45.277，共24小时6分0.804秒**：Security Audit +2,511，Impeccable +858，Hallmark +855，Taste +559，Karpathy Guidelines +409，Humanizer +383，Archify +331，Diagram Design +207，I Have ADHD +131，Last30Days +110，Book to Skill +77。这只是辅助短窗数据，不与近周榜混排，也不把滚动窗口排名变化直接称为增长加速。
- **Diagram Design值得读新的验证器。** 北京时间11月6日23:19合入OAuth返回线与可见ALT条件检查，增加对真实SVG结构、隐藏条件及普通返回线的验证和负例；随后插件清单版本升至2.6.142。已核对提交和变更文件，未执行测试；公开GitHub Releases列表为空，不把清单版本变化说成已核验的GitHub正式发布。[修复提交](https://github.com/cathrynlavery/diagram-design/commit/26dd8eae349b9d347d64c6af70fd111df0171101) · [版本清单提交](https://github.com/cathrynlavery/diagram-design/commit/e57eda6102a19444538ebd611d39534fad65b663)
- **Humanizer适合做低成本文风对照。** 可先读[技能定义](https://github.com/blader/humanizer/blob/main/SKILL.md)，以一段无敏感信息的现有文字，对照是否删掉空泛表达、保留事实与限定条件。它本期没有代码树变化，不应因换位而宣称推出新功能。
- Impeccable最新主分支提交调整的是维护者待决策时的贡献者超时计时，并非设计功能更新，也不能据此解释Stars上涨。最新核验Skill正式发布仍为10月26日的4.8.0。[维护提交](https://github.com/pbakaus/impeccable/commit/687bc381bc9091bf252c0a05303640561014a425) · [Skill 4.8.0](https://github.com/pbakaus/impeccable/releases/tag/skill-v4.8.0)

**来源与局限**

总星来自各仓库公开API，例如[Security Audit元数据](https://api.github.com/repos/cloudflare/security-audit-skill)；用途与平台以当日官方README和SKILL.md为依据，适配声明未经本期实机测试。许可证字段不是完整法律审查，脚本、模型服务、素材和第三方组件可能另有要求。短期Star变化不能证明采用率、效果、安全性或变化原因。

本期仅研究公开资料，未登录、安装或运行任何候选项目。**热度不等于成熟度、安全性或实际效果。**

[本期归档](https://github.com/BruceShenC/News/blob/main/daily/2026-11-07/ai-skills-ranking.md)
