# 大模型与 Agent 资讯｜2026年10月27日
重点：用什么证据发现问题，怎样证明测试能检出

日期：2026-10-27（Asia/Shanghai，北京时间 UTC+8）

归档说明：本文保留 2026-10-27 当期调研内容、来源和统计口径，不代表当前状态。仅将个性化称谓改为通用表述；未重新研究或执行项目。

本期5个10月26日工程案例，覆盖推理、训练及AI辅助代码审查。相关Issue/PR仍开放；以下测试均为作者报告，我未执行，也不将候选修复说成正式上线。

一、测试AI系统

1. vLLM：大批量prefill触发int32偏移溢出
发现：Qwen3.5启动崩溃后，作者逐层定位，并以PyTorch参考值检出另外的静默污染；是否最初来自生产未披露。三个Triton kernel用int32计算token×stride，超过边界后地址偏移错误。
复现：main ffcabb8c7a、Qwen3.5-9B、单B200、Torch2.14/cu132、Triton3.8。[Issue #63759](https://github.com/vllm-project/vllm/issues/63759)含独立repro.py，分别测试209716、209717、263168个token。209717时q/k/gate最大误差从约0.0078/0.0078/0跃至5.73/4.84/4.80；int64补丁恢复参考误差。
验证：[PR #63761](https://github.com/vllm-project/vllm/pull/63761)新增三个偏移边界测试，旧树全失败、补丁全通过；原有126项测试在两树均通过。入口涉及test_fused_qk_norm_rope_gate.py、test_checkpoint_exporter.py、test_fused_gdn_post_conv.py，PR列出完整命令。
借鉴：围绕整数边界造数据，并保留新测试、仅撤回修复做负控。索引修好不等于超大预算不再OOM。

2. SGLang：HTTP 200也可能返回大量NaN
发现：[Issue #46572](https://github.com/sgl-project/sglang/issues/46572)从崩溃和NaN logprob追踪，用device-printf看到CUDA graph填充行将非有限V写入KV slot 0，再主动毒化padding做故障注入；部分下游读取解释仍属源码推断。
环境：单H200、v0.5.22-cu130、Inkling-NVFP4的test revision、FA4＋Marlin、TP1、full prefill graph、page size=1。Issue提供启动参数及[文本/图片脚本](https://gist.github.com/TheDuyIT/62301bdcd0480ea81feb6815147f51a4)，max_tokens=2000跨过512-token滑窗。
结果：GUMBEL=0时第14请求死亡，重复2轮；默认GUMBEL=1虽HTTP200，2000个logprob却有1065个NaN。跳过padding写入后，作者报告一轮70请求通过。
验证：[PR #46577](https://github.com/sgl-project/sglang/pull/46577)要求空槽不变、真实KV位级一致且有限；新增2测撤回修复后全失败，补丁全通过。没有独立准确率或速度结论。
借鉴：健康检查加入有限值与缓存不变量，不只看HTTP状态。

3. Gemma4在线FP8：文本正常，图片请求让引擎退出
发现：用户做版本与模态对照，vLLM0.22文本/图片均正常，0.23起图片500、文本仍可用；0.22、0.23、0.32均重跑最小复现。
复现：[Issue #63732](https://github.com/vllm-project/vllm/issues/63732)使用RTX5080 16GB、Gemma4-E4B-it、在线FP8、max-model-len=4096、eager，提供64×64白图、16-token输出的curl。
定位：视觉投影权重转FP8后，HF forward把像素也转FP8，再量化时被拒；BF16对照正常。但vLLM与Transformers同时升级，尚未确定引入问题的具体变更。
边界：评论者在B300、main 0f6b2619上，以忽略vision_tower.patch_embedder.input_proj量化的配置成功回答“White”。这是绕过验证，不是源码修复，也未验证5080、默认graph/compile或视频音频。
建议：建立“版本×模态×量化×执行模式”矩阵；这项矩阵是迁移建议，不是已完成验证。

4. verl：极小KL误差，足以翻转训练样本是否保留
发现：[PR #8386](https://github.com/verl-project/verl/pull/8386)用数值参考及回归测试暴露消减误差，首次业务发现途径未披露。FP32小log-ratio=-1e-4时旧表达式出现负KL，32768-token累加后，在seq_sum_k3=1e-4阈值下错误保留应拒绝序列。
修复改用expm1(x)-x；以FP64小值参考检查正负小值、梯度、四种拒绝模式、padding及长序列。
验证：20个新增CPU用例旧实现8失败/12通过，补丁20全过，相关套件73通过。入口为test_core_algos_on_cpu.py、test_rollout_corr.py、test_rollout_corr_integration.py，命令在PR。
限制：Windows11、Python3.12.13、Torch2.13.0+cpu；没有Linux CI、GPU及真实训练验证。
借鉴：同时检查“数值误差”与“业务阈值决策”，小误差不一定小影响。

二、AI帮助缺陷分析，同时测试分布式同步

5. verl：AI审查发现异常路径，随后补成故障注入测试
10月26日[PR #8395](https://github.com/verl-project/verl/pull/8395)新增分片NIXL权重同步。公开CodeRabbit静态审查指出：发送中途失败，接收端可能一直等待末桶；作者修补后，再次审查发现buffer初始化失败仍漏报，继续补修。
触发包括发送迭代器异常/取消，以及staging buffer初始化OOM。测试入口test_bucketed_weight_transfer.py::test_sender_failure_aborts_receiver及buffer分支；作者称分别能让旧实现、前一提交失败。
边界：没有公开这两项修补后的独立运行汇总，不能拿整体套件通过数替代。整体测试用B300、NIXL1.4.1、vLLM0.24.0；项目固定的0.32.0尚未本地实测，fully_async另有已知限制。
借鉴的是“AI审查疑点→人为核实→异常注入→回归断言”，不是让AI口头宣布代码正确。

建议优先验证
- 数值与缓存门禁：从padding毒化、有限值检查、整数边界入手，容易补足常规正常输入覆盖不到的路径。
- 分布式生命周期：明确规定发送失败或取消后，接收端必须在有界时间退出；覆盖初始化、中途传输和收尾三个阶段，并验证重试不会被旧任务卡住。后一项是建议的测试契约。
