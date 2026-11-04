**大模型与 Agent 质量简报｜2026-11-04**

观察截止：北京时间 2026-11-04 09:31。主窗口：11月3日09:31至11月4日09:31，结合近7日背景；以下7条均为窗口内新公开报告，不是7个已发布修复。已与11月3日简报去重。本期重点是可复现质量问题，没有用未经核验的模型发布或价格新闻填充栏目。

**1. CPU 编译求和：用 float64 对照检出约一个数量级的精度差**
- 公开时间：11月4日04:40；PyTorch 问题报告，仍开放。作者在改进 CPU reduction 时发现 eager 与编译结果不同，并披露使用 Claude 定位、准备初版修复，随后人工检查代码和结果。这是“AI辅助缺陷分析”与“测试AI底层系统”的交叉案例，不是已证明的自主发现。
- 复现与判定：i7-11800H、AVX512、WSL2，float32 随机矩阵4096×4096，分别沿0和末维求和；以输入转成float64后的求和结果为数值参照，计算相对L2误差。作者报告 dim=0 编译前8.3e-7，eager 6.1e-8；末维误差接近。原帖有最小Python脚本。
- 修复证据与缺口：作者原型按向量lane数缩短纵向累加分块，4096×4096案例降到1.1e-7，并报告1/4线程、mean和vector_norm对照。test_vertical_sum_accuracy 据称在个人分支准备好，未核实为已合入测试；具体PyTorch提交、随机种子、通用容差未披露。速度变化范围较大，不能据此承诺性能无损。
- 价值：训练统计量、损失归约和CPU推理都应覆盖“归约维度×形状×线程数”，不能只测最后一维。
来源：[PyTorch #203279：脚本、前后数值和测试说明](https://github.com/pytorch/pytorch/issues/203279)。

**2. FlexAttention：掩码更新被忽略，运行成功却把全1结果变成全0**
- 公开时间：11月4日09:09；PyTorch问题仍开放。原始发现过程未披露；报告提供多后端差分输出，根因段明确标为 Claude 生成分析。
- 复现与判定：CPU源码构建 torch 2.16.0a0+gite5fed85、Python3.11.4。构造全1的(1,1,2,16)张量，在编译函数中把2×2布尔mask经scatter全部写成True，再让score_mod读取它。数学预期输出全1；eager、aot_eager及其decomp模式符合，Inductor的default/reduce-overhead/max-autotune均报告全0。index_put、index_fill、索引赋值也报告同样问题。
- 修复证据与缺口：作者评论提供个人fork的一行修改，尚非上游合入修复；未给出完整修复后矩阵、GPU验证或回归测试覆盖。不能把CPU结果直接推广为所有GPU均受影响。
- 价值：注意力测试不仅要比较算子输出，还要覆盖捕获张量先修改、再由子图读取的语义。
来源：[问题与完整复现](https://github.com/pytorch/pytorch/issues/203311)、[作者修复说明](https://github.com/pytorch/pytorch/issues/203311#issuecomment-6619776565)。

**3. FSDP2：独立tuple输出的分布布局被混合，CPU即可做训练语义检查**
- 公开时间：11月4日03:00；问题仍开放。原始发现是线上训练还是开发测试未披露，不能称为生产事故。
- 复现与判定：torch 2.16.0.dev20261030+cu132，提交bf270c3251d8e84723cedbf5b60eaa60cacaa3f2；spmd_types0.3.1、Python3.12.13、Linux aarch64。无需初始化进程组，给RegisterPostBackwardFunction输入一个sequence-sharded residual和一个replicated shared-KV张量，用typecheck比较各输出对应输入的布局。第二个布局从R变V，身份映射应保留布局的断言失败。
- 修复证据与缺口：作者报告逐输入identity规则可修复复现，两个独立加权输出的梯度分别为2、3，post_backward仅调用一次。尚未核实上游合入，也没有真实多机训练、检查点恢复或收敛曲线证据。
- 价值：这是低成本的分布式训练前置检查，但只能证明类型与局部梯度契约，不能替代真实分布式运行。
来源：[PyTorch #203261：CPU最小复现、版本和梯度验证](https://github.com/pytorch/pytorch/issues/203261)。

**4. SGLang PD分离：KV布局不一致时，传输“未报错”也可能已损坏答案**
- 公开时间：11月4日05:03；问题仍开放，报告另含10月28日nightly的失败日志，不能把底层问题起点写成本日。
- 发现与复现：报告通过准确率、负载和传输日志对照定位，未说明最初由哪一种告警发现。GLM-5.2-NVFP4、GB300 NVL72，每角色4卡；11月3日nightly叠加main 4387a94cf2、Mooncake0.3.13.post1、FP8 KV、64-token页。prefill使用flashmla_sparse/flashmla_kv，decode默认trtllm，形成每层每token 656B/576B布局差异。
- 判定：先跑50题GSM8K，再跑256个16k输入、64输出、并发64的请求。作者报告错误配置准确率0/50，负载83/256失败、78条地址错误；并提醒benchmark把部分无输出终止请求仍计为成功。匹配布局对照准确率0.88、0/256失败。
- 修复证据与缺口：对照行带有作者修复PR，因而不应描述成严格“只改一个配置”的纯A/B。建议注册peer时检查KV尺寸、及临时统一两侧backend；尚未核实修复PR合入或不匹配场景完整回归。
- 价值：成功率要同时检查非空输出、语义准确率、传输状态；只看HTTP成功或无异常不足以验证缓存正确性。
来源：[SGLang #48230：完整部署、评测及负载命令](https://github.com/sgl-project/sglang/issues/48230)。

**5. AWQ-MoE版本回归：同机最小问答能检出退化，但尚不能确认是哪一个kernel**
- 公开时间：11月4日09:20；vLLM新问题，仍开放。
- 发现与复现：报告者给出版本差分及IFEval结果，但未披露最初发现途径。4×RTX6000D、sm_120、TP4，QuantTrio/GLM-4.7-AWQ，4bit/group128/zero-point；eager、上下文8192、temperature0、关闭thinking，问“1+1等于几？”，最多300token。v0.14.0报告输出2并stop，v0.25.1及若干较新版本重复错误句直至length。
- 判定与限制：以已知答案、重复循环、finish_reason及评测准确率组合判定。旧版IFEval严格prompt准确率约0.90、空响应0；新版完整指标未给出。旧容器torch2.9.1+cu129，新容器torch2.11.0+cu130，软件栈并非完全一致，因此“确定是编译AWQ-MoE kernel”仍是作者定位假设，不是已隔离结论。
- 修复证据与缺口：报告称切换norm实现、还原sum仍失败；未见修复、固定logit差异阈值或自动回归测试。不能据此宣布所有Blackwell/AWQ模型受影响。
来源：[vLLM #65533：容器参数、请求、结果和排查记录](https://github.com/vllm-project/vllm/issues/65533)。

**6. 多模态预计算embeddings：CUDA Graph开关组合可使整个engine退出**
- 公开时间：11月4日09:16；vLLM问题仍开放。作者在给Intern-S1添加encoder CUDA Graph时发现，并用InternVL3-1B复现；明确披露Claude Code辅助追代码、运行GPU复现和写报告。
- 复现与判定：main 0c63ce9f0、单L40S、torch2.14.0+cu130；enable_mm_embeds与cudagraph_mm_encoder同时开启，传入随机BF16 (1,256,896) image embeddings、max_tokens8。预期是绕过视觉编码器并完成请求，实际输出数量0而期望1，继而EngineDeadError；关闭encoder图，同脚本报告可返回文本。随机embeddings的文本不具有语义质量判定价值。
- 修复证据与覆盖：作者报告在CPU调用真实manager检出多种图像及视频模型路径；建议按batch是否含embeds选择eager旁路，并称准备了CPU单测及Qwen2-VL像素/embedding、V1/V2 GPU测试。尚未核实已提交合入、修复后结果或所有模型覆盖。报告中GLM-4.1V只做代码判断，未实测。
- 价值：适合给多模态服务增加“原始像素/预计算embedding×图开关×runner版本”兼容性矩阵，避免只测单个开关。
来源：[vLLM #65531：完整脚本、崩溃栈与测试范围](https://github.com/vllm-project/vllm/issues/65531)。

**7. CPU KV offload：“Async”API仍可能长时间阻塞工作线程**
- 公开时间：11月4日06:31；vLLM性能改进提案，非正式发布能力。
- 发现与判定：作者通过CPU提交时间与DMA时间对照分析，未披露首个报警来源。单B200、同为6.6GB传输，161k个40KB描述符在cuMemcpyBatchAsync内耗时110ms，而DMA123ms；合并到10k个640KB描述符后CPU耗时3.1ms、DMA约123ms。API名称不保证host线程立即返回。
- 端到端证据：GLM-5.2-NVFP4、vLLM0.31.0、8×B200/TP8、880GB CPU tier、AgentX负载并发32。作者30分钟对照中，按步限量提交的原型吞吐706→859 output tok/s，平均TTFT23.8→17.4秒。
- 修复与限制：原型默认每步8192描述符，6.6GB加载约摊到20步；负载完成时间与公平性需另测。作者称main 28dd9c36b5上有单测，尚未核实PR或合入。未给完整可复制负载命令、数据与profiling脚本、统计重复次数、尾延迟和输出正确性结果；不能把单机平均收益当通用结论。
来源：[vLLM #65515：微基准、原型设计和服务对照](https://github.com/vllm-project/vllm/issues/65515)。

**值得小范围验证的两项**
1. 优先阅读并移植第1条CPU数值差分方法：不需大模型权重，成本低。建议固定种子，对sum/mean/norm覆盖不同归约轴、尺寸和线程；以float64及eager双参照，分别记录误差与性能。容差应依据业务与dtype定义，不把上述测量值直接当统一阈值。
2. 若使用多模态vLLM，优先审阅第6条开关矩阵：只在隔离环境验证请求是否完成、engine是否存活、输出项数是否正确，再用有效embedding补语义等价性测试。原帖使用trust_remote_code，验证前须审查模型远程代码，勿将随机embedding“能生成文字”视为质量通过。

**范围与证据边界**
本期检索并阅读公开PyTorch、vLLM、SGLang及verl近期问题，精选有可检查复现或测量证据的7条；不代表整个AI测试领域的穷尽扫描。未重复前期DTensor FP8、Hermes解析、max_tokens16、Marlin确定性等案例。AI辅助部分仅采用作者明确披露的代码定位、复现和分析记录，本期未另选证据不足的GUI自愈或用例生成营销消息。

上述数字为原作者报告，本简报未安装、运行项目或自行复现。全部7个问题在观察时仍开放；个人分支、拟提交单测、原型测量均不等于上游修复发布。整体启示是把“请求完成”“输出正确”“数值精度”“性能容量”分别验收，并用真实调用边界而非孤立单测补足回归覆盖。

[本期归档](https://github.com/BruceShenC/News/blob/main/daily/2026-11-04/llm-agent-quality.md)
