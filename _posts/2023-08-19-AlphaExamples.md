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



### Data Field Search & operations

Data: https://platform.worldquantbrain.com/data/search

Operation: https://platform.worldquantbrain.com/learn/data-and-operators/operators#arithmetic-operators

https://platform.worldquantbrain.com/learn/data-and-operators/detailed-operator-descriptions

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

#### Concept

1. **Call Option（看涨期权）**：是一种金融合约，允许（但不要求）买方在某个特定日期或之前，以特定价格购买某个资产，如股票。
2. **Option Premium（期权溢价）**：购买期权的成本。其价值会受到许多因素的影响，如股票的当前价格、期权的执行价格、剩余时间和预期波动性等。
3. **Breakeven Price（盈亏平衡价格）**：这是股票需要达到的价格，以使期权持有者不亏损。对于看涨期权，这等于期权的执行价格加上期权溢价。
4. “mean reversion”： 均值回归，市场不会长期处于偏见状态。
5. **share vs.  option**:

- **股票**：代表公司的所有权份额。当你购买股票时，你实际上是购买了该公司的一小部分所有权。
- **期权**：是一个合约，给予买方权利（但不是义务）在未来的某个时间，以约定的价格买入或卖出一定数量的股票。

**Hypothesis**

call_option太高，在之后的时间步里会下降，因为均值回归。

> Use call implied breakeven price to gauge the demand of call options. If investors are bullish (看涨) on one stock, the demand for call option should go up, increasing call option's premium, and then breakeven price. Apply reversion in the returns of call implied breakeven price by capturing correlation of rank returns over time.

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



### **Operating earnings yield**

**Hypothesis**

Operating earning yield (OEY) refers to the amount of earnings from operating activities an investor can obtain for each dollar invested. Higher OEY implies higher returns for the same amount of invested capital thus representing an investment opportunity.

**Implementation**

Long stocks that witnessed higher-than-average OEY growth and short the others.

**Hint to improve the Alpha**

Can normalize the difference between a stock OEY growth and the average improve the performance?

```
oey_growth = ts_rank(mdf_oey, 63);
oey_growth - group_mean(oey_growth, 1, subindustry)
###################################
Region	Universe	Language	Decay	Delay	Truncation	Neutralization	Pasteurization	NaN Handling	Unit Handling
USA	TOP3000	Fast Expression	5	1	0.08	Subindustry	On	Off	Verify
```



### **R&D Expense**

**Hypothesis**

Companies that spend more money on R&D as a proportion of their revenue may outperform others in the future

**Implementation**

Companies are ranked based on R&D expense to revenue ratio

**Hint to improve the Alpha**

Can different neutralization setting improve the alpha performance?

```
rank(mdf_rds)
#################################
Region	Universe	Language	Decay	Delay	Truncation	Neutralization	Pasteurization	NaN Handling	Unit Handling
USA	TOP3000	Fast Expression	0	1	0.01	Subindustry	On	Off	Verify
```



### **High return on equity and assets utilization**

**Hypothesis**

Stocks with simultaneously high return on equity and assets utilization may out perform others .

**Implementation**

The rank of return on equity (ROE) is multiplied by rank of sales to assets (assets utilization).

**Hint to improve the Alpha**

Can different neutralization setting improve the alpha performance?

```
fam_roe_rank*rank(sales/assets)
#################################
Region	Universe	Language	Decay	Delay	Truncation	Neutralization	Pasteurization	NaN Handling	Unit Handling
USA	TOP3000	Fast Expression	0	1	0.01	None	On	Off	Verify
```



### **Price to Earnings ratio**

**Hypothesis**

A stock with increasing Price to Earnings ratio might be comparatively expensive now and thus the stock price will likely fall. Thus we short the stocks whose current price/earnings ratio is higher than the 2 years' moving average of the ratio and vice-versa.

**Implementation**

(mdf_pec) is current price/earnings ratio. To capture this idea, Ts_av_diff() operator subtracts the moving average of 2 years' price/earnings ratio from the today's ratio. Also, Rank() operator smooths the difference between current price/earnings ratio and the mean of this value, returning equally distributed values between 0 and 1.

**Hint to improve the Alpha**

Can you try using the vector_neut(x,y) operator so that your alpha is neutralized to one-year Information ratio.

```
-group_rank(ts_av_diff(mdf_pec,480),subindustry)
########################
Region	Universe	Language	Decay	Delay	Truncation	Neutralization	Pasteurization	NaN Handling	Unit Handling
USA	TOP3000	Fast Expression	0	1	0.01	Market	On	Off	Verify
```



