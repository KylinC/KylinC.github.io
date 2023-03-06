---
dlayout:    post
title:      A Intro to Quantitative Trading: Time Series Analysis
subtitle:   Time Series Analysis
date:       2023-2-28
author:     Kylin
header-img: img/bg-quantitive-trading-intro.jpg
catalog: true
tags:
    - machine learning
---



[TOC]



# A Intro to Quantitative Trading: Time Series Analysis



## Correlation & Autocorrelation



### Correlation of percent changes

> 有的时候，直接计算两个序列的相关性是不可靠的。比如算Dow Jones和UFO sightings的时间序列相关可以得到0.94。所以我们得算correlation with percent changes。即计算差分的相关性，而不是实际数值的相关性。
>
> <img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-02-28%2009.48.07.png" alt="截屏2023-02-28 09.48.07" style="zoom:50%;" />
>
> ```python
> # Compute correlation of levels
> correlation1 = levels['DJI'].corr(levels['UFO'])
> print("Correlation of levels: ", correlation1)
> 
> # Compute correlation of percent changes
> changes = levels.pct_change()
> correlation2 = changes['DJI'].corr(changes['UFO'])
> print("Correlation of changes: ", correlation2)
> ```
>
> 在金融上，correlation_pct计算的是收益（价格的差分）相关性，而不是价格的相关性。

```
df['A'] = df['A'].pct_change()
df['B'] = df['B'].pct_change()

plt.scatter(df['A'],df['B'])
plt.show()

correlation = df['A'].corr(df['B'])
```

### * Linear Regression

- Simple linear regression:

$$
y_t=\alpha+\beta x_t+\epsilon_t
$$

**Odinary Least Squares (OLS)**：最小二乘法

- Pakages:

In statsmodels:

```
import statsmodels.api as sm
sm.OLS(y, x).fit()
```

In numpy:

```
np.polyfit (x, y, deg=1)
```

In pandas:

```
pd.ols(y, x)
```

In scipy:

```
from scipy import stats
stats.linregress(x, y)
```

- eg.

```
import statsmodels.api as sm

df['A_ret']=df['A'].pct_change()  // 注意返回的第一行是NaN，原因是没有差分
df['B_ret']=df['B'].pct_change()

df = sm.add_constant(df)
df = df.dropna()

results = sm.OLS(df['A_ret'],df[['const','B_ret']]).fit()

print(results.summary())

// intercept in results.params[0]
// slope in results.params[1]
```

> 为什么要加一个常数“1”列？
>
> You need to add a column of ones as a dependent, right hand side variable. The reason you have to do this is because the regression function assumes that if there is no constant column, then you want to run the regression without an intercept. By adding a column of ones, statsmodels will compute the regression coefficient of that column as well, which can be interpreted as the intercept of the line. The statsmodels method "add constant" is a simple way to add a constant.

### R-Squared & Correlation

> R方和相关系数的关系：

$$
\begin{aligned}
& {[\operatorname{corr}(x, y)]^2=R^2 \text { (or R-squared) }} \\
& \operatorname{sign}(\text { corr) }=\operatorname{sign}(\text { regression slope) }
\end{aligned}
$$



### Autocorrelation

> 自相关：单个时间序列与其自身滞后副本的相关性 = serial correlation

通常说autocorrelation，一般是lag-one correlation，就是滞后一个数据的correlation

- 负自相关：Mean Reversion（均值回归）

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-03-06%2017.02.53.png" alt="截屏2023-03-06 17.02.53" style="zoom:43%;" />

- 正自相关：Momentum，Trend Following（趋势跟踪）

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-03-06%2017.03.06.png" alt="截屏2023-03-06 17.03.06" style="zoom:33%;" />

- 对冲策略

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-03-06%2017.10.08.png" alt="截屏2023-03-06 17.10.08" style="zoom:50%;" />

```python
# Convert index to datetime
df. index = pd.to_datetime(df.index)
# Downsample from daily to monthly data
df = df.resample(rule='M').last()
# Compute returns from prices
df['Return'] = df ['Price'].pct_change()
# Compute autocorrelation
autocorrelation = df['Return'].autocorr()
print("The autocorrelation is: ",autocorrelation)  # 0.0567
```

- over long horizons, when interest rates go up, the economy tends to slow down, which consequently causes interest rates to fall, and vice versa.



##### Sample Autocorrelation function（ACF）

> ACF shows not only the lag-one autocorrelation from the last chapter, but the entire autocorrelation function for different lags. 
>
> **Any significant non-zero autocorrelations implies that the series can be forecast from the past.**

