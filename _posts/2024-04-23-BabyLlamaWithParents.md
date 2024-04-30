---
dlayout:    post
title:      Llama 3 蒸馏实践
subtitle:   knowledge distillation from an ensemble of teachers trained on a small dataset with no performance penalty
date:       2024-04-23
author:     Kylin
header-img: img/bg-owncloud.jpg
catalog: true
tags:
    - coding
---



[TOC]





## 理论篇

Baby Llama：knowledge distillation from an ensemble of teachers trained on a small dataset with no performance penalty



### Abstract

首先在10M-word BabyLM dataset上sft两个teachers：GPT-2 and small LLaMA

接下来蒸馏到：58M-parameter LLaMA

> We trained an ensemble consisting of a GPT-2 and small LLaMA models on the developmentally- plausible, 10M-word BabyLM dataset, then distilled it into a small, 58M-parameter LLaMA model

最终表现可以超过两个teachers，并且超过无蒸馏模型



### Intro

这个文章是一个比赛的产物：BabyLM challenge[^1] has invited researchers to investigate ways of improving the sample effi- ciency of small-scale language models, by restrict- ing the training set to a *developmentally plausible* corpus, consisting mostly of transcribed speech of either 10M (strict-small track) or 100M words (strict and loose tracks).

两个赛道选的是10M赛道

Our **proposed** solution consists in distilling an ensemble of two larger “teacher” mod- els, of different architectures (GPT-2 and LLaMA), into a smaller “student” LLaMA model.



### Pretrain using distillation

#### Knowledge Distillation

KD的发展：Knowledge distillation (Bucila et al., 2006; Hinton et al., 2015) is a technique that consists in training a (usually smaller) student model to reproduce the behaviour of one or more teacher models. This method has been successfully applied to large language models, e.g. in Sanh et al. (2019).

#### Method

1）train an ensem- ble consisting of GPT-2 (Radford et al., 2019) and a small LLaMA model (Touvron et al., 2023) on the 10M-word BabyLM dataset.

2）**distill process**：两个traget，soft target是teacher-student的logits相似度，hard taregt是next token prediction的KL散度。

> The distillation process involves guiding the training of the student model using the output of the teacher models. This output, also known as soft targets, is obtained by applying a **temperature scaling** factor to the teacher’s output logits. The student model is then trained to approximate these soft targets (**with the same temperature**) in addition to the original hard targets, resulting in a model that generalizes better and therefore performs better on unseen data.

**loss funxtion**：The loss function consists of a weighted sum of the original hard target loss (cross-entropy with the true labels) and the distillation loss (KullbackLeibler divergence with the teacher's soft targets). Formally, it can be expressed as:
$$
L=\alpha L_{C E}+(1-\alpha) L_{K L}
$$
where $\alpha$ is the weight factor, $L_{C E}$ is the original cross-entropy loss, and $L_{K L}$ is the KullbackLeibler divergence.

**tokenizer**：teacher-student是一致的，并且在training阶段进行训练：We use the same to- kenizer for both the teacher and student models, with a vocabulary size of 16000; the tokenizer is trained exclusively on the *training* split.

**细节**：student在distill之前不进行独立pre-train：i.e. the student model is *not* trained conventionally before the distillation



### Dataset

strict-small track dataset consists of approximately 10M words (as counted by the UNIX wc tool)

进行过简单正则cleaning：Some simple, regex-based cleaning is performed on both datasets, e.g. to remove HTML tags from Wikipedia articles, non-verbal cues from subtitles, or even to correct I’s that were incorrectly recog- nized as l’s in OCR’ed uppercase text. The Python script responsible for the cleaning, mrclean.py, is included along with the model; it contains one function for each data source.

**分词方法**：Byte-Pair Encoding (BPE) with a vocabulary size of 16000. 

训练样本：**Each split is divided into contiguous chunks of 128 tokens**. During each epoch of pretraining, the model is presented with a new random permutation of the chunks from the training split. The vali- dation loss is computed at the end of each epoch, by iterating in order over a fixed (but randomly sampled at the beginning) subset of the “dev” split.

> We noticed that adding a random offset between 0 and 127 to each chunk lead to marginally better performance; however, due to lack of time, the final teacher and student models were trained without such an offset.





### Reference

[^1]:Alex Warstadt, Leshem Choshen, Ryan Cotterell, Tal Linzen, Aaron Mueller, Ethan Wilcox, Williams Ad- ina, and Chengxu Zhuang. 2023. Findings of the BabyLM Challenge: Sample-efficient pretraining on developmentally plausible corpora. In *Proceedings*



## 实践篇

```
git clone https://github.com/KylinC/Llama-3-Distill
git clone https://huggingface.co/datasets/KylinC/babylm
```





### Reference

[^1]:Task-specific knowledge distillation for BERT using Hugging Face Transformers. https://github.com/philschmid/knowledge-distillation-transformers-pytorch-sagemaker/blob/master/knowledge-distillation.ipynb

