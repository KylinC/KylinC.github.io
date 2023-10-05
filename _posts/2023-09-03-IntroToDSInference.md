---
dlayout:    post
title:      DeepSpeed Inference Zero
subtitle:   Enabling Efficient Inference of Transformer Models at Unprecedented Scale
date:       2023-9-3
author:     Kylin
header-img: img/bg-owncloud.jpg
catalog: true
tags:
    - paper
---



[TOC]

SC 22

### Abstract:

- transformer-based models 差异化发展：

1）参数量不同 

2）由MoE引入的稀疏性不同 

3）the target application scenarios can be latency-critical or throughput-oriented 

4）the deployment hardware could be **single- or multi-GPU** systems  

5）different types of memory and storage

因此要设计一个高效的 Inference 系统很有挑战

- DeepSpeed Inference consists of

1）当GPU内存足够时，a solution **minimize latency** while **maximizing the throughput** of both dense and sparse transformer 

2）当GPU内存不足时，a **heterogeneous inference solution** that leverages CPU and NVMe memory in addition to the GPU memory and compute to enable high inference throughput with large models which do not fit in aggregate GPU memory.



### Arch

#### Kernel 分析

- 不同batchsize的影响

小batchsize受到内存带宽（读取weights）的影响

大batch也受到到不同GPU cores之间传输带宽的影响

- 重写一部分kernel

因此deepspeed inference重写了一部分kernel



> INFERENCE-ADAPTED DENSE TRANSFORMER MODELS ON MANY-GPU SYSTEMS

#### Inference Optimized Pipeline Parallelism

一句话咱就来学学：While model parallelism is extensively studied in the context of training, there are unique challenges in inference, requiring new solutions.

##### Tensor Parallelism

tensor-slicing parallelism (TP)：can automatically scale a dense transformer model to multiple devices by partitioning transformer operators across multiple devices while also adding appropriate communication operations needed across GPUs.

##### Pipeline Parallelism

pipeline parallelism (PP)：Activations相比Tensor Parallelism的通信开销极其小

但是由于transform-based model的inference都是autoregressive的，因此kv-cache会带来额外通信开销（且随batchsize城北增加）。

##### Inference Optimized Pipeline Parallelism

> 三个方面优化：
>
> - scheduling
> - memory footprint reduction
> - communication optimization

scheduling：减少microbatch数量（每一个mbs都是独立从内存获取权重）、减少pp的bubble

memory footprint reduction：offload activations 到 cpu

communication optimization：大多数系统的多GPU是有带宽争用的，两个GPU公用一个PCIe总线



#### Democratization of Large Model Inference

ZeRO-Inference：enables large model inference using as few as a single GPU

这里就是 throughput-oriented的了，因为offload允许了高batchsize

##### ZeRO-Inference Design

引用了一个ZeRO-Infinity

DS-inference 把 weights 放在 DRAM 或者 nvme 层次上，逐层 load，效率高的原因：

- 可以限制load的memory限制实现大batchsize
- 超大模型的计算时间是dominant的，可以overlap掉load时间

##### Performance Optimizations

prefetching 权重

Multi-GPU PCI-e bandwith utilization：先fetch之后进行权重聚合

引用了一个prior的优化工作：Zeroinfinity: Breaking the gpu memory wall for extreme scale deep learning

SC 21



### Related Work

FastTransformer 作为一个关键的baseline，支持多GPU推理



### Experiments

#### Metrics

(i) latency, i.e., end-to-end output generation time for a batch of input prompts

(ii) token throughput, i.e., tokens-per-second processed

(iii) compute throughput, i.e., TFLOPS per GPU.

 #### Setups

模型：开源GPT模型（不同参数量、layer、hidden size）、MoE（不同参数量）

硬件：256 NVIDIA Ampere A100 40GB GPUs (32 8×A100 DGX boxes [38]), a lambda A6000 workstation [39] (2×A6000-48GB-GPU, 256GB DRAM, and 2TB NVME) and a DGX2 V100 server [40] (16×V100-32GB-SXM-GPU, 1500GB DRAM, and 30TB NVME).















