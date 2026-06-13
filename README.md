# NBA Draft 2026 First-Round Prediction

本项目用于参加 CSDN BuilderX 黑客松赛题：2026 NBA 第一轮选秀预测。

赛题链接：https://builderx.csdn.net/draftcode

## 队伍信息

| 项目 | 内容 |
| --- | --- |
| 队伍 | Winds · Dennis · Marcos |
| 比赛 | 上海亚马逊云科技峰会黑客松 |
| 时间 | 2026 年 6 月 23 日 09:00 - 6 月 24 日 12:00 |
| 任务 | 用 AI 工具预测 2026 NBA 选秀第一轮顺位 |

## 项目目标

2026 NBA 选秀第一轮将于北京时间 2026 年 6 月 24 日 08:00 开始。本项目目标是在选秀开始前，基于历史选秀数据、候选球员资料、球队首轮顺位和球队需求，预测 30 支球队的完整第一轮选秀结果。

预测对象包括：

- 30 支球队的第一轮选秀顺位
- 每个顺位对应的预测球员
- 可能影响结果的关键事件，例如交易、球队需求变化、伤病、试训表现、媒体消息等

## 黑客松要求

根据赛题页面截图，比赛要求包括：

- 使用 26 年完整 NBA 选秀历史数据构建预测模型
- 预测 30 支球队的完整首轮 NBA 选秀结果
- 直播开始后，系统会逐一比对预测清单与 NBA 官方结果
- 排行榜会随官方选秀结果实时更新
- 每队 1-3 人
- 建议分工包括数据工程、模型建构和可视化

提交内容包括：

- Web Agent 界面：可访问、可交互，并能输出可视化预测结果
- GitHub 代码仓库：供 AI 评分工具拉取代码并评分
- 预测结果表单：30 支球队完整 NBA 选秀名单和里程碑事件答案

提交截止时间为北京时间 2026 年 6 月 24 日 08:00 前。

## 评分维度

赛题页面给出的评分结构为：

| 维度 | 权重 | 说明 |
| --- | ---: | --- |
| 代码出手质量 | 30% | 自动化评分工具从 GitHub 仓库拉取代码，按预设规则评分 |
| 主场路演 | 30% | 每队 6 分钟上台，演示 Web Agent 界面并讲解分析逻辑 |
| NBA 选秀命中率 | 40% | 预测 30 支球队首轮顺位结果，以及若干里程碑事件 |

最终评分细则预计在 2026 年 6 月 16 日培训直播公布。

## 当前数据

当前仓库已整理以下基础数据：

| 文件 | 说明 |
| --- | --- |
| `Data/nba_2026_draft_first_round_order.md` | 2026 NBA 选秀第一轮 1-30 顺位球队顺序 |
| `Data/nba_2026_draft_prospects.csv` | 2026 NBA Draft Prospects 候选新秀基础信息 |
| `Data/Data_2025/nba_2025_draft_prospects.csv` | 2025 NBA Draft Prospects 候选新秀基础信息 |
| `Data/Data_2025/nba_2025_draft_first_round_order.csv` | 2025 NBA 选秀第一轮实际顺位、选中球员和交易后归属 |
| `Data/Data_2025/nba_2025_draft_results.csv` | 2025 NBA 选秀最终结果，包含 1-59 顺位、两轮结果和交易后归属 |

候选新秀 CSV 字段包括：

- `name`：英文名
- `chinese_name`：中文名
- `position`：位置
- `age`：年龄
- `school_club`：学校或俱乐部
- `height`：官方页面身高
- `height_without_shoes`：裸足身高
- `wingspan`：臂展
- `standing_reach`：站立摸高
- `weight`：体重
- `status`：参选状态
- `country`：国家或地区

## 数据来源

