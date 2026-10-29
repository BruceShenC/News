# Agent 与大模型技术热点｜2026年10月28日

日期：2026-10-28（Asia/Shanghai，北京时间 UTC+8）

归档说明：本文保留 2026-10-28 当期调研内容、来源和统计口径，不代表当前状态。仅将个性化称谓改为通用表述；未重新研究或执行项目。

本期6条：4条10月27日发布或公告，另补2条近7日记忆研究。重点看技术变化和可用边界，不重复项目榜单及缺陷回归。

1. LFM3正式开放权重：小模型通过混合架构承担专门任务
事件：10月27日。1.73B主干采用24层三混合结构：13层门控短卷积、5层滑窗注意力、6层全局注意力；同主干衍生文本、视觉、音频、Omni及decision模型。训练包含22T预训练，以及蒸馏、真实Agent运行环境中的强化学习。[技术公告](https://www.liquid.ai/blog/lfm3)
价值：工具schema可延迟加载，由harness检索再注入上下文；decision模型通过一次前向输出类型化答案和概率，适合把高频窄任务从长文本推理中分离。
限制：官方明确不推荐复杂多步推理或agentic coding；256K窗口不等于全窗口可靠，约1GB的4bit宣传也不等于完整运行内存。使用自定义LFM许可，不是Apache。[模型卡](https://huggingface.co/LiquidAI/LFM3-1.7B)
热度依据：截至北京时间09:35，[LocalLLaMA讨论](https://www.reddit.com/r/LocalLLaMA/comments/1xgy8s3/liquidailfm317b_hugging_face/)约235票，仅代表社区关注，不是独立性能验证。

2. Genie 4研究预览：规划Agent与实时世界模拟器形成反馈环
事件：10月27日，已向受信测试者开放；面向Ultra用户及API早期伙伴仍是后续计划，不是公众GA。
变化：原生生成视觉、环境声及对话，Gemini指导Agent持续观察画面、拆任务、检查进展并调整计划，Genie负责生成被操作的世界。这比单纯“生成逼真视频”更接近交互环境。[DeepMind公告](https://deepmind.google/blog/genie-4-a-new-generation-of-world-simulation/)
价值与限制：可探索Agent/机器人环境生成和交互原型；但长会话、复杂改写、大范围移动仍有物体持久性和一致性漂移，不能当可信物理仿真或迁移成功证据。
热度依据：[GeminiAI讨论](https://www.reddit.com/r/GeminiAI/comments/1xgx4wc/google_announced_genie_4_their_new_world_model/)约278票，关注也包括实际用途及封闭访问限制。

3. MAI-Image-2.7编辑预览：控制“改哪里、保留什么”
事件：10月27日。微软公布图像编辑预览，强化场景/光照调整、文字编辑、修复增强；文生图仍预告，架构和训练细节未披露，不能称架构突破。[官方公告](https://microsoft.ai/news/mai-image-2-7-debut/)
可核实信号：[Arena商业设计单图编辑子榜](https://arena.ai/leaderboard/image/image-edit/single-image-commercial-design)显示第三，1499±22、718票、排名区间2–6；页面标10月23日，是预发布榜单快照，不是27日新测的所有图像任务总榜。
价值与限制：值得关注局部编辑与整体关系保持；偏好排名不能证明生产稳定性、延迟或成本优势，且样本量需与区间一起看。

4. SpecMem：把记忆用于工具调用的“解码草稿”
事件：10月22日研究，近7日补充。检索用户过去已验证的tool call作为投机解码草稿，验证后写回有界记忆，并用LRU淘汰；有副作用的工具另需执行门控。[论文及正文](https://arxiv.org/html/2610.31840v1)
价值：记忆不仅回答“以前知道什么”，还可利用重复调用结构减少解码工作。作者报告接受token平均增加27%，外部投机环路最高1.74倍墙钟加速。
限制：多用户重复结构主要是合成的，部分指标是规范化调用前缀的代理量；外部环路计时不是引擎原生集成。论文仍称将发布harness及原始结果，不能说已完整开源复现。无足够独立热度证据，列为研究线索。

5. MemoryTax：先保留原始历史，再证明“压缩记忆”有收益
事件：10月21日小规模实证研究。比较full context、BM25、Mem0、Zep，以及编码Agent搜索原始历史，指出写入阶段丢失的信息无法由更强读取模型恢复。[论文](https://arxiv.org/abs/2610.29823)
价值：为知识网关提供重要反向基线，不默认“先抽取、结构化所有记忆”就更优；成本应包括建库，而不只算查询。
限制必须前置：仅一段419轮对话、152题、每组合一次运行；不同方法看到的照片caption信息不等价，且全部历史可装进模型窗口。不能据此宣布专用记忆系统无用。这是值得验证的假设，不是已确立行业结论。

6. Salesforce Agent Fabric：统一控制平面，但自动质量干预尚未全部上线
事件：10月27日公告。跨Azure/Apigee/Kong网关策略、FinOps、Model Wallet及成本策略已GA；Agent Kill Switch本身已GA，Custom Agent Scanner为Beta。[官方可用性清单](https://www.salesforce.com/news/stories/agent-fabric-catch-failures-control-ai-costs/)
尚属预告：Agent Evals/API Evals计划11月Beta、2027年1月GA；自动触发的Triggered-Agent Kill Switch计划2027年1月GA，不能说今日已形成完整自动质量闭环。
价值：把资产发现、策略、预算和后续评估干预连入同一治理层，比单独监控更接近企业Agent运营。限制是阈值、权限及人工审批仍需团队定义；“最多减少75% token”只是测试场景自报，不作普遍收益。

值得持续关注的两个方向
- 专业化小模型＋大模型分工：将工具筛选、结构化抽取和决策等窄任务分离，按自身任务测成本与效果，不照搬发布方平均分。
- 记忆与执行分层：原始证据保真、调用缓存、用户隔离、权限和副作用门控分别解决不同问题，不能用“一个统一向量库”概括全部Agent记忆。

本期仅研究公开资料，未申请预览、注册或执行项目。
