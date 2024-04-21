---
dlayout:    post
title:      Early-Exiting Framework with Parallel Decoding
subtitle:   Fast and Robust Early-Exiting Framework for Autoregressive Language Models with Synchronized Parallel Decoding
date:       2023-01-14
author:     Kylin
header-img: img/fastexiting.jpg
catalog: true
tags:
    - paper
---



[TOC]

> *9 Oct 2023*, Arxiv



### Abs

early-exiting的本质其实就是为计算图添加条件分支，而auto-regressive本质上也就是条件计算图中比较特殊的循环计算图。

传统的ee有退化问题的原因：

- state copying mechanism
- numerous exit paths
- sensitivity to exit confidence thresholds



### Intro

Challenges of previous works:

- 为了应对KV recompute的问题，提出的state copying mechanism (Elbayad et al., 2020; Schuster et al., 2022) 对于长序列退化 (sec 4.1)
- 早停可能会造成超长生成序列（循环词）
- 过多的exiting point会加大系统开销，即the computational overhead from confidence measurement（置信度测量） at every layer
- 选择confidence threshold带来开销和不连续性（不同的ct会带来系统表现波动）



### Free

![截屏2024-01-14 15.44.10](http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2024-01-14%2015.44.10.png)



### Reference

[^1]: Kim S, Mangalam K, Moon S, et al. Speculative decoding with big little decoder[C]//Thirty-seventh Conference on Neural Information Processing Systems. 2023.
[^2]: Chen C, Borgeaud S, Irving G, et al. Accelerating large language model decoding with speculative sampling[J]. arXiv preprint arXiv:2302.01318, 2023.



