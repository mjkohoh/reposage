# EveryInc/compound-engineering-plugin 使用方式

## 使用前判断

Compound Engineering 支持多个 Agent 宿主，但安装方式并不完全一致。最简单的是 Claude Code 和 Cursor 原生插件安装；Codex 的完整体验需要同时安装原生技能和转换后的 Agent；OpenCode、Pi、Gemini 和 Kiro 依赖 Bun CLI 转换安装。

项目根目录 README 是安装方式的权威来源：

- https://github.com/EveryInc/compound-engineering-plugin#install

## Claude Code

```text
/plugin marketplace add EveryInc/compound-engineering-plugin
/plugin install compound-engineering
```

## Cursor

在 Cursor Agent chat 中执行：

```text
/add-plugin compound-engineering
```

也可以在插件市场中搜索 `compound engineering`。

## Codex

Codex 需要三个步骤：

```bash
codex plugin marketplace add EveryInc/compound-engineering-plugin
bunx @every-env/compound-plugin install compound-engineering --to codex
codex
```

启动 Codex 后执行：

```text
/plugins
```

找到 `Compound Engineering` marketplace，选择 `compound-engineering` 并安装，然后重启 Codex。

原因是当前 Codex 原生插件规范可以注册 skills，但尚不能注册 CE 自带的 custom agents：

- 原生插件安装负责 skills。
- `bunx @every-env/compound-plugin install compound-engineering --to codex` 负责把 Agent 转换并写入 Codex 配置。

如果使用非默认 Codex profile，每一步都应使用同一个 `CODEX_HOME`：

```bash
CODEX_HOME="$HOME/.codex/profiles/work" codex plugin marketplace add EveryInc/compound-engineering-plugin
CODEX_HOME="$HOME/.codex/profiles/work" bunx @every-env/compound-plugin install compound-engineering --to codex
CODEX_HOME="$HOME/.codex/profiles/work" codex
```

## 其他宿主

### GitHub Copilot CLI

```bash
copilot plugin marketplace add EveryInc/compound-engineering-plugin
copilot plugin install compound-engineering@compound-engineering-plugin
```

### Factory Droid

```bash
droid plugin marketplace add https://github.com/EveryInc/compound-engineering-plugin
droid plugin install compound-engineering@compound-engineering-plugin
```

### Qwen Code

```bash
qwen extensions install EveryInc/compound-engineering-plugin:compound-engineering
```

### OpenCode、Pi、Gemini 和 Kiro

```bash
bunx @every-env/compound-plugin install compound-engineering --to opencode
bunx @every-env/compound-plugin install compound-engineering --to pi
bunx @every-env/compound-plugin install compound-engineering --to gemini
bunx @every-env/compound-plugin install compound-engineering --to kiro
```

Pi 还需要：

```bash
pi install npm:pi-subagents
pi install npm:pi-ask-user
```

## 首次设置

安装后，在目标项目中执行：

```text
/ce-setup
```

`ce-setup` 会检查 CLI 工具、Agent skills、仓库本地配置和 `.gitignore`，并交互式询问是否安装缺失依赖。它不会无提示地自动配置环境。

## 推荐工作流

### 新功能

```text
/ce-strategy
/ce-ideate
/ce-brainstorm "make background job retries safer"
/ce-plan docs/brainstorms/background-job-retry-safety-requirements.md
/ce-work
/ce-code-review
/ce-compound
```

其中 `ce-strategy` 和 `ce-ideate` 按需使用。常规功能可以从 `ce-brainstorm` 开始。

### 集中排查 bug

```text
/ce-debug "the checkout webhook sometimes creates duplicate invoices"
/ce-code-review
/ce-compound
```

### 小而明确的改动

可以直接使用：

```text
/ce-work "rename the deprecated config key"
```

`ce-work` 会先判断任务规模：无行为变化的简单改动可直接执行，小中型任务生成任务列表，大型或高风险任务会建议先进入 `ce-brainstorm` 或 `ce-plan`。

## CLI 开发方式

仓库使用 Bun：

```bash
bun install
bun test
bun run release:validate
```

本地调试 Codex 转换安装：

```bash
bun run src/index.ts install ./plugins/compound-engineering --to codex
```

## 本次验证

已完成：

- 浅克隆仓库并检查 `main` 分支源码。
- 阅读根目录 README、插件 README、三个插件清单、CLI 入口、Claude 插件解析器、Codex 转换器和 Codex 写入器。
- 确认源码清单版本为 `3.9.4`。
- 确认当前源码扫描到 38 个 skill 目录和 43 个 Agent Markdown 文件。
- 确认 CLI 已注册 `opencode`、`codex`、`pi`、`gemini`、`kiro` 五个转换目标。
- 阅读 GitHub Actions CI：安装依赖后执行 `bun run release:validate` 和 `bun test`。

未完成：

- 本机没有安装 `bun`，因此没有实际运行 `bun test` 和 `bun run release:validate`。
- 没有在真实 Claude Code、Codex、Cursor、OpenCode、Pi、Gemini 或 Kiro 环境安装插件。
- 没有验证 CE 与 Superpowers 同时安装时的触发优先级和交互体验。

## 常见注意事项

- Codex 只做 marketplace 注册还不够，完整体验需要原生安装 skills 和 Bun 转换 Agent 两部分。
- 自定义转换目标需要 Bun 环境。
- Pi 缺少原生 subagent primitive，需要额外安装 `pi-subagents`。
- 旧版 Bun 安装遗留物可以使用 `cleanup` 子命令备份和清理，例如：

```bash
bunx @every-env/compound-plugin cleanup --target codex
```

- 大量专项 Agent 不代表每次任务都应全部调用。`ce-code-review` 会根据任务风险和模式选择 reviewer。
