---
dlayout:    post
title:      美团2025届算法面筋
subtitle:   interview to MT
date:       2024-03-26
author:     Kylin
header-img: img/bg_NLPseg.jpg
catalog: true
tags:
    - job
---



[TOC]

### 一面

bert结构，用于下游nlp任务细节

decode only和encoder-decoder结构的区别

mllm的结构

xgboost的原理

xgboost和lgbm的区别

bert和大语言模型哪个好？why？

了解哪些mllm的benchmark？

手撕：最大乘积子数组，dp O(n)过



### 二面

多模态LLM细节全部讲一次：输入输出是哪些？上线细节，有在线测评指标吗？（这一个问题聊了好久）

transformer，q，k，v 分别有什么作用

q，k，v转换只用一个w_qkv行不？

> 参考[^1] 

手撕：实现一个采样函数，
输入：vec = [(1, 0.5), (2, 0.1), (3, 0.4)]，即按照0.5概率采样值为1，0.1概率为2，0.4概率为3。

计算F(x)，二分查找，O(n)过，不算计算F(x)过程O(lgn)



```python
from types import new_class
import random

vec = [(1, 0.5), (2, 0.1), (3, 0.4)]
def choice(vec):
    p = random.uniform(0,1)
    accum = 0
    for num,p_now in vec:
        accum+=p_now
        if p<=accum:
            return num

def choice(vec):
    p = random.uniform(0,1)
    new_vec = []
    accum = 0
    for num,p_now in vec:
        accum+=p_now
        new_vec.append((accum,num))
    left,right = -1,len(new_vec)
    while left+1<right:
        mi = (left+right)>>1
        if new_vec[mi][0]<p:
            left=mi
        else:
            right=mi
    return new_vec[right][1]
```



### Reference

[^1]: Transformer中K 、Q、V的设置以及为什么不能使用同一个值. https://www.cnblogs.com/jins-note/p/14508523.html
