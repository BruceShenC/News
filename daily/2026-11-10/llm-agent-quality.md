**大模型与 Agent 质量简报｜2026-11-10**

观察窗口：北京时间11月9日09:30至11月10日09:30，结合近7日资料。以下6条与11月9日质量简报去重。日期均为北京时间；全部运行数字来自作者报告，本次未运行模型或复现代码。尚在讨论中的问题、候选修复和RFC，不代表已发布能力。

**1. vLLM：流式文本正确，logprobs却可能丢失，需要逐事件验证**

11月9日16:11公开的问题显示，Responses API虽然在delta、content_part.done、output_item.done及completed事件中携带logprobs，但output_text.done硬编码返回空列表。

- **发现与复现：** 原帖提供事件处理器级最小脚本，环境为main 821851e、Python 3.12.16、openai 3.46.0、pydantic 2.14.3。构造一个“Hello”token及logprob=-0.12，经过open、emit_delta、close_current后，done事件应包含1项，实际为0。无需下载模型；这是协议层测试，不是模型推理测试。
- **修前修后：** PR #66513将累积值转换为done事件所需类型。作者展示修前两项断言失败；修后test_streaming_events.py为17通过，指定serving测试为1通过、90未选中。PR仍开放，未核实上游完整CI成功，不能写成已合并。
- **另一个不同根因：** 11月9日08:19公开的#66436略早于24小时窗口，但其修复和审查在窗口内推进。Qwen3-4B输出“🪿”需要3个token，前两个增量解码文本为空，Responses路径提前跳过，最终只保留最后一个token的logprob。原帖给出服务启动、curl及SSE记录，并与Chat Completions作差分。
- **候选补丁仍有缺口：** #66449提出缓存空文本步的logprobs，但11月10日06:45的审查指出：缓存只进入最终结果，下一条文本事件仍缺失；新测试还创建了可见token的logprob却没有传入处理器。作者因环境缺PyTorch未能启动pytest，不能把“新增测试”当成“测试已通过”。
- **可迁移方法：** 按token与字节重建流，分别核对delta、各类done和completed；加入多token Unicode、空文本步、reasoning/tool切换及流关闭。最终文本一致并不足以证明概率数据完整。该方案属于建议，尚未自行执行。

