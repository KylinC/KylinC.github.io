---
dlayout:    post
title:      PipeFisher
subtitle:   EFFICIENT TRAINING OF LARGE LANGUAGE MODELS USING PIPELINING AND FISHER INFORMATION MATRICES
date:       2023-11-11
author:     Kylin
header-img: img/bg-owncloud.jpg
catalog: true
tags:
    - paper
---



[TOC]

### Abs

challenges：

- pipeline bubbles during startup and tear-down reduce the utilization of accelerators
- 在现有技术(micro-batch & bidirectional pipeline)下无法解决：bubbles comes from synchronous forward and backward passes

our propose:  K-FAC

- method: a second-order optimization method based on the Fisher information matrix
- goal: to the bubbles to accelerate convergence.
- result: high accelerator utilization & improved convergence



### Intro



