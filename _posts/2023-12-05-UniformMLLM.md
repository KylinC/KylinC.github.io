---
dlayout:    post
title:      A Survey on Unified Multi-Modal Models
subtitle:   统一多模态模型研究调研
date:       2023-12-05
author:     Kylin
header-img: img/uniformLLM.jpg
catalog: true
tags:
    - paper
---



[TOC]

### Comprehensive

参考[^1] 的图，介绍一下MLLM的发展主要有两个阶段：

![截屏2023-12-05 13.59.11](http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/20231205151219.png)

- 图片prompt：Resampler[^5]（Flamingo）、linear projection[^3]（Llava）、Q-former[^4]（Blip-2）这类模型还没有考虑到只能图生文，因此其对于image encoder要求不是太高，搞出来的visual embedding部分仅仅作为prompt不给予监督，训练目标是集中在根据visual content预测description上。可能的问题就是alignment压力全部给到adapter（这也是[^1]试图解决的点），并且这种模式缺乏对visual部分对监督。
- 描述图片prompt的一种表示：Kosmos-2[^7] 这类研究的MLLM能够回归图片里面的bbox才帮助回答问题，这种方法也可能用在监督Instruction Tuning上，来避免幻觉等一些问题。
- Unified：这类研究要使得图生文可行，就要借助diffusion model来进行image的解码。所以关键问题是怎么接encoder和decoder了，所以整体模块很相似。而且根据meta[^6]，图片tokenizer、transformer文生图结构至少在22年已经出现。



### cm3leon

这个研究就比较实诚，一点没魔改，全部拼起来（其实可能MLLM就是这样的）。

框架使用的是CM3 multi-modal architecture[^8]

Image Tokenizer 用的是[^9]

Retrieval Augmentation使用一个相似性匹配，也是之前的研究

objective用的是next token prediction



在image decoder部分用的Classifier Free Guidance (CFG)。

总的decoding用了一个之前只用在纯文本大模型上的Contrastive Decoding TopK (CD-K)来提升解码效果。



### Emu

Emu is then end-to-end trained with a unified **objective** of **classifying the next text token** or **regressing the next visual embedding** in the multimodal sequence. 

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/20231205165547.png" alt="截屏2023-12-05 16.55.39" style="zoom:50%;" />

- Casual Transformer把图片转化成有next-token依赖的形式，为了适用于“Unified next-token prediction”
- Image Regression最终是通过condition的方式输入进去SD的



### Dynamic Discrete Visual Tokenization

总体框架如下，和emu相似，关键在于怎么设计图片的编码和解码了。

![截屏2023-12-05 17.05.49](http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/20231205171140.png)

相比于emu分别处理图片、文字的方法，DDVT想能不能给图片也整一个离散tokenizer，这样的话objective逻辑就真正一致了，所以有了下面图示这个方法，同时训练一个动态、离散tokenizer和对应的detokenizer：

- Dynamic是由Token Selector&Merger做到的，这个结构感觉参考了Blip2的cross attention，目标就是压缩一下无关的patch。
- Discrete是由Codebook Embedding做到的。
- Reconstruction Decoder在这一步监督训练，但是只在infer中使用，解码出condition

![截屏2023-12-05 17.05.29](http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/20231205170826.png)



### NExT-GPT

总体框架十分简单，就是用了6个projection layer（projection layer用的是transformer layer）。并且这里的projection只是发生在embedding层面上，看下面alignment的原理就可以看出来。

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/20231205173619.png" alt="截屏2023-12-05 17.34.35" style="zoom:47%;" />

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/20231205173628.png" alt="截屏2023-12-05 17.36.13" style="zoom:50%;" />



### Reference

[^1]: Jin, Yang, et al. "Unified language-vision pretraining with dynamic discrete visual tokenization." arXiv preprint arXiv:2309.04669 (2023).
[^2]: Wu, Shengqiong, et al. "Next-gpt: Any-to-any multimodal llm." arXiv preprint arXiv:2309.05519 (2023). 
[^3]: Liu H, Li C, Wu Q, et al. Visual instruction tuning[J]. arXiv preprint arXiv:2304.08485, 2023.
[^4]: Li J, Li D, Savarese S, et al. Blip-2: Bootstrapping language-image pre-training with frozen image encoders and large language models[J]. arXiv preprint arXiv:2301.12597, 2023.
[^5]: Alayrac J B, Donahue J, Luc P, et al. Flamingo: a visual language model for few-shot learning[J]. Advances in Neural Information Processing Systems, 2022, 35: 23716-23736.
[^6]: Yu L, Shi B, Pasunuru R, et al. Scaling autoregressive multi-modal models: Pretraining and instruction tuning[J]. arXiv preprint arXiv:2309.02591, 2023.
[^7]: Peng Z, Wang W, Dong L, et al. Kosmos-2: Grounding Multimodal Large Language Models to the World[J]. arXiv preprint arXiv:2306.14824, 2023.
[^8]: Aghajanyan A, Huang B, Ross C, et al. Cm3: A causal masked multimodal model of the internet[J]. arXiv preprint arXiv:2201.07520, 2022.
[^9]: Gafni O, Polyak A, Ashual O, et al. Make-a-scene: Scene-based text-to-image generation with human priors[C]//European Conference on Computer Vision. Cham: Springer Nature Switzerland, 2022: 89-106.

