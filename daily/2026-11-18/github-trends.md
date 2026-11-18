# GitHub 整体与 AI 趋势｜2026-11-18

**观察时间：北京时间11月18日09:26–09:28；结合近7日发布及提交。** 本期精选6项，避开昨日6个项目。榜单初次网页检索失败，随后直接读取公开GitHub Trending页面成功。

总Stars采用该次榜单快照；“日增／周增”为页面标注的stars today／this week，不是自行测得的严格24／168小时净增。日周榜读取有先后，不据一次上榜推断持续增长，也不认定新版本造成涨星。来源：[日榜](https://github.com/trending?since=daily) · [周榜](https://github.com/trending?since=weekly)。

## 一、整体开源项目趋势

### 1. Univer：办公编辑器的基础能力，仍比“接上Agent”更重要

[dream-num/univer](https://github.com/dream-num/univer)｜**总Stars 28,913；日榜新增1,774。**

提供可嵌入产品的表格、文档、公式计算、渲染及插件机制，适合办公应用与数据产品团队。11月16日18:35发布v1.5.2；17日主干继续修复XLOOKUP／XMATCH通配字符处理和混合类型排序，不将这些后续提交算作已发版本。

**价值与限制：**人和Agent可使用同一套Facade API操作内容，但应用仍负责持久化与权限。仓库核心为Apache-2.0；协作、Office文件转换及部分AI扩展另行分发授权，不能把完整演示能力都当成免费开源功能。保存JSON快照不等于支持XLSX导出。

来源：[v1.5.2](https://github.com/dream-num/univer/releases/tag/v1.5.2) · [公式修复](https://github.com/dream-num/univer/commit/47468eaa654573cd608a954ac8e1774884461576) · [开源与商业边界](https://github.com/dream-num/univer#open-source-and-commercial-extensions)。

### 2. Sure：个人财务应用的社区接续，重点看数据完整性

[we-promise/sure](https://github.com/we-promise/sure)｜**总Stars 16,275；周榜新增4,786。**

Maybe Finance停止维护后的社区分支，支持自托管收支、资产与预算管理，亦可选配AI。11月17日15:31发布日期命名的alpha，兼容别名为v0.7.7-alpha.7；更新涉及汇率缺失处理、IBKR费用导入、分页读取及备份恢复保留贷款与信用卡字段。

**价值与限制：**这类应用真正的底座是账目一致性、备份和访问控制，而非自动给出财务结论。AGPL-3.0；本次看到的是alpha，附带移动端debug构建，不能称为生产稳定版本。自托管也不自动保证金融数据安全，真实账户接入前应先验证恢复与数据去向。

来源：[本次alpha发布说明](https://github.com/we-promise/sure/releases/tag/v2026.11.17-alpha) · [项目背景与部署说明](https://github.com/we-promise/sure#sure-the-personal-finance-app-for-everyone)。

### 3. Astryx：把组件、设计规范和CLI作为一套系统交付

[meta/astryx](https://github.com/meta/astryx)｜**总Stars 30,164；周榜新增8,700。**

Meta的可定制设计系统，面向React产品团队，亦为编码Agent提供一致工具。11月14日08:48发布v0.7.16，增加日期／时间输入错误展示时机控制等能力；18日主干仍有实验渲染与交互测试更新。

**价值与限制：**统一组件、主题与工具能减少人和Agent各自猜测设计规则。MIT，要求React 19+；项目明确仍为Beta。无障碍组件声明不能替代真实键盘、读屏与业务页面测试；实验CLI、主干改动与正式包版本要分开评估。

来源：[v0.7.16](https://github.com/meta/astryx/releases/tag/v0.7.16) · [README与运行要求](https://github.com/meta/astryx#overview)。

## 二、AI项目趋势

### 4. LoopForge：将编码Agent串成可重复交付流程

[Tencent/LoopForge](https://github.com/Tencent/LoopForge)｜**总Stars 3,175；日榜新增591。**

自托管工作台，把编码CLI、脚本、人工审批和子流程组织成SOP，适合研发效能与平台团队。11月17日22:30发布v0.4.0，纳入SSH仓库支持、执行器配置隔离、凭证脱敏和工作区编辑等更新。

**价值与限制：**值得借鉴的是阶段、制品与人工门禁，而不是仅增加Agent数量。项目主体MIT、第三方组件保留各自许可；默认服务监听所有网络接口，首个注册账户成为管理员，不能直接无防护暴露公网。模型凭证、自动安装CLI与仓库写入权限须分别审查；有Docker沙箱不等于所有执行路径均已充分隔离。

来源：[v0.4.0](https://github.com/Tencent/LoopForge/releases/tag/v0.4.0) · [SOP与部署说明](https://github.com/Tencent/LoopForge#two-ways-to-work) · [许可证](https://github.com/Tencent/LoopForge/blob/main/LICENSE)。

### 5. Plannotator：多Agent产出之后，还需要统一的人类审阅层

[backnotprop/plannotator](https://github.com/backnotprop/plannotator)｜**总Stars 13,074；日榜新增691；周榜新增2,926。**

本地浏览器界面汇集计划、文档、HTML与代码差异，将批注反馈给Claude Code、Codex等执行端。11月17日14:38发布v0.28.67，加入线程拖拽整理、跨线程提及和Linear悬浮卡片，并修正远端文件打开及配置保存问题。

**价值与限制：**从“给Agent发任务”转向“集中处理等待人判断的产物”，适合同时协调多个任务的研发人员。Apache-2.0；跨机器文件能力要求两端版本匹配。分享链接持有者可读内容，即使大文件上传前加密也不能随意公开链接。安装器会配置hooks与技能，接入前应审阅权限及联网边界。

来源：[v0.28.67](https://github.com/backnotprop/plannotator/releases/tag/v0.28.67) · [审阅工作流](https://github.com/backnotprop/plannotator#plannotator-inbox) · [分享边界](https://github.com/backnotprop/plannotator#sharing)。

### 6. PaperOrchestra：论文写作流水线热门，但研究真实性不能自动委托

[google-research/paper-orchestra](https://github.com/google-research/paper-orchestra)｜**总Stars 3,030；日榜新增1,072。**

以多个专职Agent组织提纲、文献、章节、反思与绘图，把已有实验材料变成LaTeX稿件，适合研究工具开发者。11月16日北京时间主干合入缺失依赖、按实际供应商检查密钥以及LaTeX转义修正；本次未发现正式Release，不能称为今天新发布。

**价值与限制：**适合研究如何组织写作辅助流程，不代表系统完成了科学发现。Apache-2.0；README明确生成数据集尚未随仓库提供，且非Google正式支持产品。引用、实验结果和图表须人工核对；外部模型及检索可能收费，未公开研究材料也不宜直接上传。

来源：[项目说明](https://github.com/google-research/paper-orchestra) · [近期提交](https://github.com/google-research/paper-orchestra/commit/d9bae997e793a015ed38bb1605be9eb58c00284f)。

## 三、跨项目判断与优先阅读

1. **Agent工作流正在补上交付与审阅机制。**LoopForge组织阶段和门禁，Plannotator组织人的判断；两者解决的是模型之外的协作问题。
2. **通用软件仍需关注核心正确性。**Univer的公式与排序、Sure的汇率与恢复，说明“AI化”不能替代传统数据质量保障。
3. **仓库开放不等于完整产品免费或生产可用。**商业扩展、Beta／alpha、未发布数据集与模型服务费用，应与Stars一起看。

优先深入阅读**LoopForge**的SOP节点、人工门禁和失败接续设计，以及**Plannotator**的产物审阅与反馈路径。若后续试用，可用非敏感小仓库比较人工介入次数、返工率、完成耗时和实际费用。本期只是阅读建议，未安装或运行项目。

本期读取公开日／周榜以及6项README、Release和近期提交，不是全网穷尽排名。整体部分按办公、财务和设计基础软件划分，即使提供可选AI接入，也不把它们等同纯Agent框架。热度不等于成熟度、安全性或生产采用率。

## 归档

[本期完整简报](https://github.com/BruceShenC/News/blob/main/daily/2026-11-18/github-trends.md)。正文与主题索引已归档；主题索引同时补齐已核实存在的昨日链接。根README与日期共享索引仍未更新。
