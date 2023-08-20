---
dlayout:    post
title:      Introduction to Mobius
subtitle:   Fine Tuning Large-Scale Models on Commodity GPU Servers
date:       2023-8-16
author:     Kylin
header-img: img/sparse.jpg
catalog: true
tags:
    - paper
---



[TOC]



Challenge：low **interGPU communication bandwidth** and **pressing communication contention**（通信争用） on the commodity GPU server obstruct training efficiency

![](https://kylinhub.oss-cn-shanghai.aliyuncs.com/image-20230820151459924.png)

Method：Mobius partitions the model into stages and carefully schedules them between GPU memory and DRAM to overlap communication with computation.

1）怎么分：It formulates pipeline execution into a mixed-integer program problem to find the optimal pipeline partition.

2）怎么stage-GPU映射：It also features a new stage-to-GPU mapping method termed cross mapping, to minimize communication contention.



Gaps：

- server GPU 有 NVLink，带宽有900G/s，远超PCIe的16G/s

- server GPU 可以使用 GPUDirect：其核心原理是：在不必要的时候避免CPU的参与

  GPUDirect是NVIDIA为其CUDA平台引入的一组技术，它们允许直接数据交换介于GPU与其他设备，如其他GPU或网络设备。其主要目的是避免数据在交换过程中不必要地通过主机内存，从而减少数据传输的延迟并增加系统整体的吞吐量。

  GPUDirect的几个主要部分包括：

  1. **GPUDirect P2P (Peer-to-Peer) Memory Access**: 允许CUDA上的一个GPU直接访问另一个GPU的内存。这避免了CPU介入并减少了数据传输的延迟。
  2. **GPUDirect RDMA (Remote Direct Memory Access)**: 这允许第三方设备（如网络适配器）直接读取/写入GPU内存，而不需要CPU的干预。这对于大规模高性能计算环境中的网络通信尤为有用，因为它可以减少通信的延迟。
  3. **GPUDirect Storage**: 这是一个新的技术，它旨在提高GPU与NVMe存储设备之间的数据交换速度。传统上，GPU处理数据时，数据首先需要从存储设备加载到CPU内存，然后再复制到GPU。但是，有了GPUDirect Storage，数据可以直接从存储设备传输到GPU，跳过了CPU内存。
  4. **GPUDirect for Video**: 这是为视频应用程序设计的，允许视频设备直接与NVIDIA GPU通信，从而避免数据通过主机应用程序。



### Framework

