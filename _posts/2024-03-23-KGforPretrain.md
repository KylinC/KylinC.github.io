---
dlayout:    post
title:      关于Pretrain和摩托车修理技术
subtitle:   Pretrain and How to Love
date:       2024-03-23
author:     Kylin
header-img: img/bg_NLPseg.jpg
catalog: true
tags:
    - paper
---



[TOC]

### 优化器

AdamW[^3]

Adafator[^4]



### 显存估计

显存主要包括两部分[^1]：

#### Model States

- (1) **权重**[^2]

参数量\*每个参数的内存

- (2) **梯度**

参数量\*每个参数的内存

- (3) **优化器**

AdamW: 参数量\*每个参数的内存\*2



#### Residual States

- (1) **activation**

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2024-03-23%2020.27.41.png" alt="截屏2024-03-23 20.27.41" style="zoom:50%;" />

- (2)  **Temporary buffers**

Temporary buffers used for storing intermediate results consumes non-trivial amount of memory for large models. Operations such as gradient all-reduce, or gradient norm computation tend to fuse all the gradients into a single flattened buffer before applying the operation in an effort to improve throughput. For example, the bandwidth of all-reduce across devices improves with large message sizes. While the gradient themselves are usually stored as fp16 tensors, the fused buffer can be an fp32 tensor depending on the operation. When the size of the model is large, these temporary buffer sizes are non-trivial. For example, for a model with 1.5B parameters, a flattened fp32 buffer would required 6 GB of memory.



- (3) **Memory Fragmentation**

So far we have discussed the actual memory consumption during training. Additionally, it is possible to run out of usable memory even when there is plenty of available memory. This can happen with memory fragmentation. A request for a memory will fail if there isn’t enough contiguous memory to satisfy it, even if the total available memory is larger than requested. We observe significant memory fragmentation when training very large models, resulting in out of memory issue with over 30% of memory still available in some extreme cases.



### 内存估计

pretrain和sft时候load模型都需要在内存初始化

之后数据集会占据大量内存，并且sft会更大，因为有loss_mask



### 预数据格式

文本格式：标题：文本。标题：文本。....

```python
text+=line['title']+'：'+line['summary']
for per in line['sections']:
	text+=per['title']+'：'+per['content']+'。'
text_id=tokenizer.encode(text,add_special_tokens=False)
text_id.append(tokenizer.special_tokens['<eos>'])
```

GPT的通用做法：对语料进行提前分词，对一个样本做完分词后在末尾加上一个结束符号`<eos>`，与下一个样本区分开。然后将所有的训练语料拼接成一个数组（np.uint16）以.bin二进制格式存储到磁盘上。



### 参数初始化

> 参考[^5]





### Reference

[^1]: LLM模型训练的内存消耗. https://zhuanlan.zhihu.com/p/633556934
[^2]:GPT2参数量准确计算. https://kylinchen.cn/2024/01/21/LLMcompute/
[^3]: 优化器：Adam与AdamW.https://blog.csdn.net/qq_40280673/article/details/133126283
[^4]: 大语言模型高效训练基础知识：优化器AdamW和Adafator. https://blog.csdn.net/Solo95/article/details/131609929
[^5]: 浅谈Transformer的初始化、参数化与标准化. https://kexue.fm/archives/8620/comment-page-4#comments
