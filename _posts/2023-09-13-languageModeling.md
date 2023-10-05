---
dlayout:    post
title:      Language Modeling
subtitle:   Language Modeling 的两种方式
date:       2023-9-13
author:     Kylin
header-img: img/post-bg-universe.jpg
catalog: true
tags:
    - nlp
---



[TOC]



> 一个绝好的教程：https://huggingface.co/docs/transformers/tasks/language_modeling



### Casual Language Modeling

Causal language modeling predicts the next token in a sequence of tokens, and the model can only attend to tokens on the left. This means the model cannot see future tokens. GPT-2 is an example of a causal language model.

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-09-13%2015.43.32.png" alt="截屏2023-09-13 15.43.32" style="zoom:33%;" />

#### Metrics

- Perplexity（求一个log就是cross entropy）
- Cross Entropy



#### Dataset 

can use any plaintexts



#### Usage

可以用来 Code Generation

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-09-13%2015.43.11.png" alt="截屏2023-09-13 15.43.11" style="zoom:33%;" />



### Masked Language Modeling

Masked language modeling predicts a masked token in a sequence, and the model can attend to tokens bidirectionally. This means the model has full access to the tokens on the left and right. Masked language modeling is great for tasks that require a good contextual understanding of an entire sequence. BERT is an example of a masked language model.

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-09-13%2015.50.16.png" alt="截屏2023-09-13 15.50.16" style="zoom:33%;" />

#### Metrics

- Perplexity（求一个log就是cross entropy）
- Cross Entropy



#### Dataset 

can use any plaintexts

