---
dlayout:    post
title:      LLM Science Exam Review
subtitle:   Kaggle LLM Science Exam Review
date:       2024-01-21
author:     Kylin
header-img: img/llminterview.jpg
catalog: true
tags:
    - competition
---



[TOC]

### Bg

#### Objectives

目标是让LLM做选择题（5选1）

#### Data

数据是wikimedia的数据经过GPT3.5编写的选择题（并且过滤了简单问题）

> 注意kaggle能运行的最大模型参数量约7B（因为用的是16G P100）

train提供了200个问题样本，test全部包含4000样本

#### Metrics

MAP@3 （Mean Average Precision @3）

> 选前三个预测，第一预测GT为1.0，第二预测0.5，第三预测0.3333；；对所有问题取平均；



### Core Algorithm

#### Data-Augment

直接用Kaggle开源的wikipedia STEM数据 **270K wikipedia STEM articles** 利用gpt-3.5制作选择题

三个开源数据语料库做召回

#### Pipeline

- 数据提取
- Embedding
- Index
- Retrieval
- context（知识库）+ prompt（问题）+ option（选项）构成完整样本
- train deberta-v3-large

> ![截屏2024-03-20 11.07.48](http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2024-03-20%2011.07.48.png)

- Inference

#### Embedding

sentence_transformers + gte-base

> 使用方法在这里：https://huggingface.co/thenlper/gte-base
>
> 参数量大概120m

#### RAG

query文本：三次prompt+ABCDE拼接

用FAISS检索top10

#### Train

300k 样本

#### Inference

在三个语料库上预测每个选项的概率，取平均



### Reference

[^1]: Will Lifferth, Walter Reade, Addison Howard. (2023). Kaggle - LLM Science Exam. Kaggle. https://kaggle.com/competitions/kaggle-llm-science-exam





