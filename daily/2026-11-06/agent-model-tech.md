**Agent 与大模型技术热点｜2026-11-06**

观察截至北京时间 11 月 6 日 09:26。优先关注近 24 小时，补充近 7 日有代表性的研究与工程进展；未标明时区的公告只按来源日期列出。研究数字均为作者或厂商报告，并非独立复现。本期与昨日技术简报去重，不把论文结果、预览功能或企业承诺当成已经普遍落地的能力。

**1. GPT-6 Cyber：专业能力与授权边界一起训练**

11 月 5 日，OpenAI 发布面向网络安全工作的 GPT-6 Cyber，首批向部分获批 Daybreak Red 客户开放 API 和 Codex 使用。官方称其 ExploitGym 成绩从前代的 32.1% 升至 48.5%；另一项授权范围评测中，无部署提示的测试有 99.93% 未越界。

价值在于将专业任务表现和授权边界共同纳入后训练与评估，而不是只追求更强的漏洞分析能力。限制是这两组数字均来自厂商评测，覆盖范围和真实环境不同，不能推断生产使用不会越权；可用范围也不是全面开放。[OpenAI 公告](https://openai.com/index/introducing-gpt-6-cyber/)

**2. Coca-Cola：压缩工具结果时，保留“下一步怎么做”**

论文首次提交于北京时间 11 月 4 日 21:32，属于近 7 日研究。它把工具返回的观察压缩成潜在 token，并训练压缩器尽量保持 Agent 的下一步动作分布，而非仅追求文本摘要相似。

作者在四个网页搜索问答基准、两个模型家族上报告 token 用量减少 70.01%–89.24%，端到端耗时减少 24.45%–55.97%。这提示上下文压缩可以以决策信息是否保留为目标。但这是预印本和作者自报结果，实验集中于搜索问答；需要模型嵌入层接入，不能直接视为任意闭源 API 可用的通用方案。[论文](https://arxiv.org/abs/2611.06500)

**3. ToolSpace：用真实工具依赖构造可重放的 Agent 训练任务**

论文首次提交于北京时间 11 月 3 日 23:09。研究从真实 MCP 服务和公开轨迹提取数据、状态及重试依赖，构造工具组合图，再通过带回滚的随机游走生成任务，只保留可重放验证的样本。

作者报告 Qwen3.6-35B-A3B 在七项基准上的平均分由 23.0 升至 41.0，加入强化学习后为 43.2。其价值是把真实接口约束和可执行验证引入训练数据生成，减少凭空拼接工具流程。论文仍标注进行中；提升涉及训练数据和训练过程，不能解释成仅更换推理时编排框架即可获得同样收益。[论文](https://arxiv.org/abs/2611.05036)

**4. Zep Agent Skills：把已验证轨迹沉淀为程序性记忆**

论文首次提交于北京时间 11 月 1 日 08:47。其 Reflector 与 Curator 将验证过的执行轨迹转为带证据、版本及准入控制的技能，再通过 API 或 MCP 按需检索。作者在三个评测环境中报告，较弱执行模型的通过率提升 4.5–18.8 个百分点。

值得关注的是，记忆不只保存事实，还保存经过验证的做事方法。限制是主要增益发生在教师模型已经解决过的相同任务上，不能据此宣称对未见任务同样有效；这也是预印本及厂商自评。[论文](https://arxiv.org/abs/2611.01386)

**5. 多教师在线策略蒸馏进入 TRL 工具链**

11 月 3 日的 Hugging Face 技术教程介绍，TRL 的 AsyncDistillationTrainer 可按提示所属领域路由到不同专家教师，再将数学、代码或工具使用等能力蒸馏到一个学生模型。

这为“先分别训练专家、再合并专长”提供了可使用的训练路径。它是已有方法的工程支持，不是新提出的模型架构；教程发布时同步版本支持仍处于 PR 阶段，效果和成本仍取决于教师、数据及训练配置。[教程](https://huggingface.co/blog/sergiopaniego/mopd-trl) · [实现文档](https://huggingface.co/docs/trl/async_distillation_trainer)

**6. CUDA 13.5：Vera Rubin 支持进入正式生产阶段**

NVIDIA 于 11 月 4 日发布 CUDA 13.5，将 Vera Rubin 支持从早期开发阶段推进到正式可用，同时更新 GPU 资源共享调度、CUDA Tile 多 GPU 通信及 Python、数学库。

这对新硬件上的模型训练和推理部署更有实际意义：需要评估的不只是芯片规格，还包括调度与软件栈成熟度。注意 Tile-SIMT 互操作和 Nsight Perf SDK for AI 仍为公开预览，不能把所有新功能都称为 GA。该发布也获得 [Phoronix 独立报道](https://www.phoronix.com/news/NVIDIA-CUDA-13.5)，但缺少足够依据称其为大众热榜事件。[NVIDIA 官方说明](https://developer.nvidia.com/blog/nvidia-cuda-13-5-brings-nvidia-vera-rubin-ga-support/)

**7. 算力扩张的产业约束：OpenAI 发布数据中心社区承诺**

11 月 5 日，OpenAI 发布数据中心社区承诺及政策建议，称未来自有、自营计算园区将承担用电及配套基础设施成本，并披露水、电和噪音等影响；除有限例外，不要求公职人员签保密协议。

这反映大型 AI 基础设施的约束已从芯片供应扩展到电网、地方成本和社区透明度。这是公司承诺与政策主张，不是新法律，也不自动覆盖所有合作伙伴机房。热度依据是同期出现 [DCD 专业媒体报道](https://www.datacenterdynamics.com/en/news/openai-promises-to-drop-ndas-and-increase-community-transparency-following-anti-data-center-wave/)及 [ABC News 报道](https://abcnews.com/Business/openai-issues-new-data-center-policies-after-midterm/story?id=137874775)，并不意味着已登上全网热榜。[OpenAI 公告](https://openai.com/index/setting-new-standard-responsible-ai-infrastructure/)

**值得持续关注的两个方向**

- **面向行动的上下文与记忆**：从压缩原文转向保留下一步决策信息，从事实记忆延伸至可验证的操作经验。关键仍是跨任务泛化、失效识别和权限边界。
- **用可执行环境生产训练信号**：真实工具依赖、可重放任务和多教师蒸馏都在改善 Agent 的训练材料，但离线基准提升不能直接等同生产可靠性。
