# GitHub 整体与 AI 趋势｜2026-11-28

北京时间 09:01 查阅 [GitHub Trending 全语言日榜](https://github.com/trending) 的可读页面。周榜原页本次无法直接读取，周增补用 [第三方镜像](https://githubtrending.today/weekly/) 11 月 27 日 15:02 的采集；另注明 GitHub 首页推荐数据。总 Stars 是快照；「今日」「本周」是来源标示，不是本简报测得的滚动净增。

## 整体开源

- **[Docker Compose](https://github.com/docker/compose)**｜约 39.1k 总星；GitHub 首页推荐显示本周 +427，不是 Trending 名次。11 月 27 日 [v5.8.1](https://github.com/docker/compose/releases/tag/v5.8.1) 修正计划停止容器前的 `pre_stop` 钩子执行。适合维护开发环境和 CI 的团队，重点看重建时应用是否收到停止前处理机会，而不只看手动 stop。
- **[NGINX](https://github.com/nginx/nginx)**｜33,659 总星，日榜 +148。反向代理和 Web 服务基础组件；同一项目 11 月 26 日也曾进入本报观察，今日信号是再上榜，不是新发版。平台团队应区分 [mainline 与 stable](https://github.com/nginx/nginx/releases) 维护版本。
- **[Ghost Downloader 3](https://github.com/XiaoYouChR/Ghost-Downloader-3)**｜19,813 总星，日榜 +843，对比昨天页面的 +889，仍在可读日榜上。面向桌面与 Android 多任务下载；[v4.6.1](https://github.com/XiaoYouChR/Ghost-Downloader-3/releases/tag/v4.6.1) 是已报道的修复版，本期只更新热度。两个榜单读数不等于净增差。

## AI 项目

- **[MiniMind](https://github.com/jingyaogong/minimind)**｜72,193 总星，日榜 +667。轻量模型与完整训练脚本，适合训练平台 QA 和教学；可用小样本贯通预训练、SFT、评估与保存恢复。README 的训练耗时不是跨设备承诺；小模型能跑通也不说明大型分布式训练可靠。
- **[AG-UI](https://github.com/ag-ui-protocol/ag-ui)**｜约 21.9k 总星，镜像周榜 +1,269。用带类型的事件串接 Agent 和前端。[11 月 27 日 UTC 发版](https://github.com/ag-ui-protocol/ag-ui/releases/tag/release%2F2026-11-27) 的 Mastra 适配器新增逐次运行指引与能力发现，部分支持版本默认启用；既有 LangGraphAgent 端点不自动改提示词行为。适合多框架 Agent 前端，升级时分别检查事件、状态快照和适配器默认值。
- **[Cursor 官方插件仓库](https://github.com/cursor/plugins)**｜20,001 总星，日榜 +242。每个插件独立目录、用 manifest 声明，适合做团队插件规范与分发的开发工具团队。仓库星数属于整套目录，不代表任一插件的安装量。建议先评审单插件定义与权限，再决定是否引入。
- **[OpenVINO](https://github.com/openvinotoolkit/openvino)**｜12,941 总星，日榜 +152。模型转换及端侧推理工具；[2026.4.1](https://github.com/openvinotoolkit/openvino/releases/tag/2026.4.1) 是此前发布的版本，改进了 ONNX GroupQueryAttention 和低精度算子覆盖，今日没有把上榜当作新发版。适合端侧团队；速度和输出一致性要按模型、精度、设备分别验证。

**值得看：** 一是 AG-UI，先用一条合成事件流验证运行、状态和完成事件能否跨适配器一致呈现。二是 MiniMind，适合把「保存后可恢复且能继续训练」做成小成本门禁。今天的共同点是标准化的接口能减少接线工作，但部署默认值、生命周期和训练产物仍需独立验证；Stars 不等于成熟度、安全性或采用率。
