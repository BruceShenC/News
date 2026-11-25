# 大模型与 Agent 质量、测试与工程资讯｜2026-11-25

**窗口：北京时间 11 月 24 日 09:30 至 25 日 09:30。** 本期优先补两道门禁：**多采样不能把内部共享算成客户端缓存命中；P/D 滚动升级不能用单个 Pod Ready 代替同代配对可服务。** 下列验证为上游报告，CI 单独标注。未独立运行，合入不等于进了稳定版。

1. **vLLM：冷请求可能误报 1,216/1,230 个提示 token 命中。** [#66720](https://github.com/vllm-project/vllm/pull/66720)，25 日 03:39 合入。作者发现冷 n=4 非流式取了后完成的子样本统计，而该样本复用了同一请求内先生成的 KV；流式报 0。现取有 Prefill 统计的子样本最小值。A100 定向 GPU 回归旧版失败、修后定向组 3 项通过；Llama-3.1-8B、64-token 块复核报告冷请求两种协议均为 0，暖 n=4 与 n=1 均为 1,216。块若在子样本间被淘汰仍可能低估。Prometheus 继续逐样本记，不能要求与请求级 usage 相同。
2. **Dynamo：进程内 KV 随引擎退出，路由目录也应作废。** [#19312](https://github.com/ai-dynamo/dynamo/pull/19312)，02:25 合入。worker 显式声明生命周期，状态代理在进程内模式 attach 时重置、detach 时清理，并核对替换实例与旧记录重叠、撤回失败后的重试。作者称切回旧逻辑时前四个定向测试失败，外部服务控制仍通过；[部署 CI](https://github.com/ai-dynamo/dynamo/actions/runs/44959843916/job/137023845445)通过，但未重跑两 worker Kubernetes 退出场景。worker 和状态代理都需更新，外部内存服务维持持久语义。
3. **Dynamo：新 Prefill 已 Ready，旧 P/D 还不能退。** [#18586](https://github.com/ai-dynamo/dynamo/pull/18586)，05:46 合入。Grove 关闭的升级报告里，新 Decode 不可调度，旧逻辑缩掉旧 Prefill 后没有同代配对，出现 503。现在新一代角色未齐时，在 maxUnavailable 预算内保留同代旧配对。12 个表驱动场景以 fake Kubernetes client 验副本目标，[Operator CI](https://github.com/ai-dynamo/dynamo/actions/runs/43008974865/job/130483661065)通过；不是 GPU 集群零中断证明。实际验收需持续发代表性请求、看代际配对、503 与首字时延。
4. **SGLang：流式工具解析要对同一输出做分块变形。** [覆盖 #50609](https://github.com/sgl-project/sglang/pull/50609) · [修复 #50615](https://github.com/sgl-project/sglang/pull/50615)，07:46、08:39 合入。旧解析器可让同一流块中的后续完整调用滞留。双调用样本按单调用一块、两调用同块和细分三种方式验证；保持特殊标记完整，未用模型或真实 tokenizer。作者报告覆盖 PR 137 项、24 项预期失败，修复后其中 17 项转通过，仍有 7 项预期失败；基础 CI 标为跳过。检查调用数、参数不重发、文本和调用顺序；与昨日截断语义不是同一问题。
5. **Dynamo：Prometheus 有 SLO 指标，不代表 OTLP 收到了。** [#18957](https://github.com/ai-dynamo/dynamo/pull/18957)，09:25 合入。前端 HTTP registry 独立，旧 OTLP 路径漏 45 个族，含 TTFT、token 间隔、队列深度和就绪。新增真实 HTTP 服务与测试 OTLP 接收端的集成测试，以 HTTP 原 registry 作第三判据，避免两端一起少报；停服重建后旧 series 应消失。只覆盖前端，不代表其他独立端口接齐。
6. **SGLang：慢消费者不能让 KV 回放静默丢数据或完成标记。** [#48114](https://github.com/sgl-project/sglang/pull/48114)，00:39 合入。队列满时延后重试，轮转消费者并限制单轮发送；过期不伪造完成。作者用真实 ZeroMQ 进程内及 TCP socket 的 CPU harness 报告 14 通过、1 跳过，其中 7 项覆盖慢消费者、轮转、完成标记、实时发布、过期及清理。未跑完整服务或测吞吐。门禁应检查序号连续、完成标记在数据之后、慢消费者不阻塞他人。
7. **Playwright：报告保留项目 worker 上限，但不是实际并发。** [#44313](https://github.com/microsoft/playwright/pull/44313)，04:25 合入。自定义与合并报告保留解析后项目上限，百分比转数字，未配置保持空，全局限制独立。作者报告配置、调度、blob 相关 100 项通过，无 CI 节时证据。汇总时分开项目上限、全局上限和实际峰值 worker。

**建议本周先做两项（未执行）：** 固定长 prompt，对比冷暖 × n=1/4 × 流式/非流式，分别留 API 请求统计、内部复用与路由目录证据；另做同代 P/D 滚动升级，专测新 Decode 长期 Pending。部分 PR 有 AI 辅助编写或分诊记录，不足以证明提效或 GUI 自愈。
