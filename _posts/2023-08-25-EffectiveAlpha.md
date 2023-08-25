---
dlayout:    post
title:      CheatSheet for Alpha
subtitle:   有效的Alpha列表
date:       2023-8-25
author:     Kylin
header-img: img/bg-quantitive-trading-intro.jpg
catalog: true
tags:
    - quant
---



[TOC]



```

```



```python
1/close
Use inverse of daily close price as stock weights. More allocation of capital on the stocks with lower daily close prices.
volume/adv20
Use relative daily volume to the average volume in the past 20 daysas stock weights.
ts_corr(close, open, 10)
Use correlation between daily close and open prices in the past 10 days as stock weights.
open
Use daily open price as stock weights.
(high + low)/2 - close
Use difference between A) average of daily high and low prices and B) daily close price as stock weights.
vwap < close ? high : low
Use daily high as stock weights if the stock closes higher than daily volume weighted average price (vwap), or otherwise use daily lowas stock weights.
Rank(adv20)
Use rank of average daily volume in the past 20 days (adv20) as stock weights.
Min(0.5*(open+close), vwap)
Use the lesser of open close average and vwap as stock weights.
Max(0.5*(high+low), vwap)
Use the greater of high low average and vwap as stock weights.
1/ts_stddev(returns, 22)
Use inverse of standard deviation of stock returns in past 22 days as stock weights.
ts_sum(sharesout, 5)
Use sum of outstanding shares in past 5 days as stock weights.
ts_covariance(vwap, returns, 22)
Use covariance of vwap and returns for the past 22 days as stock weights.
1/Abs(0.5*(open+close) - vwap)
Use the inverse of absolute difference between open close average and vwap as stock weights.
ts_corr(vwap, ts_delay(close, 1), 5)
Use correlation between vwap and previous day's close for past 5 days as stock weights.
ts_delta(close, 5)
Use difference between daily close and close on the date 5 days earlier as stock weights.
ts_decay_linear(sharesout*vwap, 5)
Use linear decay of vwap multiplied by sharesout over the last 5 days as stock weights.
ts_decay_exp_window(close, 5, factor=0.25)
Use exponential decay of close with smoothing factor 0.25 over the last 5 days as stock weights.
ts_product(volume/sharesout, 5)
Use product of volume/sharesout ratio for the past 5 days as stock weights.
Tail(close/vwap, lower=0.9, upper=1.1, newval=1.0)
Use close/vwap ratio as stock weights if it is less than 0.9 or greater than 1.1, or otherwise use 1 as stock weights.
Sign(close-vwap)
Use 1 if close-vwap is positive or otherwise -1 as stock weights.
SignedPower(close-open, 0.5)
Use the signed sqrt of the absolute difference between close and open as stock weights.
Pasteurize(1/(close-open))
Use inverse of close-open pasteurized (set to NAN if it is INF or if the underlying instrument is not in the universe) as stock weights.
Log(high/low)
Use natural logarithm of high/low ratio as stock weights.
group_neutralize(volume*vwap, market)
Use market neutralized volume*vwap product as stock weights.
Scale(close^0.5)
Use scaled sqrt of close (scaled such that the booksize is 1) as stock weights.
Ts_Min(open, 22)
Use minimum open over the last 22 days as stock weights.
Ts_Max(close, 22)
Use maximum open over the last 22 days as stock weights.
Ts_Rank(volume, 22)
Use rank of current volume over the past 22 days as stock weights.
Ts_Skewness(returns, 11)
Use skewness of returns over the last 11 days as stock weights.
Ts_Kurtosis(returns, 11)
Use kurtosis of returns over the last 11 days as stock weights.
Ts_Moment(returns, 11, k=3)
Use 3rd central moment of returns over the last 11 days as stock weights.
ts_count_nans((close-open)^0.5, 22)
Use number of NAN values in (close-open)^0.5 for the past 22 days as stock weights.
ts_corr(close, ts_step(1), 5)
Use correlation between close and ts_step(1) for past 5 days as stock weights. It demonstrate right way to use ts_step()
Last_Diff_Value(sales, lookback=125)
Takes the last value of sales different from today's value of sales as stock weights, with max. lookback days = 125 days.
group_rank(returns, industry)
Takes Rank of returns within the industry group as stock weights.
group_mean(returns, volume, subindustry)
Use average returns of the subindustry group of each stock, weighted by volume, as stock weights.
Ts_Regression (close, open, 20, lag =0, rettype= 2)
From open[t] = a + b * close[t] where t in (Today - 20, Today)return b as stock weights.
```