| 数据 | 来源 |
| --- | --- |
| 2026 候选新秀 | NBA 官方 Draft Prospects：https://www.nba.com/draft/2026/prospects |
| 2026 第一轮球队顺序 | NBA 官方 Draft Order：https://www.nba.com/news/2026-nba-draft-order |
| 2025 候选新秀 | NBA 官方 Draft Prospects：https://www.nba.com/draft/2025/prospects |
| 2025 最终选秀结果 | NBA 官方 Draft Results, Picks 1-59：https://www.nba.com/news/2025-nba-draft-order |
| 预测市场信号 | Kalshi Basketball / Kalshi Hoops：https://kalshi.com/category/sports/basketball |

说明：2025 年尼克斯的一个二轮签被 NBA 罚没，所以官方最终结果为 59 个顺位，不是 60 个。

## 技术思路

本项目可以按以下方向推进：

1. 收集历史选秀数据，包括球员基本信息、大学或职业联赛表现、最终选秀顺位和选中球队。
2. 收集球队侧数据，包括球队战绩、阵容结构、位置需求、合同情况和历史选秀偏好。
3. 为候选球员构建特征，包括年龄、身高、体重、位置、联赛背景、基础数据和球探评价。
4. 建立预测模型，将球员天赋排序、球队需求、选秀权顺位和交易可能性结合起来。
5. 输出第一轮 30 个顺位的预测结果，并通过 Web Agent 展示预测逻辑、情报处理流程和可视化结果。

## 为什么采用三层架构

NBA 选秀预测同时包含三类不同问题：稳定的结构化信息、快速变化的外部情报，以及按顺位互相影响的球队决策。如果把三类问题混在一个模型里，预测结果会难以解释，也很难回测和排查错误。因此本项目采用三层架构，把数据、情报和最终模拟分开处理。

| 层级 | 解决的问题 | 主要输入 | 主要输出 |
| --- | --- | --- | --- |
| 第一层：基础信息层 | 建立不依赖新闻舆论的稳定 baseline | 球员基础资料、球队需求、选秀顺位、历史结果 | `talent_score`、`need_fit_score`、baseline 预测和 2025 回测 |
| 第二层：动态分析层 | 把 mock draft、试训、伤病、记者消息、交易传闻转成可计算信号 | 外部网页、新闻、big board、论坛情绪、手动导入文本 | `consensus_score`、`intel_score`、风险信号和证据记录 |
| 第三层：整合输出层 | 模拟真实选秀中每个顺位的连锁选择 | 第一层分数、第二层情报、球队画像、BPA/Fit 权重 | 1-30 顺位预测、备选球员、置信度和简短理由 |

这样拆分有几个好处：

- 可回测：第一层可以单独用 2025 数据验证，避免动态新闻和选秀后信息污染 baseline。
- 可解释：每个预测可以拆成基础天赋、球队适配、外部共识、情报修正和风险扣分。
- 可迭代：先跑通第一层，再逐步加入第二层情报和第三层链式模拟，不需要一开始构建复杂系统。
- 可复核：第二层保留原文证据和来源，方便人工确认 AI 抽取结果是否可靠。
- 贴近真实选秀：第三层按顺位链式选择，能体现球员被提前选走、滑落、交易扰动带来的连锁变化。

## 第一层为何不用机器学习或深度学习

第一层基础信息层的目标是建立一个干净、可解释、可回测的 baseline，因此第一版不使用机器学习或深度学习，而是采用 `pandas` 数据清洗加规则评分公式。

主要原因：

- 历史样本太少。即使使用 26 年首轮数据，也只有约 `26 * 30 = 780` 条首轮样本，不适合训练深度学习模型，传统机器学习也容易过拟合。
- 第一层不使用新闻舆论、mock draft 和实时情报，只依赖年龄、位置、尺寸、学校/联赛、球队需求等结构化字段，更适合明确公式和规则权重。
- 黑客松路演需要解释预测逻辑，规则公式比黑盒模型更容易说明，也更容易定位错误来源。
- 第一层是后续第二层情报和第三层模拟的对照组，重点是稳定、透明、可复现，而不是追求复杂模型。

第一版 baseline 建议使用：

```text
baseline_score(player, team) =
talent_score(player) * 0.70
+ need_fit_score(player, team) * 0.30
```

