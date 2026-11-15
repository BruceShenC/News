# GitHub 整体与 AI 趋势｜2026-11-15

观察截止：北京时间 **2026 年 11 月 15 日 09:27**，优先查看此前 24 小时，结合近 7 日发布与维护活动。

**数据口径：**已读取 GitHub Trending [日榜](https://github.com/trending?since=daily)与[周榜](https://github.com/trending?since=weekly)，并核对下列仓库 README、release 和提交活动。抓取请求要求实时获取（`maxAge=0`），返回匿名页面及 HTTP 200；仍不保证 GitHub 榜单内部无更新延迟。下面“今日新增”“本周新增”均是页面原标注，不是自行测得的严格 24 小时／168 小时净增；另一周期未取得时不补零。这是可见榜单中的六项精选，不是全网排名，六项均未重复昨日简报的项目。

## 一、整体开源项目趋势

### 1. Sure：个人财务软件的社区接续，开始补齐数据治理

[仓库](https://github.com/we-promise/sure)｜日榜总 Stars **15,324**，标注 **659 stars today**。

Sure 是已停止维护的 Maybe Finance 的社区分支，可自托管，覆盖个人财务管理，支持浏览器及其他客户端，AI 为可选能力。适合希望掌握财务数据、且愿意承担运维责任的个人与开发者。

**近期变化：**11 月 14 日发布 **v2026.11.14-alpha / v0.7.7-alpha.6 预发布版**，加入敏感账户操作审计日志、手动交易对账、按账户／币种／类别展示资产分配等改动。11 月 15 日主分支又出现保留贷款条款和信用卡详情的备份恢复修正，不能据此认为它已进入上述 alpha 版本。[预发布说明](https://github.com/we-promise/sure/releases/tag/v2026.11.14-alpha)、[后续提交](https://github.com/we-promise/sure/commit/b10eba23352c7448ac2eec86c4c575119934d810)。

**限制：**AGPLv3；不要把 alpha 当稳定版。财务数据敏感，先用脱敏样本验证导入、金额符号和备份恢复，再考虑接入真实数据；自托管不等于自动安全。

### 2. Univer：办公 SDK 的进展集中在可编辑性和文件往返兼容

[仓库](https://github.com/dream-num/univer)｜日榜总 Stars **24,511**，标注 **495 stars today**。

提供可嵌入产品的表格和文档能力，而不只是一个独立在线办公应用，适合需要在业务系统内提供编辑、计算和文档处理的团队。

**过去 24 小时正式发布：**11 月 14 日 **v1.5.0** 增加独立图表工作表、堆叠折线图和误差线；降低公式计算内存，并改善 Office 文件兼容。值得关注的是“导入后仍可编辑、导出时保留结构”，不仅是生成一张看起来正确的截图。[发布说明](https://github.com/dream-num/univer/releases/tag/v1.5.0)。

**限制：**仓库开源包使用 Apache-2.0，商业 SDK 模块另行分发和授权。发布说明中的 Board、Slides、原生 Exchange 等能力不能一概视为免费开源核心；部分 Office 转换需要匹配的原生组件。选型应按实际文档做导入、编辑、导出往返验证。

### 3. MuJoCo：物理模拟基础设施上榜，版本选择比新功能更重要

[仓库](https://github.com/google-deepmind/mujoco)｜周榜总 Stars **17,984**，标注 **1,859 stars this week**。

通用物理模拟器，服务机器人、控制与强化学习研究，也用于非 AI 的动力学仿真。适合需要可重复物理环境的研发人员。

**近 7 日正式进展：**11 月 11 日 **4.0.1** 为 Windows 提供 Studio Web Viewer，同时修复 4.0.0 引入的稀疏分解问题；官方说明该问题会造成错误加速度、收敛变慢或发散。11 月 13 日主分支仍有发布工作流维护。这里的关注点是工具生态与升级取舍，不将其包装为今天的新模型突破。[4.0.1 发布说明](https://github.com/google-deepmind/mujoco/releases/tag/4.0.1)、[维护提交](https://github.com/google-deepmind/mujoco/commit/7252110fb3060fee5fbef9f341f36341e6b02a87)。

**限制：**主分支可能不稳定，官方推荐版本化二进制；已有仿真模型迁移时应保留版本与基准轨迹，不能假设升级后物理行为完全一致。

## 二、AI 项目趋势

### 4. Qwen Code：编码 Agent 的竞争延伸到会话、渠道和权限管理

[仓库](https://github.com/QwenLM/qwen-code)｜日榜总 Stars **30,358**，标注 **64 stars today**。

从终端编码助手扩展至编辑器、桌面、Web 和消息渠道，支持多种模型 API、本地模型、Skills、MCP 与子 Agent。适合希望在不同入口复用编码工作流的开发团队。

**过去 24 小时变化：**11 月 14 日发布的 **v0.27.2-nightly.20261113.6f754be654 是预发布版**，包括托管会话接入客户端文件系统、长渠道回复分段交付、飞书自定义机器人 webhook 交付及权限相关调整。版本名含 `20261113`，不等于实际发布日期就是 13 日。主分支随后仍有使用量面板回放优化。[nightly 发布说明](https://github.com/QwenLM/qwen-code/releases/tag/v0.27.2-nightly.20261113.6f754be654)、[后续提交](https://github.com/QwenLM/qwen-code/commit/90be7c869f668789a05431b214d6491b424e2483)。

**限制：**nightly、1.0 Preview 和主分支不是同一稳定能力集合。README 明确部分团队协作、Web／daemon 功能仍属实验性；daemon 默认监听本机但不启用 bearer 认证，同机进程可调用包括代码执行在内的 API，共享主机尤其需要重新审视权限边界。

### 5. AG-UI：Agent 与前端之间，需要的不只是 token 流

[仓库](https://github.com/ag-ui-protocol/ag-ui)｜日榜总 Stars **17,380**，标注 **9 stars today**。

用事件协议连接 Agent 后端与用户界面，支持流式消息、状态同步、前端工具和人工参与。适合已有 Agent 服务、需要接入业务前端的团队；它补充 MCP 和 Agent 间通信，而不是替代它们。

**近 7 日正式进展：**11 月 13 日发布的 `@ag-ui/langgraph 0.0.46` 读取 LangGraph 原生中断响应 schema，处理序列化工具结果，并在运行开始事件声明协议版本。11 月 14 日最新可见提交只是增加 Trending 徽章，不应据此宣称新增技术能力。[发布说明](https://github.com/ag-ui-protocol/ag-ui/releases/tag/release%2F2026-11-13)、[提交记录](https://github.com/ag-ui-protocol/ag-ui/commits/main/)。

**限制：**MIT；此次包要求 `@ag-ui/client` 等相关依赖至少为 1.0.0。各集成成熟度并不一致，README 仍将部分适配器列为开发中。上榜但“今日新增”只有 9，也提醒我们榜单位置不能直接当作增长速度排名。

### 6. LoopForge：把编码 Agent 包装成可追踪、可审批的交付流程

[仓库](https://github.com/Tencent/LoopForge)｜日榜总 Stars **921**，标注 **33 stars today**。

自托管工作台，把编码 CLI、脚本、人工审批节点和 Docker 工作空间串成 SOP，并保存日志与产物。适合已经使用多个编码 Agent、但缺少统一交付过程的团队。

**近 7 日进展：**11 月 13 日发布 **v0.3.9-rc.1 候选版本**，包含 SSH 仓库接入、运行期间编辑工作空间、更多执行器集成，以及凭证处理改进。11 月 14 日主分支继续修正慢心跳下运行状态与工作空间租约的处理；这是后续提交，不等于已进入候选版。[候选版说明](https://github.com/Tencent/LoopForge/releases/tag/v0.3.9-rc.1)、[后续提交](https://github.com/Tencent/LoopForge/commit/dd56585276d8ecb6e0d6865f55e255194d75f85a)。

**限制：**MIT，但还在快速迭代。README 提醒服务默认监听 `0.0.0.0:8090`，第一个创建的账户成为管理员；初始化期间必须限制网络访问。多个执行器、模型凭证和 Git 权限叠加，不应把“有 Docker 沙箱”直接等同于安全隔离已充分验证。

## 三、跨项目观察与建议

1. **开源热度并不只围绕模型。**财务应用、办公 SDK、物理模拟器仍有明确需求；可维护性、数据迁移与兼容性比总 Stars 更影响实际采用。
2. **Agent 工具的重心继续向工程协作移动。**Qwen Code 扩展入口和会话，AG-UI 统一前后端事件，LoopForge 组织交付过程。价值在减少接入和协调成本，不能直接推导出模型更聪明。
3. **发布渠道与主分支必须分开看。**本期同时存在正式版、alpha、nightly、RC 及发布后的提交；“仓库今天有更新”不代表用户正在使用的版本已经具备该能力。

**值得小范围验证的两项：**
- **AG-UI：**用一个非敏感 Agent 验证流式消息、一次工具调用和一次人工中断，再检查断线后状态是否一致。适合评估业务前端接入成本。
- **LoopForge：**先使用其不需要模型密钥的“脚本＋人工审批”示例，观察产物记录、退回与重试流程是否符合团队习惯，再评估接入真实仓库。

以上只做公开资料核验，未登录、安装、运行项目或执行效果测试。热度不等于成熟度、安全性，也不能据一次榜单快照判断持续增长。

## 归档

[本期完整简报](https://github.com/BruceShenC/News/blob/main/daily/2026-11-15/github-trends.md)。
根 README 与 daily/README 共享索引本期未更新，仍等待此前写入限制的确认；正文及主题索引独立归档。
