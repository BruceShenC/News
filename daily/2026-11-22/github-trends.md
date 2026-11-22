# GitHub 整体与 AI 趋势｜2026-11-22

**优先看 OpenVINO 和 Sure：**同一模型换一种导出图，运行时可能漏识别长上下文路径；同一条 CI 用例换一次数据库返回顺序或浏览器绘制时机，也可能突然失败。

**口径：**北京时间 09:28 采集 GitHub Trending 全语言[日榜](https://github.com/trending?since=daily)和[周榜](https://github.com/trending?since=weekly)。总 Star 为采集快照，“今日/本周新增”是榜单页面字段，不是自行测量的滚动 24 小时或 7 日净增；未列不等于零。昨日八个项目仍出现在日榜或周榜，本期不重复介绍。参照：[昨日](https://github.com/BruceShenC/News/blob/main/daily/2026-11-21/github-trends.md) Firecrawl 今日字段 +1,241→+1,006，AG-UI 本周字段 +3,621→+3,792，只是页面显示值变化。

## 全站热点

1. **[Astryx](https://github.com/meta/astryx)**｜总 31,817，今日未列，本周 +3,967。Meta 的 React 组件与主题系统，适合维护设计规范和前端质量基线。11 月 22 日[主干修复](https://github.com/meta/astryx/commit/31edb22a1beba3625c1355be1d6e3f579c92d6d7)将截断表头的模块推迟到表格首次绘制后加载，并补浏览器和单元测试。仍为 Beta，要求 React 19+；组件自带的能力不替代业务页面读屏、键盘与焦点验证。
2. **[Sure](https://github.com/we-promise/sure)**｜总 17,150，今日未列，本周 +2,041。可自托管个人财务应用；更值得 QA 看的是 11 月 22 日[仅改测试的提交](https://github.com/we-promise/sure/commit/8d0e4d3597cd8f39bfd2e48442555edf9e4369df)，让四个偶发失败可重复：显式排序数据库结果，把缓存命中语句也计入查询计数，按步推进动画而非等待真实绘制。自托管不意味着全链路离线，评估先用合成数据。

## AI 专项

3. **[OpenVINO](https://github.com/openvinotoolkit/openvino)**｜总 11,938，今日 +200，本周未列。模型转换与多设备推理，适合端侧和推理部署团队。11 月 22 日 [NPU 修复](https://github.com/openvinotoolkit/openvino/commit/6ed280ed683b7db4ac8f47ba75c50c49008c822c)识别缺少某个 Broadcast 节点的 Phi LongRoPE 导出图。提交举例称受影响的 Phi-4-mini 在超过原始位置上限 4,096 后曾输出异常，旧产物需重新编译；另有[独立 GPU 注意力累加修复](https://github.com/openvinotoolkit/openvino/commit/394ac70e9788ca417b46c64b7b4ac89ce0847b60)。不可外推所有硬件或发布包。
4. **[Open Code Review](https://github.com/alibaba/open-code-review)**｜总 55,117，今日未列，本周 +4,032。把确定性检查和 Agent 分析用于 PR 审查，适合工程效能与 QA。11 月 21 日[提交](https://github.com/alibaba/open-code-review/commit/5f4d905f2d5e209aaa9e2f6e5495cc1cb82c0150)补上报告的 token 用量，为按 PR 衡量成本提供入口。宜以脱敏历史 PR 比较规则命中、有效发现和人工复核耗时，不直接把模型评论设为门禁。
5. **[Last30Days](https://github.com/mvanhorn/last30days-skill)**｜总 72,017，今日 +560，本周 +4,198。跨平台资讯检索，适合找线索再回查一手来源。11 月 22 日[主干变更](https://github.com/mvanhorn/last30days-skill/pull/1556)补多种本地 Agent 客户端的简报归档，可从已有本地会话记录回收简报。先核对会话读取范围、保存目录和保留周期；独立 Skill 榜另报详情 API 净增，不与 Trending 字段混用。
6. **[AutoClip](https://github.com/zhouxiaoka/autoclip)**｜总 12,870，今日 +683，本周未列。播客、访谈和课程剪辑的候选片段、字幕、封面和文案。最新可核验[正式版 v1.5.8](https://github.com/zhouxiaoka/autoclip/releases/tag/v1.5.8)于 11 月 2 日发布，11 月 3 日另有[候选版提交](https://github.com/zhouxiaoka/autoclip/commit/cd8e6bed18d87a81273d48bcd165751708ac715c)，不是新正式版。本地渲染不代表上游分析、转写和生图均离线，需复核语境、字幕及素材权利。

## 趋势与建议

- **回归要保存运行条件，不只核对最终输出。** 可优先补一组长上下文跨产物回归：固定权重、Tokenizer 与输入，在上下文边界前后对照参考框架、旧产物和新产物，并记录设备、产物版本、关键位置数值偏差。
- **工具更容易留下成本和执行痕迹，但不自动提升质量。** 若先做低成本改进，选三条常重跑 CI 用例检查无序查询、缓存计数或真实动画时钟，并加入反例确认仍能检出回归。

本期只核验公开榜单和仓库材料，未安装或运行项目。
