# GitHub Trending 简报（2026 年 05 月）

## 摘要

- 查询时间：2026-05-29 09:43 CST（Asia/Shanghai）
- 周期：本月 / monthly
- 自然语言筛选：无，GitHub Trending 全量榜单
- 数据源：https://github.com/trending?since=monthly
- 本次结论：5 月月榜尾声进一步确认了一个结构性结论：`colbymchenry/codegraph` 稳住第 1，`anthropics/financial-services` 与 `CloakBrowser` 维持高位，而后半区从 `warp`、`jcode`、`ViMax` 进一步切换为 `ai-engineering-from-scratch`、`9router`、`maigret`。这说明月度主线从“agent 环境与生成应用”继续回收到“代码理解、垂直场景、研究取证和工程学习”。

## 筛选条件

- 时间范围：This month / monthly
- 自然语言：Any
- 说明：未添加 `spoken_language_code`，也未按开发语言筛选；排名、stars、forks 与对应周期新增 stars 来自 GitHub Trending 页面在查询时展示的数据。适用场景与关注理由基于趋势页和仓库主页可见信息推断，尚未本地验证。

## 趋势项目

### 1. colbymchenry/codegraph

- 链接：https://github.com/colbymchenry/codegraph
- 主要语言/技术栈：TypeScript / MCP / local code knowledge graph
- GitHub 简介：Pre-indexed code knowledge graph for Claude Code, Codex, Gemini, Cursor, OpenCode, AntiGravity, Kiro, and Hermes Agent — fewer tokens, fewer tool calls, 100% local
- 趋势信号：31,807 stars / 1,878 forks；本月新增 30,718 stars。
- 适用场景：为 Claude Code、Codex、Cursor 等提供本地代码索引、关系查询和上下文压缩。
- 初步关注理由：从第 2 升到第 1，说明 5 月末最强月度主题已经从“skills 清单”切换到“代码理解基础设施”。
- 不确定性：索引成本、跨语言表现与大型仓库上的稳定性尚未验证。

### 2. anthropics/financial-services

- 链接：https://github.com/anthropics/financial-services
- 主要语言/技术栈：Python / vertical AI workflow（基于仓库名与主页推断）
- GitHub 简介：Trending 页面未给出明确简介；以下定位主要基于仓库主页标题与仓库名推断。
- 趋势信号：28,522 stars / 3,996 forks；本月新增 20,790 stars。
- 适用场景：研究金融服务场景中的 AI workflow 或参考实现。
- 初步关注理由：从第 3 升到第 2，说明垂直金融场景在本月并不是边缘话题，而是持续高热主线。
- 不确定性：主页公开描述依然有限，定位、合规边界和生产可用性尚未验证。

### 3. CloakHQ/CloakBrowser

- 链接：https://github.com/CloakHQ/CloakBrowser
- 主要语言/技术栈：Python / Chromium / Playwright / fingerprint patches
- GitHub 简介：Stealth Chromium that passes every bot detection test. Drop-in Playwright replacement with source-level fingerprint patches. 30/30 tests passed.
- 趋势信号：22,106 stars / 1,763 forks；本月新增 20,646 stars。
- 适用场景：在授权测试或研究环境下处理浏览器自动化与反 bot 兼容问题。
- 初步关注理由：从第 4 升到第 3，说明浏览器自动化摩擦仍然是 agent 体系里最稳定的高需求点之一。
- 不确定性：绕过检测的宣传口径尚未验证，且存在明显合规边界。

### 4. rohitg00/agentmemory

- 链接：https://github.com/rohitg00/agentmemory
- 主要语言/技术栈：TypeScript / persistent memory / AI coding agents
- GitHub 简介：#1 Persistent memory for AI coding agents based on real-world benchmarks
- 趋势信号：19,319 stars / 1,579 forks；本月新增 17,217 stars。
- 适用场景：让 coding agent 跨会话保存项目结构、决策和用户偏好。
- 初步关注理由：今天升到第 4，说明“长期记忆”仍然是 5 月末最稳定的 agent 基础能力之一。
- 不确定性：记忆污染、隐私隔离与生命周期治理尚未验证。

### 5. mattpocock/skills

