# 大模型与 Agent 资讯｜2026年10月25日
重点：问题如何发现、如何复现与检出

日期：2026-10-25（Asia/Shanghai，北京时间 UTC+8）

归档说明：本文保留 2026-10-25 当期调研内容、来源和统计口径，不代表当前状态。仅将个性化称谓改为通用表述；未重新研究或执行项目。

本期6条都有具体检测或复现证据，按发现、复现与测试证据展开。以下测试结果均来自作者公开记录，我未执行；合入main也不等于已进入正式发行版。

一、测试AI系统

1. 训练Loss正常，梯度却被张量并行放大
10月24日，NVIDIA Model Optimizer [#2988](https://github.com/NVIDIA/Model-Optimizer/pull/2988)提出修复，尚未合并。Top-K KL蒸馏在各TP rank重复计算全局Loss，通信反向传播使学生梯度放大为TP倍，前向Loss却仍正确。
发现：最初发现途径未公开；检出证据是分片梯度与完整词表参考实现逐项比较，而非只看Loss。
复现：PR中的test_distill_megatron.py，分别用1、2、4张GPU，覆盖Top-P截断、非默认温度、反向KL。作者在A800/NCCL报告39项通过；恢复旧实现后，TP=2的3个梯度参考用例全部失败。
借鉴与边界：前向值一致、反向梯度一致应分成两道门禁；尚未验证完整训练收敛，不能外推最终模型质量。

2. INT4 KV缓存：奇数行形状引发污染和偶发崩溃
10月24日，vLLM [#63514](https://github.com/vllm-project/vllm/issues/63514)报告问题，仍开放。环境：vLLM 0.30.0、4×L4、head_dim=256、int4_per_token_head。
发现：作者遭遇首轮prefill间歇性非法访问，通过每个模块后同步CUDA，定位到reshape_and_cache_int4，再隔离Hadamard变换。
复现：无需模型，生成[N,1,256]随机张量扫描N，与FP64矩阵参考比较；最大误差/参考最大绝对值超过0.02判失败。扫描1–4352行，1101种形状错误，均为奇数，偶数行通过。
边界：受相邻内存影响；污染后方缓冲可稳定触发。补零到128倍数再切回是作者绕行方案，尚未精确定位错误load，不是正式修复。完整脚本见原始Issue。

3. 冷缓存与热缓存答案翻转：用分段不变性追踪首个分歧
10月24日，SGLang [#46259](https://github.com/sgl-project/sglang/issues/46259)报告，仍开放。环境：SGLang 0.5.19、FlashInfer 0.6.18、Blackwell SM120、Qwen3.5式混合GDN模型。
发现：同一请求、temperature=0、batch=1，冷缓存输出chat，命中1344个token后变为query_robot_status，重复3次均出现。
复现：关闭缓存，将1405-token输入拆成1344＋61仍触发；再用随机BF16 q/k/v、24个Q头/4个KV头、维度256，无模型权重比较一次性ragged attention与分段ragged＋paged＋merge。
判据：逐层对照排除GDN、卷积和GEMM路径，首个分歧落在full-attention；作者报告1–2 BF16 ULP差异放大为约0.35 nat的logit偏移并翻转标签。
边界：并非所有浮点差异都是缺陷，需同时看容差和业务输出翻转率。脚本、消融和逐层证据见Issue。

4. PD＋异步MTP：保存游标越过已确认KV前沿
10月24日，vLLM [#63511](https://github.com/vllm-project/vllm/pull/63511)提出修复，尚未合并。Mooncake保存元数据领先于已确认的block hash，可能留下永久缓存缺口。
发现与复现：回放v0.32.0原始场景，调度器要求保存64 tokens，hash却只覆盖48。scheduler/coordinator/worker单测检查两条不变量：保存范围不能超出已确认前沿；结束时最后完整块必须保存。
作者验证：修补版单测305项通过；GLM-5.3 NVFP4、SM120、TP4/DP4/EP16、异步调度＋MTP3的PD实测中，128-token输入生成129 tokens后，各D rank保存最终对齐前缀，并能跨角色重载。
边界：当前main的65项测试用了v0.32.0预编译依赖，完整main构建与CI待验证；并非吞吐或模型质量评测。

5. 量化检查点导出：张量相同不等于推理行为相同
10月24日，Model Optimizer [#2889](https://github.com/NVIDIA/Model-Optimizer/pull/2889)合并，新增经Megatron-Bridge导出vLLM fake-quant检查点路径。
发现：首次发现途径未完全披露；导出/重载对照暴露顺序量化器折叠、SVD低秩校正保留等问题，新增CPU用例在撤销对应修复后失败。
复现：test_vllm_fakequant_mbridge_export.py及PR导出命令；跨TP/EP/PP比较权重值、量化范围与recipe，再用64篇CNN/DailyMail文章各前512 tokens比较逐token NLL。
作者结果：Nemotron-Nano-9B两端NLL为2.3465/2.3467；Qwen3.6 MoE联合量化仍有2.2844/2.3008差距，作者归因于内核微差放大为专家路由变化。
边界：依赖特定Megatron-Bridge提交，MTP及lm_head激活量化尚不支持该重载路径；不能只检查权重名称和形状就放行。

二、AI帮助测试，同时检出AI服务缺陷

6. AI辅助差分Fuzzing：下一次工具调用参数串到当前调用
10月24日，SGLang [#46293](https://github.com/sgl-project/sglang/issues/46293)报告，[修复PR #46295](https://github.com/sgl-project/sglang/pull/46295)仍开放。
发现：作者用Claude Code辅助编写差分fuzzer，对比流式解析与非流式detect_and_parse，再逐条人工复现、审查。
最小复现：CPU即可，无GPU和模型；main提交052feb7，把Tokyo/3、Paris/1两个调用放进同一delta，流式结果错误变成两次Paris/1。慢消费者引起块合并、较大stream interval、推测解码均可能带来这种边界。
测试判据：同一内容换分块方式，工具名和参数不能变化。覆盖整段、每个二分点、随机切分及不切特殊token；作者12个案例修复前4个不一致、修复后0个，另3个回归测试均先红后绿。
边界：完整解析器测试还有7个修复前后均失败的用例，CI未全通过，不能称全面修复上线。

建议优先验证
- 流式分块差分测试：无需模型或GPU，成本低，可迁移到平台推理网关、工具调用解析器，检查“同一语义、不同分块”的不变性。
- 冷/热缓存与chunked-prefill对照：固定prompt，先看首token logprob和输出翻转，再逐层缩小首个分歧，避免直接归因于模型随机性。
