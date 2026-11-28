# AI 独立 Skill 热度双榜｜2026-11-28

**女娲 Skill 反超 Book to Skill，升至总榜第 10，领先 31 星。** Diagram Design 仍是近周增量第 1，领先 Last30Days 260 星。

**口径：** 北京时间 09:01:50.881–09:01:51.123 读取 13 个跟踪仓库的公开 GitHub 详情 API。补查 [agent-skills 主题](https://github.com/search?q=topic%3Aagent-skills+stars%3A%3E30000&type=repositories)、[名称](https://github.com/search?q=skill+in%3Aname+stars%3A%3E30000&type=repositories)、[描述](https://github.com/search?q=skill+in%3Adescription+stars%3A%3E50000&type=repositories)，分别返回 30、25、30 条，有重叠。仅计独立分发的单一 Skill。Archify 100,973 星，默认安装含独立的 `archify-pr`，仅观察；另核对 [Ponytail](https://github.com/DietrichGebert/ponytail) 200,135 星，整仓分发多个独立技能定义，不把整仓星数算给其中一个。以下是 **12 个合格仓库的样本榜**，可能漏掉低星快涨项目。

## 总 Stars 前十

1. **[Karpathy Guidelines](https://github.com/multica-ai/andrej-karpathy-skills)｜236,209**：约束编码改动、明确假设和可验证目标。适合研发与 QA；Claude Code、Cursor。规范不能替代测试。
2. **[Taste](https://github.com/Leonxlnx/taste-skill)｜122,348**：网页设计、素材与浏览器检查。适合前端和设计；Claude Code、Codex 等。引入前核对 hooks 与素材许可。
3. **[Impeccable](https://github.com/pbakaus/impeccable)｜118,061**：界面设计和审查。适合前端和 UI QA；Claude Code、Codex、Cursor、Copilot、Gemini CLI 等。先隔离验证 Live Apply。
4. **[Cloudflare Security Audit](https://github.com/cloudflare/security-audit-skill)｜79,278**：防御性代码审查和证据归档，面向平台与安全 QA；需要支持工具与子 Agent 的宿主。只用于获授权的代码。
5. **[Last30Days](https://github.com/mvanhorn/last30days-skill)｜76,373**：搜集近期多平台讨论并保留来源。适合研究和产品；Claude Code、Codex、Cursor 等。高热讨论仍需核对一手来源。
6. **[Diagram Design](https://github.com/cathrynlavery/diagram-design)｜74,466**：生成 HTML/SVG 技术图解。适合架构和文档；Claude Code、Codex、Cursor、Copilot 等。核对每条连线的方向和依据。
7. **[Humanizer](https://github.com/blader/humanizer)｜74,342**：减少机械化表达。适合作家和编辑；Claude Code、Codex 等。改写后检查是否增删事实。
8. **[I Have ADHD](https://github.com/ayghri/i-have-adhd)｜62,786**：用分步骤和跨轮提示降低阅读负担。适合长任务读写；Claude Code、Cursor；非诊疗用途。
9. **[Hallmark](https://github.com/Nutlope/hallmark)｜62,095**：站点设计和审查。适合前端与设计团队；Claude Code、Codex、Cursor、OpenCode。仍需真实无障碍回归。
10. **[女娲 Skill](https://github.com/alchaincyf/nuwa-skill)｜39,204**：用公开材料整理人物的思维框架，适合研究和内容整理；[定义](https://github.com/alchaincyf/nuwa-skill/blob/main/SKILL.md)列出 Claude Code、DeepSeek Harness 等。输出是模拟，不是人物原话。

## 近 7 日 Star 净增前十

从 [11 月 21 日 09:29:06.515](https://github.com/BruceShenC/News/blob/main/daily/2026-11-21/ai-skills-ranking.md) 到本次 09:01:50.881，约 **6 天 23 小时 33 分**。同仓库 ID 的净差含取消 Star，不是 Trending 数字或安装量。

1. Diagram Design：69,292 → 74,466，**+5,174（7.47%）**
2. Last30Days：71,459 → 76,373，**+4,914（6.88%）**
3. Impeccable：114,060 → 118,061，**+4,001（3.51%）**
4. Hallmark：58,602 → 62,095，**+3,493（5.96%）**
5. Taste：119,349 → 122,348，**+2,999（2.51%）**
6. Security Audit：76,312 → 79,278，**+2,966（3.89%）**
7. Humanizer：71,676 → 74,342，**+2,666（3.72%）**
8. Karpathy Guidelines：234,756 → 236,209，**+1,453（0.62%）**
9. I Have ADHD：61,814 → 62,786，**+972（1.57%）**
10. **[Book to Skill](https://github.com/virgiliojr94/book-to-skill)**：38,583 → 39,173，**+590（1.53%）**。把文档提炼为可追溯 Skill，适合知识团队；Claude Code、Codex、Copilot 等。仅处理有权使用的材料。

[归藏 PPT](https://github.com/op7418/guizang-ppt-skill) 30,528 星，净增 +338，输出网页幻灯片而非原生 PPTX。女娲在 11 月 21 日是另时补查，未在该批同 ID 基线中，不混入增长榜。较[昨日](https://github.com/BruceShenC/News/blob/main/daily/2026-11-27/ai-skills-ranking.md)实际相隔约 **22 小时 24 分**，前三是 Diagram Design +561、Last30Days +560、Impeccable +483；不是严格 24 小时净增。

## 值得看

- **[Last30Days 4.8.0](https://github.com/mvanhorn/last30days-skill/releases/tag/v4.8.0)**：11 月 27 日 12:32（北京时间）发版，调整 macOS Rust 核心的构建环境。适合检查下载的正式发布资产能否在目标 CPU 架构上启动，别只验源码构建或 CI 绿灯。
- **Diagram Design**：用一张带已知方向和节点的推理链路图做验收。结构图容易看起来正确，建议独立对照输入核验每一条边、状态与漏项。以上建议未实测。

**公开快照（仓库 ID:Stars）**：1142983825:236209、1162099055:122348、1097346685:118061、1211139949:100973（仅观察）、1273427276:79278、1140843380:76373、1212769188:74466、1136666433:74342、1238120057:62786、1222917575:62095、1201730379:39204、1226123343:39173、1219042200:30528。[API 示例](https://api.github.com/repos/cathrynlavery/diagram-design)

**项目仓库是否被维护者设为只读归档状态：** 13 个批量观察仓库均为否（`archived=false`）。这是项目属性，不是简报归档状态。未安装或运行项目；Stars 不代表成熟度、安全性或采用率。