- 链接：https://github.com/mattpocock/skills
- 主要语言/技术栈：Shell / Markdown / AI coding agent skills
- GitHub 简介：Skills for Real Engineers. Straight from my .claude directory.
- 趋势信号：110,236 stars / 9,673 forks；本月新增 75,448 stars。
- 适用场景：把需求澄清、测试、调试、重构和文档等工程习惯沉淀给 coding agent。
- 初步关注理由：虽然继续下滑到第 5，但仍稳居第一梯队，说明 workflow/skills 依旧是 5 月传播力最强的资产之一。
- 不确定性：迁移成本和对不同工程栈的真实收益仍需在具体仓库验证。

### 6. Lum1104/Understand-Anything

- 链接：https://github.com/Lum1104/Understand-Anything
- 主要语言/技术栈：TypeScript / knowledge graph / code understanding
- GitHub 简介：Graphs that teach > graphs that impress. Turn any code into an interactive knowledge graph you can explore, search, and ask questions about.
- 趋势信号：42,872 stars / 3,419 forks；本月新增 33,147 stars。
- 适用场景：快速理解大型代码库、知识库或复杂文档结构。
- 初步关注理由：升到第 6，且日榜、周榜也在高位，说明图谱化代码理解已经是跨日周月同时成立的主线。
- 不确定性：图谱构建质量、跨语言覆盖和大仓库性能尚未本地验证。

### 7. Imbad0202/academic-research-skills

- 链接：https://github.com/Imbad0202/academic-research-skills
- 主要语言/技术栈：Python / Claude Code skills / academic workflow
- GitHub 简介：Academic Research Skills for Claude Code: research → write → review → revise → finalize
- 趋势信号：23,382 stars / 1,951 forks；本月新增 19,583 stars。
- 适用场景：文献研究、学术写作、审稿与修订流程辅助。
- 初步关注理由：虽然从第 8 升到第 7 幅度不大，但说明专业知识工作流仍有稳定月度吸引力。
- 不确定性：学术诚信、引用质量与跨学科适配效果尚未验证。

### 8. rohitg00/ai-engineering-from-scratch

- 链接：https://github.com/rohitg00/ai-engineering-from-scratch
- 主要语言/技术栈：Python / AI engineering learning / 教程型仓库
- GitHub 简介：Learn it. Build it. Ship it for others.
- 趋势信号：23,896 stars / 3,879 forks；本月新增 18,067 stars。
- 适用场景：用项目化路径学习 AI 应用构建、交付与工程化实践。
- 初步关注理由：新进月榜第 8，说明 5 月后段不仅工具在涨，“工程学习路径”本身也成为高热内容资产。
- 不确定性：课程深度、代码质量与维护连续性尚未验证。

### 9. decolua/9router

- 链接：https://github.com/decolua/9router
- 主要语言/技术栈：JavaScript / AI access layer / provider routing
- GitHub 简介：Unlimited FREE AI coding. Connect Claude Code, Codex, Cursor, Cline, Copilot, Antigravity to FREE Claude/GPT/Gemini via 40+ providers.
- 趋势信号：14,902 stars / 2,231 forks；本月新增 11,689 stars。
- 适用场景：为 Claude Code、Codex、Cursor 等接入多供应商模型与自动 fallback。
- 初步关注理由：新进月榜第 9，说明“模型接入与降本路由层”仍是强需求，而不是被上层 agent 全部吞掉。
- 不确定性：治理能力、延迟开销和生产级可靠性尚未验证。

### 10. soxoj/maigret

- 链接：https://github.com/soxoj/maigret
- 主要语言/技术栈：Python / OSINT / username search
- GitHub 简介：Collect a dossier on a person by username from 3000+ sites
- 趋势信号：30,801 stars / 2,202 forks；本月新增 11,082 stars。
- 适用场景：做公开资料检索、用户名足迹收集与初步 OSINT 调查。
- 初步关注理由：它回到月榜第 10，说明 5 月后半段除了 agent 工具外，研究取证类工具也保有持续关注。
- 不确定性：结果准确性、站点覆盖和合规边界尚未验证。

## 观察

- 月榜前 3 没变，说明 5 月最稳的长期注意力仍集中在“代码理解基础设施 + 垂直金融场景 + 浏览器自动化摩擦”。
- 月榜后半区从 `warp`、`jcode`、`ViMax` 切换到 `ai-engineering-from-scratch`、`9router`、`maigret`，说明月末兴趣从“运行环境与生成应用”转向“学习、网关与研究取证”。
- `agentmemory` 升到第 4，而 `skills` 继续下滑到第 5，说明大家对“长期记忆”这类基础能力的关注在抬升。
- `Understand-Anything` 与 `codegraph` 同时高位，进一步确认“图谱/索引化代码理解”是 5 月跨周期最强主题之一。

