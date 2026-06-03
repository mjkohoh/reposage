# EveryInc/compound-engineering-plugin 项目概览

## 高信号总结

`EveryInc/compound-engineering-plugin` 是一套面向 AI 编码 Agent 的工程工作流插件。它的目标不是只让 Agent “更会写代码”，而是让每次工程工作都留下可复用资产：更清楚的需求、更可执行的计划、更系统的审查结果，以及可以被后续 Agent 检索的解决方案文档。

它和 [obra/superpowers](https://github.com/obra/superpowers) 确实很像：两者都强调先澄清、再规划、再执行、再验证，都使用 `SKILL.md` 把工程方法写成 Agent 可加载的行为协议，也都支持多个 Agent 宿主。

但两者不是简单的替代关系：

- **Superpowers** 更像一套小而强制的编码方法论。它用 14 个技能约束 Agent 的默认行为，尤其强调任何任务开始前检查技能、创造性工作前必须先设计、功能和 bugfix 必须执行严格 TDD。
- **Compound Engineering** 更像一套较宽的工程操作系统。它在开发主链之外加入产品策略、创意筛选、组织知识沉淀、会话历史检索、Slack 调研、专项审查、PR 反馈处理、演示录制和跨平台转换 CLI。它更强调团队知识如何随交付持续复利。

如果你想先建立一套稳定、克制、默认生效的 Agent 编码纪律，优先看 Superpowers。如果你已经在大量使用 Agent，希望把开发、审查、知识沉淀和团队上下文串成一个更大的工作台，Compound Engineering 更值得深入试用。

## 基本信息

- 项目地址：https://github.com/EveryInc/compound-engineering-plugin
- 仓库：`EveryInc/compound-engineering-plugin`
- 默认分支：`main`
- 许可证：MIT
- 调研快照：2026-06-02
- 本次读取提交：[`3e77a7bd8450fef7270f8b46c0f1865fd7125741`](https://github.com/EveryInc/compound-engineering-plugin/commit/3e77a7bd8450fef7270f8b46c0f1865fd7125741)
- 源码清单版本：`3.9.4`
- GitHub 页面快照：约 `19.1k` stars、`1.4k` forks

以上 stars 和 forks 为 GitHub 页面在调研时显示的四舍五入值，会随时间变化。

## 项目定位

仓库 README 用一句话概括项目：

> AI skills and agents that make each unit of engineering work easier than the last.

它背后的核心观点是：传统开发会累积技术债和局部知识，而 Compound Engineering 希望让每次工作都改善下一次工作的输入。README 把工作量分配概括为 `80% planning and review, 20% execution`。

典型主链如下：

```text
ce-strategy
  -> ce-ideate（可选）
  -> ce-brainstorm
  -> ce-plan
  -> ce-work
  -> ce-code-review
  -> ce-compound
  -> 下一次工作获得更好的上下文
```

此外，`ce-product-pulse` 从用户结果和运行表现侧补充反馈，再反哺策略和下一轮需求讨论。

## 核心能力

### 工程主链

- `ce-strategy`：维护根目录 `STRATEGY.md`，记录目标问题、方法、用户、指标和工作方向。
- `ce-ideate`：在需求细化前生成并批判性筛选候选想法。
- `ce-brainstorm`：通过对话形成需求文档，解决“做什么”。
- `ce-plan`：形成结构化实施计划，解决“怎么做”。
- `ce-work`：按计划或简短描述执行工作，支持内联、串行子 Agent 和并行子 Agent。
- `ce-debug`：复现问题、追踪因果链、形成假设并做测试优先修复。
- `ce-code-review`：通过多角色 Agent 做分层代码审查、置信度过滤、去重和修复路由。
- `ce-compound`：把已解决问题沉淀到 `docs/solutions/`，供后续检索。

### 扩展能力

- 组织上下文：`ce-sessions`、`ce-slack-research`。
- Git 工作流：`ce-worktree`、`ce-commit`、`ce-commit-push-pr`、`ce-resolve-pr-feedback`。
- 质量与交付：`ce-simplify-code`、`ce-polish-beta`、`ce-dogfood-beta`、`ce-demo-reel`。
- 专项能力：浏览器测试、Xcode 测试、前端设计、Agent-native 架构、Rails 风格、图片生成。
- 跨宿主转换 CLI：把 Claude Code 插件格式转换并安装到 Codex、OpenCode、Pi、Gemini 和 Kiro。

## 与 Superpowers 的区别

本次比较使用 Superpowers 当前 `main` 提交 [`6fd4507659784c351abbd2bc264c7162cfd386dc`](https://github.com/obra/superpowers/commit/6fd4507659784c351abbd2bc264c7162cfd386dc)，其源码清单版本为 `5.1.0`。

| 维度 | Compound Engineering | Superpowers |
|------|----------------------|-------------|
| 核心目标 | 让每次工程工作留下可复用资产，使团队知识持续复利 | 用组合式技能强制编码 Agent 遵守成熟开发流程 |
| 规模 | 本次源码扫描到 38 个技能目录、43 个 Agent 文件 | 14 个技能目录，当前版本移除了唯一的 named agent |
| 默认入口 | 常用方式是显式调用 `/ce-*`；技能描述也支持按场景触发 | `using-superpowers` 在会话启动时注入，要求任何响应和动作前先检查技能 |
| 流程强度 | 会按任务规模调整仪式感；`ce-work` 可直接处理明确的小任务 | 更刚性；创造性工作前必须先设计，行为修改默认进入完整流程 |
| TDD | `ce-debug` 明确要求测试优先；`ce-work` 根据计划中的执行说明和任务性质选择测试姿态 | `test-driven-development` 对功能、bugfix、重构、行为修改要求严格 RED-GREEN-REFACTOR |
| 子 Agent | 大量专项 Agent，覆盖调研、审查、设计、历史分析和交付；支持并行调度 | 以 fresh subagent per task 为主，每个任务依次做规格审查和代码质量审查 |
| 知识沉淀 | `ce-compound` 把经验写入 `docs/solutions/`，并做重复检测、关联文档检索和可发现性检查 | 主要沉淀设计文档和实施计划，重点是当前交付流程的可靠性 |
| 产品层能力 | 提供 `ce-strategy`、`ce-ideate`、`ce-product-pulse` | 聚焦软件开发方法论，不覆盖完整产品策略闭环 |
| 平台适配 | 原生插件加 Bun/TypeScript 转换 CLI；Codex 需要原生技能安装和 Agent 转换配合 | 使用薄适配、启动 hooks 和少量平台入口，核心技能目录更直接 |
| 学习成本 | 较高，需要挑选适合团队的子集 | 较低，主链更短，理念更统一 |

### 为什么粗看很像

两者的重叠部分非常真实：

- 都反对 Agent 收到模糊需求后立刻写代码。
- 都把 brainstorming、planning、worktree、debugging、review、verification 视为必要工程活动。
- 都依赖技能 frontmatter 中的 `name` 和 `description` 让宿主按需加载完整指令。
- 都把子 Agent 隔离上下文视为减少偏差、提高审查质量的重要方法。

### 最关键的分界

个人判断：**Superpowers 优化的是“一个编码 Agent 应该如何可靠地做一次开发任务”；Compound Engineering 优化的是“一个团队如何让多次 Agent 工程工作持续产生复利”。**

这也是 CE 为什么会出现 `STRATEGY.md`、`docs/solutions/`、session history、Slack research、persona reviewers 和产品 pulse，而 Superpowers 更集中在 TDD、计划粒度、子 Agent 执行和分支收尾。

## 适用场景

- 团队已经频繁使用 Claude Code、Codex、Cursor 或其他编码 Agent。
- 希望把需求讨论、实施计划、代码审查和经验文档串成稳定流程。
- 需要多角色代码审查，例如安全、性能、可靠性、数据迁移和项目规范。
- 希望让历史解决方案进入后续 Agent 的可检索上下文。
- 同时使用多个 Agent 宿主，需要跨平台转换和安装能力。

## 不适用或需要谨慎的场景

- 只希望获得一套轻量、统一的 Agent 编码纪律时，CE 的技能面可能过宽。
- 团队没有维护 `docs/solutions/`、需求文档和计划文档的意愿时，复利主张很难成立。
- Codex 的完整安装需要额外执行 Bun 转换步骤来安装 Agent，初始配置比 Superpowers 更复杂。
- 大量多 Agent 审查和调研会增加时间与 token 成本，应按任务风险选择性使用。
- 同时安装 CE 和 Superpowers 理论上命名冲突较少，但相似流程可能重复触发。运行级共存效果尚未验证，建议选定一套主流程后再按需补充另一套的个别能力。

## 初步评价

这是一个比 README 第一印象更完整的项目：核心不只是技能内容，还包括跨平台转换器、安装迁移、版本同步、兼容性测试和大量真实迭代记录。

个人判断：CE 最有价值的差异并不是技能数量，而是 `ce-compound` 所代表的闭环。它试图让“修过一次的问题”变成团队知识库的一部分，让后续规划、调试和审查不用重新支付同样的上下文成本。
