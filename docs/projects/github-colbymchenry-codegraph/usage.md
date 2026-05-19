# colbymchenry/codegraph 使用方式

## 最短可用流程

如果只是想把它接入一个 agent 并用于当前代码库，按这个顺序来：

```bash
npx @colbymchenry/codegraph
```

安装器会交互式选择要配置的 agent，例如 Claude Code、Cursor、Codex CLI、opencode，并写入 MCP 配置和对应的 instructions 文件。

然后进入要研究或开发的项目：

```bash
cd your-project
codegraph init -i
```

`init -i` 会创建 `.codegraph/`、写入 `.codegraph/config.json`、建立 `.codegraph/codegraph.db`，并立即索引项目。之后重启 agent，让它加载新的 MCP server。agent 看到项目里有 `.codegraph/` 后，就可以使用 `codegraph_*` 工具。

## 前置条件

- Node.js `>=18.0.0 <25.0.0`，来源：`package.json`。
- npm / npx。
- 一个支持 MCP 的 agent：Claude Code、Cursor、Codex CLI、opencode。
- 目标项目最好是 git 仓库；源码中索引器会优先用 `git ls-files` / `git status` 获取可见文件和变更文件，非 git 项目会回退到文件系统扫描。

注意：CLI 在 `src/bin/codegraph.ts` 中会阻断 Node 25，因为项目注释说明 Node 25 的 V8 / WASM JIT 问题会导致 tree-sitter grammar 编译崩溃。只有设置 `CODEGRAPH_ALLOW_UNSAFE_NODE` 才会继续，但不建议日常使用。

## 安装方式

### 交互式安装

```bash
npx @colbymchenry/codegraph
```

等价于运行交互式 installer。README 说明它会：

- 自动检测 Claude Code、Cursor、Codex CLI、opencode。
- 询问是否把 `codegraph` 安装到 PATH。
- 询问配置是全局还是当前项目本地。
- 写入 MCP server 配置和 instructions 文件。
- 如果目标包含 Claude Code，可写入 auto-allow 权限。
- 对本地安装场景初始化当前项目。

### 全局安装

```bash
npm install -g @colbymchenry/codegraph
codegraph install
```

适合想长期在多个项目复用 `codegraph` 命令的用户。

### 非交互式安装

```bash
codegraph install --yes
codegraph install --target=cursor,claude --yes
codegraph install --target=auto --location=local
codegraph install --print-config codex
```

关键参数来自 README 和 `src/bin/codegraph.ts`：

| 参数 | 作用 |
|---|---|
| `--target` | `auto`、`all`、`none` 或逗号分隔的 agent id，例如 `claude,cursor,codex,opencode` |
| `--location` | `global` 或 `local` |
| `--yes` | 非交互式，使用默认值 |
| `--no-permissions` | 跳过 Claude Code auto-allow 权限写入 |
| `--print-config <id>` | 只打印某个 agent 的配置片段，不写文件 |

## Codex CLI 手动配置

本次本地验证运行：

```bash
node dist/bin/codegraph.js install --print-config codex
```

得到的配置片段是：

```toml
[mcp_servers.codegraph]
command = "codegraph"
args = ["serve", "--mcp"]
```

应加入：

```text
~/.codex/config.toml
```

`src/installer/targets/codex.ts` 还会把 CodeGraph 使用说明写入：

```text
~/.codex/AGENTS.md
```

该文件会提醒 agent：结构性问题优先用 `codegraph_search`、`codegraph_context`、`codegraph_callers`、`codegraph_impact` 等工具；literal text、日志字符串、注释文本等才优先用 grep/read。

## Claude Code 手动配置

本次本地验证运行：

```bash
node dist/bin/codegraph.js install --print-config claude
```

得到的配置片段是：

```json
{
  "mcpServers": {
    "codegraph": {
      "type": "stdio",
      "command": "codegraph",
      "args": ["serve", "--mcp"]
    }
  }
}
```

应加入：

```text
~/.claude.json
```

