**大模型与 Agent 质量简报｜2026-11-05**

观察截止：北京时间2026-11-05 09:33。优先检索过去24小时，补充近7日有完整修复证据的案例。本期关注训练静默错误、推理状态边界，以及AI辅助测试的证据质量。

本期6个新问题仍开放；另有1项修复已合入。全部数字来自原作者公开报告，本简报未安装、运行项目或自行复现。已与[11月3日](https://github.com/BruceShenC/News/blob/main/daily/2026-11-03/llm-agent-quality.md)、[11月4日](https://github.com/BruceShenC/News/blob/main/daily/2026-11-04/llm-agent-quality.md)质量简报去重。

**1. BF16矩阵乘静默漏掉尾项：有限数值、不报错，也可能已经损坏梯度**

公开时间：11月5日07:48。Halo贡献者先在H100发现，报告者在B300确认；Claude Code协助整理细节，作者声明已审阅。

- **触发环境与复现**：PyTorch 2.11.0+cu130／cuBLAS 13.1.0.3，以及2.15.0+cu132／cuBLAS 13.4.1.3。原帖完整最小脚本令K=8193、M=32、N=4096，仅把a、b最后一行设为1，再计算BF16的a.T @ b。
- **预期与实际**：结果应有131072个非零元素，实际为0；把信号移到倒数第二行便恢复。作者在1024倍数附近扫描704种形状，两套软件栈均命中29种，缺失最后1至3个归约项；所比较的F.linear权重梯度也受影响。作者另提供直接调用cuBLAS的C复现，不经过PyTorch。
- **修复状态与边界**：同批704种形状中，显式连续化转置矩阵或改用cuBLASLt未见遗漏，但这是绕过路径，不是上游修复。未测其他GPU、autocast、torch.compile；尚未展示真实训练收敛受损，不能从局部梯度错误直接推定模型质量损失。
- **可复用检测方法**：在分块边界附近做“只点亮最后几项”的稀疏输入测试，比只比较随机矩阵相对误差更容易定位漏算。建议把前向GEMM与线性层权重梯度分别验收。

来源：[原始报告、版本和Python复现](https://github.com/pytorch/pytorch/issues/203481)；[作者提供的独立C复现入口](https://github.com/pytorch/pytorch/issues/203481#issuecomment-6646823476)。

**2. FSDP2重入checkpoint静默漏梯度：训练能跑完，不代表多个forward都参与更新**

公开时间：11月5日07:50。报告源于Halo的RL训练器，一个backward覆盖同一层的多次forward。AI生成技术细节，作者声明已审阅。

- **触发环境与复现**：逐层fully_shard，checkpoint包住模块的__call__，use_reentrant=True，同一层在一次backward前执行多次。CPU、两个Gloo rank即可复现；原帖给完整torchrun脚本、固定种子、小型三层网络及未分片深拷贝参照。
- **预期与实际**：所有forward贡献的梯度应与未分片参照一致。PyTorch 2.11.0、2.15.0和2.16.0.dev20261104中，backward和optimizer.step均可通过；完整参数梯度相对误差在1次forward时为0.000，2次为0.385，4次为0.460。切换非重入checkpoint或仅根模块分片，其余9组对照均打印0.000。
- **修复状态与边界**：旧版2.10.0会报Tensor／DTensor混用错误，新版却变成静默漏算。报告者的项目使用私有成员操作绕过，不能视为可推荐的稳定修复；未见已合入修复或回归测试。
- **可复用检测方法**：测试“checkpoint类型×分片粒度×每次backward前forward次数”，直接比较完整梯度，不仅检查loss有限、反向无异常或优化器能够执行。表中的0.000是打印精度，不等于证明位级相同。

来源：[完整CPU双rank脚本、版本差分和梯度结果](https://github.com/pytorch/pytorch/issues/203482)。

**3. 正交参数化遇上AdamW：默认weight decay一步把整个权重变成零**

公开时间：11月5日09:26。作者使用非方阵Linear加正交参数化时发现，披露Claude辅助分析，并声明亲自运行、检查源码。

- **触发环境与复现**：macOS arm64 CPU，PyTorch 2.11.0和2.16.0.dev20261102。给Linear(32,64)应用orthogonal，使用默认AdamW，执行一次平方损失反向和step；原帖仅数行即可复现。
- **预期与实际**：正交权重不应退化为全零；范数从约5.657变为0。Householder路径把对角线中的±1符号转为整数；解耦weight decay把它缩成接近±0.9999，整数截断后变0。方阵显式选householder也复现，默认matrix_exp路径未见此问题。
- **修复状态与边界**：作者报告本地用sign替换int后保留正交性，并表示可以提交测试；尚未见已提交合入的修复。原帖没有给出完整正交误差容差、长程训练或性能矩阵，不能把局部检查写成全面验证。
- **可复用检测方法**：参数化层测试不能止于初始化、前向和梯度计算，还要跨过真实optimizer.step，并覆盖默认weight decay。范数可作为便宜的报警指标，正式验收还应检查正交约束。

来源：[最小脚本、根因和作者修复说明](https://github.com/pytorch/pytorch/issues/203496)。

**4. vLLM远程KV命中后的滑窗边界：最后一个prompt token重算时读到了空块**

公开时间：11月4日10:08。报告提供CPU调度器复现、GPU端到端对照，另有贡献者独立复现并提交候选PR。

- **触发环境与复现**：GPU报告为NVIDIA Thor、vLLM 0.29.0，gpt-oss-20b，NIXL／UCX，block size=16、window=128，关闭prefix cache并使用eager、TRITON_ATTN。CPU脚本在main ab4e04d2dc遍历prompt长度129至176。
- **预期与实际**：完整prompt KV加载后会重算第N−1个token，但滑窗块已按第N个token的窗口裁剪。CPU测试48种长度中，143、159、175三种读到null block，其余45种通过。GPU受影响长度的首token top-20 logprob最大差异，跨样本中位数0.30，对照相邻长度为0.06。
- **重要限制**：受影响长度的32个GPU样本，greedy首token均未改变；不能写成“这组测试证明必然答错”。这里更强的判定器是窗口需要的KV位置不得映射空块，而不是文本肉眼看起来不同。
- **修复证据**：PR #65584对滑窗模型在NIXL和Mooncake传输中回退一个token，包含48长度的CPU回归，作者报告48/48通过。已核对代码补丁，但PR仍开放，未见修复后GPU端到端验证，不能称已修复发布。
- **可复用检测方法**：缓存测试按“窗口长度、块大小、prompt长度余数”取边界及相邻点，并分别验证调度状态、KV完整性与模型数值。

来源：[原始报告、CPU脚本和GPU对照](https://github.com/vllm-project/vllm/issues/65550)；[候选修复及实际提交的测试](https://github.com/vllm-project/vllm/pull/65584)。

**5. GLM工具调用陷入推理循环：聊天模板和约束语法对thinking状态理解不同**

公开时间：11月4日14:20。不是新模型能力发布，而是GLM-5.3-Flash部署中的可复现兼容性问题。

- **触发环境与复现**：SGLang 0.5.22，8×L20X、TP8／EP8，PyTorch 2.14.1+cu130，FP8模型，自动解析器解析为glm45／glm47。请求同时带tools与enable_thinking=false；原帖包含完整服务命令和客户端对照。
- **预期与实际**：模板忽略enable_thinking并始终开启think，但工具语法把thinking块设为空，还禁止结束think的特殊token。简单乘法和天气请求均耗尽2048 token，正文为空、无工具调用；改成true或省略，分别55 token得到391、40 token产生工具调用。
- **更接近Agent负载的证据**：约10K prompt、10个工具、reasoning_effort=low下，false组8次中5次耗尽上限，true组0/8，省略组0/16。因此不能把标题中的“always”推广为所有长prompt请求必现。
- **修复状态与边界**：作者建议对该模型省略false，用reasoning_effort控制深度；未见上游修复。v0.5.23仅检查源码路径未变，没有实际重跑，必须与0.5.22运行证据分开。
- **可复用检测方法**：模型接入验收覆盖“tools有无×thinking参数缺省/true/false×模板与parser组合”，同时检查finish_reason、有效正文、工具调用和token消耗，不能只看HTTP成功。

来源：[部署参数、客户端脚本、短长prompt结果及版本边界](https://github.com/sgl-project/sglang/issues/48336)。

**6. AI辅助分析CI：性能测试失败可能跟着机器走，重试变绿也不代表代码修好了**

公开时间：11月4日11:48，后续评论补充H100样本。报告明确标注由Claude Code整理，提供公开CI作业链接，但没有独立人工审阅声明。

- **发现方法与证据**：按runner、模型阶段和历次尝试聚合CI数据，而不是只看最终红绿灯。B200同一个Qwen图像任务七次尝试，端到端约19.5至29.4秒，最后一次通过；5090池迁移后，Wan解码超阈值从10/60次变成45/78次。
- **新增H100对照**：10月30日至11月4日主分支样本中，旧novita池24次通过、0失败；computable-d池0通过、4失败，各失败作业均耗尽七次尝试。35个可比较用例的模型加载时间比值中位数1.57，23个去噪阶段比值中位数1.04。
- **判定与限制**：现象支持把主机环境列为重要混杂因素，不足以证明具体原因就是共享租户、温度、磁盘或网络；作者也明确这些原因尚未确认。跨时间、跨池比较不是严格受控实验。
- **修复与建议严格分开**：微基准健康检查、换runner重试、分池基线、临时改成警告，均是讨论方案，没有验证为已解决。可以借鉴“阶段耗时＋机器身份＋历史基线”的分析方法，但不能简单放宽阈值掩盖真实回归。

来源：[B200／5090报告与公开CI入口](https://github.com/sgl-project/sglang/issues/48311)；[H100加载耗时分层对照](https://github.com/sgl-project/sglang/issues/48311#issuecomment-6637835464)。

**7. 近7日已合入案例：vLLM-metal补上大内存Mac才触发的GDN恢复测试**

PR于11月3日14:08合入。此项是已合入修复，不是今天新发现，也不等于已进入特定发行包。

- **发现与环境**：M5 Pro、48GB、macOS 26.5.1，vLLM 0.32.0、PyTorch 2.13.0。Qwen3.5-0.8B在默认约28GiB KV池下，123和533 token prompt正常，563 token便使engine退出；缩小KV预算可以避开。此前8GB Mac测试未触及边界。
- **根因与判定**：恢复分块prefill时把共享大buffer内的GDN状态视图传给MPSGraph。非零偏移后仍有超过INT_MAX个元素，触发尺寸异常；decode路径已有拷贝，不受同一路径影响。
- **修复验证**：修复对状态slot做clone。作者报告六个123至3003 token prompt、每个生成48 token，在大／小KV池下都与小池参照的greedy token IDs相同。
- **确实合入的测试**：已核对PR补丁包含test_gdn_state_carries_across_chunked_prefill_and_decode的pooled分支，构造超过INT_MAX个FP32元素的buffer；标记slow，并在设备容量不足时跳过。作者报告该分支修复前失败、修复后通过。
- **不能混淆的两组结果**：6247 passed、17 skipped来自排除slow的测试命令，不能用这个总数声称大buffer回归已由该命令覆盖。大buffer用例的通过另由作者报告；尚未证明所有Apple Silicon或负载下性能。PR标注Claude Code生成。
- **可复用检测方法**：让测试覆盖“更大设备配置产生更大默认分配”的边界；小机器上正常不代表大机器更安全。高内存回归要明确标记跳过原因，避免绿色CI被误读为已覆盖。

来源：[已合入PR、前后结果与测试说明](https://github.com/vllm-project/vllm-metal/pull/1244)；[合并提交](https://github.com/vllm-project/vllm-metal/commit/a44be48731005932a24dd30b55c21e992dc90b9a)。

**今天值得优先借鉴的两项**

1. **训练正确性先测贡献是否完整**：第1条的尾项稀疏输入、第2条的未分片梯度参照，能发现“结果有限、训练不崩溃”掩盖的错误。前向通过、反向通过、更新正确，应分开验收。
2. **推理回归重点覆盖状态转换**：远程KV加载后重算、模板切换到工具语法、GDN恢复prefill，比单次稳定态调用更容易暴露接口边界错误。对每个边界同时检查结构性不变量和最终输出。

**本期判断**

AI在这些案例中更可核验的作用，是帮助整理复现、缩小触发条件、聚合CI日志与提出候选修改。本期没有证据支持“AI已普遍自主完成发现和修复”。最值得复用的是明确的判定器、版本和配置对照，以及把“作者报告通过”“PR包含测试”“测试已合入”“发行包已修复”四种状态严格分开。
