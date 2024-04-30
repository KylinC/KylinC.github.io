---
dlayout:    post
title:      LLaVA-NeXT 改进推理、OCR 和世界知识
subtitle:   LLaVA NeXT Improved reasoning, OCR, and world knowledge
date:       2024-04-25
author:     Kylin
header-img: img/bg-owncloud.jpg
catalog: true
tags:
    - coding
---



[TOC]

### Abs

相比LLaVA-1.5，LLaVA-NeXT最大的更新是在**支持分辨率**上兼容672x672, 336x1344, 1344x336 三种resolution。

据说34B性能超过Gemini-Pro



### Method

#### Dynamic High-Resolution

感觉好暴力啊

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2024-04-25%2011.02.19.png" alt="截屏2024-04-25 11.02.19" style="zoom:50%;" />

#### Data Mixture

- **High-quality User Instruct Data**：多样性，高质量回复
- **Multimodal Document/Chart Data**：模仿[Qwen-VL-7B-Chat](https://huggingface.co/Qwen/Qwen-VL)增加了ChartQA



#### Scaling LLM backbone

In addition to Vicuna-1.5 ([7B](https://huggingface.co/lmsys/vicuna-7b-v1.5) and [13B](https://huggingface.co/lmsys/vicuna-13b-v1.5)), we consider more LLMs, including [Mistral-7B](https://mistral.ai/news/announcing-mistral-7b/) and [Nous-Hermes-2-Yi-34B](https://huggingface.co/NousResearch/Nous-Hermes-2-Yi-34B).



### Model Card

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2024-04-25%2010.59.53.png" alt="截屏2024-04-25 10.59.53" style="zoom:40%;" />





### Reference

[^1]:LLaVA-NeXT: Improved reasoning, OCR, and world knowledge. https://llava-vl.github.io/blog/2024-01-30-llava-next/







