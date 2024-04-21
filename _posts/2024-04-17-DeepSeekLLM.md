---
dlayout:    post
title:      DeepSeekLLM Paper Reading
subtitle:   Introduction to DeepSeekLLM
date:       2024-04-16
author:     Kylin
header-img: img/swapdp.jpg
catalog: true
tags:
    - paper
---



[TOC]

### Abstract



### Architecture

The micro design of DeepSeek LLM largely follows the design of LLaMA (Touvron et al., 2023a,b), adopting a Pre-Norm structure with RMSNorm (Zhang and Sennrich, 2019) function and using SwiGLU (Shazeer, 2020) as the activation function for the Feed-Forward Network (FFN), with an intermediate layer dimension of 8/3 𝑑_𝑚𝑜𝑑𝑒𝑙. It also incorporates Rotary Embedding (Su et al., 2024) for positional encoding. To optimize inference cost, the 67B model uses GroupedQuery Attention (GQA) (Ainslie et al., 2023) instead of the traditional Multi-Head Attention (MHA).

着重拓展深度而不是宽度。



### Reference

[^1]: DeepSeek LLM Scaling Open-Source Language Models with Longtermism. https://arxiv.org/pdf/2401.02954.pdf











