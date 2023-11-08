---
dlayout:    post
title:      Zero Offload
subtitle:   Democratizing Billion-Scale Model Training
date:       2023-9-3
author:     Kylin
header-img: img/bg-owncloud.jpg
catalog: true
tags:
    - paper
---



[TOC]

SC 21

### Abstract:

主要面向 training 的 offload

This paper present ZeRO-Infinity, a novel **heterogeneous system** technology that **leverages GPU, CPU, and NVMe memory** to allow for unprecedented model scale on limited resources without requiring model code refactoring.

Contributions：

1）memory (Sec. 3) , bandwidth (Sec. 4) requirements for different components

2）ZeRO-Infinity：A novel DL training system，五个部分：

**i) infinity offload engine** to fully leverage heterogeneous architecture on modern clusters by simultaneously exploiting GPU, CPU and NVMe memory, and GPU and CPU compute

**ii) memory-centric tiling** to handle massive operators without requiring model parallelism

**iii) bandwidth-centric partitioning** for leveraging aggregate memory bandwidth across all parallel devices

**iv) overlap-centric design** for overlapping compute and communication

**v) ease-inspired implementation** to avoid model code refactoring.



Section 3 仔细分析了 Mem 消耗

Section 4 分析了带宽 Cost



#### Infinity Offload Engine

用了一个非常重要的工具：

**DeepNVMe**, a powerful C++ NVMe read/write library in the infinity offload engine that supports bulk read/write requests for asynchronous completion, and explicit synchronization requests to flush ongoing read/writes. The support for asynchrony allows ZeROInfinity to overlap these requests with GPU/GPU or GPU/CPU communication or computation.



### Experiments

#### Baseline

ZeRO

ZeRO-Offload：ZeRO-Offload: Democratizing Billion-Scale Model Training

#### Setup

GPT-like Transformer based models























