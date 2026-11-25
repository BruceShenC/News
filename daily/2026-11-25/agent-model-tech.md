# Agent 与大模型技术热点｜2026-11-25

**窗口：北京时间 11 月 24 日 09:30 至 25 日 09:30。** 没有公开时分的来源只标原页日期。今天较值得看：Workspace 将人工确认与审计纳入官方 Agent 接入；开放模型则仍要分清**能下载、能跑起来、能上线**。

1. **Google Workspace MCP／CLI 正式可用，逐步推送。** 11 月 24 日公告：云端 Agent 接托管 MCP，终端可用官方 CLI；管理员可控访问、配人工确认、查审计，原有用户级防泄露策略继续适用。Google 计划从 2027 年 2 月 1 日起对 Agent 类访问执行 MCP 接入要求，不是现在全面停用 REST API。[官方公告](https://workspaceupdates.googleblog.com/2026/11/mcp-servers-cli-standardized-usage-tiers-now-available.html)
2. **百度 ERNIE 5.5 Flash Preview：开放的是预览权重，不是 API 首发。** 24 日北京时间 11:43 官方宣布。280B 总参数、14B 激活的 MoE，支持 100 万 token；模型卡列混合低精度、FP8、BF16，Preview 带 DSpark 草稿头。仍是后训练中间检查点；Apache 2.0 不外推到 Base，最大上下文不等于各精度、各硬件性能相同。[发布帖](https://x.com/Baidu_ERNIE/status/2125072735336546546) · [模型卡](https://huggingface.co/baidu/ERNIE-5.5-Flash-Preview)
3. **腾讯 Hy-MT3：把术语与格式一并纳入翻译。** 24 日官方发布 1.8B、7B、30B-A3B 三档 Apache 2.0 权重，覆盖 102 种语言，同时发布 TransRubric。对游戏本地化，译对但破坏占位符、富文本或术语一致性仍不能交付；语种覆盖不等于质量一致，图片示例使用 OCR，不代表权重能直接处理图像。[官方仓库](https://github.com/Tencent-Hunyuan/Hy-MT3)
4. **OpenAI Agents Python SDK 0.26.0：明确工具元数据优先级和实时回合边界。** 24 日发版。MCP 同名元数据依次取调用显式值、动态解析、发现默认值。实时回合结束会检查剩余文本与音频转写，输入护栏拒绝可阻止仍排队的函数工具。参数优先级不等于授权优先级，拦下队列任务不能撤销已发生的外部操作。[发布说明](https://github.com/openai/openai-agents-python/releases/tag/v0.26.0)
5. **Ling-3.2-air 补 vLLM 部署指南，尚未做 GPU 验证。** 24 日为文档更新，模型更早首发。MIT 多模态 MoE，124B 总参数、5.5B 激活；指南列约 262K 原始上下文，以及 YaRN 扩展至约 100 万 token 的路径。依赖 vLLM 0.35.0 nightly，不能用来承诺稳定版兼容或容量；长上下文会增加缓存需求，也可能影响短输入。[指南](https://recipes.vllm.ai/inclusionAI/Ling-3.2-air) · [模型卡](https://huggingface.co/inclusionAI/Ling-3.2-air)

**两点判断：** Agent 接入先说清身份、确认和撤销边界，再看工具数量。模型上线按权重、引擎版本、精度和上下文配置的组合分别验收。仅核对公开资料，未运行。
