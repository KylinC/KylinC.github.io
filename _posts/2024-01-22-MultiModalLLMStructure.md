---
dlayout:    post
title:      MLLM Architecture
subtitle:   MLLM 经典结构详解
date:       2024-01-22
author:     Kylin
header-img: img/llminterview.jpg
catalog: true
tags:
    - job
---



[TOC]

### **Flamingo**[^1]

![截屏2024-01-22 09.52.40](http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2024-01-22%2009.52.40.png)

主要两个结构：

- perceiver resampler：类似DETR，通过设计多个Perceiver Resampler来生成64个固定长度的tokens，主要作用在于可以从图像中提取固定长度的特征向量，能够解决图像甚至多帧视频的feature map不一致的问题。

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2024-01-22%2009.54.52.png" alt="截屏2024-01-22 09.54.52" style="zoom:30%;" />

- XAttn-Dense：在每一层LLM上都会增加corss- attention以入到LLM中与视觉向量进行交互，融合多模态信息。

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2024-01-22%2009.56.18.png" alt="截屏2024-01-22 09.56.18" style="zoom:30%;" />



### **BLIP-2**[^2][^6]

![截屏2024-01-22 10.20.42](http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2024-01-22%2010.20.42.png)



### MiniGPT-4[^3]

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2024-01-22%2013.11.01.png" alt="截屏2024-01-22 13.11.01" style="zoom:50%;" />



### LLaVA[^4]

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2024-01-22%2013.13.51.png" alt="截屏2024-01-22 13.13.51" style="zoom:50%;" />



### mPLUG-Owl[^5]

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2024-01-22%2012.46.52.png" alt="截屏2024-01-22 12.46.52" style="zoom:57%;" />



### Reference

[^1]: Alayrac, Jean-Baptiste, et al. "Flamingo: a visual language model for few-shot learning." *Advances in Neural Information Processing Systems* 35 (2022): 23716-23736.
[^2]: Li, Junnan, et al. "Blip-2: Bootstrapping language-image pre-training with frozen image encoders and large language models." *arXiv preprint arXiv:2301.12597* (2023).
[^3]: Zhu, Deyao, et al. "Minigpt-4: Enhancing vision-language understanding with advanced large language models." *arXiv preprint arXiv:2304.10592* (2023).
[^4]:Liu, Haotian, et al. "Visual instruction tuning." *arXiv preprint arXiv:2304.08485* (2023).
[^5]: Ye, Qinghao, et al. "mplug-owl: Modularization empowers large language models with multimodality." *arXiv preprint arXiv:2304.14178* (2023).
[^6]: BLIP2中Q-former详解. https://blog.csdn.net/m0_37708614/article/details/134122314
[^7]: 基于LLMs的多模态大模型(MiniGPT-4, LLaVA, mPLUG-Owl, InstuctBLIP, X-LLM). https://blog.csdn.net/qq_39388410/article/details/130772680











