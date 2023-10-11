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

> ref: https://zhuanlan.zhihu.com/p/642412124



### Subgraph Fusion

图融合技术即通过将多个 OP（算子）合并成一个 OP（算子），来减少`Kernel`的调用。因为每一个基本 OP 都会对应一次 GPU kernel 的调用，和多次显存读写，这些都会增加大量额外的开销。



#### 1.1 [FasterTransformer](https://link.zhihu.com/?target=https%3A//github.com/NVIDIA/FasterTransformer) by NVIDIA

`FasterTransformer`(FT) 是一个用于实现基于`Transformer`的神经网络推理的加速引擎。`FT`框架是用`C++/CUDA`编写的，依赖于高度优化的 cuBLAS、cuBLASLt 和 cuSPARSELt 库，与 [NVIDIA TensorRT](https://link.zhihu.com/?target=https%3A//link.juejin.cn/%3Ftarget%3Dhttps%3A%2F%2Fdeveloper.nvidia.com%2Fblog%2Foptimizing-t5-and-gpt-2-for-real-time-inference-with-tensorrt%2F) 等其他编译器相比，FT 的特点是它支持**以分布式方式推理 Transformer 大模型**。

**这种技术减少了数据传输并增加了数学密度**，从而加速了推理阶段的计算。 例如， multi-head attention 块中的所有操作都可以合并到一个内核中。



#### 1.2 [DeepSpeed Inference](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2207.00032.pdf) by Microsoft

对于 Transformer layer，可分为以下4个主要部分：

1. Input Layer-Norm plus Query, Key, and Value GeMMs and their bias adds.
2. Transform plus Attention.
3. Intermediate FF, Layer-Norm, Bias-add, Residual, and Gaussian Error Linear Unit (GELU).
4. Bias-add plus Residual.

对每一个部分进行融合



#### 1.3 [MLC-LLM](https://link.zhihu.com/?target=https%3A//github.com/mlc-ai/mlc-llm) by TVM

MLC LLM 的主要工作流基于 Apache TVM Unity，通过扩展 TVM 后端使模型编译更加透明和高效。其中以编译代码转换、融合、内存规划和库卸载（library offloading）为代表的可组合的 ML 编译优化是其中重要的优化特性。



### Model Compression

#### 2.1 Sparsity

实现稀疏(Sparsity)的一个重要方法是剪枝(Pruning)。剪枝是在保留模型容量的情况下，通过修剪不重要的模型权重或连接来减小模型大小。

- 非结构化剪枝允许删除任何权重或连接，因此它不保留原始网络架构。 非结构化剪枝通常不适用于现代硬件，并且不会带来实际的推理加速。
- 结构化剪枝旨在维持某些元素为零的密集矩阵乘法形式。 他们可能需要遵循某些模式限制才能使用硬件内核支持的内容。 当前的主流方法关注结构化剪枝，以实现 Transformer 模型的高稀疏性。



#### 2.2 Quantization

常见量化有两种常见方法：

- 训练后量化（Post-Training Quantization，PTQ）：模型首先经过训练以达到收敛，然后我们将其权重转换为较低的精度，而无需进行更多训练。 与训练相比，实施起来通常相当便宜。
- 量化感知训练（Quantization-Aware Training，QAT）：在预训练或进一步微调期间应用量化。 QAT 能够获得更好的性能，但需要额外的计算资源和对代表性训练数据的访问。



#### 2.3 Distillation

[知识蒸馏](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2006.05525)是一种构建更小、更便宜的模型（“student 模型”）的直接方法，通过从预先训练的昂贵模型中转移技能来加速推理（“ teacher 模型”）融入 student。 除了与 teacher 匹配的输出空间以构建适当的学习目标之外，对于如何构建 student 架构没有太多限制。



### Parallelism

当前的推理的并行化技术主要体现在3个维度上，即 3D Parallelism:

- Data Parallelism(DP)
- Tensor Parallelism(TP)
- Pipeline Parallelism(PP)

#### 3.1 Data Parallelism

在推理中，DP 主要是增加设备数来增加系统整体 Throughput，其中最经典的即DeepSpeed的Zero系列

#### 3.2 Tensor Parallelism

在推理中，TP 主要是横向增加设备数通过并行计算来减少 latency

当前主流的推理框架都支持 TP 的方式，包括但不限于：

- [Megatron-LM](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/1909.08053.pdf)
- [FasterTransformer](https://link.zhihu.com/?target=https%3A//github.com/NVIDIA/FasterTransformer)
- [DeepSpeed Inference](https://link.zhihu.com/?target=https%3A//github.com/microsoft/DeepSpeed/tree/master/deepspeed/inference)
- [vLLM](https://link.zhihu.com/?target=https%3A//github.com/vllm-project/vllm)
- [Text Generation Inference](https://link.zhihu.com/?target=https%3A//github.com/huggingface/text-generation-inference)
- [ParallelFormers](https://link.zhihu.com/?target=https%3A//github.com/tunib-ai/parallelformers)
- [ColossalAI](https://link.zhihu.com/?target=https%3A//github.com/hpcaitech/ColossalAI)
- [FlexFlow](https://link.zhihu.com/?target=https%3A//github.com/flexflow/FlexFlow)
- [LiBai](https://link.zhihu.com/?target=https%3A//github.com/Oneflow-Inc/libai)
- [AlpaServe](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2302.11665.pdf)

#### 3.3 Pipeline Parallelism

在推理中，PP 主要是纵向增加设备数通过并行计算来支持更大模型，同时提高设备利用率。



### Transformer-Structure Optimization

#### 4.1 FlashAttention

#### 4.2 [FLAT Attention](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2107.06419.pdf)



### Dynamic Batching

#### 5.1 Orca

Orca 不是等到 Batch 中的所有序列完成生成，而是实现 *iteration* 级调度，其中Batch size由每次迭代确定。 结果是，一旦 Batch 中的序列完成生成，就可以在其位置插入新序列，从而比静态 Batch 产生更高的 GPU 利用率。



#### 5.2 FastServe

ORCA 使用first-come-first-served (FCFS) 处理推理作业, 计划任务持续运行直至完成。 由于 GPU 内存容量有限以及推理对延时敏感，无法通过任意数量的传入函数来增加处理，由此可能会导致队列阻塞。

FastServe 使用 preemptive scheduling，通过新颖的跳跃连接 Multi-Level Feedback Queue 程序来最小化延时。 基于 LLM 推理的长度无法确定，调度程序利用输入长度信息来分配适当的初始值每个到达作业要加入的队列。 较高优先级队列跳过加入的队列以减少降级。 设计高效的GPU内存管理机制主动下载和上传 GPU 内存和主机内存之间的中间状态，以进行 LLM 推理。



#### 5.3 vLLM

vLLM 的核心是 PagedAttention，其灵感来自传统操作系统概念，例如分页和虚拟内存。 它们通过在固定大小的“页面”或块中分配内存，允许 KV 缓存变得不连续。 然后可以重写 attention 机制以对块对齐的输入进行操作，从而允许在非连续的内存范围上执行 attention 。

这意味着 cache 分配可以 just-in-time，而不是 ahead-of-time：当启动一个新的生成任务时，框架不需要分配大小为 Maximum_context_length 的连续 cache。 每次迭代，调度程序都可以决定特定生成任务是否需要更多空间，并动态分配，而不会降低 PagedAttention 的性能。 这并不能保证内存的完美利用（浪费现在限制在 4% 以下，仅在最后一个块中），但它明显改善了当今业界广泛使用的提前分配方案的浪费 。

总而言之，PagedAttention + vLLM 可节省大量内存，因为大多数序列不会消耗整个上下文窗口。 这些内存节省直接转化为更高的 Batch 大小，这意味着更高的吞吐量和更便宜的服务。



#### 5.4 [Text Generation Inference](https://link.zhihu.com/?target=https%3A//github.com/huggingface/text-generation-inference) by Huggingface

TGI 是 HuggingFace 开发的基于 Rust, Python 和 gRPC 的推理服务工具：

<img src="https://kylinhub.oss-cn-shanghai.aliyuncs.com/image-20231011081844653.png" alt="image-20231011081844653" style="zoom:67%;" />



#### 5.5 [LMDeploy](https://link.zhihu.com/?target=https%3A//github.com/InternLM/lmdeploy)

LMDeploy 是由 MMRazor 和 MMDeploy 团队开发的用于压缩、部署 LLM 服务的工具包。 它具有以下核心特点：

- TurboMind：基于FasterTransformer 的高效推理引擎。
- 交互推理：通过缓存多轮对话过程中的 k/v，记住对话历史，以避免对历史会话的重复处理。
- 多GPU模型部署和量化
- Dynamic Batch



#### 5.6 SARATHI

> refer to https://kylinchen.cn/2023/10/08/SARATHI/

ORCA的每一个batch是“计算不均衡”的，原因就是在于ORCA对于prefill和decode是不区分处理的，而两个阶段的“计算/访存”密度是明显不一致的



### Hardware Optimization

#### 6.1 [NVIDIA H100 PCIe](https://link.zhihu.com/?target=https%3A//www.nvidia.cn/data-center/h100/)

#### 6.2 AMD MI300

AMD MI300 处理器集成了24个Zen 4架构CPU核心，以及CDNA 3架构GPU核心，周围还有着8颗HBM3高速缓存，容量高达128GB，总计拥有1460亿个晶体管。与上一代 MI250相比，MI300进行AI运算的速度将提高至8倍，能效方面也将提升5倍。

目前未找到公开的在 LLM 方面的推理性能数据。

#### 6.3 [Apple M2 Ultra](https://link.zhihu.com/?target=https%3A//www.apple.com/newsroom/2023/06/apple-introduces-m2-ultra/)

M2 Ultra 采用第二代 5 纳米工艺制造，并使用 Apple 突破性的 UltraFusion 技术连接两个 M2 Max 芯片的芯片，使性能提高一倍。 M2 Ultra 由 1340 亿个晶体管组成，比 M1 Ultra 多了 200 亿个。 其统一内存架构支持突破性的192GB内存容量，比M1 Ultra多出50%，并具有800GB/s的内存带宽，是M2 Max的两倍。 M2 Ultra 配备更强大的 CPU（比 M1 Ultra 快 20%）、更大的 GPU（快 30%）以及神经引擎（快 40%）。

目前未找到公开的在 LLM 方面的推理性能数据。

#### 6.4 Graphcore IPU

Graphcore C600 IPU处理器PCIe卡是针对机器学习推理应用的高性能加速卡。每个IPU具有1472个处理核心，能够并行运行8832个独立程序线程。每个IPU都有900MB的片上SRAM存储。用户可以在单个机箱中直接连接多达8块卡，通过高带宽的IPU-Links进行桥接。在训练和推理自然语言处理 (NLP) 模型（如 BERT 和 GPT、图神经网络 (GNN)、目标检测、语音等）时表现出色的结果。

目前未找到公开的在 LLM 方面的推理性能数据。

#### 6.5 Biren BR100

BR100是由壁仞科技发布自主研发的首款通用GPU芯片，其16位浮点算力达到1000T以上、8位定点算力达到2000T以上，单芯片峰值算力达到PFlops（1PFlops等于1000万亿次浮点指令/秒）级别。其与 A100、 H100 的参数对比如下所示：

![image-20231011080944792](https://kylinhub.oss-cn-shanghai.aliyuncs.com/image-20231011080944792.png)

目前未找到公开的在 LLM 方面的推理性能数据。