---
dlayout:    post
title:      Speculative Decoding
subtitle:   Fast Inference from Transformers via Speculative Decoding
date:       2023-9-14
author:     Kylin
header-img: img/bg-owncloud.jpg
catalog: true
tags:
    - paper
---



[TOC]

> ICML 23



an algorithm to sample from autoregressive models faster without any changes to the outputs, by **computing several tokens in parallel**.

Two insights: 

1) hard language-modeling tasks often include easier subtasks that can be approximated well by more efficient models
2) using speculative execution and a novel sampling method (decoding models faster, by running them in parallel on the outputs of the approximation models, potentially generating several tokens concurrently, and without changing the distribution.)



### Intro

Observations: 

1) some inference steps are “harder” and some are “easier”, is also a key motivator for our work.
2) LLM 是 IO bounded 的，有两个途径解决：优化IO、填满计算，Spec选择后者



### Speculative Decoding

Idea：用分支预测的方法，用小模型近似大模型的采样分布

![截屏2023-09-14 16.25.12](http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-09-14%2016.25.12.png)





