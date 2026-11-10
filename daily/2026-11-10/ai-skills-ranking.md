**AI 独立 Skill 热度双榜｜2026年11月10日**

今天的变化：Cloudflare Security Audit 超过 Last30Days，升至样本总星榜第5；近周净增仍居首。Last30Days 与 Humanizer 本次仅差7星，不能把这个瞬时差距当作稳定排名。Impeccable 正式发布 Skill v4.8.2，接入配置的可撤销性值得读，但不能因“可撤销”而省略权限审查。

**范围与观察时间**

总星统一采自公开 GitHub 仓库 API，北京时间 **11月10日09:34:28.126–09:34:28.949**。本期复核12个持续跟踪候选的 README、主 SKILL.md 与完整递归目录树，均未截断；排名只计独立分发的单一 Skill，多宿主副本、测试夹具及内部开发辅助技能不另计。

补搜 GitHub 的 [agent-skills 主题且超过3万星](https://github.com/search?q=topic%3Aagent-skills+stars%3A%3E30000&type=repositories)、[名称含 skill 且超过3万星](https://github.com/search?q=skill+in%3Aname+stars%3A%3E30000&type=repositories)、[描述含 skill 且超过5万星](https://github.com/search?q=skill+in%3Adescription+stars%3A%3E50000&type=repositories)，各取按总星排序的前30项，结合昨日候选复核。因此这是**已检索、已核验样本榜，不是 GitHub 全网穷尽前十**；关键词缺失及低星快增项目可能遗漏。

本期特别复核：
- [Ponytail](https://github.com/DietrichGebert/ponytail) 虽自称“一个提示”，当前分发 ponytail、review、audit、debt、gain、help 等不同 SKILL.md，按多技能合集排除，不能把整仓星数归给主技能。
- [Nature Skills](https://github.com/Yuan1z0825/nature-skills) 有24个技能定义，[Academic Research Skills](https://github.com/Imbad0202/academic-research-skills) 有5个，均排除。
- [REA](https://github.com/morluto/rea) 的核心为逆向工程 MCP/CLI 综合产品，Skill 是接入说明，不作为独立 Skill 仓库计星。UI UX Pro Max、Graphify、Caveman等沿用此前的多技能或综合产品排除口径。

**一、总 Stars 前十**

1. [Karpathy Guidelines](https://github.com/multica-ai/andrej-karpathy-skills)｜**231,377**
约束编程代理的假设、改动范围与验证目标，适合研发、QA和代码审查者。支持 Claude Code、Cursor规则及兼容宿主。Skill 声明 MIT，但仓库 API 许可证字段仍为空；最近推送4月20日。提示规则不能替代执行测试。

2. [Taste](https://github.com/Leonxlnx/taste-skill)｜**112,541**
网站设计、素材选择及浏览器检查，适合前端与设计工程师。Claude Code、Codex等；当前 Skill 2.5.1，MIT。完整检查依赖 Node 22+、Chrome/Edge/Chromium；注意 hooks、联网取素材及图片许可。

3. [Impeccable](https://github.com/pbakaus/impeccable)｜**106,909**
界面设计、审查、润色和确定性规则检测，适合前端、设计及界面质量团队。Claude Code、Codex、Cursor、Copilot、Gemini CLI等；Apache-2.0。包含引擎二进制、hooks及宿主配置修改，必须先审查执行权限，不能当纯文本提示。

4. [Archify](https://github.com/tt-a1i/archify)｜**95,067**
把架构、流程和代码证据转成可交互HTML图解，适合架构师、工程师和技术文档作者。Claude Code、Codex、Cursor、OpenCode；Apache-2.0。核心稳定版仍标3.2.0；结构校验不能证明业务语义正确，分享前检查私有架构信息。

5. [Cloudflare Security Audit](https://github.com/cloudflare/security-audit-skill)｜**68,104**
组织覆盖驱动审查、独立验证及结构化发现，适合安全、平台和QA团队。需支持工具调用与子Agent的编码宿主，校验器需Node.js；MIT。最近推送仍为9月14日，热度上涨不代表今天发布新能力；只审计获授权目标，执行不可信代码必须隔离。

6. [Last30Days](https://github.com/mvanhorn/last30days-skill)｜**67,133**
聚合近期多平台讨论并保留来源，适合研究、产品和内容团队。Claude Code、Codex、Cursor及Agent Skills宿主；MIT，当前Skill声明3.32.0。部分来源需密钥、登录会话或付费额度，讨论热度不等于事实可信度。

7. [Humanizer](https://github.com/blader/humanizer)｜**67,126**
编辑机械化表达并保留原意，适合写作者和编辑。Claude Code、Codex、Pi及兼容宿主；MIT，Skill声明3.1.0。改写仍需核验事实和语义，不能保证绕过AI检测，也不应把偏好评测当事实正确性证明。

8. [Diagram Design](https://github.com/cathrynlavery/diagram-design)｜**66,096**
生成品牌化HTML/SVG图表，适合方案、技术文档与汇报。Claude Code、Codex、Cursor、Copilot、Qoder、Pi等；MIT。官方描述45类图形，但类型多不代表自动产物正确；需要检查数据、连线、最终可见性及外部字体依赖。

9. [I Have ADHD](https://github.com/ayghri/i-have-adhd)｜**60,644**
把回复组织为行动优先、编号步骤和跨轮状态提示，适合希望降低阅读负担的人。Claude Code、Cursor副本及兼容Skill宿主；MIT，最近推送10月19日。它是输出风格指导，不是诊断或治疗工具。

10. [Hallmark](https://github.com/Nutlope/hallmark)｜**51,814**
新页面设计、审计和明确调用后的改版，适合前端与设计团队。Claude Code、Codex、Cursor、OpenCode；MIT，Skill声明2.0.0。Node18+，完整渲染检查另需浏览器/Puppeteer；样式规则不能替代用户研究与无障碍实测。

**二、近7日 Star 净增前十**

共同起点是[11月3日实际采样](https://github.com/BruceShenC/News/blob/main/daily/2026-11-03/ai-skills-ranking.md)的北京时间20:45:06.247，共同终点为本次09:34:28.126。**实际窗口6天12小时49分21.879秒（564,561.879秒），不是严格168小时**，原因是11月3日为延迟补采。各项均用这一窗口，不折算、不混排；两端批次分别约1.16秒、0.82秒，不是仓库瞬时同步计数。

按总星快照净增降序，括号为相对起点的增长率：

1. Cloudflare Security Audit：57,827 → 68,104，**+10,277（17.77%）**。
2. Impeccable：101,089 → 106,909，**+5,820（5.76%）**。
3. Hallmark：46,518 → 51,814，**+5,296（11.38%）**。
4. Taste：109,213 → 112,541，**+3,328（3.05%）**。
5. Humanizer：64,215 → 67,126，**+2,911（4.53%）**。
6. Karpathy Guidelines：228,840 → 231,377，**+2,537（1.11%）**。
7. Archify：92,991 → 95,067，**+2,076（2.23%）**。
8. Diagram Design：64,541 → 66,096，**+1,555（2.41%）**。
9. I Have ADHD：59,896 → 60,644，**+748（1.25%）**。
10. Last30Days：66,431 → 67,133，**+702（1.06%）**。

用途、平台和风险见总榜。[Book to Skill](https://github.com/virgiliojr94/book-to-skill) 为37,484星，同窗口+650（1.76%），在11个具有共同历史端点的候选中列第11。它把书籍、文档整理为可追溯技能材料，支持Claude Code、Codex、Copilot、Amp、Hermes、OpenCode、OpenClaw等；MIT覆盖代码，输入材料的版权另行核查。

[归藏PPT Skill](https://github.com/op7418/guizang-ppt-skill) 为29,530星，已核验根目录唯一SKILL.md，支持Claude Code、Codex等，生成的是单文件HTML演示稿，而非原生PPTX。AGPL-3.0，脚本、字体与图片许可需分别检查。没有匹配11月3日起点，近周增长未知，不写零、不进入增长榜；本期总星作为后续基线保留。

以上为公开仓库总星快照差值，包含新增与取消星的净结果，**不是GitHub Trending的today/this week，也不是安装量或总星除以仓龄**。

**榜单变化与短窗观察**

相比[11月9日](https://github.com/BruceShenC/News/blob/main/daily/2026-11-09/ai-skills-ranking.md)，总星榜 Security Audit 与 Last30Days 交换第5、第6，其他名次不变；前者领先后者971星。Humanizer距Last30Days仅7星，这个差距可能很快变化。

近周增长前十的顺序与昨日相同，但本期窗口比昨日短约11小时，不能把净增数字变小说成降速。另列相邻观察：11月9日09:26:23.304 → 11月10日09:34:28.126，**24小时8分4.822秒**，不是严格24小时。Security Audit +1,214，Hallmark +835，Impeccable +679，Humanizer +604，Taste +489，Karpathy Guidelines +419，Archify +322，Diagram Design +207，Book to Skill +127，Last30Days +109，I Have ADHD +99。短窗只作辅助，不与近周榜混排。

**值得深入阅读或隔离验证的两项**

1. **Impeccable Skill v4.8.2：读接入配置的精确撤销机制。**
[正式Release](https://github.com/pbakaus/impeccable/releases/tag/skill-v4.8.2) 发布于北京时间11月10日03:55:30。新增/完善 agents 设置：为Claude Code、Codex配置hooks、MCP和指令块，撤销时只移除工具添加的内容，保留用户修改；图片生成支持OPENAI_BASE_URL，下载遭服务器错误时明确区分服务故障。
适合先在无敏感信息的空白项目检查配置差异、撤销、用户修改保留情况。这里是维护者发布说明，本期未执行验证；“可撤销”不等于获得修改宿主配置或审批行为的授权。Skill v4.8.2、CLI v4.8.2和engine v0.1.22是不同发布对象，不混为一版。

2. **Cloudflare Security Audit：读“指导模式”与“完整审计”的边界。**
[主SKILL定义](https://github.com/cloudflare/security-audit-skill/blob/main/skills/security-audit/SKILL.md) 明确加载技能不自动授权六阶段审计或创建报告；完整流程要求明确审计请求，疑似发现还要交给新Agent反证与独立核验。对QA团队的价值是把“产生候选”与“确认事实”分开，而不只是增加Agent数量。
可用一个明确授权、可隔离的玩具仓库检查误报率、证据完整性和未解决问题是否被诚实保留；这是建议方案，未在本期运行。它最近推送时间未变，今日推荐依据是持续热度和流程可借鉴性，不是新版本发布。

**来源与局限**

总星来自每个仓库的公开API，例如[Security Audit元数据](https://api.github.com/repos/cloudflare/security-audit-skill)；当前用途、平台、许可证以官方README和Skill定义为依据，不代表实机兼容性验证。资格复核依据完整目录树，例如[Ponytail](https://api.github.com/repos/DietrichGebert/ponytail/git/trees/HEAD?recursive=1)。动态页面之后可能变化，数字以本次采样为准。未以推送时间推断功能变化，也未把关注变化归因于某次发布。

本期只读研究，未登录、安装或运行候选项目。**热度不等于成熟度、安全性或实际效果。**
