---
dlayout:    post
title:      All about SparseRCNN
subtitle:   SparseRCNN 论文讲解
date:       2023-4-30
author:     Kylin
header-img: img/sparse.jpg
catalog: true
tags:
    - Object Detection
---



[TOC]

### Background

> SpaseRCNN 是在 DETR 之后的一个工作，与 Deformable DETR 同期

DETR去掉了 Anchor Generation 和 NMS， 但是在Decoder中，Object Query 和 Feature Map上每一个点要计算一次 Cross Attention，这部分计算仍然是密集的，而这部分操作产生的Attention Map难以训练进而导致DETR在训练阶段难收敛的问题。

SparseRCNN提出了一种两阶段的纯Sparse模型，收敛快，精度高。

#### Research Meaning

- 提出一种纯Sparse的pipeline

- 用一种奇怪的方式在使用和理解 Object Query



### Structure of Paper

#### abstract

- 原来的方法都是基于Dense Object Candidates，Anchor 数量和 feature map 的 size 有关
- SparseRCNN 只使用固定数量、可学习的 Anchor（h\*w\*k 减少为 N 个）
- 收敛快，精度高

#### Problem of previous research

- Anchor设计

原因：Anchor看数据集

- NMS 选择

原因：有数据集会遮挡

- DETR 收敛慢

Cross Attention中 Obj queries 和 Feature map 计算 attention 需要很长时间 



### Sparse RCNN

#### Dense to Sparse

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-05-01%2015.30.05.png" alt="截屏2023-05-01 15.30.05" style="zoom:67%;" />

> SparseRCNN 解决方案：
>
> 1. 去掉Anchor
> 2. 去掉NMS
> 3. 把Object Query的职责分成两部分，一部分表示为代表定位的 Box，另一部分为代表内容的feature, 两者都是可学习的。然 后通过适当的交互，得到最后检测所需feature。



#### Architecture

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-05-01%2015.35.57.png" alt="截屏2023-05-01 15.35.57" style="zoom:60%;" />

> <img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-05-01%2015.36.33.png" alt="截屏2023-05-01 15.36.33" style="zoom:50%;" />



#### Dynamic Conv

Paper: Dynamic Filter Networks
基本思想：Conv中的部分参数应该是和输入有关的->所以是动态的。

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-05-01%2015.40.10.png" alt="截屏2023-05-01 15.40.10" style="zoom:47%;" />























