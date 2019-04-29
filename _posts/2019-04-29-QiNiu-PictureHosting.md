---
layout:     post
title:      Build a Picture Host with iPic/QiNiu Cloud
subtitle:   用iPic/七牛云配置图床解决微博图床框架不兼容问题
date:       2019-04-29
author:     Kylin
header-img: img/post-qiniu.png
catalog: true
tags:
    - Cloud
    - Picture Host

---

# Build a Picture Host with iPic/QiNiu Cloud

## 关于图床



> Q1: 图床是什么？

> 图床是指储存图片的服务器。不同的图床由于有空间距离等因素决定访问速度、影响图片显示速度。
***
> Q2: 图床的优点？

> 1） 把图片单独存在其他服务器上，节省空间。一张图片的大小远远大于一个文本。
>
> 2） 把图片从在其他服务器上加载，尤其是大的图床网站，更稳定快速。
***
> Q1: 图床的用户人群？

> 1） 网络博客作者，避免每次需要向网站服务器上传图片，而且 Markdown 支持图床。
> 
> 2） Github 用户，readme无需从 rep 中搭载。

## 常见图床

### 新浪微博

> 微博图床是最为常见的图床， 大多数支持图片自动上传的MarkDown编辑器如 [Typora](https://typora.io/) 和图床软件如本文配置的 [iPic](https://toolinbox.net/iPic/) 基本都是（默认）使用微博图床。

- 优点：
  - 不限空间，不限流量，永久储存，而且永久 **免费**。
  - 安全、稳定。
  - 无需配置。


> 但是似乎 jekyll 框架的网站与微博图床不太兼容（至少是近几天），图片无论在 MD 还是在 HTML 里都加载不出来，而在其他地方正常显示，所以才想到自己配置图床😔

### 七牛云

> 今日主角

官网地址：[https://portal.qiniu.com](https://portal.qiniu.com)

![](http://pqpkra92p.bkt.clouddn.com/2019-04-29-%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-04-29%20%E4%B8%8B%E5%8D%887.32.30.jpg)

- 优点：
  - 支付宝认证后有10G永久免费空间，每月10G国内和10G国外流量。
  - 有免费ssl证书。
- 缺点：
  - 需要 iPic 收费版。 （ 每月 6 RMB ）
  - 需要备案域名、虚拟主机进行CND配置，否则30天以后无法使用。

### SM.MS
  
官网地址：[https://sm.ms](https://sm.ms)

![](http://pqpkra92p.bkt.clouddn.com/2019-04-29-%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-04-29%20%E4%B8%8B%E5%8D%887.32.58.png)

- 优点：
  - 使用简便，免费。
- 缺点：
  - 单张图片最大限制 5 M

  
### 又拍云

官网地址：[https://www.upyun.com](https://www.upyun.com)

![](http://pqpkra92p.bkt.clouddn.com/2019-04-29-%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-04-29%20%E4%B8%8B%E5%8D%887.33.28.jpg)

> 类似七牛云

### 其他

- **阿里云 OSS**
- **腾讯云 COS**
- **Amazon S3**

> 这些云储存都可以选择，要是在阿里云上注册过域名，可以考虑使用**阿里云 OSS**，配置更方便, 不过似乎都没有免费储存空间或者储存空间很小

## 注册七牛云

## 配置iPic

## 配置七牛云 CND 域名






