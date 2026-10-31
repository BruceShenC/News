**大模型与 Agent 资讯｜2026年10月31日**

本期聚焦6个推理、训练与AI辅助测试案例，均于**10月30日首次公开报告**，不代表故障当天才发生。状态核验至北京时间09:39；涉及的修复PR仍开放、未合入。所有测试数字均为作者报告，**本期没有运行模型、脚本或实验**。与另两份技术热点、GitHub趋势简报去重，不为填栏目重复模型发布。

**一、测试 AI 系统：正确性、稳定性与检测盲点**

**1．Kimi-K3连续输出“!”：先区分NaN的产生与跨请求传播**

发现：混合评测和高KV占用下，部分输出变成token 0，logprob为null；一次运行516条中56条异常，AIME25 avg@4由0.883降至0.700。作者进一步做层级定位、故障注入与kernel差分，确认FlashKDA会把一个请求的NaN传播到同批其他请求；FlashMLA竞态是首个NaN来源的嫌疑，**尚未证明能解释全部自然故障**。

环境：vLLM PR #61867 head 063c0c6、Kimi-K3 revision f831ab66、2×8 H200、TP8×PP2、BF16 KV、prefix caching；FlashKDA 17a037d、FlashMLA 5dceadb、Torch2.14.0+cu130。

最小检出：无需完整模型，单H200运行报告内repro_flashkda_tail.py。两段变长序列取[1,700]，向第一段一行beta注入NaN，检查另一段输出与final state仍有限；[1,704]是16行对齐负控，[700,1]改为污染v。旧实现污染受害序列688–699行，补丁后清洁。原因是越界尾行“乘零屏蔽”不能消除NaN，改为直接选择零。

作者验证：新增隔离测试旧版8失败/2通过，补丁10通过；143种污染模式全部清洁，4,988个有限输入fuzz配置保持一致。另用200µs延迟生产者放大FlashMLA PDL竞态，50次中NaN由49次降至0。双补丁自然压力25分钟零事件只是有限观察，不能宣称消灭所有首发NaN；性能仅3次重复，也不能排除1–2%成本。

