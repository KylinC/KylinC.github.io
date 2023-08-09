---
dlayout:    post
title:      Lecture: Function as a Service
subtitle:   Notes for Lecture: Boris Grot
date:       2023-7-13
author:     Kylin
header-img: img/compiler.jpg
catalog: true
tags:
    - paper
---



Poster intro



- serverless

a collection of functions : dont have states

Challenge: communication bottleneck



we dont have severless framework for academic research

vHive: https://github.com/vhive-serverless/vHive



How vHive solve Cold Start (start a new function instance is slow)

State-of-the-art solution: start from a snapshot, checkpoint of microVM



conncetion time dominate the delay time of cold start

same take away 80% of the storage footprint



