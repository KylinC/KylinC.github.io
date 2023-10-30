---
dlayout:    post
title:      FastServe - A distributed Serving System 
subtitle:   Fast Distributed Inference Serving for Large Language Models
date:       2023-10-13
author:     Kylin
header-img: img/bg-owncloud.jpg
catalog: true
tags:
    - paper
---



[TOC]

### Abs

问题：Existing LLM serving systems use run-to-completion processing for inference jobs, which suffers from head-of-line blocking and long JCT.

> JCT是从提交作业（或任务、请求等）到作业完成并得到结果的总时间。这个时间可能包括了等待时间、执行时间、通信时间、IO时间等多个组成部分。通过优化系统设计和算法，可以降低JCT，从而提高系统的吞吐量和用户满意度。

解决思路：

1）preemptive scheduling，a novel skip-join Multi-Level Feedback Queue scheduler

2）an efficient GPU memory management mechanism,  offloads and uploads intermediate states between GPU memory and host memory

### Intro

challenge：

1）未知的输出长度

2）GPU内存开销

做的事情：

1）利用一个Multi-Level Feedback Queue (MLFQ)来调度推理任务：

> 假设：first token latency > decode latency, 即长prompt短输出

因此会首先衡量prefill的时间，根据这个时间分配队列层级（就是所谓的skipped）

2）由于MLFQ启动的任务可能过多，因此要offload KV-cache，因此利用MLFQ的优先级动态offload；此外，还设计了GPU之间的分布式offload



### FastServe

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-10-11%2010.07.56.png" alt="截屏2023-10-11 10.07.56" style="zoom:50%;" />

#### Skip-Join MLFQ Scheduler

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-10-11%2011.02.09.png" alt="截屏2023-10-11 11.02.09" style="zoom:50%;" />

#### Proactive Key-Value Cache Management

MLFQ的KV-cache占用比传统系统高很多

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-10-11%2011.03.00.png" alt="截屏2023-10-11 11.03.00" style="zoom:50%;" />

### Exp

#### Metrics

**average** and **tail** job completion time (JCT)

#### Baselines

- FasterTransformer
- Orca

#### 实验设计

- overall表现：就是在不同的workload和baseline的对比
- 消融实验，说明为什么系统的2部分分别起作用
- scaling：流水线分布式实验



