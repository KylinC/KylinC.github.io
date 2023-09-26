---
dlayout:    post
title:      DeepUM(DNN Models on Unified Memory) on Aspolos 23
subtitle:   Tensor Migration and Prefetching in Unified Memory
date:       2023-9-23
author:     Kylin
header-img: img/bg-owncloud.jpg
catalog: true
tags:
    - paper
---



[TOC]

> ASPOLOS 23



### Ab

Proposed DeepUM allows **memory oversubscription** using a page fault mechanism.

DeepUM uses a new correlation **prefetching technique** to hide the page migration overhead.

outperform six state-of-the-art GPU memory swapping approaches









### Eval

#### Baseline

LMS[33], vDNN[51], AutoTM[21], SwapAdvisor[24], Capuchin[45], and Sentinel[50].

#### Setup

- Two GPUs：NVIDIA Tesla V100 PCIe 32GB NVIDIA Tesla V100 PCIe 16GB

- Models：MLperf

#### Metrics

- SpeedUp
- ratio of total energy consumption over UM



### Related Work

There are two categories in the swapping approach ：

#### One swaps-in/swaps-out the whole GPU memory objects to the CPU memory or NVMe devices[6, 21, 24, 33, 45, 49–51, 55]  

- vDNN：开山之作，swap for Deep Neural Network (DNN) workloads. **it supports only convolutional neural networks (CNNs).**  
- TFLMS ：集成在IBM Watson里面, It schedules swap-in/swap-out commands by modifying computational graphs. 需要改后端和用户代码。
- Both Superneurons[55] and FlashNeuron[6]： take a DNN model as input and derive an optimal tensor offloading schedule. 
  - Superneurons offloads tensors in the GPU memory to the CPU memory
  - FlashNeuron[6] exploits NVMe SSDs to offload the tensors.  
- AutoTM[21] and SwapAdvisor[24]：also take a DNN model as input to schedule its memory operations.  
  - AutoTM uses integer linear programming (ILP) not only to schedule data movement but also to reduce GPU memory fragmentation。
  - SwapAdvisor[24] uses a genetic algorithm to schedule operators, memory allocations, and swap decisions.   
  - Capuchin[45] identifies the tensor access patterns at run time and schedules tensor eviction, prefetching, and recomputation.  
- Sentinel：使用了page fault mechanism。the Sentinel uses the CPU page fault mechanism while DeepUM uses the GPU page fault mechanism of the GPU。**training throughput超过以上系统。**
- DeepSpeed：It keeps track of the sequence of DNN operations by hooking PyTorch APIs. Then, it provides the GPU memory swapping mechanism with the CPU main memory or NVMe SSD（**三级offload**）. However, DeepSpeed manages offloading model parameters, gradients, and optimizer states, not activations（**activation要手动checkpoint**）. 

#### exploits CUDA UM with prefetching[5, 35]  

- OC-DNN[5] manually inserts prefetch commands in front of each DNN operation. 
- DRAGON[35] uses an NVMe SSD as a backing store for UM. It targets general applications and uses a DNN workload as a showcase. It also requires user code modification and device driver modification.  

