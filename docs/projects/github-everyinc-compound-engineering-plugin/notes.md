# EveryInc/compound-engineering-plugin 调研笔记

## 调研范围

本次调研目标：

- 理解 `EveryInc/compound-engineering-plugin` 的定位、能力和架构。
- 回答它与 `obra/superpowers` 为什么相似，以及关键差异在哪里。
- 记录安装方式、验证状态和后续试用建议。

## 已阅读材料

### Compound Engineering

- GitHub 仓库：https://github.com/EveryInc/compound-engineering-plugin
- 根目录 `README.md`
- `package.json`
- `CHANGELOG.md`
- `LICENSE`
- `plugins/compound-engineering/README.md`
- `plugins/compound-engineering/.claude-plugin/plugin.json`
- `plugins/compound-engineering/.codex-plugin/plugin.json`
- `plugins/compound-engineering/.cursor-plugin/plugin.json`
- `.claude-plugin/marketplace.json`
- `src/index.ts`
- `src/commands/install.ts`
- `src/parsers/claude.ts`
- `src/converters/claude-to-codex.ts`
- `src/targets/index.ts`
- `src/targets/codex.ts`
- `src/release/metadata.ts`
- `.github/workflows/ci.yml`
- `plugins/compound-engineering/skills/ce-strategy/SKILL.md`
- `plugins/compound-engineering/skills/ce-ideate/SKILL.md`
- `plugins/compound-engineering/skills/ce-brainstorm/SKILL.md`
- `plugins/compound-engineering/skills/ce-plan/SKILL.md`
- `plugins/compound-engineering/skills/ce-work/SKILL.md`
- `plugins/compound-engineering/skills/ce-debug/SKILL.md`
- `plugins/compound-engineering/skills/ce-code-review/SKILL.md`
- `plugins/compound-engineering/skills/ce-compound/SKILL.md`
- `plugins/compound-engineering/skills/ce-setup/SKILL.md`

### Superpowers 对比基线

- GitHub 仓库：https://github.com/obra/superpowers
- 当前根目录 `README.md`
- `package.json`
- `RELEASE-NOTES.md`
- `.codex-plugin/plugin.json`
- `.claude-plugin/plugin.json`
- `hooks/session-start`
- `hooks/hooks.json`
- `skills/using-superpowers/SKILL.md`
- `skills/brainstorming/SKILL.md`
- `skills/writing-plans/SKILL.md`
- `skills/test-driven-development/SKILL.md`
- `skills/subagent-driven-development/SKILL.md`

## 调研快照

### Compound Engineering

- 日期：2026-06-02
- 默认分支：`main`
- 本次读取提交：`3e77a7bd8450fef7270f8b46c0f1865fd7125741`
- 最新提交时间：2026-06-01T17:24:36-07:00
- 最新提交摘要：`fix(ce-resolve-pr-feedback): drop clustering, default to merit-based fixing (#893)`
- 源码清单版本：`3.9.4`
- `CHANGELOG.md` 中最新 CLI 版本：`3.9.4`，发布日期为 2026-05-31
- GitHub 页面显示：约 `19.1k` stars、`1.4k` forks、829 commits
- 许可证：MIT

### Superpowers

- 日期：2026-06-02
- 默认分支：`main`
- 本次读取提交：`6fd4507659784c351abbd2bc264c7162cfd386dc`
- 最新提交时间：2026-05-29T12:30:57-07:00
- 最新提交摘要：`Require contributors to disclose authoring environment and target dev`
- 源码清单版本：`5.1.0`
- `RELEASE-NOTES.md` 中最新 release：`v5.1.0`，发布日期为 2026-04-30
- GitHub 页面显示：约 `215k` stars、`19.1k` forks、441 commits
- 许可证：MIT

GitHub stars、forks 和 commits 为公开页面在调研时显示的快照，会继续变化。

## 关键发现

### 1. CE 的差异不只是技能更多

事实：

- CE 包含 38 个 skill 目录和 43 个 Agent Markdown 文件。
- 仓库提供 Bun/TypeScript CLI，可把 Claude Code 插件格式转换到 Codex、OpenCode、Pi、Gemini 和 Kiro。
- `ce-compound` 会把经验写入 `docs/solutions/`，并执行分类、关联检索、重复检测和 YAML 校验。

