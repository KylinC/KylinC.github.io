---
dlayout:    post
title:      拼多多2025届算法笔试
subtitle:   coding to PDD
date:       2024-03-24
author:     Kylin
header-img: img/bg_NLPseg.jpg
catalog: true
tags:
    - job
---



[TOC]

### 3.24 算法

#### Q1

这里有n个正整数，a1,....,an

Alice 会先去掉其中最多d 个数

Bob 接下来会将剩余的数中最多m个数乘以 -k

Alice 想要剩余数之和尽可能大，Bob 想要剩余数之和尽可能小。假设 Alice 和 Bob 都足够聪明，请问最后剩余数之和是多少。

**输入描述**

第一行一个正整数T，接下来有T组数据

每组数据2行

第一行4 个数

n, m, k, d (2 ≤ n ≤ 10^5) (0 ≤ m, d ≤ n) (1 ≤ k < 10^4)

第二行n个数，a1,a2,..., an (1 ≤ ai ≤ 10^9)

保证sigma n不超过10^5

**输出描述**

每组数据输出一行，每行一个数，表示剩余数之和

##### A1

排序，滑动窗口解决，为啥有滑动窗口这个结论是因为有一个单调性：Alice和Bob都倾向选大端的数！

```python
import sys

t = int(input())
for i in range(t):
    n,m,k,d = map(int,input().split())
    nums = list(map(int,input().split()))
    nums.sort(key=lambda x:-x)
    tmp = -k*sum(nums[:m])+sum(nums[m:])
    res = tmp
    for i,num in enumerate(nums):
        if i>=d:
            break
        tmp+=k*nums[i]
        tmp-=(k+1)*nums[i+m] if i+m<n else 0
        res = max(res,tmp)
    print(res)
```

#### Q2

伊文有两个由0和1组成的字符串，A和B，长度分别为m和n（m>=n）。伊文希望取出A的连续子串与B构造若干长度为n的S串，满足：

- $S_{i}=A_{x+i}\oplus B_{i}, i\in[1,n], x\in[O,m-n]$
- ﻿﻿$S1\oplus S2\oplus S3\oplus ...\oplus S_{n-1}\oplus S_n=0$

伊文想知道总共能够构造出多少个不同的S串。

**输入描述**

输入有三行，第一行2个数m和n，为A和B的长度；

m,n （0 <n≤m≤5 x10^3）

第二行为长度为m的A串

第三行为长度为 的B串，A和B仅由'0'和'1'组成

**输出描述**

输出：一个数字，代表不同的S串个数

##### A2

签到题在第二题，这个数据量级直接暴力了

```python
import sys

m,n = map(int,input().split())
a = input()
b = input()

def xor(s1,s2):
    res = ""
    cnt = 0
    for a,b in zip(s1,s2):
        if (a=='1' and b=='0') or (b=='1' and a=='0'):
            res+='1'
            cnt^=1
        else:
            res+='0'
            cnt^=0
    return cnt,res

s = set()
for i in range(m-n+1):
    cnt,res = xor(a[i:i+n],b)
    if cnt:
        continue
    if res not in s:
        s.add(res)
print(len(s))
```



#### Q3

多多快递站共有n个快递点，n个快递点之间通过m条单向车道连接。快递员从任何一个快递站点出发，都无法通过单向车道回到该站点。也就是说，n个快递点组成一张有向无环图。对于快递点u，如果对于所有的快递点v（u != w），快递员都可以从w走到v，或者从v走到u，那么则评定站点u为超级快递点。

请你帮忙算一算，一共有多少个超级快递点。

**输入描述**

第一行2个数字n（2≤n≤3\*10^5）,m（1≤m ≤3\*10^5），n为快递点个数，m为单向车道个数。

接下来的m行每行两个数字u,v (1<=u,v<=n,u!=v)，表示有一条站点么指向0的单向车道。

**输出描述**

请输出1个数字，表示超级快递点的个数。

##### A3

DFS暴力ac不了不献丑了



#### Q4

多多有一个长度为n的字符串，这个字符串由26个小写字母组成.

多多可以对这个字符串进行多次操作，每次操作可以把该字符串中一段连续的回文子串删除（单个字符也属于回文串），删除后剩下的串会拼在一起.

请问最少需要多少次操作可以将这个字符串删光.

**输入描述**

第一行，包含一个正整数T（1≤T≤20）代表测试数据的组数.

对于每组测试数据，仅有一行，代表这个字符串.（1≤n≤ 500）

保证\sigma n 不超过3000

**输出描述**

对于每组数据输出一行整数，代表多多在进行最少多少次操作后，可以将这个字符串删光.

##### A4

dp\[i\]\[j\]代表最小操作数，从小到大枚举区间，并枚举区间分割点。

```python
import sys

t = int(input())
for i in range(t):
    s = input()
    n = len(s)
    dp = [[n]*n for _ in range(n)]
    for i in range(n):
        dp[i][i] = 1
    for k in range(1, n):
      for j in range(0,n-k):
        if s[j] == s[j+k]:
          if k == 1:
            dp[j][j+k] = 1
          else:
            dp[j][j+k] = dp[j+1][j+k-1]
        for i in range(j, j+k):
          dp[j][j+k] = min(dp[j][j+k], dp[j][i] + dp[i+1][j+k])
    print(dp[0][n-1])
```







