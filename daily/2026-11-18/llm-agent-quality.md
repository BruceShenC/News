# 大模型与 Agent 质量保障简报｜2026-11-18

**观察窗口：北京时间11月17日09:30至11月18日09:30。** 本期精选6项窗口内合入的工程质量修复，均属于**测试AI系统**。问题可能更早发现；合入主分支不等于已进入稳定版。以下测试结果均为作者公开报告，本次未运行模型、测试或故障注入。

## 1. SGLang：健康检查全绿，PD解码仍可能完全停滞

**11月16日提出，11月18日05:43合入。**

- **检出证据：**原始报告给出确定性CPU回归与真实服务实验，未明确最初发现途径。KV压力触发请求回撤后，调度器提前返回，不再轮询已到达的KV；这些请求占着页和解码预留量，回撤请求也无法恢复。
- **复现：**GB200、Qwen3-0.6B、同节点P/D、Mooncake TCP，提交 `d800cb859e85`；限制D侧 `--max-total-tokens`，让长请求解码时另有请求等待传输。默认配置受影响；禁用回撤备份的配置不适用。真实客户端脚本未公开，只称可向维护者提供。
- **判定：**旧版持续冻结360秒，363次健康检查全部返回200；补丁后请求全部完成。CPU入口 `test_decode_retraction_transfer_progress.py` 驱动真实队列，旧版4000步后仍有两项无法出队；修复后三例通过，清理测试22例通过。
- **边界：**真实服务每版本仅一次，未验证RDMA、TP>1、DP attention及host-receive；没有准确率或性能实验。

