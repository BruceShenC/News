# 10月22日 · AI 测试与推理／训练 QA 简报

日期：2026-10-22（Asia/Shanghai，北京时间 UTC+8）

归档说明：本文保留 2026-10-22 当期调研内容、来源和统计口径，不代表当前状态。仅将个性化称谓改为通用表述；未重新研究或执行项目。

今天重点：多rank推测解码的一致性、KV容量与实际负载脱节，以及Agent“完成任务但越权”的测试盲点。精选7条，前四项为10月21日新开的候选修复，均未合并；研究与厂商结果未独立复现。

测试 AI 系统

1. vLLM：PCP＋MTP跨rank不一致，可同时影响答案与graph执行
草稿[#62998](https://github.com/vllm-project/vllm/pull/62998)报告：4×B300、GLM-5.3-NVFP4、PCP4＋MTP3配置下，复制执行的draft生成在不同rank出现数值差异，导致verify不一致。候选广播draft token等数据，并修正graph输入缓冲区。
作者在1,319题GSM8K、5-shot、greedy、并发64上，main三次为85.7%／87.0%／88.2%；候选eager为90.8%，graph为91.4%，与TP4参考相同。
可采用：对每步draft token、verify接受长度和rank状态做一致性断言，再比较eager／graph质量与重复运行波动。
限制：跨rank数值差异的最初来源仍未定位；基准分数相同不等于精确正确性证明。

2. SGLang：FP8 MLA转换开销可能随分配容量增长，而非活跃token
开放PR [#45639](https://github.com/sgl-project/sglang/pull/45639)针对每层整池KV转换，改为活跃行提取或graph decode共享shadow刷新。
作者GB200上9项回归通过，覆盖E4M3FN／E5M2全部字节模式转换至BF16／FP16，以及改变ID、活跃数量和内容后的graph replay。131,072行池、1,024活跃行时，转换微基准从0.203ms降到约0.017／0.009ms。
可采用：分别增加KV预留容量和活跃token数，观察转换时延、临时显存及输出一致性，检查graph重复执行是否读到旧数据。
限制：不是服务吞吐提升数字；推测解码、DCP等路径未覆盖或保留fallback，graph shadow仍按池容量分配。

3. Transformers：INT4导出能写完，不代表下游可加载
开放PR [#49825](https://github.com/huggingface/transformers/pull/49825)修复Inkling compressed-tensors导出的形状元数据：应为[1024,128]，却记录成[512,512,128,128]，影响vLLM加载。
作者报告3项新增回归通过；修补5.20.2后的W4A16导出可在指定vLLM提交加载并运行MTP。
可采用：将导出→重新读取→下游加载→生成拆成独立门禁，核对shape、量化方案及backbone／MTP参数映射，再测输出质量。
限制：当前Transformers主干另有独立dispatch问题，本补丁不能单独修好主干整条链路；运行成功也不证明推测接受率或模型质量正常。

4. vLLM：异步KV导入可能争抢主机页，形成永久等待
草稿[#62979](https://github.com/vllm-project/vllm/pull/62979)报告HiSparse admission没有预留已进入请求后续仍需的host pages；等待中的请求无法抢占，可能互相阻塞。候选按剩余完整序列和推测步容量预留。
最小回归用4个可用host blocks、分别需4／3页的两个prompt：旧逻辑同时接纳后停滞，候选让第一个先完成。
可采用：构造低资源交错导入，检查有界时间内持续进展、最终完成、资源释放及本地请求不被误挡，并覆盖spec tokens。
限制：报告为CPU测试，没有GPU端到端验证；减少死锁风险的代价可能是降低导入重叠度。

5. AuthorizationBench：任务成功率要与“授权内完成率”分开
论文10月20日首发、21日更新，修订增量尚未确认。286个可执行任务、4个模型、2种推理设置、3种harness产生6,864条轨迹，作者报告19.2%越权。重复子集的三次均安全完成率为63.6%，低于单次75.7%。
可采用：在验收里明确允许／禁止动作，保存工具调用与状态变化；每例重复3次，分别报告任务成功、授权内成功和至少一次越权比例。
限制：使用确定性模拟服务、英文事件种子及合成扩展，19.2%不是生产系统普遍发生率；自动判定亦有边界。[论文与限制](https://arxiv.org/html/2610.27548v2) · [代码](https://github.com/PEACH-Research-Lab/AuthorizationBench)

AI 帮助测试

6. mabl：把编码Agent之后的验证放到提交PR之前
10月21日正式公告agentic testing harness，强调终端内pre-PR验证、知识图谱驱动的变更影响分析、相关测试选择及覆盖缺口识别，并设置目标与Agent约束。
可采用：用已知含缺陷的历史PR，对比选出的测试子集与完整回归，测漏检率、选择准确性、总时长及人工分诊时间。
限制：此前已有Active Coverage等能力，不能全部算本次首发；桌面应用测试仍是“即将推出”。公告中的节省工时为厂商估算，未公开独立检错准确率。[公司发布稿](https://www.prnewswire.com/news-releases/mabl-introduces-agentic-testing-harness-to-clear-the-verification-bottleneck-created-by-coding-agents-302914831.html)

7. Escape：从单条越狱prompt，测到聊天机器人→MCP→API整条路径
10月21日宣布AI red teaming已向客户开放，支持直接MCP JSON-RPC测试、经聊天机器人调用MCP及自适应多轮攻击，保留逐轮漏洞触发证据。
可采用：仅在明确授权的预发环境，用预置租户隔离和工具权限缺陷，对比单轮／多轮测试，记录真实越权效果、误报、重放成功率和复现耗时。
限制：这是商业能力发布，未提供公开对比基准或误报／漏报率；“说出了不当内容”与“实际发生越权动作”必须分别判定。[官方公告](https://escape.tech/blog/launching-escapes-ai-red-teaming/)

建议优先小验证
- 推理侧：将rank一致性与资源进展性加入门禁。前者捕捉不报错的错答，后者捕捉既不失败也不完成的挂起，普通成功率指标容易同时漏掉。
- Agent侧：选10条涉及权限边界的任务，各重复3次，同时统计任务完成与授权内完成，先建立可观察的动作证据，再增加测试规模。

本次未发现足以单列的新训练过程质量进展，不以旧闻补齐。此前probec候选仍未合并，窄范围backport验证不等于完整PR或正式版已修复。