### **No news is a good news**

**Hypothesis**

No news is good news. Stocks with lower sentiment volume and observing no significant sentiment recently may outperform others.

**Implementation**

Total sentiment volume over the past week is calculated and compared over the last quarter. The result is then multiplied with the ranking within a sector of the most significant sentiment observed.

**Hint to improve the Alpha**

Can different simulation settings improve the alpha?

```
sum_vol = ts_sum(vec_sum(scl12_alltype_buzzvec), 5);
significance = vec_norm (scl12_alltype_sentvec) / vec_count(scl12_alltype_sentvec);
sent_vol = -ts_rank(sum_vol, 60);
sent_sig = ts_max(significance, 10);
sent_vol * group_rank(sent_sig, sector)
########################
Region	Universe	Language	Decay	Delay	Truncation	Neutralization	Pasteurization	NaN Handling	Unit Handling
USA	TOP200	Fast Expression	0	1	0.01	Market	On	Off	Verify
```



### **High cashflow from operations to cover short term debt**

**Hypothesis**

Stocks of companies that have high cash flow from operations to cover short term debt tend to have low risk and thus market participants would prefer to buy this stock leading to potential future outperformance.

**Implementation**

Information ratio (IR) is the ratio between cash flow from operations and short term debt. If it’s calculated with a higher value that indicates a higher ratio average and/or higher stability. This result is then compared with other stocks in the market using the cross-sectional zscore operator.

**Hint to improve the Alpha**

Can the signal be improved by comparing this ratio only for companies which have positive cashflow from operations? Think why and how you can implement this using some operator.

```
zscore(ts_ir(cashflow_op/debt_st,1250))
########################
Region	Universe	Language	Decay	Delay	Truncation	Neutralization	Pasteurization	NaN Handling	Unit Handling
USA	TOP3000	Fast Expression	10	1	0.01	Subindustry	On	Off	Verify
```



### **Rising net profit margin**

**Hypothesis**

Net profit margin is associated with the profitability of a company, thus rising net profit margin would imply better business performance.

**Implementation**

Implementing ts_avg_diff operator to long stocks that are witnessing an increase in net profit margin when compared with its mean over the last two years and short the others.

**Hint to improve the Alpha**

Can different neutralization setting improve the alpha performance?

```
ts_av_diff(mdf_nps,500)
###########################
Region	Universe	Language	Decay	Delay	Truncation	Neutralization	Pasteurization	NaN Handling	Unit Handling
USA	TOP3000	Fast Expression	5	1	0.01	Market	On	Off	Verify
```



### **Put-Call Open Interest ratio (PCR-OI)**

#### Put-Call Open Interest Ratio (PCR-OI) 

Put-Call Open Interest Ratio (PCR-OI) 是衡量金融市场中看跌期权（Put options）和看涨期权（Call options）的开放利益量（或未平仓合约量）的指标。开放利益是指在某一特定时间尚未平仓的期权合约数量。

$PCR-OI =看跌期权的开放利益 / 看涨期权的开放利益$

#### Call Option and Put Option

1. **看涨期权 (Call Option)**
   - 给予持有者在未来某个特定日期或之前以特定价格买入资产的权利。
   - 买入一个看涨期权意味着你认为资产的价格将上升，因此你希望以现在约定的价格在未来买入。
   - 出售（或写入）一个看涨期权意味着你有义务在未来以约定的价格卖出资产，如果买方选择行使权利的话。
2. **看跌期权 (Put Option)**
   - 给予持有者在未来某个特定日期或之前以特定价格卖出资产的权利。
   - 买入一个看跌期权意味着你认为资产的价格将下降，因此你希望以现在约定的价格在未来卖出。
   - 出售（或写入）一个看跌期权意味着你有义务在未来以约定的价格买入资产，如果买方选择行使权利的话。

**Hypothesis**

Open Interest refers to the amount of active open positions in call and put options. Stocks with higher Put-Call Open Interest ratio (PCR-OI) might indicate a normal than usual on-going bearish sentiment for the stock and thus it might be poised for a reversal. Thus it is suggested to buy stocks with high PCR-OI.

**Implementation**

We directly use PCR-OI datafield as a signal to test our hypothesis.

**Hint to improve the Alpha**

In what universe would this strategy work best? Can increasing weight on stocks that have had high average volume help improve the signal?

```
pcr_oi_all
######################
Region	Universe	Language	Decay	Delay	Truncation	Neutralization	Pasteurization	NaN Handling	Unit Handling
USA	TOP200	Fast Expression	5	1	0.01	Market	On	Off	Verify
```



