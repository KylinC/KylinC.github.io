---
dlayout:    post
title:      幻方2025届算法面筋
subtitle:   Interview to DeepSeek
date:       2024-04-17
author:     Kylin
header-img: img/swapdp.jpg
catalog: true
tags:
    - paper
---



[TOC]

### 一面

实习经历深挖

lora初始化方法

kv cache计算方法

```
bs*layer*sequence*hidden_dim*parameter_size*2
```

vllm可以做重参数化吗？

预训练权重在vllm框架下使用需要做哪些事情？

手撕：layernorm怎么写？

```python
def layernorm(hidden_states,belta,gamma):
    b,s,h = hidden_states.shape
    eps = 1e-5
    for i in b:
        for j in s:
            meanv = np.mean(hidden_states[i,j,:])
            stdv = np.std(hidden_states[i,j,:])
            hidden_states[i,j,:] = belta*(hidden_states[i,j,:]-meanv)/(stdv+eps)+gamma
```





### Reference

[^1]: 











