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



Challenge：low **interGPU communication bandwidth** and **pressing communication contention** on the commodity GPU server obstruct training efficiency



Method：Mobius partitions the model into stages and carefully schedules them between GPU memory and DRAM to overlap communication with computation.

1）怎么分：It formulates pipeline execution into a mixed-integer program problem to find the optimal pipeline partition.

2）怎么stage-GPU映射：It also features a new stage-to-GPU mapping method termed cross mapping, to minimize communication contention.

