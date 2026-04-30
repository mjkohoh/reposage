---
name: reposage-project-research
description: 根据开源项目仓库地址做系统调研，并生成可复用的中文文档。当用户提供 GitHub、GitLab 或其他源码仓库地址，并要求研究、分析、理解、调查、总结、评估或文档化项目时，一定使用这个技能；即使用户只是说“研究一下这个项目”也应触发。
---

# Reposage 项目调研

使用这个技能把一个开源仓库沉淀成可持续维护的中文研究档案。这是深度调研流程：阅读文档和源码，必要时浅克隆仓库，合理验证使用方式，并把结论写入 `docs/projects/`。

## 触发场景

当用户给出仓库 URL 或 `owner/repo`，并要求项目调研时使用，例如：

- `研究一下 https://github.com/example/project`
- `帮我看看这个开源项目是干什么的`
- `分析一下 owner/repo`
- `这个项目值得用吗`

不要用这个技能处理 GitHub Trending 汇总请求；趋势简报使用 `reposage-github-trending-brief`。

## 语言

默认使用中文沟通，并用中文编写调研文档。代码、命令、配置键、项目名、专有名词和必要引用可以保留原文。

## 工作流程

### 1. 确认仓库身份

识别并记录：

- 来源平台和仓库地址。
- owner 与 repo 名。
- 默认分支。
- 许可证。
- stars、forks、releases、最新提交等可能变化的公开元数据。

仓库元数据会随时间变化；当这些信息影响结论时，需要浏览或用其他方式确认最新状态。

### 2. 创建稳定项目 ID

文档目录使用：

```text
docs/projects/<host>-<owner>-<repo>/
```

规范化规则：

- 全部转为小写。
- 非字母数字字符折叠为短横线。
- 去掉首尾短横线。

示例：

- `https://github.com/mattpocock/skills` -> `docs/projects/github-mattpocock-skills/`
- `https://github.com/zilliztech/claude-context` -> `docs/projects/github-zilliztech-claude-context/`

不要使用裸仓库名作为目录名，例如 `skills`、`docs`、`app`、`server`、`cli`、`core`，除非用户明确要求。

### 3. 高效阅读项目

优先阅读：

- `README`
- 官方文档
- examples
- release notes / changelog
- package、build、config 文件
- license
- contribution 文档
- 入口文件和核心模块
- 能揭示预期行为的测试

本地源码导航优先使用 `rg` 和 `rg --files`。不要无目的通读所有源码；先建立入口、核心模块、适配层、测试、示例和构建脚本索引。

### 4. 合理验证使用方式

在低风险且符合任务目标时，尝试验证安装或基础运行。记录：

- 尝试过的命令。
- 必要运行时和依赖。
- 必要环境变量和配置。
- 结果或阻塞原因。

不要把整个会话耗在安装排障上。如果因为凭据、网络、重依赖、本地服务、平台限制或潜在破坏性副作用无法运行，应停止并记录阻塞。

### 5. 区分事实和判断

重要结论尽量绑定来源：

- 文件路径
- 命令结果
- 官方链接
- 公开页面

清楚标注：

- `事实`
- `推断`
- `个人判断`
- `尚未验证`

不要把 README 的宣传语直接当作已验证能力。

## 必需文档

写入以下文件：

```text
docs/projects/<project-id>/overview.md
docs/projects/<project-id>/usage.md
docs/projects/<project-id>/architecture.md
docs/projects/<project-id>/notes.md
```

### `overview.md`

包含：

- 高信号总结。
- 项目定位。
- 目标用户。
- 核心能力。
- 适用场景。
- 不适用边界。
- 技术栈。
- 初步评价。
- 仓库来源、默认分支、许可证和调研快照。

### `usage.md`

包含：

- 安装和设置。
- 配置方式。
- 常用命令。
- 运行环境要求。
- 已尝试的验证。
- 未验证内容及原因。
- 常见失败原因。

### `architecture.md`

包含：

- 源码结构概览。
- 入口文件。
- 核心模块。
- 数据流或控制流。
- 从代码/文档可见的设计决策。
- 构建、测试、发布流程。
- 架构风险或取舍。

流程复杂时可以使用 Mermaid 图。

### `notes.md`

包含：

- 已阅读材料。
- 调研快照。
- 关键发现。
- 未解问题。
- 风险和限制。
- 后续研究方向。
- 个人评价。

## 更新 README

创建或更新项目调研档案后，更新根目录 `README.md` 的“已研究项目”：

```markdown
- [owner/repo](./docs/projects/<project-id>/overview.md)：一句话中文定位。
```

不要删除已有项目条目。

## 最终回复

最终回复包含：

- 一段高信号结论。
- 已创建或更新的文件链接。
- 未完成的验证。
- 重要不确定性。

保持简洁；详细调研内容放在文档文件里。
