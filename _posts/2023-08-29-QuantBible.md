---
dlayout:    post
title:      Quantitative Finance Interviews 50
subtitle:   Quant Review 50 Brain Teasers
date:       2023-8-29
author:     Kylin
header-img: img/quant.jpg
catalog: true
tags:
    - quant
---



[TOC]

#### 1. Screwy pirates

Five pirates looted a chest full of 100 gold coins. Being a bunch of democratic pirates, they agree on the following method to divide the loot:

The most senior pirate will propose a distribution of the coins. AH pirates, including the most senior pirate, will then vote. If at least 50% of the pirates (3 pirates in this case) accept the proposal, the gold is divided as proposed. If not, the most senior pirate will be fed to shark and the process starts over with the next most senior pirate... The process is repeated until a plan is approved. You can assume that all pirates are perfectly rational: they want to stay alive first and to get as much gold as possible second. Finally, being blood-thirsty pirates, they want to have fewer pirates on the boat if given a choice between otherwise equal outcomes. How will the gold coins be divided in the end?

> 五个海盗抢走了一个装满 100 枚金币的箱子。 作为一群民主的海盗，他们同意以下分配战利品的方法：最资深的海盗将提议分配硬币。 所有的海盗，包括最资深的海盗，都会进行投票。 如果至少50% 的海盗（在本例中为 3 名海盗）接受提议，则黄金按提议进行分配。如果没有，则将最高级的海盗喂给鲨鱼，然后从下一个最高级的海盗开始这个过程•… 重复这个过程，直到一个计划被批准。你可以假设所有的海盗都是完全理性的：他们首先想活下去，其次是获得尽可能多的金子。最后，作为嗜血的海盗，如果在其他方面均等的结果之间做出选择，他们希望船上的海盗更少。金币最终将如何分配？

Solution：

<img src="https://kylinhub.oss-cn-shanghai.aliyuncs.com/image-20231031004000514.png" alt="image-20231031004000514" style="zoom:20%;" />

递推，从最简单的version开始推导



#### 2. Tiger and Sheep

One hundred tigers and one sheep are put on a magic island that only has grass. Tigers can eat grass, but they would rather eat sheep. Assume: A. Each time only one tiger can eat one sheep, and that tiger itself will become a sheep after it eats the sheep. B. All tigers are smart and perfectly rational and they want to survive. So will the sheep be eaten?

> 一百只老虎和一只羊被放在一个只有草的魔法岛上。 老虎吃草，但他们宁愿吃羊。假设： A. 每次只有一只老虎可以吃一只羊，而这只老虎吃完羊后自己也会变成羊。B. 所有的老虎都很聪明，而且非常理性，它们都想生存。那么羊会被吃掉吗？

Solution:

<img src="https://kylinhub.oss-cn-shanghai.aliyuncs.com/image-20231101094429474.png" alt="image-20231101094429474" style="zoom:23%;" />

递推：

基础情况，一只被吃，两只谁也不敢吃

三只会吃，因为会转移到两只谁也不敢吃的“安全状态”



#### 3. River crossing  

Four people, A, B, C and D need to get across a river. The only way to cross the river is by an old bridge, which holds at most 2 people at a time. Being dark, they can't cross the bridge without a torch, of which they only have one. So each pair can only walk at the speed of the slower person. They need to get all of them across to the other side as quickly as possible. A is the slowest and takes 10 minutes to cross; B takes 5 minutes; C takes 2 minutes; and D takes 1 minute.What is the minimum time to get all of them across to the other side?

> A、B、C、D 四个人需要过河。过河的唯一方法是通过一座旧桥，一次最多可容纳 2 人。由于天黑，他们没有手电筒就无法过桥，而他们只有一个手电筒。所以每一对只能以较慢者的速度行走。 他们需要尽快让所有人都渡过对岸。 A最慢，需要10分钟才能通过； B用时5分钟； C需要2分钟； D需要1分钟。让他们全部渡过对岸的最短时间是多少？

Solution:

<img src="https://kylinhub.oss-cn-shanghai.aliyuncs.com/image-20231101100834879.png" alt="image-20231101100834879" style="zoom:20%;" />

关键性的方案就是利用C、D回传torch，务必保证A、B一组因为这样能做到最大的overlap



#### 4.

#### 5.

#### 6.

#### 7.