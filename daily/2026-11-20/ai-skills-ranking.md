# AI 独立 Skill 热度双榜｜2026年11月20日

**总星榜前十不变；近周净增榜Last30Days升至第五。Security Audit与Taste的近周净增仅差34星，不宜视为稳定领先。** 新发布重点是Book to Skill 2.0.1对文档提取证据与版权边界的处理。

## 范围与口径

北京时间 **09:32:23.277–09:32:24.162** 批次读取12个公开仓库详情API，非原子同时计数；统一以前者为观察锚点。仓库ID与历史匹配。初次直接请求部分返回403，随后通过GitHub连接器成功读取全部公开元数据、未截断目录树及最新Release；全部README和主SKILL.md也重新读取。本期排名不混入较早失败批次或搜索结果的星数。

补搜 `topic:agent-skills stars:>50000`、`skill in:name stars:>50000`，分别返回21、16项，存在重叠。只计独立分发单一Skill，排除框架、市场、awesome清单、多技能合集及综合软件附带技能；新增出现的agentic-awesome-skills属于合集，不参排。宿主副本、测试夹具及Archify内部贡献者辅助技能不另计独立产品。**这是已检索样本榜，不是GitHub全网穷尽前十**；高星筛选可能漏掉低星快增项。

## 一、总 Stars 前十

平台支持为项目声明，未实机验证；总星观察时间统一为上述批次。

