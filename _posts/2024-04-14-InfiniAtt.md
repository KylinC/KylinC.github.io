---
dlayout:    post
title:      Infini Attention 详解
subtitle:   Efficient Infinite Context Transformers with Infini Attention 详解
date:       2024-04-14
author:     Kylin
header-img: img/swapdp.jpg
catalog: true
tags:
    - coding
---



[TOC]

### Abstract

达到的效果：bounded memory and computation

方法的本质：new attention technique dubbed Infini-attention, 即修改的attention机制



### Intro

宏观的想法上就是Q分别在 previous segments 和 local segmenet上分别做attention，只对后者施加casual mask。但是乍一看这个实现的复杂度还是O(n^2)，因为KV cache策略就是这样做的。

![image-20240414234454226](https://kylinhub.oss-cn-shanghai.aliyuncs.com/image-20240414234454226.png)

### Infini-ATT





### Exp

由于Attention机制是重新设计的，所以infini-att不仅仅在系统层面，因此需要重新训练&准确率评估。

一个Gating score visualization，可以看出文本存在一些

实验主要包括 1）大海捞针、2）summarization





### Reference

[^1]: 
















