---
dlayout:    post
title:      21 Alpha Examples for WQC
subtitle:   notebooks for WQ learning
date:       2023-8-19
author:     Kylin
header-img: img/bg-quantitive-trading-intro.jpg
catalog: true
tags:
    - quant
---



[TOC]



> notebooks from [21 Alpha Examples for Beginners](https://platform.worldquantbrain.com/simulate/learn/documentation/create-alphas/19-alpha-examples)



### **Future Investment and Dividend**

**Hypothesis**

Retained Earnings (留存收益或保留盈余）是公司财务报表中的一项，特别是在资产负债表上。它代表公司自成立以来所赚取的总利润，减去已分配给股东的所有股利。这部分是未来投资和分红的指标。

> Retained Earnings is a measure of a firm's net income that it has accrued over time and saved after the distribution of dividend. So Retained Earnings are an indicator of the capability of future investment and dividend.

**Implementation**

Use sharesout (流通在外的股票) to normalize the amount of retained earnings; then apply ts_delta() operator to capture the change of the ratio over the last three months (one quarter). Finally use rank() operator to normalize the result.

**Hint to improve the Alpha**

Think about how many trading days in one quarter. Maybe there are less than 90 trading days in three months.（60）

```
rank(ts_delta(retained_earnings / sharesout, 60))
#####################
Region	Universe	Language	Decay	Delay	Truncation	Neutralization	Pasteurization	NaN Handling	Unit Handling
CHN	TOP3000	Fast Expression	5	1	0.01	Subindustry	On	Off	Verify
```



**Hypothesis**

We usually buy an asset because we are either looking for price appreciation or its generated cash flow. In case of a stock, we would want to go long on stocks with either high growth rate or dividend yield.

**Implementation**

Use mdf_gry as a metric to measure both aspects. We then identify stocks with rising mdf_gry by calculating zscore of its historical values over the last month.

**Hint to improve the Alpha**

One way to improve the Alpha is to compare the metric with those of other companies using cross-sectional operators.



