# mksglu/context-mode 项目概览

## 高信号总结

`mksglu/context-mode` 是一个面向 AI coding agent 的 MCP 工具和多平台 hook 套件，核心目标是减少上下文窗口污染，并在会话压缩/恢复后保留关键工作状态。它不是“给代码库做语义向量搜索”的项目，而是把大输出、网页、日志、命令结果、会话事件留在沙箱或 SQLite FTS5 中，只把摘要、搜索结果或继续工作所需的最小提示送进模型上下文。

项目把问题拆成四块：

1. **Context Saving**：通过 `ctx_execute`、`ctx_execute_file`、`ctx_batch_execute`、`ctx_fetch_and_index` 等 sandbox 工具避免大输出直接进入上下文。
2. **Session Continuity**：通过 hook 捕获文件编辑、git 操作、任务、错误、用户决策等事件，存入 per-project SQLite。
3. **Think in Code**：强制 agent 用代码处理数据，而不是把原始数据读进上下文后让模型“脑算”。
4. **Output Compression**：使用极简输出风格减少 assistant 自身输出 token。

来源：

- 仓库：<https://github.com/mksglu/context-mode>
- 默认分支：`main`
- 本次浅克隆提交：`8b04ae8 ci: update install stats`
- npm 包名：`context-mode`
- 当前仓库版本：`1.0.103`
- 许可证：Elastic License 2.0，见 `LICENSE`
- GitHub 页面显示：约 11.3k stars、368 forks（访问时间：2026-04-30）

## 项目定位

README 称它是 “The other half of the context problem”。结合源码看，它更像一个“上下文预算守门层”：

- MCP server 提供沙箱执行、索引、搜索、统计、诊断、升级、清理、分析仪表盘等工具。
- Hook 层拦截高风险工具调用，把 raw output 路由到 context-mode 工具。
- Session 层把重要会话事件写入 SQLite，在 compact/resume 后按需检索。
- 多平台 adapter 把不同 agent/IDE 的 hook 输入输出统一成一套内部模型。

## 目标用户

- 使用 Claude Code、Gemini CLI、Codex CLI、Cursor、VS Code Copilot 等 agent 的开发者。
- 经常处理大日志、大测试输出、网页文档、MCP tool list、GitHub issue/PR 列表的用户。
- 希望 agent 在自动 compact 后还能记得任务、文件、决策和错误的团队。
- 接受通过 hook 改写/阻止部分工具调用，以换取更稳定上下文预算的用户。

## 核心能力

### Sandbox 执行

`ctx_execute` 在子进程中执行 JavaScript、TypeScript、Python、Shell、Ruby、Go、Rust、PHP、Perl、R、Elixir。只把 stdout 摘要或受控输出返回上下文。大输出会被索引到 FTS5，再用 `ctx_search` 查。

来源：`src/server.ts`、`src/executor.ts`、`src/runtime.ts`

### 文件处理

`ctx_execute_file` 把文件内容放入沙箱变量中供代码处理，避免直接 `Read` 大文件进入上下文。适合日志、CSV、JSON、构建输出、测试输出等。

来源：`src/server.ts`

### FTS5 知识库

`ContentStore` 使用 SQLite FTS5，包含 porter tokenizer 和 trigram tokenizer，支持 BM25 排序、fallback、fuzzy 修正、按 heading 分块、保留代码块。

来源：`src/store.ts`

### 批量执行与搜索

`ctx_batch_execute` 顺序执行多条命令，将每条输出按 heading 合并索引，然后在同一次 MCP 调用里执行多条查询。README 和 tool description 都强调它是主力工具，用来替代大量单独 shell/read/search 调用。

来源：`src/server.ts`

### URL 抓取与索引

`ctx_fetch_and_index` 拉取 URL，按 Content-Type 处理：HTML 通过 Turndown 转 Markdown，JSON pretty print 后索引，文本直接索引。它有 24 小时 TTL cache。

来源：`src/server.ts`

### 会话连续性

PostToolUse hook 抽取事件写入 SessionDB；PreCompact 构建 resume snapshot；SessionStart 在 startup/compact/resume 时注入 routing block 和会话指令，并把事件文件写给 MCP server 自动索引。

来源：`hooks/posttooluse.mjs`、`hooks/precompact.mjs`、`hooks/sessionstart.mjs`、`src/session/db.ts`、`hooks/session-directive.mjs`

### 多平台支持

项目支持 Claude Code、Gemini CLI、VS Code Copilot、JetBrains Copilot、Cursor、OpenCode、Codex CLI、Antigravity、Kiro、Qwen Code、OpenClaw、Pi、Zed 等多种环境。MCP server 层通用；hook 层通过 adapter 适配不同平台。

来源：`docs/platform-support.md`、`src/adapters/`

## 适用场景

- 大量命令输出、日志、测试结果需要分析，但不想污染上下文。
- 需要把网页/API 文档/工具清单保存后按需搜索。
- agent 经常因为 compact 忘记正在修改的文件、用户决策和未完成任务。
- 多平台 agent 团队希望统一一套上下文治理规则。
- 希望用统计面板观察 agent 会话质量、委派、上下文健康等指标。

## 不适用边界

- 不适合不能接受 hook 拦截/改写工具调用的环境。
- 不适合严格要求最小权限或无需执行任意代码的场景；它的核心能力之一就是沙箱执行多语言代码，需要认真评估权限模型。
- 不适合把 ELv2 当作宽松开源许可使用的商业场景；ELv2 禁止将该软件作为提供其主要功能的托管/管理服务。
- Codex CLI 等平台的 hook 能力有限，例如文档说明 PreToolUse 不支持参数改写和 context 注入。
- 它优化的是上下文管理，不替代代码语义索引、LSP、编译器、测试系统或安全沙箱。

## 技术栈

- 语言：TypeScript / JavaScript ESM
- 运行：Node.js 18+，项目开发/CI 使用 Node 20；Bun 可作为 JS/TS 快速执行 runtime
- MCP：`@modelcontextprotocol/sdk`
- 数据库：SQLite + FTS5，`better-sqlite3` 为 optional dependency
- 打包：TypeScript + esbuild，产物包含 `server.bundle.mjs`、`cli.bundle.mjs`
- 测试：Vitest
- 前端分析仪表盘：`insight/`，Vite + React/TanStack Router 风格结构

## 初步评价

事实层面：这是一个工程化程度较高的 MCP/Hook 工具，覆盖多平台、CI、benchmark、诊断命令和大量测试。

个人判断：它对“agent 上下文被工具输出撑爆”和“compact 后断片”这两个痛点很直接。代价是系统复杂度明显上升：hook、MCP、SQLite、平台 adapter、沙箱执行、安全规则都要协同工作，排障成本不会低。
