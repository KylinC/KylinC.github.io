---
dlayout:    post
title:      Fundamentals of Quantitative Trading
subtitle:   notebooks for WQ learning
date:       2023-8-19
author:     Kylin
header-img: img/bg-quantitive-trading-intro.jpg
catalog: true
tags:
    - quant
---



[TOC]

### Key concepts of Quantitative Finance

#### 股票市场如何运作

股票市场是指为发行、购买和出售在证券交易所或场外交易的股票而存在的公共市场。股票，也被称为股权，代表着对一家公司的部分所有权，而股市是一个投资者可以购买和出售这种可投资资产所有权的地方(Corporate Finance Institute. (2022, October 28). Stock market. https://corporatefinanceinstitute.com/resources/wealth-management/stock-market/)。



#### 交易量的定义

交易量是指在一段时间内，通常在一天内，一种资产或证券的换手率。例如，股票交易量可以指在每天开盘和收盘之间交易的股票数量。交易量，以及交易量在一段时间内的变化，是技术交易者的重要信息。

换手率=某段时间内的交易量/该资产的总发行量或平均持有量×100%



#### 什么是收盘价/开盘价

开盘价是指证券在一个交易日开盘时首次交易的价格。收盘价是交易时段结束前的最后一笔交易的价格。这些价格很重要，因为它们被用来创建传统的线型股票图表，以及在计算移动平均线和其他技术指标时使用。



#### 统计套利

随着信息技术、信息处理、自动交易等的发展，市场总是接近100%的效率，但从未达到100%。我们寻求从这种不完全效率中套利获利。
预测某只股票的未来回报很困难。但是，如果我们每天押注很多股票，并且经常获胜，那么我们可以在长期内统计地期望获得利润。**基于金融数据模式押注许多股票的过程被称为统计套利**。

**统计套利也被称为Alpha**。一个好的统计套利模型通常涉及大量证券，并且可能具有短持有期和正长期预期收益的特点*.*



#### 头寸

“头寸”是金融和投资领域的术语，它描述的是投资者或交易者在特定资产或合约上的持仓数量。简而言之，头寸代表了一个人或机构在某个资产上的投资大小或风险敞口。

头寸管理是交易和投资策略的核心组成部分，因为它涉及到如何决定买入或卖出的数量以及如何为整个投资组合分配资金。

头寸可以有以下几种类型：

1. **多头头寸 (Long Position)**：这意味着投资者拥有或已购买某个资产，期望它的价格上涨，从而在未来获利。
2. **空头头寸 (Short Position)**：这意味着投资者已借入并卖出某个资产，期望它的价格下跌，从而在未来以较低的价格买回并获利。
3. **平头寸 (Flat or Square Position)**：这意味着投资者没有多头或空头的持仓，即他们已经完全清仓。



#### Alpha

Alpha是一种算法，它将输入数据（价格-成交量、新闻、基本面等）转化为一个向量，其中的数值与我们每天要持有的每种金融工具的头寸和权重成正比。



#### PnL

> **累积盈亏** Profit and Loss'

我们从Alpha表达式中得到了股票的权重，下一步就是要得到每一天的利润和损失（PnL）。

从上表中，我的权重_A = 0.2，权重_B = -0.5，权重_C = 0.3。现在，我的投资金额被称为 "账面规模"。假设我的账面金额是100美元。所以我计算我想投资于每只股票的资金：

money_A = 0.2 * 100 USD = 20 USD 多头

money_B = -0.5 * 100 USD = 50 USD 空头

money_C = 0.3 * 100 USD = 30 USD 多头

现在，我买入价值20美元的A，卖出价值50美元的B，买入价值30美元的C。

我持有这个投资组合一整天，并在模拟期的第二天把它卖掉。现在在一天之内，股票A、B、C的价格都发生了变化。因此，我的投资组合的总价值也发生了变化，例如从100美元到105美元。因此，我在这一天赚了5美元。

现在我再次计算股票的Alpha值，再次计算权重，再次交易价值100美元的投资组合。[在BRAIN平台上，我们对所有的日子都使用恒定的账面大小，无论你的投资组合是赚钱还是亏钱。］

在回测期的每一天都要重复这样的操作，以计算和绘制累积的PnL



**降低风险和波动性**

一个好的阿尔法最好是有持续增长的净值，高的年回报率，更重要的是，在累积利润图中的波动很小。如果标准差较低，图表中的波动就会较小。如果图表中显示出高波动/波幅，尽管回报率很高，那么阿尔法就不会被认为是足够好。

WorldQuant旨在开发具有低波动性和低风险的股票多空市场中性阿尔法指数。这种投资具有吸引力，因为它们有望产生比只做多头的投资组合更好的风险调整后的回报。**股票多空市场中性策略被对冲基金普遍使用，其目标是最大限度地减少市场风险，并从两只股票的价差变化中获利。**



#### 市值 Market Capitalization

衡量一家上市公司总体规模的指标。它等于公司每股股票的市场价格与公司的总流通股票数（outstanding shares）的乘积。

计算公式如下：

市值=每股股票价格×流通在外的股票总数



#### 样本内和样本外

由于PnL是为历史数据生成的，因此被称为样本内PnL。提交Alpha后，它必须通过BRAIN平台设置的一些标准。如果通过标准，则将根据实时数据进行评估，评估期间称为样本外（OS）。在这种情况下生成的PnL称为样本外PnL。

<img src="https://kylinhub.oss-cn-shanghai.aliyuncs.com/isos.png" alt="isos" style="zoom:90%;" />



#### **卖空**

“做多”股票仅意味着购买它。如果购买后股票价格上涨，您就可以赚钱。如果价格下跌，您就会亏钱。“卖空”（又称做空）则创建了相反的价格利润关系，操作如下：

当您卖空一只股票时，您的[broker](http://www.investopedia.com/terms/b/broker.asp) 会将其借给您。股票来自经纪公司自己的存货、公司的另一位客户或另一个经纪公司。股票被卖出，所得款项将存入您的账户。早晚，您必须通过购买相同数量的股票（称为回补[covering](http://www.investopedia.com/terms/s/shortcovering.asp)) 并将其归还给您的经纪人来“平仓”。如果股票价格下跌，您可以以较低的价格买回股票并从差价中获利。如果股票价格上涨，您必须以更高的价格买回它，因此造成了亏损。

大多数情况下，您可以持有空头很长时间，但因为经纪商会对保证金账户收取利息，因此长时间保持空头仓位将会成本更高。但是，如果出借者想要回收您借出的股票，您则可能被迫回补。经纪公司无法出售他们没有的股票，因此您要么必须提供新股票进行借贷，要么必须回补。这被称为“[called away](http://www.investopedia.com/terms/c/calledaway.asp)”. 它并不经常发生，但如果许多投资者在做空特定证券，则有可能发生。

由于您没有拥有您正在卖空的股票（您借入并出售了它），因此您必须向股票的出借人支付任何在做空期间宣布的 [dividends](http://www.investopedia.com/terms/d/dividend.asp)（股息） 或 [rights](http://www.investopedia.com/terms/r/right.asp)（权益） 有关更多信息，请访问[Investopedia - Short Selling](http://www.investopedia.com/university/shortselling/).



#### Neutralization 中性化

中性化是将原始Alpha值分成组，然后在每个组内进行归一化（从每个值中减去平均值）的操作。该组可以是整个市场，也可以使用其他分类（如行业或子行业）进行分组。

这样做是为了关注组内股票的相对回报，并将风险暴露于该组的回报最小化。由于中性化的结果，组合是半多头，半空头，并可能保护组合免受市场或行业冲击。

例如，在交易时，我们不想在市场方向上押注，以最小化“市场风险”。这是通过持有相等的多头和空头仓位实现的，即长头仓位中投资的金额与空头仓位中的金额大致相等。这称为“市场中性化”。为了在BRAIN平台中实现这一点，我们在回测设置中将中性化设置为市场（或行业或子行业等）。

假设Alpha = Delta（close，5），其中Alpha是值向量。
设置中性化=市场会使Alpha向量的平均值等于零，即Alpha向量将经历以下更改：Alpha = Alpha-mean(Alpha)。

然后将对该新向量进行归一化和缩放以适应账面价值。因此形成的投资组合将包含等量的多头和空头投资，可用于计算当天的PnL。

WQ建议用户始终选择中性化值；仅在Alpha表达式中包含中性化时将其保留为“NONE”.

WQ可以选择的Neutralization选项：

1. **Market Neutralization（市场中性化）**：此中性化旨在消除策略与整体市场之间的相关性。当投资者希望他们的策略与市场整体的表现无关时，通常会使用这种中性化。
2. **Sector Neutralization（行业中性化）**：此中性化旨在消除策略与特定经济行业或部门的相关性。例如，如果一个因子在某些行业中表现出强烈的偏见（例如，它可能偏向技术股），通过进行行业中性化，投资者可以消除这种偏见，确保策略在所有行业中都有相似的风险敞口。
3. **Industry Neutralization（产业中性化）**：这进一步细化了行业中性化，针对更为具体的产业类别进行中性化。
4. **Subindustry Neutralization（子产业中性化）**：这是对产业划分的进一步细化，消除策略与更为特定的子产业类别之间的相关性。



#### 股票池

回测股票池由要评估的股票集合组成。BRAIN平台仅交易流动性好的股票。BRAIN平台提供标准的股票池，如每个地区的TOP3000、TOP2000、TOP1000等。

TOP-N股票池由过去3个月内平均美元交易量最高的N只该地区股票组成。例如，在流动性最好的股票池中，TOP3000是具有最高流动性的3000只股票的集合；TOP2000是具有最高流动性的2000只股票的集合；依此类推。TOP2000是TOP3000股票的子集。

提示：较大的股票池包括流动性较低的股票，其交易成本和市场交易影响较高。因此，在这种池中的Alphas应该包含加权（例如按市值或成交量）以更少地交易低流动性股票。



### Alpha 构建示例

- 数据字段解释：[Data](https://platform.worldquantbrain.com/learn/data-and-operators/price-volume-data-for-equity)
- 运算符解释：[Learn/Ops](https://platform.worldquantbrain.com/learn/data-and-operators/operators?_gl=1*je5iyn*_ga*MTAwMTk0NzY2OS4xNjcyNzA5NzU3*_ga_9RN6WVT1K1*MTY3MzM2MDU0OC4zNi4xLjE2NzMzNjQ3MjQuNTkuMC4w)

![graph2.png](https://api.worldquantbrain.com/content/images/CkufFbl94S5uR8mcY2ibtUNFKeA=/166/original/graph2.png)



### 股市分析方法

#### 技术分析

技术分析是一种交易纪律，通过分析交易活动中收集的统计趋势（如价格变动和成交量）来评估投资和识别交易机会。技术分析师相信，一个证券的过去交易活动和价格变化可以是该证券未来价格变动的有价值指标。

在整个行业中，已经有数百种模式和信号被研究人员开发出来用于支持技术分析交易。这些包括趋势线、通道、移动平均线和动量指标等。

[更多技术指标](https://www.investopedia.com/terms/t/technicalanalysis.asp)

#### 示例：成交量

如果一家公司的股票成交量很高，意味着许多人在买卖该股票。假设我们的假设是，具有更多交易量的公司比交易量较低的另一家公司更有吸引力。然后我们将更多的权重分配给交易量更高的公司。

```
volume
```

#### 示例：基本面分析

基本面分析是通过检查相关的经济和财务因素来衡量证券的内在价值的一种方法。基本面分析师研究任何可能影响证券价值的因素，从宏观经济因素，如经济状况和行业条件，到微观经济因素，如公司管理的有效性。

分析师可以将公司的增长率与其所在的行业和部门进行比较，以及提供的其他信息，以确定该公司是否被正确地估值。

#### 示例：存货周转率

存货周转率是一种活动比率，用于衡量公司销售和替换存货的速度。计算公式为:

存货周转率=销售额/平均存货

通常，较高的存货水平与更高的存储成本、保险费用和损耗有关。**假设**（这只是假设）是具有较高存货周转率的股票将具有较差的股票表现，因此将被分配负权重。

```
inventory_turnover
```

#### 示例：时间序列和横截面

时间序列分析可以用于观察给定变量随时间的变化情况。假设您想分析一家给定股票在一年期间的每日收盘价格时间序列。您可以获取过去一年内该股票每天的所有收盘价格列表，并使用技术分析工具分析时间序列数据，以确定该股票的时间序列是否显示任何季节性。这将帮助您确定该股票是否每年定期经历峰值和谷值[[details](https://www.investopedia.com/terms/t/timeseries.asp)].

或者，您可以使用横截面分析，将特定公司与其同行业竞争对手进行比较。横截面分析可以针对单个公司进行头对头分析，也可以从行业整体的视角来考虑，以确定具有特定优势的公司。基本上，横截面分析可以向投资者展示，基于您所关心的指标，哪家公司是最好的[[details](https://www.investopedia.com/terms/c/cross_sectional_analysis.asp)].



### Sources of Alpha ideas and research

#### Factor 和 Alpha 的区别

1. **因子 (Factor)**:
   - 因子通常指的是能够解释资产收益率的某些特性或数据。这些特性可以是基于市场的、基于宏观经济的、基于基本面的等等。例如，市值、账面市值比、动量、波动率都可以是因子。
   - 量化投资者使用多因子模型来评估资产的预期收益率，其中每个因子都有一个相应的权重或风险暴露。
   - 因子投资的目标通常是找到哪些因子在未来可能会带来超额收益，并据此构建投资组合。
2. **Alpha (α)**:
   - Alpha 通常指的是投资策略或投资组合相对于某个基准或市场的超额收益。它是在考虑了其他所有已知因子的影响后，仍然无法解释的那部分收益。
   - 简单地说，Alpha 是投资者通过其独特技能、策略或信息获得的超出市场平均水平的收益。正Alpha意味着投资组合的表现超过了基准，而负Alpha意味着其表现不及基准。
   - 在量化投资中，Alpha策略通常是指那些尝试捕捉超额收益的策略，这些超额收益不能被传统的市场因子解释。

因此，可以说因子是用来预测或解释收益的特性或变量，而Alpha是在已知所有因子的基础上，仍然能够获得的超额收益。



#### Alpha思路的来源

Alpha理念可以通过在线研究论文、金融杂志和技术指标等途径获取。

技术指标用于分析短期价格波动，是从股票/资产的一般价格活动中派生出来的。它们通过查看过去的模式来预测一个证券的未来价格水平或价格方向。

常见的技术指标包括相对强弱指数、资金流量指数、移动平均收敛/发散指标（MACD）、布林带等。您可以在像[Stock Charts](https://stockcharts.com/school/doku.php?id=chart_school)和[Incredible Charts](https://www.incrediblecharts.com/)这样的网站上找到它们的解释、公式和相应的分析。

如果您正在寻找Alpha理念，[SSRN](http://papers.ssrn.com/sol3/JELJOUR_Results.cfm?form_name=journalBrowse&journal_id=179252)是一个很好的起点。[Seeking Alpha](http://seekingalpha.com/)和[Wilmott](http://www.wilmott.com/)这样的网站也是不错的选择。博客也是Alpha研究的一个很好的来源，例如[Epchan](http://epchan.blogspot.sg/)和[AuTraSy](http://autrasy.wpengine.netdna-cdn.com/)。

我们还添加了示例[Alpha](https://platform.worldquantbrain.com/learn/documentation/create-alphas/sample-alpha-concepts)，这可以作为构建新的和更强大的Alpha的起点。

供您深入探索的相关概念：

- 价格走势和技术指标
- 波动率指标-历史波动率、隐含波动率、波动率指数、日内波动率等。
- 成交量与价格的互动-成交量与绝对价格变化呈正相关等。
- 短期和长期趋势-[识别市场趋势](https://www.investopedia.com/articles/technical/03/060303.asp)。



#### Alpha入门的相关论文

- 101个公式化的Alpha
  - https://arxiv.org/ftp/arxiv/papers/1601/1601.00991.pdf
  - 点评：本文列出了101个Alpha表达式。虽然大多数不可提交，但它们提供了一个很好的起点。
- 预期股票回报的横截面研究
  - https://www.ivey.uwo.ca/media/3775518/the_cross-section_of_expected_stock_returns.pdf
  - 点评：这是著名的Fama-French论文，它提出了Fama-French因子模型和横截面股票研究方法的基本原理。
- 无处不在的价值和动量
  - https://pages.stern.nyu.edu/~lpederse/papers/ValMomEverywhere.pdf
  - 点评：这是价值和动量的一个很好的介绍；它们可以在几个国家的个股、跨国家股票指数、政府债券、货币和商品中产生超额回报。
- 股票价格的均值回归：证据和影响
  - https://papers.ssrn.com/sol3/papers.cfm?abstract_id=227278
  - 点评：这是反映股票价格均值回归性质的最早的论文之一。
- 价格动量和交易量
  - https://technicalanalysis.org.uk/volume/LeSw00.pdf
  - 点评：提供了有关交易量的角色的思路：过去的交易量为动量和价值策略提供了重要的联系。



### Recommended readings on Finance for advanced users

1. **主动投资组合管理**
   **来源：**[**http://en.wikipedia.org/wiki/Active_management**](http://en.wikipedia.org/wiki/Active_management)
   **来源：Richard Grinold和Ronald Kahn的《主动投资组合管理：产生卓越回报和控制风险的量化方法》。
   点评：提供了主动投资组合管理、预期回报和估值以及实现的基础。
   **
2. **市场中性**
   **来源：**[**http://en.wikipedia.org/wiki/Market_neutral**](http://en.wikipedia.org/wiki/Market_neutral)
   **点评：定义了市场中性、股票市场中性，并提供了示例。
   **
3. **资本资产定价模型**
   **来源：**[**http://en.wikipedia.org/wiki/Capital_asset_pricing_model**](http://en.wikipedia.org/wiki/Capital_asset_pricing_model)
   **来源：**[**http://papers.ssrn.com/sol3/papers.cfm?abstract_id=440920**](http://papers.ssrn.com/sol3/papers.cfm?abstract_id=440920)
   **点评：介绍了CAPM、其公式、证券市场线、资产定价、特定的资产所需回报率、风险和多元化、有效前沿、市场组合和假设。
   **
4. **Fama和French模型**
   **来源：**[**http://www.moneychimp.com/articles/risk/multifactor.htm**](http://www.moneychimp.com/articles/risk/multifactor.htm)
   **点评：概述了使用Fama-French模型进行投资组合分析、投资未来和从分析中推断出的结论。
   **
5. **市场效率**
   **来源：**[**http://www.investopedia.com/articles/02/101502.asp**](http://www.investopedia.com/articles/02/101502.asp)
   **来源：**[**http://en.wikipedia.org/wiki/Financial_market_efficiency**](http://en.wikipedia.org/wiki/Financial_market_efficiency)
   **点评：解释了市场效率是什么，效果、挑战和效率程度以及其影响。
   **
6. **信息比率**
   **来源：**[**http://en.wikipedia.org/wiki/Information_ratio**](http://en.wikipedia.org/wiki/Information_ratio)
   **来源：**[**http://www.cfapubs.org/doi/abs/10.2469/ipmn.v2011.n1.7**](http://www.cfapubs.org/doi/abs/10.2469/ipmn.v2011.n1.7)
   **来源：**[**http://seekingalpha.com/article/63911-clarifying-the-information-ratio-and-sharpe-ratio**](http://seekingalpha.com/article/63911-clarifying-the-information-ratio-and-sharpe-ratio)
   **点评：信息比率的公式和定义，以及将Sharpe与IR进行比较。
   **
7. **公司基本面分析**
   **来源：**[**https://www.tradeking.com/education/stocks/fundamental-analysis-explained**](https://www.tradeking.com/education/stocks/fundamental-analysis-explained)
   **来源：**[**http://en.wikipedia.org/wiki/Fundamental_analysis**](http://en.wikipedia.org/wiki/Fundamental_analysis)
   **点评：概述了基本面分析的步骤、优点和缺点以及五个重要因素。**