## 更新记录

- 2026-05-15 13:30 CST：创建 2026 年 05 月 GitHub Trending 月度简报初版，覆盖 Top 10 项目。
- 2026-05-23 13:22 CST：更新同周期简报。新增 Top 10 项目：`CloakHQ/CloakBrowser`、`colbymchenry/codegraph`、`refactoringhq/tolaria`、`rohitg00/agentmemory`、`Imbad0202/academic-research-skills`；掉出 Top 10 项目：`lsdefine/GenericAgent`、`ComposioHQ/awesome-codex-skills`、`NousResearch/hermes-agent`、`addyosmani/agent-skills`、`Z4nzu/hackingtool`。主要排名变化：`mattpocock/skills` 第 2 -> 第 1，`Alishahryar1/free-claude-code` 第 3 -> 第 2，`anthropics/financial-services` 第 10 -> 第 5，`multica-ai/andrej-karpathy-skills` 第 1 -> 第 8，`AIDC-AI/Pixelle-Video` 第 4 -> 第 10。
- 2026-05-24 11:26 CST：更新同周期简报。新增 Top 10 项目：`ComposioHQ/awesome-codex-skills`、`soxoj/maigret`；掉出 Top 10 项目：`refactoringhq/tolaria`、`AIDC-AI/Pixelle-Video`。主要排名变化：`colbymchenry/codegraph` 第 4 -> 第 2，`anthropics/financial-services` 第 5 -> 第 3，`CloakHQ/CloakBrowser` 第 3 -> 第 4，`rohitg00/agentmemory` 第 7 -> 第 5，`Alishahryar1/free-claude-code` 第 2 -> 第 6，`Imbad0202/academic-research-skills` 第 9 -> 第 8，`multica-ai/andrej-karpathy-skills` 第 8 -> 第 9。
- 2026-05-26 10:07 CST：更新同周期简报。新增 Top 10 项目：`TauricResearch/TradingAgents`、`AIDC-AI/Pixelle-Video`；掉出 Top 10 项目：`Alishahryar1/free-claude-code`、`ComposioHQ/awesome-codex-skills`。主要排名变化：`Imbad0202/academic-research-skills` 第 8 -> 第 6，`soxoj/maigret` 第 10 -> 第 7；`mattpocock/skills`、`colbymchenry/codegraph`、`anthropics/financial-services`、`CloakHQ/CloakBrowser`、`rohitg00/agentmemory`、`multica-ai/andrej-karpathy-skills` 排名保持不变。
- 2026-05-28 10:03 CST：更新同周期简报。新增 Top 10 项目：`warpdotdev/warp`、`Lum1104/Understand-Anything`、`1jehuang/jcode`、`HKUDS/ViMax`；掉出 Top 10 项目：`soxoj/maigret`、`TauricResearch/TradingAgents`、`multica-ai/andrej-karpathy-skills`、`AIDC-AI/Pixelle-Video`。主要排名变化：`colbymchenry/codegraph` 第 2 -> 第 1，`anthropics/financial-services` 第 3 -> 第 2，`CloakHQ/CloakBrowser` 第 4 -> 第 3，`mattpocock/skills` 第 1 -> 第 4，`rohitg00/agentmemory` 第 5 -> 第 6，`Imbad0202/academic-research-skills` 第 6 -> 第 8。
- 2026-05-29 09:43 CST：更新同周期简报。新增 Top 10 项目：`rohitg00/ai-engineering-from-scratch`、`decolua/9router`、`soxoj/maigret`；掉出 Top 10 项目：`warpdotdev/warp`、`1jehuang/jcode`、`HKUDS/ViMax`。主要排名变化：`rohitg00/agentmemory` 第 6 -> 第 4，`mattpocock/skills` 第 4 -> 第 5，`Lum1104/Understand-Anything` 第 7 -> 第 6，`Imbad0202/academic-research-skills` 第 8 -> 第 7；`colbymchenry/codegraph`、`anthropics/financial-services`、`CloakHQ/CloakBrowser` 排名保持不变。
