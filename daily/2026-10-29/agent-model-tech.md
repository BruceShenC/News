# Agent 与大模型技术热点｜2026年10月29日

本期6条：5条10月28日新发布或工程进展，另补一项近7日研究。值得关注的变化是：模型能力之外，多模型调度、证据化记忆和可复用程序正在承担更多工作。

1. Gemini 4 Carbon：强化长程Agent任务，仍是受限预览
10月28日宣布，本周先向部分网络防御人员开放，随后扩至Fairwind可信伙伴，尚未面向公众全面可用。Google强调持续多日工程任务、失败恢复和复杂工具工作流，但未披露新底层架构。[官方发布](https://blog.google/innovation-and-ai/models-and-research/gemini-models/introducing-gemini-4-carbon/)
关注依据：Artificial Analysis当日独立评测中，Carbon High在Intelligence Index v5得59分、居首；同文也指出并非所有编码和知识工作都领先。[独立评测](https://artificialanalysis.ai/articles/gemini-4-carbon-new-number-1)
价值与限制：值得观察长任务的规划与恢复能力；“运行数天”仍是内部观察，不能当作无人值守可靠性保证，也不能把发布价格理解为已可普遍购买。

2. Copilot Fusion正式GA：模型选择成为运行时编排
10月28日正式发布，将模型路由与HydraFusion编排结合：Single直接完成；Cascade先由较小模型起草、检查后决定是否升级；Critique由第二模型审阅、再让原模型修订。[GA公告](https://github.blog/changelog/2026-10-28-route-and-orchestrate-models-with-github-copilot-fusion/)
价值：团队可以选择投入水平，让系统按任务协调模型；尽量保持主对话模型稳定以利用缓存，审阅只获取必要上下文。[机制说明](https://docs.github.com/en/enterprise-cloud%40latest/copilot/concepts/models/copilot-fusion)
边界：这是编排系统，不是新基础模型，也不等于自动创建子Agent。AA已发布独立分析，但成本收益依任务而变，多模型调用仍分别计费。同日“本地模型智能路由”仅为实验预览，不能与GA混写。[AA分析](https://artificialanalysis.ai/articles/github-copilot-fusion-orchestrates-anthropic-and-openai-models-to-balance-performance-and-cost)

3. OpenWiki＋证据图：把知识结论连接回可更新的来源
10月28日，LangChain公布工程实验：先将文档编译为带引用的wiki，再把引用、修订和替代关系变成图边，Agent通过locate/expand导航；来源行哈希变化可标记需要重新审核的claim。[完整实验](https://www.langchain.com/blog/building-agent-memory-openwiki-graphs)
作者在111份合成保险文档、130道题、每题3次运行中发现：跨文档题的准确率由关键词搜索基线0.10提高至wiki的0.52；加入证据图后，相比wiki再提升27%。
适用限制：适合反复跨源推理。简单查找没有明显收益且更慢，成本分析未计编译和维护，引用图也继承模型生成引用的错误风险。这是厂商实验，尚不能证明全面优于混合检索RAG。

4. Nemotron 3.5 Transcribe：开放权重的指令式多任务语音模型
10月28日正式发布。组合语音编码器、冻结的说话人模型与Mamba–Transformer MoE语言模型，约34B总参数、4.3B激活参数；通过指令完成转录、翻译和说话人标注，支持40种语言转录及最长一小时录音。[NVIDIA技术页](https://research.nvidia.com/labs/nemotron/NVIDIA-Nemotron-3.5-Transcribe/)
价值：会议、客服录音可以通过统一模型进入后续Agent流程，原生MTP和可选DFlash2提供推理优化路径。
限制：面向离线录音，不是实时流式语音；不同任务的语言覆盖不同。模型卡最高吞吐依赖NIM优化，其中部分服务优化尚未开源，不能把权重开放等同于全部性能栈可复现。[模型卡](https://huggingface.co/nvidia/NVIDIA-Nemotron-3.5-Transcribe-30B-A4B)

5. Rime Prism：语音说出口前，保留可检查的文本层
10月28日宣布上线。以音频理解作决策，在生成语音前提供可检查、可改写的文本检查点；讲话时继续接收用户声音，工具调用与语音可以并行。[一手发布](https://www.rime.ai/resources/prism-first-speech-to-speech-for-enterprise)
价值：尝试同时保留语气信息与文本级控制，适合需要审核措辞或业务规则的语音Agent。
限制：公开τ-Voice成绩是Rime使用AA测试框架自测，不是AA独立验证；自助RL/SFT API仍与Beta客户建设中，“可拥有、可微调”也不代表公开下载权重。暂不把不同口径的延迟宣传合成一个性能数字。

6. FlashScript：把重复Agent任务变成可复用程序
10月24日提交的研究预印本。将成功轨迹中的操作编成代码、变化值变为参数，只将语义判断保留为有类型的LLM调用，再通过缓存及合并调用减少重复推理。[论文](https://arxiv.org/abs/2610.33876)
作者在120个WorkBench分析任务中报告：最佳配置的输入token约为Skill引导Agent的1/70，运行时间约为1/3。
关键限制：最佳结果还使用任务模板和规划指令，并非仅凭一次成功轨迹。仅轨迹生成的程序在未见实例准确率0.89，组合任务降至0.53，泛化仍受任务边界约束。适合研究稳定、重复的工作流，不宜直接套用于开放式任务。

值得跟进的两个方向
- 多模型编排与程序化复用：先判断哪些步骤需要模型，再决定由哪个模型做，减少每次从头推理。
- 可追溯记忆：知识系统可以重点参考claim、来源定位、版本替代关系与变更失效检测，把“找到相关材料”推进到“说明结论依据及是否过期”。

除列明的独立评测外，其余按正式发布、工程实验或研究线索呈现，不把营销或自报指标当作技术突破。本期仅研究公开资料。