判断：

CE 的核心差异是把知识沉淀变成工程主链的一部分，而不是在任务完成后依赖人手补文档。

### 2. Superpowers 更强调整体行为重塑

事实：

- Superpowers 的 `using-superpowers` 要求 Agent 在任何响应和动作前检查相关 skill。
- `brainstorming` 对任何创造性工作设置硬 gate：用户批准设计前不能写代码。
- `test-driven-development` 要求功能、bugfix、重构和行为修改都先写失败测试。
- 当前版本有 14 个 skills，`v5.1.0` 已移除唯一 named agent。

判断：

Superpowers 的优势是规则少、主张集中、默认行为一致。它更像编码 Agent 的基础方法论层。

### 3. CE 更务实，也更复杂

事实：

- `ce-brainstorm` 会根据任务轻量、标准、深度三种规模调整讨论强度。
- `ce-work` 可以直接处理 bare prompt，并根据任务复杂度选择直接执行、任务列表或先回到 brainstorm/plan。
- `ce-work` 对测试采用执行说明驱动的策略：test-first、characterization-first 或务实执行。
- `ce-debug` 对 bug 修复明确要求测试优先。

判断：

CE 对小任务的处理更灵活，但这也意味着团队需要理解何时用哪个 skill。Superpowers 的统一纪律更容易形成稳定习惯。

### 4. Codex 安装链路值得特别注意

事实：

- CE 的 `.codex-plugin/plugin.json` 声明 `skills: "./skills/"`。
- Codex 原生插件流程负责发现 skills。
- Bun 转换器默认只输出 Agent TOML，避免 skills 被重复注册。
- README 明确要求 Codex 用户同时执行 marketplace 注册、Bun Agent 安装和 TUI 插件安装。

判断：

这套混合方案是对 Codex 当前插件能力边界的适配，不是文档冗余。只执行其中一部分会得到不完整体验。

### 5. 文档中的组件计数存在漂移

事实：

- 根 README 写 `37 skills and 51 agents`。
- 插件 README 写 `38+ skills` 和 `50+ agents`。
- 本次源码扫描得到 38 个 skill 目录和 43 个 Agent Markdown 文件。
- `src/release/metadata.ts` 的统计逻辑也是统计 `plugins/compound-engineering/agents` 下 Markdown 文件和 `skills` 下带 `SKILL.md` 的目录。

判断：

README 数字应视为近似宣传信息，不应作为精确能力清单。使用时以源码和 `plugins/compound-engineering/README.md` 的具体列表为准。

## 与仓库内旧 Superpowers 档案的关系

本仓库已有 `docs/projects/superpowers/` 调研档案，但它记录的是 `v5.0.7`。本次为了可靠比较，重新浅克隆并读取了 Superpowers 当前 `main` 和 `v5.1.0` 资料。

本次没有修改旧档案，避免把“研究 CE”扩展成对另一个项目的完整重写。后续可以单独刷新 Superpowers 档案。

## 未验证内容

- 没有安装 Bun，因此没有运行 CE 的 `bun test` 和 `bun run release:validate`。
- 没有在真实 Agent 宿主中安装 CE。
- 没有执行完整链路：`ce-brainstorm -> ce-plan -> ce-work -> ce-code-review -> ce-compound`。
- 没有测量 CE 和 Superpowers 在同一项目中的 token 成本、耗时和产出差异。
- 没有验证 CE 与 Superpowers 共存时的触发优先级。
- GitHub 匿名 API 在调研时触发限流；易变元数据使用 GitHub 官方仓库页面和 GitHub 连接能力交叉确认。

## 建议的下一步试验

如果要做使用决策，建议在同一个小型示例仓库中跑两轮对照：

1. 选择一个包含少量业务逻辑、测试和一个小 bug 的项目。
2. 用 Superpowers 完成一个明确功能和一个 bugfix。
3. 用 CE 完成同类任务，并至少执行一次 `ce-compound`。
4. 记录总耗时、用户确认次数、token 消耗、生成文档数量、审查发现数量和复用体验。
5. 最后决定主流程：Superpowers 作为默认纪律层，或 CE 作为更完整的团队工程工作台。
