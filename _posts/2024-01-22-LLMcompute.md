---
dlayout:    post
title:      GPT2参数量准确计算
subtitle:   LLM参数量估计
date:       2024-01-21
author:     Kylin
header-img: img/llminterview.jpg
catalog: true
tags:
    - competition
---



[TOC]

### LLM参数量

#### e.g. GPT2-small 参数量计算

```
{
  "activation_function": "gelu_new",
  "architectures": [
    "GPT2LMHeadModel"
  ],
  "attn_pdrop": 0.1,
  "bos_token_id": 50256,
  "embd_pdrop": 0.1,
  "eos_token_id": 50256,
  "initializer_range": 0.02,
  "layer_norm_epsilon": 1e-05,
  "model_type": "gpt2",
  "n_ctx": 1024,
  "n_embd": 768,
  "n_head": 12,
  "n_layer": 12,
  "n_positions": 1024,
  "resid_pdrop": 0.1,
  "summary_activation": null,
  "summary_first_dropout": 0.1,
  "summary_proj_to_labels": true,
  "summary_type": "cls_index",
  "summary_use_proj": true,
  "task_specific_params": {
    "text-generation": {
      "do_sample": true,
      "max_length": 50
    }
  },
  "vocab_size": 50257
}
```

- input embedding

词表embedding 50257*768 = 38597376

(不计入，不可学习) 位置编码embedding 1024*768 = 786432

- transformer block * 12

attention模块权重矩阵加偏置：768 \* 768 \* 3 + 768 \* 3 = 1771776

attention结尾的线性变换：768 \* 768 + 768 = 590592

第一层全连接：768 \* 768 \* 4 + 768 \* 4 = 2362368

第二层全连接：768 \* 768 \* 4 + 768 = 2360064

2个Layer Normalization (其中参数量只涉及 $\gamma$ 和 $\beta$): 2 \* (768 + 768) = 3072

- output embedding

一般和input embedding共享参数，一般bert、GPT都是采用这样的策略

相加上述数据，得到总的参数量为0.12365184B，与官方公布的124M一致

```python
hidden_dim = 768
vocal_size = 50257
n_layers = 12

hidden_dim_2 = 4*hidden_dim
res = 0
res += hidden_dim * vocal_size
for i in range(n_layers):
    res += 4*(hidden_dim * hidden_dim + hidden_dim)
    res += 2*(2*hidden_dim)
    res += hidden_dim*hidden_dim_2+hidden_dim_2
    res += hidden_dim_2*hidden_dim+hidden_dim
print(f"{res/10**9}B")
```



### Reference

[^1]: 分析transformer模型的参数量、计算量、中间激活、KV cache. https://zhuanlan.zhihu.com/p/624740065
[^2]: GPT（四）GPT2参数量剖析 https://zhuanlan.zhihu.com/p/640501114
[^3 ]: 利用huggingface深入理解GPT模型结构. https://zhuanlan.zhihu.com/p/617643272







