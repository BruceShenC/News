# 10月21日 · AI 测试与推理／训练 QA 简报

日期：2026-10-21（Asia/Shanghai，北京时间 UTC+8）

归档说明：本文保留 2026-10-21 当期调研内容、来源和统计口径，不代表当前状态。仅将个性化称谓改为通用表述；未重新研究或执行项目。

今天重点是“表面成功、内部错误”：训练能跑但专家没梯度，缓存有记录但状态未写入，性能提升但长上下文答案仍错。精选7条，与技术热点、GitHub趋势分开；上游状态核对至北京时间约09:33，实验数据均为作者报告，未独立复现。

测试 AI 系统

1. MoE训练：专家梯度修复已合并，辅助loss归一化仍有候选问题
10月20日，Transformers [#49710](https://github.com/huggingface/transformers/pull/49710)合并，较此前报道有实质进展：修复特定旧torch版本专家并行通信缺少autograd支持，训练正常进行却没有expert／hidden-state梯度的问题。合并不等于已进入正式发行版。
同日新开的[#49769](https://github.com/huggingface/transformers/pull/49769)仍未合并，处理Qwen3-MoE主损失与router辅助损失混用DDP／梯度累积归一化；19项专用测试通过，但尚无真实多进程DDP集成验证。
可采用：除loss有限、下降外，检查应参与计算的expert梯度非None；固定逻辑batch与路由，比较单卡／多卡及不同累积步数下的CE、aux loss、梯度relative-L2和单步权重变化。

2. SGLang：Mamba checkpoint记录存在，不代表对应state已写入
10月20日提出的[#45435](https://github.com/sgl-project/sglang/pull/45435)仍开放。mixed-chunk prefill与decode混跑时，prefill追踪字段可能被清掉，缓存却仍捐出对应slot；后续前缀复用可能恢复旧state而静默错答。
可采用：冷请求与复用请求对照，交叉mixed-chunk及extra_buffer／extra_buffer_lazy策略；覆盖checkpoint边界前后、请求结束和slot回收再分配，核对“写入位置、checkpoint标记、捐出slot”一致。
限制：作者报告15项测试、96个subtests通过，但属于CPU逻辑回归，没有GPU端到端结果。

3. NVFP4：有效权重相同，scale表示变化也可能改变答案
SGLang [#45386](https://github.com/sgl-project/sglang/pull/45386)于10月20日提出，仍未合并。TRT-LLM的单一GEMM1 clamp与独立gate／up scales组合，可能改变up分支实际截断阈值；候选补丁在加载时统一有效alpha。
作者对一个公开模型做精确等价重参数化：旧实现输出从373 tokens变成95，候选恢复原文本与logprobs。
可采用：用不改变有效权重的2的幂次scale变换做变形测试，再覆盖graph replay、reload与不同分支。
限制：非二进制比例会引入FP8重新舍入，作者测到2.7%–3.2%的权重relative-L2变化，不能称为普遍无损修复；GPU实验也未在最新rebase后重跑。

4. 大batch＋推测解码＋结构化输出：需要按乘积扫整数边界
vLLM [#62790](https://github.com/vllm-project/vllm/pull/62790)于北京时间今晨09:21新开，仍未合并。MRV2 grammar mask kernel的int32行偏移，在logits行数×词表大小超过2³¹时可能溢出，造成越界写入或目标行未正确mask。
作者RTX4090边界测试中旧实现出现illegal memory access，int64候选通过相关测试。
可采用：按“logits行数×词表大小”扫描边界，而不只扫描请求数；同时断言内存安全与允许token集合。
限制：尚无真实服务大batch端到端结果，测试自身也有较高显存需求。

5. 长上下文：吞吐改善与信息可见性必须分开验收
10月20日23:50 UTC的[vLLM #62776](https://github.com/vllm-project/vllm/issues/62776)报告：双DGX Spark／GB10、Qwen3.8-Flash-Next NVFP4、带本地补丁的nightly上，长提示中的定位任务失败；若干补丁把decode从50.7提升到62.5 tok/s，却未解决症状。
可采用：围绕稀疏索引budget及倍数，在不同位置放随机标记与已知答案，测命中率、位置偏差及greedy重复一致性；不要只看prompt_tokens计数和短提示smoke。
限制：根因未确认，不能据此断言所有v0.32都会截断，或已证明模型只能看到前2K。编号计数也不是精确oracle，需要独立探针对照。

AI 帮助测试

6. TESSARA：把浏览器操作变成能重放、能断言的测试
论文10月19日提交、20日进入arXiv公告，属于近7日研究。生成带断言和步骤来源的Playwright脚本，只修复失败后缀、保留有效前缀。
作者在WebVoyager报告满足oracle的脚本比例59.34%，SeeAct为27.49%；但使用GPT-4V等旧模型比较，不是与当前最强Agent对比。150例人工检查中，语义验证器仍有7%误报。
可采用：选10条预发流程，从干净状态各重放5次，再注入已知UI／行为错误，记录检错率、假通过率、修复token及人工修改时间。[论文与实验](https://arxiv.org/html/2610.25601v1) · [代码](https://github.com/webuitestingagent/tessara)

7. 缺陷定位不一定能提升修复Agent：诊断成本也要计入
10月19日提交的新研究比较6种定位方法、3个模型、16,248条修复轨迹。18组设置中，加入定位让pass@1变化为−4.45至+0.40个百分点，其中17组下降。
价值：不应默认“先跑一遍定位，再让Agent修复”总是更有效；定位成本可能超过节省的修复token。
可采用：历史缺陷上对比“不定位／总是定位／失败后才定位”，统计包含诊断的每个成功修复总费用，并按原始报告是否已指明文件分类。
限制：每个任务配置只运行一次，使用共同修复框架，不能外推到所有交互式定位方案；文中的最大39.2%节省使用理想位置，不是生产工具已实现收益。[论文](https://arxiv.org/html/2610.25826v1)

建议优先验证
- 推理／训练侧：加一组“不变量门禁”，覆盖固定逻辑batch下梯度一致性、冷请求与缓存复用一致性、有效权重等价变换。比只检查服务成功和loss下降更容易暴露静默错误。
- GUI Agent侧：把“任务完成率”拆成脚本可重放、断言正确、注入缺陷可检出三层，再评估自愈，避免修复后只是更容易通过。

此前讨论的probec相关候选#45049尚未核到新的已发布修复，本期不重复原有单测证据。
