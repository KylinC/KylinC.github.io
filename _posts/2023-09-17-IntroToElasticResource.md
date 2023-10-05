---
dlayout:    post
title:      Elastic Resource Sharing for Distributed Deep Learning
subtitle:   Paper Scanning
date:       2023-9-17
author:     Kylin
header-img: img/bg-owncloud.jpg
catalog: true
tags:
    - paper
---



[TOC]

#### Abstract

Shortest-Remaining-Time-First (SRTF) 不是一个作业调度的好方法，主要体现在：
 (1) resource efficiency often matters more than short job prioritization
 (2) applying greedy algorithms to existing jobs inflates average job completion time(JCT) due to overly optimistic views toward future resource availability.

给出的方案：Apathetic Future Share (AFS) balances resource efficiency and short job prioritization