README 还给出 Claude auto-allow 权限示例，允许这些 MCP 工具：

- `mcp__codegraph__codegraph_search`
- `mcp__codegraph__codegraph_context`
- `mcp__codegraph__codegraph_callers`
- `mcp__codegraph__codegraph_callees`
- `mcp__codegraph__codegraph_impact`
- `mcp__codegraph__codegraph_node`
- `mcp__codegraph__codegraph_status`
- `mcp__codegraph__codegraph_files`

## 初始化项目

进入目标项目：

```bash
cd your-project
codegraph init -i
```

常见变体：

```bash
codegraph init              # 只初始化，不立即索引
codegraph index             # 对已初始化项目做完整索引
codegraph index --force     # 强制重新索引
codegraph sync              # 增量同步变更
codegraph status            # 查看索引状态
```

初始化后会生成：

```text
.codegraph/
  .gitignore
  codegraph.db
  config.json
```

`.codegraph/.gitignore` 默认忽略数据库、WAL、缓存、日志和脏标记，因此索引产物不应提交到仓库。

## 配置 `.codegraph/config.json`

本次临时项目验证生成的默认配置包含：

```json
{
  "version": 1,
  "include": ["**/*.ts", "**/*.tsx", "**/*.js", "..."],
  "exclude": ["**/.git/**", "**/node_modules/**", "**/dist/**", "..."],
  "languages": [],
  "frameworks": [],
  "maxFileSize": 1048576,
  "extractDocstrings": true,
  "trackCallSites": true
}
```

常改项：

- `include`：要纳入索引的文件 glob。
- `exclude`：排除目录和文件，例如生成物、依赖、缓存。
- `languages`：为空时自动识别；也可以显式限制语言。
- `frameworks`：框架提示，帮助路由和引用解析。
- `maxFileSize`：默认 1MB，超过会跳过。
- `extractDocstrings`：是否抽取文档字符串。
- `trackCallSites`：是否记录调用位置。

## 常用 CLI 命令

### 查看状态

```bash
codegraph status
codegraph status /path/to/project
codegraph status --json
```

本地验证中，一个 2 文件 TypeScript 临时项目输出：

```text
Files:     2
Nodes:     7
Edges:     11
DB Size:   0.13 MB
Backend:   native
Index is up to date
```

`Backend: native` 表示使用 `better-sqlite3` 快速路径。

### 搜索符号

```bash
codegraph query Auth --path /path/to/project
codegraph query Auth --kind class --limit 5
codegraph query Auth --json
```

本地验证中，搜索 `Auth` 返回了 `AuthService` class、`createAuthService` function、`login` method 和 `src/auth.ts` file，并带有文件路径、行号、签名等结构化字段。

### 查看文件结构

```bash
codegraph files --format tree
codegraph files --format flat
codegraph files --filter src
codegraph files --pattern "**/*.test.ts"
codegraph files --max-depth 2
```

本地验证中输出：

```text
src/auth.ts (typescript, 4 symbols)
src/main.ts (typescript, 3 symbols)
```

### 为任务构建上下文

```bash
codegraph context "login flow" --max-nodes 20 --max-code 10
codegraph context "fix user auth bug" --format json
codegraph context "login flow" --no-code
```

本地验证中，`codegraph context "login flow"` 能返回 entry points、related symbols 和相关 TypeScript 代码块。这个命令适合让 agent 在改代码前先拿一份聚焦上下文。

### 查受影响测试

```bash
codegraph affected src/utils.ts src/api.ts
git diff --name-only | codegraph affected --stdin
codegraph affected src/auth.ts --filter "e2e/*"
```

它会沿导入依赖关系向外追踪，找出可能依赖变更文件的测试文件。README 给出的 hook 示例：

```bash
AFFECTED=$(git diff --name-only HEAD | codegraph affected --stdin --quiet)
if [ -n "$AFFECTED" ]; then
  npx vitest run $AFFECTED
fi
```

## MCP 工具怎么用

给 agent 的实用规则：

