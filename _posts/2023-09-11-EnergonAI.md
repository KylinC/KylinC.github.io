---
dlayout:    post
title:      EnergonAI as a prototype of Alpa
subtitle:   An Inference System for 10-100 Billion Parameter Transformer Models
date:       2023-9-11
author:     Kylin
header-img: img/bg-owncloud.jpg
catalog: true
tags:
    - paper
---



[TOC]

**Background**：Deployment of 10-100 billion parameter models is still uncertain due to the latency, throughput, and memory constraints.



**Contribution**：

EnergonAI adopts a hierarchy-controller system architecture to coordinate multiple devices and efficiently support different parallel patterns.

Technics：non-blocking pipeline parallelism, distributed redundant computation elimination, and peer memory pooling



这里回答了一个问题：为啥要多个host？
For pipeline parallelism, it partitions a model by layers, and it can optimize throughput and memory footprint, other than latency.



**insights：** 随着LLM参数量增加，矩阵计算时间占比是增加的：

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-09-14%2015.17.52.png" alt="截屏2023-09-14 15.17.52" style="zoom:53%;" />