### **Growth in EPS and Sales**

#### Earnings Per Share，EPS，每股收益

*EPS* = (净利润−优先股股息)/普通股在外流通股数

**Hypothesis**

Companies with growing EPS indicates improving profitability. Buy more stocks of companies that have sales growth as well as eps growth as it signals overall strength in business performance.

**Implementation**

Use ts_avg_diff operator to compare current three-year eps growth (mdf_eg3) with it's average over one year. Use ts_corr operator to identify stocks with a combination of high eps growth and sales growth.

**Hint to improve the Alpha**

Can expanding the lookback period improve the alpha?

```
ts_av_diff(mdf_eg3, 250)*ts_corr(mdf_eg3, mdf_sg3, 250)
##############################
Region	Universe	Language	Decay	Delay	Truncation	Neutralization	Pasteurization	NaN Handling	Unit Handling
USA	TOP200	Fast Expression	0	1	0.01	Subindustry	On	Off	Verify
```



### **CAPM**

**Hypothesis**

Outperforming stocks would continue to outperform. If a stock's actual return exceeds its expected return using the CAPM model, it would continue to perform well in the future.

**Implementation**

According to Capital Asset Pricing Model (CAPM), a stock's expected return is equal to risk-free-rate (Rf) plus its beta (β) multiplied by the market risk premium (Rm-Rf)

**Hint to improve the Alpha**

Can different simulation settings improve the alpha?

```
market_ret = ts_product(1+group_mean(returns,1,market),250)-1;
rfr = vec_avg(fnd6_newqeventv110_optrfrq);
expected_return = rfr+beta_last_360_days_spy*(market_ret-rfr);
actual_return = ts_product(returns+1,250)-1;
actual_return-expected_return
############################
Region	Universe	Language	Decay	Delay	Truncation	Neutralization	Pasteurization	NaN Handling	Unit Handling
USA	TOP1000	Fast Expression	5	1	0.01	Subindustry	On	Off	Verify
```



### **High earnings**

**Hypothesis**

Buy more of stocks with a higher earnings yield rank in the sector.

**Implementation**

Using group_rank operator, earning yield rank of a stock is compared against other companies within the same sector.

**Hint to improve the Alpha**

Can you try a different group to improve the alpha performance?

```
group_rank(fam_est_eps_rank, sector)
#################################
Region	Universe	Language	Decay	Delay	Truncation	Neutralization	Pasteurization	NaN Handling	Unit Handling
USA	TOP3000	Fast Expression	0	1	0.01	Subindustry	On	Off	Verify
```



### **Pay back the debt**

**Hypothesis**

It is usually safer to go long on companies that can easily pay back the short term debt using high liquid assets.

**Implementation**

Zscore of the ratio between cash and short term debt is calculated with higher readings refer to higher ratio when compared with the market.

**Hint to improve the Alpha**

Try comparing a stock with its peers instead of the whole market.

```
group_rank(zscore(cash_st/debt_st),subindustry)
########################
Region	Universe	Language	Decay	Delay	Truncation	Neutralization	Pasteurization	NaN Handling	Unit Handling
USA	TOP500	Fast Expression	0	1	0.01	Subindustry	On	Off	Verify
```



### ***News release**

**Hypothesis**

If the deviation of percent changes between the price at the time of the news release to the price at the close of the session (nws12_prez_results) in a day is large, there may exist an investment opportunity.

**Implementation**

If percent change in a day is large it might imply high risk. Thus, we should short the stocks with high risk.

**Hint to improve the Alpha**

Use trade_when operator to lower your turnover.

```
percent = ts_rank(vec_stddev(nws12_prez_result2),50);
-ts_rank(ts_decay_linear(percent,150),50)
##########################
Region	Universe	Language	Decay	Delay	Truncation	Neutralization	Pasteurization	NaN Handling	Unit Handling
USA	TOP3000	Fast Expression	20	1	0.01	Market	On	Off	Verify
```



### **EBIT vs CAPEX**

#### EBIT

**EBIT (Earnings Before Interest and Taxes)** - 利息和税前盈利

- EBIT 是一家公司从其主要或常规营业活动中获得的总收入减去运营费用（但在扣除利息和税之前）的利润。
- 它通常用来度量公司的运营性能，因为它排除了由资本结构（如债务和利息支出）和税务状况产生的影响。
- EBIT 也经常被称为“运营利润”或“运营收益”。

#### CAPEX

**CAPEX (Capital Expenditures)** - 资本支出

