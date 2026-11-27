# GitHub 整体与 AI 趋势｜2026-11-27

**今天优先看 Univer 和 ffmpeg-skill。** 前者把 DOM 测试统一到 jsdom；后者把「修一处时检查同类路径」写进 PR 门禁。两项都能借鉴，但门禁跑绿不等于覆盖充分。

榜单口径：北京时间 11 月 27 日 10:37 实采 GitHub Trending 全语言[日榜](https://github.com/trending?since=daily)和[周榜](https://github.com/trending?since=weekly)。总 Stars 是快照；今日／本周增加是榜单字段，不是自行计算的滚动 24 小时／7 天净增；「未列」不等于零。项目更新日期用北京时间。

## 整体开源

1. **[Ghost Downloader 3](https://github.com/XiaoYouChR/Ghost-Downloader-3)｜总 19,221，今日 +889，本周未列。** 跨平台下载与任务管理。[v4.6.1](https://github.com/XiaoYouChR/Ghost-Downloader-3/releases/tag/v4.6.1) 修正桌面偶发崩溃、重启后任务记录丢失及 Linux 休眠问题。桌面应用团队可以借鉴：将活跃、暂停、取消三种状态与休眠、重启交叉验证，不能只测单次下载成功。
2. **[AFFiNE](https://github.com/toeverything/AFFiNE)｜总 79,014，今日 +525，本周未列。** 文档、白板和结构化数据工作区。11 月 26 日主干[修复了白板中键平移后选区丢失](https://github.com/toeverything/AFFiNE/commit/b85148565c9803b683939dff3c5dc437a620505f)。适合做交互回归的团队参考：除视口确实移动外，还要检查下一次修改是否仍落在原选中对象。主干修复不代表发布包已包含。
3. **[Univer](https://github.com/dream-num/univer)｜总 31,979，今日未列，本周 +1,825。** 面向嵌入表格、文档和公式计算的团队。11 月 26 日主干[将 DOM 测试统一到 jsdom](https://github.com/dream-num/univer/commit/3a5d271536bba7f79e85b1f41038e5a21ce8ff48)，并[弃用 `getCellDatas`，推荐 `getCellDataGrid`](https://github.com/dream-num/univer/commit/f8cf6c3c6e576906d3f745d685d7d69b5b86e59a)。弃用不等于移除；建议分别验证旧调用和新接口的数据形状，浏览器渲染、焦点和布局仍用真实浏览器回归。

## AI 项目

4. **[Obsidian Skills](https://github.com/obsidianmd/obsidian-skills)｜总 54,049，今日未列，本周 +704。** 官方维护的技能合集，本期进入所观察的周榜，不是新项目。11 月 27 日 [README 补充 Hermes Marketplace 入口](https://github.com/obsidianmd/obsidian-skills/commit/9c9560a870fd490b0e1a847f2c8401a50e2d931d)，属分发文档变化；适合想用 Agent 处理笔记的人先拿副本验证可读写目录。它是合集，不计入独立 Skill 排名。
5. **[Firecrawl](https://github.com/firecrawl/firecrawl)｜总 211,006，今日 +1,046，本周未列。** 把网页转成 Agent 或检索链路所需的 Markdown、HTML。11 月 27 日主干[移除输出内的 NUL 字符](https://github.com/firecrawl/firecrawl/commit/cb8e1c09106eaa600dafad67806fbde5e0a708a8)。做知识入库可用正常页与含异常字符的合成页做对照，检查内容保真、下游可索引、来源可追溯；尚未验证托管服务是否包含该提交。
6. **[ffmpeg-skill](https://github.com/kajisho5/ffmpeg-skill)｜总 11,558，今日未列，本周 +3,229。** 将本地剪辑与导出包装成 Agent 可调用的流程，面向批量媒体处理。11 月 27 日有 [v4.16.0](https://github.com/kajisho5/ffmpeg-skill/releases/tag/v4.16.0)；另有主干 [PR #755](https://github.com/kajisho5/ffmpeg-skill/pull/755)，把同类路径排查加进修复 PR 的 `Siblings` 检查。这是项目自身的 CI 规则，并非该版本给用户新增的剪辑功能。

**两条趋势：** 开源维护在补状态切换与同类路径的覆盖；验证工具则需要按层选，jsdom 看结构，真浏览器看交互，入库结果看内容是否真可用。**建议先读 Univer 与 ffmpeg-skill：** 前者用于划分测试层次，后者可借来给历史缺陷加「同类入口、执行状态、证据」三列。未安装或运行项目，榜单热度不能代表成熟度。
