# Superpowers 使用方式

## 安装方式概览

Superpowers 针对不同 Agent 平台提供不同安装方式。以下内容来自仓库 README、`.codex/INSTALL.md`、各平台插件清单与本地源码检查。

## Claude Code

官方 marketplace 安装：

```text
/plugin install superpowers@claude-plugins-official
```

Superpowers marketplace 安装：

```text
/plugin marketplace add obra/superpowers-marketplace
/plugin install superpowers@superpowers-marketplace
```

## OpenAI Codex CLI / Codex App

Codex CLI 可以通过插件搜索安装：

```text
/plugins
```

然后搜索：

```text
superpowers
```

也可以按 `.codex/INSTALL.md` 手动安装：

```bash
git clone https://github.com/obra/superpowers.git ~/.codex/superpowers
mkdir -p ~/.agents/skills
ln -s ~/.codex/superpowers/skills ~/.agents/skills/superpowers
```

安装后重启 Codex，让 Codex 扫描 `~/.agents/skills/` 并发现技能。

如果使用需要子 Agent 的技能，例如 `dispatching-parallel-agents` 或 `subagent-driven-development`，Codex 文档建议在配置中启用：

```toml
[features]
multi_agent = true
```

Codex App 中可以在侧边栏 Plugins 页面找到 Coding 分类下的 `Superpowers` 并安装。

## Cursor

在 Cursor Agent chat 中安装：

```text
/add-plugin superpowers
```

或在插件市场里搜索 `superpowers`。

`.cursor-plugin/plugin.json` 声明了技能、agents、commands 和 hooks：

```json
{
  "skills": "./skills/",
  "agents": "./agents/",
  "commands": "./commands/",
  "hooks": "./hooks/hooks-cursor.json"
}
```

## OpenCode

README 建议让 OpenCode 抓取安装说明：

```text
Fetch and follow instructions from https://raw.githubusercontent.com/obra/superpowers/refs/heads/main/.opencode/INSTALL.md
```

源码中的 `.opencode/plugins/superpowers.js` 会做两件关键事情：

- 把 Superpowers 的 `skills/` 目录加入 OpenCode 配置中的 `skills.paths`。
- 在会话首个用户消息前注入 `using-superpowers` 的 bootstrap 内容和 OpenCode 工具映射。

这意味着 OpenCode 使用时不需要手动创建技能 symlink。

## GitHub Copilot CLI

README 给出的安装命令：

```bash
copilot plugin marketplace add obra/superpowers-marketplace
copilot plugin install superpowers@superpowers-marketplace
```

`RELEASE-NOTES.md` 显示 `v5.0.7` 主要增加了 Copilot CLI 的 sessionStart context injection 和工具映射文档。

## Gemini CLI

安装：

```bash
gemini extensions install https://github.com/obra/superpowers
```

更新：

```bash
gemini extensions update superpowers
```

`gemini-extension.json` 指定 `contextFileName` 为 `GEMINI.md`，用于在 Gemini 环境中导入 Superpowers 上下文和工具映射。

## 基本工作流

Superpowers 的推荐开发流程如下：

1. `brainstorming`：在写代码前澄清意图、探索方案、形成设计文档。
2. `using-git-worktrees`：设计批准后创建隔离工作区和分支。
3. `writing-plans`：把设计拆成 2-5 分钟粒度的可执行任务。
4. `subagent-driven-development` 或 `executing-plans`：按计划执行任务。
5. `test-driven-development`：实现阶段执行 RED-GREEN-REFACTOR。
6. `requesting-code-review`：任务间进行代码审查。
7. `finishing-a-development-branch`：完成测试验证、选择合并、PR、保留或清理分支。

## 实际使用方式

安装后，用户不需要每次手动调用所有技能。Superpowers 的核心机制是让 Agent 在任务开始时检查是否有相关 skill。比如：

```text
我想给这个项目加一个导出 PDF 的功能。
```

理想情况下，Agent 会先触发 `using-superpowers` 和 `brainstorming`，而不是马上写代码。等设计被用户确认后，再触发 `writing-plans`，随后进入实现。

也可以显式点名技能：

```text
请使用 systematic-debugging 帮我排查这个测试失败。
```

## 验证方式

本次研究没有在真实 Claude/Codex/Cursor/OpenCode/Gemini 宿主中安装运行 Superpowers，只完成了以下验证：

- 克隆仓库并检查目录结构。
- 阅读 README、`package.json`、平台插件清单、`.codex/INSTALL.md` 和核心技能文件。
- 确认 `skills/` 下存在 14 个技能目录。
- 确认插件配置覆盖 Claude、Codex、Cursor、OpenCode、Gemini、Copilot CLI 等平台。

后续如果要做运行级验证，建议在目标宿主中完成一次安装，并用一个小型示例项目验证 `brainstorming -> writing-plans -> subagent-driven-development` 的完整链路。

