# Agent 与大模型技术热点｜2026-11-21

观察窗口：北京时间 11 月 20 日 09:30 至 11 月 21 日 09:30。前三条有窗口内可核验的公开记录，后两条是近 7 日补充。未运行模型或独立复现评测。

**今天最值得看的是 Hugging Face 的适配器实验：同一个基座、同一个 Agent 框架，只改变适配器，就能改变任务通过率与工具调用次数。但跨框架通用性并不免费，专用适配器在部分任务上仍明显领先。**

## 窗口内更新

### 1. Hugging Face：为 Agent 执行框架训练 LoRA，再合成通用适配器

**状态：** 官方报告标注 11 月 20 日，其公开博客仓库于北京时间 21:36 合并正文。这是公开合并时间，不是博客前端的精确上线时间。

团队分别在 pi、Hermes Agent 和 OpenCode 的原生执行框架内，为 Qwen3.8-27B 训练了三个 LoRA，再把它们合成一个适配器。在六个框架与基准组合中，按各框架所有对照都完成评分的任务计算，关闭 thinking 时平均 pass@1 从 **39.5% 升至 49.0%**；使用同样的默认高推理档时，从 **55.5% 升至 63.5%**。但在 pi 的 SWE-bench Pro 子集上，关闭 thinking 时专用适配器达到 **64%**，合并版只有 **46%**，基座为 **40%**。

**边界：** 工具调用减少 53%–73%、生成 token 减少 44%–60%，仅指关闭 thinking 时两者都做对的任务，不是所有请求的成本降幅。合并权重选择用过 Terminal-Bench 2.1 的早期实验，相关成绩可能偏乐观。

来源：[Hugging Face 技术报告](https://huggingface.co/blog/harness-adapters-at-scale) · [公开合并记录](https://github.com/huggingface/blog/commit/bca468d4d659aacb38146d155af4a96e8bd49920)

### 2. 腾讯 Hy4 开放权重：从 API 选项变成可自部署模型

**状态：** 腾讯混元官方于北京时间 11 月 20 日 11:52 宣布开放 Hy4 Instruct、FP8 Instruct 和预训练 Base 权重。这是 11 月 4 日模型发布后的权重开放，不是新模型首发；各平台首次可下载的准确时刻未核实。

Hy4 支持文本、图像和视频理解。语言骨干为 **770B 总参数、每文本 token 激活 49B、约 100 万 token 上下文**。官方部署材料覆盖 vLLM 和 SGLang；投机解码的草稿模型复用主模型的 KV Cache 和稀疏索引，不另建独立 KV Cache。

**边界：** 49B 激活不等于只需驻留 49B 权重，100 万上下文也不是吞吐或延迟承诺。权重使用 **Tencent Hy License 1.0**，不是纯 Apache 2.0；面向第三方提供通用模型服务的团队应核对其中的商业条款。

来源：[腾讯混元权重公告](https://x.com/TencentHunyuan/status/2123625549147070538) · [官方模型与部署说明](https://github.com/Tencent-Hunyuan/Hy4#model-links) · [许可证](https://github.com/Tencent-Hunyuan/Hy4/blob/main/LICENSE)

### 3. Inkling 2 Mini：24 层权重，每个 token 执行 48 层

**状态：** Thinking Machines Lab 于北京时间 11 月 21 日 04:10 官宣。模型权重以 Apache 2.0 发布，模型卡列出本地运行与 Tinker API 入口。

这款 26.4B 多模态 MoE 让中间 12 层共享权重、重复执行三次，首尾各 6 层只执行一次。它不靠新增等量参数增加推理深度，而是把更多计算放进单个 token；官方混合精度检查点约 20 GB。

**边界：** 少生成 token 不等于端到端一定更快，要把每 token 的额外计算算进去。官方也列出了多轮聊天和复杂约束任务上的相对弱项；检查点大小不等于完整运行内存。

来源：[Thinking Machines Lab 发布说明](https://thinkingmachines.ai/news/inkling-2-mini/) · [官方发布时间](https://x.com/thinkymachines/status/2123871643797995872) · [模型卡](https://huggingface.co/thinkingmachines/Inkling-2-Mini)

## 近 7 日补充

### 4. SageMaker 上线跨区推理路由，但尚不支持响应流式返回

**状态：** AWS 11 月 18 日宣布可用，19 日发布工程说明。不是本期 24 小时新增。

路由器复用现有端点，优先把请求发往本区，在错误上升时把新请求转到其他健康区域，本区恢复后再逐步回流。实时响应标明实际服务端点和路由原因，端点也可限制接收的跨区流量，为本地调用留余量。

**边界：** 当前不支持响应流式返回，实时目的端约一分钟未响应会失败；不能直接替代面向流式生成的 LLM 网关。

来源：[AWS 上线公告](https://aws.amazon.com/about-aws/whats-new/2026/11/sagemaker-cross-region-routing/) · [技术详解](https://aws.amazon.com/blogs/machine-learning/keep-inference-running-through-regional-capacity-shortfalls-with-cross-region-inference-routing-in-amazon-sagemaker-ai/)

### 5. Google Cloud 把 MCP 工具策略提示放进注册目录

**状态：** Agent Registry 的 11 月 19 日更新，**预览**。

注册目录新增 MCP 工具协议级元数据，包括与工具是否启用、是否需要人工介入有关的 Identity-Aware Proxy 策略提示，以及提供方属性。这样，工具目录可以表达的不只是“有哪些工具”，还包括“调用时应注意哪些条件”。

**边界：** 目录中的策略提示不等于运行时授权已经强制执行。这一更新也不是 Agent Registry 整体重新发布。

来源：[Google Cloud Agent Registry 发布说明](https://docs.cloud.google.com/agent-registry/release-notes)

## 两个值得跟进的方向

- **把模型与执行框架一起选型。** 固定模型版本、框架、推理档和预算，再比较成功率、全部请求的成本，以及双方都成功任务的成本。三者回答的是不同问题。
- **把跨区兜底分成流式和非流式两条链路。** SageMaker 的现有能力适合对标本地优先、故障分流和渐进回流，但不能据此推断已经覆盖流式请求。
