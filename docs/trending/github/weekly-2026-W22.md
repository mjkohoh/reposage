# GitHub Trending 简报（2026 年第 22 周）

## 摘要

- 查询时间：2026-05-29 09:43 CST（Asia/Shanghai）
- 周期：本周（ISO 周 2026-W22）
- 自然语言筛选：无，GitHub Trending 全量榜单
- 数据源：https://github.com/trending?since=weekly
- 本次结论：本周榜单继续重排，`Lum1104/Understand-Anything` 明确站上第 1，`colbymchenry/codegraph` 退到第 2；同时周榜后半区集中出现 `MoneyPrinterTurbo`、`presenton`、`stop-slop`、`Anthropic-Cybersecurity-Skills`、`cursor/plugins`，说明热度已经从“代码理解基础设施”进一步扩散到内容生成、演示文稿生成、输出风格治理和专业安全技能目录。

## 筛选条件

- 时间范围：This week / weekly
- 自然语言：Any
- 说明：未添加 `spoken_language_code`，也未按开发语言筛选；排名、stars、forks 与对应周期新增 stars 来自 GitHub Trending 页面在查询时展示的数据。适用场景与关注理由基于趋势页和仓库主页可见信息推断，尚未本地验证。

## 趋势项目

### 1. Lum1104/Understand-Anything

- 链接：https://github.com/Lum1104/Understand-Anything
- 主要语言/技术栈：TypeScript / knowledge graph / code understanding
- GitHub 简介：Graphs that teach > graphs that impress. Turn any code into an interactive knowledge graph you can explore, search, and ask questions about.
- 趋势信号：42,872 stars / 3,419 forks；本周新增 26,212 stars。
- 适用场景：快速理解大型代码库、知识库或复杂文档结构。
- 初步关注理由：今天进一步升到周榜第 1，说明“可探索知识图谱”已经压过传统 agent 壳层，成为本周最强入口形态。
- 不确定性：图谱构建质量、跨语言覆盖和大仓库性能尚未本地验证。

### 2. colbymchenry/codegraph

- 链接：https://github.com/colbymchenry/codegraph
- 主要语言/技术栈：TypeScript / MCP / local code knowledge graph
- GitHub 简介：Pre-indexed code knowledge graph for Claude Code, Codex, Gemini, Cursor, OpenCode, AntiGravity, Kiro, and Hermes Agent — fewer tokens, fewer tool calls, 100% local
- 趋势信号：31,806 stars / 1,878 forks；本周新增 19,128 stars。
- 适用场景：为 Claude Code、Codex、Cursor 等提供本地代码索引、关系查询和上下文压缩。
- 初步关注理由：虽然退到第 2，但仍与第 1 共同构成“代码理解图谱双雄”，周榜主线没有变。
- 不确定性：索引成本、跨语言表现与大型仓库上的稳定性尚未验证。

### 3. rohitg00/ai-engineering-from-scratch

- 链接：https://github.com/rohitg00/ai-engineering-from-scratch
- 主要语言/技术栈：Python / AI engineering learning / 教程型仓库
- GitHub 简介：Learn it. Build it. Ship it for others.
- 趋势信号：23,896 stars / 3,879 forks；本周新增 13,159 stars。
- 适用场景：用项目化路径学习 AI 应用构建、交付与工程化实践。
- 初步关注理由：排名守住第 3，说明“工程教育内容”仍是本周稳定传播层。
- 不确定性：课程深度、代码质量与维护连续性尚未验证。

### 4. anthropics/knowledge-work-plugins

- 链接：https://github.com/anthropics/knowledge-work-plugins
- 主要语言/技术栈：Python / Claude Cowork / 插件目录
- GitHub 简介：Open source repository of plugins primarily intended for knowledge workers to use in Claude Cowork
- 趋势信号：17,762 stars / 2,083 forks；本周新增 5,282 stars。
- 适用场景：为知识工作型 agent 组合资料检索、写作、资料整理和工作流插件。
- 初步关注理由：从第 5 升到第 4，说明插件分发层正在从“新入榜”变成稳定主线。
- 不确定性：插件质量、权限边界与对非 Claude 生态的可迁移性尚未验证。

### 5. harry0703/MoneyPrinterTurbo

- 链接：https://github.com/harry0703/MoneyPrinterTurbo
- 主要语言/技术栈：Python / 短视频生成 / Web UI / API
- GitHub 简介：利用 AI 大模型，一键生成高清短视频；只需提供主题或关键词，即可自动生成文案、素材、字幕、背景音乐并合成视频。
- 趋势信号：66,514 stars / 9,620 forks；本周新增 7,948 stars。
- 适用场景：快速生成营销、知识讲解或自媒体短视频原型。
- 初步关注理由：新进周榜前 5，说明本周热点不只在 agent infra，也开始更明确地奖励直接产出内容的应用层产品。
- 不确定性：成片质量、模型成本、版权处理与批量生产稳定性尚未验证。

### 6. presenton/presenton

- 链接：https://github.com/presenton/presenton
- 主要语言/技术栈：TypeScript / AI presentation / API
- GitHub 简介：Open-Source AI Presentation Generator and API (Gamma, Beautiful AI, Decktopus Alternative)
- 趋势信号：7,307 stars / 1,184 forks；本周新增 1,740 stars。
- 适用场景：自动生成演示文稿、汇报稿或初版 slide deck。
- 初步关注理由：它排到第 6，说明“让模型直接生成可交付物”仍是高频注意力来源。
- 不确定性：生成质量、版式一致性和真实商务汇报可用性尚未验证。

