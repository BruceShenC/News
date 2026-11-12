**GitHub 整体与 AI 趋势｜2026-11-12**

本期精选6项：通用开发工具3项，AI应用与运行工具3项。值得关注的是编辑器与设计系统继续补齐日常工作流，而Agent产品开始更明确地区分并行执行、会话恢复、权限和数据边界。

**观察窗口与统计口径**

北京时间11月12日约09:27至09:29读取公开[日榜](https://github.com/trending?since=daily)、[周榜](https://github.com/trending?since=weekly)，并核对仓库README、release和默认分支提交。结合近24小时和11月5日至12日的近7日背景。

总Stars为榜面存量；stars today／stars this week是GitHub榜面原始口径，**不是自行保存快照计算的精确24小时／7日净增**。未取得的另一周期数据不补零，也不从一次快照推断持续增长。下文发布与提交日期均为UTC。

**一、整体开源项目趋势**

**1. [Zed](https://github.com/zed-industries/zed)：编辑器升级仍然围绕Git、远程开发和可控的Agent接入**

榜面：**总Stars 94,718；2,310 stars this week**。

Zed是面向macOS、Linux和Windows的高性能协作代码编辑器，核心编辑、Git和远程开发不依赖AI，因此归入通用工具。

11月11日19:08正式发布的[v1.28.1](https://github.com/zed-industries/zed/releases/tag/v1.28.1)加入可选自动Git fetch，默认关闭；改进Git Graph、工作区选择和终端操作。Agent方面改进ACP兼容、权限请求取消，以及超长文件行的截断，避免上下文被单行内容耗尽。语言服务器和Agent下载也补齐校验和验证、HTTP失败拒绝及部分下载清理。

同日19:20的[v1.29.1-pre](https://github.com/zed-industries/zed/releases/tag/v1.29.1-pre)是**预发布**，包含对基准分支查看全部变更的Git Panel模式，不能混写为稳定版功能。11月12日最新[主分支提交](https://github.com/zed-industries/zed/commit/3e23a0c48201a9cc34ad4a3a4225910247de41f4)又调整文件查找器默认设置，也应与已发布版本区分。

适合需要统一编辑、远程开发和多种编码Agent入口的研发团队。源码主要采用GPL-3.0-or-later，部分组件为Apache-2.0。建议先验证现有SSH、语言服务器和Agent配置；更新编辑器不等于所有外接Agent行为已被验证。

**2. [Astryx](https://github.com/meta/astryx)：设计系统的热点延续，更新重点是复杂交互的一致性**

榜面：**总Stars 25,037；2,776 stars today**。

Meta的React 19+设计系统，提供组件、主题、模板和CLI。虽然面向人和Agent共同使用，组件本身并不依赖模型，因此不重复计入AI部分。

11月11日19:54正式发布的[v0.7.14](https://github.com/meta/astryx/releases/tag/v0.7.14)为禁用的ComplexSelector增加原因提示与可访问性描述；让浮层动画遵循实际展开方向；改进Tokenizer与InputGroup的标签、状态和键盘交互组合。这些变化更接近真实业务界面需要的行为细节，而不是增加一批静态样式。

需注意部分触屏菜单默认从长按改为按下打开，旧交互要显式配置才能保留。最新[主分支提交](https://github.com/meta/astryx/commit/3406f73838e926fdeb7bc088b831deaab84dcb31)优化测试打包与分片，不应包装为新组件能力。

MIT许可，但项目整体仍标注Beta；lab实验组件未发布到npm，图表相关包仅有canary版本。适合React团队小范围试验，重点核对触屏、键盘、读屏与定制组件组合。此前已介绍过Astryx，本期因新正式版本跟进，不称首次发现。

**3. [The Algorithms — Java](https://github.com/TheAlgorithms/Java)：传统学习型项目仍占有日榜位置**

榜面：**总Stars 67,036；51 stars today**。

这是Java算法实现集合，适合学习数据结构、算法和阅读不同实现。README明确说明用途是教育，性能可能不如Java标准库，不能把榜单热度当作生产依赖推荐。

仓库[release列表](https://github.com/TheAlgorithms/Java/releases)没有正式发布条目。11月11日21:54最新可见[提交](https://github.com/TheAlgorithms/Java/commit/c46808958406c3b74726542fc9ad7e8038fe924e)更新GitHub Actions的Java环境配置，属于维护，不是新算法发布。

它的价值在于提供可阅读、可比较的实现样本。适合开发者培训或设计算法边界测试，但直接复制到生产前仍需检查复杂度、溢出、输入约束和标准库替代方案。本期将其作为非AI开源生态观察，而非技术突破。

**二、AI项目趋势**

**4. [OpenMausBot](https://github.com/milind-soni/OpenMausBot)：个人Agent应用加快迭代，权限和数据处理值得先读**

榜面：**总Stars 21,250；9,257 stars this week**。

它把多个Agent组织成聊天应用，接入本地Claude、Codex等执行引擎，并提供计算机、连接器和远程访问功能。适合愿意自行管理运行环境的个人Agent开发者；它是独立项目，不能把README中的竞品比较当作已核实事实。

近24小时内有两次正式发布：

- 11月11日13:24的[v0.1.153](https://github.com/milind-soni/OpenMausBot/releases/tag/v0.1.153)加入Burst并行任务拆分、Ollama发现，以及部分模型推理级别选择。
- 11月12日00:19的[v0.1.154](https://github.com/milind-soni/OpenMausBot/releases/tag/v0.1.154)重组设置、加入流式回复和网站发布入口，并改进自托管升级的备份、失败回退及连接恢复后的停止操作。

**重要限制：** 发布说明明确指出流式文本按生成内容直接显示，保存后才按原逻辑遮蔽已保存秘密。不能假设存储阶段的脱敏同时保护了实时显示。README还区分桌面包与CLI的凭据存储方式：部分CLI配置以明文保存；本地优先也不代表模型和连接器不向外部传输数据。

核心为Apache-2.0，enterprise目录另有source-available许可。建议仅用无敏感内容的测试工作区评估并行任务、停止、断线恢复及权限提示，暂不连接真实生产账户或高影响设备。

**5. [Codex Security](https://github.com/openai/codex-security)：安全工具向扫描、证据与既有扫描器整合延伸**

榜面：**总Stars 11,760；52 stars today**。

项目提供CLI与TypeScript SDK，用于发现、调查、验证和修复代码漏洞，也支持GitHub Actions、保存扫描历史及导出报告。适合需要把AI审查接入现有安全流程的研发与安全团队。

近7日正式版为11月10日01:09发布的[npm-v0.9.1](https://github.com/openai/codex-security/releases/tag/npm-v0.9.1)。Deep Scan将独立Standard审查分批执行，允许工作者共享线索、交叉核验证据，再按共同修复点整理结果；外部扫描器导入默认开启，GitHub Action汇总也会显示扫描范围限制，即使结果为零发现。

11月12日01:08主分支出现[准备0.10.0的提交](https://github.com/openai/codex-security/commit/f465432bc4c8882a8b667985a9f68045483ed796)，本次未核实对应正式release，**不写成0.10.0已经发布**。

升级时需重新检查Deep Scan停止条件，旧的外部导入关闭环境变量不再有效。开源客户端不代表扫描免费；README中的费用上限是估算，进行中的请求可能超限。“零发现”也不代表完整覆盖或没有漏洞。本期聚焦工具生态，不重复展开单个漏洞复现。

**6. [herdr](https://github.com/herdrdev/herdr)：多Agent长期工作需要会话运行层，但恢复不等于进程幸存**

榜面：**总Stars 50,344；2,213 stars this week**。

herdr是Rust实现的终端运行工具，管理Claude Code、Codex等既有Agent的终端，不替代模型或Agent本身。支持后台会话、多机器连接、工作中／阻塞／空闲状态以及CLI和socket接口。

本期依据是周榜关注，而不是新发布：最新正式版[v0.9.5](https://github.com/herdrdev/herdr/releases/tag/v0.9.5)发布于10月14日，最新可见[默认分支提交](https://github.com/herdrdev/herdr/commit/ee5bfb5041bb3e4a49d591c72308aae3c81ecdde)为10月29日，均早于近7日窗口。不能把Star增长写成维护或功能更新加速。

README对恢复边界说明得很清楚：关闭客户端或SSH断连时，后台服务可继续运行终端；**服务器或机器重启后，原进程不会幸存**，恢复的是布局及受支持Agent会话。适合需要管理多个本地、远程编码会话的人。

Apache-2.0。建议先测试客户端断开、后台服务重启和机器重启三种不同故障，并检查哪些Agent能够恢复、哪些任务必须重跑。终端管理不等于文件系统、网络或凭据隔离。

**三、跨项目观察**

1. **Agent入口正在融入既有工作流。** 编辑器、终端运行层和聊天应用都在承接Agent，选择时应看状态可见性、停止与恢复，不只看模型数量。
2. **发布状态比版本数字更重要。** Zed同时发布稳定版和预览版，Codex Security只有下一版准备提交，herdr仍因周榜受关注。三者不能统称“今日升级”。
3. **便利功能带来新的数据边界。** 流式回复、外部扫描器导入、远程会话和网站发布都需明确数据去向及权限；本地部署和开源许可不自动消除风险。

**建议优先深入两项**

- **Zed v1.28.1：**以已有项目检查Git、SSH和外接Agent工作流，适合判断开发工具整合是否减少切换成本。
- **herdr：**用可丢弃任务测试断连、服务重启、机器重启后的行为，先确认恢复边界，再用于长时间编码任务。

**去重与局限**

已读取[11月11日完整简报](https://github.com/BruceShenC/News/blob/main/daily/2026-11-11/github-trends.md)，本期不重复展开Topcoat、Sure、Auto Ontology、agent-desktop、PR-Agent与Hex。Astryx仅跟进新正式版；独立Skill与Skills合集留给对应简报。

本期只阅读公开榜单、README、release和提交，没有登录、安装、运行或自行复测。以上是所查样本的编辑精选，不是GitHub全网穷尽排名。Stars不等于使用量、成熟度、安全性或性能突破。

归档正文：[2026-11-12 GitHub 趋势简报](https://github.com/BruceShenC/News/blob/main/daily/2026-11-12/github-trends.md)