来源：[原始复现与控制组](https://github.com/sgl-project/sglang/issues/50711) · [修复及回归入口](https://github.com/sgl-project/sglang/pull/50712)。

## 2. SGLang：NVFP4 kernel不报错，不代表量化模型算对了

**11月12日提出，11月18日08:21合入。**

- **症状与根因：**SM100上Nemotron-3-Nano NVFP4的GSM8K显著下降；隐藏维2688不是TRT-LLM kernel要求的256倍数，原路径仍运行却返回错误结果。此前覆盖的2048、4096形状均对齐，漏掉边界。
- **对照：**GB200、FlashInfer 0.7.3、TP1、完整1319题、关闭thinking并greedy。补齐到2816后，GSM8K从17.36%到89.92%；对BF16的top-20 KL由0.903到0.0376，top-1一致率68.9%到93.1%。BF16参考为90.67%，不是宣称量化无损。
- **回归：**`test_flashinfer_trtllm_gen_moe_backend.py`新增Nano形状，200题阈值0.80；作者TP4/EP4实验两次0.925，关闭padding仅0.405。该文件属于weekly，并非每个PR默认执行。
- **边界：**padding增加约4.8%的相关矩阵维度，分发还须改用BF16激活。另一compressed-tensors路径仍未覆盖，不能泛化为全部NVFP4修复。最早发现过程未完整披露。

来源：[模型配置、数值对照与测试范围](https://github.com/sgl-project/sglang/pull/50008)。

## 3. vLLM：NaN的符号位，可把数值错误扩大成越界写

**11月10日提出，11月18日01:02合入。**

- **发现：**作者验证另一项top-k改动时检出。FP16分桶把所有NaN排在顶部，但FP32键将带符号位的NaN排到负无穷以下，两阶段顺序不一致。
- **复现：**B200，对照基线 `4e3463c874`，相同编译选项。10万列、top-k=2048：80行且末行8000个带符号NaN时，sampled路径覆盖输出后的64KiB哨兵；单行4096个此类NaN使cooperative路径非法访存。修复后二者不再越界。
- **回归：**`test_top_k_per_row.py -k test_topk_nan_ordering_matches_torch`，首版新增用例旧实现12失败、修复72通过；现版本又补充BF16转换后的符号位设置，作者未本地运行这版。已有套件885通过、45跳过。
- **边界：**ROCm保留旧逻辑且跳过新符号用例，尚未验证其转换行为；交替四轮微基准未见超过0.8%的单配置减速，不等于服务性能结论。

来源：[触发位型、哨兵检测和测试版本区别](https://github.com/vllm-project/vllm/pull/66839)。

## 4. vLLM：缓存容量够用，也可能被写时复制的瞬时显存击穿

**10月11日提出，11月17日21:34合入；本期报道最终方案合入。**

- **机制：**部分前缀命中触发CoW时，索引赋值先汇集全部源块为临时张量；启动profiling和warmup不经过该路径，临时显存未被预算。大型Mamba状态块可放大到GiB级。原始发现途径未披露。
- **修复与检测：**GPU改为Triton直接按连续字节段复制，只上传描述表。GB200/CUDA13.0，`test_attn_utils.py`与`test_utils.py`共134通过；新增 `test_copy_kv_cache_blocks_inplace_cuda_skips_gather_temporary` 检查额外分配小于1MiB，旧版失败。
- **负载：**Qwen3.5-35B-A3B-FP8、align缓存、匹配粒度64、异步调度；先预热2500-token前缀，再并发256个共享前缀请求。四轮中位完成时间1383→1148毫秒。另一个Granite模型的1319题准确率0.7885→0.7877，命中率同为87.9%，不能据此声称完全等价。
- **边界：**链式复制和CPU/XPU仍走旧实现；ROCm未本地验证。早期256MiB分块及批量Memcpy不是最终方案。

来源：[内存断言、端到端负载和方案演进](https://github.com/vllm-project/vllm/pull/61051)。

## 5. vLLM：进程退出不等于GPU资源释放

**11月17日05:42提出，11月18日08:35合入。**

- **发现环境：**节点PID 1为 `sleep infinity`，不回收僵尸进程，暴露Ray worker残留；正常回收的init让CI较难看见。Ray初始化替换SIGTERM处理器，3秒后的abort可能早于vLLM终止actor。
- **复现入口：**双MI300X，在tests目录运行 `TARGET_TEST_SUITE=MI300 pytest -v -s basic_correctness/models/test_basic_correctness.py -m 'distributed(num_gpus=2)' -k ray`。
- **前后：**旧版3失败、1通过，两个残留worker各持有约137GiB；恢复原Python信号处理器后，10轮每轮4项通过，无残留。另有CPU自发SIGTERM实验验证信号处理器被覆盖；多进程后端四项控制测试仍通过。
- **边界：**只在本调用负责初始化Ray、且位于主线程并已有可调用处理器时恢复。DP actor manager的另一初始化入口未覆盖；PR没有新增专门测试文件，证据是既有测试重复运行及独立复现。

来源：[退出竞态、容器条件与重复验证](https://github.com/vllm-project/vllm/pull/68026)。

## 6. SGLang：多轮Agent缓存劣化，要看“连续未命中”而不是只看平均命中率

**11月12日提出，11月18日02:12合入。**

- **触发：**SWA模型单chunk prefill在分支点插入缓存后，没有同时保护prompt尾部；decode超过一个窗口后尾部被回收，下一轮再次未命中。多chunk路径原本正确。最初发现过程未完整披露。
- **验证：**DeepSeek-V4.1-Flash、MI355X、TP2、Rust TreeCore、并发32，同节点相同 `5325cfabd7` 构建，仅替换缓存文件；AgentX轨迹每组运行1小时。未命中定义为缓存长度比上一轮prompt末尾少至少512 token。
- **结果：**可关联日志的轮次约占全部请求一半；其未命中率4.3%→1.1%，前轮未命中后的再次未命中概率0.796→0，最长6轮的连续失效不再出现。全负载新增prefill token减少2.0%，不能把局部统计当全量命中率。
- **回归：**`test_unified_radix_cache_core.py`覆盖整页、尾部不满页及多chunk控制组，Python/Rust两种TreeCore。GSM8K差3题，处于作者报告的一倍标准误范围，非逐token一致性证明。

来源：[缓存所有权、统计口径和回归覆盖](https://github.com/sgl-project/sglang/pull/50059)。

## 优先小范围验证

1. **把“持续进展”加入健康门禁。**在KV压力、回撤与传输同时出现时，监测出队、完成请求、资源释放，而非仅HTTP健康码；退出时检查子进程和GPU占用。可直接借鉴第1、5项的控制组。
2. **建立结构与异常数值边界集。**加入非256对齐隐藏维、带符号NaN、部分前缀CoW、单chunk与多chunk切换；结合数值参考、内存哨兵及峰值分配断言，避免只跑整齐形状和常规输入。

以上为建议，未在本次执行。未找到足够新增证据补充AI用例生成、GUI自愈或训练恢复案例，不凑栏目。PR标注AI辅助不等于证明AI测试效果。已核查PR元数据、正文和代码差异，部分页面仍显示失败或未运行的CI，不将合入解释为所有CI全绿。

## 归档

[本期完整简报](https://github.com/BruceShenC/News/blob/main/daily/2026-11-18/llm-agent-quality.md)。正文已归档；主题索引和共享索引本期未更新。

