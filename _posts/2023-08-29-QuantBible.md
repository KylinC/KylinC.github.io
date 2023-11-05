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



#### 4. Birthday problem  

You and your colleagues know that your boss A's birthday is one of the following 10 dates: Mar 4, Mar 5, Mar 8 Jun 4, Jun 7 Sep 1, Sep 5 Dec 1, Dec 2, Dec 8

A told you only the month of his birthday, and told your colleague C only the day. After that, you first said: "I don't know A's birthday; C doesn't know it either." After hearing what you said, C replied: "I didn't know A's birthday, but now I know it." You smiled and said: "Now I know it, too." After looking at the 10 dates and hearing your comments, your administrative assistant wrote down A's birthday without asking any questions. So what did the assistant write?  

> 你和你的同事知道你的老板 A 的生日是以下 10 个日期之一：3 月 4 日、3 月 5 日、3 月 8 日6月4日，6月7日9月1日, 9月5日12 月1日、12 月 2 日、12 月 8 日A只告诉你他生日的月份，而只告诉你的同事C生日那天。之后，你先说：“我不知道A的生日，C也不知道。”听完你的话，C回答说：“以前不知道A的生日，现在知道了。”你笑着说：“现在我也知道了。” 在查看了 10 个日期并听取了您的意见后，您的行政助理在没有询问任何问题的情况下写下了 A 的生日。那么助理写了什么？

Solution:

1) I don't know A's birthday; C doesn't know it either：不能是独立号月份，因此是Mar和Sep

2）I didn't know A's birthday, but now I know it：Mar和Sep里面的独立号，是1，4，8

3）Now I know it, too：4和8都在Mar，因此是Sep，因此是Sep 1



#### 5. Card game  

A casino offers a card game using a normal deck of 52 cards. The rule is that you turn over two cards each time. For each pair, if both are black, they go to the dealer's pile; if both are red, they go to your pile; if one black and one red, they are discarded. The process is repeated until you two go through all 52 cards. If you have more cards in your pile, you win $100; otherwise (including ties) you get nothing. The casino allows you to negotiate the price you want to pay for the game. How much would you be willing to pay to play this game?

> 赌场提供使用一副普通的 52 张牌的纸牌游戏。 规则是每次翻两张牌。 对于每一对，如果都是黑色，则它们进入庄家堆； 如果两者都是红色的，它们就会进入你的堆； 如果一黑一红，则丢弃。 重复该过程，直到你们两个完成所有 52 张卡片。 如果您的牌堆中有更多牌，您将赢得100 美元； 否则（包括关系）你什么也得不到。 赌场允许您协商要为游戏支付的价格。你愿意花多少钱玩这个游戏？

Solution: 

最终牌堆数目肯定一致，一分钱别出！



#### 6. Burning ropes  

You have two ropes, each of which takes 1 hour to bum. But either rope has different densities at different points, so there's no guarantee of consistency in the time it takes different sections within the rope to bum. How do you use these two ropes to measure 45 minutes?  

> 你有两根绳子，每根绳子燃烧需要1 小时。 但是任何一根绳子在不同点都有不同的密度，所以不能保证绳子内不同部分燃烧的时间的一致性。你如何用这两条绳子来测量 45 分钟？

Solution:

<img src="https://kylinhub.oss-cn-shanghai.aliyuncs.com/e4c8ea4f5e3b920ca18efc0caec6997.jpg" alt="e4c8ea4f5e3b920ca18efc0caec6997" style="zoom:20%;" />



#### 7. Trailing zeros  

How many trailing zeros are there in 100! (factorial of 100)?  

> 100! 中有多少个尾随零

