---
dlayout:    post
title:      C3 Intro To Clockwork
subtitle:   Serving DNNs like Clockwork Performance Predictability from the Bottom Up
date:       2023-9-5
author:     Kylin
header-img: img/bg-owncloud.jpg
catalog: true
tags:
    - quant
---



[TOC]

> OSDI 20

clockwork 是写service文章一个重要的模版，尤其是background部分

### Abstract





### BG

service-level objectives (SLOs) 引用自 [40], 一般的形式是 latency SLO：Site Reliability Engineering: How Google Runs Production Systems

copious batching 是什么？据说之前的研究会把使用率最高的放在 GPU 里面