1. **[Karpathy Guidelines](https://github.com/multica-ai/andrej-karpathy-skills)｜234,505 Stars**  
明确假设、最小改动和可验证目标，适合研发、QA及代码审查者。支持Claude Code，提供Cursor规则。Skill声明MIT；本期API许可证字段仍为空，最近推送4月20日。提示规范不能替代测试。[技能定义](https://github.com/multica-ai/andrej-karpathy-skills/blob/main/skills/karpathy-guidelines/SKILL.md)。

2. **[Taste](https://github.com/Leonxlnx/taste-skill)｜118,915 Stars**  
指导网站设计、素材选择和浏览器完成检查，适合前端及设计工程师。支持Claude Code、Codex等宿主；MIT，主Skill仍为2.5.1。完整检查依赖Node 22+和Chromium类浏览器，需审查hooks、联网取素材及素材许可。[技能定义](https://github.com/Leonxlnx/taste-skill/blob/main/skills/taste-skill/SKILL.md)。

3. **[Impeccable](https://github.com/pbakaus/impeccable)｜113,495 Stars**  
组织UI设计、审查、润色和规则检测，适合前端及界面质量团队。支持Claude Code、Codex、Cursor、Copilot、Gemini CLI等；Apache-2.0。主分支Skill与最新Skill Release均为4.10.0，北京时间11月19日06:26:52发布，不是今日新版本，不与引擎或CLI版本混同。README仍披露live模式Apply的CLI回退可能关闭审批、权限检查与沙箱，不宜未经审查启用。[技能定义](https://github.com/pbakaus/impeccable/blob/main/plugin/skills/impeccable/SKILL.md)。

4. **[Archify](https://github.com/tt-a1i/archify)｜98,051 Stars**  
把架构、流程及代码证据转成可交互HTML图解，适合架构师和技术文档作者。支持Claude Code、Codex、Cursor、OpenCode；Apache-2.0。结构验证不保证业务语义正确；分享前检查源码和私有架构信息。[技能定义](https://github.com/tt-a1i/archify/blob/main/archify/SKILL.md)。

5. **[Cloudflare Security Audit](https://github.com/cloudflare/security-audit-skill)｜75,852 Stars**  
覆盖驱动的安全审查、独立验证与结构化报告，适合安全、平台及QA团队。完整流程需要工具调用、子Agent和Node.js校验器；MIT，最近推送仍为9月14日。实际采用前应评审隔离、权限和证据要求。[技能定义](https://github.com/cloudflare/security-audit-skill/blob/main/skills/security-audit/SKILL.md)。

6. **[Humanizer](https://github.com/blader/humanizer)｜71,276 Stars**  
编辑机械化表达并尽量保留原意，适合写作者和编辑。支持Claude Code、Codex及兼容宿主；MIT，主Skill 4.0.0。4.0.0允许补充原文没有的具体细节，需逐项核对事实；自然文风不保证事实准确或任何AI检测结果。[技能定义](https://github.com/blader/humanizer/blob/main/SKILL.md)。

7. **[Last30Days](https://github.com/mvanhorn/last30days-skill)｜70,816 Stars**  
汇总近期多平台讨论与来源，适合研究、产品和内容团队。支持Claude Code、Codex、Cursor、Copilot、Gemini CLI等；MIT，主Skill及最新正式Release仍为4.3.2，发布时间为11月17日23:56:44。部分来源涉及密钥、登录会话或付费额度，互动量不等于事实可靠性。[技能定义](https://github.com/mvanhorn/last30days-skill/blob/main/skills/last30days/SKILL.md)。

8. **[Diagram Design](https://github.com/cathrynlavery/diagram-design)｜68,647 Stars**  
生成HTML/SVG图解，适合方案、技术文档和汇报。支持Claude Code、Codex、Cursor、Copilot、Qoder等；MIT。README现声明51类图形，仍需核对数值、连线、语义与字体网络依赖。[技能定义](https://github.com/cathrynlavery/diagram-design/blob/main/skills/diagram-design/SKILL.md)。

9. **[I Have ADHD](https://github.com/ayghri/i-have-adhd)｜61,722 Stars**  
强调行动优先、编号步骤和跨轮状态提示，适合偏好较低阅读负担的人。提供Claude Code插件与Cursor副本，也可用于兼容Skill的宿主；MIT；最近推送仍为10月19日。它是表达方式指导，不是诊断或治疗工具。[技能定义](https://github.com/ayghri/i-have-adhd/blob/main/skills/i-have-adhd/SKILL.md)。

10. **[Hallmark](https://github.com/Nutlope/hallmark)｜58,072 Stars**  
提供新页面设计、审计和明确调用后的改版流程，适合前端及设计团队。支持Claude Code、Codex、Cursor、OpenCode；MIT，主Skill 2.0.0。脚本和渲染检查有环境要求，照片遵循各自许可；自动检查不能替代无障碍实测。[技能定义](https://github.com/Nutlope/hallmark/blob/main/skills/hallmark/SKILL.md)。


## 二、近7日 Star 净增前十

起点为[11月13日快照](https://github.com/BruceShenC/News/blob/main/daily/2026-11-13/ai-skills-ranking.md)北京时间09:35:36.034，终点为本次09:32:23.277，实际 **6天23小时56分47.243秒**，不是严格168小时。两端均为仓库详情API，按总星净差降序，包含新增和取消Star；括号为相对起点增长率，不是日均速度。

1. [Security Audit](https://github.com/cloudflare/security-audit-skill)：71,228 → 75,852，**+4,624（6.49%）**
2. [Taste](https://github.com/Leonxlnx/taste-skill)：114,325 → 118,915，**+4,590（4.01%）**
3. [Impeccable](https://github.com/pbakaus/impeccable)：109,008 → 113,495，**+4,487（4.12%）**
4. [Hallmark](https://github.com/Nutlope/hallmark)：54,025 → 58,072，**+4,047（7.49%）**
5. [Last30Days](https://github.com/mvanhorn/last30days-skill)：67,445 → 70,816，**+3,371（5.00%）**
6. [Humanizer](https://github.com/blader/humanizer)：68,423 → 71,276，**+2,853（4.17%）**
7. [Archify](https://github.com/tt-a1i/archify)：95,942 → 98,051，**+2,109（2.20%）**
8. [Karpathy Guidelines](https://github.com/multica-ai/andrej-karpathy-skills)：232,557 → 234,505，**+1,948（0.84%）**
9. [Diagram Design](https://github.com/cathrynlavery/diagram-design)：66,732 → 68,647，**+1,915（2.87%）**
10. [Book to Skill](https://github.com/virgiliojr94/book-to-skill)：37,697 → 38,498，**+801（2.12%）**

重复项目的用途、平台和风险见总榜。[Book to Skill](https://github.com/virgiliojr94/book-to-skill)为38,498星，把书籍文档转为可追溯技能，适合知识整理者；支持Claude Code、Codex、Copilot、Amp、Hermes、OpenCode、OpenClaw。MIT只覆盖代码，不解决输入作品版权。[技能定义](https://github.com/virgiliojr94/book-to-skill/blob/master/SKILL.md)。

I Have ADHD同窗+700（1.15%），列第11；[归藏PPT](https://github.com/op7418/guizang-ppt-skill)为30,131星，同窗+416（1.40%），列第12。适合演讲作者，支持Claude Code、Codex，AGPL-3.0，输出单文件HTML而非原生PPTX。

## 榜单变化与短窗观察

相较[昨日](https://github.com/BruceShenC/News/blob/main/daily/2026-11-19/ai-skills-ranking.md)，总星榜顺序不变；近周榜Last30Days与Humanizer交换第五、第六，其他名次不变。Security Audit近周净增4,624、Taste为4,590，相差34，远小于它们单日变化，不据此推断稳定优势。滚动起点改变，也不能单凭相邻两期近周差值判断加速。

11月19日09:26:57.120至本次09:32:23.277，共 **24小时5分26.157秒**，两端均为仓库详情API。以下仅作短窗辅助，不混入近周排序：

Karpathy Guidelines +236；Taste +475；Impeccable +578；Archify +296；Last30Days +781；Security Audit +465；Humanizer +441；Diagram Design +430；I Have ADHD +86；Hallmark +528；Book to Skill +93；归藏PPT +70。

这些不是GitHub Trending的today/this week，不是安装量，也没有用总星除以仓龄推算速度。

## 值得深入阅读的两项

**1. Book to Skill 2.0.1：把“提取到内容”与“证据仍有效”分开。** 北京时间11月20日05:48:15正式发布；2.0.0为前一日21:22:36。2.0.1修正旧页码证据残留、页数不匹配的证据传播、表格单元格段落边界和跨行章节标题等，并停止把未经核实的Crossref DOI直接判成开放获取。适合QA用自有或明确开放许可文档，人工对照章节、表格及页码来源，检查更新输入后旧证据是否清除。这是维护者发布说明，本期未运行或验证效果。其可选MCP检索与发布功能需要另审权限和来源合法性，不因工具可下载就认定内容可使用或公开。[2.0.1发布说明](https://github.com/virgiliojr94/book-to-skill/releases/tag/v2.0.1)。

**2. Diagram Design：把图的语义校验与外观检查拆开。** 当前主Skill提供图形类型与生成规范，但没有正式Release可支持“今天升级”的说法。其近周净增1,915星仅是关注度。适合用已知节点和关系的非敏感样本，分别检查箭头方向、数值与标签完整性，以及不同宽度的可读性；不要把HTML/SVG能打开或视觉漂亮当作业务含义正确。这是建议方案，未执行。[技能定义](https://github.com/cathrynlavery/diagram-design/blob/main/skills/diagram-design/SKILL.md)。

补充版本边界：Impeccable今天02:16:28发布的是 **engine-v0.1.28** 预编译引擎；Skill仍是昨日4.10.0。引擎Release说明未给出足够变更细节，本期不猜测新增能力，也不重复当作新Skill发布。[引擎发布记录](https://github.com/pbakaus/impeccable/releases/tag/engine-v0.1.28)。

## 公开观察记录与限制

来源为各仓库公开详情API，例如[Security Audit元数据](https://api.github.com/repos/cloudflare/security-audit-skill)。以下均采用本期09:32:23.277批次锚点，供后续同口径比较。

| 仓库 | GitHub ID | 总Stars |
|---|---:|---:|
| Karpathy Guidelines | 1142983825 | 234505 |
| Taste | 1162099055 | 118915 |
| Impeccable | 1097346685 | 113495 |
| Archify | 1211139949 | 98051 |
| Last30Days | 1140843380 | 70816 |
| Security Audit | 1273427276 | 75852 |
| Humanizer | 1136666433 | 71276 |
| Diagram Design | 1212769188 | 68647 |
| I Have ADHD | 1238120057 | 61722 |
| Hallmark | 1222917575 | 58072 |
| Book to Skill | 1226123343 | 38498 |
| 归藏PPT | 1219042200 | 30131 |

**项目仓库是否被维护者设为只读归档状态：本期12项均为否（GitHub archived=false）。** 这与本简报是否保存到News仓库是两回事；也不等于维护充分、成熟或安全。仅研究公开资料，未安装、运行、star、fork或付费；未把版本发布与涨星作因果关联。热度不等于实际效果。
