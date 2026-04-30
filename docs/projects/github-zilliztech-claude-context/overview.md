# zilliztech/claude-context 项目概览

## 高信号总结

`zilliztech/claude-context` 是一个面向 Claude Code、Codex、Cursor、Gemini CLI 等 AI coding agent 的语义代码搜索工具。它通过 MCP server 暴露 `index_codebase`、`search_code`、`clear_index`、`get_indexing_status` 等工具，把本地代码库切分成代码块、生成 embedding、写入 Milvus/Zilliz Cloud，并在 agent 需要上下文时用自然语言查询相关代码。

它解决的核心问题是：大代码库不能每次都整体塞进模型上下文，但传统关键词搜索又经常找不到“语义相关”的实现。Claude Context 的方案是把代码库预先索引到向量数据库中，之后按查询只取相关代码片段进入 agent 上下文，从而降低上下文成本并减少多轮文件探索。

该项目不是 Claude 专用。README 明确给出 Claude Code、OpenAI Codex CLI、Gemini CLI、Qwen Code、Cursor、Void、Claude Desktop、Windsurf、VS Code、Cherry Studio、Cline、Augment、Roo Code 等 MCP 客户端配置方式。仓库还包含一个独立 VS Code 扩展和可直接用的 core npm 包。

来源：

- 仓库：<https://github.com/zilliztech/claude-context>
- 默认分支：`master`
- 本次浅克隆提交：`3675469 feat: deduplicate overlapping search results (#333)`
- 许可：MIT，见 `LICENSE`
- GitHub 页面显示：约 10.3k stars、766 forks、194 commits（访问时间：2026-04-30）

## 项目定位

README 将项目定位为 “Your entire codebase as Claude's context”。更准确地说，它是一个“代码库语义索引层”，可以被三类入口使用：

- MCP server：`@zilliz/claude-context-mcp`
- TypeScript core library：`@zilliz/claude-context-core`
- VS Code 扩展：`semanticcodesearch`

核心能力来自 `packages/core`，MCP server 和 VS Code 扩展都是它的不同壳层。

## 目标用户

- 使用 AI coding agent 维护中大型代码库的开发者。
- 希望让 agent 在改代码前更快定位相关实现的团队。
- 希望用 Milvus/Zilliz Cloud 存储代码 embedding 的用户。
- 需要在 VS Code 中做自然语言代码搜索的开发者。

## 核心能力

### 语义代码搜索

用户可以用自然语言查询代码，例如 “function that handles user authentication”。项目会把查询转成 embedding，并在对应代码库 collection 中检索相关代码片段。

来源：`packages/core/src/context.ts`、`packages/mcp/src/index.ts`

### 异步索引

MCP 的 `index_codebase` 会立即返回，实际索引在后台执行。用户可以通过 `get_indexing_status` 查看进度，也可以在索引过程中搜索，结果可能不完整。

来源：`docs/dive-deep/asynchronous-indexing-workflow.md`、`packages/mcp/src/handlers.ts`

### 增量同步

项目使用 `FileSynchronizer` 和 Merkle 快照检测文件新增、删除、修改。MCP server 启动后会做后台同步，并支持通过触碰 `~/.context/.sync-trigger` 触发立即同步。

来源：`packages/core/src/sync/synchronizer.ts`、`packages/mcp/src/sync.ts`

### 混合检索

默认 `HYBRID_MODE=true`，collection 名使用 `hybrid_code_chunks_<pathHash>`。检索时同时发起 dense vector search 和 sparse/BM25 search，并使用 RRF rerank。

来源：`packages/core/src/context.ts`、`docs/getting-started/environment-variables.md`

### 多 embedding provider

支持 OpenAI、VoyageAI、Gemini、Ollama。源码中 MCP config 还出现了 `OpenRouter` 配置项；但 README 与环境变量文档主要列的是前四者，因此 OpenRouter 支持状态建议标为“源码可见，文档覆盖不充分”。

来源：`packages/core/src/embedding/`、`packages/mcp/src/config.ts`

### 文件包含/排除规则

最终索引文件 = 支持扩展名集合 - ignore pattern 集合。扩展名来源包括默认扩展、MCP 参数、环境变量；ignore 来源包括默认规则、MCP 参数、环境变量、`.gitignore`、`.xxxignore`、全局 `~/.context/.contextignore`。

来源：`docs/dive-deep/file-inclusion-rules.md`

## 适用场景

- 在大型代码库中让 agent 快速找到相关实现、接口、调用点。
- 在开始改功能前检索已有模式，减少 agent 盲目读目录。
- 用 MCP 给多个 AI 编程客户端共享同一套语义检索能力。
- 用 VS Code 扩展给开发者提供语义搜索面板。
- 用 core 包在自定义工具里嵌入代码索引和搜索能力。

## 不适用边界

- 不适合不想使用外部向量数据库的场景；当前官方支持重点是 Milvus/Zilliz Cloud。
- 如果使用 OpenAI、VoyageAI、Gemini 等云 embedding provider，代码片段会发送到第三方 embedding 服务；对严格内网或敏感代码场景，需要评估 Ollama 本地 embedding 与本地 Milvus。
- 语义搜索不能替代精确符号分析。查重、重构影响面、类型引用关系仍需要 `rg`、LSP、编译器或静态分析配合。
- 初次索引大仓库会产生 embedding 成本和较长耗时；项目有 450,000 chunk 限制，超大仓库可能索引不完整。
- 默认按绝对路径识别代码库；同一仓库通过 symlink 或不同路径索引，会被当作不同 codebase。

## 技术栈

- 语言：TypeScript
- 包管理：pnpm workspace
- Node：README 要求 Node.js >= 20；`CONTRIBUTING.md` 写 Node.js >= 20 且 < 24；CI 覆盖 20、22、24，存在轻微不一致。
- MCP：`@modelcontextprotocol/sdk`
- 向量数据库：Milvus / Zilliz Cloud，Node SDK `@zilliz/milvus2-sdk-node`
- Embedding：OpenAI、VoyageAI、Gemini、Ollama
- 代码切分：tree-sitter AST splitter + LangChain fallback
- VS Code 扩展：VS Code Extension API、webpack

## 初步评价

事实层面：项目已经形成 core/mcp/vscode-extension 的完整 monorepo，并且有较多使用文档、deep dive、CI 和发布 workflow。

个人判断：它的最大价值是把“语义搜索”做成 MCP 工具，让 agent 能主动检索相关代码，而不是靠用户手动塞上下文。主要风险在于外部服务依赖多、索引状态需要维护、以及语义检索结果仍可能漏掉精确依赖关系。