| 问题 | 优先工具 |
|---|---|
| “X 在哪里定义？” | `codegraph_search` |
| “这个任务相关代码有哪些？” | `codegraph_context` |
| “谁调用了 Y？” | `codegraph_callers` |
| “Y 调用了什么？” | `codegraph_callees` |
| “改 Z 会影响什么？” | `codegraph_impact` |
| “看某个符号的签名/源码/文档” | `codegraph_node` |
| “初次了解一个陌生模块” | `codegraph_explore` |
| “某目录下有哪些已索引文件？” | `codegraph_files` |
| “索引是否健康？” | `codegraph_status` |

注意：`codegraph_explore` 输出较重。`src/mcp/tools.ts` 根据项目文件数建议调用预算：小于 500 文件建议 1 次，小于 5000 文件建议 2 次，小于 15000 文件建议 3 次，小于 25000 文件建议 4 次，更大建议 5 次。

## 作为 TypeScript 库使用

README 给出的库用法：

```ts
import CodeGraph from '@colbymchenry/codegraph';

const cg = await CodeGraph.init('/path/to/project');
// 或：const cg = await CodeGraph.open('/path/to/project');

await cg.indexAll({
  onProgress: (p) => console.log(`${p.phase}: ${p.current}/${p.total}`)
});

const results = cg.searchNodes('UserService');
const callers = cg.getCallers(results[0].node.id);
const context = await cg.buildContext('fix login bug', {
  maxNodes: 20,
  includeCode: true,
  format: 'markdown'
});
const impact = cg.getImpactRadius(results[0].node.id, 2);

cg.watch();
cg.unwatch();
cg.close();
```

## 本次验证情况

已验证：

- `npm install` 成功，安装 99 个包；`npm audit` 汇总显示 8 个漏洞，其中 6 个 moderate、2 个 high。
- `npm run build` 成功，生成 `dist/` 并复制 schema / wasm grammar 资源。
- `node dist/bin/codegraph.js --help` 能列出 `init/uninit/index/sync/status/query/files/context/serve/unlock/affected/install`。
- `install --print-config codex` 和 `install --print-config claude` 能打印配置片段，且未写入用户配置文件。
- 对 `/private/tmp/codegraph-demo` 两个 TypeScript 文件运行 `init -i` 成功，索引 2 文件、7 nodes、5 edges，随后 `status` 显示 11 edges；差异来自后续解析/引用解析后的统计口径。
- `query Auth --json`、`files --format flat`、`context "login flow"` 均返回符合预期的结构化结果。

未验证：

- 未把它真正写入当前机器的 Claude / Codex / Cursor / opencode 配置，因为这会修改用户目录。
- 未验证 Cursor、opencode 的实际 MCP 启动行为。
- 未在大型真实项目上验证 README 的性能收益数据。
- 未运行完整测试套件；本次只做了构建和 CLI 冒烟验证。

## 常见失败和排障

### `CodeGraph not initialized`

进入项目根目录运行：

```bash
codegraph init -i
```

或者显式传路径：

```bash
codegraph init -i /path/to/project
```

### MCP server 不连接

检查：

```bash
codegraph serve --mcp
codegraph status /path/to/project
```

同时确认 agent 配置中的命令是：

```text
codegraph serve --mcp
```

如果 agent 没有加载新 MCP 配置，需要重启 agent。

### 索引慢或 `database is locked`

先看：

```bash
codegraph status
```

如果看到 `Backend: wasm`，说明没有走 `better-sqlite3` 原生后端。README 建议安装构建工具后重建：

```bash
npm rebuild better-sqlite3
```

macOS 可能还需要：

```bash
xcode-select --install
```

### 符号缺失

检查：

- 文件扩展名是否在 `.codegraph/config.json` 的 `include` 中。
- 文件是否被 `exclude` 或 `.gitignore` 排除。
- 文件是否超过 `maxFileSize`。
- 是否刚刚保存文件，等待同步 debounce 后再查。
- 必要时运行 `codegraph sync` 或 `codegraph index --force`。
