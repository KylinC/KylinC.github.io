---
dlayout:    post
title:      PowerInfer
subtitle:   Fast Large Language Model Serving with a Consumer-grade GPU
date:       2023-01-04
author:     Kylin
header-img: img/power.jpg
catalog: true
tags:
    - paper
---



[TOC]

### Abs

**Objective**: PowerInfer is introduced as a high-speed LLM inference engine optimized for personal computers equipped with a single consumer-grade GPU.

**Insight**：

minority, *hot neurons*: consistently activated across inputs.

majority, *cold neurons*, vary based on specific inputs.

**Innovation**: The core innovation of PowerInfer is the exploitation of the high **locality** inherent in LLM inference, characterized by a power-law distribution in neuron activation. This leads to a novel **GPU-CPU hybrid inference engine design**.

**Experiment**：OPT-175B on a single NVIDIA RTX 4090 GPU, only 18% lower than that achieved by a top-tier server-grade A100 GPU.  



### Intro

按层进行model offload[14] 不是一个好方案，因为会造成 **locality mismatch** between hardware architecture and the characteristics of LLM inference：因为储存是按着data locality设计的层次化结构，但是layer-wise的划分不符合data locality设计。

而LLM本身是具有locality的。For example, in the OPT model, less than 10% of the elements in the activation map are non-zero, and these can be predicted with more than 93% accuracy at runtime [21]。

基于这个insight，PowerInfer认为我们可以把hot neurons和cold neurons进行**预处理分层**。但是存在以下三个挑战：

- 构建的predictor会占用一部分GPU内存=>自适应的predictor（稀疏、高偏度的predictor可以缩小）
- cuSPARSE  不适用 neuron-aware
- 初始化neuron placement是困难的，integer linear programming problem



### Reference

[^1]: 


