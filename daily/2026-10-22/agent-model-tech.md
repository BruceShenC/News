# 10月22日 · Agent 与大模型技术热点

日期：2026-10-22（Asia/Shanghai，北京时间 UTC+8）

归档说明：本文保留 2026-10-22 当期调研内容、来源和统计口径，不代表当前状态。仅将个性化称谓改为通用表述；未重新研究或执行项目。

今天更值得看的是检索、KV缓存、执行框架与权限边界，而非单纯模型榜分。精选6条：前三项为10月21日官方发布／解读，后三项补充近7日研究。以下效果数字来自作者或厂商实验，不代表通用收益。

1. Nemotron 3.5 Retriever：检索质量开始与Agent总token成本一起评估
10月21日正式发布Embed、Rerank、Parse，通过多模态检索与交叉编码器重排，让表格、图表等证据更早进入上下文。固定Agent模型和执行设置后，官方报告加入reranker可减少约13%–14%的推理token。
价值：适合相关知识网关，检索不只是“找得准”，还可能减少反复搜索。
限制：token节省不等于总账单下降，须计入检索、重排开销；OpenSearch对照为AWS／NVIDIA联合实验，非独立复现。[官方发布](https://huggingface.co/blog/nvidia/nemotron-retriever-world-class-agentic-retrieval) · [对照实验](https://opensearch.org/blog/nvidia-nemotron-embeddings-for-agentic-search-in-opensearch/)

2. OpenShell新参考架构与Sentry：将安全策略放到Agent无法修改的位置
10月21日宣布新参考架构已提供，并介绍Sentry。OpenShell在沙箱外执行策略；Sentry利用BlueField-4独立信任域，结合网络策略、OPA／Rego及主机内存旁路观测约束运行。
价值：长程、多Agent任务的权限边界不应只依赖提示词。
限制：不是OpenShell首次发布，也不意味着全部组件和合作集成都已全面上线；依赖特定硬件、流量路径与正确策略，不能视为阻断所有提示注入的证明。[架构与可用状态](https://nvidianews.nvidia.com/news/openshell-reference-architecture) · [技术细节](https://developer.nvidia.com/blog/inside-nvidia-sentry-secure-ai-agents-at-runtime/)

3. 分布式科研Agent：把天然可拆分的问题分工，再合并证据
OpenAI于10月21日介绍与PerturbAI、Scripps的工作，相关预印本实际10月16日发布。系统将基因扰动逐项分配给Agent，汇合细胞数据、文献和解释；官方报告32分钟完成大规模分析，最大工作量对照中分布式方案引用700余篇文献，单Agent为11篇。
价值：适用于可独立拆分、需要逐项保留证据的大规模研究。
限制：引用数量不等于结论质量；32分钟是墙钟时间，不是总计算成本，所得仍是待实验验证的假设。[官方解读](https://openai.com/index/scaling-ai-research-for-complex-biological-data/) · [作者代码与分析材料](https://github.com/jinlabneurogenomics/distributed_agent)

4. Cache Engineering：上下文优化不只改文本，还直接管理KV cache
论文10月20日提交、进入21日arXiv公告批次。直接丢弃、重定位KV，减少文本改写后重复prefill，配套AKV基于SGLang。
作者在3个模型、4类任务上，相比阈值触发摘要基线，报告平均得分提高1.7个百分点、估算成本下降27.7%。
价值：将长程Agent的历史管理与推理执行连起来，而不是分别优化。
限制：需要可控缓存后端，不能直接套用任意闭源API；成本按活跃token及公开费率估算，不是实际云账单，也非所有任务均提升。[论文](https://arxiv.org/abs/2610.28577) · [AKV代码](https://github.com/thu-nics/AKV)

5. OO-Harness：按任务选择专用执行框架
论文10月20日提交、21日进入公告批次。从执行轨迹提炼失败模式，用继承机制生成专用harness，再根据任务和基础执行轨迹路由；优化提示、工具与控制流，而非主模型权重。
作者在AutomationBench报告平均部分得分61.62→77.41，严格成功率25.44%→37.22%。
价值：把harness作为独立可优化、可版本化的组件，而非所有任务共用一套固定流程。
限制：路由需要先观察基础执行，可能重跑；表中Steps未计入这些额外调用，不能据此判断更省成本。[论文](https://arxiv.org/abs/2610.28380) · [实验口径](https://arxiv.org/html/2610.28380v1#S4.T1)

6. ALTK-Evolve：从轨迹提炼经验，但规则并非越多越好
10月20日发布新评测解读，不是框架首发。将成功／失败经验提炼为系统提示规则，不更新权重。40个留出零售任务、每题4次尝试中，GPT-4.1的Pass^4从57.5%升至67.5%；航空任务中精简规则更可靠。
价值：经验优化应同时观察平均效果和重复执行可靠性。
限制：完整零售规则使评测会话成本增加65%；榜单提交含训练任务且仍待合并，不能把“榜首”宣传当独立验证。[IBM评测解读](https://huggingface.co/blog/ibm-research/altk-evolve-bc) · [榜单提交及限制](https://github.com/sierra-research/tau2-bench/pull/542)

热度说明：北京时间约09:30，Nemotron官方文章29票、ALTK文章24票，仅属早期平台关注信号，本期不将它们称为“爆火”或普遍采用。

值得关注的两个方向
- 按成功任务算全链路成本：同时计入检索、重排、缓存、路由、重试和P95完成时延，避免局部省token、整体更贵。
- 将可学习的记忆与harness同授权边界分离：经验可以更新，权限不能随经验自动扩大。
