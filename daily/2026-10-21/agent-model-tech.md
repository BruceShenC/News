# 10月21日 · Agent 与大模型技术热点

日期：2026-10-21（Asia/Shanghai，北京时间 UTC+8）

归档说明：本文保留 2026-10-21 当期调研内容、来源和统计口径，不代表当前状态。仅将个性化称谓改为通用表述；未重新研究或执行项目。

今天的重点：记忆开始进入模型内部，语音 Agent 出现两条不同集成路线，推理平台则开始围绕多轮任务优化。精选6条；10月20日的产品公告优先，另补充近7日研究。论文与厂商实验均未在目标环境独立复现。

1. MemT-1.0：从保存完整历史，转向模型内的有界记忆
论文10月19日UTC首发，20日更新。通过 Gated DeltaNet-2 与 Mixture-of-Storage 构建原生记忆，发布 Nano／Flash。作者在128K历史下报告：Nano 相对完整上下文KV缓存约快1.86倍，持久存储缩减约66倍。
价值：为长程 Agent 降低历史状态成本提供不同于纯RAG的路线。
限制：固定容量状态存在细节遗忘；Agent实验仍依赖外部构造、检索经验，不等于自主持续学习。此次版本更新不能单独视为新突破。
热度：北京时间约09:29，[HF论文页](https://huggingface.co/papers/2610.26825)146票，为累计快照。[论文与限制](https://arxiv.org/html/2610.26825v2) · [代码](https://github.com/MemTensor/MemT-1.0)

2. OpenVLHarness：视觉 Agent 的关键不只是“多想”，还有证据组织
论文10月19日UTC提交，20日进入HF推荐。统一工具接口与证据渲染，把图片、mask、数组、网页引用保存为可寻址的中间产物，供后续步骤和Python复用。
作者在22个基准、8个骨干模型上，相对匹配的无工具基线报告平均提高9.7–12.6分。
价值：适合作为视觉 Agent 的工具结果组织与上下文工程参考。
限制：这里的持久记忆是单个问题内跨步骤保留，不是跨会话记忆；成本优势只计算模型token，未计工具与评审费用，不能当总成本降幅。
热度：同一时点[HF页面](https://huggingface.co/papers/2610.26860)97票。[方法与实验](https://arxiv.org/html/2610.26860v1) · [代码](https://github.com/uclanlp/OpenVLHarness)

3. Voxtral 3：把可替换的语音链路做成流式 Agent
Mistral于10月20日发布语音模型家族及基于LiveKit的SDK。ASR、LLM、TTS分离但并行流式运行，支持打断及SIP／WebRTC。
价值：适合需要替换推理模型、声音或考虑数据驻留的客服和服务台。
限制：不是单一端到端音频模型；官方每轮低于1秒的表述不是通用SLA，完整私有化需联系厂商，不能理解为所有权重均已开放。[官方发布](https://mistral.ai/news/voxtral-3/)

4. Eleven Realtime：用一体化语音接口区分轻量交谈与复杂工具任务
10月20日发布Mini／Max，分别面向自然交谈与复杂指令、多步工具调用；采用兼容OpenAI Realtime的WebSocket接口。
评测方Artificial Analysis当前记录Max首音频0.57秒、工具任务成功率87.1%；Mini分别为0.62秒、60.3%，说明产品分层不能只按延迟理解。
价值：减少自行拼装识别、轮次控制、推理和发声的工作。
限制：逐步开放，Max有区域限制；API不是WebRTC直接替代，托管模式也不能自由选择底层LLM。以上是特定评测结果，不是实际业务保证。[官方发布](https://elevenlabs.io/blog/realtime) · [产品限制](https://elevenlabs.io/docs/eleven-agents/customization/realtime-models) · [评测原始榜单](https://artificialanalysis.ai/speech-to-speech)

5. llm-d 1.0：让推理集群围绕多租户和多轮 Agent 运转
10月20日正式发布1.0，加入多租户优先级与公平流控、路由热备、零停机升级、稳定v1配置API，并支持会话感知路由及Responses API状态。
价值：把上下文复用、调度和生产运维连起来，对相关推理网关、多租户平台有参考意义。
限制：官方引用的5–10倍成本优势来自特定高缓存复用实验，不是1.0普遍收益；相关实验规模达544张H100，不能直接套用小集群。[版本公告](https://llm-d.ai/blog/llm-d-v1.0-state-of-the-art-inference-ready-for-day-2) · [实验条件](https://research.ibm.com/blog/running-open-models-on-h100-gpus-with-llmd)

6. Mercury 3：扩散语言模型继续挑战逐token生成路线
10月19日正式发布，API上线。Inception称总参数规模超过Mercury 1的40倍，上下文扩至1M，并报告NVIDIA GPU上1,068 tokens/s。
价值：并行生成若能维持任务质量，可缩短Agent每轮等待，而不只提高单次文本吞吐。
限制：速度与竞品对比属于厂商口径，绝对参数量、完整训练配方未公开；引用的网页Agent实验仅50个任务，不能泛化为全面领先。[官方发布及实验口径](https://www.inceptionlabs.ai/blog/introducing-mercury-3)

值得关注的两个方向
- 记忆分层：模型内压缩记忆、任务内工具产物、外部知识库解决的是不同问题。选型时分别衡量细节召回、状态成本与证据可追溯性，不宜互相替代。
- 按“完成一个任务”优化：语音链路、并行解码和会话路由都在减少等待。比较时应同时看成功率、完整任务时延和总费用，而非只看tokens/s或首包时间。

热度数字只代表关注，不代表技术成熟度；本期未将产品宣传或社区票数当作突破证据。
