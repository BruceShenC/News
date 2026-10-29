# 大模型与 Agent 资讯｜2026年10月29日
重点：测试要能揭示“正常表象”下的错误，并证明旧代码确实过不了。

本期6项10月28日新报告或候选修复，涵盖推理、训练数据和AI辅助测试。以下均为作者公开测试结果，我未执行；相关PR仍未合入。多模态缓存条目的原始问题发现于10月16日，28日新增完整修复与验证。

一、推理正确性、稳定性与容量

1. vLLM：PP各rank对暖机是否参与意见不同，启动死锁
发现与触发：B200、Qwen3-4B、PP=2，目标模型用FLASHINFER，DFlash草稿模型用FLASH_ATTN，投机token数2。草稿模型仅存在于最后一个rank，旧判断让部分rank进入含send/recv的暖机，其他rank跳过。
实际：启动挂起，每60秒报共享内存broadcast block不可用；enforce-eager及关闭FlashInfer autotune都不能绕过。PR提供完整启动命令，CUDA/PyTorch版本未披露。[PR #64338](https://github.com/vllm-project/vllm/pull/64338)
修复：通过PP CPU all-reduce统一判断，所有rank同意才暖机。作者报告修后启动完成，四组测试96项通过；新增测试覆盖PP1/PP2及混合backend。[固定提交测试入口](https://github.com/vllm-project/vllm/blob/d7670a30e0698c9173bff4b790056fe388aac89c/tests/model_executor/test_flashinfer_attention_warmup.py)
借鉴：把“异构attention backend×PP×投机解码”的启动就绪、有界超时纳入门禁。这里证明启动正确性，没有吞吐提升证据。

2. SGLang：VLM缓存身份错误，可能把上一张图的答案复用给下一张
原始发现：10月16日，Qwen2.5-VL-3B、RTX4090、native /generate，依次输入A/哈希A、B/哈希B、B/哈希A，第三次返回A内容，cached_tokens由15升至193；3次冷重启均复现。不要泛化为OpenAI接口也暴露相同字段。[原Issue #44641](https://github.com/sgl-project/sglang/issues/44641)
10月28日新进展：[PR #47120](https://github.com/sgl-project/sglang/pull/47120)以完整BLAKE3身份贯穿媒体、embedding、batch去重及KV缓存，并结合媒体顺序和cache_salt；调用方提示不能覆盖实际内容身份。
检测入口：红/蓝图、相同或部分碰撞hash，执行cold-blue→red→warm-blue→warm-red，要求内容不串、不同图缓存命中为0、同图复用大于0；另测A-B-A和Python/Rust路径。[真实VLM测试](https://github.com/sgl-project/sglang/blob/a4fca2a4b43d48d46d2ff9c4f22c72171827c3af/test/registered/vlm/test_vlm_input_format.py)
验证与代价：H200、Torch2.14.1/cu130、Triton3.8；作者报告CPU等976项、GPU等51项通过，另有权限与基线同现失败，并非全绿。真实Qwen换图中位延迟约68.3→72.7ms，同图约29.6ms。跨节点、非NVIDIA端到端未测。
借鉴：缓存门禁同时验证内容隔离与复用收益；更完整身份可能牺牲共享文本前缀复用，不能称统一加速。

3. vLLM：异步KV准入调度中的平方复杂度，用操作计数抓回归
10月28日，[PR #64379](https://github.com/vllm-project/vllm/pull/64379)将重复扫描已准入请求的O(k²)逻辑改为增量统计。首次业务发现途径未披露，公开证据是CPU基准及负控测试。
复现：真实Scheduler＋async mock_kv，matched_tokens=64、80-token prompt，分别准入256/1024/4096个请求；PR有完整脚本。macOS arm64 CPU实测约33.6→3.3ms、490→11.7ms、8000→46.5ms，4096规模仅单次测量。
判定：新测试提交16请求但只允许10次load，旧版辅助计算调用55次，新版10次，同时检查恰好准入10；保留测试撤销修复后断言失败。[固定测试入口](https://github.com/vllm-project/vllm/blob/eda4ccc74a0ee211b4405eeb1e7718217ba87297/tests/v1/core/test_scheduler.py#L7945)
限制：相关回归327项通过，另有主线同现的环境相关失败；没有GPU或P/D端到端TTFT验证。不能把调度微基准写成整机服务172倍加速。

二、训练数值与数据质量

4. PyTorch：同位dropout统计正常，错位比较才暴露随机数重用
10月28日报告。Linux、L40、CUDA12.6，已复现版本为2.7.0和2.14.1；BF16连续张量4096×128、16字节对齐，eager native_dropout、p=0.5，仅设一次seed后连续调用两次。[Issue #202370及完整脚本](https://github.com/pytorch/pytorch/issues/202370)
实际：两次掩码同位置相等比例约0.499，看似正常；但每8项中第一次后4项与第二次前4项的262,144次比较全部相等，比例1.0，独立采样应约0.5。将指针偏移8字节或改FP32后，错位比例约0.502。
定位边界：作者观察Philox offset每次只推进4，怀疑向量化路径消耗8个随机数却仅预留4；候选引入提交未做bisect确认。最初注意到异常的途径未披露，目前无已验证修复。
建议：随机算子测试补跨调用、跨向量位置的相关性及地址对齐矩阵；这是建议，不是项目已合入回归。

5. verl：混合模态Parquet中的None，让SFT数据加载崩溃
10月28日，[PR #8420](https://github.com/verl-project/verl/pull/8420)处理“媒体列存在但值为空”的情况。旧实现只处理列不存在，images=None进入长度检查后抛TypeError。
复现：main 2ab3529、CPU、Qwen3-VL-2B-Instruct processor；两行Parquet分别是64×64红图对话、images=None的纯文本，max_length=1024、no_padding。Python/pandas/pyarrow版本未给出，最初发现过程未披露。
检测：将既有fixture空images改为None，并增加全None的videos列，逐项比较input_ids、loss_mask、position_ids应与原输入一致。作者报告主线加新测试1失败/6通过，补丁7通过。[测试文件](https://github.com/verl-project/verl/blob/b88da379590b7858e2c82c0a80a99ea0f52d31fd/tests/utils/dataset/test_multiturn_sft_dataset_on_cpu.py)
限制：只证明提供processor的该路径；processor=None的已有缺口仍在，不能宣称全部nullable媒体输入已支持。

三、AI帮助缺陷分析和回归设计

6. verl GPG：AI辅助修正奖励缩放，用负控确认测试有效
10月28日，[PR #8422](https://github.com/verl-project/verl/pull/8422)纠正GPG alpha：应依据非均匀奖励组的有效响应数，而旧实现按非零奖励数计。两组奖励[1,0,0,0]和全零组，旧alpha=8、预期2；改成±1编码后旧值变1、仍应为2，更新尺度错误依赖奖励编码。
公开协作链：Claude Code调查、起草补丁与测试，Codex审查复跑，提交者报告逐行审核及亲自复跑。环境为macOS CPU、Python3.12.15、Torch2.14.1。
验证：保留8个新增用例、恢复旧生产代码，6失败/2通过；恢复补丁后8通过，整个文件31通过。覆盖奖励编码、混合/均匀组、singleton和全均匀batch。[固定测试入口](https://github.com/verl-project/verl/blob/7f38292d6203706787aa377cf3dce4d067978255/tests/trainer/ppo/test_core_algos_on_cpu.py)
限制：未跑全仓CPU套件及GPU端到端训练，不能说收敛或奖励已改善。价值在于“AI提议→人工核对→旧代码失败→补丁通过”的证据链。

优先验证两项
- 多模态缓存隔离：同提示换图片、A-B-A、并发及不同cache_salt，既看答案，也看命中和延迟。
- 随机算子相关性：从BF16、向量位置和内存对齐补小型探针，避免只看均值、有限值或可复现性而漏掉随机序列重用。
