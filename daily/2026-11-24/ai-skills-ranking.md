# AI 独立 Skill 热度双榜｜2026-11-24

**两榜前十顺序不变。Last30Days 近周净增 +5,303 星，领先 Impeccable 1,058 星。** 今天值得看 Diagram Design：它不仅校验发布 ZIP 有多大，还检查裁剪后会不会缺少技能实际引用的文件。

**样本与口径：** 北京时间 **09:26:12.050–09:26:13.378** 读取 13 个候选的公开详情 API，核对 ID、README、技能定义、目录与近期 Releases。补搜 [agent-skills 主题](https://github.com/search?q=topic%3Aagent-skills+stars%3A%3E30000&type=repositories)、[名称含 skill](https://github.com/search?q=skill+in%3Aname+stars%3A%3E30000&type=repositories)、[描述含 skill](https://github.com/search?q=skill+in%3Adescription+stars%3A%3E50000&type=repositories)，抽查 30、25、30 条，有重叠。排除框架、市场、合集、综合工具附带能力；宿主副本、示例和测试夹具不重复计。**这是 12 个合格仓库的样本榜，不是全站排名**；高星门槛和历史覆盖可能漏掉低星快涨项目。

[Archify](https://github.com/tt-a1i/archify) 99,127 星，仅作观察：可选 [archify-pr](https://github.com/tt-a1i/archify/blob/main/archify-pr/SKILL.md)虽依赖核心，但有自己的技能定义且可单独选择安装，不把整仓热度算给其中一项。

## 总 Stars 前十

1. **[Karpathy Guidelines](https://github.com/multica-ai/andrej-karpathy-skills)｜235,321**：帮助编码代理明确假设、控制改动和定义验证目标。研发／QA；Claude Code、Cursor 规则。技能声明 MIT，仓库 API 许可字段为空；提示不替代测试。
2. **[Taste](https://github.com/Leonxlnx/taste-skill)｜120,669**：网站设计、素材与浏览器检查。前端／设计工程师；Claude Code、Codex 等，MIT，2.5.1。完整检查需 Node 22+ 和浏览器，先审阅 hooks 与素材授权。
3. **[Impeccable](https://github.com/pbakaus/impeccable)｜115,649**：设计、审查和润色界面。前端／界面 QA；Claude Code、Codex、Cursor、Copilot、Gemini CLI 等，Apache-2.0。主 Skill 4.10.0；Loop 0.2.2 是伴随工具版本，Live Apply 先查权限边界。
4. **[Cloudflare Security Audit](https://github.com/cloudflare/security-audit-skill)｜77,564**：防御性代码审查和覆盖记录。安全／平台 QA；适合支持配套工具的 Agent 宿主，MIT。最近推送为 9 月 14 日，增星不等于发版。
5. **[Last30Days](https://github.com/mvanhorn/last30days-skill)｜73,450**：跨平台汇总近期讨论并引用。研究／产品／内容；Claude Code、Codex、Cursor、Copilot、Gemini CLI 等，MIT，4.6.0。热度不等于事实。
6. **[Humanizer](https://github.com/blader/humanizer)｜72,750**：减少机械化文风。作者／编辑；Claude Code、Codex 等，MIT，4.0.0。检查是否添加原文没有的细节。
7. **[Diagram Design](https://github.com/cathrynlavery/diagram-design)｜70,987**：生成单文件 HTML/SVG。方案／文档／汇报；Claude Code、Codex、Cursor、Copilot、Qoder 等，MIT。新增 [Git 分支和发布图](https://github.com/cathrynlavery/diagram-design/commit/4773b47f78e218329c35751db1bc425a9e63b0dc)，README 共列 52 类；仍需验数据和方向。
8. **[I Have ADHD](https://github.com/ayghri/i-have-adhd)｜62,066**：用行动优先、编号和跨轮状态降低阅读负担。适合偏好分步指引的人；Claude Code、Cursor，MIT，0.4.1。不作诊疗。
9. **[Hallmark](https://github.com/Nutlope/hallmark)｜60,094**：站点设计、审查和改版。前端／设计；Claude Code、Codex、Cursor、OpenCode，MIT，2.0.0。不替代无障碍和浏览器实测。
10. **[女娲 Skill](https://github.com/alchaincyf/nuwa-skill)｜38,891**：从人物材料整理思考框架。研究／内容推演；Claude Code、Codex、Cursor 等，MIT，2.0.2。生成内容不是人物原话；缺同口径近周起点。

## 近 7 日 Star 净增前十

[11 月 17 日](https://github.com/BruceShenC/News/blob/main/daily/2026-11-17/ai-skills-ranking.md)用的是 GitHub Search API 精确仓库查询，因此本期另用同口径搜一次。北京时间 **11 月 17 日 09:37:27.446 → 11 月 24 日 09:26:54.354–09:26:58.612**，约 **6 天 23 小时 49 分**。同 ID 快照相减，包含取消 Star；括号为相对增幅，不同于 Trending 或安装量。搜索端 Taste 比详情端多 1、Diagram Design 多 2；两接口不拿来计算实时增量。

1. Last30Days：68,147→73,450，**+5,303（7.78%）**
2. Impeccable：111,404→115,649，**+4,245（3.81%）**
3. Hallmark：56,150→60,094，**+3,944（7.02%）**
4. Taste：116,945→120,670，**+3,725（3.19%）**
5. Security Audit：73,987→77,564，**+3,577（4.83%）**
6. Diagram Design：67,651→70,989，**+3,338（4.93%）**
7. Humanizer：69,930→72,750，**+2,820（4.03%）**
8. Karpathy Guidelines：233,749→235,321，**+1,572（0.67%）**
9. [Book to Skill](https://github.com/virgiliojr94/book-to-skill)：38,137→38,870，**+733（1.92%）**。把文档制成可追溯的知识 Skill，面向知识团队；Claude Code、Codex、Copilot、Amp、Hermes、OpenCode、OpenClaw，MIT，2.0.1。只处理有权使用的材料。
10. I Have ADHD：61,398→62,066，**+668（1.09%）**

[归藏 PPT](https://github.com/op7418/guizang-ppt-skill) 30,315 星、近周 +419（1.40%），第 11；Claude Code、Codex，AGPL-3.0，输出网页幻灯片而非原生 PPTX。女娲不估算；Archify 整仓同窗 +1,951，不入榜。

**对比[昨天](https://github.com/BruceShenC/News/blob/main/daily/2026-11-23/ai-skills-ranking.md)：** 近周领先差 684→1,058 星；起点及接口不同，不据此认定增速变化。两天详情锚点相距 23 小时 59 分 33.641 秒；短窗前三 Last30Days +715、Diagram Design +673、Impeccable +614，不是严格 24 小时。

**验证建议：** Diagram Design 的 Claude Desktop ZIP 门禁最多 180 个文件、解压后不超过 30 MiB，且不能漏掉主技能引用。团队分发可照这三项再加一条：从实际产物安装后，宿主能否发现主技能。项目主干已合入、清单 2.6.174，未见对应 Release，也未自行安装实测。[ZIP 提交](https://github.com/cathrynlavery/diagram-design/commit/817d8666a574226fcf1f1585b0a1228d21ca7794) · [清单](https://github.com/cathrynlavery/diagram-design/commit/0eeda75a708f419560d5e032693618bb9954f17b)

**观察记录（ID:详情 Stars）**：1142983825:235321、1162099055:120669、1097346685:115649、1211139949:99127（仅观察）、1273427276:77564、1140843380:73450、1136666433:72750、1212769188:70987、1238120057:62066、1222917575:60094、1201730379:38891、1226123343:38870、1219042200:30315。[详情示例](https://api.github.com/repos/mvanhorn/last30days-skill) · [同口径搜索示例](https://api.github.com/search/repositories?q=repo%3Amvanhorn%2Flast30days-skill)

**项目仓库是否被维护者设为只读归档状态：本期 13 个观察仓库均为否（GitHub archived=false）。** 这是项目属性，与简报归档无关。未运行候选项目；平台支持来自公开声明，热度不证明成熟度、安全性或实际采用率。
