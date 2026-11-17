# GitHub 整体与 AI 趋势｜2026-11-17

**观察时间：北京时间11月17日09:30–09:33；结合近7日发布与提交活动。** 本期选6个项目，与昨日6项去重。总Stars固定采用本次Trending页面快照，不用稍后API读数覆盖。

“日榜新增”“周榜新增”是GitHub页面标注的 `stars today`、`stars this week`，不是独立测得的严格24/168小时净增；两榜并非同一瞬间读取。不能把一次上榜解释为持续增长，也不能认定新版本导致涨星。来源：[日榜](https://github.com/trending?since=daily) · [周榜](https://github.com/trending?since=weekly)。

## 一、整体开源项目趋势

### 1. Arcane：Docker管理进入大版本迁移期

[getarcaneapp/arcane](https://github.com/getarcaneapp/arcane)｜**总Stars 8,754；日榜新增46。**

面向自托管和平台运维，集中管理Docker环境、容器、更新与备份。北京时间11月15日发布v3.0.0，16日01:05发布v3.0.1；后者修正agent模式启动、备份及Docker行为，并强化证书与更新校验。17日凌晨主分支仍在修复请求停滞问题，不能把主分支变化都算进3.0.1。

**值得关注：**热度不算最高，但有明确的大版本工程变化。v3调整配置前缀、迁移机制和旧协议，适合研究多环境管理，而不是直接原地升级生产环境。BSD-3-Clause；应先核对迁移与备份恢复。这里的agent是管理代理，不是大模型Agent。

来源：[3.0.1发布说明](https://github.com/getarcaneapp/arcane/releases/tag/v3.0.1) · [3.0.0迁移变化](https://github.com/getarcaneapp/arcane/releases/tag/v3.0.0)。

### 2. Svelte：成熟前端框架的活跃，不必包装成新发布

[sveltejs/svelte](https://github.com/sveltejs/svelte)｜**总Stars 89,008；日榜新增115。**

通过编译声明式组件生成DOM更新代码，适合Web应用开发者。最近可见正式Release仍是北京时间10月27日的5.57.3；近7日主要可见文档及开发指导变化，11月15日合入面向编码Agent的测试指导更新。本次没有核实到过去24小时的新版本。

**值得关注：**成熟框架仍在日榜，但上榜并不代表功能突然跃迁。MIT；选型更应看团队经验、组件生态和迁移成本，不能据这份榜单断言优于其他框架。

来源：[正式发布记录](https://github.com/sveltejs/svelte/releases/tag/svelte%405.57.3) · [近期开发指导更新](https://github.com/sveltejs/svelte/pull/19269)。

### 3. 江山时序：非AI的交互知识工具也有明显关注度

[DeanChensj/jiangshan-map](https://github.com/DeanChensj/jiangshan-map)｜**总Stars 6,777；日榜新增288。**

用时间轴联动历史疆域、地名、事件和人物行迹，适合历史学习与可视化探索。11月13日发布v1.3.0，增加古海岸线、事件沙盘、语音讲解和出处审计；16日仍更新检索与界面。项目报告198幅疆域快照，但这些是示意性近似，不是测绘边界或权威历史结论。

**使用边界：**代码与多数自建数据为GPL-3.0；CHGIS衍生的郡州府图层另有限制，仅学术非商业使用、不得再分发，截图也可能受其条款约束。适合辅助理解，正式教学引用应另核对史料。

来源：[v1.3.0](https://github.com/DeanChensj/jiangshan-map/releases/tag/v1.3.0) · [数据来源与许可](https://github.com/DeanChensj/jiangshan-map#data-sources--license)。

## 二、AI项目趋势

### 4. Stitch Skills：设计工作流的技能合集，不是单一Skill

[google-labs-code/stitch-skills](https://github.com/google-labs-code/stitch-skills)｜**总Stars 9,741；日榜新增529。**

将设计生成、设计系统提取、静态HTML提取和组件构建分成设计、构建、工具三个插件，适合前端与设计工程团队。此次上榜，但最近正式Release为5月19日北京时间的v1.0，主分支最新提交为8月18日北京时间；没有证据把今天上涨归因于新发布。

**使用边界：**Apache-2.0；依赖Stitch MCP及相应服务配置，单独安装部分技能可能缺依赖。README明确说明并非Google正式支持产品。上传代码或设计到服务前需检查保密要求；兼容Agent Skills规范也不保证各宿主无需适配。本仓库是合集，不应进入“独立单Skill”榜。

来源：[README及兼容性说明](https://github.com/google-labs-code/stitch-skills#prerequisites) · [v1.0发布](https://github.com/google-labs-code/stitch-skills/releases/tag/v1.0)。

### 5. NVIDIA SkillEvaluator：从“有没有Skill”走向“是否真的有增益”

[NVIDIA/SkillEvaluator](https://github.com/NVIDIA/SkillEvaluator)｜**总Stars 2,720；日榜新增873。**

提供静态质量与安全检查、语义重叠检测、带/不带Skill的Agent任务对照。11月13日发布v0.9.0，加入版本比较、可复用基线、成本预算及更明确的证据记录；17日上午主分支继续补充宿主加载兼容性检查的说明。它是评估框架，不是单个可直接分发的业务Skill。

**值得关注：**对维护内部技能库的QA/研发团队，比单看Stars更有价值的是无Skill基线、成本和任务结果是否同时可比。Apache-2.0，项目支持级别为Experimental、无企业支持SLA。静态通过不代表安全或业务有效；完整评估依赖扫描器、模型及隔离运行环境，可能产生费用，自动生成的测试集也需人工复核。

来源：[v0.9.0](https://github.com/NVIDIA/SkillEvaluator/releases/tag/v0.9.0) · [分层评估与支持限制](https://github.com/NVIDIA/SkillEvaluator#three-tier-overview)。

### 6. OpenCodeReview：专用审查流程与通用Agent分工

[alibaba/open-code-review](https://github.com/alibaba/open-code-review)｜**总Stars 52,953；日榜新增951；周榜新增3,056。**

结合确定性文件筛选、规则匹配、评论定位与LLM上下文检索，输出行级审查意见，适合开发平台及代码审查团队。11月15日发布v1.13.10，增强工作区审查恢复和比较侧代码访问；16日主分支继续处理会话路径、多字节路径与MCP配置问题。

**使用边界：**Apache-2.0。项目自报基准强调精确率、承认召回率低于通用Agent，不能当作所有缺陷均能发现。默认模式使用OCR自己的模型配置，不自动继承宿主订阅；delegation模式才使用宿主模型。源码会进入所配置模型的上下文，采用前应明确数据去向、费用和权限。本文不重复展开缺陷回归案例。

来源：[v1.13.10](https://github.com/alibaba/open-code-review/releases/tag/v1.13.10) · [架构、基准及模式说明](https://github.com/alibaba/open-code-review#what-is-open-code-review)。

## 三、跨项目观察

1. **Skill生态正在形成生产、分发和评估链条。**Stitch Skills提供工作流，SkillEvaluator衡量增益；标准格式只是接入起点，宿主兼容、依赖和任务效果仍需单独验证。
2. **专用工具把确定性流程留在工程层。**OpenCodeReview的文件筛选与定位体现这一方向；不必把每一步都交给模型决策。
3. **热度与发布节奏不一致。**Stitch Skills没有近期发布仍涨星，Arcane则有明确迁移成本。非AI应用和成熟框架也应保留在整体视野，而非把GitHub趋势简化成AI榜。

**优先深入阅读：**SkillEvaluator的无Skill基线与版本比较设计；OpenCodeReview的确定性流程和两种模型调用模式。若以后小范围试用，先用非敏感样本和明确预算，分别衡量有效发现、误报、漏报与运行成本。这是建议，本次未安装或运行项目。

本期读取了公开日/周榜、6个仓库README、Release与近期提交；不是全网穷尽排名。所有项目本次均未归档，但活跃提交和高热度均不等于成熟、安全或适合生产。

## 归档

[本期完整简报](https://github.com/BruceShenC/News/blob/main/daily/2026-11-17/github-trends.md)。主题与共享索引本期未更新。
