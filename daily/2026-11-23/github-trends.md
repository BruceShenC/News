# GitHub 整体与 AI 趋势｜2026-11-23

**今天优先看两个质量问题：** App Store Compliance 修复了把正常词误判为违规声明的规则，适合参考发布门禁怎么测误拦截；OpenSRE 给外部写操作补上确认，适合参考 Agent 工具权限怎么测副作用。

**榜单口径：** 北京时间 11 月 23 日 09:27 采集公开 GitHub Trending 全语言[日榜](https://github.com/trending?since=daily)及[周榜](https://github.com/trending?since=weekly)。“今日／本周新增”是榜单页面字段，不是自行计算的滚动 24 小时或 7 天净增；“未列”不等于零。版本与提交日期按北京时间。

**与[昨天](https://github.com/BruceShenC/News/blob/main/daily/2026-11-22/github-trends.md)相比：** 昨日六项仍出现在今天日榜或周榜，本期不重复介绍。Last30Days 本周字段从 +4,198 到 +4,731，AutoClip 今日字段从 +683 到 +641，仅代表页面显示值变化。

## 全站热点

1. **[App Store Compliance](https://github.com/mjmirza/app-store-compliance)｜总 4,000，今日 +621，本周未列。** 移动端提交前检查，适合研发、测试和发布团队。11 月 22 日的 [v2.5.1](https://github.com/mjmirza/app-store-compliance/releases/tag/v2.5.1)补充审核声明与 Windows 兼容性；之后主干[修复规则误报](https://github.com/mjmirza/app-store-compliance/commit/ad76746e6304a7f623ecd50dbf9da91117a7d6f7)，例如不再把 Freeform 判成免费价格声明。后续修复不等于已随该版本发布，第三方规则也不代表官方审核结论。
2. **[AFFiNE](https://github.com/toeverything/AFFiNE)｜总 76,838，今日 +542，本周未列。** 文档、白板和结构化数据协作工作区。11 月 22 日主干[改善同步与恢复](https://github.com/toeverything/AFFiNE/commit/3d2572707628be4e7e71e4dee78ea1f79c4f10c3)，涉及修复不完整注册和检测版本冲突。适合知识管理团队验证断网编辑后的冲突可见性；本地优先不等于可选 AI 功能也走本地。
3. **[Univer](https://github.com/dream-num/univer)｜总 31,272，今日未列，本周 +6,436。** 面向在业务系统中嵌入表格、文档与公式计算的工程团队。11 月 22 日主干[修复非整数滚动位置造成的残影](https://github.com/dream-num/univer/commit/926e17847916b3466997c325cba80e1c8ca380ed)，适合转成不同缩放与像素比下的视觉回归。公开 Web SDK 核心为 Apache-2.0，部分扩展另行授权。

## AI 项目

4. **[OpenSRE](https://github.com/Tracer-Cloud/opensre)｜总 14,433，今日 +399，本周未列。** 从 CI 失败与运行信号辅助诊断，面向平台测试、DevOps 和 SRE。11 月 22 日主干[为工具声明副作用，并在非最高自动化档给外部写操作加确认](https://github.com/Tracer-Cloud/opensre/commit/34743ae0a6537b42610efcc315b826ae17617d6d)，也[补了 MCP 服务管理](https://github.com/Tracer-Cloud/opensre/commit/5cda5a6de392d061d51814e56003c4dc8022e7da)。仍是 Public Alpha；最高档不受同一确认规则约束。
5. **[MiniMind](https://github.com/jingyaogong/minimind)｜总 68,677，今日 +473，本周未列。** 用轻量模型演示预训练、微调和对齐。11 月 22 日主干新增[学习率预热比例](https://github.com/jingyaogong/minimind/commit/b16d0b1dc940bf7882d56b127a1202eccb72d9c3)，并修复[CPU／Windows 兼容性](https://github.com/jingyaogong/minimind/commit/53975ab446733dd52d7244bbb34d8e65fba87155)。适合学习训练链路的工程师。单卡 SFT 成本示例不能外推到完整训练。
6. **[ffmpeg-skill](https://github.com/kajisho5/ffmpeg-skill)｜总 9,734，今日未列，本周 +3,748。** 面向媒体自动化团队，把 FFmpeg 剪辑、音频和导出包装成 Agent 工具。11 月 22 日发布 [v4.7.0](https://github.com/kajisho5/ffmpeg-skill/releases/tag/v4.7.0)，[支持命名和标语言的多音轨](https://github.com/kajisho5/ffmpeg-skill/commit/7b3429eb41de529c13ada8fb181e902085e28bbb)。本地工具不等于配套模型离线；验收应看目标播放器能否正确选择默认轨与语言轨。
7. **[Stitch Skills](https://github.com/google-labs-code/stitch-skills)｜总 11,930，今日 +494，本周 +2,683。** 面向设计与前端交接团队，复用设计生成、规范提取和实现流程。11 月 21 日主干[识别 CSS-first 的 Tailwind 设置](https://github.com/google-labs-code/stitch-skills/commit/62fdcd679747aca0792e5dd6591b10ddfad56825)，并[检查 React Native 不支持的 Web 标签](https://github.com/google-labs-code/stitch-skills/commit/9ba9f9edc85e145f57824bf237ea4b4b1c46ef21)。依赖 Stitch MCP，仓库不属于 Google 官方支持产品，静态检查不能替代真机。

## 值得试的两项

- **App Store Compliance：** 发布门禁用例成对设计，给每条规则配一个该拦截、一个相似但不该拦截的样本，再补大小写和环境变体。先旁路记录误拦截与漏检，再决定是否作为门禁。
- **OpenSRE：** 在工具替身上验权限副作用。拒绝后必须零写入，批准后只写一次，取消重试不能重复写，日志应能解释发生了什么。

本期为公开榜单与项目一手材料核验，未安装、运行项目。热度不代表成熟度、安全性或实际效果。
