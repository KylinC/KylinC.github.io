---
dlayout:    post
title:      An Intro to Financial Trading
subtitle:   Breif Introduction 
date:       2023-03-10
author:     Kylin
header-img: img/bg-quantitive-trading-intro.jpg
catalog: true
tags:
    - quant
---



[TOC]



### Trading Basics



#### what is financial trading?

> Financial trading is the buying and selling of financial assets, also called financial securities. 

- financial instruments:
  - **equities**: shares of stocks representing ownership of companies	
  - **bonds**: debt instruments issued by the government or corporations
  - **forex or foreign exchange market of currencies**
  - **commodities** such as gold, silver, and oil
  - **cryptocurrencies** like Bitcoin



- why people trade?

going long/**long position** (做多): profit from upward price movement

eg. 低买高卖

going short/**short position** (做空):  profit from downward price movement 

eg. 高价抛售（借来的）股票，在低价时再买入



- what's market participants?

institution traders: 对冲风险、平衡资产配置

retail traders: 散户

- Different types of traders:

<img src="https://kylinhub.oss-cn-shanghai.aliyuncs.com/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202023-03-10%20223721.png" alt="屏幕截图 2023-03-10 223721" style="zoom:100%;" />



- Trading vs. Investing

<img src="https://kylinhub.oss-cn-shanghai.aliyuncs.com/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202023-03-10%20232222120.png" alt="屏幕截图 2023-03-10 232222120" style="zoom:87%;" />



- What's technical indicators: various types of data transformations



#### Manipulation with Python

- resample the data

<img src="https://kylinhub.oss-cn-shanghai.aliyuncs.com/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202023-03-10%20224201.png" alt="屏幕截图 2023-03-10 224201" style="zoom:80%;" />



- pct_change() 计算行变化百分比

```
stock_data['daily_return']=stock_data['CLose'].pct_change()*100
```



- candlestick chart

<img src="https://kylinhub.oss-cn-shanghai.aliyuncs.com/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202023-03-10%20222249.png" alt="屏幕截图 2023-03-10 222249" style="zoom:87%;" />

```
import plotly.graph_objects as go

candlestick = go.Candlestick(
	x = bitcoin_data.index,
	open = bitcoin_data['Open'],
	high = bitcoin_data['High'],
	low = bitcoin_data['Low'],
	close = bitcoin_data['Close']
)

fig = go.Figure(data=[candlestick])
fig.show()
```



- Sample moving average (SMA) : 指定滑动窗口内数据的聚合平均值

```
stock_data['sma_50']=stock_data['Close'].rolling(window=50).mean()
```



#### Backtesting (回测)

>  the bt package gives a flexible framework for defining and backtesting trading strategies

<img src="https://kylinhub.oss-cn-shanghai.aliyuncs.com/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202023-03-10%20230049.png" alt="屏幕截图 2023-03-10 230049" style="zoom:100%;" />



- The bt process

![屏幕截图 2023-03-10 230216](https://kylinhub.oss-cn-shanghai.aliyuncs.com/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202023-03-10%20230216.png)



```
import bt

# get the data
# 默认根据股票代码从yahoo财经下载“调整后的收盘价”
bt_data = bt.get('goog, amzn, tsla', start='2020-6-1', end='2020-12-1')

# define our strategy
bt_strategy = bt.Strategy('Trade_Weekly', # startegy's name
[bt.algos.RunWeekly(), #run weekly 交易频率
bt.algos.SelectAll(), # use all data 使用哪些数据
bt.algos.WeighEqually(), # maintain euqal weights 多种资产配置下，每种资产的权重（持股）
bt.algos.Rebalance()]) # 重平衡资产权重

# backtest
bt_test = bt.Backtest(bt_strategy,bt_data)

# run
bt_res = bt.run(bt_test)

# plot
bt_res.plot(title="backtest result")

# get trade details
bt_res.et_transactions()
```



### Trading Indicators

#### Indicators Basics

- What's technical indicators?

![屏幕截图 2023-03-10 234311](https://kylinhub.oss-cn-shanghai.aliyuncs.com/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202023-03-10%20234311.png)

- Types of indicators:

![屏幕截图 2023-03-10 235439](https://kylinhub.oss-cn-shanghai.aliyuncs.com/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202023-03-10%20235439.png)

- The TA-Lib package

```
import talib
```



#### SMA (Simple Moving Average)

> 随价格变化，平滑数据之后可以指示价格变化的方向



#### EMA (Exponential Moving Average)

> 随价格变化，平滑数据之后可以指示价格变化的方向



#### ADX



#### RSI



#### Bollinger Bands







