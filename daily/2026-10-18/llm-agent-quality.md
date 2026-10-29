# 10月18日 · AI 测试与推理／训练 QA 简报

日期：2026-10-18（Asia/Shanghai，北京时间 UTC+8）

归档说明：本文保留 2026-10-18 当期调研内容、来源和统计口径，不代表当前状态。仅将个性化称谓改为通用表述；未重新研究或执行项目。

今天重点是 PD 静默数据损坏、请求历史引起的性能漂移，以及权重生命周期竞态。前5项均为10月17日新提出的 PR，尚未合并发布；后2项为近一周研究。数据来自作者公开实验，本次未独立复现。

测试 AI 系统：推理与服务

1. vLLM：统一校验 PD 传输的 KV 内容
[#62211](https://github.com/vllm-project/vllm/pull/62211)，Open PR。producer／consumer 分别计算 checksum，不匹配或无法验证时进入 KV 加载失败路径。
价值：发现“传输成功、内容却错了”的静默故障。当前 CPU 测试报告647通过、145跳过、2失败，后两项在 main 也失败；当前提交尚未完成 CUDA kernel 和端到端 GPU 验证，不能确认开销已经可忽略。MRV1 不支持，draft KV 不参与校验。
可验证：损坏、缺失校验信息是否都明确失败；同时测正常路径 TTFT、吞吐与误报率。

2. vLLM：分片权重布局错配，可能产生错误回答
[#62155](https://github.com/vllm-project/vllm/pull/62155)，Open PR。sharded_state 保存与加载采用不同并行布局时，旧逻辑可能只填入部分权重，补丁改为启动即报告形状错误。
作者用 Qwen3.8-27B-FP8、TP2、2×H20复现：weights 保存／data 加载出现330条形状警告，单图乱码、48图请求500；相同布局4/4正确。
价值：checkpoint 验收应贯穿“保存→加载→推理”，明确布局契约。新增拒绝用例及其余26项测试通过，但标准 Llama GPU round-trip 套件未完整跑通，尚不代表全部量化模型已覆盖。

3. SGLang：采样请求退出后，greedy 请求可能仍走慢路径
[#44923](https://github.com/sgl-project/sglang/pull/44923)，Open PR。非 greedy 请求退出 batch 后，is_all_greedy 未刷新，剩余请求持续走采样和 speculative rejection 路径。
作者报告特定 DSpark、batch1 场景下 decode step 耗时增加约37%。token 仍取 argmax，但 logprob 可能因 kernel 路径不同出现末位差异。
价值：GoldenPrompt 要加入“请求加入又退出”的历史回放，同时验 token、logprob、TPOT。新增 CPU 用例覆盖合并、过滤和重排；37%不能外推为普遍线上损失，上游验证尚未完成。

4. vLLM：batch-invariant 加速，要分开验两种一致性
[#62219](https://github.com/vllm-project/vllm/pull/62219)，Draft PR。调优 H100 SXM 上 Llama-3.1-8B／Qwen3-8B 的 BF16、TP1 matmul 配置。
作者报告2,028/2,028个跨 batch 探针逐位一致，固定 batch 延迟改善1.85–2.79倍；但在线吞吐仅 Llama 提升13.6%，Qwen 持平。
价值：同版本 batch 不变性，与跨版本数值／准确率回归，应使用不同验收标准。新 BLOCK_K 会改变舍入；GSM8K 差异小于0.5个标准误，不代表跨版本逐位一致。尚未覆盖 TP>1及其他硬件。

5. SGLang：权重释放与热更新竞态，增加确定性复现
[#44928](https://github.com/sgl-project/sglang/pull/44928)，Open PR。权重内存释放后磁盘更新可能杀死 scheduler；等待写锁的更新也可能被 release 超越。补丁在 loader 前拒绝操作并返回400。
作者用 RTX3050 8GB、Qwen3-0.6B 验证顺序及竞态路径；resume 后可重新加载，greedy 输出不变、health=200，CPU 测试23项及8个子测试通过。
价值：补齐 rollout／在线权重更新生命周期测试。尚未本地验证四卡 Blackwell、真实 speculative／draft 执行，也没有吞吐结果。

测试 AI 系统：训练与可靠性

6. Meta：模型级校验与重放，补充硬件健康检查
10月15日首发、MICRO 2026待刊研究。模型级 checksum＋replay 标记异常，再用主机内 GPU 两两比较定位设备、隔离主机，也用于上线前烧机。
作者报告筛出的静默数据损坏超过厂商健康工具的2倍；数千 GPU、数十模型的跨厂商训练 loss 差在0.3%内、NE差约1e-4。
价值：硬件健康通过之后，仍需模型级数值回放。限制：主要为推荐模型和 MI350X 经验，不能直接外推到 LLM；训练失败率下降还涉及其他修复。[论文相关章节](https://arxiv.org/html/2610.22230v1#S6.SS3)

7. RankLens：将捕获的分布式异常缩小到单卡调试
10月13日研究预印本。单 GPU 执行一个 rank 的真实计算，在通信边界重放结果与时间信息。
DeepSpeed／Megatron-LM 的8–64 GPU实验中，作者报告训练张量通信签名100%匹配、步耗时误差低于5%，小型人工 NaN 案例能重放异常位置。
价值：降低已捕获问题的重复定位成本。限制：100%排除了控制／管理消息，不等于全系统正确；资源争用模拟仍有误差，数值重放须预先记录 payload，代码可得性未确认。[论文](https://arxiv.org/abs/2610.18009)

今天没有找到证据同样扎实、值得新增的 AI 辅助用例生成或 GUI 自愈发布，这一栏不重复昨天内容。

建议小范围验证两项
- GoldenPrompt 历史回归：比较纯 greedy 与“短采样请求加入后退出”两组，检查 token、logprob 及退出后的 TPOT 是否恢复基线。
- PD 故障测试：隔离环境中注入 KV 内容损坏和缺失校验信息，确认不会返回看似成功的错误答案，同时量化正常路径开销。

上述 PR 适合作为测试设计线索；尚未发布的补丁，不宜直接视为生产环境已有保障。
