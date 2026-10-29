# 大模型与 Agent 资讯｜2026年10月26日
重点：缺陷如何发现、复现与检出

日期：2026-10-26（Asia/Shanghai，北京时间 UTC+8）

归档说明：本文保留 2026-10-26 当期调研内容、来源和统计口径，不代表当前状态。仅将个性化称谓改为通用表述；未重新研究或执行项目。

本期5个10月25日工程案例＋1项近7日AI辅助测试研究。工程修复目前均未合入，测试数字来自作者公开记录，我未执行；模型发布及项目趋势已在另外两份简报展开。

一、测试AI系统：推理与训练质量

1. vLLM：正文隐藏EOS，logprobs却多出一个token
发现：[Issue #63591](https://github.com/vllm-project/vllm/issues/63591)对照API输出，发现正文“OK.”之外仍返回结束标记的logprob；最初业务发现途径未披露。
复现：nightly c50aca144、CPU、Qwen3-0.6B，temperature=0、关闭thinking，要求只输出“OK.”并返回logprobs；比较可见正文与logprob序列。
验证：[PR #63607](https://github.com/vllm-project/vllm/pull/63607)用约束采样固定“A＋EOS/显式stop”，覆盖chat/completions、流式/非流式和length结束。新增26项用例修前12失败；作者报告修后Python completion套件51通过、Rust实模型43通过。验证使用RTX3080、float32、eager，原始token IDs和usage应保持不变。
入口：PR中的test_completion.py及logprobs_match_visible_output测试。PR声明Codex辅助，但不代表自动验证了全部语义。
可迁移：把“正文、token、logprob、结束原因、usage”的对应关系纳入接口契约，而非只判断回答文字。

2. SGLang：固定输入，混合调度仍使greedy输出翻转
发现：[Issue #46446](https://github.com/sgl-project/sglang/issues/46446)通过双请求调度差分检出；原始业务发现途径未披露。
环境：main a99c7d050、Qwen3.5-9B、RTX PRO 6000 Blackwell、TP1、BF16＋FP32 LM head、Triton attention、关闭CUDA Graph、chunked prefill=2048、seed=42、temperature=0，并启用确定性。
复现：4118-token请求生成128 tokens，开始流式后加入255-token请求生成8 tokens；分别冷启动，开/关mixed-chunk。Issue附件提供launch.py、repro.py及精确输入token IDs。
判据：前81个输出一致，第82个变为41228与8129；main及叠加候选补丁各3/3复现。
边界：没有prefix-cache命中，不能归因于昨天的缓存案例；唯一数值根因尚未确认，也未证明哪份答案事实错误。需确认该混合模型是否属于确定性承诺范围。

3. HunyuanImage-3：权重正常，算子原地写却把图片变成噪声
发现：[SGLang #46461](https://github.com/sgl-project/sglang/issues/46461)先出现噪声图，再对照官方实现与真实checkpoint做FP32逐层比较。attention余弦接近0.99999，MoE第31层却只有0.166；router、top-k及权重一致，最终定位到FusedMoE覆盖输入，后续shared MLP读错数据。
复现：main ee98bb0、2×H200、CUDA13、TP2、HunyuanImage-3.0-Instruct，1024×1024、50步、guidance=2.5、seed=42，蓝色茶壶提示。先执行shared MLP后，作者恢复了正常茶壶图。
验证：[Draft #46454](https://github.com/sgl-project/sglang/pull/46454)报告9项CPU测试通过；撤回算子顺序和时间步两处修复，恰有2项失败。入口test_hunyuan_image3_tp_broadcast.py；逐层调试插桩未随PR公开。
可迁移：权重、shape一致不够，还要检查别名、原地写及算子顺序；另一硬件后端通过不能代替CUDA路径验证。

4. Megatron：高置信token的Loss和目标梯度错误归零
发现：[Issue #8476](https://github.com/NVIDIA/Megatron-LM/issues/8476)给出FP32与FP64数值反例，并引用softmax-collapse研究；未披露是否最初来自线上训练。
最小复现：main 2052769、PyTorch2.8.0，logits=[20,0,0]、target=0。FP32实现得到loss=0、目标梯度=0，但其他类别梯度仍约2.06×10⁻⁹；FP64参考loss约4.12×10⁻⁹、目标梯度约−4.12×10⁻⁹，行梯度和应为0。Issue含CPU单rank替身脚本。
验证：[Draft #8477](https://github.com/NVIDIA/Megatron-LM/pull/8477)扫描margin 5–80，并把目标与最大非目标logit分置不同rank；作者报告1GPU由3失败/10通过变为13通过，4×L4跨rank检查由全部失败变为全部通过。测试入口test_cross_entropy.py、test_fused_cross_entropy.py。
边界：8GPU、Transformer Engine路径及完整训练收敛尚未验证。值得借鉴的是同时核对loss、目标梯度与梯度守恒，而非只看Loss曲线。

5. verl：评分器给了奖励，训练却收不到
发现：[PR #8368](https://github.com/verl-project/verl/pull/8368)用真实SWE轨迹及单测展示问题；首次发现过程未披露。
触发：多轮轨迹末尾有聊天模板后缀，最后token的response_mask=0，奖励却被放到这个位置，GAE、REINFORCE++等会忽略它；GRPO/RLOO不受此问题影响。
复现：Qwen3-4B-Instruct-2507 tokenizer＋TrajectoryReconstructor，gamma=lam=1、V=0。原版把1分放在第51个换行token，首token return=0；移到最后一个有效生成token后return=1。
验证：test_agent_loop_extra_fields_schema_on_cpu.py由2失败/19通过变为21通过，6个CPU测试文件共42通过；逐一撤回三处修改都会使新测试失败。
边界：没有GPU训练验证。应检查“评分→token归属→mask→优势估计”全链，不能以评分器输出正常作为训练正确性的依据。

二、AI帮助测试：从业务规则生成Agent测试

6. AgenTest：任务完成了，也可能越权或违反业务规则
10月20日预印本。从系统提示、工具定义与领域知识挖掘规则，转换成Given–When–Then，构造环境和模拟用户，再检查整段消息、工具调用及状态变化。判定结合结构化断言与LLM判断。
作者在tau2-bench的航空、零售、通信三个领域生成551条细化规则；对两个模型分别确认89、121类规则违规，按规则去重，不是生产缺陷数量。约60%的触发运行仅看最终状态会漏检。
重要限制：规则人工核验正确率约88%，所有计入结果的违规均经两位作者独立审查；不能把LLM猜出的行业惯例直接当强制需求。本文未核验到可直接运行的完整工具发布。
来源：[论文](https://arxiv.org/abs/2610.28116)；[方法、人工确认与实验](https://arxiv.org/html/2610.28116v1)。

建议优先小范围验证
- 低成本接口门禁：先借鉴vLLM案例，检查可见输出与logprobs等字段的语义对应关系，固定采样结果可减少模型波动干扰。
- Agent场景质量：借鉴AgenTest，将已由业务确认的权限、确认步骤和操作顺序写成规则，同时验证中间调用与最终状态。不要直接把自动挖掘规则当成真值。
