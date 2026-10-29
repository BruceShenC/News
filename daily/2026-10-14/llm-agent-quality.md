# 10月14日｜大模型与 Agent 简报

日期：2026-10-14（Asia/Shanghai，北京时间 UTC+8）

归档说明：本文保留 2026-10-14 当期调研内容、来源和统计口径，不代表当前状态。仅将个性化称谓改为通用表述；未重新研究或执行项目。

今天优先看三类质量风险：量化后的静默数值错误、多卡推理死锁、训练样本边界污染。以下区分问题复现、待合并修复和已合并代码；上游测试结果不等于我们已完成验证。

1. 推理正确性：NVFP4 KV-cache 布局问题获独立复现
10月13日，vLLM 的一个特定集成路径新增算子级复现，发现物理 stride 与数据／scale 地址重叠可能导致静默数值损坏。
可借鉴：量化回归加入真实内存布局下的写入→读取校验、地址不重叠检查，再跑短题正确率和长上下文检索。问题仍开放，不能泛化为所有 vLLM NVFP4 服务都有故障。[复现记录](https://github.com/vllm-project/vllm/issues/61225#issuecomment-6138274791)

2. 推理可靠性：MoE LoRA＋CUDA Graph 死锁有新验证
10月13日，报告者在双 A100、TP=2、fully-sharded LoRA 配置下测试：原版1,000请求全部超时；关闭共享专家辅助 stream 或采用候选补丁后，1,000请求全部完成。
可借鉴：把 CUDA Graph、TP、LoRA 和辅助 stream 的组合加入兼容矩阵，检查完成率与超时。修复 PR 仍开放，成功完成请求也不代表输出精度已验证。[测试结果](https://github.com/vllm-project/vllm/issues/60845#issuecomment-6139756869)

3. 训练质量：packed sequence 的 fallback 可能跨样本污染
Transformers 10月13日推进一项待审修复：依赖缺失时，参考计算路径可能丢失序列边界，让卷积或循环状态跨文档传播；候选方案是不支持时明确报错。
可借鉴：对比“后一段单独运行”和“打包运行”，再改变前一段，检查后一段结果是否受影响。还要覆盖依赖缺失和 kernel fallback。PR 尚未合并，不能只凭 loss 正常下降判断训练正确。[问题与复现](https://github.com/huggingface/transformers/issues/49472) · [候选修复](https://github.com/huggingface/transformers/pull/49474)

4. 混合精度：Marlin／MTP 后端选择修复已合并主线
vLLM 10月13日合并修复，使 NVFP4 主模型使用 Marlin 时，BF16 MTP draft MoE 可以选择适合自己的非量化后端，避免继承设置导致启动失败。
可借鉴：分别核对主模型、draft 和跳过量化层的实际后端，并检查 draft／accepted token 计数。已合并 main，不代表已进入稳定发行版。[合并与验证](https://github.com/vllm-project/vllm/pull/60219)

5. AI辅助测试：先审查需求映射，再让 Agent 生成测试
MathWorks 10月13日发布实操文章，展示用 Codex skills 将 EARS 需求映射到 Simulink 模型，再经人工审查生成测试序列草稿。
值得借鉴的是把审核放在“需求如何变成断言”这一步。它是新文章，不是当天新发布的工具；生成文件、编译成功都不能替代测试执行与覆盖分析。[官方演示](https://blogs.mathworks.com/deep-learning/2026/10/13/jump-into-test-authoring-straight-from-requirements-using-an-ai-agent/)

6. 模型生命周期：部分 GPT-4 系列 API 将下线
OpenAI 官方页面已列出10月14日公告：指定 GPT-4o、GPT-4o mini、GPT-4.1 和 GPT-4.1 mini 快照将于2027年1月14日停止服务，相关微调版本同步。
建议先清点实际模型 ID，再做替代模型的结构化输出、工具调用、业务正确率及延迟回归，不只验证接口能通。[官方下线清单](https://developers.openai.com/api/docs/deprecations/)

今天最值得小范围验证的两项：
- 推理侧：增加 KV-cache 写读一致性测试，覆盖真实 stride 和量化 scale 布局。
- 训练侧：增加 packed sequence 边界隔离测试，尤其覆盖依赖缺失后的 fallback。

这两项都能补上“服务能跑、指标看似正常，但结果已错”的盲区。
