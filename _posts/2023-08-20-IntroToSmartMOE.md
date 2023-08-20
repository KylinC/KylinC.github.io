---
dlayout:    post
title:      Introduction to SmartMOE
subtitle:   Efficiently Training Sparsely-Activated Models through Combining Offline and Online Parallelization
date:       2023-8-20
author:     Kylin
header-img: img/bg-owncloud.jpg
catalog: true
tags:
    - paper
---



[TOC]



Motivation: 之前的 research 都是按照一个 static plan 进行系统执行，而且一般考虑到：model architecture 和 hardware specification，现在我们想整一个 workload-aware 的 。

SmartMOE split the process of automatic parallelization into two stages, performed offline and online, respectively.   





### Evaluation

#### setup

用了3个不同拓扑结构的Clusters：

![image-20230820205840937](https://kylinhub.oss-cn-shanghai.aliyuncs.com/image-20230820205840937.png)

Model 用了 GPT-MOE 和 Swin-MOE

#### Baselines

- DeepSpeed-MoE   
- Tutel  
- FasterMoE
- Alpa



#### Metrics

training latency  