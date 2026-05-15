# GitHub Trending 简报（2026 年 05 月）

## 摘要

- 查询时间：2026-05-15 13:30（Asia/Shanghai）
- 周期：monthly / This month
- 数据源：[GitHub Trending this month](https://github.com/trending?since=monthly)，并轻量查看了入选项目主页。
- 本次结论：本月榜单被 AI coding agent 相关项目强势占据，尤其是 `skills`、`CLAUDE.md`、agent 平台、agent 记忆与自动化执行框架。值得注意的是，前 10 中也混入安全工具集合与金融场景样例仓库，说明热点不只是“模型调用”，还在向可复用工作流、行业场景和工具生态扩散。

## 趋势项目

### 1. multica-ai/andrej-karpathy-skills

- 链接：https://github.com/multica-ai/andrej-karpathy-skills
- 主要语言/技术栈：未在 Trending 行显示主要语言；仓库核心是 `CLAUDE.md`、`CURSOR.md`、skills 目录与 Claude Code/Cursor 使用说明。
- GitHub 简介：一个用于改善 Claude Code 行为的 `CLAUDE.md` 文件，基于 Andrej Karpathy 对 LLM 编程问题的观察整理。
- 趋势信号：129,777 stars / 13,159 forks；本月新增 99,894 stars。
- 适用场景：给 Claude Code、Cursor 或类似 AI coding agent 增加“先思考、少改动、避免过度设计、以验证目标驱动”的行为约束。
- 初步关注理由：它反映了本月最强的主题：用户开始把 agent 质量问题沉淀为可复制的规则文件，而不是只依赖单次 prompt。
- 不确定性：尚未验证这些规则在不同代码库中的实际收益；README 中的效果描述需要通过真实任务对比确认。

### 2. mattpocock/skills

- 链接：https://github.com/mattpocock/skills
- 主要语言/技术栈：Shell；面向 Claude/Codex 等 coding agent 的技能与工程工作流集合。
- GitHub 简介：来自 Matt Pocock `.claude` 目录的工程技能集合。
- 趋势信号：82,594 stars / 7,134 forks；本月新增 66,519 stars。
- 适用场景：复用成熟工程师的 agent 工作流，例如调试、重构、测试、类型系统和文档任务。
- 初步关注理由：与 `andrej-karpathy-skills` 相比，它更偏“任务技能库”，说明 agent 使用正在从单一规则走向可组合工作流。
- 不确定性：项目内容更新快，具体 skill 的质量和适用边界需要逐项阅读验证。

### 3. Alishahryar1/free-claude-code

- 链接：https://github.com/Alishahryar1/free-claude-code
- 主要语言/技术栈：Python。
- GitHub 简介：声称可在终端、VSCode 扩展或 Discord 中免费使用 claude-code，并类似 OpenClaw，支持语音。
- 趋势信号：24,617 stars / 3,659 forks；本月新增 22,809 stars。
- 适用场景：对 Claude Code 类体验有兴趣、希望尝试替代接入方式的个人用户。
- 初步关注理由：需求信号非常明确：开发者对低门槛、低成本的 coding agent 接入方式有强烈兴趣。
- 不确定性：涉及第三方服务接入与“免费”使用声明，合规性、稳定性、安全性均尚未验证；不建议在生产或敏感代码环境中直接采用。

### 4. AIDC-AI/Pixelle-Video

- 链接：https://github.com/AIDC-AI/Pixelle-Video
- 主要语言/技术栈：Python。
- GitHub 简介：AI 全自动短视频引擎。
- 趋势信号：16,735 stars / 2,423 forks；本月新增 12,733 stars。
- 适用场景：短视频内容生成、自动化剪辑、批量素材处理、AI 内容生产流水线。
- 初步关注理由：从 coding agent 之外看，AI 视频生成仍是开源热点；“全自动短视频引擎”切中了内容生产工作流。
- 不确定性：尚未验证生成质量、模型依赖、版权/素材处理方式与部署成本。

### 5. lsdefine/GenericAgent

- 链接：https://github.com/lsdefine/GenericAgent
- 主要语言/技术栈：Python；浏览器、终端、文件系统、键鼠、屏幕视觉、ADB 等本机控制能力。
- GitHub 简介：一个自进化 agent 框架，强调从约 3K 行核心代码和 9 个原子工具出发，自动沉淀技能树。
- 趋势信号：11,408 stars / 1,300 forks；本月新增 10,252 stars。
- 适用场景：个人自动化 agent、本机系统控制、跨应用任务自动化、长期积累个人工作流。
- 初步关注理由：它把“agent 记忆/技能沉淀”作为核心卖点，和本月 skills 热点形成呼应。
- 不确定性：README 中关于 token 效率、系统控制和自举能力的实验结论尚未本地复现；系统级控制也带来安全边界问题。

### 6. ComposioHQ/awesome-codex-skills

- 链接：https://github.com/ComposioHQ/awesome-codex-skills
- 主要语言/技术栈：Python；Codex CLI/API 工作流技能清单。
- GitHub 简介：实用 Codex skills 的精选列表，用于自动化 Codex CLI 和 API 工作流。
- 趋势信号：9,622 stars / 897 forks；本月新增 8,774 stars。
- 适用场景：寻找 Codex skills 模板、扩展自动化工作流、建立团队内部技能目录。
- 初步关注理由：它说明 Codex 技能生态正在形成“awesome list”式入口，有利于后续筛选高价值 skill 做深度研究。
- 不确定性：精选列表不等于质量保证；每个条目的维护状态、适配版本和权限边界仍需单独确认。

### 7. NousResearch/hermes-agent

- 链接：https://github.com/NousResearch/hermes-agent
- 主要语言/技术栈：Python。
- GitHub 简介：项目主页标语为 “The agent that grows with you”。
- 趋势信号：150,513 stars / 23,841 forks；本月新增 68,208 stars。
- 适用场景：关注可成长 agent、长期记忆、个人化工作流的开发者。
- 初步关注理由：总 stars 和本月新增 stars 都非常高，且与 GenericAgent 一样强调“随使用成长”的 agent 方向。
- 不确定性：这里只做主页级轻量观察，具体架构、真实可用性和项目活跃度尚未验证。

### 8. addyosmani/agent-skills

- 链接：https://github.com/addyosmani/agent-skills
- 主要语言/技术栈：Shell；面向 AI coding agent 的工程技能集合。
- GitHub 简介：面向生产级工程的 AI coding agent skills。
- 趋势信号：41,633 stars / 4,571 forks；本月新增 26,393 stars。
- 适用场景：把常见工程任务封装成 agent 可复用技能，尤其适合团队规范、代码质量、交付流程场景。
- 初步关注理由：与 Matt Pocock 的 skills 仓库形成强烈同类信号，说明“技能化的工程知识”正在成为 coding agent 落地的重要载体。
- 不确定性：尚未逐项验证 skill 的完整性、可迁移性和是否绑定特定工具链。

### 9. Z4nzu/hackingtool

- 链接：https://github.com/Z4nzu/hackingtool
- 主要语言/技术栈：Python；安全研究、渗透测试工具集合。
- GitHub 简介：面向安全研究人员和渗透测试人员的 all-in-one hacking tool。
- 趋势信号：74,507 stars / 8,408 forks；本月新增 16,576 stars。
- 适用场景：安全研究、工具索引、渗透测试实验环境。
- 初步关注理由：这是本月 Top 10 中少数非 agent 方向项目，安全工具集合仍然有很强传播性。
- 不确定性：包含攻击、安全测试相关能力，只应在授权环境中使用；工具链安全性、依赖来源和法律边界需要严格审查。

### 10. anthropics/financial-services

- 链接：https://github.com/anthropics/financial-services
- 主要语言/技术栈：Python。
- GitHub 简介：Trending 页面未显示简介；基于仓库名推断，项目面向金融服务场景。
- 趋势信号：22,821 stars / 3,126 forks；本月新增 15,279 stars。
- 适用场景：金融行业 AI/自动化样例、金融服务 demo 或参考实现。
- 初步关注理由：行业场景仓库进入本月榜单，说明 AI 工具化热点正从通用 agent 扩展到金融等垂直领域。
- 不确定性：尚未进一步核验 README 具体内容，以上场景判断主要来自仓库名和 Trending 元数据；金融相关项目不能直接作为生产合规依据。

## 观察

- 本月最强主线是 “agent skills”：`andrej-karpathy-skills`、`mattpocock/skills`、`awesome-codex-skills`、`agent-skills` 全都在榜，说明开发者正在把 AI coding agent 的经验沉淀成文件、规则和可复用技能。
- 第二条主线是 “agent 可成长/可长期使用”：`GenericAgent`、`hermes-agent`、`multica` 等项目都在强调记忆、技能树、任务管理或 agent 平台化。
- AI 内容生产仍有热度：`Pixelle-Video` 代表视频生成与短视频自动化方向。
- 安全与金融是两个值得谨慎跟进的垂直场景：前者存在合规和授权边界，后者存在准确性、审计和监管要求。
- 初步优先研究建议：如果目标是提升日常编码 agent 质量，优先看 `multica-ai/andrej-karpathy-skills`、`mattpocock/skills`、`addyosmani/agent-skills`；如果目标是研究长期 agent 架构，再看 `lsdefine/GenericAgent` 和 `NousResearch/hermes-agent`。

## 更新记录

- 2026-05-15 13:30：创建 2026 年 05 月 GitHub Trending 月度简报初版，覆盖 Top 10 项目。
