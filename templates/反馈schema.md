# 反馈数据 Schema

统一 schema 是反馈池能用的前提。三个月没有 schema 的池子，会变成无法查询的垃圾场。

## 字段定义

| 字段 | 类型 | 必填 | 取值 / 说明 |
|------|------|------|-----------|
| `id` | string | ✅ | 唯一 ID |
| `source` | enum | ✅ | support / appstore / social / survey / sales / interview |
| `category` | enum | ✅ | bug / feature_request / ux_issue / performance / pricing / praise |
| `severity` | enum | ✅ | critical（阻塞）/ high（严重）/ medium / low |
| `affected_flow` | enum | ✅ | onboarding / core / billing / search / settings / other |
| `user_tier` | enum | ✅ | free / pro / enterprise / churned（**必填，不可空**） |
| `user_id` | string | | 用于分层和闭环 |
| `sentiment` | float | | -1.0 ~ 1.0 |
| `feature_area` | string | | 具体模块，便于聚合 |
| `raw_text` | text | ✅ | **原文，永不删改** |
| `tags` | array | | 自由标签，用于聚类 |
| `created_at` | datetime | ✅ | 反馈发生时间（不是录入时间） |
| `resolved_at` | datetime | | 已处理时间 |
| `resolution_note` | string | | 怎么处理的 |
| `notified_user` | bool | | 是否已闭环通知用户 |

## 周报输出（一屏内）

```markdown
# 反馈周报 W[N]（n=）

**一句话**：

## Top 5 主题
| # | 主题 | 条数 | 占比 | 环比 | 涉及层级 |

## 需要决策的事
1.
2.

## 已闭环
- 
```

## 分层交叉（必做，否则会被大客户绑架）

```markdown
| 主题 | 免费 | 付费 | 企业 | 流失 | 判读 |
|------|------|------|------|------|------|
|      |      |      |      |      |      |
```

## 频次 × 影响矩阵

| | 影响大 | 影响小 |
|---|-------|-------|
| **频次高** | 立即处理 | 常规迭代 |
| **频次低** | ⚠️ **最容易忽视，往往是留存杀手** | 忽略 |

## 反馈闭环模板

> 你上个月提到的 [问题]，我们在 [版本] 里改了：[一句话说改了什么]。谢谢你说出来——这条反馈直接推动了这次改动。

**闭环率目标**：≥50%。做到之后，反馈池的质量会明显变好。
