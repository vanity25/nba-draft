# Todo: 基于 PDF 框架的三层选秀预测架构

## 目标

用 `Preparation/ds对话.pdf` 最后部分的预测框架推进项目：先建立基础信息层，再加入动态情报分析，最后做顺位链式模拟和结果输出。最终交付 2026 NBA 首轮 1-30 顺位预测、备选方案、置信度和简短理由。

## 第一层：基础信息层

目标：建立不依赖新闻舆论的纯结构化 baseline，用来做 2025 回测和后续模型对照。第一层先用 `pandas + 规则评分公式`，不使用深度学习；`sklearn` 只作为后续对照模型，不作为第一版核心。

- [ ] 整理候选球员池，字段建议：

```csv
player,position,age,school_club,height,weight,country,status
```

- [ ] 用 pandas 清洗候选球员数据，保留原始数据并另存 clean 文件，至少处理：
  - 统一列名和文本空格。
  - 年龄、身高、体重转成可计算数字。
  - 删除重复球员。
  - 标记缺失值和异常值。
  - 输出 `height_inches`、`weight_lbs` 等建模字段。
- [ ] 整理首轮顺位表，字段建议：

```csv
pick,team,original_team,reported_final_team
```

- [ ] 补充历史数据：优先收集 2023、2024、2025 年 prospects、draft order 和最终结果。
- [ ] 建立球员基础标签：位置、年龄段、尺寸、学校/联赛背景、是否国际球员、是否大一新生。
- [ ] 建立球员基础评分 `talent_score`，只使用结构化信息，不加入 ESPN、BR、USA Today 等媒体排名。
- [ ] 第一版 `talent_score` 先用可解释规则公式，例如：

```text
talent_score =
size_score * 0.25
+ age_score * 0.20
+ position_value_score * 0.15
+ background_score * 0.20
+ status_score * 0.20
```

- [ ] 建立球队基础需求表，字段建议：

```csv
team,stage,need_pg,need_sg,need_wing,need_big,need_shooting,need_defense,need_creation
```

- [ ] 建立 `need_fit_score`，第一版先用位置适配和球队需求计算，后续再加入投射、防守、创造力等技能需求。
- [ ] 建立第一版 baseline 公式：

```text
baseline_score(player, team) =
talent_score(player) * 0.70
+ need_fit_score(player, team) * 0.30
```

- [ ] 按真实选秀顺序做链式选择：每个顺位从未被选走的球员中选择 `baseline_score` 最高者，并从候选池移除。
- [ ] 用 2025 数据回测：只使用 2025 选秀前可获得的数据，对比真实首轮结果，记录命中率和偏差。
- [ ] 记录 2025 回测指标：完全命中数、乐透区命中数、平均顺位误差、首轮覆盖率。
- [ ] 可选：用 `sklearn` 做传统机器学习对照组，例如 Ridge、LogisticRegression、RandomForest，不做深度学习。

## 第二层：动态分析层

目标：把外部共识和实时情报转化为可计算信号。

- [ ] 建立 mock draft / big board 表，字段建议：

```csv
date,source,player,mock_pick,mock_team,big_board_rank,url
```

- [ ] 优先收集高价值来源：ESPN、The Athletic、Bleacher Report、Tankathon、NBA Draft Room、USA Today。
- [ ] 将媒体排名转成数值特征：平均排名、最高排名、最低排名、排名方差、进入首轮次数、被某队预测次数。
- [ ] 建立情报流表，字段建议：

```csv
date,source,type,player,team,signal,confidence,url
```

- [ ] 情报类型至少包含：`workout`、`promise`、`injury`、`team_interest`、`trade_rumor`、`reporter_signal`、`forum_sentiment`。
- [ ] 记录每条信息的发布时间，回测时禁止使用选秀后发布的信息。
- [ ] 将试训、承诺、记者消息和交易传闻转成修正分 `intel_score`。
- [ ] 将虎扑、Reddit、X 等论坛舆论作为低权重热度或情绪信号，不作为核心判断依据。
- [ ] 给来源设置权重：mock draft 高权重，可靠记者中高权重，论坛舆论低权重，单条未经证实传闻低权重。

## 第三层：整合输出层

目标：模拟 PDF 中的 BPA、Fit、风险偏好、顺次挑选和联动调整，形成最终榜单。

- [ ] 为 30 支球队建立决策画像，字段建议：

```csv
team,stage,core_players,risk_preference,trade_likelihood,bpa_weight,fit_weight,draft_tendency
```

- [ ] 评估球队阶段：争冠、季后赛边缘、重建、围绕年轻核心建队。
- [ ] 标注球队需求：控卫、侧翼、防守、投射、护框、替补深度等。
- [ ] 总结球队历史偏好：年龄、位置、尺寸、大学/海外背景、即战力或潜力。
- [ ] 建立 BPA 分：不考虑球队需求，只衡量当前可选球员中谁最强。
- [ ] 建立 Fit 分：衡量球员位置、技能和球队短板是否匹配。
- [ ] 建立风险分：伤病、年龄、投射稳定性、对抗、角色不清晰等扣分项。
- [ ] 建立最终匹配分：

```text
最终匹配分 =
BPA 分 * bpa_weight
+ Fit 分 * fit_weight
+ 共识排名分
+ 情报修正分
+ 球队历史偏好分
- 风险扣分
```

- [ ] 做第一遍顺次挑选：从 1 号签开始，每个顺位选择当前匹配分最高的球员，并从候选池移除。
- [ ] 做第二遍联动调整：检查前几顺位爆冷、滑落、跳选、交易传闻对后续顺位的影响。
- [ ] 标注预测类型：确定性预测、概率预测、范围预测。
- [ ] 为每个签位输出预测球员、2-3 个备选球员、置信度和简短理由。
- [ ] 加入三种情景：结构化 baseline、舆论共识版、交易扰动版。

## 数据表与脚本建议

- [ ] `Data/players_2026.csv`：2026 候选球员基础表。
- [ ] `Data/draft_order_2026.csv`：2026 首轮顺位表。
- [ ] `Data/team_needs_2026.csv`：球队需求表。
- [ ] `Data/mock_drafts_2026.csv`：mock draft 和 big board 汇总表。
- [ ] `Data/intel_signals_2026.csv`：新闻、试训、承诺、伤病、交易和舆论信号表。
- [ ] `Preparation/baseline_predict.py`：第一层 baseline 脚本。
- [ ] `Preparation/backtest_2025.py`：2025 回测脚本。
- [ ] `Preparation/simulate_draft.py`：链式顺位模拟脚本。

## 交付物

- [ ] `Data/` 中的结构化输入数据。
- [ ] `Preparation/` 中的数据清洗和回测脚本。
- [ ] 一份 2025 回测结果，用于验证方法。
- [ ] 一份 2026 首轮预测表，包含 `pick`、`team`、`player`、`confidence`、`alternatives`、`reason`。
- [ ] Web Agent 或可视化界面，展示预测名单、BPA/Fit 理由、球队需求、球员匹配和情景切换。

## 当前优先级

1. 先把第一层做干净：球员池、顺位、球队需求、baseline、2025 回测。
2. 再补第二层：mock draft、big board、试训、承诺、伤病、交易传闻。
3. 最后实现第三层：BPA vs Fit、风险偏好、链式模拟、备选方案和置信度。
