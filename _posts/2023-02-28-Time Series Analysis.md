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

### Linear Regression

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

### R-Squared