其中 `talent_score` 衡量球员基础天赋和结构化潜力，`need_fit_score` 衡量球员位置与球队需求的匹配程度。`sklearn` 可以作为后续对照实验，例如 Ridge、LogisticRegression 或 RandomForest，但不作为第一版主线。

## Web Agent 与第二层 AI 用法

本项目中的 Web Agent 不优先设计成自然语言问答入口，而是作为比赛交付界面和第二层动态情报流水线的操作台。它的核心任务是展示预测结果，同时帮助团队把外部情报转成可计算、可追踪、可复核的数据。

AI 工具（例如 Claude、GPT-5.5）主要放在第二层动态分析层，用于理解非结构化信息，而不是直接凭感觉生成最终选秀表。推荐分工如下：

| 环节 | 主要负责方 | 说明 |
| --- | --- | --- |
| 爬取 / 导入原始资料 | 普通代码 | 抓取或上传 mock draft、big board、新闻、记者消息、试训名单、伤病信息、交易传闻和预测市场信号 |
| 轻量 RAG / Evidence Store | 普通代码 + Embedding 可选 | 保存原文、URL、日期、来源权重和文本片段，按球员、球队、日期、来源检索相关证据，供 AI 抽取时使用 |
| 文本理解 / 情报抽取 | Claude / GPT-5.5 | 从文章、网页或手动粘贴文本中抽取球员、球队、事件类型、信号强度和证据摘要 |
| 数据校验 / 清洗 / 去重 | 普通代码 | 标准化球员名、球队名、日期、来源，检查重复记录和不合法字段 |
| 写入结构化数据表 | 普通代码 | 生成或更新 `Data/mock_drafts_2026.csv`、`Data/intel_signals_2026.csv` 等文件 |
| 转成模型特征 | 普通代码 | 根据来源权重、事件类型和置信度计算共识排名分、情报修正分和风险修正 |
| 预测排序 / 链式模拟 | 普通代码 | 使用明确公式和规则生成 1-30 顺位预测，保证结果可复现 |
| 结果解释文案 | Claude / GPT-5.5 | 根据结构化分数和证据生成简短理由、备选方案说明和情景差异说明 |

第二层 AI 抽取结果建议统一写成以下结构：

```csv
date,source,type,player,team,signal,confidence,url
```

其中 `type` 至少包含：

- `workout`：试训或会面信息
- `promise`：疑似承诺或强绑定信号
- `injury`：伤病、体测、健康风险
- `team_interest`：球队兴趣
- `trade_rumor`：交易传闻或签位变动信号
- `reporter_signal`：可靠记者的倾向性判断
- `market_signal`：Kalshi Hoops 等预测市场或赔率信号，作为外部预期参考
- `forum_sentiment`：论坛或社交媒体情绪，低权重使用

Web Agent 对这一层应提供的能力：

- 导入或刷新外部来源
- 维护轻量 RAG / Evidence Store，用于保存原文并按球员、球队、日期和来源检索证据
- 展示 AI 抽取出的情报记录
- 标记每条记录的来源、时间、置信度和是否已通过校验
- 支持人工确认、修正、忽略可疑情报
- 展示情报如何影响 `intel_score`、共识排名和最终预测

原则上，第一层 baseline 不依赖 AI；第二层大量使用 AI 做情报理解和结构化抽取；第三层由代码负责排序和模拟，AI 只辅助生成理由和情景说明。

## 第三层链式模拟

第三层负责把第一层 baseline 和第二层动态情报合并成最终 1-30 顺位预测。它不是简单给球员排一个总榜，而是按真实选秀顺序模拟每支球队的选择。

链式选择的意思是：每个顺位只从当前还没有被选走的球员中选择；一名球员被前面的球队选走后，后面的球队不能再选他。前一个顺位的选择会影响后一个顺位，爆冷、滑落、交易和球队需求变化都会形成连锁反应。

简化流程：

