---
dlayout:    post
title:      KV Cache Optimization
subtitle:   KV Cache Reading Sheet
date:       2023-10-29
author:     Kylin
header-img: img/VIT.jpg
catalog: true
tags:
    - nlp
---



[TOC]

### Why KV cache?

> refer to https://kylinchen.cn/2023/08/21/KVCache/

需要注意的是，KV cache本身也是一种优化策略，用缓存Key Value（空间）避免重复计算（时间），但是现在发现这个KV cache实在太大了，尤其是在Serving System了里面，轻松达到模型参数占用4倍。

所以优化一般分为两个方向：

- 储存优化（主要）
- 计算优化



### Attention 机制改进

#### 减少Head

- [Fast Transformer Decoding: One Write-Head is All You Need](https://arxiv.org/abs/1911.02150)

MQA (Multi Query Attention，多查询注意力) 是多头注意力的一种变体。**MQA不同的注意力头共享一个K和V的集合，每个头只单独保留了一份查询参数 **。支持对已经训练好的模型进行微调来添加MQA。

- [GQA: Training Generalized Multi-Query Transformer Models from Multi-Head Checkpoints](https://arxiv.org/pdf/2305.13245.pdf)

保留了一定数量的KV-cache，使得其在MHA和MQA之间平衡。

![image-20231010231326213](https://kylinhub.oss-cn-shanghai.aliyuncs.com/image-20231010231326213.png)



#### Window Attention

![image-20231010232527341](https://kylinhub.oss-cn-shanghai.aliyuncs.com/image-20231010232527341.png)

LongFormer

- [Efficient Streaming Language Models with Attention Sinks](https://arxiv.org/abs/2309.17453)

> arxiv 23.09

- [LM-INFINITE: SIMPLE ON-THE-FLY LENGTH GENERALIZATION FOR LARGE LANGUAGE MODELS](https://arxiv.org/pdf/2308.16137.pdf) 

> arxiv 23.10 



#### Sparse Attention

<img src="https://kylinhub.oss-cn-shanghai.aliyuncs.com/image-20231030000753270.png" alt="image-20231030000753270" style="zoom:67%;" />

- [H2O: Heavy-Hitter Oracle for Efficient Generative Inference of Large Language Models](https://link.zhihu.com/?target=https%3A//browse.arxiv.org/pdf/2306.14048.pdf)    

> arxiv 23.06

通过动态的评价方式来判断需要保留和废弃的KV值



### 储存优化

- [Efficient Memory Management for Large Language Model Serving with PagedAttention](https://arxiv.org/pdf/2309.06180.pdf)

> SOSP 23





### 计算优化

- 





