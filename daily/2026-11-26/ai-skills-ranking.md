# AI 独立 Skill 热度双榜｜2026-11-26

**Last30Days 近周净增 5,075 星，仍居第一；Diagram Design +4,820 星，差距从昨日 986 星缩到 255 星。** 总榜前十顺序不变，第十名 Book to Skill 仍只领先女娲 7 星。值得读的是 Last30Days 的去重修复：同一条证据已被一个主题看到，不应让另一个主题漏报。

## 样本与口径

北京时间 **08:58:16.021–08:58:17.079** 批量读取 13 个公开仓库详情 API，核对仓库 ID、README、技能定义、目录树与 Releases。另搜 [agent-skills 主题](https://github.com/search?q=topic%3Aagent-skills+stars%3A%3E30000&type=repositories)、[名称含 skill](https://github.com/search?q=skill+in%3Aname+stars%3A%3E30000&type=repositories)与[描述含 skill](https://github.com/search?q=skill+in%3Adescription+stars%3A%3E50000&type=repositories)，分别检查 30、25、30 条，有重叠。排除框架、市场、合集及附带技能。**这是 12 个合格仓库的样本榜，不是 GitHub 全网排名**，可能漏掉低星快涨项目。[Archify](https://github.com/tt-a1i/archify) 99,922 星仅观察：[3.3.2 发布](https://github.com/tt-a1i/archify/releases/tag/v3.3.2)默认附带独立的 archify-pr，不将整仓热度归给单项。

## 总 Stars 前十

1. [Karpathy Guidelines](https://github.com/multica-ai/andrej-karpathy-skills) **235,717**：用明确假设、小改动和可检验目标约束编码代理。研发与 QA；Claude Code、Cursor。不能替代测试。
2. [Taste](https://github.com/Leonxlnx/taste-skill) **121,567**：网站设计、素材选择与浏览器检查。前端与设计工程师；Claude Code、Codex 等。完整检查需 Node 22+ 和浏览器，留意 hooks 与素材授权。
3. [Impeccable](https://github.com/pbakaus/impeccable) **116,897**：设计、界面审查与润色。前端与 UI QA；Claude Code、Codex、Cursor、Copilot、Gemini CLI 等。默认分支主 Skill 已与 4.11.0 标签一致；[引擎 0.1.31](https://github.com/pbakaus/impeccable/releases/tag/engine-v0.1.31)未列行为变化，Live Apply 宜隔离验收。
4. [Cloudflare Security Audit](https://github.com/cloudflare/security-audit-skill) **78,368**：防御性代码审查、覆盖记录及独立复核。安全与平台 QA；支持工具及子 Agent 的宿主和 Node.js。仅用于授权代码，最近推送仍是 9 月 14 日。
5. [Last30Days](https://github.com/mvanhorn/last30days-skill) **75,110**：汇总近期讨论与引用。研究与产品团队；Claude Code、Codex、Cursor、Copilot、Gemini CLI 等。Skill 和 Release 仍为 4.6.0，讨论热度不是事实核验。
6. [Humanizer](https://github.com/blader/humanizer) **73,592**：减少机械化文风。作者和编辑；Claude Code、Codex 及兼容宿主。复核改写时新增的细节。
7. [Diagram Design](https://github.com/cathrynlavery/diagram-design) **73,037**：生成 HTML／SVG 技术图解。方案、文档与汇报；Claude Code、Codex、Cursor、Copilot、Qoder 等。[主分支插件 2.6.179](https://github.com/cathrynlavery/diagram-design/commit/b228c5d77794a84150fca41b22940bb316b1398d)无对应 Release，仍须核对节点和箭头。
8. [I Have ADHD](https://github.com/ayghri/i-have-adhd) **62,364**：用行动优先、编号和跨轮状态降低阅读负担。适合偏好分步指令的用户；Claude Code、Cursor，[Devin 清单](https://github.com/ayghri/i-have-adhd/pull/256)已合入主分支，未做宿主实测。不作诊断或治疗。
9. [Hallmark](https://github.com/Nutlope/hallmark) **61,101**：网站设计、审查及按需改版。前端与设计团队；Claude Code、Codex、Cursor、OpenCode。自动检查不代替无障碍实测。
10. [Book to Skill](https://github.com/virgiliojr94/book-to-skill) **39,036**：把文档整理成可追溯的知识 Skill。知识库与文档维护者；Claude Code、Codex、Copilot、Amp、Hermes、OpenCode、OpenClaw。只处理有权使用的材料。

[女娲 Skill](https://github.com/alchaincyf/nuwa-skill) 39,029 星，总榜第 11；缺 11 月 19 日同口径快照，不估算近周净增。

## 近 7 日 Star 净增前十

[11 月 19 日基线](https://github.com/BruceShenC/News/blob/main/daily/2026-11-19/ai-skills-ranking.md) 09:26:57.120 至本期 08:58:16.021（北京时间），实际 **6 天 23 小时 31 分 18.901 秒**。两端同一仓库详情 API 按 ID 配对。净差含取消 Star，括号为相对起点增幅，不是 Trending 或安装量。

1. Last30Days：70,035 → 75,110，**+5,075（7.25%）**
2. Diagram Design：68,217 → 73,037，**+4,820（7.07%）**
3. Impeccable：112,917 → 116,897，**+3,980（3.52%）**
4. Hallmark：57,544 → 61,101，**+3,557（6.18%）**
5. Taste：118,440 → 121,567，**+3,127（2.64%）**
6. Security Audit：75,387 → 78,368，**+2,981（3.95%）**
7. Humanizer：70,835 → 73,592，**+2,757（3.89%）**
8. Karpathy Guidelines：234,269 → 235,717，**+1,448（0.62%）**
9. I Have ADHD：61,636 → 62,364，**+728（1.18%）**
10. Book to Skill：38,405 → 39,036，**+631（1.64%）**

[归藏 PPT](https://github.com/op7418/guizang-ppt-skill) 30,425 星，同窗 +364（1.21%），净增第 11。支持 Claude Code、Codex，输出网页幻灯片而非原生 PPTX。

**较昨日：** 净增榜第 9、10 名对换。相邻两期采样相隔 **23 小时 27 分 33.081 秒**，不是严格 24 小时。短窗前三：Diagram Design +975、Last30Days +879、Impeccable +593。

**建议看 Last30Days 的去重修复。** 同一链接首次命中主题 A 和 B，都应分别通知；再次命中 A 不应重复通知。验收要独立查送达记录，不只看任务执行成功。[主分支提交](https://github.com/mvanhorn/last30days-skill/commit/ed7ec6752b766ea3eb62239b374900764e9b5453)，未见进入新稳定版，本期未实测。

**观察记录（仓库 ID:详情 Stars）**：1142983825:235717、1162099055:121567、1097346685:116897、1211139949:99922（仅观察）、1273427276:78368、1140843380:75110、1136666433:73592、1212769188:73037、1238120057:62364、1222917575:61101、1226123343:39036、1201730379:39029、1219042200:30425。[详情 API 示例](https://api.github.com/repos/mvanhorn/last30days-skill)

**项目仓库是否被维护者设为只读归档状态：本期 13 个观察仓库均为否（GitHub `archived=false`）。** 这是仓库属性，不是简报归档状态。宿主支持来自公开声明，未安装运行；Stars 不证明成熟度、安全性或采用率。
