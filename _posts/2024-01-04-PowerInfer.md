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



### Intro





### Reference

[^1]: 


