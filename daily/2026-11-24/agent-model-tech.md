# Agent 与大模型技术热点｜2026-11-24

观察窗口：北京时间 11 月 23 日 09:30 至 24 日 09:30。1 条窗口内官宣、3 条近周补充及 1 条近期研究报道。未运行模型或独立复现。

**今天值得看 GLM-5.4：共享权重增加推理深度，但不代表每 token 更省。** MemT-1.0-Pro 则改变长任务的另一项成本：把历史压入固定大小的内部状态。

1. **GLM-5.4 进入 Coding Plan：参数规模与计算深度分开。** Z.ai 于 23 日 18:27 发布介绍。模型重复执行基座中的部分 Transformer 层，复用权重增加深度，并加入视觉输入，面向能看截图、设计稿和界面状态的编码场景。Coding Plan 已开放，通用 API 与权重另有安排。复用参数不意味着实际计算或显存按比例下降，输出更短也未必抵消每 token 的计算增加。[技术介绍](https://z.ai/blog/glm-5.4) · [发布帖](https://x.com/Zai_org/status/2124811995912286352) · [开放状态](https://autoclaw.z.ai/en/blog/model/GLM-5.4/)
2. **MemT-1.0-Pro 已公开权重：保存动态状态，而不是重送全部历史。** MemOS 23 日 06:00 宣布，早于本期窗口，模型卡许可为 OpenMDW-1.1。基于 Nemotron 3 Ultra，在主干注意力旁加入记忆模块，生成时读取、每轮或每段结束更新。它和昨日 ARISE 不同：MemT 改模型内部怎么记，ARISE 改运行时向外部记忆写什么。固定大小不等于无损，细粒度证据回溯仍不应省掉原文与出处。[发布帖](https://x.com/MemOS_dev/status/2124623963087126562) · [模型卡](https://huggingface.co/MemTensor/MemT-1.0-Pro)
3. **Xing4.1-Preview：17B 激活不是 17B 的部署开销。** 22–23 日已有可下载权重记录，首次开放的确切时刻未核实。模型卡列 285B 总参数、约 17B 每 token 激活、100 万 token 最大上下文；MLA 压缩键值表示，DSA 选择实际读取位置。BF16 预览权重约 521 GiB，激活参数不等于权重驻留。SGLang 主分支、vLLM 外置插件、仍在审查的 TensorRT-LLM 支持也不是同一成熟度。[模型卡](https://huggingface.co/XingChen-AGI/Xing4.1-Preview) · [分片与许可](https://huggingface.co/XingChen-AGI/Xing4.1-Preview/tree/main)
4. **GPT-Live-1.1 API：WebRTC 保持连接，闲置时暂停推理。** 官方日志标注 19 日，不是今天发布。模型可边听边说，将复杂推理或工具任务委派给后端；可选待机在语音输入后恢复。官方会话计价为活跃 0.05 美元/分钟，待机 0.02 美元/分钟；只适用于待机时长，后端模型和工具另计，短输入恢复后可能更慢。[更新日志](https://developers.openai.com/api/docs/changelog) · [计费说明](https://developers.openai.com/api/docs/models/gpt-live-1.1) · [待机文档](https://developers.openai.com/api/docs/guides/live-conversations#managing-idle-sessions)
5. **MIT 介绍 SLIC：用仿真反馈训练小模型多轮调电路。** 23 日研究报道未核实具体时区和时刻，原论文 10 日提交。约 4B 本地模型调节参数、调用仿真器、根据可核对奖励迭代。适合反馈明确、可重复试错的窄任务；证据限于模拟放大器优化，不说明小模型普遍超过旗舰模型。[MIT 报道](https://news.mit.edu/2026/ai-driven-technique-could-help-engineers-design-analog-chips-faster-and-less-expensive-1123) · [论文](https://arxiv.org/abs/2611.13452)

**两点判断：** 容量评估分别算权重驻留、每 token 计算和历史状态；成本比较要看减少的是空闲推理、模型调用还是历史重送，并同时记录恢复延迟、适用范围与细节保真度。
