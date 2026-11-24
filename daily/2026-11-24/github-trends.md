# GitHub 整体与 AI 趋势｜2026-11-24

**今天优先看三处：** NGINX 修复后端 SSL 路径上的高 CPU，OpenVINO 扩展 GPU FP8 重排支持，Modern Web Guidance 改正一条写反的无障碍规则。

**口径：** 北京时间 11 月 24 日 09:26 读取 GitHub Trending 全语言[日榜](https://github.com/trending?since=daily)与[周榜](https://github.com/trending?since=weekly)。总 Star 是快照，今日／本周新增用页面原字段，不是自行统计的滚动 24 小时／7 日净增；未列不等于零。[昨日](https://github.com/BruceShenC/News/blob/main/daily/2026-11-23/github-trends.md)七项仍上榜，今天换项目；MiniMind 今日字段 +473→+828，Univer 本周 +6,436→+4,324，仅是显示值变化。

## 全站热点

1. **[NGINX](https://github.com/nginx/nginx)｜33,059，今日 +174，本周未列。** 反向代理、负载均衡和网关基础组件，面向网络与推理平台。11 月 23 日 [1.31.10](https://nginx.org/en/CHANGES)修复使用后端 SSL 连接时可能出现的高 CPU。发行版软件包不一定同步；在授权环境对比 worker CPU、尾延迟与流式完成率，不只看 HTTP 成功率。
2. **[Astryx](https://github.com/meta/astryx)｜32,575，今日未列，本周 +3,165。** Meta 的 React 组件与主题系统，面向设计系统和前端 QA。24 日主干[修正下拉与上下文菜单](https://github.com/meta/astryx/commit/61362fc1a0b3c36640aac2df54cd8b42dea15132)，进入子菜单后无障碍树只呈现当前菜单，并更新浏览器测试。Beta，要求 React 19+；还要在业务页面验证退回父级时焦点是否正确。
3. **[Ghost Downloader 3](https://github.com/XiaoYouChR/Ghost-Downloader-3)｜16,343，今日 +1,087，本周未列。** 面向普通文件下载与续传用户。23 日主干[调整 Linux 打包与安装](https://github.com/XiaoYouChR/Ghost-Downloader-3/commit/83aaefb67bc2641b3f0bfff9f3773f219e35963a)，涉及用户目录安装、下载源回退和发布包；不代表现有 Release 资产已替换。桌面 QA 应同时复查升级、卸载和失败后的状态。

## AI 项目

4. **[OpenVINO](https://github.com/openvinotoolkit/openvino)｜12,287，今日 +184，本周未列。** 面向模型转换与部署团队。24 日主干[将 FP8 扩展到 GPU 重排算子](https://github.com/openvinotoolkit/openvino/commit/147c7acd49a7fb457f82b9a6fbfc1703a14f57cd)。不代表全链路 FP8、全设备支持或提速；先核对执行路径，再测误差、吞吐与延迟。
5. **[Chrome DevTools MCP](https://github.com/ChromeDevTools/chrome-devtools-mcp)｜59,033，今日未列，本周 +1,581。** 让编码 Agent 获取浏览器控制台、网络和性能证据，适合前端与 Web QA。24 日主干[加入专用 Web Worker 堆栈解析](https://github.com/ChromeDevTools/chrome-devtools-mcp/commit/976216507ae16c88ae6268cc140ea6ac637ad1cf)，并[修复扩展列表的 JSON 字段](https://github.com/ChromeDevTools/chrome-devtools-mcp/commit/02b146bb562e6d2c454af12e70a2ef7ae2422db1)。Worker 抛普通值仍可能无堆栈；调试接口接触页面数据，宜用隔离浏览器和脱敏账户。
6. **[Modern Web Guidance](https://github.com/GoogleChrome/modern-web-guidance-src)｜10,519，今日未列，本周 +2,694。** 为编码 Agent 提供 Web 指导和评估源码，适合维护前端规则。23 日 [#3684](https://github.com/GoogleChrome/modern-web-guidance-src/pull/3684)修正非模态对话框规则：[WCAG 2.4.11](https://www.w3.org/WAI/WCAG22/Understanding/focus-not-obscured-minimum.html)的最低要求是焦点组件不被作者创建的内容**完全**遮挡，不是任何部分都不能遮挡；规则文本正确不等于实测通过。
7. **[Cursor 官方插件仓库](https://github.com/cursor/plugins)｜18,816，今日 +397，本周未列。** 用插件与清单封装研发流程。24 日主干[pstack reframe](https://github.com/cursor/plugins/commit/9e278fe4d4ecd8bfe98b1b79bf4be662efb338cc)要求先为模糊描述列出两三种可能和区分信号。适合研发效能团队用已标注工单验证候选是否覆盖真因；不能按生成结论直接派责。

**两个趋势与试点：** 一是 AI 指南也要做规格审查与正反例。用键盘验非模态对话框焦点遮挡，再验菜单切换前后的无障碍树。二是新诊断与算子能力要核对实际路径。用同模型同输入对照 FP8 开关，在日志标明内核、不支持和回退，再报误差与尾延迟。

仅核对公开榜单、项目及发版记录，未安装或实测。热度不代表成熟度或安全性。
