# GitHub 整体与 AI 趋势｜2026-11-21

**给 QA 的优先项：** 看 AG-UI 怎么处理取消后未结束的工具调用，再用 Chrome DevTools MCP 采集可复核的浏览器证据。两个项目分别补齐 Agent 的协议层和运行时验证。

**采集口径：** 北京时间 11 月 21 日 09:25–09:26，公开 GitHub Trending 全语言 [Today](https://github.com/trending?since=daily) 与 [This week](https://github.com/trending?since=weekly) 快照。总 Star 与两个榜单新增字段分开报告；“未列”不是 0；页面口径不是自行计算的滚动 24 小时或 7 天净增。以下提交、发布日均按北京时间，入选不等于当日发布，也不把 Star 当作质量或成熟度证明。

**和昨天比：** [昨日简报](https://github.com/BruceShenC/News/blob/main/daily/2026-11-20/github-trends.md)的六个项目今天仍在日榜，本期不重复推荐。两个变化最明显的例子是 MiniMind 的日榜显示值由 +238 到 +847，OpenSRE 由 +38 到 +466；这是两次页面字段的比较，不代表严格的日净增或增长率。

## 一、全站热点

### 1. Ghost Downloader 3：跨平台下载管理
**总 Star 13,718｜今日 +1,082｜本周未列。** 面向需要集中管理普通文件下载的用户，提供分段下载和任务续传能力。[仓库](https://github.com/XiaoYouChR/Ghost-Downloader-3)  
**新证据：** 11 月 20 日 v4.5.11 新增五种语言，并修复 Windows 任务栏不支持进度显示时反复报错。维护者同时预告更新放缓。[发布说明](https://github.com/XiaoYouChR/Ghost-Downloader-3/releases/tag/v4.5.11)  
**边界：** 发布说明未证明对不同网络或站点都能提速；不要据此判断所有平台功能一致。

### 2. App Store Compliance：移动端提交前规则检查
**总 Star 2,760｜今日 +869｜本周未列。** 为移动端研发、QA 和发布团队提供审核清单及自动检查，用于提前发现部分配置风险。[仓库](https://github.com/mjmirza/app-store-compliance)  
**新证据：** 11 月 16 日 v2.4.0 把更多上传链路纳入扫描，并增加 Android release 可调试标记、64 位 ABI 等检查。[发布说明](https://github.com/mjmirza/app-store-compliance/releases/tag/v2.4.0)  
**边界：** 第三方清单不等于平台最新规则或过审承诺；静态规则不能证明产物和运行时行为合规。可先做告警，不直接拦截发布。

### 3. Univer：把表格、文档引擎嵌入业务应用
**总 Star 30,951｜今日未列｜本周 +7,539。** 面向要在产品内嵌编辑能力的应用平台团队，提供表格、文档、公式、渲染和插件系统。[仓库](https://github.com/dream-num/univer)  
**新证据：** 11 月 20 日主干修复撤销操作后恢复旧版图形的问题，适合做导入旧数据、编辑、再撤销的场景回归。[提交](https://github.com/dream-num/univer/commit/4767b3b309467c283bbd52b725536ca73861e0fc)  
**边界：** Apache-2.0 覆盖公开 Web SDK 核心，部分 Web 插件、Server SDK 和 AI 扩展另行授权；不能用完整演示推断开源版能力。

## 二、AI 项目

### 4. AG-UI：Agent 与用户界面的事件协议
**总 Star 20,821｜今日未列｜本周 +3,621。** 将运行、消息、工具和人工介入等状态表达为事件，适合 Agent 前端、网关与 SDK 团队定义端到端契约。[仓库](https://github.com/ag-ui-protocol/ag-ui) · [协议](https://docs.ag-ui.com/spec/1.0)  
**新证据：** 11 月 21 日合入 LangGraph 适配修复，处理运行取消后悬空的工具调用。[提交](https://github.com/ag-ui-protocol/ag-ui/commit/a6f2539a2ad9ba016129217637b83b7068adc6c1)  
**边界：** 协议是事件格式与语义，不保证每个适配器同样处理取消、断线重连或重复事件；需对你们实际的客户端与服务端分别验证。

### 5. Chrome DevTools MCP：浏览器运行时的调试证据
**总 Star 58,543｜今日未列｜本周 +3,306。** 让编程 Agent 通过 Chrome 查看控制台、网络请求、截图与性能跟踪，适合 Web QA 和前端排障。[仓库](https://github.com/ChromeDevTools/chrome-devtools-mcp)  
**新证据：** 11 月 20 日修复 CLI 对负数参数的判断，使行为与所用解析器一致。[提交](https://github.com/ChromeDevTools/chrome-devtools-mcp/commit/eb9b030280e028df0b33a1d8721349002b3cf18f)  
**边界：** 官方支持 Google Chrome 与 Chrome for Testing；MCP 客户端可读写浏览器数据。只在隔离实例用脱敏测试数据，不接个人浏览器；采集证据也不替代明确断言。

### 6. Modern Web Guidance：把浏览器兼容性要求放到编码前
**总 Star 9,535｜今日未列｜本周 +5,224。** 这是技能的源码和评估仓库，为编程 Agent 提供 Web 平台最佳实践及浏览器兼容性数据，适合前端技术与质量规范维护者。[仓库](https://github.com/GoogleChrome/modern-web-guidance-src) · [官方说明](https://developer.chrome.com/docs/modern-web-guidance)  
**新证据：** 11 月 21 日主干为旧用法扫描新增文件数量上限，并在扫描不完整时显式报告覆盖不足。[提交](https://github.com/GoogleChrome/modern-web-guidance-src/commit/634d0f83bb41092832797411d148350196b8258d)  
**边界：** “部分扫描未发现问题”不是“兼容性通过”；还要按真实浏览器版本、企业 WebView 和目标用户范围运行测试。

### 7. Plannotator：人工批注计划、规格与代码差异
**总 Star 13,902｜今日未列｜本周 +3,555。** 为 AI Coding 团队提供本地浏览器评审界面，能在计划和差异上定位批注，再把意见送回对应 Agent 会话。[仓库](https://github.com/backnotprop/plannotator)  
**新证据：** 11 月 21 日主干合并了共享 Markdown 界面的四项阅读体验修复。[提交](https://github.com/backnotprop/plannotator/commit/23587ea5aee12a246d2b22f24dc29cb9f5f8f301)  
**边界：** 人工评审入口不等于审批门禁；分享可能让拿到完整链接的人读到内容。内部试点应关掉分享并保留审批记录。

### 8. Firecrawl：把网页内容变成 Agent 可用输入
**总 Star 205,293｜今日 +1,241｜本周未列。** 面向知识接入和检索团队，把网页抓取、解析成 Markdown 等结构化输入；仓库区分 Local、自托管服务器和云服务。[仓库](https://github.com/firecrawl/firecrawl)  
**新证据：** 11 月 21 日两个主干变更分别涉及托管 MCP 请求鉴权和数据调用的搜索来源标记，可作为鉴权与可追溯性测试线索。[鉴权变更](https://github.com/firecrawl/firecrawl/commit/41b36dfd28ad69df7da221d1d51f3d5fb4838be3) · [来源标记](https://github.com/firecrawl/firecrawl/commit/e6f43250c51fb9811f1e60c016f146599d38b9e6)  
**边界：** 不同部署形态的能力和数据路径不同；Local 的部分搜索和数据服务仍可能走云，不应把它理解成全链路离线。抓取成功也不等于内容完整、来源可信。

## 三、两条趋势与两项建议

- **验证开始分成三层。** Modern Web Guidance 提供编码前规则，Plannotator 保留人工评审，DevTools MCP 补运行时证据。三者能互补，但没有任何一层等同于通过测试。
- **异常状态正进入 Agent 契约。** AG-UI 的取消收尾、Firecrawl 的来源标记、扫描工具报告覆盖不足，都提醒我们：前端和运维需要能区分“做完了”“只做了一部分”和“已取消”。

**优先试两件事：**

1. **在现有网关加一条取消契约。** 用测试替身模拟“已开始流式输出、工具未返回就取消”，检查前端退出加载态、工具状态有明确结局、服务端不再输出，关联日志可定位同一运行。
2. **在已有 Playwright 用例旁做证据实验。** 用同一批历史前端缺陷，对照只跑自动化与补采控制台、网络、性能证据两种做法，记录复现成功率、确认耗时和人工复核成本。先补证据，不替换原有断言。

本期为公开来源调研，未安装、运行或复测所列项目。
