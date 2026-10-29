# 大模型与 Agent 资讯｜2026年10月28日
重点：传输成功、loss正常、输出流畅，都不等于计算正确。

日期：2026-10-28（Asia/Shanghai，北京时间 UTC+8）

归档说明：本文保留 2026-10-28 当期调研内容、来源和统计口径，不代表当前状态。仅将个性化称谓改为通用表述；未重新研究或执行项目。

精选6条，主要进展发生在10月27日。以下为作者公开复现与测试报告，我未执行；相关候选修复尚未合入，不等同正式发布。

一、测试AI系统：推理与训练质量

1. Mooncake KV传输：命中缓存，却覆盖其他请求
10月27日，作者通过布局、流量与输出差分定位：BHLNC布局下，4096字节的head片段被按196608字节的地址跨度传输，累计放大48倍，并越界污染邻接KV。
环境：vLLM0.29.0、Torch2.13/cu130、T4、Qwen2.5-0.5B-Instruct FP16、block16、MooncakeStoreConnector。复现入口包含启动命令：重复约1000-token文档、换问题制造命中，再扩为8并发。[Issue #64117](https://github.com/vllm-project/vllm/issues/64117)
证据：600请求、3轮，BHLNC相对无connector基线出现60/56/54次回答差异，LBNHC为6/7/8；这些是答案差异，不是独立判定的错误率。
验证：[PR #64118](https://github.com/vllm-project/vllm/pull/64118)将寻址stride与传输长度分开。test_mooncake_store_layout.py新增用例在main失败、补丁通过，文件共10项通过；未公开修复后600请求复跑。
可迁移方法：校验目标KV之外，还断言旁观请求、相邻block和其他层没有改变。

2. GH200 P/D：DMA报告完成，4GiB边界之后数据却回绕
10月27日定位进展，原NIXL报告为26日。两台GH200、vLLM0.31、NIXL1.5、Torch2.13/cu130、Qwen3-8B；GPU内存注册基址恰好按4GiB或更大边界对齐时触发。
发现路径：输出乱码→逐层checksum→增加CUDA同步仍失败→反复新进程对照→强制地址对齐。作者撤回了最初“GPU readiness race”的解释。[vLLM #64048](https://github.com/vllm-project/vllm/issues/64048)
复现：[上游nixl_rw.py](https://github.com/ai-dynamo/nixl/issues/2512#issuecomment-6437913290)用两节点、VRAM、3500 blocks、2轮，initiator强制2^32对齐并启用probe；按随轮次变化的数据模式逐字节判定，不能只看DONE和传输字节数。
结果：强制4GiB对齐时，两轮分别出现6823/6819个全零坏block。两侧关闭dma-buf后0坏block；vLLM对照由6请求首5token全部不匹配，变为6/6匹配，36层checksum一致。
限制：这是作者验证的绕过，不是驱动修复；具体底层责任仍未定位，不能泛化为所有硬件应关闭dma-buf。

3. MI355X FP8 KV退化：量化评测本身也需要审计
10月27日，8×MI355X、TP8、vLLM0.32.1rc1.dev602、ROCm7.2.3、Kimi-K3，在同一200题脚本中仅切KV类型，FP8得分0.305，BF16为0.975；关闭prefix cache仍仅0.325。Issue含服务命令和完整gsm8k_fewshot.py。[Issue #64106](https://github.com/vllm-project/vllm/issues/64106)
关键限制：阅读脚本发现，test前8题及答案既作few-shot，又进入评测集合；因此只能称这份脚本的配对对照，不能当无污染的正式GSM8K成绩。最后整数抽取也不等于完整标准判分。
目前仅有差分现象，prefill/decode责任未拆分，维护者表示调查，无已验证修复。建议同时建立“量化精度门禁”和“评测数据无泄漏门禁”，避免指标本身误导结论。

4. verl GMPO：被拒绝的token，梯度没有归零
10月27日，作者读policy loss代码发现问题，用CPU脚本与vanilla loss差分确认。verl 0.10.0.dev、commit 4d3ed12a、Python3.12.3、Torch2.14.1；无需GPU或模型权重。[Issue #8406及完整脚本](https://github.com/verl-project/verl/issues/8406)
最小触发：2条16-token序列，old logprob均为-1；第二条指定token的rollout logprob改为-4，IcePop区间0.5_5.0使其权重为0，再分别backward。
预期：拒绝token梯度0，其他token梯度比1。实际geo_mean为-0.00741和0.237；vanilla符合预期。原因是零权重加1e-10取log参与序列几何平均，既保留被拒token，也压低整条序列。
状态：无已验证修复，真实训练触发频率与收敛影响未测。可直接借鉴“零梯度＋无关位置不变”语义断言，不能仅检查loss是否有限。

5. verl后端默认分派：模型标FP8，不代表专家也是FP8
原故障10月26日，候选修复27日。64节点×8 H100的DeepSeek-V4-Pro GRPO作业，8个rollout副本在load_model失败；显式指定Marlin后3/3步成功。[Issue #8381](https://github.com/verl-project/verl/issues/8381)
原因：dense层FP8与专家FP4配置分开存放，旧逻辑错误默认到需要SM100+的DeepGEMM MXFP4路径，而H100是SM90。环境包括vLLM0.29、CUDA13、rollout TP8/DP8/EP64。
检测：[PR #8398](https://github.com/verl-project/verl/pull/8398)新增14项CPU分派测试，覆盖Hopper/Blackwell、量化格式和显式覆盖，作者报告全过；入口test_vllm_quant_backend_defaults_on_cpu.py。
修复验证：作者报告H100集群3/3 GRPO步完成，rollout/actor logprob Pearson约0.9981–0.9984。第一次复测另遇NCCL错误，后续重跑成功，不能省略这项波动。无长期收敛证明，也不代表另一GB200非法地址问题已解决。
价值：先测配置分派，再做单GPU能力探针，最后端到端RL；不要只按模型顶层量化标签设计兼容矩阵。

二、AI帮助缺陷分析与回归设计

6. FunctionGemma解析：修复字符串，也不能破坏数值参数
10月27日，AI辅助调查、实现及测试对照官方格式发现：字符串参数“123”“true”“null”被json.loads改成数值、布尔或空值。CPU复现无需模型。[Issue #63906](https://github.com/vllm-project/vllm/issues/63906)
第一版“全部保留字符串”又导致5个非字符串schema回归，最终改为依schema转换。[PR #63947](https://github.com/vllm-project/vllm/pull/63947)
证据：main有16个字符串用例失败；最终CPU文件40项通过。HTTP测试以6种字符串×流式/非流式组成12项，main全失败、补丁全通过；PR提供两个pytest入口。
环境：main 8701c9b75、Linux、Python3.12、Torch2.14/cu132、H200 NVL。HTTP层使用Qwen2-0.5B配精确regex生成FunctionGemma协议，因此验证的是服务解析忠实性，不是FunctionGemma自然工具选择能力；仍未覆盖未转义非字符串语法。
借鉴：AI提出修改后，以确定性协议输入验证，并保留原有类型行为作为反例，避免“修一类、坏一类”。

优先小范围验证
- P/D与KV链路：逐层checksum、边界地址、旁观缓存不变检查，比只看HTTP成功、传输DONE更能抓静默污染。
- Agent工具协议：受约束输出＋流/非流式＋schema类型矩阵，适合纳入推理网关版本回归，并与模型工具选择准确率分开统计。
