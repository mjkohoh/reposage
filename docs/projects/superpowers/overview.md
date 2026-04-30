# Superpowers 项目概览

## 基本信息

- 项目地址：https://github.com/obra/superpowers
- 仓库：`obra/superpowers`
- 作者：Jesse Vincent / Prime Radiant
- 许可证：MIT
- 当前研究版本：`v5.0.7`，仓库 `package.json` 与插件清单均标记为 `5.0.7`
- 项目定位：面向编码 Agent 的技能框架与软件开发方法论

## 这个项目是什么

Superpowers 不是一个传统意义上的业务库或 CLI 工具，而是一套给 AI 编码助手使用的工作流系统。它把“需求澄清、方案设计、写计划、TDD、调试、代码审查、分支收尾”等软件工程实践拆成一组可组合的 `skills`，并通过不同平台的插件机制让 Agent 在合适的任务场景自动加载这些技能。

官方 README 对它的描述是：一套完整的软件开发方法论，构建在可组合技能和初始化指令之上，用来确保编码 Agent 会使用这些技能。

## 解决的问题

Superpowers 主要解决的是编码 Agent 工作时常见的失控问题：

- 过早写代码：用户只给一个模糊想法，Agent 直接开始实现，导致返工。
- 缺少工程纪律：不写测试、跳过复现、靠猜修 bug、完成前不验证。
- 计划不可执行：计划停留在抽象层，没有文件路径、代码片段、命令和验收标准。
- 长上下文污染：一个会话里既规划又实现又审查，容易遗忘约束或引入偏差。
- 多平台不一致：Claude Code、Codex、Cursor、OpenCode、Gemini 等 Agent 环境的能力和工具名不同。

它的思路是把这些问题前置成强约束流程：先澄清和设计，再写精细计划，再按 TDD 和审查循环推进，最后完成分支验证与合并选择。

## 核心能力

- 技能库：`skills/` 下提供多种 `SKILL.md`，覆盖 brainstorming、TDD、系统化调试、写计划、执行计划、代码审查、分支收尾等。
- 平台适配：提供 `.claude-plugin/`、`.codex-plugin/`、`.cursor-plugin/`、`.opencode/`、`gemini-extension.json` 等入口。
- 自动触发：通过技能 frontmatter 的 `name` 和 `description` 让宿主 Agent 判断何时加载技能。
- 子 Agent 工作流：在支持多 Agent 的平台上，推荐用 fresh subagent per task 的方式实现任务，并经过规格符合性审查和代码质量审查。
- 可视化辅助：`brainstorming` 技能内置一个零依赖 Node.js brainstorming server，用于在设计阶段展示视觉材料。

## 适用场景

- 想让编码 Agent 更稳定地完成中大型功能开发。
- 想强制 Agent 在实现前进行需求澄清、设计和计划。
- 想把 TDD、系统化调试、代码审查等实践固化成 Agent 可执行流程。
- 团队已经在使用 Claude Code、OpenAI Codex、Cursor、OpenCode、Gemini CLI 等 Agent 工具。
- 希望建立一套跨平台可复用的 Agent 技能库。

## 不适用或需要谨慎的场景

- 只需要一次性问答、简单代码片段解释，Superpowers 的流程约束可能显得偏重。
- 不支持技能发现或插件机制的平台，需要手动适配。
- 对 TDD、分支隔离、用户审批 gate 不接受的团队，需要先调整技能或项目级指令。
- 如果任务非常紧急但缺少测试环境，Superpowers 会要求先复现、定位根因、补验证，这会改变团队原有节奏。

## 一句话总结

Superpowers 是一套把成熟软件工程流程“编译”成 Agent 技能的项目，目标不是替你提供某个业务功能，而是让编码 Agent 更像一个有流程、有测试意识、会审查自己工作的工程团队成员。

