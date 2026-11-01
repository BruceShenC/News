**大模型与 Agent 资讯｜2026年11月1日**

本期7项，聚焦推理/训练质量和有明确参与披露的AI辅助测试。原始报告发布于**10月31日或11月1日UTC**，均在本次约24小时窗口内；不是故障首次发生日期。状态核验至北京时间09:29，相关修复PR均仍开放、未合入。下列实验均为提交者报告，**本期未运行模型、脚本或测试**。

**一、测试AI系统：先证明正确，再比较速度**

**1．SGLang：PD解码吞吐上升，可能是跨microbatch读错内存**
日期：10月31日，issue #47613、PR #47614。
发现：Kimi-K3在PD decode与流水并行组合下连续输出token 0（“!”）；作者扫描PP拓扑和异步深度，再在接收后、forward读取前分别打checksum。两个位置不一致，定位到接收stream分配的proxy tensor未登记forward stream，引用提前释放后被下一次接收复用。原始是线上还是实验首次遇到，未披露。
环境/入口：SGLang v0.5.22（baa9bef）、FlashInfer0.7.1、Torch2.14.1+cu130、CUDA13、单机8×B300。公开启动命令采用TP4/PP2、pp-async-batch-depth2、fake PD、cutedsl_mla、BF16 SSM、关闭radix cache；bench_serving随机输入64000、输出1000、并发64、192请求。
Oracle：统计至少20个连续token 0，并检查两个stream的checksum相同。depth1对照0/192；depth2一次80/192，76/7498次checksum不符；record_stream补丁后0/192、0/7499不符。新增CPU两例旧版1失败1通过、补丁2通过，只验证调用顺序与跳过CPU张量，不能替代GPU竞态复现。
限制：GPU结果在v0.5.22，main仅源码比较；全部PD用fake transfer，未跑真实跨机PD。专用通信stream追加实验使用CUDA等价修复而非提交diff原样，且没开checksum。性能3次顺序采样约0.52–0.93%下降，不足以证明零开销。**错误输出时较高吞吐不可作为有效性能结果**，也不能把本例与昨日vLLM的NaN传播混成同一原因。
[原报告与完整命令](https://github.com/sgl-project/sglang/issues/47613) · [修复与分层验证](https://github.com/sgl-project/sglang/pull/47614)

**2．vLLM双向NIXL：请求等待一个根本没发出的KV读取**
日期：10月31日，issue #64859、PR #64860。
发现证据：真实scheduler与connector的CPU状态机复现、源码定位；首次业务发现过程未披露。P侧第二轮先从D拉KV，若读取失败后recompute，或读取成功后被抢占且缓存驱逐，再次lookup仍宣布远端命中，但已处理标记阻止注册新read，于是无限WAITING_FOR_REMOTE_KVS，abort也等待不存在的完成通知，KV块不释放。
复现：Linux x86_64、Python3.13、Torch2.14、main a20280f30e；macOS arm64的2b009fc6d1也报告同样红绿结果。无需GPU，运行test_bidirectional_kv_transfer.py中recomputes_locally两例：分别注入失败回调，以及成功后抢占并用reset_prefix_cache模拟驱逐。
Oracle：请求必须有token进展或真实read，最终完成且scheduler清空。旧版两例失败，补丁两例通过；联合两文件从230通过到232通过，**同样3例仍失败**，其中两例因离线无法加载权重，另一例packed cache注册失败。不能写成全套通过。
可迁移：在超时测试中同时断言“等待状态是否有对应在途操作”和资源回收。作者披露AI辅助、人工复核测试；GPU/NIXL端到端仍待验证，CPU通过不是跨机恢复已成立。
[状态机复现](https://github.com/vllm-project/vllm/issues/64859) · [补丁与回归](https://github.com/vllm-project/vllm/pull/64860) · [固定测试文件](https://github.com/vllm-project/vllm/blob/8ab70abc09d4b30c5edbca61b7eac88d76ad328c/tests/v1/kv_connector/unit/test_bidirectional_kv_transfer.py)

**3．权重同步失败，不一定是NCCL：3字节张量即可触发packing错位**
日期：11月1日00:22 UTC，vLLM issue #64936，尚无已核验合入修复。
发现：在PR #64692审查中的首次权重更新，多个模型出现Byte转Long偏移错误，作者随后把问题缩到CPU序列化，无需模型或GPU。根因是uint8打包直接拼接，不做padding；后面的宽类型起点不满足元素大小对齐，contiguous()也不会复制已连续切片。
最小步骤：在当前main导入pack_tensors/unpack_tensor，顺序放3个uint8的ones和4个int64的arange，打包再解包。期望名称、shape、dtype和值完整往返；实际报storage_offset=3不能被8整除。main精确SHA及Python/Torch版本未披露，应先补齐再做严格复现。
作者报告接收侧只clone错位切片可解除上述运行阻塞，但未提供完整回归矩阵。发送端padding则需训练侧和接收侧同步变更协议，不能单边替换。
建议低成本加入“不同dtype×奇偶长度×张量顺序×分包边界”的往返性质测试，再验证NCCL与IPC两条调用链；这是简报建议，未执行。
[CPU最小脚本、发现来源及修复取舍](https://github.com/vllm-project/vllm/issues/64936)

**4．verl：首轮零token被误当成整段prompt，污染评分和长度指标**
日期：11月1日01:18 UTC，issue #8478、PR #8479。
发现证据：代码路径检查＋真实ToolAgentLoop.run的CPU桩复现，初始发现途径未说明。main 8df5baee、Python3.12；首token前abort或权重同步gate拒绝可产生空TokenOutput。末尾切片prompt_ids[-0:]返回整段prompt而非空列表。
复现/判定：输入prompt [11,12,13,14,15]，stub server返回token_ids=[]、log_probs=None、stop_reason=aborted。旧版response_ids等于prompt、response_mask为空；后处理后prompt落在response区域并参与解码评分和长度统计。正确应保留prompt，response ids/mask均空、logprobs仍None。
作者报告新增两例旧版1失败1通过、补丁2通过；正常非空首轮为负控，关联CPU测试234通过。补丁在空mask时显式返回[]，与SingleTurnAgentLoop对齐。测试使用真实run与合并逻辑，但生成器和初始token构造被stub，**不是实际模型、权重同步或训练收益验证**。AI参与准备，人工复核测试，不推断AI独立发现。
[报告](https://github.com/verl-project/verl/issues/8478) · [PR与验证](https://github.com/verl-project/verl/pull/8479) · [固定CPU回归](https://github.com/verl-project/verl/blob/1fcab32aecf3a7dc9cc4198fc31504df2dd11f0f/tests/experimental/agent_loop/test_tool_agent_loop_empty_response_on_cpu.py)

**5．PyTorch Elastic：训练还在跑，成员账本可能已错**
日期：10月31日，两份独立故障报告，未见已验证修复。
时钟案例：Torch2.15.0+cpu、单机c10d/TCPStore、3个agent，gloo worker每2秒all_reduce。只让C的墙钟快60秒，保持monotonic不变，并每2秒读rendezvous状态。4/4次约3秒三者入组、约9秒只剩C，A/B却仍存活训练，持续120秒；不改时钟的对照保持三者。源码证据指向跨主机比较writer/reader墙钟，默认15秒活性阈值，不能把60秒实验当所有系统的故障阈值。
身份案例：同版本Docker、独立TCPStore，关闭共享rendezvous store干扰，nnodes2:3、worker仅sleep。两个容器同hostname且torchrun均PID1，4/4次共享同一descriptor与group_rank1；换不同hostname后rank0/1/2、world size3正常。
可迁移：故障注入覆盖墙钟偏移、容器重建时旧实例未退出，以及rank/成员唯一性；不能只盯loss或GPU利用率。两案提供步骤和结果，但未附完整一键脚本，初始发现途径未披露；身份案例没有实际训练collective验证，不能把风险写成已发生梯度损坏。
[墙钟偏移报告](https://github.com/pytorch/pytorch/issues/202899) · [容器身份碰撞报告](https://github.com/pytorch/pytorch/issues/202900)

**二、AI帮助测试：保留人类核验与工具证据**

**6．Agent辅助分析scatter越界：用负零哨兵发现“数值没变”的写入**
日期：10月31日，PyTorch issue #202905，未见已验证修复。作者明确说分析由agent提出、已人工阅读认可；原始故障先由ROCm设备AddressSanitizer发现，不能宣称AI独立发现。
环境：ROCm gfx950/MI355X、源码6c3af026fb5、HIP7.18；CUDA侧Torch2.14.1+cu126、V100。5×5 fp16张量最后一列scatter_add_使用4字节packed atomic，末尾越界触及2字节；原因是相对当前行的index与全tensor numel混用，末行安全判断失真。
最小oracle：分配26个half，第26个置−0.0，前25个view成5×5并向第4列scatter_add。应不触及view外元素，实际signbit从True变False。关闭CUDA缓存分配后，compute-sanitizer memcheck另报4字节越界atomic。V100上fp16失败、bf16/fp32哨兵不变；ROCm报告fp16/bf16均受影响，**不能跨架构照搬dtype结论**。
价值：数值相等的−0/+0也能揭示非法写入；建议把邻接哨兵、sanitizer和不同stride/sub-iterator共同纳入kernel回归。报告讨论修复选项，尚无前后回归或性能证据。
[最小脚本、sanitizer记录与AI披露](https://github.com/pytorch/pytorch/issues/202905)

**7．Claude Code辅助视频采样修复：验收配置是否真的改变输入token**
日期：10月31日，vLLM issue #64882、PR #64899。
发现：Qwen2-VL-2B显式num_frames8仍取18帧，作者以在线/tokenize与离线LLM.generate对照，再用Claude Code追踪、编写修改和测试；声明逐行复核并亲自运行。环境main900b8b08e2、macOS26.3.1/M4/CPU、Torch2.14、Transformers5.22.1，baby_reading视频243帧/25fps。
Oracle与前后：固定min_pixels784/max_pixels151200，原版无论4或8帧都1620个video tokens；补丁server/request四帧360、request八帧720，未设参数仍1620。每个loader保留其默认策略，Qwen显式参数优先。独立评论者另做合成视频CPU检查，确认帧数差异，但未重跑原视频/tokenize，不能把它称完整E2E复核。
回归入口含test_tokenization_video_num_frames.py，以及Qwen/HF采样索引对照、默认32帧/默认fps负控。作者报告新测试旧版失败、补丁通过；相关全文件147通过15失败，同15例在main也失败，包含macOS缺FFmpeg等，**不是全绿**。未跑GPU；GLM/Molmo等未纳入此次修复，num_frames/fps冲突策略与HF也非完全相同。
价值：性能/容量测试应核对实际帧数与token预算，不能只存配置值。旧策略有刻意设计背景，因此还需明确接口契约，而非只认一次数值差。
[问题及独立组件对照](https://github.com/vllm-project/vllm/issues/64882) · [PR与验证](https://github.com/vllm-project/vllm/pull/64899) · [固定服务级测试](https://github.com/vllm-project/vllm/blob/0c4c5b26cf992c78ee4d91361df4e1bce0e3034b/tests/entrypoints/serve/tokenize/test_tokenization_video_num_frames.py)

**优先小范围验证**

• **NIXL状态机进展/回收不变量**：CPU即可红绿对照，能提前检出“有等待、无操作”的PD挂起；通过后再扩展真实跨机故障注入。
• **空首轮rollout契约**：成本低、oracle清晰，覆盖abort和同步gate的空响应边界，防止把基础数据污染误诊为模型训练退化。

共性是：**成功返回、吞吐提高、训练进程存活都不是充分的正确性证明。**需要独立参考、状态与资源不变量、边界输入及失败负控。上述扩展验证均为建议，不是已执行结果；本期未为填栏目重复模型发布或营销消息。

[本期归档](https://github.com/BruceShenC/News/blob/main/daily/2026-11-01/llm-agent-quality.md)
