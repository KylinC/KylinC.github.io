---
dlayout:    post
title:      Introduction to FlexGen
subtitle:   High-Throughput Generative Inference of Large Language Models with a Single GPU
date:       2023-8-20
author:     Kylin
header-img: img/bg-owncloud.jpg
catalog: true
tags:
    - paper
---



[TOC]



> ICML 2023

Motivation: **high-throughput** LLM inference using limited resources, such as a **single** and **commodity** GPU.  





### Offloading Strategy





### Approximate Methods  

> For Better Inference Throughput



### Evaluation

#### setup

Google Cloud NVIDIA T4 GPU

![image-20230820225336592](https://kylinhub.oss-cn-shanghai.aliyuncs.com/image-20230820225336592.png)

The read bandwidth of SSD is about 2GB/s and the write bandwidth is about 1GB/s.  

#### Metrics



#### Baseline

DeepSpeed ZeRO-Inference   

Hugging Face Accelerate    

Petals  (decentralized collaborative inference)



































