---
dlayout:    post
title:      Gosper's Hack
subtitle:   Gosper's Hack原理解析
date:       2023-01-04
author:     Kylin
header-img: img/Gosper.jpg
catalog: true
tags:
    - coding
---



[TOC]

### Gosper's Hack[^1] 

Gosper‘s Hack用来逐一生成二进制枚举中n位含有k位1的二进制数。

```python
def GospersHack(k:int,n:int)->None:
	cur = (1<<k)-1
    limit = (1<<n)
    while cur<limit:
        lb = cur&-cur
        higher_bit = lb+cur
        cur = higher_bit | (((higher_bit^cur)>>2)//lb)
```



### Gosper's Hack原理解析[^2] 

![image-20240104211523873](https://kylinhub.oss-cn-shanghai.aliyuncs.com/image-20240104211523873.png)

### 时间复杂度

$$O(C_n^k)$$

即子集个数



### Reference

[^1]: http://programmingforinsomniacs.blogspot.com/2018/03/gospers-hack-explained.html
[^2]: 算法学习笔记(75): Gosper's Hack https://zhuanlan.zhihu.com/p/360512296


