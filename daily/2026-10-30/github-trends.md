**GitHub 整体与 AI 趋势｜2026年10月30日**

精选7项。观察时间：北京时间09:28–09:31；版本、提交日期按来源UTC日期。
已读取[全局日榜](https://github.com/trending?since=daily)、[全局周榜](https://github.com/trending?since=weekly)，以[Go周榜](https://github.com/trending/go?since=weekly)、[Rust周榜](https://github.com/trending/rust?since=weekly)补充非AI与基础设施。**日增/周增均为Trending的today/this week口径，不是快照净增，也不证明持续增长。**语言榜补充有选择偏向，以下不是综合排名。

**一、整体开源项目趋势**

**1．[PHP / php-src](https://github.com/php/php-src)**
总Stars **41,724**；全局周榜 **+960 / this week**。
PHP解释器及扩展生态，适合Web后端、运行时维护者。10月30日main仍在合入PHP-8.6分支的Zend字符串格式处理迁移，这是开发活动，不是8.6正式发布。已读取的最新正式版本是10月21日PHP8.5.12、8.4.27安全版，早于近7日窗口，不能当今日新发布或断定其导致上榜。当前主分支README声明BSD-3-Clause；落地需核对发行分支及扩展兼容性，不直接跟master。
[当前提交](https://github.com/php/php-src/commit/82b01e22298a49033f4c651ed60a73b7bb4986c9) · [8.5.12发布](https://github.com/php/php-src/releases/tag/php-8.5.12)

**2．[Wails](https://github.com/wailsapp/wails)**
总Stars **36,827**；Go周榜 **+188 / this week**。
将Go后端与Web前端打包为原生WebView桌面应用。10月26日v3.0.0正式发布：多窗口、服务生命周期、事件和平台管理器进入明确应用对象模型，静态分析生成JS/TS绑定。适合已有Go服务、想做桌面工具的团队。
10月28日v3.1 nightly是预览版，其资源服务、打包、缓存改进不能混算为v3.0.0稳定能力。MIT；v3要求Go1.26+，仍需验证各平台WebView、窗口及文件操作。
[稳定版](https://github.com/wailsapp/wails/releases/tag/v3.0.0) · [nightly](https://github.com/wailsapp/wails/releases/tag/v3.1.0-nightly.20261028)

**3．[Docker Compose](https://github.com/docker/compose)**
总Stars **38,519**；Go周榜 **+123 / this week**。
用Compose文件管理多容器应用，适合开发环境、集成测试和小规模部署。10月29日main合并配置加载、OCI文件描述生成的去重重构，并启用重复代码检查；最近正式版v5.6.1仍为10月21日，未查到更近正式release。
Apache-2.0；main重构不等于稳定版已包含。v5.6.0 jobs仍仅手动触发，不能宣称支持定时任务；Compose与Swarm语法支持也不完全一致。
[维护提交](https://github.com/docker/compose/commit/ed976f1cd44706721029ef2b9e02371e558b5228) · [v5.6.1](https://github.com/docker/compose/releases/tag/v5.6.1) · [jobs限制](https://github.com/docker/compose/releases/tag/v5.6.0)

**二、AI 项目趋势**

**4．[OpenMausBot](https://github.com/milind-soni/OpenMausBot)**
总Stars **7,496**；全局日榜 **+1,199 / today**。
聊天式桌面界面运行多Agent并接入模型、电脑和应用。10月30日00:37 UTC发布v0.1.134：应用监视条件，事件触发后才唤醒Agent；Mac可保存已完成电脑任务的步骤，下次复用，失败再退回普通Agent执行。适合个人助理编排和重复桌面流程研究。
Apache-2.0、发布密集。“无需AI重放”是项目描述，不是本简报验证过的成功率。电脑控制及连接器权限大，需隔离环境和测试账号；UI变化可令保存步骤失效。
[v0.1.134](https://github.com/milind-soni/OpenMausBot/releases/tag/v0.1.134)

**5．[Claude-Mem](https://github.com/thedotmack/claude-mem)**
总Stars **104,826**；全局日榜 **+336 / today**。
捕获工具使用、压缩语义记忆，为跨会话编码任务补上下文。10月29日v15.6.0把My Work分散状态收敛为统一store，改进URL恢复、实时更新和保存冲突；v15.5.0补敏感字段脱敏。随后main图像记忆提交不能全部算进v15.6.0。
Apache-2.0；核查记忆提供方、云同步和遥测。“本地运行插件”不等于内容只留本地，Grok Bot集成区分默认托管记忆与可选本地observer。
[v15.6.0](https://github.com/thedotmack/claude-mem/releases/tag/v15.6.0) · [v15.5.0](https://github.com/thedotmack/claude-mem/releases/tag/v15.5.0) · [README](https://github.com/thedotmack/claude-mem#quick-start)

**6．[Agentgateway](https://github.com/agentgateway/agentgateway)**
总Stars **5,784**；Rust周榜 **+289 / this week**。
统一代理Agent与模型、MCP工具及其他Agent通信，可独立或Kubernetes部署。10月27日v1.7.0增加模型目录上下文窗口、模态、能力信息；按实际服务档位核算成本，按模型/用户分析token、缓存命中率和延迟；可调用自定义路由服务选择虚拟模型目标。10月29日original model显式表示是后续main开发。
适合平台团队，Apache-2.0。升级默认工具输出检查可能增加拒绝及向外部guardrail传输数据；MCP会话加密、动态后端地址限制、日志库迁移均需评估。
[v1.7.0及升级说明](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0) · [后续提交](https://github.com/agentgateway/agentgateway/commit/1a72e4cc62b6ac3308c7541d03699e453aad2221)

**7．[Uzu](https://github.com/trymirai/uzu)**
总Stars **3,042**；Rust周榜 **+496 / this week**。
将推理嵌入应用，提供Rust、Python、Swift、TypeScript接口，主要面向Apple设备。10月29日22:22 UTC发布0.6.14，release正文为空，进一步核对差异：近7日含WebGPU runtime、decision models、低batch矩阵乘路径、流式reasoning_content读取等提交。
适合端侧AI开发，MIT；使用自有模型格式，Windows/Linux/WASM部分目标仍在进行中。新增WebGPU不等于所有平台生产可用；“零延迟、无推理成本”不能当测量结论。
[0.6.14](https://github.com/trymirai/uzu/releases/tag/0.6.14) · [版本差异](https://github.com/trymirai/uzu/compare/0.6.13...0.6.14) · [平台与格式](https://github.com/trymirai/uzu#development)

**跨项目观察**
• Agent长期运行所需的外围能力受到关注：等待/唤醒、步骤复用、记忆状态、路由和成本治理。
• 本地优先与云端依赖并存，选型需沿真实数据路径核查，不能只看客户端部署位置。
• 热榜与发布不同步：PHP/Compose主要结合维护活动观察；稳定版、nightly、main必须分开判断。

**建议深入看**
**Agentgateway**：小范围验证统一模型/MCP入口、成本维度和升级默认策略变化。
**Uzu**：在Apple测试设备比较同一小模型的内存、首token延迟和集成成本，先确认格式及平台支持。

本期仅公开研究，未安装或执行项目。热度不等于成熟度、安全性或业务收益。

[本期归档](https://github.com/BruceShenC/News/blob/main/daily/2026-10-30/github-trends.md)
