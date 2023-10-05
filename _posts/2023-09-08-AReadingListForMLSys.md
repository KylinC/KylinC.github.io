---
dlayout:    post
title:      A Reading List for MLSys
subtitle:   A Reading List for MLSys
date:       2023-9-8
author:     Kylin
header-img: img/bg-owncloud.jpg
catalog: true
tags:
    - nlp
---



[TOC]

> Refer to https://jeongseob.github.io/readings_mlsys.html



## **A reading list for machine learning systems**

### **Generative & LLMs**

- [SOSP '23] Efficient Memory Management for Large Language Model Serving with PagedAttention
- [ICML '23] BPipe: Memory-Balanced Pipeline Parallelism for Training Large Language Models
- [ICML '23] Fast Inference from Transformers via Speculative Decoding
- [ICML '23] FlexGen: High-Throughput Generative Inference of Large Language Models with a Single GPU
- [ASPLOS '23] Optimus-CC: Efficient Large NLP Model Training with 3D Parallelism Aware Communication Compression
- [MICRO '22] DFX: A Low-latency Multi-FPGA Appliance for Accelerating Transformer-based Text Generation
- [OSDI '22] Orca: A Distributed Serving System for Transformer-Based Language Generation Tasks

### **Serving Systems (& inference acceleration)**

- [SOSP '23] Paella: Low-latency Model Serving with Virtualized GPU Scheduling
- [OSDI '23] AlpaServe: Statistical Multiplexing with Model Parallelism for Deep Learning Serving
- [EuroSys '23] Fast and Efficient Model Serving Using Multi-GPUs with Direct-Host-Access
- [NSDI '23] SHEPHERD: Serving DNNs in the Wild
- [ATC '22] Serving Heterogeneous Machine Learning Models on Multi-GPU Servers with Spatio-Temporal Sharing
- [OSDI '22] Achieving μs-scale Preemption for Concurrent GPU-accelerated DNN Inferences
- [ATC '21] INFaaS: Automated Model-less Inference Serving
- [OSDI '20] Serving DNNs like Clockwork: Performance Predictability from the Bottom Up
- [ISCA '20] MLPerf Inference Benchmark
- [SOSP '19] Nexus: A GPU Cluster Engine for Accelerating DNN-Based Video Analysis
- [ISCA '19] MnnFast: a fast and scalable system architecture for memory-augmented neural networks
- [EuroSys '19] μLayer: Low Latency On-Device Inference Using Cooperative Single-Layer Acceleration and Processor-Friendly Quantization
- [EuroSys '19] GrandSLAm: Guaranteeing SLAs for Jobs in Microservices Execution Frameworks
- [OSDI '18] Pretzel: Opening the Black Box of Machine Learning Prediction Serving Systems
- [NSDI '17] Clipper: A Low-Latency Online Prediction Serving System

### **Parallelism & Distributed Systems**

- [NSDI '23] ARK: GPU-driven Code Execution for Distributed Deep Learning
- [OSDI '22] Unity: Accelerating DNN Training Through Joint Optimization of Algebraic Transformations and Parallelization
- [EuroSys '22] Varuna: Scalable, Low-cost Training of Massive Deep Learning Models
- [SC '21'] Chimera: Efficiently Training Large-Scale Neural Networks with Bidirectional Pipelines
- [ICML '21] PipeTransformer: Automated Elastic Pipelining for Distributed Training of Large-scale Models
- [OSDI '20] A Unified Architecture for Accelerating Distributed DNN Training in Heterogeneous GPU/CPU Clusters
- [ATC '20] HetPipe: Enabling Large DNN Training on (Whimpy) Heterogeneous GPU Clusters through Integration of Pipelined Model Parallelism and Data Parallelism
- [NeurIPS '19] GPipe: Efficient Training of Giant Neural Networks using Pipeline Parallelism
- [SOSP '19] A Generic Communication Scheduler for Distributed DNN Training Acceleration
- [SOSP '19] PipeDream: Generalized Pipeline Parallelism for DNN Training
- [EuroSys '19] Parallax: Sparsity-aware Data Parallel Training of Deep Neural Networks
- [arXiv '18] Horovod: fast and easy distributed deep learning in TensorFlow
- [ATC '17] Poseidon: An Efficient Communication Architecture for Distributed Deep Learning on GPU Clusters
- [EuroSys '16] STRADS: A Distributed Framework for Scheduled Model Parallel Machine Learning
- [EuroSys '16] GeePS: Scalable Deep Learning on Distributed GPUs with a GPU-specialized Parameter Server
- [OSDI '14] Scaling Distributed Machine Learning with the Parameter Server
- [NIPS '12] Large Scale Distributed Deep Networks

### **GPU Cluster Management**

- [OSDI '22] Looking Beyond GPUs for DNN Scheduling on Multi-Tenant Clusters
- [NSDI '22] MLaaS in the Wild: Workload Analysis and Scheduling in Large-Scale Heterogeneous GPU Clusters
- [OSDI '21] Pollux: Co-adaptive Cluster Scheduling for Goodput-Optimized Deep Learning
- [NSDI '21] Elastic Resource Sharing for Distributed Deep Learning
- [OSDI '20] Heterogeneity-Aware Cluster Scheduling Policies for Deep Learning Workloads
- [OSDI '20] AntMan: Dynamic Scaling on GPU Clusters for Deep Learning
- [NSDI '20] Themis: Fair and Efficient GPU Cluster Scheduling
- [EuroSys '20] Balancing Efficiency and Fairness in Heterogeneous GPU Clusters for Deep Learning
- [NSDI '19] Tiresias: A GPU Cluster Manager for Distributed Deep Learning
- [ATC '19] Analysis of Large-Scale Multi-Tenant GPU Clusters for DNN Training Workloads
- [OSDI '18] Gandiva: Introspective cluster scheduling for deep learning

### **Memory Management for Machine Learning**

- [ASPLOS '23] DeepUM: Tensor Migration and Prefetching in Unified Memory
- [ATC '22] Memory Harvesting in Multi-GPU Systems with Hierarchical Unified Virtual Memory
- [MobiSys '22] Memory-efficient DNN Training on Mobile Devices
- [HPCA '22] Enabling Efficient Large-Scale Deep Learning Training with Cache Coherent Disaggregated Memory Systems
- [ASPLOS '20] Capuchin: Tensor-based GPU Memory Management for Deep Learning
- [ASPLOS '20] SwapAdvisor: Push Deep Learning Beyond the GPU Memory Limit via Smart Swapping
- [ISCA '19] Interplay between Hardware Prefetcher and Page Eviction Policy in CPU-GPU Unified Virtual Memory
- [ISCA '18] Gist: Efficient Data Encoding for Deep Neural Network Training
- [PPoPP '18] SuperNeurons: Dynamic GPU Memory Management for Training Deep Neural Networks
- [MICRO '16] vDNN: Virtualized Deep Neural Networks for Scalable, Memory-Efficient Neural Network Design

### **Scheduling & Resource Management**

- [ASPLOS '23] ElasticFlow: An Elastic Serverless Training Platform for Distributed Deep Learning
- [arXiv '22] EasyScale: Accuracy-consistent Elastic Training for Deep Learning
- [MLSys '22] VirtualFlow: Decoupling Deep Learning Models from the Underlying Hardware
- [SIGCOMM '22] Multi-resource interleaving for deep learning training
- [EuroSys '22] Out-Of-Order BackProp: An Effective Scheduling Technique for Deep Learning
- [ATC '21] Zico: Efficient GPU Memory Sharing for Concurrent DNN Training
- [NeurIPS '20] Nimble: Lightweight and Parallel GPU Task Scheduling for Deep Learning
- [OSDI' 20] KungFu: Making Training in Distributed Machine Learning Adaptive
- [OSDI '20] PipeSwitch: Fast Pipelined Context Switching for Deep Learning Applications
- [MLSys '20] Salus: Fine-Grained GPU Sharing Primitives for Deep Learning Applications
- [SOSP '19] Generic Communication Scheduler for Distributed DNN Training Acceleration
- [EuroSys '18] Optimus: An Efficient Dynamic Resource Scheduler for Deep Learning Clusters
- [HPCA '18] Applied Machine Learning at Facebook: A Datacenter Infrastructure Perspective

### **Deep Learning Compiler**

- [PLDI '21] DeepCuts: A Deep Learning Optimization Framework for Versatile GPU Workloads
- [OSDI '18] TVM: An Automated End-to-End Optimizing Compiler for Deep Learning

### **Deep Learning Recommendation Models**

- [SOSP '23] Bagpipe: Accelerating Deep Recommendation Model Training
- [OSDI '22] FAERY: An FPGA-accelerated Embedding-based Retrieval System
- [OSDI '22] Ekko: A Large-Scale Deep Learning Recommender System with Low-Latency Model Update
- [EuroSys '22] Fleche: An Efficient GPU Embedding Cache for Personalized Recommendations
- [ASPLOS '22] RecShard: statistical feature-based memory optimization for industry-scale neural recommendation
- [HPCA '22] Hercules: Heterogeneity-Aware Inference Serving for At-Scale Personalized Recommendation
- [MLSys '21] TT-Rec: Tensor Train Compression for Deep Learning Recommendation Model Embeddings
- [HPCA '21] Tensor Casting: Co-Designing Algorithm-Architecture for Personalized Recommendation Training
- [HPCA '21] Understanding Training Efficiency of Deep Learning Recommendation Models at Scale
- [ISCA '20] DeepRecSys: A System for Optimizing End-To-End At-scale Neural Recommendation Inference
- [HPCA '20] The Architectural Implications of Facebook’s DNN-based Personalized Recommendation
- [MICRO '19] TensorDIMM: A Practical Near-Memory Processing Architecture for Embeddings and Tensor Operations in Deep Learning

### **Hardware Support for ML**

- [ISCA '18] A Configurable Cloud-Scale DNN Processor for Real-Time AI
- [ISCA '17] In-Datacenter Performance Analysis of a Tensor Processing Unit

### **ML at Mobile & Embedded Systems**

- [MobiCom '20] SPINN: Synergistic Progressive Inference of Neural Networks over Device and Cloud
- [RTSS '19] Pipelined Data-Parallel CPU/GPU Scheduling for Multi-DNN Real-Time Inference
- [ASPLOS '17] Neurosurgeon: Collaborative Intelligence Between the Cloud and Mobile Edge

### **ML Techniques for Systems**

- [ICML '20] An Imitation Learning Approach for Cache Replacement
- [ICML '18] Learning Memory Access Patterns

### **Frameworks**

- [VLDB '20] PyTorch Distributed: Experiences on Accelerating Data Parallel Training
- [NeurIPS '19] PyTorch: An Imperative Style, High-Performance Deep Learning Library
- [OSDI '18] Ray: A Distributed Framework for Emerging AI Applications
- [OSDI '16] TensorFlow: A System for Large-Scale Machine Learning