```python
available_players = all_players.copy()
results = []

for pick in draft_order:
    team = pick.team

    scored_candidates = []
    for player in available_players:
        score = final_score(player, team)
        scored_candidates.append((player, score))

    selected_player, selected_score = max(scored_candidates, key=lambda item: item[1])

    results.append({
        "pick": pick.number,
        "team": team,
        "player": selected_player,
        "score": selected_score,
    })

    available_players.remove(selected_player)
```

第三层的核心公式可以从以下版本开始：

```text
final_score(player, team) =
bpa_score(player) * team.bpa_weight
+ fit_score(player, team) * team.fit_weight
+ consensus_score(player) * 0.20
+ intel_score(player, team) * 0.15
- risk_penalty(player) * 0.10
```

其中：

- `bpa_score`：不考虑球队需求，只衡量当前可选球员的综合强度。
- `fit_score`：衡量球员位置、技能和球队需求的匹配程度。
- `consensus_score`：来自 mock draft 和 big board 的外部共识。
- `intel_score`：来自试训、承诺、记者消息、伤病和交易传闻等第二层情报。
- `risk_penalty`：伤病、投射不稳定、尺寸不足、年龄偏大、角色不清晰等风险扣分。

第三层应至少输出三种情景：

- `baseline`：只使用第一层结构化数据。
- `consensus`：加入 mock draft / big board 共识。
- `intel`：加入试训、承诺、伤病、交易传闻等动态情报。

最终预测表建议输出：

```csv
scenario,pick,team,player,confidence,alternatives,reason
```

`confidence` 可以根据第一名和第二名的分差、共识强度和情报可靠性计算；`alternatives` 记录同一顺位得分第 2-4 名候选人；`reason` 由结构化分数和证据生成，AI 只负责生成简短文案，不直接决定排序。

## 备战计划

### Week 1: 工具上手与数据准备（6/8 - 6/13）

- 跑通 Cursor / Python / pandas 基础流程
- 建立 GitHub 仓库与协作流程
- 整理 NBA 数据源清单，并把基础数据下载到本地
- 编写 CSV 读取、数据预览、身高排序、散点图等基础脚本
- 阅读 2024/2025 NBA Draft 前 30 名球员资料
- 跑通第一版 end-to-end baseline，rule-based 方法也可以
- 6/13 20:00 进行第一次团队同步会

### Week 2: 实战演练与交付准备（6/15 - 6/22）

- 6/15 - 6/16：用 2024 数据做一次 mini-hackathon，模拟预测 2024 选秀
- 6/17 - 6/18：复盘预测误差，改进 features
- 6/19：加入第二层 AI 情报抽取，用 Claude/GPT 将 mock draft、新闻、试训、伤病和交易传闻转成结构化信号
- 6/20：进行 8 小时 Dry Run，完整走一遍比赛日流程
- 6/21：微调分工，确认最终工具栈
- 6/22：完成设备、账号、API、数据和提交材料检查

## 分工方向

| 方向 | 目标 |
| --- | --- |
| 数据工程 | 数据源整理、历史选秀数据清洗、候选人数据更新、特征表构建 |
| 模型建构 | baseline、规则模型、排序模型、情报修正分、预测结果融合 |
| 可视化与 Web Agent | 预测结果展示、情报流水线、AI 抽取结果审核、球队/球员解释、路演界面 |

API key 和其他密钥不提交到 GitHub。开发时使用本地环境变量、`.env` 文件或云端 secret 管理。

## 关键时间节点

| 日期 | 事件 |
| --- | --- |
| 2026-06-13 20:00 | 第一次团队同步会 |
| 2026-06-16 | 预计公布最终评分细则 |
| 2026-06-20 | 8 小时 Dry Run |
| 2026-06-22 | 装备与提交材料检查 |
| 2026-06-23 09:00 | 比赛开始 |
| 2026-06-24 08:00 | 提交截止 |
| 2026-06-24 12:00 | 公布结果 |

## 项目状态

当前阶段：数据准备与方案设计。

下一步工作：

- 补充历史 NBA 选秀数据
- 补充候选球员赛季数据和体测数据
- 建立第一版预测规则或 baseline 模型
- 设计 Web Agent 展示界面