来源：[done事件问题及最小脚本](https://github.com/vllm-project/vllm/issues/66507)；[修前修后测试](https://github.com/vllm-project/vllm/pull/66513)；[Unicode复现与SSE入口](https://github.com/vllm-project/vllm/issues/66436)；[缓存补丁与审查意见](https://github.com/vllm-project/vllm/pull/66449)。

**2. PyTorch/cuSPARSE：只检查batch 0，会漏掉后续批次的静默错算**

11月10日05:24公开。作者在测试运行时库版本相关改动#202968时发现：batch内CSR稀疏结构不同、加载cuSPARSE早于12.7时，torch.bmm的第0批正确，后续批次错误。

- **环境与复现：** RTX PRO 4000 Blackwell/sm_120，驱动610.57；torch 2.16.0.dev20261102+cu130搭载cuSPARSE 12.6.3。原帖十余行脚本构造3个2×5矩阵，每批非零元素数量相同、位置不同，转CSR后乘3×5×2的稠密矩阵。
- **判定依据：** 同一输入的稠密torch.bmm作参考，逐batch计算最大绝对误差。作者一次运行得到0、5.741、9.485；脚本使用随机右矩阵且未固定seed，这些数值不是可照搬的阈值。
- **交叉验证：** torch 2.16.0.dev20261109+cu132、cuSPARSE 12.7.10下各批正确。作者还报告独立C程序在12.5.8/12.6.3复现、12.9.4正确；另一开发者在CUDA 13.0.2原生调用中复现，错误符合所有批次误用batch 0行偏移的结果。
- **修复与限制：** issue仍开放。对旧库转COO并用COO_ALG2是候选回退方案，作者报告该路径正确，但未核实已合入回归测试或正式修复发布。
- **价值：** 稀疏算子验收应交叉覆盖相同/不同稀疏结构、多个batch及实际加载的数学库版本。不能只记CUDA版本，也不能只抽查第一批。是否影响某个LLM训练任务仍取决于该任务是否使用此路径。

来源：[发现过程、最小复现及版本对照](https://github.com/pytorch/pytorch/issues/204102)。

**3. MPS：混合dtype的非法输入，应报错却直接终止进程**

11月10日07:02公开。报告者在Mac上让SAM3以BF16运行时遇到问题：嵌套autocast后出现不匹配dtype，CPU抛出可捕获异常，MPS触发Metal断言并Abort。

- **环境与复现：** Apple M5 Max、macOS 27.2、Python 3.13.18、torch 2.16.0.dev20261109。原帖将8×16的BF16输入与4×16的FP32权重送入F.linear，先运行CPU，再搬到MPS。
- **预期与实际：** CPU返回“mat1 and mat2 must have the same dtype”的RuntimeError；MPS输出NDArray目标与累加器dtype不一致的断言，进程以Abort trap: 6结束。要求不是让非法混合输入计算成功，而是可控拒绝，避免整个服务或测试进程退出。
- **修复与限制：** issue开放，本次未检得关联修复及修后测试。SAM3嵌套autocast的说明属于作者报告，已公开的最小脚本验证的是线性算子的错误处理边界。
- **可迁移方法：** 用独立子进程执行预期失败用例，比较后端异常类型、退出码与服务存活性；覆盖输入/权重/偏置dtype组合及autocast嵌套。它是稳定性测试，不是正常输入的数值精度测试。

来源：[原始场景、脚本、错误日志与环境](https://github.com/pytorch/pytorch/issues/204125)。

**4. PyTorch：两个结果相加仍为1，也可能一起丢失Float64精度**

11月10日07:25公开。报告者将gammainc/gammaincc与认证高精度参考值比较，发现a>20且x接近a时，Float64可能只剩约9位有效数字；定位到渐近展开分支使用单精度π常量。

- **环境与复现：** torch 2.15.0+cpu、Windows 11、Python 3.12.13。令a=x=25并使用float64，gammainc实际为0.5266015310735231，Arb参考约0.5266015314436506；gammaincc也偏离，但两者仍相加为1，因此互补性测试会漏报。
- **发现与分析：** 作者明确披露Claude参与分析、另一Agent重跑，作者已审阅。源码模板的double实例仍使用c10::pi<float>，估算误差与观测量级一致。
- **修复验证的边界：** 作者用Python复刻公式检查53个落入该分支的测试点：单精度常量时51个误差超过1e-12，换双精度π后没有超过，最坏相对误差7e-15。这是公式复刻对照，不是修改PyTorch并完成原生构建后的回归结果。
- **状态与价值：** issue开放，尚无已验证的合入测试。main源码仍见相同常量，但作者未在nightly运行。此案例适合学习“高精度外部参考＋误差预算＋常量dtype检查”；不是已证实的LLM训练退化，也不能由AI参与推导出测试效率提升。

来源：[最小调用、Arb参考、53点实验及AI使用披露](https://github.com/pytorch/pytorch/issues/204127)。

**5. SGLang：profiling状态冲突被包装成500，旧测试本身也可能固化错误契约**

11月9日15:29公开。连续停止profiling，或profiling已自动完成后再停止，会返回裸HTTP 500和ASGI traceback；启动已在运行的profiling也走同一路径。

- **复现与发现：** 原帖给出依次POST start_profile、stop_profile、stop_profile的三个curl命令。作者观察环境为Python 3.12；源码c394281存在该路径，但完整环境信息缺失，也未在最新main完成GPU复现。
- **判定依据：** scheduler已返回success=False及明确状态消息，管理层却转换成通用RuntimeError。候选API契约是用结构化409表达已知状态冲突，成功仍为200，意外运行时故障仍为500。
- **测试变化：** PR #49266更新重复stop、自动完成后stop、运行中start的集成测试；旧main测试原本明确期待500。作者仅执行了从AST提取生产函数的CPU冒烟测试，覆盖200/409/500，未覆盖完整SGLang导入、自定义HTTP异常处理器或GPU模型链路。
- **状态与限制：** PR仍开放；页面显示Base、Extra、AMD相关检查失败，以及标题/Lint门禁问题，未把失败归因为功能回归。Rust前端保持原行为，不能宣称所有前端都统一为409。
- **价值：** 自动化测试的预期也要对照接口语义审查。建议监控区分状态冲突与服务故障，并分别验证Python/Rust前端，而非仅将现有断言继续复制。

来源：[问题及curl复现](https://github.com/sgl-project/sglang/issues/49261)；[候选修复、实际验证范围及检查状态](https://github.com/sgl-project/sglang/pull/49266)。

**6. vLLM KV offload：更早“健康”不等于更早具备P/D服务能力**

11月9日20:34发布RFC。作者针对大CPU缓存层初始化提出异步化方案，同时披露CUDA graph竞争、注册内存导致服务卡顿及多worker就绪状态问题。这是设计提案及原型实验，不是已正式发布的稳定能力。

- **原始发现与证据：** 大缓存初始化实验显示，GLM-5.3、16张H100、4个pod、每rank 280GiB时，pre-fault约117秒、pinning约12秒。连续cudaHostRegister还会争用驱动锁；微基准相对无后台线程的3.7ms步时，9MiB连续注册约慢303倍，1GiB块约慢5,800倍。
- **方案及已报告结果：** 将预触页、pinning移出启动关键路径，并在注册间插入等待。Qwen3-8B使用dummy权重、TP=4、1,250GiB缓存的3次实验中，/health就绪由241.5秒降到57–61秒，但CPU tier约在健康后160秒才就绪，P2P约197秒。不能把健康时间改善等同于完整缓存能力启动提速。
- **测试风险：** readiness必须按不同rank去重，不能按报告次数累计；晚挂载的tier不能处理此前未收到on_new_request的请求。作者还指出，异步提交内存可能把pod内存不足的OOM从启动期推迟到服务期。
- **关键限制：** P2P tier的P/D模式第三阶段尚未实现，也未测试async_init下的P/D模式。部分GLM实验使用未加pacing的早期构建，默认配置下tier就绪可能更晚；完整微基准脚本未附，不能声称可一键复现。
- **可迁移验收：** 将进程健康、模型可服务、CPU缓存可用、P2P可用及P/D可用分别设为状态，测试空闲实例首次请求、启动时流量、worker重复报告、tier失败、重启和内存不足；同时测TTFT与延迟分位数。以上是本简报建议，并非项目已全部执行的回归。

来源：[RFC、实测数据及未实现范围](https://github.com/vllm-project/vllm/issues/66570)；[此前graph capture故障](https://github.com/vllm-project/vllm/issues/46933)。

**本期优先验证的两项**

1. **流式API数据完整性。** 从多token Unicode开始，建立各类事件与最终结果的对照，比只检查文本和HTTP成功更容易发现隐蔽协议缺陷，且可以先做低成本事件处理器测试。
2. **分层就绪与异常隔离。** 对推理平台分别验证“健康、缓存、P/D就绪”，并把非法输入造成的进程崩溃纳入负向测试，防止启动优化把风险转移到线上。

本期主要是“测试AI系统”；AI帮助测试的明确材料来自vLLM修复及PyTorch精度调查中的披露，尚不足以量化效率收益。未找到足够新增、可核实的训练检查点恢复或GUI自愈成果，因此不以旧闻填充。模型正式发布已由同日技术热点简报覆盖，本份不再展开。
