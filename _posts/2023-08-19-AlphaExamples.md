---
dlayout:    post
title:      21 Alpha Examples for WQC
subtitle:   21 个有效的 Alpha
date:       2023-8-19
author:     Kylin
header-img: img/bg-quantitive-trading-intro.jpg
catalog: true
tags:
    - quant
---



[TOC]



> notebooks from [21 Alpha Examples for Beginners](https://platform.worldquantbrain.com/simulate/learn/documentation/create-alphas/19-alpha-examples)



### Data Field Search

https://platform.worldquantbrain.com/data/search



### **Future Investment and Dividend**

**Hypothesis**

Retained Earnings (留存收益或保留盈余）是公司财务报表中的一项，特别是在资产负债表上。它代表公司自成立以来所赚取的总利润，减去已分配给股东的所有股利。这部分是未来投资和分红的指标。

> Retained Earnings is a measure of a firm's net income that it has accrued over time and saved after the distribution of dividend. So Retained Earnings are an indicator of the capability of future investment and dividend.

**Implementation**

Use sharesout (流通在外的股票) to normalize the amount of retained earnings ([ref](https://platform.worldquantbrain.com/learn/data-and-operators/company-fundamental-data-for-equity));  then apply ts_delta() operator to capture the change of the ratio over the last three months (one quarter). Finally use rank() operator to normalize the result.

**Hint to improve the Alpha**

只能统计60天的，因为数据默认按照交易日进行统计，一个季度只有60天交易日

> Think about how many trading days in one quarter. Maybe there are less than 90 trading days in three months.

```
rank(ts_delta(retained_earnings / sharesout, 60))
#####################
Region	Universe	Language	Decay	Delay	Truncation	Neutralization	Pasteurization	NaN Handling	Unit Handling
CHN	TOP3000	Fast Expression	5	1	0.01	Subindustry	On	Off	Verify
```

Q：为什么必须只能是季度单位？



### **Growth or dividends**



**Hypothesis**

要买就买增长好的or产生分红的

> We usually buy an asset because we are either looking for price appreciation or its generated cash flow. In case of a stock, we would want to go long on stocks with either **high growth rate or dividend yield.**

**Implementation**

Use mdf_gry（projected growth rate + dividend yield, [ref](https://platform.worldquantbrain.com/learn/data-and-operators/model-data)） as a metric to measure both aspects. We then identify stocks with rising mdf_gry by calculating zscore of its historical values over the last month.

> `zscore` 是一个统计学上的术语，表示一个数值与样本平均值的差，除以样本的标准差。

**Hint to improve the Alpha**

One way to improve the Alpha is to compare the metric with those of other companies using cross-sectional operators.

```
rank(ts_zscore(mdf_gry, 20))
######################
Region	Universe	Language	Decay	Delay	Truncation	Neutralization	Pasteurization	NaN Handling	Unit Handling
USA	TOP3000	Fast Expression	3	1	0.08	Sector	On	Off	Verify
```

Q: 为什么Neutralization选择其他选项的时候不可以



###　**Demand of call options**

**Hypothesis**

Use call implied breakeven price to gauge the demand of call options. If investors are bullish on one stock, the demand for call option should go up, increasing call option's premium, and then breakeven price. Apply reversion in the returns of call implied breakeven price by capturing correlation of rank returns over time.

**Implementation**

The rate of change is stored in x. ts_corr() is used to capture both the trends mentioned in the hypothesis. rank() is used to capture the ranked returns.

**Hint to improve the Alpha**

Boost the signal with other option data. For example, if too many option investors (including uninformed traders) are betting on put options, compared to call options, it is likely to revert.

```
w = call_breakeven_20;
x = (w /last_diff_value(w, 1) ) - 1;
-ts_corr(rank(x), ts_step(120), 120)
###################################
Region	Universe	Language	Decay	Delay	Truncation	Neutralization	Pasteurization	NaN Handling	Unit Handling
USA	TOP3000	Fast Expression	0	1	0.08	Market	On	Off	Verify
```

