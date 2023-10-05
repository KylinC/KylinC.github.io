---
dlayout:    post
title:      Orca the origin of continous batching
subtitle:   A Distributed Serving System for Transformer-Based Generative Models
date:       2023-9-8
author:     Kylin
header-img: img/bg-owncloud.jpg
catalog: true
tags:
    - paper
---



[TOC]

> osdi 22



这是continuous batching的思想来源：

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-09-14%2009.18.38.png" alt="截屏2023-09-14 09.18.38" style="zoom:50%;" />



实验做了一个latency和throughput的图：

![截屏2023-09-14 09.19.52](http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-09-14%2009.19.52.png)
