---
dlayout:    post
title:      淘天2025届算法面筋
subtitle:   interview to TT
date:       2024-04-01
author:     Kylin
header-img: img/bg_NLPseg.jpg
catalog: true
tags:
    - job
---



[TOC]

### 一面

项目：

- CrossAtt Adaptor的作用

- Vision Token用几个

- MLLM预训练分作几个阶段

- 了解的MLLM架构

- mplug怎么处理视频

```
video_attention_mask = torch.ones(video_embeds.size()[:-1], dtype=torch.long, device=video_embeds.device)  
video_attention_mask = einops.rearrange(video_attention_mask, 'b t n -> b (t n)')
```

video_attention_mask初始形状与video_embeds除最后一维外形状相同,全部由1填充。之后使用einops库的rearrange方法,将时间维度t和空间维度n合并,得到每个视频样本的attention mask,形状为(batch_size, num_frames * num_patches)。这个mask将在之后用于abstractor模型。

```
query_tokens = self.query_tokens.expand(video_embeds.shape[0], -1, -1)
temporal_query_tokens = self.temporal_query_tokens.expand(video_embeds.shape[0], -1, -1)
```

query_tokens和temporal_query_tokens都是可学习的查询向量,形状分别是(1, num_query_tokens, hidden_size)。这里将它们扩展到与当前batch中视频样本数相同,即(batch_size, num_query_tokens, hidden_size)。

- 介绍一下LLaVa，为什么用线性层就没有cross attention好

- LLM是怎么捕捉到图像的二维特征？

md这不是因为ViT吗；**自注意力机制**：ViT使用自注意力机制处理这些嵌入向量，该机制允许模型在处理一个patch时，考虑到与其它所有patch的关系。这种机制能够帮助模型捕捉图像内部的长距离依赖关系，包括图像的二维空间结构。

- 最近看的论文，了解到MLLM结构，为什么会好，怎么预训练

八股：

Lora原理

Lora初始化

手撕：

给定一个含有 n 个正整数的数组和一个正整数 s ，
找出该数组中满足其和 <= s 的长度最大的 连续 子数组，并返回其长度。
如果不存在符合条件的子数组，返回 0。

示例：
● 输入：s = 7, nums = [2,3,1,2,4,3]
● 输出：3
● 解释：子数组 [2,3,1]、 [3,1,2] 、 [1,2,4]是该条件下的长度最小的子数组。

```python
res = 0 
left = 0
sum = 0
for right,num in enumerate(nums):
  sum+=num
  while sum>s:
    sum-=nums[left]
    left+=1
  res = max(res,right-left+1)

print(res)
```



### 二面

聊项目

试想怎么通过稀疏attention实现视频attention，瓶颈在哪里？

读过最近的视频多模态paper吗？

手撕

```python
# 实现attention

class attention(self):
  def __init__(self,num_heads,hidden):
    self.hidden = hidden
    self.wq = nn.Linear(hidden,hidden)
    self.wk = nn.Linear(hidden,hidden)
    self.wv = nn.Linear(hidden,hidden)
    self.num_heads = num_heads
    self.wo = nn.Linear(hidden,hidden)
  def forward(x):
    q,k,v = einop(self.wq(x),'b,s,h->b,self.num_heads,s,new_h'),
    einop(self.wk(x),'b,s,h->b,self.num_heads,s,new_h'),
    einop(self.wv(x),'b,s,h->b,self.num_heads,s,new_h'),
    attention = softmax(matmul(q,k)/sqrt(self.hidden/self.num_heads))
    return self.wo(einop(matmul(attention,v),'b,head,s,h->b,s,(head,h)'))

# 最大和连续子数组，要求至少一个元素

def maxsum(nums):
  res,minv,pre = nums[0],0,0
  for num in nums:
    pre += num
    res = max(res,pre-minv)
    minv = min(minv,pre)
  return res
```



### Reference

[^1]: 
