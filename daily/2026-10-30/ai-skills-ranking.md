**AI 独立 Skill 热度双榜｜2026年10月30日**

总 Stars 前十顺序未变。Impeccable继续领跑近期净增；Hallmark本期已有统一基线，进入净增榜第二。

**统计口径**
观察时间：北京时间10月30日09:28:35–09:28:47。重新读取公开GitHub API，11个仓库数字ID与昨日一致；复核完整文件树，3个变化仓库重读README及技能定义，其余8个文件树SHA与昨日已核验版本一致。

补充检索agent-skills主题、名称含skill且超过4万Stars的仓库及关键词搜索。Ponytail、Nature Skills、Academic Research Skills、Reverse Skill等多技能分发不入榜，REA等MCP/CLI产品不以整个仓库热度代表内含Skill。**这是已检索样本榜，不是全网穷尽榜。**总Stars取同一批次，不混入稍晚搜索结果；精确快照已保存。

**一、总 Stars 前十**
1. [Karpathy Guidelines](https://github.com/multica-ai/andrej-karpathy-skills)：**226,458**
强调明确假设、最小改动和可验证目标，减少过度工程。适合研发与QA，支持Claude Code、Cursor等。README/Skill声明MIT，API许可证字段为空；最近推送4月20日。规则提示不等于质量门禁。

2. [Taste](https://github.com/Leonxlnx/taste-skill)：**105,574**
网站美术指导、参考素材与截图检查，适合前端和设计工程师；面向Claude Code、Codex等Skill宿主。MIT；需Node 22+及Chromium系浏览器，联网素材许可须另核实。

3. [Impeccable](https://github.com/pbakaus/impeccable)：**95,391**
UI设计、审查、可访问性和设计规则检测，适合前端、设计工程及GUI质量团队；支持Claude Code、Codex、Cursor、Copilot、Hermes等。Apache-2.0；自包含引擎首次运行可能下载二进制，hooks执行与信任边界需单独审查。

4. [Archify](https://github.com/tt-a1i/archify)：**91,450**
把代码、架构和解释性内容转成交互图解，适合架构师与技术文档作者；支持Claude Code、Codex、Cursor、OpenCode。Apache-2.0；结构校验不能证明架构语义正确。可关闭更新清单请求；贡献者内部辅助Skill不计作面向用户的多技能合集。

5. [Last30Days](https://github.com/mvanhorn/last30days-skill)：**65,896**
汇总近期多源讨论和公开资料，适合研究、产品及内容团队；支持Claude Code、Codex、Cursor、Copilot、Gemini CLI等。MIT；第三方API、浏览器会话、付费数据的权限与费用须分别确认。

6. [Diagram Design](https://github.com/cathrynlavery/diagram-design)：**62,924**
生成HTML/SVG图解，适合技术文档、方案评审与汇报；支持Claude Code、Codex、Cursor、Copilot等。MIT；近期文件树未变，连线、数值和引用仍需人工核对。

7. [Humanizer](https://github.com/blader/humanizer)：**62,371**
改写机械化表达，适合编辑及写作者；适配Claude Code、Codex、Claude.ai/Desktop等。MIT；改善文风不保证事实正确或规避AI检测。

8. [I Have ADHD](https://github.com/ayghri/i-have-adhd)：**59,373**
让回答行动优先、步骤简短，适合偏好低阅读负担的人；支持Claude Code、Codex、Cursor、Gemini、Copilot等。MIT；最近推送10月19日。它是表达风格Skill，不是医疗干预。

9. [Cloudflare Security Audit](https://github.com/cloudflare/security-audit-skill)：**52,886**
组织多Agent代码安全审查、证据复核与结构化报告，适合安全和平台QA；宿主需支持工具调用、并行子Agent及Node.js。MIT；最近推送9月14日。审查不等于漏洞已验证，更不能据此直接执行不可信目标代码。

10. [Hallmark](https://github.com/Nutlope/hallmark)：**42,118**
设计品牌页面，提供明确调用后的改版与审计模式，适合前端和设计团队；支持Claude Code、Codex、Cursor、OpenCode。MIT；基础脚本需Node 18+，渲染另需Chrome及puppeteer-core，照片各自遵循原许可。

**二、近期 Star 净增前十**
共同批次窗口：北京时间**10月29日09:34:05→30日09:28:36，约23小时54分30秒**。按两批总Stars快照相减排序，括号为相对起点增长率。批次采样有秒级先后，不是严格24小时，也不是Trending的today/this week口径。

1. Impeccable：**+2,634（2.84%）**
2. Hallmark：**+1,354（3.32%）**
3. Cloudflare Security Audit：**+1,176（2.27%）**
4. Taste：**+1,063（1.02%）**
5. Diagram Design：**+459（0.73%）**
6. Karpathy Guidelines：**+454（0.20%）**
7. Archify：**+441（0.48%）**
8. Humanizer：**+412（0.66%）**
9. I Have ADHD：**+141（0.24%）**
10. Last30Days：**+102（0.16%）**

项目说明同总榜。第11个候选[Book to Skill](https://github.com/virgiliojr94/book-to-skill)总Stars 36,513，同窗口净增86，未入本期两榜前十。

**7日净增仍未形成完整可比历史**，不填零、不推算。Hallmark昨日因基线时间不同单列，今天纳入共同窗口，“进入第二”不代表从零热度突然飙升。[昨日观察记录](https://github.com/BruceShenC/News/blob/main/daily/2026-10-29/ai-skills-ranking.md)

**变化与建议**
• 总榜无换位。Impeccable领先Archify的差距从1,748扩大到3,941；每日窗口略不同，不据此计算精确增长加速度。
• Impeccable于10月29日main补齐.mjs/.cjs扫描，修复monorepo中surface-brief路径定位等；属于维护改进，不等同技术突破或Stars上涨原因。正式发布仍为10月26日skill-v4.8.0。[扫描提交](https://github.com/pbakaus/impeccable/commit/9f11a709e385b54580c14a8c50cd2f6b147701cf)、[发布](https://github.com/pbakaus/impeccable/releases/tag/skill-v4.8.0)
• Hallmark于10月29日合入符号链接目录去重、CRLF主题头兼容修复，重点是本地脚本跨平台可用性。[去重提交](https://github.com/Nutlope/hallmark/commit/9d2d115d478b70d683b2059318b7252617935b4f)、[CRLF提交](https://github.com/Nutlope/hallmark/commit/4f6a46c6dc325d40ab268c93e25c7b96676ebccd)

建议隔离小范围验证：**Impeccable**用非敏感页面对比纯Skill审查与hook检测的发现质量，记录误报、网络访问和执行权限；**Hallmark**先用audit模式评估规则价值，不默认改写现有产品设计体系。

热度不等于成熟度、安全性或实际效果。本期未安装或执行项目。

[本期归档](https://github.com/BruceShenC/News/blob/main/daily/2026-10-30/ai-skills-ranking.md)
