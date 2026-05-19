# colbymchenry/codegraph 调研笔记

## 已阅读材料

- `README.md`
- `package.json`
- `CHANGELOG.md`
- `LICENSE`
- `src/bin/codegraph.ts`
- `src/index.ts`
- `src/types.ts`
- `src/config.ts`
- `src/directory.ts`
- `src/extraction/index.ts`
- `src/mcp/tools.ts`
- `src/mcp/server-instructions.ts`
- `src/installer/instructions-template.ts`
- `src/installer/targets/codex.ts`
- 项目文件列表和测试目录概览
- GitHub 页面：<https://github.com/colbymchenry/codegraph>
- npm 页面：<https://www.npmjs.com/package/@colbymchenry/codegraph>

## 调研快照

- 日期：2026-05-19
- 本地浅克隆目录：`/private/tmp/reposage-codegraph`
- 目标仓库：`colbymchenry/codegraph`
- 默认分支：`main`
- 本次提交：`9b6a917d3235235c01d5046941a50a65551fa8db`
- npm 包版本：`0.7.9`
- Node 要求：`>=18.0.0 <25.0.0`
- 许可：MIT
- GitHub 页面约数：1.1k stars、121 forks、11 watchers

## 关键发现

### 它是面向 agent 的结构化代码上下文层

CodeGraph 的核心不是“给人看的 UI”，而是让 agent 通过 MCP 查询预先索引好的代码图谱。README 和 `src/mcp/server-instructions.ts` 都强调：结构性问题应优先用 CodeGraph，literal text 才用 grep。

### 使用门槛低

最短流程是：

```bash
npx @colbymchenry/codegraph
cd your-project
codegraph init -i
```

无需 API key，无需外部数据库。对习惯 Node/npm 的用户很友好。

### 安装器是重要能力

`0.7.7` changelog 显示项目从 Claude-only 扩展到 Claude Code、Cursor、Codex CLI、opencode 的 multi-agent installer。对 Codex CLI，它会写 `~/.codex/config.toml` 和 `~/.codex/AGENTS.md`。

### 文档中的性能数据需要按“官方基准”理解

README 给出多个仓库的 benchmark，并称平均减少 92% tool calls、加快 71%。这些是作者测试条件下的结果，不应直接推断到所有代码库。影响因素包括语言、项目规模、索引质量、agent 是否遵守 instructions、问题类型等。

### 本地验证效果符合预期

临时 TypeScript 项目中，`init -i` 能成功抽取 class/function/method/import/file，`query` 能按 `Auth` 找到相关符号，`context` 能把 `login` 和调用它的 `runLogin` 放进上下文。

## 本次验证命令

在 `/private/tmp/reposage-codegraph`：

```bash
npm install
npm run build
node dist/bin/codegraph.js --help
node dist/bin/codegraph.js install --print-config codex
node dist/bin/codegraph.js install --print-config claude
```

在 `/private/tmp/codegraph-demo`：

```bash
node /private/tmp/reposage-codegraph/dist/bin/codegraph.js init -i /private/tmp/codegraph-demo
node /private/tmp/reposage-codegraph/dist/bin/codegraph.js status /private/tmp/codegraph-demo
node /private/tmp/reposage-codegraph/dist/bin/codegraph.js query Auth --path /private/tmp/codegraph-demo --json
node /private/tmp/reposage-codegraph/dist/bin/codegraph.js files --path /private/tmp/codegraph-demo --format flat
node /private/tmp/reposage-codegraph/dist/bin/codegraph.js context "login flow" --path /private/tmp/codegraph-demo --max-nodes 5 --max-code 3
```

结果摘要：

- 构建成功。
- CLI help 正常。
- 配置片段打印正常。
- 临时项目索引成功。
- `status` 显示 `Backend: native`。
- `query/files/context` 返回结果正常。

## 风险和限制

- `npm install` 后 audit 显示 8 个漏洞，其中 6 个 moderate、2 个 high；本次未展开审计详情，也未判断是否影响运行路径。
- `better-sqlite3` 是 optional dependency；如果原生模块装不上，会退到较慢的 WASM backend。
- README 中 supported languages 很广，但具体语言的抽取质量需要按项目语言单独验证。
- 对动态语言、框架魔法、运行时依赖注入、宏等场景，静态图谱可能漏边或误连。
- `.codegraph/` 是本地索引目录，不应提交；团队环境中每个人都要本地初始化。
- agent 是否真的优先使用 CodeGraph，取决于目标客户端是否正确加载 MCP 和 instructions。

## 未解问题

- 当前 `0.7.9` 相比 `CHANGELOG.md` 最新记录 `0.7.8` 的具体新增内容尚未在 changelog 中记录。
- Cursor / opencode 的本地配置写入和实际启动未验证。
- 大型仓库索引速度、内存占用、SQLite 锁行为未实测。
- `codegraph affected` 对不同测试命名约定的准确性未验证。
- 多语言混合仓库中的跨语言调用解析能力需要用真实项目测试。

## 后续研究方向

- 在一个真实中型 TypeScript 或 Python 项目中完整接入 Codex CLI，观察 agent 是否减少文件扫描。
- 对比 `codegraph_context` 与手动 `rg/read` 的上下文质量和 token 使用量。
- 跑完整 `npm run test`，查看测试覆盖和失败风险。
- 深入 `src/db/schema.sql` 和 `src/db/queries.ts`，明确图谱 schema 和查询性能边界。
- 研究 `src/resolution/frameworks/`，评估路由识别对实际 Web 项目的可用度。

## 个人评价

这是一个值得关注的 agent 基础设施工具。它抓住了 AI coding agent 的一个真实痛点：agent 不擅长长期记住项目结构，每轮任务又经常浪费大量上下文在“找文件”。CodeGraph 用本地索引把结构性问题变成低成本查询，方向很实用。

我会把它当作“提升探索效率的辅助层”，而不是代码理解的唯一来源。真正改代码前，仍然需要打开关键源码、运行类型检查和测试。
