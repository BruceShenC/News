**GitHub 整体与 AI 趋势｜2026-11-04**

本期精选 6 项，均避开 11 月 2 日、3 日已展开介绍的项目。总体看，通用开发基础设施仍有关注度，AI 配套工具则继续走向长期运行、企业服务接入和知识治理。这里是有限样本精选，不是 GitHub 全网排名。

**观察窗口与统计口径**

- 北京时间 2026 年 11 月 4 日 **09:28–09:29**，现场读取公开榜单及仓库 README、release、commit。此次读取略早于计划的 09:30，不冒称该时点快照。近 24 小时与近 7 日背景分别以观察时点回看；下文提交、发布日期沿用来源的 UTC 日期。
- 来源为 [全站日榜](https://github.com/trending?since=daily)、[全站周榜](https://github.com/trending?since=weekly)，补充 [Go 周榜](https://github.com/trending/go?since=weekly) 与 [Rust 周榜](https://github.com/trending/rust?since=weekly)。使用现场新抓取，不以搜索缓存代替实时榜单。
- **总 Stars 是存量；today / this week 是 GitHub Trending 页面原始统计，不是本简报固定起止时刻的 24 小时 / 7 日净增。**没有用两次快照复算，也不计算增长加速。未取得的数字不是零。
- 同批观察记录：Ghostty 总 Stars **63,442**，全站 this week **771**；Docker Compose **38,592 / Go this week 143**；Gitleaks **30,650 / Go this week 261**；Spotify Portal AI Plugins **3,047 / 全站 today 91**；Herdr **47,886 / Rust this week 1,719**；RAGFlow **93,448 / Go this week 520**。除 Spotify 取得日榜数字外，其余入选项目未取得日增；Spotify 未取得周增。语言榜与全站榜不混排。

**一、整体开源项目趋势：不只 AI**

**1. [Ghostty](https://github.com/ghostty-org/ghostty)：原生终端与可嵌入终端核心**

解决开发者日常终端交互，以及其他应用嵌入终端的需求。README 区分 macOS/Linux 原生应用与面向 C、Zig 的 libghostty-vt，适合终端重度用户、IDE 和开发工具作者。

本期依据是全站周榜 771 stars this week，同时有 **11 月 3 日主分支维护**：修复 macOS 标签栏首次出现时布局被裁切，并优化终端样式扫描。两者是具体工程维护，不是新的模型能力。见 [标签栏修复提交](https://github.com/ghostty-org/ghostty/commit/c7f68a7f48c6a5b357689eb8c2328bc2dab5d8ed) 与 [样式扫描提交](https://github.com/ghostty-org/ghostty/commit/6fba0873efb7765e9d0f79de449063f3214b3d75)。作者给出的特定基准性能不能外推为所有终端任务的加速。

限制：GitHub 的 [tip release](https://github.com/ghostty-org/ghostty/releases/tag/tip) 明确是滚动 nightly，不是稳定版本；README 也说明 libghostty API 签名仍在变化。做嵌入开发应固定提交，日常使用优先官方稳定版。

**2. [Docker Compose](https://github.com/docker/compose)：多容器应用的启动与管理**

用声明式配置协调应用、数据库及其他依赖，适合应用开发、平台工程及本地集成环境维护者。Go 周榜显示 143 stars this week，热度不算“爆发”，但有实质版本背景。

**11 月 2 日发布 [v5.7.0](https://github.com/docker/compose/releases/tag/v5.7.0)**：将 Up 的 Create、Start 阶段合为基于同一次 daemon 状态观察的执行计划；新增跨项目统计及聚合等功能。发布说明特别提醒作为库调用的用户：参数组合校验、错误时机和部分 API 类型发生变化。**11 月 3 日**另有 [events 流正常结束处理修复](https://github.com/docker/compose/commit/a74e89a7ccd258128743f11bf64adb5cdd3d8a19)，属于发布后的主分支，不能算进 v5.7.0 已交付内容。

限制：Apache-2.0。普通 CLI 用户与嵌入 Compose API 的开发者受到的影响不同；升级应检查库接口迁移、容器启动失败行为及凭据挂载，不只核对配置能否解析。

**3. [Gitleaks](https://github.com/gitleaks/gitleaks)：代码与提交历史中的密钥扫描**

检测误入源码、文件和历史提交的凭据，可输出 JSON、SARIF、JUnit 等报告，适合开发团队、CI 维护者及安全工程师。本期作为通用安全工具介绍，不把它归为大模型测试框架。

入选依据是 Go 周榜 **261 stars this week**。须明确：这不是当天新发布。当前 [release 列表](https://github.com/gitleaks/gitleaks/releases) 最新仍为 **v8.30.1**，资产记录时间为 **2026 年 3 月 21 日 UTC**；[提交页](https://github.com/gitleaks/gitleaks/commits) 最近可见记录为 **10 月 23 日**，没有核验到近 7 日新提交。近期关注与功能交付不能混为一谈。

限制：MIT。规则匹配存在误报与漏报，基线和忽略规则会影响覆盖；扫出密钥后的吊销、轮换与历史处置仍需单独流程。不要把“没有发现”当作没有泄露的证明。

**二、AI 项目趋势**

**4. [Spotify Portal AI Plugins](https://github.com/spotify/portal-ai-plugins)：把企业软件目录接入编码 Agent**

面向已使用 Spotify Portal 的平台团队，提供目录与文档搜索、服务负责人/健康/事件简报、只读诊断，以及带预览和确认的操作流程。README 支持 Claude Code、Codex、Cursor、pi。这是插件与工作流仓库，不是本期另一份简报所统计的独立单 Skill。

全站日榜显示 **91 stars today**。仓库当前 [没有 releases](https://github.com/spotify/portal-ai-plugins/releases)；最近变化为 **10 月 23 日 [停止处理被拒任务时的 kill-switch 循环](https://github.com/spotify/portal-ai-plugins/commit/aa252eb21df5bbf32b0beb45f84a2c03341ccad8)**，此前 10 月 21 日加入 Fleetshift 工作流。因此这是今日关注的既有项目，不写成过去 24 小时的新产品发布。

限制：Apache-2.0。开源插件不代表无需 Portal 服务、认证或相应服务权限。工作流既有读取，也有触发任务、创建 PR 等写操作；实际试用应先选只读诊断、假数据与最低权限，不能因 README 提到确认机制就默认端到端安全。

**5. [Herdr](https://github.com/herdrdev/herdr)：让多个编码 Agent 的终端持续运行**

通过后台终端服务、面板状态和远程机器连接管理 Claude Code、Codex、Cursor、OpenCode 等既有工具，适合同时跑多个编码任务、经常切断 SSH 或切换机器的开发者。Rust 周榜显示 **1,719 stars this week**。

README 的关键边界很清楚：关闭客户端或断开 SSH 后后台任务可继续；**服务器或机器重启后恢复的是布局及受支持的会话，不是原进程存活**。最新正式版仍为 **10 月 14 日 [v0.9.5](https://github.com/herdrdev/herdr/releases/tag/v0.9.5)**，修复前缀快捷键、Codex 状态跟踪及部分 Windows 兼容问题。近 7 日 [提交页](https://github.com/herdrdev/herdr/commits) 可见 10 月 28–29 日的维护与文档指引调整，不据此宣称本周发布新运行时功能。

限制：Apache-2.0。它管理终端和会话，不替代底层 Agent 的权限隔离；插件、socket 接口、SSH 及会话持久化需要分别评估。重点应验证任务状态识别与断线恢复，而不是只看能同时打开多少面板。

**6. [RAGFlow](https://github.com/infiniflow/ragflow)：从文档检索走向 Agent 上下文基础设施**

将文档解析、知识组织、多步检索和 Agent 工作流结合，适合企业知识库、检索增强应用及上下文平台团队。Go 周榜显示 **520 stars this week**。

**10 月 30 日 [v1.0.0-rc3](https://github.com/infiniflow/ragflow/releases/tag/v1.0.0-rc3)** 加入工具循环内证据是否充分的判断、分类组件、多版本摄取流水线 DSL，并在聊天历史展示 prompt、token 用量和执行时间。虽然页面标为 Latest，发布说明明确它是 **1.0 正式版前的第三个候选版本**。**11 月 3 日 [主分支](https://github.com/infiniflow/ragflow/commits)**继续加入 MinerU 解析超时配置、调整团队共享说明，并维护文件操作的作用域限制。

限制：Apache-2.0。rc3 不等于正式 1.0，也不能假设已包含后续权限边界修复。共享数据、多租户部署与模型提供方的数据流向应先审查；建议用隔离的测试资料和固定版本验证，再接真实企业知识。

**三、跨项目观察与本期变化**

1. **工程基础设施的价值仍独立于 AI。**Ghostty 的交互体验、Compose 的执行计划、Gitleaks 的凭据扫描，各自解决具体开发问题。本次补看语言榜，是为了避免全站 AI 热点挤掉这些项目；不代表它们拥有相同热度规模。
2. **Agent 配套从“会调用”转向“能持续运行、能接企业上下文”。**Herdr 处理会话生命周期，Portal 连接软件目录和服务操作，RAGFlow组织知识与检索。新增能力同时扩大状态保留、操作权限及数据治理责任。
3. **热度与新发布经常错位。**本期只有 Compose 有近一周正式版，RAGFlow 是候选版，其他项目既有主分支维护，也有仅近期关注增加的情况。新入选是编辑选择，不能解释为首次上榜或增长加速。

与 [11 月 2 日](https://github.com/BruceShenC/News/blob/main/daily/2026-11-02/github-trends.md)、[11 月 3 日简报](https://github.com/BruceShenC/News/blob/main/daily/2026-11-03/github-trends.md)相比，本期不重复展开 claude-mem、agent-desktop、Worktrunk、Colibri 等。日榜仍能看到其中多项，但缺少完整同口径历史榜单，**不报告升降名次**。

**四、值得深入看或隔离验证**

- **Docker Compose v5.7.0：**对已有多容器工作流的团队最直接。先读执行计划和库 API 迁移说明，用可丢弃环境检查启动失败、重建与依赖处理，收益与风险都容易界定。
- **Herdr：**对并行编码 Agent 用户有清晰需求。建议用无敏感数据的任务检查 SSH 断线、客户端退出、主机重启三种不同情形，以及阻塞状态是否准确；不要把会话恢复误认为进程容灾。

以上是建议，**本次未安装、执行项目或自行复测**。热度不等于成熟度、安全性或适合生产。覆盖仅限公开全站日/周榜、Go/Rust 周榜与上述六个仓库，语言选择和编辑去重均会产生样本偏差；不是 GitHub 全量检索，也未以总 Stars 推算近期速度。

[本期归档](https://github.com/BruceShenC/News/blob/main/daily/2026-11-04/github-trends.md)
