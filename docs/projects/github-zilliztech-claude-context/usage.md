# zilliztech/claude-context 使用方式

## 前置条件

官方文档要求：

- Node.js >= 20.0.0
- MCP 客户端，例如 Claude Code、Codex CLI、Cursor 等
- 一个 embedding provider 的 API key 或本地 Ollama
- Milvus/Zilliz Cloud 向量数据库

来源：`README.md`、`packages/mcp/README.md`、`docs/getting-started/environment-variables.md`

注意：`CONTRIBUTING.md` 写 Node.js >= 20 且 < 24，但 CI matrix 覆盖 Node 20、22、24。这说明运行/开发版本约束在文档中不完全一致。

## Claude Code MCP 配置

README 推荐：

```bash
claude mcp add claude-context \
  -e OPENAI_API_KEY=sk-your-openai-api-key \
  -e MILVUS_ADDRESS=your-zilliz-cloud-public-endpoint \
  -e MILVUS_TOKEN=your-zilliz-cloud-api-key \
  -- npx @zilliz/claude-context-mcp@latest
```

如果只使用全局配置文件，可以把环境变量放到：

```text
~/.context/.env
```

环境变量优先级：

1. 进程环境变量
2. `~/.context/.env`
3. 默认值

来源：`docs/getting-started/environment-variables.md`

## Codex CLI 配置

README 给出的 TOML 示例：

```toml
[mcp_servers.claude-context]
command = "npx"
args = ["@zilliz/claude-context-mcp@latest"]
env = { "OPENAI_API_KEY" = "your-openai-api-key", "MILVUS_TOKEN" = "your-zilliz-cloud-api-key" }
startup_timeout_ms = 20000
```

注意 README 特别提示 Codex CLI 顶层 key 是 `mcp_servers`，不是 `mcpServers`。

## 全局环境变量配置

可以创建：

```bash
mkdir -p ~/.context
cat > ~/.context/.env << 'EOF'
EMBEDDING_PROVIDER=OpenAI
OPENAI_API_KEY=sk-your-openai-api-key
EMBEDDING_MODEL=text-embedding-3-small
MILVUS_ADDRESS=your-zilliz-cloud-public-endpoint
MILVUS_TOKEN=your-zilliz-cloud-api-key
EOF
```

常见变量：

- `EMBEDDING_PROVIDER`：`OpenAI`、`VoyageAI`、`Gemini`、`Ollama`
- `EMBEDDING_MODEL`：embedding 模型名
- `OPENAI_API_KEY`、`VOYAGEAI_API_KEY`、`GEMINI_API_KEY`
- `OLLAMA_HOST`、`OLLAMA_MODEL`
- `MILVUS_ADDRESS`、`MILVUS_TOKEN`
- `HYBRID_MODE`：默认 `true`
- `EMBEDDING_BATCH_SIZE`：默认 `100`
- `SPLITTER_TYPE`：`ast` 或 `langchain`
- `CUSTOM_EXTENSIONS`
- `CUSTOM_IGNORE_PATTERNS`
- `CODE_CHUNKS_COLLECTION_NAME_OVERRIDE`
- `CLAUDE_CONTEXT_TRIGGER_WATCHER`

来源：`.env.example`、`docs/getting-started/environment-variables.md`、`packages/mcp/src/config.ts`

## MCP 工具

`packages/mcp/src/index.ts` 定义了四个工具。

### `index_codebase`

索引指定代码库目录。

关键参数：

- `path`：必须是目标代码库绝对路径。
- `force`：是否强制重建索引。
- `splitter`：`ast` 或 `langchain`，默认 `ast`。
- `customExtensions`：额外纳入索引的扩展名，例如 `.vue`、`.svelte`。
- `ignorePatterns`：额外排除规则。

行为：

- 校验路径存在且是目录。
- 检查 collection limit。
- 写入 `~/.context/mcp-codebase-snapshot.json` 中的 indexing 状态。
- 后台启动索引，立即返回。

### `search_code`

在已索引代码库中搜索。

关键参数：

- `path`：代码库绝对路径，也可以是已索引代码库的子目录路径。
- `query`：自然语言查询。
- `limit`：最多结果数，最大 50。
- `extensionFilter`：可选扩展名过滤，例如 `.ts`、`.py`。

行为：

- 如果代码库未索引，会提示先运行 `index_codebase`。
- 如果索引中，仍可搜索，但结果可能不完整。
- 对重叠结果做去重，当前最新提交就是相关改动。

### `clear_index`

删除某个代码库的向量 collection，并删除本地 Merkle 快照。

### `get_indexing_status`

查看索引状态。状态包括：

- `indexed`
- `indexing`
- `indexfailed`
- `not_found`

进度是阶段性百分比，不是精确文件比例。

## Core 包用法

可以直接安装 core 包：

```bash
npm install @zilliz/claude-context-core
```

核心调用流程：

```ts
import {
  Context,
  OpenAIEmbedding,
  MilvusVectorDatabase
} from "@zilliz/claude-context-core";

const embedding = new OpenAIEmbedding({
  apiKey: process.env.OPENAI_API_KEY!,
  model: "text-embedding-3-small"
});

const vectorDatabase = new MilvusVectorDatabase({
  address: process.env.MILVUS_ADDRESS!,
  token: process.env.MILVUS_TOKEN
});

const context = new Context({ embedding, vectorDatabase });

await context.indexCodebase("/absolute/path/to/repo");
const results = await context.semanticSearch(
  "/absolute/path/to/repo",
  "where is authentication handled",
  5
);
```

来源：`packages/core/README.md`、`examples/basic-usage/index.ts`

## VS Code 扩展

扩展名：`Semantic Code Search`

主要命令：

- `Semantic Code Search: Index Codebase`
- `Semantic Code Search: Semantic Search`
- `Semantic Code Search: Clear Index`

配置项包括 embedding provider、模型、API key、Milvus 地址、auto sync、splitter 类型、chunk size、chunk overlap 等。

来源：`packages/vscode-extension/README.md`、`packages/vscode-extension/package.json`

## 开发与构建

仓库使用 pnpm workspace：

```bash
pnpm install
pnpm build
pnpm dev
```

常用脚本：

- `pnpm build`：构建所有包和 examples。
- `pnpm build:core`
- `pnpm build:mcp`
- `pnpm build:vscode`
- `pnpm typecheck`
- `pnpm lint`
- `pnpm clean`
- `pnpm example:basic`

来源：`package.json`、`CONTRIBUTING.md`

## 本次验证情况

已验证：

- 成功浅克隆仓库到 `/private/tmp/reposage-claude-context`。
- 确认默认分支 `master`，本地提交 `3675469`。
- 阅读 README、docs、package 配置、核心源码入口、MCP handlers、sync/snapshot、VS Code 扩展配置。

未执行：

- 未运行 `pnpm install` / `pnpm build`，因为会下载较多 npm 依赖，且当前目标是源码调研。
- 未实际启动 MCP server，因为需要真实 embedding provider key 与 Milvus/Zilliz 配置。
- 未执行索引与搜索端到端验证。

常见阻塞原因推断：

- API key 或 Milvus/Zilliz endpoint/token 配置缺失。
- MCP client 启动超时，需要提高 `startup_timeout_ms`。
- 同一代码库使用不同绝对路径导致 search 找不到已有索引。
- 初次索引尚未完成，搜索结果不完整。
- collection limit 或 chunk limit 导致索引无法创建或不完整。