### 7. hardikpandya/stop-slop

- 链接：https://github.com/hardikpandya/stop-slop
- 主要语言/技术栈：文本规则文件 / prose editing / AI writing guardrail
- GitHub 简介：A skill file for removing AI tells from prose
- 趋势信号：6,445 stars / 470 forks；本周新增 2,486 stars。
- 适用场景：约束 AI 写作，减少模板化、宣传腔和明显的“AI 味”。
- 初步关注理由：它不仅进周榜，而且进入前 8，说明“输出质量治理”已经成为周级别主线。
- 不确定性：对中文写作、不同文体和复杂编辑任务的效果尚未验证。

### 8. ruvnet/RuView

- 链接：https://github.com/ruvnet/RuView
- 主要语言/技术栈：Rust / WiFi CSI / spatial intelligence
- GitHub 简介：π RuView turns commodity WiFi signals into real-time spatial intelligence, vital sign monitoring, and presence detection — all without a single pixel of video.
- 趋势信号：67,465 stars / 8,940 forks；本周新增 4,690 stars。
- 适用场景：做无摄像头空间感知、存在检测和生命体征监测实验。
- 初步关注理由：虽然名次跌到第 8，但仍能留榜，说明物理世界感知在本周仍有稳定吸引力。
- 不确定性：能力表现依赖硬件与实验环境，尚未独立验证。

### 9. mukul975/Anthropic-Cybersecurity-Skills

- 链接：https://github.com/mukul975/Anthropic-Cybersecurity-Skills
- 主要语言/技术栈：Python / cybersecurity skills / framework mapping
- GitHub 简介：754 structured cybersecurity skills for AI agents，映射 MITRE ATT&CK、NIST CSF、ATLAS、D3FEND 与 NIST AI RMF 等框架，并适配 Claude Code、Copilot、Codex CLI、Cursor、Gemini CLI 等平台。
- 趋势信号：11,578 stars / 1,300 forks；本周新增 4,904 stars。
- 适用场景：为安全分析、攻防研究或合规映射准备结构化技能清单。
- 初步关注理由：新进周榜第 9，说明 skill 资产正在向专业、高约束场景继续下沉。
- 不确定性：技能准确性、更新频率与对真实安全任务的帮助程度尚未验证。

### 10. cursor/plugins

- 链接：https://github.com/cursor/plugins
- 主要语言/技术栈：TypeScript / Cursor ecosystem / plugin specification
- GitHub 简介：Cursor plugin specification and official plugins
- 趋势信号：1,108 stars / 103 forks；本周新增 629 stars。
- 适用场景：为 Cursor 生态接入扩展能力、外部服务和工作流插件。
- 初步关注理由：榜单尾部开始出现编辑器生态插件目录，说明“工具分发层”正在从独立仓库进一步下沉到具体平台。
- 不确定性：插件质量、生态成熟度与平台依赖性尚未验证。

## 观察

- `Understand-Anything` 与 `codegraph` 仍然包揽前 2，说明“图谱化代码理解”仍是本周最稳主线。
- 周榜新增的 `MoneyPrinterTurbo` 与 `presenton` 都更接近终端成果物，表明热点正在从 agent 内核扩散到内容与可交付物生成层。
- `stop-slop` 与 `Anthropic-Cybersecurity-Skills` 同时进入前 10，说明 skill 资产开始明显分化成“写作治理”和“专业领域目录”两条线。
- `cursor/plugins` 上榜意味着插件分发已经从通用知识工作流扩散到具体编辑器生态。

## 更新记录

- 2026-05-26 10:07 CST：创建 2026 年第 22 周趋势简报初版，记录 GitHub Trending weekly Top 10。
- 2026-05-28 10:03 CST：更新同周期简报。新增 Top 10 项目：`anthropics/knowledge-work-plugins`、`rmyndharis/OpenWA`、`HKUDS/ViMax`；掉出 Top 10 项目：`rohitg00/agentmemory`、`CloakHQ/CloakBrowser`、`supertone-inc/supertonic`。主要排名变化：`Lum1104/Understand-Anything` 第 3 -> 第 2，`rohitg00/ai-engineering-from-scratch` 第 5 -> 第 3，`ruvnet/RuView` 第 6 -> 第 4，`Imbad0202/academic-research-skills` 第 4 -> 第 6，`tinyhumansai/openhuman` 第 2 -> 第 9；`colbymchenry/codegraph` 和 `can1357/oh-my-pi` 排名保持不变。
- 2026-05-29 09:43 CST：更新同周期简报。新增 Top 10 项目：`harry0703/MoneyPrinterTurbo`、`presenton/presenton`、`hardikpandya/stop-slop`、`mukul975/Anthropic-Cybersecurity-Skills`、`cursor/plugins`；掉出 Top 10 项目：`Imbad0202/academic-research-skills`、`rmyndharis/OpenWA`、`HKUDS/ViMax`、`tinyhumansai/openhuman`、`can1357/oh-my-pi`。主要排名变化：`Lum1104/Understand-Anything` 第 2 -> 第 1，`colbymchenry/codegraph` 第 1 -> 第 2，`anthropics/knowledge-work-plugins` 第 5 -> 第 4，`ruvnet/RuView` 第 4 -> 第 8，`presenton/presenton` 进入第 6，`rohitg00/ai-engineering-from-scratch` 保持第 3。
