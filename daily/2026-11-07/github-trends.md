**GitHub 整体与 AI 趋势｜2026-11-07**

**本期结论：**代码检查、结构化数据处理、网页采集和 AI 工作流继续获得关注。本期 6 项中，更值得看的是自动化结果是否正确、状态能否隔离，以及成本和回退是否可控。整体工具与 AI 工作流各列 3 项，交叉项目只计一次。

**观察时间与统计口径**

- 公开全站周榜于 **11 月 7 日约 01:29 UTC，北京时间 09:29** 重新抓取，日榜约 **09:30**；README、release 与默认分支提交核验至约 **09:31**。
- 参考窗口：近 24 小时为 **11 月 6 日至 7 日北京时间 09:30 前后**；近 7 日为 **10 月 31 日至 11 月 7 日同一时点前后**。
- **总 Stars 是观察时的存量；today、this week 保留 GitHub 原始口径，不等于严格的 24 小时或 7 日净增。**没有用不同时间的总 Stars 相减计算增长。
- 日榜：Ruff **50,700 总 Stars／169 stars today**；Nushell **40,988／131**；Zed **93,648／306**。[日榜来源](https://github.com/trending?since=daily)
- 周榜：Scrapling **98,853 总 Stars／8,372 stars this week**；Worktrunk **11,806／2,184**；OpenMontage **77,714／4,405**。[周榜来源](https://github.com/trending?since=weekly)
- 以下版本与提交日期统一标注 **UTC**。近期进入榜单不代表项目刚发布，也不代表首次上榜。

**一、整体项目趋势：自动化工具的正确性与边界**

**1. [Ruff](https://github.com/astral-sh/ruff)：自动修复的安全边界比速度更值得关注**

Rust 实现的 Python linter 与 formatter，适合将代码检查、导入整理和格式化纳入编辑器与 CI。它不是类型检查器，也不能替代完整安全审计。

最新正式版 **[0.17.4](https://github.com/astral-sh/ruff/releases/tag/0.17.4)** 于 **11 月 5 日 18:53 UTC** 发布，属于近 7 日而非近 24 小时更新，修复 BOM 文件诊断渲染 panic，并改善不可打印字符的差异展示。G004 自动修复改进与 tarfile 规则仍有 preview 边界。

**11 月 6 日**默认分支 [修复 FURB103](https://github.com/astral-sh/ruff/commit/a3554f468ccfeef2511c4156774bc7d8d8c6c324)：当 write 调用嵌套在布尔、条件或其他函数表达式中时，不再提供可能删除外层表达式的自动修复。其后还有 [B008 对 frozen dataclass 的误报修复](https://github.com/astral-sh/ruff/commit/5fd37cea5d171dd3862bda0472f54a3f1abd3aeb)。**这两项是 main 的变化，不能称已包含在 0.17.4。**

限制：MIT。自动修复仍需检查 diff、运行测试，preview 行为也可能变化。仓库中 ty 的性能结果不能直接算作 Ruff 的提速。建议先用项目现有测试覆盖自动修复，再扩大规则启用范围。

**2. [Nushell](https://github.com/nushell/nushell)：结构化管道仍在打磨日期与兼容性细节**

跨平台 shell，将目录、进程、JSON、TOML 等作为表和记录处理，适合数据筛选、转换与可回放脚本。其核心价值是结构化值，不只是给传统文本管道换一套语法。

最新正式版仍是 **10 月 4 日 15:26 UTC** 的 **[0.116.1](https://github.com/nushell/nushell/releases/tag/0.116.1)**，不属于近 7 日发布。**11 月 5 日** main [修复 date to-timezone](https://github.com/nushell/nushell/commit/4492226e1bdfc9f02708792678838a647716da73)，保留带秒数的时区偏移，并拒绝错误后缀和分隔符，改善历史时区的往返转换及日期取整。

**11 月 6 日**最新可见 [提交](https://github.com/nushell/nushell/commit/192e5533546f89eddbb65cc73359aca1598a3076)只调整配置路径测试的比较方式，作者明确写明“Test only, no behavior change”，不能包装成当天用户功能更新。

限制：MIT。README 仍提示部分命令可能不稳定、设计会变化。宜先用于可回放的数据处理脚本，检查结构化值与外部程序之间的转换、时区和编码边界，不宜未经验证就替换全部生产 shell 脚本。

**3. [Scrapling](https://github.com/D4Vinci/Scrapling)：网页采集升级，需要同步检查浏览器状态隔离**

Python 网页采集框架，整合 HTTP、浏览器渲染、解析、并发采集、暂停恢复与导出。核心采集不依赖模型，也提供 MCP 和 Agent Skill 入口，因此归入整体工具区，不把它描述成完全没有 AI 功能。

最新正式版 **[v0.5.1](https://github.com/D4Vinci/Scrapling/releases/tag/v0.5.1)** 于 **11 月 4 日 20:29 UTC** 发布，新增远程浏览器专用 RemoteSession／AsyncRemoteSession，加入短视频逐帧观察与分步拖放，并修复 XPath 空字符串、0、false 的处理，以及并发 robots.txt 获取和缓存临时文件冲突。

**远程浏览器连接存在破坏性变更，需要使用新的 session 类。**默认 new_context=True 创建独立上下文；设为 False 会复用默认上下文与登录态，不能再视为隔离会话。**11 月 5 日**最新可见 [main 提交](https://github.com/D4Vinci/Scrapling/commit/bc602488b44e978da3266841e913af17577ec0fd)只是添加视频示例文档，不是新版本。

限制：BSD-3-Clause。浏览器依赖需另装；访问网站仍应遵守条款、robots.txt 与隐私要求，反检测宣传不构成访问授权。建议用获授权的测试页验证空值、零值和并发恢复，再考虑扩大采集规模。

**二、AI 项目趋势：长任务、并行工作与生产流程**

**4. [Zed](https://github.com/zed-industries/zed)：AI 编辑器升级要先看回退机制和版本通道**

Rust 协作编辑器，覆盖 macOS、Linux、Windows，提供 Agent 与外部 Agent Client Protocol 入口。它同时是常规开发工具与 AI 编辑器，本期只计一次。

正式版 **[v1.27.0](https://github.com/zed-industries/zed/releases/tag/v1.27.0)** 于 **11 月 4 日 17:04 UTC** 发布，按需加载 Agent 的 diff 与 Markdown，减轻长历史的 CPU 和磁盘负担；AI commit message 可参考最近三条提交的风格，并修复 disable_ai=true 时仍读取 LLM 凭据的问题。

升级前特别注意：**v1.27.0 要求 macOS 12+，并移除了 agent checkpoints 和 Restore Checkpoint 按钮。**版本说明建议拒绝编辑或用 Git 撤销，不能继续假设每轮都有检查点。

另有同日发布的 **[v1.28.0-pre](https://github.com/zed-industries/zed/releases/tag/v1.28.0-pre)**，它才包含长 turn 自动压缩只运行一次、缓存复用以及受限模式仍执行 worktree task hooks 等修复。**这些预发布变化不能写成稳定版已具备。**11 月 6 日的 [main 提交](https://github.com/zed-industries/zed/commit/9a8ab54db4f9f9af4dc6efb42b304c55c6a62bd3)又增加关闭 Agent 自动生成线程标题的选项，以减少额外 token 和推理开销，也未据此宣称已进入正式版。

限制：主要为 GPL-3.0-or-later，标注组件采用 Apache-2.0；Web 版尚不可用。性能与成本收益为维护者描述，未自行测量。优先核对升级通道、Git 回退和凭据设置，再测试真实项目的长任务。

**5. [Worktrunk](https://github.com/max-sixty/worktrunk)：并行 AI 编程首先是工作目录和清理问题**

Rust Git worktree 管理工具，用分支名创建、切换、查看和清理工作目录，并提供合并、hooks 与构建缓存。它不是模型，但 README 明确面向并行 AI Agent，因此列在 AI 工作流区，不充作纯非 AI 项目。

最新正式版 **[v0.81.0](https://github.com/max-sixty/worktrunk/releases/tag/v0.81.0)** 发布于 **10 月 20 日 21:28 UTC**，不属于本周新发布。近 24 小时的具体变化是 **11 月 6 日** main [修复 PowerShell 按前缀补全](https://github.com/max-sixty/worktrunk/commit/6fdb42ab39ae4c16c5917191005b7dc96f8441ed)，不能认为已进入上述正式版。

现有版本的 hooks 参数、模板和并发执行规则值得单独核对。根据 [安全说明](https://worktrunk.dev/hook/#security)，项目 hooks 首次执行及命令变化时需要批准，但用户全局配置的 hooks 不要求同样确认；并发 hooks 仍可能竞争共享文件或 Git 状态。

限制：[MIT 或 Apache-2.0 双许可，可择其一](https://github.com/max-sixty/worktrunk/blob/main/LICENSE)。合并和清理可能涉及提交、变基、删除 worktree 或分支。**Git worktree 是工作目录隔离，不是进程或权限安全沙箱。**应先在可丢弃仓库检查 hooks、共享缓存和回退路径，再扩大并行规模。

**6. [OpenMontage](https://github.com/calesthio/OpenMontage)：视频 Agent 开始把审批、预算和产物检查放进流程**

Python AI 视频生产系统，把资料检索、脚本、素材生成、剪辑与合成串起来，由外部 AI 编程助手编排。它包含真实素材、AI 视频及静态图动画等路径，并非只有 Skills 清单。

当前 README 描述了 Backlot 分镜审批、预算估算／预留／结算、渲染前检查，以及渲染后的 ffprobe、帧和音频检查。这些是**当前能力描述，不是已核实的本周新增功能**。

仓库目前 [没有 GitHub Releases](https://github.com/calesthio/OpenMontage/releases)。最新可见 main 是 **11 月 3 日 17:41 UTC** 的 [README 早期用户邀请](https://github.com/calesthio/OpenMontage/commit/d21f5d4eaa08422b09c7091757a05affcbabb35b)，因此本期入选依据是周榜热度，而非刚发布正式版。

限制：[AGPL-3.0](https://github.com/calesthio/OpenMontage/blob/main/LICENSE)，对外提供改造后的网络服务应审查源码提供义务，模型与素材另有条款。需要 Python、FFmpeg、Node 和 AI 编程助手；云供应商可能接收素材并产生费用。Ollama／LM Studio 本地 LLM 支持仍标为 Coming soon，不能说全流程已经离线。可配置预算机制也不保证任何情况下都不会超支，效果与可靠性尚未自行复测。

**三、跨项目观察**

1. **正确性比“自动完成”更值得追踪。**Ruff 限制危险修复，Nushell 严格处理日期边界，Scrapling 修补空值与并发行为。这些变化可能比功能数量更影响真实工作流。
2. **并行与长任务的成本正在显性化。**Zed 处理长历史、自动标题和回退，Worktrunk 处理目录与 hooks，OpenMontage 引入审批与预算。它们解决不同层面的约束，不能仅用 Agent 数量比较。
3. **隔离、权限和可恢复性仍要分别验证。**浏览器 context、Git worktree、预算阈值都不是完整安全保证。这是本期样本中的工程方向，不是 GitHub 全站占比结论。
4. **热度、维护和发布是三件事。**本期包括周内正式版、main 修复、测试或文档提交，也有无 release 的项目；没有证据将某次提交直接解释成 Stars 上涨原因。

**四、值得进一步深入的两项**

- **Ruff：**用代表性的 Python 代码检查自动修复 diff、preview 规则和测试回归，尤其关注嵌套表达式是否保持语义。
- **Worktrunk：**在可丢弃仓库梳理项目与全局 hooks 的权限差别，检查共享缓存、并发清理和错误后的恢复。先证明可控，再增加并行 Agent。

**去重与局限**

已阅读 [11 月 6 日完整简报](https://github.com/BruceShenC/News/blob/main/daily/2026-11-06/github-trends.md)，不重复展开 Floci、Hyprland、Ghostty、agent-desktop、DeepTutor、VibeVoice；并依据前期记录避开 11 月 5 日已展开的 Auto Ontology、PR-Agent。没有逐日复核更早全部归档，**不声称近一周完全不重复**。纯 Skills 仓库留给独立 Skill 榜，交叉项目未重复计数。

本期仅阅读公开榜单和一手仓库资料，**未安装、运行或自行复测任何项目**。覆盖限于本次全站日／周榜与所选仓库，不是 GitHub 全量调查；Stars 不等于实际使用量、成熟度或安全性。
