# GitHub 整体与 AI 趋势｜2026-11-25

**先看两项：** Firecrawl 补请求结构校验，App Store Compliance 修复安装器处理用户文件的边界。一个适合转成 API 错误契约，另一个适合转成升级／卸载门禁。

**口径：** 北京时间 11 月 25 日 09:30 读取公开 GitHub Trending 全语言[日榜](https://github.com/trending?since=daily)与[周榜](https://github.com/trending?since=weekly)。总 Star 为快照，今日／本周新增是页面字段，不是自行统计的滚动 24 小时／7 日净增；未列不等于零。[昨天](https://github.com/BruceShenC/News/blob/main/daily/2026-11-24/github-trends.md)七项仍在日榜或周榜，今天换选。App Store Compliance 今日字段 +761→+928，不据此判断原因。Diagram Design 留给独立 Skill 榜。

## 全站热点

1. **[App Store Compliance](https://github.com/mjmirza/app-store-compliance)｜5,872 星，今日 +928，本周未列。** 面向移动端研发与发布 QA 的提交前规则检查。25 日主干[修改 OpenCode 安装器](https://github.com/mjmirza/app-store-compliance/commit/68e75248494df800ec0c121566094f7076410dbf)，区分工具管理的插件与用户自身文件并处理卸载链接，另补 [macOS Bash 3.2](https://github.com/mjmirza/app-store-compliance/commit/92f1dd2d7a87d4d74a05c4304cd37b6ae9a7a07f)兼容。不是审核规则更新、过审承诺或确认已进发布包。
2. **[Univer](https://github.com/dream-num/univer)｜31,694 星，今日未列，本周 +3,278。** 为业务系统嵌入表格、文档和公式计算。24 日主干修复[通配模式字面转义](https://github.com/dream-num/univer/commit/7e2aa438e391d18d27a69952f53e5aceeef26f93)和 [SORT／SORTBY 布尔值等键排序](https://github.com/dream-num/univer/commit/bc5c6e20020bb7958cdc99e1415bd62e4d84d96b)。适合导入、公式与持久化回归；两处修复不代表跨表格产品全面一致。
3. **[Sure](https://github.com/we-promise/sure)｜17,780 星，今日未列，本周 +1,567。** 面向重视自托管的个人财务记录用户。25 日主干收紧[跨域许可](https://github.com/we-promise/sure/commit/9089ccb84d4811e0a2b496e68a43fd2e837c6e94)，启用[内容和权限策略](https://github.com/we-promise/sure/commit/fefce9b496c7be480b5cc4780d17745637f910b1)。部分效果依赖部署配置，升级应以合成数据回归合法登录与账户连接，不假定自托管自动隔离。

## AI 项目

4. **[Firecrawl](https://github.com/firecrawl/firecrawl)｜208,958 星，今日 +991，本周未列。** 把页面转换为 Agent 可读内容，面向资料接入与平台 QA。25 日主干将 [v2 请求校验接到 Schema](https://github.com/firecrawl/firecrawl/commit/eea02a6e8b4fb6dfdebe3cc24b55034a43770403)，并把 [API 类型检查加进 CI](https://github.com/firecrawl/firecrawl/commit/ab1492d7e9694d0aaa769cbdcbd488e3006c04ba)。类型检查不替代运行时；主干不等于托管版已部署。
5. **[Plannotator](https://github.com/backnotprop/plannotator)｜14,896 星，今日未列，本周 +1,956。** AI Coding 的人工评审，可给计划、文档和 diff 定位批注。25 日主干修复[选区无法绘制引起页面失效](https://github.com/backnotprop/plannotator/commit/713446da1f60193d10734a9e4abc4e2625ce354d)，并补[旧高亮清理检查](https://github.com/backnotprop/plannotator/commit/93a7c991b50060338825025acea2188ab4b49e00)。批注可用不代表意见准确或审批完成，团队先核查共享与审计。
6. **[Open Code Review](https://github.com/alibaba/open-code-review)｜56,055 星，今日未列，本周 +2,638。** 结合规则和模型辅助评审，面向研发效能与 QA。25 日主干统一[模型端点配置去空格及校验](https://github.com/alibaba/open-code-review/commit/6e63a26ba6b2c0b928e6772807e86d40f6b80825)，另补越南语。URL 校验不代表连通或流向合规；用模拟端点验各配置入口，不送出真实代码或凭证。

**两项建议：** 插件升级／卸载用隔离仓库验证只动管理范围内的文件，对照用户文件摘要；知识 API 用缺字段、错误类型、分页和上游超时验证状态码、重试语义和追踪标识。**共同趋势：** 配置和安装本身也要验，出错后的输出同样属于产品契约。公开资料核验，未安装或实测；热度不代表成熟度与安全性。
