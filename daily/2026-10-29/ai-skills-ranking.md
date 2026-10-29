# AI 独立 Skill 热度双榜｜2026年10月29日

总Stars前十名次未变；同窗口净增由Impeccable领跑，Cloudflare第二。Hallmark也有较快增长，但比较窗口不同，单列。

统计范围：复核11个已有候选的README、文件树与技能定义，并补充Trending、关键词及关联候选；这是已检索样本排名，不是GitHub全网穷尽榜。UI UX Pro Max继续按7技能分发排除；Graphify的内含Skill不使用整个软件仓库Stars。

观察时间：北京时间10月29日09:34:05–09:34:10，重新读取公开GitHub API，并按仓库数字ID匹配历史。下列总Stars均来自本批次。

一、总Stars前十

1. [Karpathy Guidelines](https://github.com/multica-ai/andrej-karpathy-skills)｜226,004
减少过度工程、明确假设和验证目标，适合研发与QA；Claude Code、Cursor。README/Skill声明MIT，API许可证字段为空；最近推送仍为4月20日。

2. [Taste](https://github.com/Leonxlnx/taste-skill)｜104,511
网站美术指导、素材选择与截图检查，适合前端工程师；Claude Code、Codex等Skill宿主。MIT；需要Node 22+及Chromium系浏览器，素材搜索涉及联网。

3. [Impeccable](https://github.com/pbakaus/impeccable)｜92,757
UI设计、审查和规则执行，适合前端、设计工程及GUI质量团队；Claude Code、Codex、Cursor、Copilot、Hermes等。Apache-2.0；部分hooks在模型命令审批之外运行，首次编辑可能下载引擎，需先审查执行边界。

4. [Archify](https://github.com/tt-a1i/archify)｜91,009
代码与架构转交互图解，适合架构师、文档作者；Claude Code、Codex、Cursor、OpenCode。Apache-2.0；图形结构校验不保证实际架构语义正确，有可关闭的更新清单请求。

5. [Last30Days](https://github.com/mvanhorn/last30days-skill)｜65,794
近期多源调研，适合研究、产品及内容团队；Claude Code、Codex、Cursor、Copilot、Gemini CLI等。MIT；第三方API、付费来源及浏览器会话的权限和成本需单独确认。

6. [Diagram Design](https://github.com/cathrynlavery/diagram-design)｜62,465
生成HTML/SVG图解，适合技术文档及汇报；Claude Code、Codex、Cursor等Agent Skills宿主。MIT；仍需人工核对连线、数值与来源。

7. [Humanizer](https://github.com/blader/humanizer)｜61,959
改善机械化文风，适合编辑与写作者；Claude Code、Codex、Claude.ai/Desktop等适配。MIT；改写不保证事实正确，也不保证规避AI检测。

8. [I Have ADHD](https://github.com/ayghri/i-have-adhd)｜59,232
让回答行动优先、步骤简短，适合偏好低阅读负担的人；Claude Code、Codex、Cursor、Gemini、Copilot等。MIT；是表达风格，不是医疗干预。

9. [Cloudflare Security Audit](https://github.com/cloudflare/security-audit-skill)｜51,710
多Agent代码安全审查和证据验证，适合安全及平台QA；需要工具调用、并行子Agent和Node.js。MIT；最近推送9月14日。缺少OS级隔离时，保留“待验证”结论，不应直接运行目标代码。

10. [Hallmark](https://github.com/Nutlope/hallmark)｜40,764
品牌页面设计、明确授权后的改版及审计，适合前端和设计团队；Claude Code、Codex、Cursor、OpenCode。MIT；基础脚本需Node 18+，浏览器检查另需Chrome及puppeteer-core，照片各有许可，不能统一视为MIT。

二、同窗口Star净增前十

窗口：北京时间10月28日09:28:30→29日09:34:05，约24小时5分36秒。按总Stars快照相减排序，括号为相对起点的增长率；不是严格24小时，也不与Trending的today/this week混排。

1. Impeccable：+2,182（2.41%）
2. Cloudflare Security Audit：+1,484（2.95%）
3. Taste：+801（0.77%）
4. Diagram Design：+644（1.04%）
5. Archify：+536（0.59%）
6. Karpathy Guidelines：+492（0.22%）
7. Humanizer：+450（0.73%）
8. I Have ADHD：+174（0.29%）
9. [Book to Skill](https://github.com/virgiliojr94/book-to-skill)：+121（0.33%），总Stars 36,427
10. Last30Days：+97（0.15%）

重合项目说明见总榜。Book to Skill将书籍、文档转为按需加载的知识Skill，适合学习和知识管理；支持Claude Code、Codex、Copilot CLI、OpenCode等。MIT不覆盖输入资料版权，生成内容不宜擅自公开。

不同窗口单列：Hallmark从39,051增至40,764，净增1,713（4.39%）；窗口为10月28日09:33:35→29日09:34:05，约24小时30秒，比共同窗口短约5分钟，不插入上榜十项。今日已统一采集并保存，后续可按共同基线比较。

7日净增仍缺历史总量快照。另核查了GitHub新历史API：它按当前仍保留的Stars创建日期聚合，取消Star的记录已移除，不能替代历史净增。[GitHub口径说明](https://github.blog/engineering/architecture-optimization/from-2500-requests-to-24-rebuilding-star-history-without-the-stargazers/)

变化与建议
- Impeccable超过Cloudflare成为本期净增第一；其总Stars领先Archify的差距由102扩大至1,748。不同日期窗口长度有差异，不据此计算精确加速度。
- Impeccable在10月28日main加入Hermes design hook及可信项目检查，并修复重复问题报告；最新稳定版仍是26日skill-v4.8.0。[提交](https://github.com/pbakaus/impeccable/commit/b4d55bb73a83585c4fb1b0e70f9ea39f22fe7ced)
- 优先小范围验证Impeccable：用非敏感页面比较纯Skill审查与hook执行后的收益、权限及网络行为。
- 其次读Hallmark：适合从零做品牌页面，先用审计模式检查规则是否有帮助。其28日提交主要是致谢和NOTICE调整，热度上涨不能解释为新功能发布。

热度不等于成熟度、安全性或生产效果。本期未安装、执行项目或进行外部写操作。
