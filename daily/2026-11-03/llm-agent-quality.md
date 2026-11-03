**大模型与 Agent 质量简报｜2026-11-03**

**本期重点：推理与训练的可复现 QA，以及 AI 协助测试时应保留的证据边界。**

- **观察截止：2026-11-03 20:45 北京时间（12:45 UTC）。** 本期为补做版，续接上期截止时间，检索区间为 11 月 2 日 09:35 至 11 月 3 日 20:45 北京时间，约 35 小时，并非严格过去 24 小时。
- 以下 7 项均为该区间提交的一手 issue，已逐项排除 [11 月 2 日简报](https://github.com/BruceShenC/News/blob/main/daily/2026-11-02/llm-agent-quality.md) 的案例。日期指公开提交时间，不推定首次发现时间。
- 检索覆盖 PyTorch、vLLM、SGLang 公开报告及相关讨论；不代表全网穷尽。全部为报告者的测试或代码分析，本简报未自行运行脚本、模型或补丁。截止时这 7 个 issue 均开放；本地修复通过不等于上游已修复。

**一、推理、训练与 Agent 链路 QA**

**1. FP8 训练接口迁移：旧矩阵乘可分片，新公开接口却没有 DTensor 策略**

**提交：11 月 3 日 19:33。** 作者从 torchao 的张量并行 FP8／MXFP8 训练失败，缩小到 F.scaled_mm 使用的 aten._scaled_mm_v2 未注册分片策略；不是 GPU 算不出乘积，而是通信之前的分片推导失败。

- **复现环境与步骤**：GH200、Ubuntu 24.04 aarch64、Python 3.12.3；PyTorch 2.14.1+cu130 与 2.16.0.dev20261030+cu130 均复现。原文完整脚本用单 GPU、world_size=1，生成 M=128、K=1024、N=512 的 FP8 输入，将收缩维分别设为 Shard(1)、Shard(0)，比较旧 torch._scaled_mm 与 F.scaled_mm。
- **判定器（oracle）**：旧接口返回形状 (128,512)、Partial(sum)；新接口抛出“没有注册 sharding strategy”。这是接口支持／分片语义对照，不是输出数值精度测试。作者另在 B300、TP=2、MXFP8 block-wise scales 的 torchao 路径复现。
- **修复与 AI 边界**：作者提出注册 v2 策略及新增测试，未披露修复后结果。明确披露 Claude Code 编写复现与初稿，作者运行 GH200/B300 并手动检查代码。不能把草拟补丁方向写成已修好。
- **本简报建议**：接口升级回归除普通 Tensor 外，应覆盖 DTensor、eager／compile，以及标量／分块 scale；这是建议覆盖，不是已执行矩阵。

[原始报告、完整单卡脚本与 AI 分工](https://github.com/pytorch/pytorch/issues/203215)

**2. 推测解码性能悬崖：allocator 之外的显存，也需要进入 QA 指标**

**提交：11 月 3 日 00:59。** PyTorch flex decoding 默认 num_warps=2，在多 token GQA 的折叠 BLOCK_M 达到 128 时出现寄存器溢出及大量线程栈占用。报告明确限定单 token 解码不受所测悬崖影响。

- **复现环境与步骤**：PyTorch main c685ca344de 源码构建、CUDA 13.0、GB300，Triton 随构建提供。原文附生成张量、causal block mask、torch.compile(flex_attention) 脚本，以及禁用缓存、用 cuobjdump 查看资源的命令；比较默认与显式 fwd_num_warps=4/8。
- **判定器与测量**：B=8、Hq=64、Hkv=8、Q_LEN=16、KV_LEN=8192、D=128、bf16，默认约 6019 微秒、allocator 外增加 3.55 GiB；4 warps 为 133 微秒、额外占用约零。时间用 do_bench；allocator 外显存以 total−free−memory_reserved 的前后差衡量。另一形状的 cubin 显示每线程 36864 字节栈。
- **修复验证边界**：修改 launch 配置是作者对照，不是已合入修复；该报告没有输出精度 oracle 或完整训练／服务验证。只测 GB300，不能推广到 H100/A100 或宣称固定 4 warps 通用最优。报告披露 AI agent 实际测量并起草，ezyang 审阅后提交。
- **本简报建议**：性能回归同时记录设备总占用与框架 allocator 占用，围绕 Q_LEN×GQA 比值跨边界取样；加速对照仍需另做数值正确性检查。

[测量表、复现脚本、资源分析与 AI 披露](https://github.com/pytorch/pytorch/issues/203041)

**3. vLLM render→generate：显式 16 token 上限在序列化后变成“没有提供”**

**提交：11 月 3 日 20:08。** 报告把输出超长定位到默认值省略：SamplingParams 的 max_tokens 默认 16，序列化省略该键，generate 再按“客户端未提供”应用服务端上限。completions 请求省略或传 null 同样会先归一到 16。

- **环境与复现**：main 66360a5cd0、Qwen/Qwen3-0.6B，CPU render 服务加单 GPU engine，后者开启 --enable-scale-out --enforce-eager，默认上下文 40960。精确 GPU 型号／运行库版本未在所阅报告中披露。原文完整脚本在 temperature=0 下比较 coupled API 与 render 输出原样送 generate，并测 15、16、17 三个值。
- **判定器**：请求体中上限应保留且生成不超过限制。15/17 对照正常；16 在 wire 上缺失，chat 生成 700 token，completions 生成 40943 token，而 coupled 接口均停在 16。另有无需服务的 model_dump→model_validate 往返脚本：provided 标志从 True 变 False，即使反序列化数值仍显示 16。
- **修复与覆盖**：报告指出显式保留该键可恢复“已提供”的判定，但未给完整修复后的端到端回归。chat/completions 做了非流式生成；messages、responses 等仅检查 render，Cohere endpoint 返回 404，不属于验证成功。未测 streaming、n>1、Rust 或 gRPC。Claude 协助调查起草，人工审阅。
- **本简报建议**：跨服务契约测试必须经过真实序列化，覆盖默认值、相邻值、缺省与 null；手写一个包含 max_tokens 的请求不能覆盖这条故障路径。

[端到端脚本、序列化最小例与覆盖限制](https://github.com/vllm-project/vllm/issues/65388)

**4. Agent 工具调用：Hermes 流式解析把 arguments 后的字段也塞进参数**

**提交：11 月 3 日 06:03。** 报告发现流式／非流式路径对 JSON 字段顺序的假设不同：流式方法截取 arguments 后全部内容，遇到后续 name 或 id 时形成非法参数 JSON；并非模型一定生成了非法 JSON。

- **环境与步骤**：vLLM main 28dd9c36b5、Python 3.12、Ubuntu 26.04 WSL2、CPU torch；只需 Qwen/Qwen3-32B tokenizer，不需模型/GPU。原文完整脚本对合法对象 {"arguments":{"a":1},"name":"f"} 按 token 调用流式解析，再拼接参数增量。
- **判定器**：拼接结果应可 json.loads，且等于非流式提取的 arguments。实际多出 ,"name":"f"，报 Extra data；arguments 后带 id 的第二种结构同样失败。
- **作者修复验证**：本地改为 raw_decode 提取恰好一个 JSON 值，报告 11 种结构×200 种随机分块均与非流式一致，原测试 30 passed、1 skipped，并补两个顺序回归。所阅报告表示准备提交 PR，不能写成已合入；原文未披露 AI 参与，不将其归为 AI 自动发现。
- **本简报建议**：工具解析回归应同时变换字段顺序、token／网络分块边界，使用“流式重组等于非流式语义”作性质测试，而非只检查一段看起来合理的输出。

[纯 CPU 最小脚本、本地补丁与随机分块结果](https://github.com/vllm-project/vllm/issues/65242)

**5. MCP 工具发现：第一页正常，不等于全部工具都可被模型看见**

**提交：11 月 2 日 13:22。** vLLM MCPToolServer 只调用一次 list_tools，没有继续 next_cursor；第二页工具能直接调用，却不会进入向模型公布的工具集合。

- **环境与步骤**：源码 318c4db、MCP 2.3.0、Python 3.12.3、Ubuntu 24.04.5、PyTorch 2.15.0+cpu。原文完整脚本启动真实本地 SSE 服务，把两个工具放在两页，注册后读取 advertised 集合，再在同一 MCP session 手动翻页并调用 second_page。没有 mock MCP 响应；作者在三个新进程复现。
- **判定器**：服务可枚举、可执行的两项工具都应被发现。实际 advertised 只有 first_page，手动翻页能获得 second_page，调用 is_error=False。证据证明发现链路漏项，不是工具执行端坏掉。
- **修复状态**：所阅 issue 未给修复后测试；最新讨论有人表示“接近完成”，这不是交付或合入证据。没有 AI 参与披露。
- **本简报建议**：将两页以上工具列表、分页终止和既有工具过滤规则纳入契约回归，避免“模型不会选工具”的表象掩盖目录不完整。

[真实本地 SSE 复现、结果与当前讨论](https://github.com/vllm-project/vllm/issues/65074)

**二、AI 帮助测试：有局部证据，也要说清没有证明什么**

**6. Codex 辅助验证编译器对齐：最小内核复现成功，不冒充原始 AOT 故障复现**

**提交：11 月 3 日 19:33。** 作者原始场景是冻结常量视图的 AOT 推理图首轮执行失败。AI 辅助准备的报告进一步隔离：FP16 storage_offset=1 意味着 2 字节偏移，却被 Inductor 标为 16 字节对齐，Triton 因此产生不适用的向量 load。

- **复现环境与步骤**：官方 PyTorch 2.14.1+cu130（5c488690…）、Triton 3.8.0、Python 3.10.4、RTX PRO 5000 72GB Blackwell。完整脚本生成 arange(4097)[1:]，把 Inductor 实际 config_of 产生的元数据传给 Triton copy kernel，未手工注入对齐提示；前后在独立进程执行，同一 wheel 仅修改生产补丁。
- **判定器与前后结果**：偏移 2 字节的指针不应宣称 16 字节对齐；拷贝后 4096 元素应精确相等。补丁前生成 128-bit load 并报 CUDA 716，补丁后改为 16-bit load、精确相等。五个 FP16/FP32 offset 条件由 3/5 变为 5/5，已对齐的非零偏移仍保留对齐信息。
- **最重要的边界**：合成 AOT 模型在补丁前后都通过，不能宣称重现并修复原始 AOT 模型故障。当前 main 未源码构建、完整上游测试模块和原负载未重跑。PR #203207 明确是 Codex 协助整理作者既有修复；截止时开放、未合入，正文说明仍待维护者支持。
- **本简报建议**：AI 测试报告应分别列出原作者观察、独立最小验证、未复现部分；不能用“同样错误码”替代同一故障路径证据。

[最小复现与日志](https://github.com/pytorch/pytorch/issues/203214) · [候选 PR、前后验证及未覆盖项](https://github.com/pytorch/pytorch/pull/203207)

**7. SGLang Marlin 确定性：只关 atomic add，仍漏掉专家内 token 顺序**

**提交：11 月 3 日 19:14。** Claude Code 根据 DGX Spark 实测编写、按人设定规则发布的报告，将不确定性拆成两个来源：atomic 浮点累加与 moe_align_block_size 的专家内 token 排序。这里没有独立人工复核声明，不能与“人工逐项验证”混写。

- **环境与步骤**：GB10/DGX Spark，SGLang 0.5.17；内核测试在 0.5.23、torch 2.14.1+cu130、Triton 3.8.0 复测。完整脚本用随机 NVFP4 权重、256 experts、hidden 4096、top-6、bf16，固定输入重复 20 次；分别固定 token 顺序、关闭 atomic add、两者同时做。服务端另测 Inkling-Small-NVFP4 b6a99534、TP2/TP4、temperature=0，单请求且每次 flush_cache。
- **判定器与结果**：内核使用 torch.equal；单 token 仅关闭 atomic add 就可恢复重复一致，但 512/2700 token 还需固定排序。服务端即使开确定性标志，报告的 48 对输出无一完全一致；两项诊断修改同时应用后 TP2/TP4 均 48/48 一致。逐层 dump 把首处分歧定位到 NVFP4 专家层。
- **修复边界**：这是一份诊断补丁，不是按原样建议合入的生产修复。单流吞吐 23.8→23.4 tok/s、首 token 735→757 ms；未测负载下性能。作者明确并发下仍受 batch 组成影响，不能称实现 batch invariance。标题涉及 sm_90+，但本次实测是 GB10，未验证所有相关硬件。
- **本简报建议**：确定性 QA 分开测试“同输入同状态重跑”和“跨 batch 不变”；用逐层 dump 加两因素消融，避免修掉一种来源就宣布整体确定。

[完整内核脚本、服务端对照、诊断补丁与 AI 披露](https://github.com/sgl-project/sglang/issues/48150)

**本期判断与值得先做的两项**

1. **低成本优先补接口契约**：Hermes 的流式／非流式对照、MCP 两页真实服务、render 序列化默认值往返，都能在不跑大模型或只用小模型的条件下检出实质故障。先验证数据是否完整到达下一层，再归因模型能力。
2. **训练／推理回归分层设 oracle**：DTensor 检查接口与 placement，编译器检查实际元数据与输出，性能记录 allocator 外占用，确定性区分重跑与 batch。某一层通过不自动证明其他层健康。

**共同趋势**：AI 的价值在于辅助缩小故障、生成变形输入、运行局部对照和整理证据；本期没有独立统计证明它普遍提升测试效率，也没有证据支持“AI 已自主修复这些项目”。更值得复用的是清楚的判定器、真实序列化／分页边界，以及对未测范围的明确声明。

[本期归档](https://github.com/BruceShenC/News/blob/main/daily/2026-11-03/llm-agent-quality.md)
