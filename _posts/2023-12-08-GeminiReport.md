---
dlayout:    post
title:      Gemini Technic Report
subtitle:   Gemini技术报告解析
date:       2023-12-08
author:     Kylin
header-img: img/yz-bg.jpg
catalog: true
tags:
    - paper
---



[TOC]

### Model Architecture

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/20231208130043.png" alt="截屏2023-12-08 13.00.31" style="zoom:50%;" />

若干技术细节：

- decoder-only 结构
- multi-query attention
- interleaved multi-modal input data
- **text** and **image** output data
- visual encoder：Flamingo，CoCa，PaLI（with the important distinction that the models are multimodal from the beginning and can natively output images using discrete image tokens）
- visual decoder：discrete image tokens [^2][^3]
- video understanding：encoding the video as a sequence of frames，video frames or images can be interleaved naturally with text or audio as part of the model input
- audio understanding：can understand signals from Universal Speech Model (USM) 





### Reference

[^1]: Gemini: A Family of Highly Capable Multimodal Models
[^2]: Yu, Jiahui, et al. "Scaling autoregressive models for content-rich text-to-image generation." *arXiv preprint arXiv:2206.10789* 2.3 (2022): 5.
[^3]: Ramesh, Aditya, et al. "Zero-shot text-to-image generation." *International Conference on Machine Learning*. PMLR, 2021.
