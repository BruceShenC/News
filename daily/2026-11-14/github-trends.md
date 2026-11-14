# GitHub 整体与 AI 趋势｜2026-11-14

观察窗口：北京时间 11 月 13 日 09:30 至 11 月 14 日 09:30；近 7 日作为背景。精选 6 个项目，与昨日所列项目去重。

**数据口径与限制：**日榜取自 GitHub Trending 的 11 月 14 日 09:32:36 北京时间快照，周榜取自当日 09:20:45 快照。抓取服务命中上述当天缓存，不是无延迟实时数据。下列“今日新增”“本周新增”均是 GitHub 页面原标注，不是自行保存的严格 24 小时／168 小时净增；未拿到的另一周期不补零。仓库 README、发布页与最新提交另行核验。这是可见榜单中的编辑精选，不是全网排名，也不能据一次快照断言持续增长。来源：[日榜](https://github.com/trending?since=daily)、[周榜](https://github.com/trending?since=weekly)。

## 一、整体开源项目趋势

### 1. Uptime Kuma：自托管监控仍有实际需求，安全更新比热度更重要

[仓库](https://github.com/louislam/uptime-kuma)｜总 Stars **93,872**；日榜标注 **71 stars today**。

用于 HTTP、TCP、DNS、WebSocket、推送监控与状态页，适合小团队运维、自托管服务维护者。11 月 12 日正式发布 **2.5.11**，更新依赖并修复一项需要管理员权限的远程代码执行问题；发布说明表示细节延后 7 日披露。它属于近 7 日背景，不冒充过去 24 小时发布。仓库还可见 11 月 12 日针对 Push monitor 实时数据显示的后续提交，不能直接认定该提交已包含在 2.5.11 中。[发布说明](https://github.com/louislam/uptime-kuma/releases/tag/2.5.11)、[后续提交](https://github.com/louislam/uptime-kuma/commit/732da77bc45e3b251980fb14eb224957f3069933)。

**落地限制：**MIT 许可；README 明确不支持将数据放在 NFS 上，默认示例会监听所有网络接口。已有部署者应优先核对版本、数据备份和管理入口暴露情况，不因“自托管”而默认安全。

### 2. Tailcat：把加密点对点传输做成小工具，而非完整 VPN

[仓库](https://github.com/tailscale/tailcat)｜总 Stars **9,470**；日榜标注 **256 stars today**。

将 Tailscale 的数据平面封装为类似 netcat 的 CLI／Go 库，支持 WireGuard 端到端加密、NAT 穿透和必要时经 DERP 中继；不要求 Tailscale 账户或修改系统路由。适合开发者临时传输、远程调试及受控端口转发。

**本期依据是今日上榜，不是新版本。**最新可见提交为 11 月 5 日采用 netstack 关闭修复，最新正式 release 为 9 月 20 日 **v0.7.0**，均不包装为近 7 日新进展。[提交](https://github.com/tailscale/tailcat/commit/e00cdde6ff787f801361ff85ed085775c775d4c5)、[发布页](https://github.com/tailscale/tailcat/releases/tag/v0.7.0)。

**落地限制：**默认公共中继有限速；浏览器演示目前只能通过中继连接，不能套用 CLI 的直连预期。加密传输也不能替代对连接地址、转发端口及接收方的权限管理。

### 3. Sherlock：老牌工具显著上榜，但没有对应的新功能发布证据

[仓库](https://github.com/sherlock-project/sherlock)｜周榜总 Stars **111,152**；标注 **13,364 stars this week**。

按用户名检查多个社交网站上的公开账户，适合本人数字足迹检查或获授权的开源情报研究。README 列出 400 多个站点及 CSV／XLSX 等输出方式。

**需要降温解读：**最新可见提交只是 11 月 1 日调整横幅，最新正式发布 **v0.16.2** 在 9 月 8 日。周榜关注度明显，但没有证据把这轮增长归因于近期技术突破。[提交](https://github.com/sherlock-project/sherlock/commit/c588d7c8b476df9f9c84912a05d1430fb8cdcdae)、[发布页](https://github.com/sherlock-project/sherlock/releases/tag/v0.16.2)。

**落地限制：**MIT 许可；同名账户不等于同一人，站点规则变化可能造成误报。README 还警告部分第三方系统包存在问题；不宜将输出直接用于身份判断或不利决定。

## 二、AI 项目趋势

### 4. Plannotator：竞争点从“多开 Agent”转向“人如何审阅和反馈”

[仓库](https://github.com/backnotprop/plannotator)｜总 Stars **10,469**；日榜标注 **108 stars today**。

提供本地浏览器审阅界面，把 Agent 的计划、文档、HTML 产物及代码差异集中展示，再将批注送回相应会话。适合同时使用 Claude Code、Codex、Gemini CLI 等工具的开发者和评审者。

11 月 13 日正式发布 **v0.28.64**：增加视频产物筛选，GitLab 整文件评论改为独立讨论线程，安装更新支持压缩二进制；这些是协作与交付体验改进，不是模型能力提升。11 月 14 日主分支已有后续提交，勿把分支状态与该正式版本混为一谈。[发布说明](https://github.com/backnotprop/plannotator/releases/tag/v0.28.64)、[主分支后续提交](https://github.com/backnotprop/plannotator/commit/8b96e80315f35c95390e91fb5c0c981ab1b24211)。

**落地限制：**GitLab 老版本可能不支持独立文件线程，会回退到主评论。部分 PR 媒体功能通过用户自己的 GitHub 登录读取附件；应先用非敏感仓库验证权限与反馈去向。

### 5. Chrome DevTools MCP：Agent 工具正在补齐调试工作流，而不只是点击网页

[仓库](https://github.com/ChromeDevTools/chrome-devtools-mcp)｜总 Stars **55,465**；日榜标注 **67 stars today**。

让编码 Agent 通过 MCP 或 CLI 检查真实 Chrome，读取网络、控制台和性能 trace。适合前端开发者及希望让 Agent 带着浏览器证据排查问题的团队。

近 7 日正式版本 **v1.13.0（11 月 11 日）**加入 logpoints、页面范围网络 mock 和上游响应 mock；11 月 13 日又合入在 `get_css_styles` 中内联解析 CSS 自定义变量的提交。后者是主分支变化，尚不能据此声称 npm 正式版本已经提供。[发布说明](https://github.com/ChromeDevTools/chrome-devtools-mcp/releases/tag/chrome-devtools-mcp-v1.13.0)、[最新功能提交](https://github.com/ChromeDevTools/chrome-devtools-mcp/commit/8ba2ac945ebaf1adb92208c29f3a30e73a920956)。

**落地限制：**官方仅保证 Chrome／Chrome for Testing；浏览器内容会暴露给 MCP 客户端，使用统计默认开启。建议隔离浏览器配置，不连接日常敏感登录会话。

### 6. Alibaba Open Code Review：代码审阅开始强调“检查覆盖了什么”

[仓库](https://github.com/alibaba/open-code-review)｜总 Stars **50,645**；日榜标注 **215 stars today**。

读取 Git diff，由可配置模型配合工具访问代码上下文，生成行级评论，也支持整文件扫描。适合把 AI 审阅接入现有开发流程的团队。

11 月 13 日正式发布 **v1.13.9**：在运行覆盖记录中保存被排除文件，零发现的 PR 摘要仍显示警告，并强化逐请求工具声明约束。值得关注的是审阅结果变得更可解释，避免把“没有发现”误读成“所有文件都检查且无问题”，而非宣称漏洞检出率已有突破。[发布说明](https://github.com/alibaba/open-code-review/releases/tag/v1.13.9)。

**落地限制：**README 说明变更文件会发送到配置的 LLM；部署前必须确认代码数据流、模型端点及凭证权限。项目自报的内部使用规模不作为独立效果验证，不能据此替代人工审阅或既有安全扫描。

## 三、跨项目观察与建议

1. **热度与研发进展并不同步。**Sherlock、Tailcat 的上榜没有对应窗口内新版本；应把关注度、维护活动和新增能力分开判断。
2. **AI 开发工具正在围绕人工审阅形成完整流程。**Plannotator 解决反馈组织，DevTools MCP 提供现场调试信息，Open Code Review 说明检查范围。它们改善的是工作流，不代表模型本身更可靠。
3. **能否安全采用取决于细节。**浏览器会话、源代码上传、管理端暴露和中继限制，比总 Stars 更直接影响风险。

**建议深入看两项：**
- **Plannotator**：先用一个非敏感小仓库验证“产物审阅 → 批注 → 正确返回原会话”的闭环，适合多 Agent 协作已产生管理负担的人。
- **Chrome DevTools MCP**：在独立 Chrome for Testing 环境试验网络 mock 与 logpoints，观察是否减少人工复制日志和截图的工作量。

以上仅做公开资料核验，未安装、运行项目或执行效果测试。热度不等于成熟度、安全性或适合生产环境。
