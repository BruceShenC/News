# 大模型与 Agent 质量保障简报｜2026-11-17

**观察窗口：北京时间11月16日09:30至11月17日09:30。** 本期精选6项工程质量进展：5项修复或能力已合入，1项仍在评审。旧问题以本窗口的合入进展报道，不把合入日期当首次发现日期；合入主分支不等于进入稳定发行版。以下实验均来自公开作者报告，本次未运行代码。

## 1. SGLang：修复断连处理，不能只验“能取消”，还要验流式吞吐

**11月16日提出；截至核查仍为开放PR。属于测试AI系统，作者注明使用Claude Code辅助，但不是AI测试工具效果评估。**

- **如何发现：**夜间性能CI在多模型、GB200/GB300/B300上发现高并发流式吞吐下降，二分92个提交定位到逐chunk调用 `is_disconnected()` 的修改。GPU调度器吞吐基本不变，瓶颈在HTTP输出事件循环。
- **如何复现：**4张GB200、gpt-oss-120b TP4，固定nightly镜像与客户端，强制前缀缓存未命中；8000输入/1000输出、并发256、1280请求。对照提交 `f76ac26543` 与 `84685e7334`，交替启动服务器，每组4次测量。
- **判定与结果：**配对实验输出吞吐均值6654→8362 tok/s，提升25.7%；并发1的差异处于运行波动范围。不要与另一组6992→8894的实验混成一组。
- **回归：**同一等待窗口内8个输出只能探测断连一次，旧实现调用8次；超出窗口后必须再次探测。测试入口为 `test_tokenizer_manager_rid_cleanup.py`，作者报告62项通过、47个subtests。另用TCP RST检查三个流式端点，运行请求数在0.14秒内归零，但指标每40个decode step刷新，不能解释成精确取消时延。
- **限制：**修复保留默认4秒周期的轮询兜底，并依赖正常事件驱动断连路径；代理、ASGI实现改变时需重验。尚未合入，也不能称所有CI通过。

