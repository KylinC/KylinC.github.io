---
dlayout:    post
title:      Intro To WQ and Factor Fundamentals
subtitle:   WQ介绍和因子投资基础
date:       2023-8-18
author:     Kylin
header-img: img/sparse.jpg
catalog: true
tags:
    - quant
---



[TOC]



量化回测：利用大量历史数据，模拟多种投资组合

回测指标：年化收益、Sharpe、最大回撤



### 量化交易发展历史

- 早期阶段。20世纪50年代，美国商品期货市场开始采用电子交易平台。从60年代开始，随着计算机和数据技术的快速发展，量化交易开始在不同的商业应用领域得到广泛应用。

- 1980年代。从80年代开始，量化交易在金融领域开始得到广泛应用，并受到各大金融机构的重视。早期的 交易系统以技术指标分析为基础，随着数据分析和算法等技术的不断发展，量化交易模型不断升级。
- 1990年代。随着互联网的发展，金融建模工具和交易平台的使用不断普及，量化交易被越来越多的机构和个人采用。这个阶段出现了各种自动化交易系统、智能匹配算法和更加高效的交易策略。
- 2000年以后。随着大数据、机器学习、深度神经网络等技术的快速发展，量化交易进入了新的阶段。交易策略的多样化和高频率交易的规模化应用，成为量化交易发展的重要特征。



>  A股的散户比例很高，远超美股。



 ### 股票数据

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F12023-08-19%2015.25.02.png" alt="截屏12023-08-19 15.25.02" style="zoom: 33%;" />



<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-08-19%2015.31.24.png" alt="截屏2023-08-19 15.31.24" style="zoom:33%;" />

配股：10股配10股，使得股价降一半

分红：赚钱之后，分钱给股东，需要调整股价



<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-08-19%2015.36.07.png" alt="截屏2023-08-19 15.36.07" style="zoom:33%;" />

财务粉饰：把财报搞好看一点

财务造假：完全瞎搞

每一个行业的股票都有其特有的数字特征：万科重资产、科技公司重研发



<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-08-19%2015.41.51.png" alt="截屏2023-08-19 15.41.51" style="zoom:33%;" />

九坤在做指数增强（相比指数收益）



<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-08-19%2015.43.05.png" alt="截屏2023-08-19 15.43.05" style="zoom: 33%;" />



<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-08-19%2015.44.51.png" alt="截屏2023-08-19 15.44.51" style="zoom:33%;" />

期权、期货、正股？



<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-08-19%2015.46.43.png" alt="截屏2023-08-19 15.46.43" style="zoom:33%;" />

- Wind数据是最全的，可以把非结构化数据转结构化数据
- akshare和tushare差不多



<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-08-19%2015.49.33.png" alt="截屏2023-08-19 15.49.33" style="zoom:33%;" />



财务数据一个季度更新一次，最后一个季度和年报更新



<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-08-19%2015.53.50.png" alt="截屏2023-08-19 15.53.50" style="zoom:33%;" />



<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-08-19%2015.56.54.png" alt="截屏2023-08-19 15.56.54" style="zoom:33%;" />

涨停的股票第二天涨的概率大概60%

买入资金量：单天5%～10%





### WQ平台

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-08-19%2016.04.10.png" alt="截屏2023-08-19 16.04.10" style="zoom: 33%;" />

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-08-19%2016.04.30.png" alt="截屏2023-08-19 16.04.30" style="zoom:33%;" />

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-08-19%2016.05.19.png" alt="截屏2023-08-19 16.05.19" style="zoom:33%;" />

加杠杆



### Backtrader介绍

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-08-19%2016.06.46.png" alt="截屏2023-08-19 16.06.46" style="zoom:30%;" />

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-08-19%2016.07.54.png" alt="截屏2023-08-19 16.07.54" style="zoom:33%;" />



<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-08-19%2016.08.34.png" alt="截屏2023-08-19 16.08.34" style="zoom:30%;" />



### 量化回测指标

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-08-19%2016.10.33.png" alt="截屏2023-08-19 16.10.33" style="zoom:33%;" />

PnL是最重要的，WQ是用Return（但是除以一半本金）



