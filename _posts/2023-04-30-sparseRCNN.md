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



1. 生成N个可学习的proposal boxes和N个可学习的proposal Features。前者用来去Feature Map上抠图, 后者用来生成 $1 \times 1$ Conv中的参数。
2. Input -> Backbone+FPN -> Feature Map
3. Proposal boxes去Feature Map上crop得到 $7 \times 7$ 的roi feature。
4. Roi Feature和Proposal feature一起进Dynamic Head得到预测用feature。
5. 过rcnn head出最后的预测结果。



#### Dynamic Conv

Paper: Dynamic Filter Networks
基本思想：在inference阶段之后，Conv中的部分参数应该是和输入有关的->所以是动态的。

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-05-01%2015.40.10.png" alt="截屏2023-05-01 15.40.10" style="zoom:47%;" />



#### Dynamic Head

##### ROI (region of Interest) feature

每个Proposal大小不一样而RCNN Head中会使用nn.Linear，因此需要一个操作将大小不同的Proposal, resize到同样大小的Feature。
目前最优的操作是Roi Align (使用双线性插值取点) , 一般使用的目标大小为 $7 \times 7$.

![image-20230501175148550](https://kylinhub.oss-cn-shanghai.aliyuncs.com/image-20230501175148550.png)

但是问题是 $7 \times 7$ 的 ROI feature 并不是都有用，因此需要 Attention，这就是 dynamic head 的作用。

<img src="https://kylinhub.oss-cn-shanghai.aliyuncs.com/image-20230501175457125.png" alt="image-20230501175457125" style="zoom:67%;" />



##### Dynamic Head Architecture

Dynamic Head 和 Dynamic Conv 是一样的

<img src="https://kylinhub.oss-cn-shanghai.aliyuncs.com/image-20230501184055124.png" alt="image-20230501184055124" style="zoom:87%;" />

> Code:
>
> ![image-20230501184503845](https://kylinhub.oss-cn-shanghai.aliyuncs.com/image-20230501184503845.png)
>
> BMM 这一步本质上是为了拿到 attention，论文里面提到bmm 本质是1x1的卷积: 
>
> <img src="https://kylinhub.oss-cn-shanghai.aliyuncs.com/image-20230501184558516.png" alt="image-20230501184558516" style="zoom:97%;" />



#### Iterative Refine

借鉴Cascade RCNN的思想，多个RCNN head串联, 下一个RCNN的输入是上一个RCNN的输出
这里预设的Proposal boxes和Proposal features都会被不断更新。





### Experiment

<img src="https://kylinhub.oss-cn-shanghai.aliyuncs.com/image-20230501185207580.png" alt="image-20230501185207580" style="zoom:80%;" />





### Ablation Study

做了Sparse、Iterative、Dynamic消融对比：

<img src="https://kylinhub.oss-cn-shanghai.aliyuncs.com/image-20230501185400174.png" alt="image-20230501185400174" style="zoom:80%;" />