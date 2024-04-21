---
dlayout:    post
title:      LLMSys Reading List
subtitle:   LLMSys论文列表
date:       2024-03-28
author:     Kylin
header-img: img/bg_NLPseg.jpg
catalog: true
tags:
    - job
---



[TOC]



### ASPLOS 24

- [ASPLOS '24] AttAcc! Unleashing the Power of PIM for Batched Transformer-based Generative Model Inference
- [ASPLOS '24] NeuPIMs: NPU-PIM Heterogeneous Acceleration for Batched LLM Inferencing



- [ASPLOS '24] ExeGPT: Constraint-Aware Resource Scheduling for LLM Inference
- [ASPLOS '24] SpotServe: Serving Generative Large Language Models on Preemptible Instances



### HPCA 24

- [HPCA '24] An LPDDR-based CXL-PNM Platform for TCO-Efficient GPT Inference



### OSDI 24

- Bitter: Enabling Efficient Low-Precision Deep Learning Computing through Hardware-aware Tensor Transformation.From MSRA 

- Llumnix: Dynamic Scheduling for Large Language Model Serving. From Alibaba 



- dLoRA: Dynamically Orchestrating Requests and Adapters for LoRA LLM Serving. From PKU 

> 类似工作 [S-Lora](https://github.com/S-LoRA/S-LoRA) 

- DistLLM: Disaggregating Prefill and Decoding for Goodput-optimized Large Language Model Serving.Yinmin Zhong, Shengyu Liu, Junda Chen, Jianbo Hu, Yibo Zhu, Xuanzhe Liu, Xin Jin, Hao Zhang. **From PKU** [link](https://arxiv.org/abs/2401.09670)

- Cuber: Constraint-Guided Parallelization Plan Generation for Deep Learning Training From USTC & MSRA 
- Fairness in Serving Large Language Models. Ying Sheng, Shiyi Cao, Dacheng Li, Banghua Zhu, Zhuohan Li, Danyang Zhuo, Joseph E. Gonzalez, Ion Stoica. **From UC Berkeley** [link](https://arxiv.org/abs/2401.00588)

> 短文本和长文本 request 的平衡

- ServerlessLLM: Locality-Enhanced Serverless Inference for Large Language Models.Yao Fu, Leyang Xue, Yeqi Huang, Andrei-Octavian Brabete, Dmitrii Ustiugov, Yuvraj Patel, Luo Mai. **From UoE**  [link](https://arxiv.org/abs/2401.14351)

> Checkpoint loading 优化

- Automatic and Efficient Customization of Neural Networks for ML Applications. Yuhan Liu, Chengcheng Wan, Kuntai Du, Henry Hoffmann, Junchen Jiang, Shan Lu, Michael Maire. **From UChicago** [link](https://arxiv.org/abs/2310.04685)
- MonoInfer: Enabling a New Monolithic Optimization Space for Neural Network Inference Tasks on Modern GPU-Centric Architectures. Donglin Zhuang, Zhen Zheng, Haojun Xia, Xiafei Qiu, Junjie Bai, Wei Lin, Shuaiwen Leon Song. **From USYD** 
- Taming Throughput-Latency Tradeoff in LLM Inference with Sarathi-Serve.Amey Agrawal, Nitin Kedia, Ashish Panwar, Jayashree Mohan, Nipun Kwatra, Bhargav S. Gulavani, Alexey Tumanov, Ramachandran Ramjee. **From GaTech**  [link](https://arxiv.org/pdf/2403.02310.pdf)
- Enabling Tensor Language Model to Assist in Generating High-Performance Tensor Programs for Deep Learning. **From HNU & Huawei** 
- Caravan: Practical Online Learning of In-Network ML Models with Labeling Agents. Qizheng Zhang, Ali Imran, Enkeleda Bardhi, Tushar Swamy, Muhammad Shahbaz, Kunle Olukotun. **From Stanford** 
- Dynamic Scheduling of ML Training across Geo-Distributed Datacenters: Principles and Experiences. Arnab Choudhury, Yang Wang, Tuomas Pelkonen, Kutta Srinivasan, Abha Jain, Shenghao Lin, Delia David, Siavash Soleimanifard, Michael Chen, Abhishek Yadav, Ritesh Tijoriwala, Chunqiang Tang. **From OSU** 

- USHER: Holistic Interference Avoidance for Resource Optimized ML Inference. Sudipta Saha Shubha, Haiying Shen, and Anand Iyer. 
- From MSR Parrot: Efficient Serving of LLM-based Applications with Semantic Variable. **From SJTU & MSRA**

