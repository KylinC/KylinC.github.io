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
    - AliCloud
   
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

### QQ空间

> QQ空间定位上属于微博同类产品，自然就有自己的图床，相比腾讯云，免费是其最大的优点，但是同时不支持一件索引图片地址成为其弊端。

### 阿里云 OSS

官网地址：[https://aliyun.com](https://aliyun.com)

![](http://kylinhub.oss-cn-shanghai.aliyuncs.com/2019-04-30-aliyun-1.jpg)

- 优点：
  - 稳定而无需域名备案。
  - 图片地址易于管理。
- 缺点：
  - 需要收费。 （ 40G 约 10 RMB/Year ）

### 七牛云


官网地址：[https://portal.qiniu.com](https://portal.qiniu.com)

![](http://kylinhub.oss-cn-shanghai.aliyuncs.com/2019-04-30-qiniu.jpg)

- 优点：
  - 支付宝认证后有10G永久免费空间，每月10G国内和10G国外流量。
  - 有免费ssl证书。
- 缺点：
  - 需要 iPic 收费版。 （ 每月 6 RMB ）
  - 需要备案域名、虚拟主机进行CND配置，否则30天以后无法使用。（备案可能带来支出）

### SM.MS
  
官网地址：[https://sm.ms](https://sm.ms)

![](http://kylinhub.oss-cn-shanghai.aliyuncs.com/2019-04-30-smms.png)

- 优点：
  - 使用简便，免费。
- 缺点：
  - 单张图片最大限制 5 M

  
### 又拍云

官网地址：[https://www.upyun.com](https://www.upyun.com)

![](http://kylinhub.oss-cn-shanghai.aliyuncs.com/2019-04-30-youpai.jpg)

> 类似七牛云

### 其他

- **腾讯云 COS**
- **Amazon S3**

> 这些云储存都可以选择，要是在阿里云上注册过域名，可以考虑使用**阿里云 OSS**，配置更方便, 不过似乎都没有免费储存空间或者储存空间很小

## 七牛云 + iPic

### 注册七牛云

- 登陆七牛云官网[https://portal.qiniu.com](https://portal.qiniu.com)注册并实名认证（支持支付宝认证），等待1～2日即可领取10G免费对象储存空间。

### 创建七牛云对象储存（Bucket）

- 直接创建即可

![](http://kylinhub.oss-cn-shanghai.aliyuncs.com/2019-04-30-qiniu_obj.jpg)

> 此处**储存空间名称**即为bucket标识符，相当于本地储存器的C盘，由于标识储存路径。

### 配置iPic端七牛云

![](http://kylinhub.oss-cn-shanghai.aliyuncs.com/2019-04-30-ipic.jpg)

- Bucket: 创建的空间名字

- Access Key: 个人中心-密钥管理-创建

- Secret Key: 个人中心-密钥管理-创建

- 网址前缀： 此处可以填**融合 CDN 测试域名**(仅支持30个自然日) 或者配置CND域名（见下一步）

![](http://kylinhub.oss-cn-shanghai.aliyuncs.com/2019-04-30-netbasic.jpg)

### 配置七牛云 CND 域名（重要）

- 绑定一个域名用于 CND 加速

> 域名需备案，阿里云（原万网）支持备案但是需购买虚拟主机服务

## 阿里云OSS + iPic

### 注册阿里云

> 直接个人支付宝账号登陆，并完成实名认证

### 创建阿里云对象储存（Bucket）

- 阿里云 OSS 是属于付费增值服务，不存在免费初始空间，目前价格在 10RMB/Year/40G 左右。

> 配置为：标准储存、公共读、区域就访问点近即可

### 配置iPic端阿里云

- Bucket: 创建的空间名字

- Access Key: 个人中心-accesskeys-创建

- Secret Key: 个人中心-accesskeys-创建

- 网址前缀： 填写 Bucket外网访问域名即可

![](http://kylinhub.oss-cn-shanghai.aliyuncs.com/2019-04-30-yuming.jpg)

## 唱不了一首欢乐的歌

![](http://kylinhub.oss-cn-shanghai.aliyuncs.com/2019-04-30-love_song.jpg)

> 这首歌的 ukulele 版超棒！




