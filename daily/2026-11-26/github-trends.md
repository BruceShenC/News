# GitHub 整体与 AI 趋势｜2026-11-26

**今天优先看 Nod 和 OpenSRE。** 前者展示 Agent 的调用轨迹，后者提供了一个值得验证的问题：多文件修改失败时，是否会留下半成品？

**口径：** 北京时间约 09:00 采集 GitHub Trending 全语言[日榜](https://github.com/trending?since=daily)与[周榜](https://github.com/trending?since=weekly)。总 Star 是快照，今日／本周新增照录网页，不是滚动 24 小时／7 天净增；未列不等于零。Nod 昨日周榜未列，今天显示 +1,008，不是今天首发。[昨日简报](https://github.com/BruceShenC/News/blob/main/daily/2026-11-25/github-trends.md)

## 全站热点

1. **[NGINX](https://github.com/nginx/nginx)｜总 33,371，今日 +176，本周未列。** 反向代理与网关基础组件。11 月 26 日主干[调整 OpenSSL QUIC 握手的错误处理](https://github.com/nginx/nginx/commit/6d81cb7c629bd4bb17250d2995e92b2ff309d41b)，后续检查不再覆盖较早的错误。适合网关团队参考：客户端结果、服务端错误分类和归因独立验；主干合入不是已发版。
2. **[Astryx](https://github.com/meta/astryx)｜总 33,132，今日未列，本周 +2,635。** React 组件与主题系统。11 月 26 日主干[修正表格选中计数的无障碍播报](https://github.com/meta/astryx/commit/afa53dafa398f90f7fe1c14bfc3a215838216206)，并[增加对话框内打开菜单的测试](https://github.com/meta/astryx/commit/e148305294ee6a36980a439a2d4e0602a0077e4c)。适合设计系统与前端 QA；仍是 Beta，要求 React 19+。计数正确和读屏适时播报要分开验。

## AI 项目

3. **[Nod](https://github.com/n8n-io/nod)｜总 3,580，今日未列，本周 +1,008。** n8n 的开源个人 Agent，面向需要跨浏览器和办公应用办事、保留人工确认的人，仍是研究预览。[当前发布 v0.9.7](https://github.com/n8n-io/nod/releases/tag/nod-v0.9.7)；11 月 26 日主干另[合入开发模式追踪](https://github.com/n8n-io/nod/pull/1497)，能分轮查看模型、工具与 Token。该追踪不在桌面版或自托管服务器记录，不代表普通用户现在可用。
4. **[OpenSRE](https://github.com/Tracer-Cloud/opensre)｜总 15,518，今日 +348，本周未列。** 面向 DevOps 与平台质量团队，结合 CI 与运行信号辅助排障。11 月 26 日主干[补齐 Agent 的 `apply_patch`](https://github.com/Tracer-Cloud/opensre/commit/41c323cc1807337a56154b8efaa117c5869cd2f5)，设计为整份补丁先校验再应用，目标文件变化时停止。仍是 Public Alpha，确认要求随模式变化；设计承诺不等于验证通过。
5. **[Chrome DevTools MCP](https://github.com/ChromeDevTools/chrome-devtools-mcp)｜总 59,357，今日未列，本周 +1,203。** 为 Web QA 与前端 Agent 提供调试与自动化证据。11 月 26 日主干[改用浏览器原生标签关联](https://github.com/ChromeDevTools/chrome-devtools-mcp/commit/8c43179464d368a6a3c7f8ce2ee024e1e736b795)检查无标签输入框，避免特殊 ID 使扫描中断。只改善一类规则，不替代键盘、焦点与读屏验证。
6. **[OpenVINO](https://github.com/openvinotoolkit/openvino)｜总 12,652，今日 +191，本周未列。** 面向模型转换、推理部署团队。11 月 26 日主干[调整 CI 模型下载与缓存](https://github.com/openvinotoolkit/openvino/commit/cf023984644fda0b564764a71e478666de19ab0a)，代理提供共享缓存，各任务使用各自的本地缓存。这是 CI 依赖链变化，不是推理性能优化。

**两个趋势：** 执行轨迹与实际结果要分开验；单点异常要准确报告，同时不能让后续结果悄悄丢失。优先读 Nod 的轨迹设计，再用隔离仓库验证 OpenSRE：多文件补丁中一处冲突时，既查停止原因，也断言两份文件都没变。仅核对公开资料，未安装或运行。
