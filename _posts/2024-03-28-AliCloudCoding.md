---
dlayout:    post
title:      阿里云2025届算法笔试
subtitle:   coding to AliCloud
date:       2024-03-28
author:     Kylin
header-img: img/bg_NLPseg.jpg
catalog: true
tags:
    - job
---



[TOC]

### 3.28 算法

### 选择题

- 协同过滤的变体是：A

  A 矩阵分解

  B 关联规则

  C 决策树

  D 逻辑回归

- 5个人选3种裙子，某一种裙子被3个人选择的概率

- 冒泡排序的交换次数

- 双端队列的出队序列（后端可以进可以出，前端只能出）

- 最佳适应算法

> https://www.cnblogs.com/linhaostudy/p/12242850.html

- 矩阵乘法
- dropout是随机在一次forword中随机去掉一些神经元及其连接
- 某天,小明向区域$D((x,y)|x^2+y^2\leq 1)$内跳跃,落在其中任意一点,用(X,Y)表示该点的坐标,设该点落在D内任意小区域内的概率与该小区域的面积成正比,则关于X与Y的说法正确的是? （A）
  A 不相关且不相互独立
  B 不相关且相互独立
  C 相关且相互独立 
  D 相关且不相互独立

> https://www.math.pku.edu.cn/teachers/xirb/Courses/statprobB/psbd04.pdf 
>
> 例3.2



（不定项）

- 常见的记录式的有结构文件有：堆、顺序文件、索引顺序文件、索引文件和直接文件，下列叙述正确的有（ABCD） 

  A 堆是最简单的文件组织形式，堆的目的是积累大量数据并保存数据 

  B顺序文件是最常用的文件组织形式，这类文件中，每个记录都使用一种固定的格式，但是顺序文件的检索效率不高 

  C索引顺序文件中记录按照关键字的顺序组织 

  D直接文件使用基于关键字的散列，直接访问磁盘中任何一个地址已知的块

- 在生成对抗网络中，Mode Collapse是指什么现象？以下哪些说法是正确的？ （AD）

  A 生成器只学习生成数据分布的一部分模式 

  B 判别器对所有输入都给出相似的输出 

  C GAN模型无法收敛 

  D 生成器生成的样本缺乏多样性

- 在半监督学习中，哪些是可选的主动学习（Active Learning）的策略？ 

  A 不确定性抽样（Uncertainty Sampling） 

  B 边界抽样 (Boundary Sampling) 

  C 多样性抽样（Diversity Sampling） 

  D 基于聚类的抽样（Cluster-based Sampling） 

  E 信息熵抽样（Entropy Sampling）

- 给出完全二叉树的层次遍历，判断哪些是BST

- BN

  BN一般在Conv之后使用

  BN引入了模型复杂度

  BN对large model对效果显著

- 非齐次方程的两个解的问题

- 限制最大深度栈的出栈序列



### OJ

#### Q1

小苯有一个长为n的数组a，他定义一个数组的权值为：数组的中位数的值。大白熊希望小苯从数组a中选出k个数字，使得这k个数字组成的数组权值最大，请你帮帮小苯吧。

**输入描述**

输入包含两行。

第一行两个正整数n， （1≤ n≤10^5），表示数组a的长度和需要选择的数字个数。（保证k是奇数）

第二行n个正整数an（1≤ a_i ≤ 10^9），表示数组的元素值。

**输出描述**

输出包含一行一个正整数，表示选出k个数字组成数组的最大权值。

**示例 1**

**输入**

```
4 3
1 1 2 2
```

**输出**

```
2
```

##### A1

排序数一下，签到！

```python
n,k = map(int,input().split())
nums = list(map(int,input().split()))
nums.sort(key=lambda x:-x)
print(nums[k//2])
```



#### Q2

小红给了小苯一个长为n的数组a。

她希望小苯从数组a中选出一些数字，使这些数字构成一个新数组b，满足a中任意一对数字，都满足较大者被较小者整除，且较大者除以较小者的结果都是b中最小值的整数次幂。形式化的，若记b数组的最小值为 min，则对于任意$1≤i≤len(b)$，如果有$b_i≤b_j$，则必有$b_j\%b_i=0$且$b_j/b_i = min^k$，其中$k $是任意非负整数。（其中，b的下标从1开始，len(b)表示b的长度。）

小苯想知道，他最多能选多少个数字，换句话说，b的长度len最大能有多少，请你帮帮他吧。

**输入描述**

输入包含T＋1行。

第一行一个正整数T（1≤ T ≤ 10^3），表示测试数据组数。

接下来对于每组测试数据，输入包含两行。

第一行一个正整数 n（1≤ n ≤10^5），表示数组a的长度。

第二行n个正整数 a_i（1 ≤ a_i ≤10^9）。保证所有测试用例中，n的总和不超过10^5。

**输出描述**

输入包含T行，每行一个正整数表示每个测试用例对应的答案。

**示例1**

输入

```
2
5
1 2 3 4 5
4
2 2 2 2
```

输出

```
2
4
```



##### A2

hash一下，对每一个数字，累积幂次的出现次数，狠狠签到！

```python
from collections import Counter

t = int(input())
for i in range(t):
    n = int(input())
    nums = list(map(int,input().split()))
    cnt = Counter(nums)
    nums = set(nums)
    res = cnt[1]
    for num in nums:
        if num==1:
            continue
        tmp = num
        tmp_res=0
        while tmp<=10**9+7:
            tmp_res+=cnt[tmp]
            tmp*=num
        res = max(res,tmp_res)
    print(res)
```



#### Q3

小红拿到了一个大小为n的数组，她希望从数组中取恰好k个元素，满足这k个元素之和恰好是p的倍数。

小红想知道有多少种不同的取数方案，你能帮帮她吗？

**输入描述**

第一行输入三个正整数n,k,p，含义如题目描述所述。

第二行输入n个正整数a_i，代表小红拿到的数组。

1 ≤ n,p ≤ 100

1 ≤ k ≤ n

1≤ a_i ≤ 10^9

**输出描述**

一个整数，代表最终的取数方案数。由于答案可能过大，请对10^9 +7取模。

**示例1**

**输入**

```
3 2 3
1 2 1
```

**输出**

```
2
```



##### A3

递归，并且记录一个dp，不过python可以用cache签到！

```python
from functools import cache

n,k,p = map(int,input().split())
nums = list(map(int,input().split()))
mod = 10**9+7

@cache
def dfs(start,acc,m):
    res = 0
    if acc==k and m==0:
        res+=1
        return res%mod
    if start>=n:
        return 0
    res+=dfs(start+1,acc+1,(m+nums[start])%p)
    res+=dfs(start+1,acc,m)
    return res%mod

res = dfs(0,0,0)
print(res)
```



