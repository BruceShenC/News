**GitHub 整体与 AI 趋势｜2026-11-10**

**本期结论：**值得关注的变化集中在“把任务可靠地做完”：浏览器操作开始支持流程重放，记忆系统修复未提交内容的遗漏，股票分析工具纠正评分语义，家庭自动化则把配置校验交回真实运行环境。本期精选 **6 项：通用开发工具 2 项，AI 应用与基础设施 4 项**，两类不重复。

**观察时间与统计口径**

- 于 **11 月 10 日约 01:33 UTC，北京时间 09:33**，重新抓取公开[全站日榜](https://github.com/trending?since=daily)和[全站周榜](https://github.com/trending?since=weekly)，仓库 README、release 与默认分支核验至约 **09:36**。
- 参考窗口：近 24 小时为 **11 月 9 日至 10 日北京时间 09:33 前后**；近 7 日为 **11 月 3 日至 10 日同一时点前后**。
- **总 Stars 是观察时存量；stars today、stars this week 是 GitHub 榜面原始统计，不等于上述固定窗口的精确净增。**不采用跨时刻总量相减，也不据此推断增长加速。
- 日榜：GBrain **32,169 总 Stars／216 stars today**；HA-MCP **6,000／31**；daily_stock_analysis **68,888／506**；agent-browser **48,502／916**。
- 周榜：Worktrunk **12,280 总 Stars／2,395 stars this week**；Scrapling **100,577／8,226**；agent-browser **48,502／2,994**。
- 未取得的另一窗口数据不补零。下文 release 时间按 **UTC**；版本号、正式发布、预发布与主分支提交分别说明。

**一、整体项目趋势：通用开发工具**

**1. [Worktrunk](https://github.com/max-sixty/worktrunk)：并行开发的难点仍是工作区管理**

Rust 编写的 Git worktree 管理 CLI，简化创建、切换、合并和清理工作区，也提供 hooks、构建缓存共享及 PR 检出。虽然针对并行编码 Agent 设计，核心工作区管理不依赖模型，因此归入通用工具。适合同时处理多个分支的人，而非只有使用 AI 才有价值。

周榜为 **2,395 stars this week**，但最新正式版仍是 **10 月 20 日 21:28 UTC 的 [v0.81.0](https://github.com/max-sixty/worktrunk/releases/tag/v0.81.0)**，不在近 7 日窗口。**11 月 9 日**最新可见[提交](https://github.com/max-sixty/worktrunk/commit/e864cb9ef685701baad487a5a63c3c2c541b490f)是 pre-commit 配置自动更新，不能写成当天新功能发布。

该版本的迁移边界值得补看：前台 hooks 可读取标准输入，但不再通过 stdin 接收旧式 JSON 上下文；模板对象输出也有变化，需显式转为 JSON。清理时部分 hooks 会并发运行，写共享状态的脚本必须协调。

限制：worktree 隔离不等于整个运行环境隔离，数据库、端口、凭据和 hooks 仍可能共享。合并与清理会改变仓库和文件，建议先用可丢弃分支检查未提交内容、hooks 和退出状态，别直接交给自动循环。

**2. [Scrapling](https://github.com/D4Vinci/Scrapling)：网页采集继续向完整交互工作流扩展**

Python 3.10+ 网页采集框架，把 HTTP 请求、浏览器渲染、解析和并发爬取组合起来，也提供 CLI 与 MCP。核心抓取和解析无需大模型，本期不把随附的 Agent Skill 单独计入榜单。

**11 月 4 日 20:29 UTC 的 [v0.5.1](https://github.com/D4Vinci/Scrapling/releases/tag/v0.5.1)** 属于近 7 日正式发布。新增专用远程浏览器会话，明确区分新建独立上下文与复用现有登录状态；AI 交互可获取带时间戳的连续画面，并在拖动过程中观察和调整。解析方面修复 XPath 标量返回值、异常 HTML、畸形链接和部分 feed 读取问题。

**11 月 9 日**默认分支最新可见[提交](https://github.com/D4Vinci/Scrapling/commit/c5121c7c06a8595ed3a0a9b7b7dcd6802b5ebfc5)只是新增赞助商文档，不能解释为上述能力当天发布。

限制：BSD-3-Clause。远程连接存在破坏性迁移，升级前需阅读新版会话接口。复用登录浏览器会引入账号数据与操作权限；自动重试、代理和页面交互能力不代表获得网站访问授权。建议从允许采集的公开页面开始，核对频率、失败记录和字段准确性，不把“抓取成功”当成“内容完整”。

**二、AI 项目趋势**

**3. [agent-browser](https://github.com/vercel-labs/agent-browser)：从逐步点击走向可重放流程**

原生 Rust 浏览器自动化 CLI，以可访问性树和元素引用让 Agent 逐步观察、操作、再检查结果，支持 MCP。日榜 **916 stars today**，周榜 **2,994 stars this week**。

**11 月 10 日 00:37 UTC 发布 [v0.42.0](https://github.com/vercel-labs/agent-browser/releases/tag/v0.42.0)**，属于近 24 小时实质更新。新增 **saved flows，仍属实验性**：记录一次任务后，重放先尝试直接 HTTP，再尝试页面内请求，最后回退到记录的命令，并保存成功方式。发布说明称请求仅发送到公共地址，保存流程时会等待进行中的请求并处理重定向。

同时修复会话复用与重新启动、安全设置跨守护进程重启保留、Safari／iOS 会话、文本输入和附着 Chrome 的下载行为。包声明的最低 Node 版本提升至 **22**。默认分支最新可见[提交](https://github.com/vercel-labs/agent-browser/commit/e7519957b9067c541825afa21a792fbe1f483047)即该版本指向的提交，不另外包装为发布后的新能力。

限制：Apache-2.0。**重放可能再次产生提交、购买或删除等副作用**，不能因为目标是公共地址就认定安全。先选只读任务测试参数变化、登录失效与重复执行；DOM 变化后重新获取引用。它在 11 月 8 日已被介绍，本期因新的正式版本而跟进，不称首次发现。

**4. [GBrain](https://github.com/garrytan/gbrain)：记忆系统同时处理检索成本与落盘完整性**

给现有 Agent 提供带来源的事实记录、纠正和撤回，以及跨客户端检索。可从本地关键词检索开始，再选择语义检索和综合回答。它是完整记忆系统，本期不把其中的 Skill 路由功能当独立 Skill 排名。

**11 月 10 日 01:21 UTC 发布 [v0.60.633.0](https://github.com/garrytan/gbrain/releases/tag/v0.60.633.0)**：加入默认关闭的本地 Skill resolver，用词项匹配从目录选出少量相关项，最多 5 条、400 tokens；该检索环节不依赖模型或运行中的记忆服务。**本地匹配不等于整个 GBrain 工作流都不外发数据。**

更值得看的是同日 **01:04 UTC 的 [v0.60.632.0](https://github.com/garrytan/gbrain/releases/tag/v0.60.632.0)**：修复被 Git 标为 assume-unchanged 或 skip-worktree 的文件明明改动，却从待提交记录中消失的问题。现在无法提交的内容继续保留在待处理记录里，并明确报告失败，避免“没有需要提交的内容”掩盖遗漏。

限制：MIT，自托管免费不代表模型、后台计算和托管服务免费。README 明确指出云端 embedding、重排、提取及综合服务可能接收文本；即使不用单独 API key，宿主模型仍会看到召回内容。远程授权与本地文件、共享数据库凭据是不同边界；Markdown 导出也不是完整数据库备份。适合先验证纠错、撤回、共享可见性和恢复，再接入敏感资料。

**5. [HA-MCP](https://github.com/homeassistant-ai/ha-mcp)：家庭自动化入口更方便，权限也更值得先看**

通过 MCP 让 AI 助手配置和自动化 Home Assistant。推荐方式是在 Home Assistant 内运行集成，覆盖不同安装类型；README 标明该方式要求 **Home Assistant 2026.8.0+**。适合已有家庭自动化系统、希望评估自然语言管理的人。

最新稳定版 **[v9.1.2](https://github.com/homeassistant-ai/ha-mcp/releases/tag/v9.1.2)** 于 **11 月 8 日 15:45 UTC**发布：修复重复商店条目，并通过组件调用 Core 检查 automation、script 和 scene 引用。

**11 月 9 日 22:30 UTC 的 [9.2.0.dev3439](https://github.com/homeassistant-ai/ha-mcp/releases/tag/9.2.0.dev3439)** 明确是预发布，修正 zone 与 helper 图标存储位置。默认分支最新可见提交是该开发版的发布维护，不可把它写成 9.2.0 稳定版已经发布。

限制：MIT。设置需要管理员访问身份，可能影响真实设备和家庭配置。README 提醒长期令牌泄露可被远程使用，秘密连接 URL 也必须保护。建议使用专门身份、默认关闭不需要的远程入口，先在测试实例与非关键设备验证。不要把自然语言理解正确等同于设备操作安全，门锁、安防和高风险电器尤其需要明确确认。

**6. [daily_stock_analysis](https://github.com/ZhuLinsen/daily_stock_analysis)：金融分析工具的价值先看数据和错误语义**

Python 3.10+ 的多市场行情、新闻和 LLM 分析工作台，可生成报告并通过多种渠道推送，支持 GitHub Actions、Docker 和本地运行。本期仅评估软件工程，不提供个股买卖建议。

**11 月 9 日 12:39 UTC 发布 [v3.34.0](https://github.com/ZhuLinsen/daily_stock_analysis/releases/tag/v3.34.0)**。值得关注的不是报告更长，而是纠正 **Market Light 将 VIX 涨跌幅纳入市场上涨评分**的问题，并保留研究超时的阶段、原因和已知 token 用量。另加入可选 FastAPI 安全中间件、重复启动分析跳过选项及股票范围偏移诊断。

**11 月 9 日**默认分支另有[消息序列化处理 NaN／Infinity 的修复](https://github.com/ZhuLinsen/daily_stock_analysis/commit/15c7cdc0ca07bd71158e268fb8596f3ae356ad9b)，应与已发布版本内容分开。发布说明也明确撤回了误合入的集成，不能再把这些项目列为本版新功能。

限制：MIT。免费行情源可能限流、改接口或不稳定；“零成本定时运行”不保证模型、数据和搜索服务全部免费。评分修复本身说明金融指标的含义需要人工核对，模型生成的点位和收益叙述不能作为交易保证。建议先检查来源时间、缺失值、市场差异和历史回测偏差，不连接交易执行。

**三、跨项目观察**

1. **可靠性越来越具体。**GBrain 处理未提交内容，股票分析修评分语义，HA-MCP 校验真实配置引用。评估要看最终状态与数据，而不是只看命令是否返回成功。
2. **复用提高效率，也复用风险。**浏览器保存流程、登录状态、共享工作区和跨 Agent 记忆都能省事，但必须明确会重复什么、共享什么，以及失败后如何恢复。
3. **热门不等于新发布。**Worktrunk 本周受关注但正式版较早；Scrapling 最新提交是文档；HA-MCP 最新列表首先显示开发版。不能把这些都写成“今日升级”。

**四、建议优先深入两项**

- **agent-browser v0.42.0：**选一个公开、只读且可重复核验的网页任务，比较正常运行、参数变化和登录失效时的结果。saved flows 值得研究，但暂不用于有外部副作用的重放。
- **GBrain：**重点检查来源、更正、撤回、待提交记录和备份恢复，而不是先追求记忆条目数量。先以无敏感内容的小数据集验证整个生命周期。

**去重与局限**

已核对[11 月 9 日完整简报](https://github.com/BruceShenC/News/blob/main/daily/2026-11-09/github-trends.md)，本期不重复展开 Sherlock、Astryx、MuJoCo、NVKE、Claude Quickstarts、billion-context、Spec Kit。agent-browser 为新版本跟进；其他项目也不声称历史首次介绍。纯 Skills 与 Skills 合集未计入清单，跨类项目只计一次。

本期仅阅读公开榜单、一手仓库、README 和发布记录，**未安装、运行、登录项目服务或自行复测**。覆盖是编辑选择后的样本，不是 GitHub 全量调查；功能与性能描述以维护者材料为据。Stars 不代表实际使用量、成熟度、安全性或投资价值。

归档正文：[2026-11-10 GitHub 趋势简报](https://github.com/BruceShenC/News/blob/main/daily/2026-11-10/github-trends.md)
