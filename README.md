# reposage

`reposage` 是一个用于研究开源项目和跟踪 GitHub Trending 的文档工作台。它的目标是：当用户给出一个项目地址时，根据问题深浅判断这个项目有什么用、怎么用、解决了什么问题；当用户询问 GitHub 趋势时，生成可持续更新的轻量简报。

## 这个项目做什么

- 研究开源项目的定位、功能、技术栈和适用场景。
- 按需梳理安装、配置、运行和基础使用方式。
- 在需要深度研究时，阅读关键源码，解释核心模块、入口、调用链路和设计思路。
- 在用户明确要求时，记录调研结论、验证步骤、风险点和后续问题。
- 为值得沉淀的项目建立可持续维护的中文研究档案。
- 跟踪 GitHub Trending 日/周/月趋势，并可按自然语言筛选，生成轻量中文简报，帮助筛选值得深入研究的项目。

## 适合解决的问题

- 想快速判断一个开源项目是否值得采用。
- 想理解一个项目的使用方式和工程结构。
- 想对比项目能力、依赖、限制和维护状态。
- 想把零散的项目阅读过程整理成团队可复用资料。
- 想定期了解 GitHub Trending 上的新项目，但暂时不需要下载源码深度研究。

## 内置 Skills

- [reposage-project-research](./skills/reposage-project-research/SKILL.md)：给定开源项目地址，按意图分为快速回答、轻量简报或深度档案。
- [reposage-github-trending-brief](./skills/reposage-github-trending-brief/SKILL.md)：按日/周/月获取 GitHub Trending，可选按自然语言筛选，生成或更新趋势简报。

## 推荐文档目录

```text
.
├── AGENTS.md
├── README.md
├── skills/
│   ├── reposage-project-research/
│   │   └── SKILL.md
│   └── reposage-github-trending-brief/
│       └── SKILL.md
└── docs/
    ├── projects/
    │   ├── index.md
    │   └── <host>-<owner>-<repo>/
    │       ├── brief.md
    │       ├── overview.md
    │       ├── usage.md
    │       ├── architecture.md
    │       └── notes.md
    └── trending/
        └── github/
            ├── daily-YYYY-MM-DD.md
            ├── weekly-YYYY-Www.md
            ├── weekly-zh-YYYY-Www.md
            └── monthly-YYYY-MM.md
```

## 文档说明

- [AGENTS.md](./AGENTS.md)：定义 `reposage` 助手的角色、工作流程、输出要求和工程约定。
- `skills/`：项目内工作流技能，分别覆盖项目调研和 GitHub 趋势简报。
- [docs/projects/index.md](./docs/projects/index.md)：已沉淀项目的统一索引。
- `docs/projects/<host>-<owner>-<repo>/brief.md`：轻量中文简报，帮助判断是否值得继续研究或采用。
- `docs/projects/<host>-<owner>-<repo>/overview.md`：项目简介、核心价值、功能边界和适用场景。
- `docs/projects/<host>-<owner>-<repo>/usage.md`：安装配置、运行方式、常用命令和验证结果。
- `docs/projects/<host>-<owner>-<repo>/architecture.md`：源码结构、核心模块、关键流程和设计分析。
- `docs/projects/<host>-<owner>-<repo>/notes.md`：调研笔记、疑问、风险和后续研究方向。
- `docs/trending/github/`：GitHub Trending 日/周/月趋势简报，可按自然语言生成独立报告。

## 已研究项目

已研究项目统一维护在 [docs/projects/index.md](./docs/projects/index.md)。

## 使用方式

向 `reposage` 提供一个开源项目地址，例如：

```text
请研究 https://github.com/example/project
```

也可以请求趋势简报，例如：

```text
告诉我本周 GitHub 趋势
拉取本周中文 GitHub 趋势
```

`reposage` 会根据对应 skill 执行：项目调研默认先做轻量判断，只有用户明确要求记录、简报或深度档案时才写入文档；趋势简报只做主页级轻量观察，不下载源码，并会复用同一周期的既有报告进行更新。
