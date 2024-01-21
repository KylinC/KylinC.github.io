---
dlayout:    post
title:      LLM 常见面试问题
subtitle:   Interview for LLM
date:       2024-01-14
author:     Kylin
header-img: img/llminterview.jpg
catalog: true
tags:
    - nlp
---



[TOC]



### 基础篇

#### 1、为什么主流LLM都是Decoder-Only的？[^1]

先横向比较下，再说为什么：

LLM架构主要分为三类：

Encoder-Only、Encoder-Decoder、Decoder-Only



首先纯Encoder+MLM的模型（bert）只适合做NLU，不适合做生成。

现在做Encoder-Decoder、Decoder-Only的比较：

1）decoder-Only的zero-shot效果更好，因为decoder-Only不会出现低秩问题，建模难度更大

2）in-context learning：Decoder-Only更好适应，因为其prompt会更直接作用于decoder参数

> 但是LLM架构问题和Language Modeling是紧密相关的，其实就是在于用什么样的mask和什么样的objective：
>
> 用non-casual modeling的模型在进行malti-task ft之后zero-shot效果是最好的。
>
> 至于为什么现在decoder-only比较火，其实有一点历史原因。





### 多模态篇







### Reference

[^1]: Mainstream architecture of LLMs https://kylinchen.cn/2023/12/04/whichLLM/



