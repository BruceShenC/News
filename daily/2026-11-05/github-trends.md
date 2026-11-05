**GitHub 整体与 AI 趋势｜2026-11-05**

**本期结论：**非 AI 热点出现了光盘创作工具和基础工程学习资源；AI 项目则从“多做任务”进一步转向业务语义、审查上下文和内容工作流的可靠性。本期精选 6 项，避开近几日已展开的项目。最值得关注的是 NVIDIA Auto Ontology，但其数据库权限与部署边界，比 Stars 数字更重要。

**观察时间与统计口径**

- 全站日榜、周榜现场读取约为 **11 月 5 日 01:35 UTC，即北京时间 09:35**；README、release 和提交核验至 **09:38**。这是实际观察时点，不冒称 09:30 的历史快照。
- 近 24 小时背景窗口为 11 月 4 日 01:35 至 11 月 5 日 01:35 UTC；近 7 日为 10 月 29 日至 11 月 5 日同一时点。下文发布与提交日期均按 UTC。
- 已强制重新抓取公开 [GitHub Trending 日榜](https://github.com/trending?since=daily)和[周榜](https://github.com/trending?since=weekly)，不采用搜索入口返回的旧缓存。
- **总 Stars 是观察时的存量；today、this week 是 GitHub 页面原始统计，不等于本简报固定窗口内精确的 24 小时、7 日净增。**没有用跨时刻存量相减，也不推断排名升降或增长加速。

本期六项均从全站日榜入选，数字如下：

- DiscImageStudio：总 Stars **6,633**；today **1,778**；this week **未取得**。
- 90DaysOfDevOps：总 Stars **30,440**；today **97**；this week **未取得**。
- NVIDIA/auto-ontology：总 Stars **5,099**；today **2,050**；this week **未取得**。
- PR-Agent：总 Stars **14,782**；today **280**；this week **未取得**。
- Horizon：总 Stars **12,514**；today **121**；this week **未取得**。
- MoneyPrinterTurbo：总 Stars **138,997**；today **617**；this week **未取得**。

以上总量及 today 均来自同批[全站日榜](https://github.com/trending?since=daily)。“未取得”不是零。本次周榜仍可见此前介绍过的 T3 Code，页面为 **14,580 stars this week**，以及 claude-mem 的 **8,822 stars this week**；设计 Skills 也占据多个位置。本期不重复展开，也不将周榜数字与上述日榜数字混排。[周榜来源](https://github.com/trending?since=weekly)

**一、整体项目趋势：不只 AI**

**1. [DiscImageStudio](https://github.com/JiaFeiMiao-K-Cat/DiscImageStudio)：用普通光盘介质制作可见图案**

将图片转换为符合 CD、DVD、单层 BD-R 物理约束的数据，通过刻录在盘面形成肉眼可见的图案。适合光盘创作、数字艺术和介质编码爱好者，核心功能不依赖大模型。

**11 月 4 日 17:34 UTC 发布 [v1.4.0](https://github.com/JiaFeiMiao-K-Cat/DiscImageStudio/releases/tag/v1.4.0)**，属于近 24 小时正式 release。更新加入粗标定、边界标定与精细标定流程，扩展三次轨距拟合，修复刻录速度切换，并隔离多实例图片缓存。采样优化强调保持输出字节和顺序；本期未复测速度或成品效果。

限制：GPL-3.0-or-later。**正式版本不代表每项功能都已稳定**：即时流式刻录以及 Linux、macOS CLI 仍属实验性，Windows 便携 EXE 未签名。仓库明确提示实盘覆盖有限，断电、中断或生成速度不足可能报废盘片，只宜用可丢弃介质验证；不能由 README 的广泛兼容描述推定所有光驱、盘片均已实测。

**2. [90DaysOfDevOps](https://github.com/MichaelCade/90DaysOfDevOps)：基础工程知识重新获得关注**

这是一套 DevOps、DevSecOps 与社区课程学习地图，适合入门者和需要补齐运维基础的开发者。本期归入 GitHub 整体项目观察，**它是学习资源，不是新发布的软件产品**。

入选依据是日榜 **97 stars today**。当前默认分支最近可见提交仍为 **2026 年 6 月 24 日**的[移除 Discord 邀请链接](https://github.com/MichaelCade/90DaysOfDevOps/commit/2941b895eaac2056ec9431a21f3d09a60a3752f8)；最新 release [0.2](https://github.com/MichaelCade/90DaysOfDevOps/releases/tag/0.2)则是 **2022 年 1 月 11 日**。没有核验到近 7 日的新课程交付，不能将此次关注描述为近期更新驱动。

限制：内容采用 **CC BY-NC-SA 4.0**，带非商业使用条件，不应当成 MIT 式软件许可证。部分课程与命令形成于较早年份，适合用来搭知识框架，执行示例前仍应核对工具的当前官方文档。

**二、AI 项目趋势**

**3. [NVIDIA Auto Ontology](https://github.com/NVIDIA/auto-ontology)：让 Agent 先理解业务定义，再查询数据**

从企业元数据构建术语、指标和表关系，结合人工确认的定义支持自然语言查询，并通过 MCP、CLI、API 暴露上下文。适合数据平台、企业分析和 text-to-SQL 团队。重点不只是“生成 SQL”，而是明确收入等指标的含义、关联路径和答案来源。

当前[没有 GitHub releases](https://github.com/NVIDIA/auto-ontology/releases)。**11 月 4 日**的[依赖迁移提交](https://github.com/NVIDIA/auto-ontology/commit/2a24f5d34ef6b6e168a7f22d308504d3ab36a198)将 Ossie converter 改为从 apache/ossie-converters 安装，说明明确是把相同固定代码指向新位置，**不能写成新增推理能力或新正式版本**。同日 **01:17 UTC** 的[文档提交](https://github.com/NVIDIA/auto-ontology/commit/6f2e5f7a0ee467476e135f7c50a84afe1cbffade)补充默认密码更换和端口隔离要求，略早于本期近 24 小时窗口。

限制：Apache-2.0，目前不接受外部贡献。README 明确要求 **3001、3002 端口保持私有**：后端信任前端转发的身份，摄取服务的 ingest、delete、reset、compile 等接口没有独立认证。模型端点可托管或自建，元数据、抽样值与查询的数据路径需要单独审核。语义图与人工认证是降低错误的机制，不是答案正确率保证。

**4. [PR-Agent](https://github.com/The-PR-Agent/pr-agent)：代码审查开始更认真地处理仓库上下文**

为 PR 提供审查、说明、改进建议和问答，支持 CLI、Actions、Docker 等部署方式，适合希望把 AI 审查纳入现有流程的团队。README 明确它是社区维护项目，**不是 Qodo 商业服务的免费套餐**。

**11 月 5 日 00:11 UTC** 的[主分支提交](https://github.com/The-PR-Agent/pr-agent/commit/a8c604794ddb5766dc3fa674cbb64aac6f9f509f)支持展开上下文文件中的 @path 引用。该能力默认关闭，评论参数不能开启，并限制目录越界、嵌套层数、读取次数和总上下文行数。它属于近 24 小时主分支变化；最新正式版仍为 **10 月 25 日 [v0.49.0](https://github.com/The-PR-Agent/pr-agent/releases/tag/v0.49.0)**，不应混为一谈。

限制：MIT。自托管并不意味着代码不外发，实际由所选模型提供方决定。新增上下文可能扩大披露范围，也可能引入不可信仓库内容。README 仍标注 /help_docs 因凭据暴露问题而暂时禁用。建议固定版本或 commit、限制机器人权限，并保留人工审查，不直接把 AI 结论设成合并依据。

**5. [Horizon](https://github.com/Thysrael/Horizon)：把新闻聚合做成可配置的编辑流程**

从 RSS、社区与其他来源收集内容，用 profiles 分别定义筛选、评分、去重和输出结构，再生成中英文简报，支持 Markdown、Pages、邮件和 webhook。适合有固定信息源、希望控制每天阅读量的研究者和开发者。

当前[没有 GitHub releases](https://github.com/Thysrael/Horizon/releases)。最近核验的代码更新是 **11 月 3 日 17:20 UTC** 的[RSS 请求身份修复](https://github.com/Thysrael/Horizon/commit/d7406f738c711df82b8f060fa1d5e9999d36cca0)：为 feed 请求明确标识 Horizon，处理部分出版方拒绝默认 HTTP 客户端的情形。改动仅作用于 feed 请求；**403、410 仍作为来源失败处理**，没有把拒绝访问改成成功或宣称绕过限制。

限制：MIT。摘要、评分和去重可能漏掉重要事实，网页与社区内容也可能不可靠。配置的模型、搜索及分发服务有各自成本和数据流向。建议先用公开来源、关闭自动发布，检查原文链接、日期、重复判定及失败来源是否清晰呈现，再考虑接入日常阅读流程。

**6. [MoneyPrinterTurbo](https://github.com/harry0703/MoneyPrinterTurbo)：视频自动化的难点仍是完整交付**

把脚本、素材、配音、字幕与剪辑串成短视频工作流，提供 WebUI、API、CLI 和 Agent 入口，适合内容工具开发者及批量视频制作流程评估者。

**11 月 4 日 02:11 UTC** 的[主分支提交](https://github.com/harry0703/MoneyPrinterTurbo/commit/c8dbf96e9dfee43b30f7c3f440175d6bd4de61de)修复 FFmpeg 读取控制台按键导致拼接提前结束或阻塞的问题：某些情况下任务会成功退出，但视频只完成一部分。同期还维护了字幕对齐失败后的转录保留。最新正式版仍是 **10 月 21 日 [v1.3.9](https://github.com/harry0703/MoneyPrinterTurbo/releases/tag/v1.3.9)**，上述修复不能当作该版已包含。

限制：MIT 不覆盖所有素材、音色、模型与第三方服务的使用权。模型生成、语音和视频服务可能收费并接收内容；自动发布还涉及平台账号和受众。项目名称不构成收益保证，日榜关注也不代表成片质量或商业回报。验证时应检查完整时长、字幕、素材授权和失败恢复，不能只看任务是否显示成功。

**三、跨项目观察**

1. **上下文质量正在成为独立工程问题。**Auto Ontology 管业务定义与来源，PR-Agent 管仓库规则与引用。更多上下文可能提高实用性，也会扩大权限、外发和不可信输入的边界。
2. **状态“成功”不等于交付正确。**MoneyPrinterTurbo 的视频截断、Horizon 的来源失败、DiscImageStudio 的缓存与标定，都提醒我们检查最终产物和失败路径，而非只看执行返回值。
3. **热度、维护与发布是不同信号。**本期既有近 24 小时正式 release，也有未发布主分支更新，以及没有近期代码变化的学习资源。没有证据把某次提交直接解释为 Stars 上涨原因。

以上是本期样本归纳，不是整个 GitHub 的市场份额结论。

**四、值得深入看两项**

- **Auto Ontology：**先阅读语义定义、人工认证和部署边界，用样例数据库比较“同一指标不同表述”“数据无法回答”“关联路径不明确”三类问题。重点核对 SQL、来源和拒答是否正确，不先接生产数据库。
- **Horizon：**其 profiles、分类配额和来源失败处理，适合研究如何做更可控的个人简报。先用一小组公开来源生成本地草稿，人工核对原文与日期，再评估是否值得进入长期流程。

本期仅公开研究，**未安装、运行或自行复测任何项目**，以上是阅读与隔离验证建议。

**去重与覆盖边界**

已核对 10 月 30 日至 11 月 4 日的已发简报，本期六项均未在这些简报中展开。可对照最近的 [11 月 3 日](https://github.com/BruceShenC/News/blob/main/daily/2026-11-03/github-trends.md)和[11 月 4 日简报](https://github.com/BruceShenC/News/blob/main/daily/2026-11-04/github-trends.md)。

覆盖仅限公开全站日／周榜和所选仓库，不是 GitHub 全量检索。本期为避免重复，六项均取自日榜，这会形成编辑选择偏差。“新入选”不代表首次上榜；热度不等于成熟度、安全性或实际收益。
