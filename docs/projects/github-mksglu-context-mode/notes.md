# mksglu/context-mode 阅读笔记

## 已阅读材料

- GitHub 仓库页：<https://github.com/mksglu/context-mode>
- `README.md`
- `LICENSE`
- `package.json`
- `.claude-plugin/plugin.json`
- `.mcp.json`
- `configs/codex/AGENTS.md`
- `configs/codex/config.toml`
- `configs/codex/hooks.json`
- `docs/platform-support.md`
- `BENCHMARK.md`
- `CONTRIBUTING.md`
- `.github/workflows/ci.yml`
- `tests/benchmark-results-v04.json`
- `src/server.ts`
- `src/cli.ts`
- `src/executor.ts`
- `src/store.ts`
- `src/runtime.ts`
- `src/security.ts`
- `src/types.ts`
- `src/session/db.ts`
- `src/adapters/types.ts`
- `src/adapters/detect.ts`
- `src/adapters/client-map.ts`
- `hooks/pretooluse.mjs`
- `hooks/posttooluse.mjs`
- `hooks/precompact.mjs`
- `hooks/sessionstart.mjs`
- `hooks/core/routing.mjs`
- `hooks/session-directive.mjs`

## 仓库快照

- 默认分支：`main`
- 本地浅克隆提交：`8b04ae8 ci: update install stats`
- npm package：`context-mode`
- 版本：`1.0.103`
- 许可证：Elastic-2.0
- 包管理：packageManager 字段为 pnpm 10.23.0，但 README/CONTRIBUTING 开发命令主要用 `npm install`、`npm run build`

## 关键发现

### 1. 它优化的是“上下文流量”，不是“代码理解”

和 `zilliztech/claude-context` 不同，context-mode 不依赖向量数据库做代码库语义搜索。它使用 SQLite FTS5 处理工具输出、网页、文档和会话事件，目标是减少进入模型上下文的原始数据。

### 2. Hook 是产品体验核心

MCP-only 安装也能用工具，但 README 明确说没有自动 routing。完整体验依赖 PreToolUse、PostToolUse、PreCompact、SessionStart 等 hooks，把高风险 raw output 路由到 `ctx_*` 工具。

### 3. Session continuity 采用“两阶段注入”

它不是把所有历史事件塞回上下文，而是：

- SessionDB 存原始事件。
- compact/resume 时写 Markdown 事件文件。
- MCP server 自动索引事件文件到 ContentStore。
- 只注入少量 directive 和建议搜索。

这个设计比直接注入历史更节省上下文，但要求 agent 真的会调用 `ctx_search` 查细节。

### 4. 许可证需要特别注意

项目不是常见 MIT/Apache/BSD，而是 ELv2。允许使用、复制、分发和修改，但禁止把软件作为提供其实质功能的托管/管理服务提供给第三方，也不能绕过 license key 功能。

### 5. README 宣传强，文档需克制引用

README 有 “Used across teams at Microsoft/Google/Meta...” 等徽章式表达，但没有在本次调研中验证这些公司的正式采用情况。文档中不应把这些营销信号当作事实结论。

### 6. 平台支持文档可能滞后

`docs/platform-support.md` 开头说支持九个平台，但源码和 README 已覆盖 Qwen Code、OpenClaw、Pi、Zed 等更多入口。结论应以源码/config 为准，并标注文档可能滞后。

## 风险与疑问

- `server.ts` 职责过宽，包含 MCP tools、统计、版本提示、存储路径、session event indexing、平台修复等逻辑。
- Hook 重复注册是显式风险，CONTRIBUTING 提到会导致 double invocation、split session ID、SQLite locking errors。
- `better-sqlite3` 是 optional dependency，但核心 FTS5/SessionDB 都依赖 SQLite 能力，安装失败时实际降级行为需要端到端验证。
- 安全检查有 fail-open 路径，不能把它当作安全容器。
- Benchmark 结果来自 fixture 和项目定义的用例，不能直接外推到所有 agent 工作流。

## 后续可深入方向

- 在测试环境安装 Claude Code plugin，运行 `/context-mode:ctx-doctor` 验证实际注册状态。
- 运行 `npm test`，确认当前浅克隆提交测试是否全绿。
- 实测 Codex CLI hook 能力，确认 `configs/codex/hooks.json` 在当前 Codex 版本里的可用性。
- 深读 `src/session/extract.ts`，整理 23+ event categories 的抽取规则。
- 深读 `insight/`，补充 analytics 指标、dashboard 数据来源和使用价值。
- 对比 `context-mode` 与 `claude-context`：前者管理上下文流量和会话记忆，后者做代码库语义检索，两者问题域相邻但不同。

## 初步评价

个人判断：context-mode 是“把 agent 当成会浪费上下文的系统来治理”的工具，思路很务实。它的强项是把大输出治理、FTS 搜索、session memory 和平台 hooks 结合起来；弱项是引入了一个很重的运行时层，出了问题时排障面很大。
