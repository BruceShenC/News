# 大模型与 Agent 质量、测试与工程资讯｜2026-11-23

**观察窗口：北京时间 11 月 22 日 09:30 至 11 月 23 日 09:30。** 7 项窗口内合入，另有一组容量问题待跟踪。**最值得补的门禁：切换部署、缓存或协议路径后，不能只验“请求成功”，还要核对实际工具范围、输出完整性和缓存命中。** 实验数据来自作者；合入不等于稳定版发布，未独立运行测试或压测。

1. **vLLM Rust 前端：允许调用的工具变了，完整提示不必变。** [#68408](https://github.com/vllm-project/vllm/pull/68408)，11 月 22 日 17:03 合入。Chat Completions 与 Responses 现在保留所有已声明工具供模板渲染，只在生成约束中限制本轮子集。作者的模拟引擎测试核对了渲染器和引擎分别收到什么，也覆盖空子集和无效工具名；报告 Rust workspace 2898 通过、13 跳过。未报告真实多轮缓存收益。门禁建议：固定完整工具列表并轮换子集，分别验渲染 token 不变、约束随请求改变、执行时仍检查权限。
2. **SGLang：同一 seed，编码器常驻与卸载的图也可能不同。** [#51187](https://github.com/sgl-project/sglang/pull/51187)，22 日 16:16 合入。作者在单 H200 上对照 Qwen-Image-Edit-2511、固定图片和种子、8 个编辑提示：旧版常驻与卸载 0/8 像素一致，独立常驻对照 8/8。视觉 RoPE 在不同设备初始化，舍入差异随编码传播；在当前设备重建 FP32 表后，显式卸载与原常驻 8/8 一致。新增 BF16 循环没跑 GPU，未复测自动卸载；Edit-2509 也未启用，不能泛化到整个家族。
3. **SGLang HiCache：减少 CPU 等待，但仍需守住 GPU 页复用顺序。** [#50469](https://github.com/sgl-project/sglang/pull/50469)，23 日 00:46 合入。作者观察到 D2H 备份占 Prefill 调度时间 50%–56%，将等待移到相应 GPU stream。GB300、14 台 Prefill 与 DEP16 Decode、多轮 Agent 回放，各两轮 1200 秒，同镜像只替换补丁：每 GPU 端到端 token 吞吐 +4.2%，TTFT p90 12.0→9.6 秒，TPOT p90 7.25→7.32 毫秒。并行 GSM8K 是准确率观察，不是逐 token 等价；额外 CI 有失败，L3、buffer pipeline、cache linker、统一内存池和 PD Decode 仍走阻塞。建议同时验备份、页复用、回载的事件顺序。
4. **vLLM ARC：剩余块数不等于可回载的连续前缀长度。** [#69110](https://github.com/vllm-project/vllm/pull/69110)，22 日 18:17 合入。并发写入时，复用的已有前缀块未参与同一淘汰追踪，可能导致后缀仍在但前缀无法命中。作者报告 CPU manager 88 通过，旧版两项定向 ARC 用例失败；GB10、SmolLM2-135M-Instruct、4 个错峰请求，每版本交替 3 轮，保留 CPU 缓存并重置 GPU 前缀，重播命中 0→1664 token，单 token 重播中位时延 268.3→133.9 毫秒，并发生成总时长基本没变。这不是大模型线上吞吐证据。
5. **SGLang 流式 logprob：文本看起来对，不代表 UTF-8 字节和候选键都对。** [#49982](https://github.com/sgl-project/sglang/pull/49982)，22 日 19:09 合入。旧版用词表抽样推断 byte-level tokenizer，特殊 token 会误导判断；修复优先依据 fast tokenizer 的 decoder 声明，覆盖 Python 和 Rust。作者用重音字母、汉字及 emoji 核对字节聚合与候选键，DeepSeek-V4.1 定向用例旧版失败、修复后 Hugging Face 和 fastokens 路径通过。首次 tokenizer 序列化有成本，之后读缓存；未改模型 forward。
6. **SGLang 视频：接单成功，不代表后台能生成多个结果。** [#51753](https://github.com/sgl-project/sglang/pull/51753)，22 日 16:15 合入。Hy-Nomas API 原先把两个样本送入单样本会话，离线 CLI 已逐输出拆分所以不显现；现在 API 复用既有顺序生成。新增回归走实际输入校验与 latent 准备，断言逐次单样本、相邻 seed、第一份初始 latent 与单输出一致；旧版失败，仅放宽形状校验也不通过。没有 GPU 双输出端到端验证，峰值显存仍须实测。
7. **SGLang 约束解码：同一 EOS 既可结束序列，也可作为合法语法字面量。** [#52180](https://github.com/sgl-project/sglang/pull/52180)，22 日 10:28 合入。作者用 llguidance 1.9.1 的 CPU 正反例验证：合法位置接收并停止，过早出现则拒绝且状态不损坏。首例旧版失败；基础 CI 仍有失败，语法字面量回滚仍受上游限制。门禁应把提前拒绝、合法结束和 draft 回滚分开，不用其中一项通过替代另两项。

**跟踪而非升级依据：** Dynamo [元数据发布端 #17512](https://github.com/ai-dynamo/dynamo/pull/17512)、[纯 Decode #19356](https://github.com/ai-dynamo/dynamo/pull/19356)、[纯 Prefill #19360](https://github.com/ai-dynamo/dynamo/issues/19360) 核查时均开放。是同一贡献者的 CPU 估算验证，没有独立 GPU 吞吐或 SLO 数据。建议按 **token/rank/step** 对账，P、D 分开；纯 Prefill 按请求核对 rank 归属，混合步骤另立场景。

**优先做两项：** 第一，工具、流式 logprob、异步输出的门禁延伸到引擎约束、字节和任务终态。第二，快路径用同一输入和种子对照，分开报告正确性、命中、延迟和未测条件。以上主要是测试 AI 系统；未见足够新增证据证明 AI 辅助测试或 GUI 自愈提升覆盖与工时。
