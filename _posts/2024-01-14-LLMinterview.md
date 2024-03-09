---
dlayout:    post
title:      LLM 常见面试问题
subtitle:   Interview for LLM
date:       2024-01-22
author:     Kylin
header-img: img/llminterview.jpg
catalog: true
tags:
    - job
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

#### 2、GPT-2参数量怎么计算？（ByteDance-1）

> 参考[^2]

#### 3、Lora原理？Lora的参数量计算？Lora参数是包含Attention还是MLP？Lora参数的初始化？（ByteDance-1）



#### 4、LLM预训练参数的初始化？



#### 5、ROPE

> 参考[^4] 



### 多模态篇

#### 1、为什么MLLM普遍都是2阶段训练，而不是1阶段？（ByteDance-1）

主要是从数据上来考虑，pretrain阶段的数据是多样的，而instructionTuning阶段的数据只是特定任务的（i.e., 评论生成）



#### 2、为什么不将ViT加入到MLLM训练过程中？ViT的参数量以及显存占用量？（ByteDance-1）

ViT是通过CLIP进行对比学习得到的，有**跨模态理解能力**和**zero-shot**的能力。





#### 3、几种MLLM的架构极其特点？优势？（ByteDance-1）

> 参考[^3]





### Reference

[^1]: Mainstream architecture of LLMs https://kylinchen.cn/2023/12/04/whichLLM/

[^2]: GPT2参数量准确计算. https://kylinchen.cn/2024/01/22/LLMcompute/
[^3]: MLLM 经典结构详解. https://kylinchen.cn/2024/01/22/MultiModalLLMStructure/
[^4]: Transformer中的位置编码(Position Encoding). https://0809zheng.github.io/2022/07/01/posencode.html

