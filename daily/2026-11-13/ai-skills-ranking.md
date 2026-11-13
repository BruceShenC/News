# AI 独立 Skill 热度双榜｜2026年11月13日

总星前十名次不变；近周净增榜中，Hallmark升至第二、Impeccable降至第三，Security Audit仍居第一。值得细读的新发布是Last30Days对Digg话题检索、相关性筛选和截止时间处理的调整。

## 范围与统计口径

总星观察：北京时间11月13日09:35:36.034–09:35:36.959，来自公开GitHub仓库API的新抓取。逐项复核12个跟踪候选的README、主SKILL.md及未截断递归目录树。相同技能的多宿主副本、测试夹具不另计；Archify的仓库内部开发辅助技能不等同于其对外分发的Archify产品。

补搜[agent-skills主题且超过3万星](https://github.com/search?q=topic%3Aagent-skills+stars%3A%3E30000&type=repositories)、[名称含skill且超过3万星](https://github.com/search?q=skill+in%3Aname+stars%3A%3E30000&type=repositories)、[描述含skill且超过5万星](https://github.com/search?q=skill+in%3Adescription+stars%3A%3E50000&type=repositories)，各取按Stars排序前30项，返回30、23、30项，存在重叠。排除通用框架、市场、awesome清单、多技能合集，以及不能把技能与更广泛CLI/MCP产品热度分开的仓库。

**以下是已检索并核验的样本前十，不是GitHub全网穷尽前十。**关键词、星数门槛和结果上限会遗漏低星但快速增长的项目；Skill产品边界也可能变化。支持平台为仓库声明，未逐个运行验证。

## 一、总 Stars 前十

1. [Karpathy Guidelines](https://github.com/multica-ai/andrej-karpathy-skills)｜**232,557 Stars**
约束编码代理的假设、改动范围与验证目标，适合研发、QA和审查者；Claude Code、Cursor及兼容宿主。Skill声明MIT，仓库API许可证字段为空；最近推送4月20日。提示规范不能代替测试。
[技能定义](https://raw.githubusercontent.com/multica-ai/andrej-karpathy-skills/main/skills/karpathy-guidelines/SKILL.md)。

2. [Taste](https://github.com/Leonxlnx/taste-skill)｜**114,325 Stars**
网站设计、素材选择和浏览器检查，适合前端与设计工程师；Claude Code、Codex等。MIT，Skill 2.5.1；完整检查依赖Node 22+和浏览器，需审查hooks、联网素材及图片许可。
[技能定义](https://raw.githubusercontent.com/Leonxlnx/taste-skill/main/skills/taste-skill/SKILL.md)。

3. [Impeccable](https://github.com/pbakaus/impeccable)｜**109,008 Stars**
界面设计、审查、润色和规则检测，适合前端与界面质量团队；Claude Code、Codex、Cursor、Copilot、Gemini CLI等。Apache-2.0，Skill 4.8.2；包含引擎、hooks及宿主配置修改，尤其需要先读live模式的CLI权限边界，不能把安装Skill等同于只加载提示词。
[技能定义](https://raw.githubusercontent.com/pbakaus/impeccable/main/plugin/skills/impeccable/SKILL.md)。

4. [Archify](https://github.com/tt-a1i/archify)｜**95,942 Stars**
把架构、流程和代码证据变成可交互HTML图解，适合架构师和技术文档作者；Claude Code、Codex、Cursor、OpenCode。Apache-2.0；结构校验不能保证业务语义正确，分享前检查私有架构信息。
[技能定义](https://raw.githubusercontent.com/tt-a1i/archify/main/archify/SKILL.md)。

5. [Cloudflare Security Audit](https://github.com/cloudflare/security-audit-skill)｜**71,228 Stars**
覆盖驱动的安全审查、独立验证与结构化发现，适合安全、平台及QA团队；完整流程需要工具调用、子Agent及Node.js校验器。MIT，最近推送仍为9月14日；热度不代表新功能，审计目标须获授权。
[技能定义](https://raw.githubusercontent.com/cloudflare/security-audit-skill/main/skills/security-audit/SKILL.md)。

6. [Humanizer](https://github.com/blader/humanizer)｜**68,423 Stars**
编辑机械化表达，适合写作者与编辑；Claude Code、Codex、Pi及兼容宿主。MIT，Skill 4.0.0；改写后应核对原意和新添细节，不能把自然文风当作事实准确性，也不能保证绕过AI检测。
[技能定义](https://raw.githubusercontent.com/blader/humanizer/main/SKILL.md)。

7. [Last30Days](https://github.com/mvanhorn/last30days-skill)｜**67,445 Stars**
聚合近期多平台讨论并保留来源，适合研究、产品及内容团队；Claude Code、Codex、Cursor、Copilot、Gemini CLI及兼容Agent Skills宿主。MIT，Skill已到3.38.1；需Node和Python 3.12+，部分来源需要密钥、会话或付费额度，讨论不等于事实。
[技能定义](https://raw.githubusercontent.com/mvanhorn/last30days-skill/main/skills/last30days/SKILL.md)。

8. [Diagram Design](https://github.com/cathrynlavery/diagram-design)｜**66,732 Stars**
生成HTML/SVG图表，适合方案、文档与汇报；Claude Code、Codex、Cursor、Copilot、Qoder、Pi等。MIT；README声明支持45类图形，仍需核对数据、连线、可见性及字体依赖。
[技能定义](https://raw.githubusercontent.com/cathrynlavery/diagram-design/main/skills/diagram-design/SKILL.md)。

9. [I Have ADHD](https://github.com/ayghri/i-have-adhd)｜**61,022 Stars**
行动优先、编号步骤和跨轮状态提示，适合希望降低阅读负担的人；Claude Code、Cursor及兼容宿主。MIT，最近推送10月19日；属于输出风格指导，不是诊断或治疗。
[技能定义](https://raw.githubusercontent.com/ayghri/i-have-adhd/main/skills/i-have-adhd/SKILL.md)。

10. [Hallmark](https://github.com/Nutlope/hallmark)｜**54,025 Stars**
新页面设计、审计和明确调用后的改版，适合前端与设计团队；Claude Code、Codex、Cursor、OpenCode。MIT，Skill 2.0.0；Node18+，完整渲染检查另需浏览器，不能替代用户研究与无障碍实测。
[技能定义](https://raw.githubusercontent.com/Nutlope/hallmark/main/skills/hallmark/SKILL.md)。

## 二、近7日 Star 净增前十

起点为[11月6日快照](https://github.com/BruceShenC/News/blob/main/daily/2026-11-06/ai-skills-ranking.md)北京时间09:28:44.473–09:28:45.456；终点为本期09:35:36.034–09:35:36.959。以批次起点计算，统一观察窗口为**7天6分51.561秒**（605,211.561秒），并非精确168小时。各项目实际读取时刻有不足1秒的批次偏移，不是原子计数。匹配同一仓库ID后比较两端总星；按净增降序，括号是相对起点增长率。

1. Cloudflare Security Audit：61,555 → 71,228，**+9,673（15.71%）**。
2. Hallmark：48,789 → 54,025，**+5,236（10.73%）**。
3. Impeccable：103,964 → 109,008，**+5,044（4.85%）**。
4. Taste：110,676 → 114,325，**+3,649（3.30%）**。
5. Humanizer：65,275 → 68,423，**+3,148（4.82%）**。
6. Karpathy Guidelines：229,888 → 232,557，**+2,669（1.16%）**。
7. Archify：93,875 → 95,942，**+2,067（2.20%）**。
8. Diagram Design：65,298 → 66,732，**+1,434（2.20%）**。
9. I Have ADHD：60,224 → 61,022，**+798（1.33%）**。
10. Last30Days：66,745 → 67,445，**+700（1.05%）**。

用途、平台和风险见总榜。11个候选有匹配周窗口：

- [Book to Skill](https://github.com/virgiliojr94/book-to-skill)：37,697星，37,079 → 37,697，同窗+618（1.67%），列第11。把书籍、文档转为可追溯技能，适合知识整理者；Claude Code、Codex、Copilot、Amp、Hermes、OpenCode、OpenClaw。MIT仅覆盖代码，输入书籍版权需另查。[定义](https://github.com/virgiliojr94/book-to-skill/blob/master/SKILL.md)。
- [归藏PPT Skill](https://github.com/op7418/guizang-ppt-skill)：29,715星，缺少匹配11月6日起点，近周增长**未知**，不写零、不参排。适合汇报制作，Claude Code、Codex等；输出单文件HTML演示稿而非原生PPTX，AGPL-3.0。[定义](https://github.com/op7418/guizang-ppt-skill/blob/main/SKILL.md)。

## 榜单变化与短窗观察

相较[昨日](https://github.com/BruceShenC/News/blob/main/daily/2026-11-12/ai-skills-ranking.md)，总星榜前十顺序不变。Humanizer领先Last30Days由646星扩大到978星。增长榜Hallmark和Impeccable互换第二、第三名，本期近周净增差192星；不据此推断技术质量或实际采用优劣。

相邻快照为11月12日09:31:07.980至11月13日09:35:36.034，实际**24小时4分28.054秒**，不是严格24小时。该独立短窗净增：Security Audit +837、Taste +735、Impeccable +685、Hallmark +596、Humanizer +431、Karpathy Guidelines +324、Archify +290、Diagram Design +193、I Have ADHD +128、Last30Days +99、Book to Skill +83、归藏PPT +57。仅作辅助，不与近周净增混排。

这些是定时总星快照的净差，包含新增与取消星，**不是GitHub Trending的today/this week、安装量或总星除以仓龄**。滚动窗口长度与起点变化，不能仅比较两期净增值声称增长加速；没有独立证据将版本发布与Star增长联系起来。

## 值得深入阅读或隔离验证的两项

### 1. Last30Days：把“热门”与“相关”分开处理

北京时间11月13日04:13:56发布的[正式版3.38.0](https://github.com/mvanhorn/last30days-skill/releases/tag/v3.38.0)增加寻找Digg话题热门内容并排除无关条目的处理；04:45:33发布的[3.38.1](https://github.com/mvanhorn/last30days-skill/releases/tag/v3.38.1)调整评分搜索，在运行截止前结束并保留较突出的副本。此前02:16:05的[3.37.1](https://github.com/mvanhorn/last30days-skill/releases/tag/v3.37.1)涉及长短查询接收及将被拒绝内容排除在确认之外。以上为维护者正式发布说明，不是我们验证过的效果。

适合研究团队细读其相关性过滤和去重边界。建议用一个公开且同名歧义明显的话题，人工核对保留/排除结果、原始链接及超时后的覆盖缺失，避免把“不相关内容被滤掉”与“来源没取到”混为一谈。仅在无私人会话和密钥的隔离环境评估；本期未运行。

### 2. Diagram Design：验证图的语义，而不只看外观

它以单个可移植技能生成HTML/SVG图形，适合技术文档和汇报团队阅读。近周+1,434并不证明图的正确性；本次读取未发现可据以宣布“今日正式升级”的release，最近推送时间也不作为功能发布日期。

可用一个节点关系已知的小系统，检查标签是否完整、箭头方向及分支是否忠实、缩放后文字是否可读，再由作者核对业务含义。此为建议方案，不是已执行测试。与其他设计Skill相比，优先选择它做小范围验证的原因是输出边界相对清晰，容易人工对照。

## 来源与局限

每项仓库链接、技能定义和上述release均为一手来源。Stars来自公开GitHub API的stargazers_count，例如[Security Audit元数据](https://api.github.com/repos/cloudflare/security-audit-skill)；动态数据之后会变化，以标注的批次时间为准。本期所有候选均非归档状态，但“未归档”和“有推送”都不等于维护充分。

已保存带时间、仓库ID、起点及终点总星的观察记录供后续同口径比较。仅公开只读研究，未登录、star、fork、安装、执行候选项目或付费。**热度不等于成熟度、安全性、兼容性或实际效果。**

