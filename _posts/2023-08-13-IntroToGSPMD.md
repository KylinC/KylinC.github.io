---
dlayout:    post
title:      GSPMD for ops partition across muti-devices
subtitle:   General and Scalable Parallelization for ML Computation Graphs
date:       2023-8-13
author:     Kylin
header-img: img/sparse.jpg
catalog: true
tags:
    - paper
---



[TOC]



Alpa 中说：Alpa optimizes the intra-operator parallelism plan within a device mesh. Alpa adopts the SPMD-style intra-op parallelism [31,57] which **partitions operators evenly across devices and executes the same instructions on all devices**, as per the fact that devices within a single mesh have equivalent compute capability.

Alpa因为用了GSPMD和XLA（就是论文里面做出的实现）做 excutables 到编译，因此可能限制在一个 Mesh 上只能均匀划分算子，因此来看看 SPMD 里面是不是有可以解决这个问题的 insights



实现的： https://www.tensorflow.org/xla?hl=zh-cn



本质上属于算子分布式编译
