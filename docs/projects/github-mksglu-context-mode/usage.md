# mksglu/context-mode 使用方式

## 安装方式

### Claude Code 插件市场

README 推荐 Claude Code 使用插件市场：

```text
/plugin marketplace add mksglu/context-mode
/plugin install context-mode@context-mode
```

然后重启 Claude Code 或运行：

```text
/reload-plugins
```

验证：

```text
/context-mode:ctx-doctor
```

Claude Code 插件 manifest 位于 `.claude-plugin/plugin.json`，其中注册 MCP server：

```json
{
  "mcpServers": {
    "context-mode": {
      "command": "node",
      "args": ["${CLAUDE_PLUGIN_ROOT}/start.mjs"]
    }
  },
  "skills": "./skills/"
}
```

### MCP-only 方式

如果只想使用 MCP 工具，不启用 hooks 和 slash commands：

```bash
claude mcp add context-mode -- npx -y context-mode
```

README 说明这种方式只有 sandbox tools，没有自动路由。

### npm 全局安装

多数非 Claude Code 平台要求：

```bash
npm install -g context-mode
```

然后在对应客户端配置 MCP server 命令：

```json
{
  "mcpServers": {
    "context-mode": {
      "command": "context-mode"
    }
  }
}
```

Codex CLI 示例：

```toml
[mcp_servers.context-mode]
command = "context-mode"
```

来源：`README.md`、`configs/codex/config.toml`

## 主要 MCP 工具

### `ctx_execute`

执行代码片段。适合：

- CLI 命令会输出超过 20 行时，用代码过滤后只输出结论。
- 调用 API、处理 JSON、解析日志、统计文件。
- 用 JavaScript/Python/Shell 等语言执行小型分析脚本。

关键参数：

- `language`
- `code`
- `timeout`
- `background`
- `intent`

大输出策略：

- `intent` 存在且输出超过 5KB：索引输出并返回匹配 section 预览。
- 输出超过 100KB：自动索引到 FTS5，只返回后续搜索提示。

### `ctx_execute_file`

读取文件到沙箱变量后执行处理代码，避免文件内容直接进入上下文。

适合：

- 日志
- CSV/JSON/XML
- 测试输出
- 构建输出
- 大源码文件分析

### `ctx_index`

把文档/API 参考/skill prompt/README 等文本索引进 FTS5。支持 `content` 或 `path`，可设置 `source` 标签。文件路径索引会保存 hash，搜索时可自动检测 stale source。

### `ctx_search`

搜索已索引内容。支持：

- `queries`：一次传入多个查询。
- `limit`
- `source`
- `contentType`
- `sort`：`relevance` 或 `timeline`

它有渐进节流：60 秒窗口内超过一定次数会限制每 query 结果数，继续单独调用会被阻止并要求使用批量工具。

### `ctx_fetch_and_index`

抓取 URL、转换/整理内容、索引入 FTS5，再返回约 3KB 预览。

特点：

- HTML -> Markdown
- JSON -> pretty JSON 后索引
- Text -> 直接索引
- 24 小时 TTL cache

### `ctx_batch_execute`

批量执行多条 shell command，自动索引所有输出，并在同一次调用中用多个 query 搜索。项目描述中称它是 primary tool。

输入结构：

```json
{
  "commands": [
    { "label": "README", "command": "sed -n '1,200p' README.md" }
  ],
  "queries": ["project purpose", "install command"],
  "timeout": 60000
}
```

### 工具类命令

- `ctx_stats`：当前会话上下文消耗统计。
- `ctx_doctor`：诊断 runtime、hooks、FTS5、版本、插件注册等。
- `ctx_upgrade`：升级/修复 hooks、权限、缓存。
- `ctx_purge`：删除索引内容。
- `ctx_insight`：打开本地分析仪表盘。

来源：`src/server.ts`、`README.md`

## Hook 与路由

Hook 的作用是让 agent 更倾向使用 context-mode 工具，而不是直接把大输出塞进上下文。

典型路由规则：

- 禁止直接 `curl` / `wget` 输出到 stdout，建议用 `ctx_fetch_and_index` 或 `ctx_execute`。
- Shell 只适合 git 写操作、文件变更、安装等；大输出命令建议进 sandbox。
- 文件用于编辑可以正常读；文件用于分析则建议 `ctx_execute_file`。
- grep/search 大结果建议进 sandbox。

来源：`configs/codex/AGENTS.md`、`hooks/core/routing.mjs`

## 会话连续性

数据路径按平台隔离。例如 Claude Code：

```text
~/.claude/context-mode/sessions/
~/.claude/context-mode/content/
```

关键流程：

1. `PostToolUse` 捕获工具调用结果，抽取事件写入 SessionDB。
2. `UserPromptSubmit` 捕获用户提示。
3. `PreCompact` 在压缩前构建 resume snapshot。
4. `SessionStart` 在 startup/compact/resume 时注入 routing block 和会话恢复指令。
5. MCP server 在 `getStore()` 时扫描 `*-events.md`，索引到 ContentStore 后删除文件。

来源：`CONTRIBUTING.md`、`hooks/sessionstart.mjs`、`src/server.ts`

## 支持平台

`docs/platform-support.md` 说明项目支持三种 hook paradigm：

- JSON stdin/stdout：Claude Code、Gemini CLI、VS Code Copilot、JetBrains Copilot、Cursor、Codex CLI、Qwen Code 等。
- TS Plugin：OpenCode。
- MCP-only：Antigravity、Kiro。

注意：不同平台 hook 能力不完全一致。Codex CLI 文档写明 PreToolUse 不能改写参数，不能改写输出，context injection 主要通过 PostToolUse 和 SessionStart。

## 开发使用

开发命令：

```bash
npm install
npm run build
npm test
npm run typecheck
npm run bundle
```

CLI：

```bash
context-mode
context-mode doctor
context-mode upgrade
context-mode hook <platform> <event>
context-mode insight
```

`start.mjs` 会优先加载 `server.bundle.mjs`，如果不存在再加载 `build/server.js`。本地开发时 `CONTRIBUTING.md` 特别提醒：如果想验证 `build/server.js` 改动，需要删除 `server.bundle.mjs`。

## 本次验证情况

已验证：

- 成功浅克隆仓库到 `/private/tmp/reposage-context-mode`。
- 阅读 README、LICENSE、package、核心源码、hook、adapter、benchmark、CI、platform support。
- 确认版本 `1.0.103`，默认分支 `main`，许可证为 Elastic-2.0。

未执行：

- 未运行 `npm install`、`npm run build`、`npm test`，避免下载依赖和构建本地插件。
- 未实际安装到 Claude Code/Codex/Gemini 等客户端。
- 未实际运行 `ctx_doctor` 或 MCP 工具端到端。

常见阻塞点推断：

- `better-sqlite3` 原生依赖安装失败或 FTS5 不可用。
- hook 配置重复导致双重调用、SQLite locking 或非阻塞 hook 错误。
- 平台 hook 能力不支持参数改写，导致只能注入提示，不能强制路由。
- Windows 路径、Git Bash、PowerShell cmdlet 与 bash sandbox 之间有兼容成本。
- MCP server 未就绪时，路由 hook 会放行，自动治理能力下降。
