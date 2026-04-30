# mattpocock/skills 阅读笔记

## 已阅读材料

- GitHub 仓库页：<https://github.com/mattpocock/skills>
- `README.md`
- `LICENSE`
- `.claude-plugin/plugin.json`
- `CLAUDE.md`
- `CONTEXT.md`
- `docs/adr/0001-explicit-setup-pointer-only-for-hard-dependencies.md`
- `scripts/link-skills.sh`
- `skills/engineering/README.md`
- `skills/productivity/README.md`
- `skills/misc/README.md`
- 抽样阅读：
  - `skills/engineering/setup-matt-pocock-skills/SKILL.md`
  - `skills/engineering/grill-with-docs/SKILL.md`
  - `skills/engineering/tdd/SKILL.md`
  - `skills/engineering/diagnose/SKILL.md`
  - `skills/engineering/to-prd/SKILL.md`
  - `skills/engineering/to-issues/SKILL.md`
  - `skills/engineering/triage/SKILL.md`
  - `skills/engineering/improve-codebase-architecture/SKILL.md`
  - `skills/productivity/grill-me/SKILL.md`
  - `skills/productivity/caveman/SKILL.md`
  - `skills/productivity/write-a-skill/SKILL.md`
  - `skills/misc/git-guardrails-claude-code/SKILL.md`
  - `skills/misc/setup-pre-commit/SKILL.md`

## 仓库快照

- 默认分支：`main`
- 本地浅克隆提交：`f71bb97 Add out-of-scope note: issue trackers must be mainstream`
- GitHub 页面显示：60 commits、MIT license、无 releases
- 顶层目录：`.claude-plugin`、`.out-of-scope`、`docs/adr`、`scripts`、`skills`

## 关键发现

### 1. 这是工作流提示包，不是软件库

仓库没有传统应用入口，也没有 package manifest。README 的 Quickstart 依赖外部 `skills.sh` 安装器，仓库自身主要提供可被 agent 加载的 skill 目录。

### 2. 设计重点是缩小 agent 失败模式

README 把问题归纳为误解用户、表达冗长、缺少反馈环、架构退化。源码中各 skill 的流程也围绕这些失败模式设计，而不是围绕某个技术栈。

### 3. `setup-matt-pocock-skills` 是工程类 skill 的配置枢纽

它会把 issue tracker、triage label、domain docs 写成目标仓库内的 `docs/agents/*.md`。`to-prd`、`to-issues`、`triage` 等硬依赖这些配置。

### 4. `CONTEXT.md` / ADR 是贯穿设计

多个 skill 都要求读取领域 glossary 和 ADR。`grill-with-docs` 甚至会在追问过程中即时更新 `CONTEXT.md`，只在满足条件时建议写 ADR。

### 5. 项目有自洽的维护规范，但存在一处疑问

`CLAUDE.md` 要求 `engineering`、`productivity`、`misc` 里的 skill 都进入 `.claude-plugin/plugin.json`。实际 manifest 只列了 `engineering` 和 `productivity`，没有列 `misc`。这可能是遗漏，也可能是作者只想默认发布常用 skill。尚未验证。

## 风险与限制

- 没有正式 release，版本 pinning 可能依赖 Git commit 或外部 installer 行为。
- skill 是自然语言指令，效果依赖 agent 能否遵守流程。
- 对外部工具有隐含依赖，例如 GitHub 用 `gh` CLI、GitLab 用 `glab` CLI、前端调试可能用 Playwright/Puppeteer。
- `scripts/link-skills.sh` 会删除目标中同名且非软链接的目录，虽然目标是 `~/.claude/skills/<name>`，但执行前仍需确认不会覆盖个人已有 skill。
- `git-guardrails-claude-code`、`setup-pre-commit` 等 misc skill 涉及写配置和安装依赖，实际使用前应在目标仓库中审查。

## 可继续深入的问题

- 验证 `npx skills@latest add mattpocock/skills` 的实际安装产物和兼容 agent。
- 查看 `skills` npm 包或 `skills.sh` installer 的实现，确认 plugin manifest 如何被解析。
- 对每个 skill 做更细的“触发条件、输入、输出、依赖、失败模式”矩阵。
- 在一个测试仓库中跑 `/setup-matt-pocock-skills`，观察生成的 `docs/agents/*.md` 是否能被其他 skills 顺利消费。
- 提 issue 或进一步确认 `.claude-plugin/plugin.json` 未包含 misc skills 是否符合维护意图。

## 初步评价

事实层面：项目结构清晰，文档驱动，能被复制和改写；核心工程 skills 对反馈环、垂直切片、领域语言和 ADR 的重视是一致的。

个人判断：这个项目最大的价值不是单个 skill 的措辞，而是它把“先澄清、再验证、再实现、再沉淀语言/决策”的工程节奏植入 agent 工作方式。对习惯直接让 agent 写代码的用户，它会显著增加前期对齐成本，但也可能减少后期返工。
