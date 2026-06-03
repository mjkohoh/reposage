# humanlayer/12-factor-agents 使用方式

## 最简单的使用方式：阅读原则指南

对于多数读者，本项目不需要安装。建议按以下路径阅读：

1. 先阅读根目录 `README.md`，确认作者的问题定义和 12 项原则总览。
2. 如果关注 prompt/context 设计，优先阅读 `content/factor-02-own-your-prompts.md` 与 `content/factor-03-own-your-context-window.md`。
3. 如果关注审批、异步工作流与可靠执行，阅读 `factor-05` 到 `factor-09`。
4. 如果关注产品边界和入口集成，阅读 `factor-10` 到 `factor-12`。

## 运行教学模板

仓库在 `packages/create-12-factor-agent/template/` 提供了逐章推进的 TypeScript 教学模板。README 显示其目标是从 Hello World 开始，逐步加入 agent loop、calculator tools、BAML 测试、人类澄清/审批、上下文格式与 HTTP 服务。

### 运行环境

- Node.js，教学 README 示例推荐 Node.js 20。
- `npm`；根目录 `Makefile` 也会依次尝试 `npm install`、`bun install` 或 `yarn install`。
- BAML 工具链。
- 运行带模型调用的步骤需要模型 provider 的凭据。
- 运行涉及人类通知/审批的 server 示例预计需要 HumanLayer 相关配置。

### 模板依赖

当前模板 `package.json` 可见的核心依赖包括：

```json
{
  "@boundaryml/baml": "latest",
  "express": "^5.1.0",
  "humanlayer": "^0.7.7",
  "tsx": "^4.15.0",
  "typescript": "^5.0.0",
  "zod": "^3.25.64"
}
```

### 教学文档给出的基础命令

以下命令来自模板 README，尚未在本次调研中实际执行：

```bash
node --version
npm install
npx baml-cli generate
npx tsx src/index.ts hello
npx baml-cli test
```

模板 `package.json` 另外声明：

```bash
npm run dev
npm run build
```

## 配置方式

### 模型提供方

这里存在一个值得注意的文档/模板差异：

- 教学 README 的中间章节说明示例默认以 `BASEten_API_KEY` 与 `BASEten_BASE_URL` 使用 Qwen3。
- 当前最终模板的 `baml_src/agent.baml` 则直接声明 `client "openai/gpt-4o"`。

因此，在真实试跑前应先选择目标 provider，并同步修改 BAML client 与环境变量；不要假设 README 中某一章节的配置就是最终模板当前默认值。

### 人类审批与回调

`src/server.ts` 展示的流程是：

- HTTP 请求创建或恢复一次 conversation/thread。
- Agent 产生 `request_more_information` 或高风险动作时，服务通过 HumanLayer SDK 请求人工交互或批准。
- 后续 callback 带回状态标识，加载线程并继续执行。

这是一份设计示例，不等于已经具备生产所需的认证、签名校验、持久化、权限和审计配置。

## 已尝试的验证

- 已阅读官方仓库主页、README、LICENSE、Makefile。
- 已阅读 12 项原则中的核心文档：`factor-01` 到 `factor-12` 的关键内容，重点核验 context、state、control flow、human-in-the-loop 与 scope 原则。
- 已静态阅读模板 `package.json`、`README.md`、`baml_src/agent.baml`、`src/agent.ts`、`src/state.ts`、`src/server.ts`。
- 未执行依赖安装、模型调用或本地服务运行。

## 未运行验证的原因

- 用户当前请求是项目介绍与调研，验证核心理念不要求下载并运行模板。
- 示例运行会引入外部依赖安装和模型/HumanLayer 凭据需求。
- 模板静态阅读已发现需要先核验的实现疑点，贸然运行不是本次最小必要步骤。

## 常见失败原因与注意项

- 未生成 BAML client 就运行 TypeScript 示例，可能导致导入缺失。
- 未配置对应模型 provider 的 API key 或配置与 BAML client 不一致。
- 只复制概念代码而未实现持久化、回调验证、审批权限与错误上限。
- 直接复制仓库的 `CLAUDE.md`：该文件是一个泛化 persona 指令模板，并非运行示例所必需的配置，且可能改变本地 agent 的行为约束。
- 静态疑点：当前 `src/server.ts` 中对返回 Promise 的 `store.get(...)` 与 `store.create(...)` 调用未显式 `await`；在将模板用于实际开发前，应先执行 TypeScript 构建并确认该路径可用。

## 来源

- https://github.com/humanlayer/12-factor-agents/blob/main/README.md
- https://github.com/humanlayer/12-factor-agents/blob/main/Makefile
- https://github.com/humanlayer/12-factor-agents/tree/main/packages/create-12-factor-agent/template
- https://github.com/humanlayer/12-factor-agents/blob/main/packages/create-12-factor-agent/template/README.md
- https://github.com/humanlayer/12-factor-agents/blob/main/packages/create-12-factor-agent/template/package.json
