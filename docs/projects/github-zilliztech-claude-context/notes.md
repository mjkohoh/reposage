# zilliztech/claude-context 阅读笔记

## 已阅读材料

- GitHub 仓库页：<https://github.com/zilliztech/claude-context>
- `README.md`
- `LICENSE`
- `package.json`
- `pnpm-workspace.yaml`
- `.env.example`
- `CONTRIBUTING.md`
- `.github/workflows/ci.yml`
- `.github/workflows/release.yml`
- `docs/README.md`
- `docs/getting-started/environment-variables.md`
- `docs/dive-deep/file-inclusion-rules.md`
- `docs/dive-deep/asynchronous-indexing-workflow.md`
- `packages/core/README.md`
- `packages/core/package.json`
- `packages/core/src/context.ts`
- `packages/core/src/splitter/ast-splitter.ts`
- `packages/core/src/sync/synchronizer.ts`
- `packages/core/src/vectordb/milvus-vectordb.ts`
- `packages/mcp/README.md`
- `packages/mcp/package.json`
- `packages/mcp/src/index.ts`
- `packages/mcp/src/config.ts`
- `packages/mcp/src/handlers.ts`
- `packages/mcp/src/snapshot.ts`
- `packages/mcp/src/sync.ts`
- `packages/vscode-extension/README.md`
- `packages/vscode-extension/package.json`
- `examples/basic-usage/index.ts`

## 仓库快照

- 默认分支：`master`
- 本地浅克隆提交：`3675469 feat: deduplicate overlapping search results (#333)`
- 顶层版本：`0.1.11`
- 包：
  - `@zilliz/claude-context-core`
  - `@zilliz/claude-context-mcp`
  - `semanticcodesearch`
- 许可：MIT

## 关键发现

### 1. 项目是“语义检索基础设施”，不是普通 Claude 插件

虽然名字里有 Claude，但实现分为 core、MCP server、VS Code extension。MCP server 只是把 core 的能力暴露给 agent。

### 2. 默认 hybrid search

`Context.getIsHybrid()` 默认返回 true。索引 collection 使用 `hybrid_code_chunks` 前缀；搜索时会同时使用 dense vector 和 sparse vector，再做 RRF rerank。

### 3. 绝对路径是 codebase identity

所有 MCP 工具都强调绝对路径。collection 名由 `path.resolve(codebasePath)` 的 MD5 前 8 位生成。文档也明确说不同绝对路径会被视为不同 codebase。

### 4. 异步索引是 MCP 体验关键

`handleIndexCodebase()` 不是同步等待 `Context.indexCodebase()` 完成，而是设置 snapshot 状态后启动 `startBackgroundIndexing()`。这能让 agent 立即继续工作，但也带来“搜索结果可能不完整”的行为。

### 5. 状态恢复逻辑很重

`handlers.ts` 和 `snapshot.ts` 有大量针对 snapshot 与云端 collection 不一致的恢复逻辑，尤其避免写入 `0 files / 0 chunks / completed` 这种会导致客户端误判的状态。源码注释多次提到 Issue #295。

### 6. 文档和源码有少量不一致

- README 要求 Node.js >= 20；`CONTRIBUTING.md` 要求 >=20 且 <24；CI 又跑 Node 24。
- `packages/mcp/src/config.ts` 出现 `OpenRouter` provider，但 README 和环境变量文档主列表未把它作为正式 provider 展开。
- core README 说 AST splitter 是 default；`Context` 构造确实默认 `AstCodeSplitter(2500, 300)`，但 VS Code 扩展配置默认 `chunkSize=1000`、`chunkOverlap=200`，不同入口默认参数可能不同。

## 风险与限制

- 安全/隐私：云 embedding provider 会接收代码 chunk；需要按团队数据政策评估。
- 成本：大仓库首次索引会消耗 embedding token/请求额度。
- 准确性：语义检索结果受 chunking、embedding model、query 表述、hybrid 参数影响，不等同于完整调用图。
- 运维：Milvus/Zilliz collection、本地 snapshot、Merkle 快照三者需要保持一致。
- 可测试性：CI 主要 build，未见完整端到端索引/搜索自动测试。

## 后续可深入方向

- 实际配置本地 Ollama + 本地 Milvus，验证完全本地化索引。
- 对比 `HYBRID_MODE=true/false` 的检索质量与耗时。
- 跑 `evaluation/` 中的 MCP efficiency 评估脚本，理解项目宣称的效率收益。
- 阅读 `milvus-restful-vectordb.ts`，确认 RESTful 与 gRPC 行为差异。
- 深入 VS Code 扩展 UI 和命令实现，补充用户交互细节。
- 检查 npm 包当前最新版本，确认仓库 `0.1.11` 与发布版本是否一致。

## 初步评价

个人判断：这个项目对 AI coding agent 的实用价值比较直接，尤其适合“代码库太大，agent 每次都在找文件”的场景。它把搜索上下文从 prompt 中移到可复用索引里，方向是对的。

但它不是银弹。真正可靠的代码理解仍需要组合语义搜索、文本搜索、类型检查、测试和人工判断。Claude Context 更像是 agent 的“高召回上下文雷达”，而不是代码事实数据库。
