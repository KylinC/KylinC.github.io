---
dlayout:    post
title:      Cheatsheet for Quant Basics
subtitle:   Quant概念速查手册
date:       2023-8-26
author:     Kylin
header-img: img/bg-quantitive-trading-intro.jpg
catalog: true
tags:
    - quant
---



[TOC]



### 指标类

#### OEY

Operating earnings yield（经营盈利收益率）表示企业的经营盈利与市场价值之间的比率。

```
Operating Earnings Yield= Operating Earnings / Market Capitalization
```

> Operating Earnings 是公司的经营盈利，有时被称为EBIT（息税前盈利）或经营收入。
>
> Market CapitalizationMarket Capitalization 是公司的市场价值，计算方法是股票的市场价格x该公司总的流通股票数。

- usage

OEY越高，说明公司的估值相对较低，这可能表示投资机会。然而，也应考虑其他许多因素，如行业趋势、公司的增长预期和其它风险因素。



#### EBIT

Earnings Before Interest and Taxes（息税前盈利、经营盈利、经营收入）

```
EBIT=Net Income+Interest+Taxes
= Revenue−Operating Expenses
```

> Net Income 是公司的净收入。
>
> InterestInterest 是公司支付的利息费用。
>
> TaxesTaxes 是公司支付的税费。
>
> RevenueRevenue 是公司的总收入。
>
> Operating ExpensesOperating Expenses 是公司的经营费用，不包括利息和税费。

- usage

EBIT 可以帮助投资者和分析师更好地理解公司核心经营活动的盈利能力，因为它剔除了融资成本和税务因素。这意味着，通过 EBIT，你可以在不同的公司或同一家公司在不同时期之间进行比较，而不必担心它们的债务结构或税务地位如何影响其盈利。



#### PE

Price-to-Earnings"（市盈率）衡量股票估值的一个基本比率，为公司当前的股价与其每股盈利（EPS，Earnings Per Share）之间的比率。

```
PE= Market Price Per Share/Earnings Per Share (EPS)
```

> Market Price Per Share 是公司股票的当前市场价格。
>
> Earnings Per Share (EPS)Earnings Per Share (EPS) 是公司在一定时期内（通常是过去的 12 个月）为每股股东赚取的净收益。

- usage

通常，PE 被用来评估一个公司是否被高估或低估。

例如，如果一家公司的股票价格是 $50，而其 EPS 是 $5，则 PE 为 10。这意味着投资者愿意支付 $10 以获得 $1 的盈利。

高的 PE 通常表示市场对该公司未来的盈利增长有高期望。而低的 PE 可能表示市场对公司的未来前景不太乐观，或者公司被低估。但解读 PE 时需要注意，不同行业的 PE 标准是不同的。另外，PE 仅是估值工具之一，投资决策还需要考虑其他多种因素。



### 分析术语类

#### 前复权 & 后复权

1. **前复权**:
   - 所有的历史数据都会按照最新的股本变动（如股票分红、配股等）进行调整。
   - 这意味着假设投资者在最初购买了股票，并持有至今，那么前复权的数据会显示他原始投资的价值变化。
   - 前复权的主要优点是股票的持有期收益率是连续的，但其缺点是交易量不连续。
2. **后复权**:
   - 仅调整最近的股本变动，而不会影响之前的历史数据。
   - 这意味着假设投资者最近购买了股票，那么后复权的数据会显示他从购买到现在的价值变化。
   - 后复权的主要优点是交易量是连续的，但其缺点是可能会导致历史股价出现断层，特别是在大幅度的股本变动后。
3. **未复权**:
   - 不进行任何调整，直接使用原始的交易数据。
   - 未复权数据可以真实地反映历史上的每一天的交易情况，但它不考虑因股票分红、配股等股本变动所导致的价格和持有期收益率的连续性问题。

在实际的投资研究和分析中，选择哪种复权方法取决于特定的研究需求。例如，进行长期的回测和策略研究时，通常会选择前复权数据，因为它可以提供连续的持有期收益率。而进行短期的策略研究或交易分析时，可能会选择后复权或未复权数据。

#### 回测 & 实盘

#### Golden Cross & Death Cross

- **Golden Cross (金叉)**：当短期移动平均线上穿长期移动平均线，通常被看作是一个牛市（上涨市场）的信号。
- **Death Cross (死叉)**：当短期移动平均线下穿长期移动平均线，通常被看作是一个熊市（下跌市场）的信号。



#### Bull Market & Bear Market

- **Bull Market (牛市)**：指的是金融市场（如股票、外汇或商品市场）中的价格普遍上涨的时期。在这种市场中，投资者的信心和乐观情绪是主导的。
- **Bear Market (熊市)**：指的是金融市场中的价格普遍下跌的时期。在这种市场中，投资者的信心减少，悲观情绪是主导的。

>  这两个术语的起源与公牛和棕熊的攻击方式有关：公牛使用其角向上挑，象征市场上升；而熊则使用爪子从上到下挥，象征市场下跌。



#### Benchmark Index | Major Index

benchmark index（大盘）是一个金融术语，常用于描述股票市场的整体表现或特定的股票指数。

在中国，当人们提到“大盘”，他们通常指的是上海证券交易所的上证综指或深圳证券交易所的深证成指。这两个指数都反映了其所代表的股票交易所的主要上市公司的整体表现。

与此相类似，在美国，当人们提到“大盘”，他们可能指的是S&P 500、Dow Jones Industrial Average (DJIA) 或 Nasdaq Composite Index，这些都是描述美国股票市场整体表现的主要指数。



#### Moving Average

移动平均线的计算是基于最近的数据点（如最近的10天、20天、50天等）的平均值。根据所采用的时间段，存在不同种类的移动平均线，包括：

1. **简单移动平均线（Simple Moving Average，SMA）**：简单移动平均线是在指定的时间段内价格的算术平均值。例如，10天的SMA是最近10天收盘价的平均值。
2. **指数移动平均线（Exponential Moving Average，EMA）**：指数移动平均线赋予最近的价格数据更大的权重，因此它会对价格的变化反应得更快。
3. **加权移动平均线（Weighted Moving Average，WMA）**：加权移动平均线也考虑了数据的时间顺序，但与EMA不同，它允许用户自定义权重。







