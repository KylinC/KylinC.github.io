---
dlayout:     post
title:      Install Neo4j server on AliCloud
subtitle:   在阿里云服务器上部署Neo4j
date:       2019-09-17
author:     Kylin
header-img: img/bg-neo4j.jpg
catalog: true
tags:
    - Cloud
    - Database

---



# Install Neo4j server on AliCloud



> 本文从零开始介绍了如何在阿里云服务器上安装、部署neo4j服务器，目标在于利用服务器IP访问Neo4j Browser或者Neobolt 
>
> 教程同样适用于有独立公网IP的主机，若无公网IP，可以参考另外一篇文章 [Use SSH to Connect Intranet Server](https://kylinchen.top/2019/05/29/SSH/) 



- 首先在官网下载 [Neo4j Server Community](https://neo4j.com/download-center/#community) , 依据系统需求选择，服务器一般选择Linux版的。这里可以在远端下载、之后用FTP上传，也可以直接在服务器端进行下载。

![](http://kylinhub.oss-cn-shanghai.aliyuncs.com/2019-09-17-neo1.jpg)



- 下载之后通过FTP将解压后的文件传至服务器端。
- 进入 **neo4j-community-3.3.0/conf**，编辑目录下的 neo4j.conf

```bash
cd neo4j-community-3.3.0/conf
vim neo4j.conf
```



进行如下修改 (删除注释)



![](http://kylinhub.oss-cn-shanghai.aliyuncs.com/2019-09-17-neo2.jpg)

> 解除了注释

```
dbms.connector.default_listen_address=0.0.0.0
```



![](http://kylinhub.oss-cn-shanghai.aliyuncs.com/2019-09-17-neo3.jpg)

> 填加（如上解除注释，没有的添加）
>
> 允许了三个访问端口，7687(Neobolt)、7474(Neo4j Browser) 、7473



- 在阿里云上配置安全规则(在阿里云防火墙设置处)

添加之前允许访问的三个端口，类型为自定义\TCP

![](http://kylinhub.oss-cn-shanghai.aliyuncs.com/2019-09-17-tcp.png)

- 在服务端安装 Java JDK 或是 Open JDK，由于 Open JDK在Ubuntu 上容易安装，故选择

```
sudo apt-get update
sudo apt-get install openjdk-8-jdk
```



- 在服务器端启动neo4j sever

```
./neo4j-community-3.3.0/bin/neo4j start
```



- 现在可以利用IP访问你的neo4j Browser了，即 http://ip:7474/browser/

![](http://kylinhub.oss-cn-shanghai.aliyuncs.com/2019-09-17-neobrowser.jpg)



若出现 "ServiceUnavailable: WebSocket connection failure. " 报错，则检查阿里云服务器防火墙配置