- CAPEX 是公司为购买、升级、改进其长期资产（如物业、工厂、技术或设备）所做的支出。
- 与营运支出 (OPEX) 不同，CAPEX 通常是为期多年的大额投资，其价值在其使用寿命期间逐年折旧。
- CAPEX 可以被视为公司为确保其未来增长或维持其现有运营水平所进行的投资。

**Hypothesis**

Stocks with higher EBIT compared to CapEx can be a sign of the company not investing much in growth and the stock may not grow as much, thus we should sell those stocks.

**Implementation**

CapEx (capital expenditure) is money spent on acquiring/maintaining fixed assets. EBIT, which is a company's net income before interest and taxes, and short stocks with inadequate CapEx.

**Hint to improve the Alpha**

Can different neutralization settings improve the alpha performance?

```
-rank(ebit/capex)
##################
Region	Universe	Language	Decay	Delay	Truncation	Neutralization	Pasteurization	NaN Handling	Unit Handling
USA	TOP3000	Fast Expression	0	1	0.01	Sector	On	Off	Verify
```



### **Retained earning**

**Hypothesis**

Retained earnings are the cumulative net earnings of a company after dividend payments to the shareholders. In certain situations, shareholders might want to receive their dividends and realize their profit. Thus, some shareholders might not be happy with excessive retained earnings.

**Implementation**

We should long the stock that has its retained earnings decreasing and vice-versa.

**Hint to improve the Alpha**

Can different neutralization setting improve the alpha performance?

```
-ts_rank(retained_earnings,250)
################################
Region	Universe	Language	Decay	Delay	Truncation	Neutralization	Pasteurization	NaN Handling	Unit Handling
USA	TOP3000	Fast Expression	10	1	0.01	Subindustry	On	Off	Verify
```



### ***Attention**

**Hypothesis**

Too much attention towards a stock usually followed by overreaction in its stock price.

**Implementation**

You should trade top 5% highest sentiment volume stocks by longing/shorting stocks with negative/positive sentiment, respectively.

**Hint to improve the Alpha**

Volumes of all kinds usually face the problem of outliners, try smoothing the data for better alpha performance.

```
sent_vol = vec_sum(scl12_alltype_buzzvec);
trade_when(rank(sent_vol)>0.95,-zscore(scl12_buzz)*sent_vol,-1)
###########################
Region	Universe	Language	Decay	Delay	Truncation	Neutralization	Pasteurization	NaN Handling	Unit Handling
USA	TOP3000	Fast Expression	0	1	0.01	Subindustry	On	Off	Verify
```



### ***Total Risk of a stock**

**Hypothesis:**

Since we are long-short neutral in our alpha, we can further minimize our systemtic risks exposure, thus place long positions on alphas that have more total risk and consequently have possibility to generate more returns.

**Implementation :**

To keep the inherent distribution of total risks intact, we use the zscore operator to place weights on our alpha.

**Hint to improve the Alpha**

Since we are creating an alpha on risk of individual stocks, the alpha may perform well if we filter stocks that are highly correlated to the index. Can you improve the alpha using trade_when operator using the 'correlation_last_60_days_spy' datafield?

```
SR = systematic_risk_last_60_days;
USR = unsystematic_risk_last_60_days;

zscore(USR + SR)
#########################
Region	Universe	Language	Decay	Delay	Truncation	Neutralization	Pasteurization	NaN Handling	Unit Handling
USA	TOP3000	Fast Expression	0	1	0.01	Subindustry	On	Off	Verify
```



### ***Customers' Web Ranking**

**Hypothesis:**

The higher the instrument's customers rank on PageRank, HITS hub and HITS authority, the more likely these customers would grow in future, which in turn results in growth for the instrument too.

**Implementation :**

Derive an overall ranking of the instruments' customers from the three types of ranking. Focus more on instruments with higher number of customers in each sector, because these instruments are likely to be the larger players in that sector and have more customers with web rankings.

**Hint to improve the Alpha**

This alpha might not perform as well with illiquid stocks (TOP3000 universe), because these stocks, and in turn their customers, would be less well known and would rely less on web rankings. Can this alpha give better performance in liquid universe? (TOP 500 universe)

```
alpha = 
rank(-pv13_ustomergraphrank_page_rank) 
* rank(-pv13_ustomergraphrank_hub_rank) 
* rank(-pv13_ustomergraphrank_auth_rank);

group_rank(rel_num_cust,sector)>0.7?alpha:alpha*0.5
########################
Region	Universe	Language	Decay	Delay	Truncation	Neutralization	Pasteurization	NaN Handling	Unit Handling
USA	TOP500	Fast Expression	0	1	0.01	Subindustry	On	Off	Verify
```



