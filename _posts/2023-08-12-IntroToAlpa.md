---
dlayout:    post
title:      Alpa Distributed ML Compiler
subtitle:   Automating Inter- and Intra-Operator Parallelism for Distributed Deep Learning
date:       2023-8-12
author:     Kylin
header-img: img/sparse.jpg
catalog: true
tags:
    - paper
---



[TOC]



### Architecture

is all you need!

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-08-12%2015.45.12.png" alt="截屏2023-08-12 15.45.12" style="zoom:37%;" />



### Framework

主要的idea：: 结合 intra-op 和 inter-op 两种并行策略，进行比较好的计算图划分、设备分配策略

> 什么是 intra-op 和 inter-op ？
>
> ![截屏2023-08-12 15.51.53](http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-08-12%2015.51.53.png)
>
> - Intra-op: 对数据对划分、对算子的划分
> - inter-op: 对计算图的划分，并用流水线方式运行（流水线方式和Operator parallelism最大的区别在于“Instead of partitioning ops, pipeline parallelism places different groups of ops from the model graph, referred as stages, on different workers”，即模型的不同部分放在不同的线程worker上，流动的数据在不同worker上是并行执行的）



整体架构分做四个阶段：

- Given a model description, in the form of a **Jax intermediate** representation (IR), and a **cluster configuration**, the inter-op compilation pass slices the IR into a number of stages, and slices the device cluster into a number of device meshes. 
- （ inter-op 指派 subgraph(stage) 到 mesh ）The inter-op pass uses a Dynamic Programming (DP) algorithm to assign stages to meshes and invokes the intra-op compilation pass on each stage-mesh pair, to query the execution cost of this assignment. 
- （ inter-op call intra-op ）Once invoked, the intra-op pass optimizes the intra-op parallel execution plan of the stage running on its assigned mesh, by minimizing its execution cost using an Integer Linear Programming (ILP) formulation, and reports the cost back to the inter-op pass. 
- （ 回到 inter-op ）By repeatedly querying the intra-op pass for each allocation of a stage-mesh pair, the inter-op pass uses the DP to minimize the inter-op parallel execution latency and obtains the best slicing scheme of stages and meshes.



所以，讲清楚 alpa 的 intra-op 和 inter-op 策略，论文的方法论部分就清楚了。



### Intra-op

alpa 用了 [SPMD-style intra-op parallelism](https://arxiv.org/abs/2105.04663) 策略：partitions operators **evenly** across devices and executes the same instructions on all devices, as per the fact that devices within a single mesh have **equivalent** compute capability。

op并行策略的话，alpa将其建模成一个 ILP 问题



### Inter-op

Optimization goal:  is to minimize the end-to-end pipeline execution latency for the entire computational graph.

这就涉及到三个地方：1）计算图划分、2）设备集群划分、3）子计算图-子设备群的配对指派



### Experiments

#### Setup

使用模型：GPT-3 [10], GShard Mixtureof-Experts (MoE) [31], and Wide-ResNet [59]

设备：8 nodes and 64 GPUs. Each node is an Amazon EC2 p3.16xlarge instance with 8 NVIDIA V100 16 GB GPUs, 64 vCPUs, and 488 GB memory. The 8 GPUs in a node are connected via NVLink. The 8 nodes are launched within one placement group with 25Gbps crossnode bandwidth.

#### Metrics

>  alpa主要是测量在training时候的performance

- training throughput (paper 说因为alpa没改梯度聚合、同步算法，因此收敛速度、AI performance不用测了)
- 系统稳定性 weak scaling of the system when increasing the model size along with the number of GPUs
- 集群浮点运算速度 aggregated peta floatingpoint operations per second (PFLOPS) of the whole clusters



