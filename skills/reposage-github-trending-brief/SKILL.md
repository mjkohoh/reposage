---
name: reposage-github-trending-brief
description: 生成或更新 GitHub Trending 的中文趋势简报，支持日、周、月三个周期，不克隆项目源码。当用户询问今天、本周、本月、daily、weekly、monthly、latest GitHub trending、GitHub 趋势、趋势简报或类似内容时，一定使用这个技能。
---

# Reposage GitHub 趋势简报

使用这个技能基于 GitHub Trending 生成轻量中文简报。这不是深度项目调研：只浏览趋势页和项目主页，不克隆仓库，不安装依赖，不运行项目代码。

## 触发场景

当用户提出以下请求时使用：

- `告诉我本周 GitHub 趋势`
- `今天 github trending 有什么`
- `生成本月 GitHub 趋势简报`
- `看看 GitHub trending`
- `weekly trending report`

如果用户之后要求深入研究某个趋势项目，切换到 `reposage-project-research`。

## 确认周期

把用户表达映射到 GitHub Trending 周期：

- `今天` / `今日` / `daily` / `日趋势` -> daily
- `本周` / `weekly` / `周趋势` -> weekly
- `本月` / `monthly` / `月趋势` -> monthly

如果用户没有说明周期且无法从上下文推断，简短询问一次。如果上下文已经足够明确，可以直接选择周期并说明假设。

## 获取最新数据

必须浏览 GitHub Trending 获取当前数据：

```text
https://github.com/trending
https://github.com/trending?since=daily
https://github.com/trending?since=weekly
https://github.com/trending?since=monthly
```

对入选项目打开 GitHub 项目主页做轻量了解：

- GitHub description。
- 主要语言或可见技术栈。
- stars/forks，如页面可见。
- README 开头或项目 tagline。
- Trending 页面显示的趋势信号，例如本周期新增 stars。

不要克隆仓库。不要运行代码。不要夸大能力。

## 报告路径

报告存放在：

```text
docs/trending/github/
```

文件名：

- 日报：`daily-YYYY-MM-DD.md`
- 周报：`weekly-YYYY-Www.md`，使用 ISO week，例如 `weekly-2026-W18.md`
- 月报：`monthly-YYYY-MM.md`

使用环境中的当前日期和时间。用户使用相对日期时，在报告中写清楚绝对日期。

## 避免重复报告

写入前先检查同周期报告文件是否已经存在。

如果不存在：

- 创建新报告。
- 标记为初版。

如果已存在：

- 重新获取最新 Trending 数据。
- 与已有报告对比。
- 更新同一个文件，不创建重复报告。
- 新增或更新“更新记录”小节，记录：
  - 更新时间
  - 新增项目
  - 消失项目
  - 排名变化
  - 重要元数据或描述变化

除非用户明确要求保留多个快照，否则不要为同一周期创建多个文件。

## 报告结构

使用这个结构：

```markdown
# GitHub Trending 简报（<周期中文>）

## 摘要

- 查询时间：
- 周期：
- 数据源：
- 本次结论：

## 趋势项目

### 1. owner/repo

- 链接：
- 主要语言/技术栈：
- GitHub 简介：
- 趋势信号：
- 适用场景：
- 初步关注理由：
- 不确定性：

## 观察

## 更新记录
```

默认选择 Top 10 项目，除非用户要求更多或更少。

## 写作要求

使用中文。简报要适合快速扫读和决策，帮助用户判断哪些项目值得后续深度调研。

每个项目：

- 摘要保持短。
- 用具体定位替代宣传话术。
- 只读主页时使用“基于主页推断”。
- 未本地验证的能力标注“尚未验证”。

## 更新 README

如果这是项目中第一次加入趋势简报能力或目录，更新根目录 `README.md`，提到：

```text
docs/trending/github/
```

不要在 README 中列出每一份趋势报告，除非用户明确要求。

## 最终回复

最终回复包含：

- 报告文件路径。
- 本次是创建还是更新。
- 最高信号发现。
- 如果是更新，列出与上一版的差异。

保持简洁；完整简报放在 Markdown 文件中。
