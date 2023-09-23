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



