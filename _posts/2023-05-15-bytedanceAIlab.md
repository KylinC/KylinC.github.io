---
dlayout:    post
title:      ByteDance Robots in AI Lab
subtitle:   算法岗面试细节整理
date:       2023-5-15
author:     Kylin
header-img: img/post-cache.jpg
catalog: true
tags:
    - Job-Oriented Learning
---



[TOC]

### 一面

- 自我介绍
- 项目介绍，主要追问了论文（background、EKF）
- 算法题：环形链表
- 算法题：下面c++代码为什么会segment fault

```c++
#include<iostream>
#include<cstring>
#include<string>
#include<cstdio>
#include<cstdlib>
using namespace std;

int main(){
  char* str;
  string s="hello";
  strcpy(str,&s);
  printf(str);
	return 0;
}
```

> 正确的做法：
>
> ```c++
> int main(){
>     string s = "hello";
>     char* str = new char[s.length() + 1];
>     strcpy(str, s.c_str());
>     printf("%s", str);
>     delete[] str;
>     return 0;
> }
> ```
