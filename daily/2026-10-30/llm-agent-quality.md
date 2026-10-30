**大模型与 Agent 资讯｜2026年10月30日**
重点：推理与训练质量保证。以下6项均为10月29日的新公开报告/PR，截至本次核查仍非正式已发布修复。测试数字来自作者报告或提交的测试代码，**本简报未自行运行**。

**一、测试 AI 系统：推理正确性与稳定性**

**1．SGLang：确定性模式、输出相同，logprob仍会因缓存路径改变**
日期/状态：10月29日，issue公开复现包与本地候选补丁，尚无该候选的远程PR。
发现：作者做串行/并发差分，发现输出token相同但原生logprob不同，再替换缓存状态定位：Decode产生的GDN recurrent state被复用为Prefill state。最初进入调查的线上/实验背景未披露。
环境：Qwen3.5-9B revision c202236235762e1c871ad0ccb60c8ee5ba337b9a，RTX PRO 6000 Blackwell/SM120、TP1、BF16，LM head/SSM state FP32；Python3.12.13、Torch2.14.1、CUDA13.0、Triton3.8.0。确定性模式，Mixed、CUDA Graph和投机解码关闭，extra_buffer缓存、tracking interval64、chunked prefill2048。
复现入口：包内恢复冻结源码和依赖，再各起全新Engine运行concurrency=1/2，temperature0、seed42、输出24token；按token ID对齐selected/top5 logprob。其基线是#46525 head加五项前置修复，**不是普通main**。必须使用包内255/257-token fixture，不能任意换同长度prompt。
结果：三对基线每对117/119共同token分数不等，一处top5集合不同；候选补丁后三对0/120；撤回补丁后两对恢复117/119。step0/token198分数由两路径−4.51172876/−4.49568176体现差异。补丁保留192-token兼容Prefill checkpoint，避免复用256-token Decode checkpoint；另测cold reference与正常warm reuse。
价值/限制：证明仅对比文本或提高状态保存精度不够；存在额外65-token prefill成本，TP>1、图模式、其他硬件未覆盖。
[原报告](https://github.com/sgl-project/sglang/issues/47213) · [完整复现包](https://github.com/user-attachments/files/34115988/reproducer.tar.gz)

**2．vLLM：RL rollout中的首token分数异常，追到前缀缓存bounded replay**
日期/状态：10月29日issue，尚未确认修复。
发现：每prompt采12个response时，11个命中共享前缀，rollout与trainer的logprob异常按prompt聚集，常在首response token。作者用冷Prefill对已采样response重评分，发现异常更接近Decode路径。
环境：vLLM0.32.0、RunnerV2、8×B200/TP8、CUDA Graph、DeepSeek-V4.1-Flash BF16转换权重、FLASHMLA_SPARSE_DSV41、raw_logprobs。Torch/CUDA版本、转换细节未披露。
入口：公开脚本读取model和rows.pt，开启prefix caching、repeats12，对照禁用SWA bounded replay；参考Prefill请求使用独立cache_salt。**实际69 prompts的rows.pt未提供下载，精确数值复现仍缺输入。**
作者结果：默认replay在671,571 tokens中52个绝对分差>1，最大2.81；禁用后675,353 tokens中14个>1，最大1.94；关闭缓存约同量级。两次token数不同，不能当完全配对测试；>1是诊断分桶，不是行业阈值。
价值/限制：应看尾部误差及首token聚集，而非只看平均值；禁用replay仍有基线误差，属于绕过/精度权衡，不等于修复或bitwise一致。脚本可选patch开关在报告中未明确，不能自行补参数。
[原报告](https://github.com/vllm-project/vllm/issues/64584) · [差分脚本](https://github.com/avigyabb/SkyRL/blob/dsv4-flash/examples/train/deepseek_v4_flash/dev/decode_vs_prefill_v41.py)

**3．SGLang：容量压力触发retract，采样惩罚历史被丢失**
日期/状态：10月29日issue及PR，补丁未合入。
发现：强制retract/no-retract的HTTP输出差分，结合代码检查，发现output_ids仍在但新penalizer没有重放历史。最初是线上还是主动测试发现，未披露。
复现：main fd5c52588、CPU、Qwen2.5-0.5B-Instruct；greedy、max_new_tokens64、ignore_eos=true、frequency_penalty2，要求列不重复英文词。参考不中断；实验请求中调用pause_generation(mode=retract)再continue_generation，比output_ids；penalty0为控制。
作者结果：惩罚组3/3从回收点分叉，零惩罚组3/3一致。自然压力用max-total-tokens1024、估计clip128、8并发、每条224tokens，5批15个被回收请求全部分叉，oom_kill未变；零惩罚组仍偶见CPU argmax tie。
补丁/回归：重放frequency/presence/repetition/min_new_tokens历史；新增测试比批量重放与逐token累积、EOS阈值解除、无penalizer noop。相关两文件作者报告48 passed、2 skipped。
**验收缺口：未跑server E2E/GPU；PD prepare_for_prebuilt未覆盖，PR有CI红叉但原因未核实。**不能把单测通过写成端到端已修复；原运行故障仅frequency实测，其他三类来自代码检查。
[Issue](https://github.com/sgl-project/sglang/issues/47245) · [PR](https://github.com/sgl-project/sglang/pull/47252) · [固定测试入口](https://github.com/sgl-project/sglang/blob/45aa029e5330a0f60cc2c5f2a6ef9f838e36f168/test/registered/unit/sampling/test_penaltylib.py)

**二、训练质量与 AI 辅助测试**

**4．verl：融合LM头漏掉模型缩放，logprob错误乃至梯度全NaN**
日期/状态：10月29日PR #8441，未合入。
发现证据：源码路径分析、fused/native差分及真实checkpoint验证；最初发现途径未披露。通用fused forward绕过模型forward，遗漏Granite、Cohere、Falcon-H1、HyperCLOVA X的post-lm_head缩放及bias处理。
最小复现：PR完整脚本随机小Granite，hidden128、2层4heads、vocab2048、logits_scaling8，float32/eval，LM头权重乘40，输入1×16、temperature0.7；原生log_softmax对照torch fused backend。最大logprob差51.74467468→0。
真实验证：H100上Granite-4.0-350m、Falcon-H1-0.5B-Instruct，修复后与原生最大差2.47e−5 nats；Falcon Triton路径LM头梯度非有限元素33,570,816/33,570,816→0。完整依赖版本及真实输入全文未披露；观察差值不是通用验收阈值。
补丁把缩放折入温度，遇bias显式拒绝。59项CPU测试通过，新增10项旧实现失败/补丁通过，另1项H100测试通过。**CPU新增用例mock kernel，只证明温度折算和拒绝行为，不是Triton数值验证。**Megatron路径、softcapping及部分VLM wrapper未覆盖，也无完整收敛收益。
[PR与复现](https://github.com/verl-project/verl/pull/8441) · [固定测试文件](https://github.com/verl-project/verl/blob/07fbd6f0d9bc4f7aed6c7f88931cd0ba60647ba2/tests/models/test_fused_kernels_ulysses_sp_on_cpu.py)

**5．AI帮助测试的具体案例：答案只加Markdown，奖励却由+1变−1**
日期/状态：10月29日verl issue #8449/PR #8450，未合入。
发现/AI参与：提交方披露MiniMax-M3经ZCode定位Minerva答案提取缺陷、写补丁和回归用例并运行CPU测试；属于提交方报告，无独立第三方重跑证明。
复现：issue基线a08ea02a、Python3.12.13、CPU，无需模型/GPU。对ground truth42调用compute_score，输入加粗“Answer:”或加粗答案42、带空行标签，旧实现抽出强调标记，score=−1/acc=False；纯文本得+1。
Oracle与负例：正确格式变体须score1、acc=True、pred42；错误答案仍−1，并保护x_1下标、乘法和boxed路径。补丁只去外围Markdown。
作者回归：PR checkout dc709afb、pytest9.1.0，21新增用例旧实现14失败7通过，补丁21通过；联合reward-manager 39通过。固定测试文件可直接用python -m pytest -v运行。
价值/限制：这是可迁移的**格式变形测试＋错误答案负例**；只覆盖Minerva提取，不代表全部数学等价判定可靠，无RL学习曲线证据。
[Issue](https://github.com/verl-project/verl/issues/8449) · [PR](https://github.com/verl-project/verl/pull/8450) · [固定测试入口](https://github.com/verl-project/verl/blob/e8e82c2fc349c64e199392eaf6f7aaf79d185d42/tests/utils/reward_score/test_math_dapo_minerva_markdown_on_cpu.py)

**6．PyTorch NCCL2：GPU利用率100%，异步barrier却没有进展**
日期/状态：10月29日issue #202500，未提供已验证修复。
发现证据：最初发现过程未说明；公开证据包括最小脚本、跨版本/后端差分及hung-rank栈，报告者称11个GB200节点均复现。
环境/入口：单节点4×GB200、aarch64、Python3.14，Torch2.15.0+cu134/CUDA13.4/NCCL2.30.7；另测2.14.1+cu132。issue附repro.py，以torchrun --nproc-per-node 4运行，每rank执行5轮barrier(async_op=True).block_current_stream()后CUDA synchronize，60秒faulthandler退出输出栈。
Oracle：应20/20轮完成。2.15默认NCCL2仅1–2/20后挂起；2.14.1显式NCCL2为0/20；两版本legacy均20/20。NCCL2的barrier.wait()和all_reduce+block_current_stream也能通过，限定问题为特定API组合。
报告者归因：barrier忽略asyncOp，host-blocking work与StreamBlock自旋/取future时同步形成死锁；有源码定位，但暂无修复前后验证。
建议纳入“collective×异步API×后端”的升级测试矩阵，监控step/完成计数及超时，而非仅GPU利用率。**此门禁是简报建议，不是已执行结果。**
[完整报告、脚本与源码定位](https://github.com/pytorch/pytorch/issues/202500)

**优先小范围验证**
• 奖励打分器格式变形测试：CPU即可验证、oracle清楚，旧代码失败负控完整。
• 缓存状态等价性与retract扰动差分：把“输出token相同”与“logprob/采样历史一致”分开验收，并保留冷参考及撤回补丁负控。

[本期归档](https://github.com/BruceShenC/News/blob/main/daily/2026-10-30/llm-agent-quality.md)
