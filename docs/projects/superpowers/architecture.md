# Superpowers 架构分析

## 总体结构

Superpowers 的仓库结构可以理解为四层：

1. 技能层：`skills/`，项目核心。
2. 平台适配层：`.claude-plugin/`、`.codex-plugin/`、`.cursor-plugin/`、`.opencode/`、`gemini-extension.json`。
3. 命令与 hooks 层：`commands/`、`hooks/`、`agents/`。
4. 测试与文档层：`tests/`、`docs/`、`RELEASE-NOTES.md`。

`package.json` 很薄，只声明项目名、版本、模块类型和 OpenCode 入口：

```json
{
  "name": "superpowers",
  "version": "5.0.7",
  "type": "module",
  "main": ".opencode/plugins/superpowers.js"
}
```

这说明 Superpowers 的核心不是 npm 包 API，而是文件系统中的技能、插件元数据和宿主 Agent 的加载机制。

## 核心目录

### `skills/`

核心技能目录。每个技能通常是一个目录，里面有 `SKILL.md`，部分技能还带有提示词、脚本或参考资料。

本次检查到的技能包括：

- `using-superpowers`
- `brainstorming`
- `writing-plans`
- `using-git-worktrees`
- `subagent-driven-development`
- `executing-plans`
- `test-driven-development`
- `systematic-debugging`
- `requesting-code-review`
- `receiving-code-review`
- `dispatching-parallel-agents`
- `finishing-a-development-branch`
- `verification-before-completion`
- `writing-skills`

每个 `SKILL.md` 顶部都有 frontmatter，例如：

```yaml
---
name: test-driven-development
description: Use when implementing any feature or bugfix, before writing implementation code
---
```

宿主 Agent 根据 `description` 判断何时加载技能。

### `skills/using-superpowers`

这是整个系统的入口技能。它要求 Agent 在任何任务开始前检查是否有相关 skill，并强调只要有很小概率适用，就应该加载技能。

它还定义了指令优先级：

1. 用户显式指令最高。
2. Superpowers skills 次之。
3. 默认系统提示最低。

这个设计很关键：Superpowers 试图强制流程纪律，但仍承认用户和项目本地指令拥有最高优先级。

### `skills/brainstorming`

设计阶段技能。它要求在任何创造性工作或行为修改前先进行需求澄清，并且有硬 gate：在用户批准设计前不能写代码、不能 scaffold、不能进入实现技能。

它还要求把设计文档写入：

```text
docs/superpowers/specs/YYYY-MM-DD-<topic>-design.md
```

该技能包含 `scripts/`，实现一个本地 visual companion server，用于设计阶段展示视觉材料。

### `skills/writing-plans`

计划生成技能。它把已批准的设计拆成小任务，要求每个任务包含：

- 精确文件路径。
- 测试代码。
- 实现代码。
- 运行命令。
- 预期输出。
- 提交命令。

默认计划路径：

```text
docs/superpowers/plans/YYYY-MM-DD-<feature-name>.md
```

这个技能的核心理念是计划必须足够具体，能让一个缺少上下文的工程师逐步执行。

### `skills/subagent-driven-development`

实现阶段的推荐模式。它要求每个任务派发一个新的子 Agent，并在实现后进行两轮审查：

- 规格符合性审查。
- 代码质量审查。

这个设计解决的是长上下文污染和自我确认偏差。实现者、规格审查者、质量审查者分别持有不同任务上下文，避免一个 Agent 在同一上下文里既写又评。

### `skills/test-driven-development`

实现纪律技能。它要求严格执行：

```text
RED -> GREEN -> REFACTOR
```

关键约束是“没有先失败的测试，就不能写生产代码”。如果先写了代码，技能要求删除并从测试重新开始。

### `skills/systematic-debugging`

调试纪律技能。它强调先定位根因，再修复症状。流程分四阶段：

1. 根因调查。
2. 模式分析。
3. 假设与验证。
4. 实施修复。

该技能适用于测试失败、生产 bug、构建失败、集成问题和性能问题等。

## 平台适配

### Claude Code

`.claude-plugin/plugin.json` 提供 Claude 插件元数据，README 提供 marketplace 安装方式。

### OpenAI Codex

`.codex-plugin/plugin.json` 声明：

- `skills`: `./skills/`
- 显示名、描述、作者、图标、分类和能力。

`.codex/INSTALL.md` 则说明 Codex 通过 `~/.agents/skills/` 原生技能发现机制加载 Superpowers。

### Cursor

`.cursor-plugin/plugin.json` 声明技能、agents、commands 和 Cursor hooks。Cursor 可以从插件市场安装。

### OpenCode

`.opencode/plugins/superpowers.js` 是一个实际 JS 插件入口。它会：

- 解析 `using-superpowers/SKILL.md`。
- 生成 bootstrap 文本。
- 把 `skills/` 加入 OpenCode 配置。
- 把 bootstrap 注入到会话首个用户消息。

### Gemini CLI

`gemini-extension.json` 声明 extension 名称、描述、版本和上下文文件 `GEMINI.md`。仓库还提供 Gemini 工具映射参考。

## 测试结构

`tests/` 目录覆盖多个方面：

- `tests/claude-code/`：Claude Code skill 相关测试。
- `tests/opencode/`：OpenCode 插件加载和优先级测试。
- `tests/brainstorm-server/`：brainstorming server 的 HTTP、WebSocket、生命周期测试。
- `tests/subagent-driven-dev/`：子 Agent 开发流程示例。
- `tests/skill-triggering/`：技能触发测试。
- `tests/codex-plugin-sync/`：Codex 插件同步测试。

这说明项目维护者把“技能是否会被正确触发”和“平台适配是否可用”当作主要测试目标，而不是传统库 API 的单元测试。

## 设计特点

- 文档即执行协议：`SKILL.md` 既是文档，也是 Agent 行为规约。
- 平台薄适配：尽量复用同一套 `skills/`，针对不同宿主只补工具映射、插件清单或注入方式。
- 强流程 gate：brainstorming、TDD、debugging 等技能都包含硬性禁止项。
- 子 Agent 隔离：实现、规格审查、代码质量审查被拆成不同角色。
- 用户优先：虽然技能很强势，但 `using-superpowers` 明确用户指令优先。

## 潜在架构风险

- 过度依赖宿主 Agent 是否真的遵守技能文本。技能是规约，不是强制执行的编译器或运行时。
- 多平台适配容易漂移，不同平台工具名、hook 格式、上下文注入机制会持续变化。
- 强流程可能增加小任务成本，尤其是一次性脚本或低风险改动。
- 子 Agent 工作流依赖宿主是否支持多 Agent，不支持时只能退化为 `executing-plans`。

