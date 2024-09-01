---
dlayout:    post
title:      LLM推理优化Review202408
subtitle:   LLM Infer Paper Review202408
date:       2024-08-30
author:     Kylin
header-img: img/bg-owncloud.jpg
catalog: true
tags:
    - paper
---



[TOC]

#### RelayAttention[^1] :

场景：长system prompt

问题：对于batched request，KV caches are transferred from off-chip DRAM to on-chip SRAM multiple times，也就是对于request是独立的。

解决方案：RelayAttention allows reading these hidden states from DRAM exactly once for a batch of input tokens

##### challenge

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2024-08-30%2016.02.13.png" alt="截屏2024-08-30 16.02.13" style="zoom:50%;" />

- The computation of attention is memory-bound: 主要就是GEMV相对GEMM计算不饱和度问题
- There are redundant memory accesses in the typical scenarios where a shared system prompt is prepended to request-specific contexts：diss vllm说prefix虽然share了但是access要两次

##### Methodology

> memory-bound (Section 3.1）有定义：
>
> Given a processor that takes tm for per-byte access, and tc for a floating operation on average, the ratio r of the total computation time over the total memory access time for an operator is:
>
> <img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2024-08-30%2015.57.56.png" alt="截屏2024-08-30 15.57.56" style="zoom:53%;" />
>
> where I is the arithmetic intensity of the operator:
>
> <img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2024-08-30%2015.58.16.png" alt="截屏2024-08-30 15.58.16" style="zoom:50%;" />
>
> When $I < t_m/t_c$, $r$ is less than 1, the operator is memory-bound. 因此对于不同的硬件来说，要克服memory-bound需要的computation intensity是不一样的。

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2024-09-01%2017.41.41.png" alt="截屏2024-09-01 17.41.41" style="zoom:50%;" />



#### Hydragen[^2]

![截屏2024-09-01 23.48.46](http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2024-09-01%2023.48.46.png)



### Reference

[^1]: RelayAttention for Efficient Large Language Model Serving with Long System Prompts
[^2]: Hydragen: High-Throughput LLM Inference with Shared Prefixes

