---
dlayout:    post
title:      LLM Inference Optimization
subtitle:   LLM 推理优化技术综述
date:       2023-10-05
author:     Kylin
header-img: img/bg-owncloud.jpg
catalog: true
tags:
    - paper
---



[TOC]

### Subgraph Fusion

图融合技术即通过将多个 OP（算子）合并成一个 OP（算子），来减少`Kernel`的调用。因为每一个基本 OP 都会对应一次 GPU kernel 的调用，和多次显存读写，这些都会增加大量额外的开销。

#### 1.1 [FasterTransformer](https://link.zhihu.com/?target=https%3A//github.com/NVIDIA/FasterTransformer) by NVIDIA

`FasterTransformer`(FT) 是一个用于实现基于`Transformer`的神经网络推理的加速引擎。`FT`框架是用`C++/CUDA`编写的，依赖于高度优化的 cuBLAS、cuBLASLt 和 cuSPARSELt 库，与 [NVIDIA TensorRT](https://link.zhihu.com/?target=https%3A//link.juejin.cn/%3Ftarget%3Dhttps%3A%2F%2Fdeveloper.nvidia.com%2Fblog%2Foptimizing-t5-and-gpt-2-for-real-time-inference-with-tensorrt%2F) 等其他编译器相比，FT 的特点是它支持**以分布式方式推理 Transformer 大模型**。



#### 1.2 [DeepSpeed Inference](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2207.00032.pdf) by Microsoft



#### 1.3 [MLC-LLM](https://link.zhihu.com/?target=https%3A//github.com/mlc-ai/mlc-llm) by TVM





### Model Compression





### Parallelism





### Transformer-Structure Optimization





### Dynamic Batching





### Hardware Optimization





