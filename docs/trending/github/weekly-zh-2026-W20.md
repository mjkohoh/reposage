# GitHub Trending 简报（2026 年第 20 周，中文）

## 摘要

- 查询时间：2026-05-15 15:41:04 CST
- 周期：weekly / This week
- 自然语言筛选：中文 (`zh`)
- 数据源：https://github.com/trending?since=weekly&spoken_language_code=zh
- 本次结论：本周中文筛选下的 GitHub Trending Top 10 呈现出很强的“中文用户长期实用项目”特征：游戏自动化、代理/规则配置、技术周刊、桌面应用、股票数据、IM/Agent 框架、建站工具和算法教程同时上榜。`521xueweihan/HelloGitHub` 虽在总榜排名第 11，但以 1,514 stars this week 的趋势信号明显高于 Top 10 多数项目，值得单独关注。以下结论主要基于 GitHub Trending 页面与项目主页，尚未本地运行验证。

## 筛选条件

- 时间范围：This week / weekly
- 自然语言：中文 (`zh`)
- 说明：自然语言筛选来自 GitHub Trending 官方 `spoken_language_code` 参数，可能包含多语言项目；它不等同于开发语言、项目作者所在地或项目唯一面向人群。

## 趋势项目

### 1. MaaAssistantArknights/MaaAssistantArknights

- 链接：https://github.com/MaaAssistantArknights/MaaAssistantArknights
- 主要语言/技术栈：C++；项目 README 还提到支持 C、Python、Java、Rust、Golang、Java HTTP、Rust HTTP 等接口。
- GitHub 简介：《明日方舟》小助手，全日常一键长草；项目主页说明其基于图像识别技术，一键完成日常任务。
- 趋势信号：20,817 stars / 2,597 forks；227 stars this week。
- 适用场景：明日方舟玩家自动化日常任务、刷图、公开招募识别、材料识别和作业执行。
- 初步关注理由：中文社区成熟度高，文档、官网、多语言支持和外服适配信息都较完整，属于垂直游戏自动化里的高活跃项目。
- 不确定性：未本地运行；游戏自动化工具可能受游戏规则、客户端版本和平台策略影响，使用前需自行确认合规边界。

### 2. Johnshall/Shadowrocket-ADBlock-Rules-Forever

- 链接：https://github.com/Johnshall/Shadowrocket-ADBlock-Rules-Forever
- 主要语言/技术栈：Trending 页面未显示主要语言；核心是 Shadowrocket 规则与自动构建产物。
- GitHub 简介：提供多款 Shadowrocket 规则，拥有广告过滤功能，每日 8 时重新构建规则。
- 趋势信号：25,455 stars / 1,696 forks；278 stars this week。
- 适用场景：Shadowrocket 用户订阅广告过滤、去广告和规则分流配置。
- 初步关注理由：规则类仓库 stars 与周新增都较高，说明中文用户对移动端代理规则和广告过滤仍有稳定需求。
- 不确定性：未验证规则质量、误杀率和更新稳定性；代理/分流配置涉及地区、网络和服务条款差异。

### 3. ruanyf/weekly

- 链接：https://github.com/ruanyf/weekly
- 主要语言/技术栈：内容型仓库，GitHub Trending 未显示主要语言。
- GitHub 简介：科技爱好者周刊，每周五发布。
- 趋势信号：90,812 stars / 4,085 forks；656 stars this week。
- 适用场景：跟踪中文技术社区新闻、工具、文章、观点和独立开发者项目。
- 初步关注理由：这是中文技术内容分发的长期高影响力项目，周新增 stars 很高，适合作为趋势发现的外部信号源。
- 不确定性：它不是软件库或工具包，价值主要在内容策展；需要结合每期内容判断具体项目价值。

### 4. LorisYounger/VPet

- 链接：https://github.com/LorisYounger/VPet
- 主要语言/技术栈：C#；WPF 相关桌面应用。
- GitHub 简介：虚拟桌宠模拟器，一个开源桌宠软件，可以内置到任何 WPF 应用程序。
- 趋势信号：6,102 stars / 607 forks；127 stars this week。
- 适用场景：Windows 桌面宠物、互动角色、桌面小组件，以及 WPF 应用的内置互动组件。
- 初步关注理由：中文桌面应用项目中较少见的高星互动软件，兼具娱乐属性和 WPF 集成价值。
- 不确定性：未验证运行体验、资源占用、插件生态和跨平台可用性。

### 5. mootdx/mootdx

- 链接：https://github.com/mootdx/mootdx
- 主要语言/技术栈：Python 96.1%，少量 JavaScript。
- GitHub 简介：通达信数据读取的一个简便使用封装；README 显示支持通达信离线数据、线上行情和财务数据读取。
- 趋势信号：1,666 stars / 501 forks；54 stars this week。
- 适用场景：A 股量化研究、行情读取、通达信本地数据解析、财务数据抓取学习。
- 初步关注理由：面向中文金融数据生态的实用型 Python 包，README 给出安装和代码示例，入门成本较低。
- 不确定性：项目 README 声明仅作学习交流、不得用于商业目的；数据源稳定性和合规边界需进一步核验。

### 6. xiaojieonly/Ehviewer_CN_SXJ

