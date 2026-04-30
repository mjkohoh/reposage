# mattpocock/skills 使用方式

## 安装方式

README 推荐使用 `skills.sh` 安装器：

```bash
npx skills@latest add mattpocock/skills
```

安装时选择需要的 skills 和目标 coding agents。README 特别提示需要选择 `/setup-matt-pocock-skills`，之后在 agent 中运行：

```text
/setup-matt-pocock-skills
```

该 setup skill 会询问三类配置：

- issue tracker：GitHub、GitLab、local markdown 或其他。
- triage label：`needs-triage`、`needs-info`、`ready-for-agent`、`ready-for-human`、`wontfix` 的实际标签字符串。
- domain docs：项目使用单一 `CONTEXT.md` + `docs/adr/`，还是通过 `CONTEXT-MAP.md` 管理多个上下文。

## 本地 Claude Code 软链接方式

仓库还提供了 `scripts/link-skills.sh`。它会把仓库内所有 `SKILL.md` 所在目录链接到：

```text
~/.claude/skills
```

脚本逻辑来源：`scripts/link-skills.sh`

关键行为：

- 计算仓库根目录和目标目录。
- 如果 `~/.claude/skills` 是指向当前仓库的软链接，会拒绝继续，避免把 skill 链接写回仓库自身。
- 遍历 `skills/**/SKILL.md`。
- 以 skill 目录名作为目标名，创建软链接。
- 如果目标已存在且不是软链接，会先删除再创建。

命令：

```bash
./scripts/link-skills.sh
```

注意：这个脚本会写入用户 home 目录下的 Claude 配置目录。本文档未在当前机器上执行该脚本。

## 推荐初始化流程

1. 通过 `npx skills@latest add mattpocock/skills` 安装。
2. 选择 `setup-matt-pocock-skills` 和需要的工程/生产力 skills。
3. 在目标项目中运行 `/setup-matt-pocock-skills`。
4. 根据项目实际情况回答 issue tracker、triage labels、domain docs 三组问题。
5. 后续再运行 `/diagnose`、`/tdd`、`/to-prd`、`/to-issues`、`/triage` 等 skills。

## 生成的目标仓库配置

`setup-matt-pocock-skills/SKILL.md` 说明，它会编辑目标仓库中已有的 `CLAUDE.md` 或 `AGENTS.md`：

- 如果 `CLAUDE.md` 存在，编辑它。
- 否则如果 `AGENTS.md` 存在，编辑它。
- 如果两者都不存在，询问用户创建哪个。
- 如果已有 `## Agent skills` 区块，原地更新，避免重复追加。

它还会写入：

```text
docs/agents/issue-tracker.md
docs/agents/triage-labels.md
docs/agents/domain.md
```

这些文件由同目录模板生成：

- `skills/engineering/setup-matt-pocock-skills/issue-tracker-github.md`
- `skills/engineering/setup-matt-pocock-skills/issue-tracker-gitlab.md`
- `skills/engineering/setup-matt-pocock-skills/issue-tracker-local.md`
- `skills/engineering/setup-matt-pocock-skills/triage-labels.md`
- `skills/engineering/setup-matt-pocock-skills/domain.md`

## 常用 skill 调用意图

### `/grill-with-docs`

适合在正式编码前澄清计划。它会：

- 阅读代码中能回答的问题，而不是把所有问题都问用户。
- 用 `CONTEXT.md` 检查术语冲突。
- 对模糊概念提出更精确的领域词。
- 只在决策难以逆转、没有上下文会令人意外、且存在真实取舍时建议写 ADR。

### `/diagnose`

适合处理难复现 bug 或性能回退。它的核心要求是先建立 agent 可运行的 pass/fail 反馈环，候选方式包括失败测试、HTTP 脚本、CLI fixture、Playwright/Puppeteer、trace replay、throwaway harness、fuzz loop、bisect harness、differential loop 等。

### `/tdd`

适合以测试驱动做功能或修 bug。它明确反对“先写完所有测试，再写完所有实现”的水平切片，要求一条行为一个测试、一小步实现、一轮反馈。

### `/to-prd` 与 `/to-issues`

`to-prd` 根据已有对话上下文生成 PRD 并发布到 issue tracker，不再采访用户。`to-issues` 把计划或 PRD 拆成可独立领取的垂直切片 issue，并区分 `HITL` 和 `AFK`。

### `/triage`

适合整理 issue。它把 issue 分成 category role 和 state role，并要求每条由 AI 发布到 issue tracker 的 triage 评论都以免责声明开头。

## 运行验证

本次调研未执行 `npx skills@latest add mattpocock/skills`，原因是该命令会访问 npm 并可能写入本机 agent 配置；当前目标是项目研究而不是安装。

已验证内容：

- 成功浅克隆仓库到 `/private/tmp/reposage-skills`。
- 阅读了 `README.md`、`LICENSE`、`.claude-plugin/plugin.json`、`CLAUDE.md`、`CONTEXT.md`、`docs/adr/0001-explicit-setup-pointer-only-for-hard-dependencies.md`。
- 抽样阅读了工程、生产力、杂项多个 `SKILL.md`。

尚未验证：

- `npx skills@latest add mattpocock/skills` 的交互安装体验。
- 不同 agent 对 `.claude-plugin/plugin.json` 的实际加载兼容性。
- GitHub/GitLab/local markdown issue 发布流程是否端到端可用。