来源：[原始性能回归](https://github.com/sgl-project/sglang/issues/50722) · [完整配置、脚本和配对实验](https://github.com/sgl-project/sglang/pull/50741)。

## 2. vLLM Mooncake：P节点失效后，D侧KV不能继续被“无望请求”占满

**11月4日提出，北京时间11月17日03:17合入。属于测试AI系统。**

- **症状与定位：**P进程死亡后ZMQ静默重连，D侧等待回复并保留KV；死节点地址还被缓存，后续请求再次等待。PR关联早期挂起报告，未披露最初发现者如何定位，不将其称为生产事故。
- **故障注入：**2张A800、DeepSeek-MoE-16B、P/D各TP1、`kv_lease_duration=30`；128请求，负载达到D侧KV容量的2.4倍，约41秒时对P进程组发信号。
- **前后对照：**SIGKILL后旧版请求分别等待约545秒和1084秒，修复后2–4秒失败；D侧KV使用率降至50%以下由556秒缩至5.5秒。SIGSTOP冻结场景仍需12–102秒失败，不能与进程死亡混为同一恢复目标。
- **回归入口：**`pytest tests/v1/kv_connector/unit/test_mooncake*.py`。新增独立ZMQ子进程模拟死亡、冻结与未监听；作者报告750通过、5跳过，另4项Hub取数错误在重跑后通过。
- **代价：**短暂连接失败现在可能更早失败，而不是无限重试；验证应同时检查失败策略、地址重新发现及KV回收，不只检查错误码。

来源：[根因、故障注入和结果](https://github.com/vllm-project/vllm/pull/65575)。

## 3. vLLM：batch-invariant开关必须约束实际选中的量化kernel

**11月16日05:07提出，11月17日02:40合入，均为北京时间。属于测试AI系统。**

- **问题与检出：**开启 `VLLM_BATCH_INVARIANT=1` 后，MXFP8仍可能选择随batch大小改变行输出的kernel。公开证据是batch 1与batch N的logprob逐位差分；更早的发现过程未说明。
- **复现：**B200、Qwen3-0.6B-MXFP8，基线 `919d45fc3a` 对照 `18b921b1ba`，FlashInfer 0.7.3、humming-kernels 0.1.19，关闭前缀缓存。运行 `test_batch_invariance.py::test_logprobs_bitwise_batch_invariance_bs1_vs_bsN -k TRITON_ATTN`。
- **结果：**旧版32/32提示违反batch不变性、两个测试失败；修复后两个通过。选择器过滤不声明该能力的kernel，B200示例从FlashInfer切换到Emulation。
- **代价与边界：**示例加载显存0.73→1.14 GiB。GSM8K小幅变化不能证明准确率提升；本修订未重跑H100或Ampere/Ada，也不覆盖MXFP8 BMM、ROCm/XPU路径。

来源：[复现命令、版本和数值对照](https://github.com/vllm-project/vllm/pull/67806)。

## 4. vLLM ROCm：长上下文“跑得快”，可能是漏读了历史KV

**9月提出的旧问题，北京时间11月17日02:32合入。本期报告修复进展。属于测试AI系统。**

- **触发：**同批次中新prefill排在已有KV的续算请求前面，旧splitter把后者误分到不读历史的路径。公开材料给出needle检索、真实backend测试和漏读token统计，未完整披露最初发现过程。
- **复现：**单MI300X、固定nightly `96710245e`、ROCM_AITER_FA，Phi-4-mini、chunk budget 8192、并发16、关闭前缀缓存；`lm_eval niah_single_2`、30k上下文、500样本。
- **结果：**检索分数0.626→1.000；旧版漏读233,430个上下文token，修复后全部读取。因此该负载吞吐29,911→28,034 tok/s、TTFT 7.27→8.00秒，是恢复正确计算的成本，不能只按性能回归拒绝修复。
- **回归：**`test_rocm_aiter_fa_unordered_batch` 两例旧版失败、修复通过；另覆盖全decode但 `max_query_len` 过大的情况。32k恰好为chunk整数倍的Qwen对照两边都正确，说明只测整齐长度会漏检。
- **限制：**这些结果不等于所有ROCm模型、所有上下文长度都已验证；NVIDIA DCP路径另有结构检查，但该实验输出未见变化。

来源：[完整模型命令、测试入口及漏读计数](https://github.com/vllm-project/vllm/pull/59098)。

## 5. SGLang：MoE混合长度压测能抓到均匀输入不暴露的跨rank挂起

**北京时间11月17日03:00提出、07:46合入。属于测试AI系统。**

- **问题：**Kimi-K3、attention DP与MegaMoE组合下，不同DP rank的prefill长度不同，各rank按本地行数选择不同tile，最终combine超时。修复向kernel传递EP全局token数，并覆盖DP组内attention TP。
- **作者实验：**4张GB300，K3截断为40层，ADP4/EP4，nightly `8c7ec052` 外加idle-rank top-k前置修复；每轮4并发、64–8192输入token、8输出token。
- **判定：**旧版两次启动均挂起，修复后30/30轮完成。完整K3、8张GB300的另一实验进一步发现只修K3调用点仍会挂起，修改全局hint条件后完成。
- **限制：**PR给出了参数与结果，但未给出可直接执行的独立最小复现脚本，也未明确最初检出途径；已有CPU测试通过不替代跨GPU测试。前置修复不可遗漏。

来源：[根因、两阶段验证和配置限制](https://github.com/sgl-project/sglang/pull/50838)。

## 6. SGLang与RL：训练/rollout精度一致性要验配置传播，而不只看模型名称

**11月11日北京时间提出，11月17日02:52合入。属于测试AI系统的训练与推理衔接。**

- **问题：**DeepSeek-V4-Flash的训练器发送MXFP8 routed experts、1×32缩放块，但rollout端忽略配置，仍按128×128 block-wise FP8分配。修复只改变routed experts，其他线性层保持原格式。
- **检测与回归：**CPU测试断言loader传递标志、专家使用1×32块，同时共享配置不被污染；新增两例在无修复时失败。相关三个文件作者报告分别8、5、9项通过。原始发现途径未披露。
- **端到端证据：**8节点×4张GB300的Miles RL实验，在每次rollout前在线更新专家，跨恢复片段累计超过170步；观察到train/rollout logprob绝对差0.023–0.029。
- **边界：**该长跑使用较早的sglang-miles上的同一修改，不是当前上游修订的完整复现；该差值不是通用验收阈值，也不证明两端逐位一致。其他checkpoint的group配置仍可能需要额外处理。

来源：[配置错误、CPU测试路径及RL验证](https://github.com/sgl-project/sglang/pull/49591)。

## 优先小范围验证

1. **断连功能与吞吐联合门禁。**在并发1与高并发两端交替运行版本对照，固定客户端、缓存命中策略与输入长度；同时检查输出吞吐、ITL、取消后的资源释放。它能防止“可靠性修复牺牲服务能力”被单项测试掩盖。
2. **批次结构差分。**同一请求分别单独运行、混入不同长度请求、放在新prefill前后，再比较logprob、needle命中和KV读取状态。对PD另加死亡与冻结两类故障，分开定义释放时限。

以上两项为建议，未在本次执行。没有足够新增证据支持另列GUI自愈或用例生成产品发布，因此不凑栏目。作者披露AI辅助不等于证明AI测试有效性；已合入PR也不代表所有CI全绿或所有硬件验证完成。

## 归档

[本期完整简报](https://github.com/BruceShenC/News/blob/main/daily/2026-11-17/llm-agent-quality.md)。根README与daily/README共享索引仍等待此前写入限制的确认，本期未更新。