可迁移方法：**请求隔离不变量＋非有限值注入＋对齐边界负控**。完整模型压力复现脚本也已公开，包含长MRCR请求、排队和取消，不应只跑单请求文本一致性。
[故障与完整脚本](https://github.com/vllm-project/vllm/issues/64758) · [FlashKDA补丁及回归](https://github.com/vllm-project/FlashKDA/pull/16) · [PDL竞态脚本与限制](https://github.com/vllm-project/FlashMLA/pull/41)

**2．DeepGEMM：JIT门禁通过，线上请求仍可能首次编译**

发现：作者从生产超时排查，报告两个集群一周28次同类停顿，再比较请求前后DeepGEMM缓存目录。DeepSeek-V4-Flash、H200、DP4+EP、无持久JIT缓存；冷vLLM0.28约1k-token请求触发4–6次编译，停顿30–40秒。现有jit_monitor没有覆盖DeepGEMM的nvcc JIT，因而不能把其error模式通过当完整“运行期无编译”证明。

复测方案与结果：PR #64827在H200/DeepGEMM2.6.1上，用真实o_proj形状（8组、d1024、r4096）和新DG_JIT_CACHE_DIR，先预热max_tokens8192，再扫描真实运行路径M=1…4096。未预热新增9个kernel；补丁relax预热207个M值、耗时16.5秒，随后扫描新增0个。CPU测试文件test_deep_gemm_warmup.py报告14通过，覆盖检测、M集合、SM90/SM100布局及计数。

限制：GPU只验证SM90，SM100仅CPU布局测试；其融合RopeQuant布局及其他DeepGEMM入口仍可能遗漏。线上测量脚本原文称可提供，**并未给出完整生产复现包**；PR测试入口公开。建议把“预热后缓存新增数、首请求长尾、各rank进展、实际生成探针”联合验收，不能只看/health和启动成功；这是建议，不是本期已执行门禁。
[生产报告](https://github.com/vllm-project/vllm/issues/64824) · [补丁与验证](https://github.com/vllm-project/vllm/pull/64827) · [固定测试入口](https://github.com/vllm-project/vllm/blob/cab09647f46abe2237f524fb7e8691ceef75cae4/tests/model_executor/test_deep_gemm_warmup.py)

**3．raw_logprobs遇到结构化输出：返回的是原始还是约束后分布？**

发现：报告者在请求raw_logprobs时观察到被强制选择的token近乎确定，再做有/无grammar对照及源码追踪；最初业务场景未披露。报告也承认grammar mask不按普通logit processor实现，因此目前包含**接口语义争议**，不能当已获维护者确认的缺陷。

环境与最小请求：vLLM0.31.0、默认V2 runner、xgrammar0.2.7、Torch2.13.0+cu130、RTX4090；模型cyankiwi/Qwen3.8-27B-AWQ-INT4。向/v1/completions发送“The capital of France is”，max_tokens1、temperature0、logprobs3，并设structured_outputs.choice为单项“ not”；与不加约束的请求对照。

实际：“ not”分数约−0.052，top项也受grammar限制；无约束分布该token约−3.229、“ Paris”约−0.417。作者本地保留未mask logits供打分、仍用mask logits采样后，恢复预期原始分数且采样不变。无公开完整回归套件或合入修复。

价值：评测/置信度/RL数据契约应明确分数在grammar、温度、惩罚前还是后；仅核验输出符合JSON/choice并不能验收概率语义。这个数值差不是通用容差阈值。
[完整curl、环境与本地对照](https://github.com/vllm-project/vllm/issues/64809)

**4．verl：rollout和训练token一致，仍可能共同偏离聊天模板**

发现：在main 8e48d72d重放工具调用与用户追问，比较增量拼接和整段apply_chat_template。默认builder回退使GLM-4.7-Flash重复observation边界，Qwen3-Next漏掉结束标记后的换行。代码审查还发现tokenization_sanity_check_mode的旧实现已经失去调用者；配置写strict不等于检查真正执行。最初线上发现途径未披露。

复现/判定：按AgentLoopWorker相同参数创建builder，将assistant渲染截到模型stop token，合并下一轮，再对照完整模板。新检查锚定最后保留的运行时token，区分真实边界错配与模板主动改写更早历史，避免把所有think历史差异都误报为错误。相关CPU测试无需模型推理；真实tokenizer依赖其配置，完整Python/Transformers版本未披露。

PR #8463报告两文件184项CPU测试通过，另40项agent-loop测试通过。Qwen3-Next Instruct原8/8边界错配，注册Qwen builder后8/8匹配；GLM-4.7-Flash原2/2可渲染边界错配，注册GLM后2项匹配、6项跳过。**跳过不是通过验证**；DeepSeek-V3.1和Phi-4-mini全部8项跳过，不能称兼容已证实。

限制：这是首轮rollout前固定会话检查，不覆盖所有真实轨迹；作者承诺的端到端GRPO结果截至核查尚未见到。适合加为训练前token契约门禁，但不能代替真实轨迹抽检或训练收益验证。
[问题与模型矩阵](https://github.com/verl-project/verl/issues/8462) · [PR与测试报告](https://github.com/verl-project/verl/pull/8463) · [固定CPU测试](https://github.com/verl-project/verl/blob/d9d1f9165c550b3effc4aebc0fd5d94365a70273/tests/utils/test_continuous_token_check_on_cpu.py)

**5．PyTorch：编译后别名修改绕过检查，反向传播静默用错值**

发现证据：作者给出最小脚本和后端差分，披露Codex协助起草、人工验证；**不能据此说AI独立发现了缺陷**。环境PyTorch2.15.0（d0f2af8b）、Python3.14.10、macOS27.0.1、M3 Max，CPU和MPS均复现。

复现：weight=[1]，先计算需要梯度的x与weight乘积，再向torch.compile的mutate函数同时传weight及weight.detach()，对alias原地加1，最后对原loss求导。观察weight变2、版本变化0、梯度返回2。

Oracle：该用例故意修改反向传播所需值，正确行为是像未编译路径一样**抛出原地修改错误**，不是悄悄返回梯度1。CPU对照中uncompiled与eager后端版本加1并报错；aot_eager、aot_eager_decomp_partition和inductor均版本不变并返回2，定位范围早于Inductor生成代码。

尚无已验证修复或回归结果。可迁移方法是“值、别名关系、版本检查、异常行为”同时做eager/compiled差分，而不只比较正常输入前向输出；不能从此推断所有编译训练都会受影响。
[完整最小脚本与后端对照](https://github.com/pytorch/pytorch/issues/202806)

**二、AI帮助测试：有明确参与披露和硬件验证的案例**

**6．Claude Code辅助定位EAGLE3接收率退化：分位置指标比平均值更有用**

10月30日issue #64652及PR #64777，仍未合入。提交方明确披露claude-opus-5-5参与根因定位、补丁编写和测试，人工提交前复核；以下是提交方证据，不是独立第三方复测。

环境：vLLM nightly 3966924d5，1个4-GPU GB200节点、aarch64；MiniMax-M3-NVFP4 TP2，MiniMax-M3-EAGLE3-GQA草稿，3个投机token、FP8 KV。原issue有完整启动命令、6种编码prompt重复8次的48请求脚本，并发2、max_tokens1024、temperature1、top_p0.95；读取/metrics前后增量，计算各位置accepted_tokens/drafts。

发现：block-size128正常，256/512时第一位置正常、后两位置骤降。PR追到target使用128-token kernel pages，而drafter按512解读KV；首位置复用target元数据所以掩盖问题。补丁传递target的kernel block sizes。相同512配置，接收比例由0.83/0.16/0.03升至0.80/0.62/0.48，与128控制组0.80/0.63/0.48接近。

回归：新增单测2项通过，但旧版因不接受新参数失败，不能把这个负控等同真实硬件行为复现；GB200端到端对照才补足证据。小型Qwen3-8B+EAGLE3在H100原本正常，说明仅用小模型回归会漏掉异构target/draft布局。作者称影响提速而非输出质量，报告未提供独立的大规模质量等价性结果。
[发现及48请求脚本](https://github.com/vllm-project/vllm/issues/64652) · [根因、AI参与及硬件前后对照](https://github.com/vllm-project/vllm/pull/64777) · [固定单测](https://github.com/vllm-project/vllm/blob/8dac3ec9b648f770ef9c04889216fa8ad56706ef/tests/v1/worker/test_gpu_attn_utils_kernel_block_size.py)

**优先深入或隔离验证**

• **FlashKDA请求间NaN隔离**：单GPU、无需整模型，oracle明确，有对齐负控和修复前失败证据；适合扩展为变长batch边界与非有限值污染回归。
• **verl多轮token边界检查**：可先在CPU/tokenizer层建立训练前门禁，显式统计匹配、历史变化、边界错误和跳过，成本低于等训练收益异常后排查。

本期最值得迁移的是：**“服务成功返回”“监控无告警”“两条链路一致”都可能遗漏共同错误，需加入独立参考、边界扰动及负控。**上述验证建议未执行，也不构成直接升级或部署建议。

[本期归档](https://github.com/BruceShenC/News/blob/main/daily/2026-10-31/llm-agent-quality.md)
