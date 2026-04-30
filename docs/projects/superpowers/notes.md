# Superpowers 调研笔记

## 本次调研来源

- GitHub 仓库：https://github.com/obra/superpowers
- 仓库 README
- `package.json`
- `RELEASE-NOTES.md`
- `.codex/INSTALL.md`
- `.codex-plugin/plugin.json`
- `.claude-plugin/plugin.json`
- `.cursor-plugin/plugin.json`
- `.opencode/plugins/superpowers.js`
- `gemini-extension.json`
- `skills/*/SKILL.md`

## 已确认事实

- 项目是公开 GitHub 仓库，许可证为 MIT。
- 当前研究到的版本号是 `5.0.7`。
- README 显示最新 release 为 `v5.0.7`，发布日期为 2026-03-31。
- 仓库主要语言占比显示 Shell、JavaScript、HTML、Python、TypeScript、Batchfile 都有涉及。
- 仓库不是一个传统 SDK，核心是 `skills/` 目录里的技能文档和各平台插件入口。
- `skills/` 下存在 14 个技能目录。
- Codex 安装方式主要依赖 `~/.agents/skills/` 的技能发现机制。
- OpenCode 插件会在运行时把 Superpowers 技能目录加入 OpenCode 配置，并注入 bootstrap。

## 关键判断

Superpowers 的真正价值不在某个脚本，而在“把 Agent 行为约束为可复用工程流程”。它通过技能文件把作者的工程偏好固化下来，包括：

- 写代码前先设计。
- 计划必须可执行。
- 功能和 bugfix 必须测试先行。
- Debug 必须先找根因。
- 实现要经过审查。
- 复杂任务优先用子 Agent 隔离上下文。

这些规则本身并不新，但 Superpowers 把它们包装成跨平台 Agent 可加载的 skills，这是项目的核心差异。

## 与普通提示词的区别

普通提示词通常是一段静态 instructions，容易被长上下文稀释。Superpowers 把提示拆成多个按场景触发的技能，每个技能有明确触发条件和执行步骤。这样做有几个好处：

- 初始上下文更小，不必一次加载所有规则。
- 不同任务加载不同流程，例如调试时加载 `systematic-debugging`，实现时加载 `test-driven-development`。
- 技能可以单独测试、更新和跨平台映射。
- 能通过插件机制分发，而不是让用户手动复制大段提示词。

## 使用前需要注意

- 需要确认你使用的 Agent 宿主是否支持技能或插件发现。
- 团队需要接受较强流程约束，否则会频繁与本地习惯冲突。
- 项目本地指令仍然重要，例如 `AGENTS.md`、`CLAUDE.md`、`GEMINI.md` 可以覆盖 Superpowers 技能。
- 如果平台不支持子 Agent，推荐工作流会降级，效果可能不如 README 中描述的理想状态。

## 后续可深入研究方向

- 在 Codex App 中真实安装 Superpowers，观察技能触发是否符合预期。
- 用一个小型功能开发任务跑完整链路，记录实际耗时、上下文消耗和质量收益。
- 对比 Claude Code、Codex、Cursor、OpenCode 四个平台下的安装和触发差异。
- 研究 `hooks/session-start` 如何在不同平台注入 bootstrap。
- 阅读 `tests/skill-triggering/`，了解项目如何验证技能触发。
- 分析 `brainstorming` visual companion server 的协议和生命周期设计。

## 初步结论

Superpowers 适合把“AI 写代码”从即兴问答推进到可审查、可复现、可管理的工程流程。它最适合已有一定工程纪律、愿意用 Agent 承担较大开发任务的团队；对于很轻量的一次性问答，它可能显得流程较重。