sample 1: 下图显示出可以用前两个值预测下一个值（类似fib），因为lag 1和lag 2的自相关系数不为0（说明有自相关，值越大自相关越强）

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/22.png" alt="22" style="zoom:33%;" />

sample 2: 季度性自相关

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-03-06%2019.33.56.png" alt="截屏2023-03-06 19.33.56" style="zoom:63%;" />

- plot ACF

```
from statsmodels.graphics.tsaplots import plot_acf

plot_acf(x,lags=20,alpha=0.05)
// lags表明绘制0～20的自相关，alpha表示置信区间

// 计算值而不是画图
from statsmodels.tsa.stattools import acf
print(acf(x))
```

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-03-06%2019.50.46.png" alt="截屏2023-03-06 19.50.46" style="zoom:53%;" />



## Simple Time Series

### White Noise

- White Noise except Gaussian:
  - Constant mean
  - Constant variance
  - Zero autocorrelation at all lags
- Gaussian White Noise:
  - Normal distribution

```
# 生成白噪声
import numpy as np
noise = np.random.normal(loc=0,scale=1,size=500) # 均值loc为0，方差scale=1

plot.plot(noise)
plot_acf(noise,lags=50)
```

股票returns非常接近白噪声



### Random Walk

- Random Walk：

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/2%E5%BE%B7%E5%BE%B712%E4%B8%BA.png" alt="2德德12为" style="zoom:50%;" />

- Random Walk With Drift：

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-03-06%2020.25.29.png" alt="截屏2023-03-06 20.25.29" style="zoom:50%;" />

#### Dickey-Fuller Test

> 测试时间序列是否是随机游走模型

- form 1

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-03-06%2021.36.55.png" alt="截屏2023-03-06 21.36.55" style="zoom:50%;" />

- form 2

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-03-06%2021.37.21.png" alt="截屏2023-03-06 21.37.21" style="zoom:50%;" />

当右侧的滞后价格多于1个的时候，叫做 Augmented Dickey-Fuller Test

```
from statsmodels.tsa.stattools import adfuller

results = adfuller(df['SPX'])

# print the p-value
print(results[1])

# p-value 小于0.05，则可以以95%的置信度拒绝序列是随机游走的原假设
```



#### Stationarity

> 平稳性

- What's Stationarity

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-03-06%2022.01.02.png" alt="截屏2023-03-06 22.01.02" style="zoom:50%;" />

- 平稳的序列容易建模，参数少；而参数随时间变化的不容易建模
- unstationary 的序列可以经过转化变为 stationary的序列

sample 1:

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-03-06%2022.06.47.png" alt="截屏2023-03-06 22.06.47" style="zoom:50%;" />

sample 2:

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-03-06%2022.06.54.png" alt="截屏2023-03-06 22.06.54" style="zoom:48%;" />

sample 3:

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-03-06%2022.07.47.png" alt="截屏2023-03-06 22.07.47" style="zoom:50%;" />



## Autoregressive (AR) Model

### AR(k) model

- Methematical Description

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-03-06%2022.21.29.png" alt="截屏2023-03-06 22.21.29" style="zoom:50%;" />

$\phi>0$  则是 momentum 模型，$\phi<0$ 则是 Mean Reversion模型

- exponential decay of autocorreletion 

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-03-06%2022.26.56.png" alt="截屏2023-03-06 22.26.56" style="zoom:57%;" />
$$
auto\_correlation(t)=\phi^{t}
$$

- Higher Order AR models

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/HOARmodels.png" alt="HOARmodels" style="zoom:50%;" />

```
from statsmodels.tsa.arima_process import ArmaProcess
ar=np.array([1,-0.9])
ma=np.array([1])
AR_object = ArmaProcess(ar, ma)
simulated_data = AR_object.generate_sample(nsample=1000)
plt.plot(simulated_data)
```



### Estimating and Forecasting AR model

- Estimating

```
from statsmodels.tsa.arima.model import ARIMA
mod = ARIMA(data,order=(1,0,0)) // order = (p,d,q), 代表ar步长、d代表差分阶、q代表ma
result = model.fit()
print(result.summary())

print(result.params)  //返回的第一个值是\mu,第二个值是\phi
```

- Forecating

```
from statsmodels.graphics.tsaplots import plot_predict
fig, ax = plt.subplots()
data.plot(ax=ax)
plot_predict(result, start='2012-09-27',end='2012-10-16',alpha=0.05)
plt.show()
```











































































