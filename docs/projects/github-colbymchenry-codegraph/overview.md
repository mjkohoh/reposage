# colbymchenry/codegraph 项目概览

## 高信号总结

`colbymchenry/codegraph` 是一个本地优先的代码知识图谱工具，主要给 Claude Code、Cursor、Codex CLI、opencode 这类 AI coding agent 提供 MCP 工具。它会用 tree-sitter 解析项目源码，把文件、符号、调用、导入、继承、路由等关系写入项目本地的 `.codegraph/codegraph.db` SQLite 数据库；agent 后续可以通过 `codegraph_search`、`codegraph_context`、`codegraph_callers`、`codegraph_impact` 等工具查询结构化上下文，而不是每次都用 grep/read 从头扫仓库。

最值得关注的使用路径是：先全局安装并配置 agent，然后在每个目标项目里运行 `codegraph init -i` 建索引。建好以后，agent 检测到 `.codegraph/` 就可以优先用 CodeGraph 查询。它尤其适合“我要快速理解某个模块、追踪调用链、评估改动影响面”的场景；不适合替代编译器、类型检查、测试，也不适合要求 100% 精确静态分析的重构。

来源：

- 仓库：<https://github.com/colbymchenry/codegraph>
- npm 包：<https://www.npmjs.com/package/@colbymchenry/codegraph>
- 默认分支：`main`
- 本次浅克隆提交：`9b6a917d3235235c01d5046941a50a65551fa8db`，提交时间 `2026-05-18T08:29:18-05:00`
- 包版本：`@colbymchenry/codegraph@0.7.9`，见 `package.json`
- 许可：MIT，见 `LICENSE`
- GitHub 页面显示：约 1.1k stars、121 forks、11 watchers（访问时间：2026-05-19，数值会变化）

## 项目定位

README 的定位是 “Supercharge Claude Code, Cursor, Codex, and OpenCode with Semantic Code Intelligence”。更准确地说，它是一个“代码结构索引 + MCP 查询层”：

- CLI：负责安装 agent 配置、初始化项目、索引、同步、查询、输出上下文。
- MCP server：把索引结果暴露成 `codegraph_*` 工具。
- TypeScript library：允许其他 Node/TypeScript 工具直接调用 `CodeGraph.init/open/indexAll/searchNodes/buildContext`。
- 本地数据库：每个项目独立存储 `.codegraph/codegraph.db`，不依赖外部 API。

## 目标用户

- 使用 Claude Code、Cursor、Codex CLI、opencode 的开发者。
- 经常让 agent 阅读中大型代码库、希望减少盲扫文件和上下文浪费的团队。
- 希望在本机保留代码索引，不把源码发给第三方检索服务的用户。
- 需要快速回答“符号在哪、谁调用它、改它会影响什么”的维护者。

## 核心能力

### 本地代码知识图谱

`src/index.ts` 中的 `CodeGraph` 类负责初始化、打开、索引、同步和查询项目。索引产物存储在 `.codegraph/codegraph.db`，目录是否初始化由 `src/directory.ts` 检查 `.codegraph/` 和 `codegraph.db` 判断。

### 多语言符号抽取

`src/types.ts` 定义的语言集合包含 TypeScript、JavaScript、Python、Go、Rust、Java、C/C++、C#、PHP、Ruby、Swift、Kotlin、Dart、Svelte、Vue、Liquid、Pascal/Delphi、Scala 等。README 标称 19+ 语言。实际抽取逻辑分散在 `src/extraction/languages/`、`src/extraction/*-extractor.ts`。

### 结构化查询

MCP 工具定义在 `src/mcp/tools.ts`，核心工具包括：

- `codegraph_search`：按符号名搜索。
- `codegraph_context`：根据任务构建相关代码上下文。
- `codegraph_callers` / `codegraph_callees`：追踪调用关系。
- `codegraph_impact`：分析修改符号的影响半径。
- `codegraph_node`：查看单个符号详情。
- `codegraph_explore`：面向陌生模块的深度探索，输出更重。
- `codegraph_files`：查看已索引文件结构。
- `codegraph_status`：查看索引健康状态。

### Agent 配置安装器

`src/installer/targets/` 下分别实现 Claude、Cursor、Codex CLI、opencode 的配置写入。以 Codex 为例，`src/installer/targets/codex.ts` 会把 MCP 配置写入 `~/.codex/config.toml`，并把使用说明写入 `~/.codex/AGENTS.md`。

### 自动同步

`CodeGraph.watch()` 使用 `FileWatcher` 监听文件变化并触发 `sync()`。README 描述它使用原生 OS 文件事件，并对变更做 debounce。源码中 `SERVER_INSTRUCTIONS` 也提醒索引对文件写入有约 1 秒以内的延迟。

## 适用场景

- 开始改一个陌生模块前，让 agent 快速收集相关符号和文件。
- 做重构前检查某个函数、类、方法的调用方和影响面。
- 在大型项目中按符号名定位定义、签名、文档字符串和相关代码。
- 给 Claude Code / Cursor / Codex CLI / opencode 统一配置一个本地 MCP 代码结构服务。
- 在 CI 或 git hook 中用 `codegraph affected` 根据变更文件推断受影响测试文件。

## 不适用边界

- 它不是编译器、LSP 或测试框架，不能证明改动正确。
- 跨文件解析是基于静态抽取和名称解析，源码说明中也承认 ambiguous calls 可能返回多个候选。
- 刚编辑文件后索引有短暂滞后，不适合立即作为唯一事实来源。
- 需要 Node.js；`package.json` 要求 `>=18.0.0 <25.0.0`，CLI 对 Node 25 还有硬阻断逻辑。
- 如果 `better-sqlite3` 原生模块不可用，会退到 WASM SQLite，README 说明可能慢 5-10 倍，并可能出现 `database is locked`。

## 技术栈

- 语言：TypeScript
- CLI：`commander`、`@clack/prompts`
- 解析：`web-tree-sitter`、`tree-sitter-wasms`
- 数据库：SQLite；优先 `better-sqlite3`，备用 `node-sqlite3-wasm`
- 配置解析：JSON、JSONC、TOML 手写窄序列化
- 测试：Vitest
- 分发：npm 包 `@colbymchenry/codegraph`

## 初步评价

事实层面：项目已经有完整 CLI、MCP server、多 agent 安装器、多语言抽取、测试套件和 changelog，代码结构清晰，使用门槛低。

个人判断：它的价值点不是“语义搜索”本身，而是给 agent 一个本地、结构化、低延迟的代码图谱入口。对 Claude Code / Codex 这类会频繁探索文件的 agent 来说，这类工具能显著改善“先扫半天目录再读文件”的体验。但它仍属于辅助上下文层，关键改动仍需要结合源码阅读、类型检查和测试验证。