- 链接：https://github.com/xiaojieonly/Ehviewer_CN_SXJ
- 主要语言/技术栈：C；Topics 包含 Android、Java、HTML5、APK、EhViewer 等。
- GitHub 简介：ehviewer，用爱发电，快乐前行。
- 趋势信号：23,940 stars / 624 forks；158 stars this week。
- 适用场景：Android 客户端相关阅读器/浏览器项目，偏特定内容社区用户。
- 初步关注理由：长期高星且中文筛选下持续有热度，说明其用户群稳定。
- 不确定性：项目用途涉及特定内容平台和成人向标签；合规、内容风险和分发方式需要用户自行评估。

### 7. AstrBotDevs/AstrBot

- 链接：https://github.com/AstrBotDevs/AstrBot
- 主要语言/技术栈：Python。
- GitHub 简介：AI Agent Assistant 与开发框架，集成多个 IM 平台、LLM、插件和 AI 功能，可作为 OpenClaw alternative。
- 趋势信号：31,894 stars / 2,200 forks；608 stars this week。
- 适用场景：QQ、Telegram、Discord 等 IM 场景的机器人/Agent 助手，LLM 插件化应用开发。
- 初步关注理由：中文 IM + Agent 框架方向热度很高，周新增 stars 位居 Top 10 前列；主题覆盖 agent、mcp、chatbot、openai、gemini、llama 等。
- 不确定性：未验证多平台接入质量、插件安全模型、生产部署复杂度和与 OpenClaw 的真实差异。

### 8. halo-dev/halo

- 链接：https://github.com/halo-dev/halo
- 主要语言/技术栈：Java。
- GitHub 简介：开源建站工具，覆盖个人博客、知识库、企业官网和在线商城等场景。
- 趋势信号：38,559 stars / 10,257 forks；133 stars this week。
- 适用场景：中文用户自建博客、知识库、官网、内容站和轻量商业站点。
- 初步关注理由：成熟度和 forks 都很高，说明它不只是新晋热点，更像中文开源建站生态里的基础设施型项目。
- 不确定性：未验证插件生态、升级成本、主题质量和商城等扩展能力的生产成熟度。

### 9. krahets/hello-algo

- 链接：https://github.com/krahets/hello-algo
- 主要语言/技术栈：多语言代码实现，GitHub 语言占比包含 Java、Swift、C++、JavaScript、Go、Python 等。
- GitHub 简介：《Hello 算法》：动画图解、一键运行的数据结构与算法教程，支持简中、繁中、English、日本語等语言。
- 趋势信号：126,130 stars / 15,122 forks；327 stars this week。
- 适用场景：数据结构与算法入门、编程语言对照学习、算法教学材料。
- 初步关注理由：教育属性强，支持多语言代码和多自然语言版本，是中文开源学习资源中非常成熟的项目。
- 不确定性：内容质量需要按具体章节评估；它更适合学习与教学，不是可直接集成的软件组件。

### 10. gaotianliuyun/gao

- 链接：https://github.com/gaotianliuyun/gao
- 主要语言/技术栈：JavaScript。
- GitHub 简介：FongMi 影视和 tvbox 配置文件，README/Trending 提醒使用前仔细阅读仓库说明。
- 趋势信号：7,338 stars / 2,570 forks；23 stars this week。
- 适用场景：特定影视/TVBox 配置文件自用和研究。
- 初步关注理由：配置类仓库 forks 较多，说明用户复制自用需求明显。
- 不确定性：涉及影视源和配置分发，存在版权、合规和可用性风险；不建议在未理解规则来源和法律边界前使用。

## 观察

- 中文筛选榜与全量 Trending 的气质差异明显：这里更偏向中文用户长期使用的“实用资产”，例如规则、周刊、建站、学习资源、游戏/桌面工具，而不是只围绕最新 AI 基础设施。
- AI 相关仍然存在，但不是唯一主线。`AstrBotDevs/AstrBot` 是 Top 10 中最突出的 IM/Agent 框架；`jeecgboot/JeecgBoot` 和 `zhayujie/CowAgent` 虽未进 Top 10，但在中文筛选下也有明显热度。
- 教育和内容策展项目仍然能获得持续新增关注：`ruanyf/weekly`、`krahets/hello-algo`、`521xueweihan/HelloGitHub` 都是成熟中文内容/学习型项目，说明 Trending 中文筛选不是只反映“新项目爆发”，也反映长期项目的持续传播。
- 风险较高的项目类型需要单独标注：代理规则、影视配置、特定内容客户端、游戏自动化都可能有合规、版权、服务条款或平台政策问题，后续深度调研时应优先核验边界。

## 关注但未进入 Top 10

- `521xueweihan/HelloGitHub`：156,227 stars / 11,914 forks；1,514 stars this week。虽然位于本次页面第 11，但周新增 stars 远高于 Top 10 大多数项目，是中文开源项目发现和入门推荐的重要入口。
- `jeecgboot/JeecgBoot`：AI 低代码平台，110 stars this week。适合后续关注“中文企业级低代码 + AI 应用”方向。
- `zhayujie/CowAgent`：中文多平台 AI 助理/数字员工项目，338 stars this week。和 AstrBot 同属中文 IM + Agent 生态，可作为对比调研对象。

## 更新记录

- 2026-05-15 15:41 CST：创建 2026 年第 20 周中文 GitHub Trending 简报初版，覆盖 `spoken_language_code=zh` 的 Top 10，并补充 3 个高关注未入 Top 10 项目。
