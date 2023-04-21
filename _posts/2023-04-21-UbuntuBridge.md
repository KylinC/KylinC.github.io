---
dlayout:    post
title:      SSH 跳板连接配置
subtitle:   vscode ssh 跳转连接内网服务器
date:       2023-4-21
author:     Kylin
header-img: img/ubuntu.jpg
catalog: true
tags:
    - Linux
---



[TOC]

### SSH 多重跳接

> 如果使用vim来debug的话，可以一层层连接到目标服务器。但是vscode的Remote-SSH需要一次ssh命令执行连接。

#### Remote SSH

有些代码我们需要在服务器上跑，但是debug有点复杂， 这时，我们可以使用 VScode 的 Remote SSH 这个拓展来实现在本地直接debug服务器上的程序， 从而提升开发体验。

#### 问题描述

我需要用先连接到一个跳板机然后再去访问内网的服务器: `local -> server_A -> server_B`.
`local` 的操作系统是`win10`. `server_A` 和 `server_B` 的操作系统都是`linux`. 为了方便日常工作， 我们使用`SSH公钥连接`.



### 配置SSH公钥

```
ssh-keygen -t rsa -C "Youremail@email.com"
cat id_rsa.pub
```

把`id_rsa.pub`里的内容复制到`server_A`和`server_B`的`~/.ssh/authorized_keys`文件中 (如果没有请新建)。



### 配置本地 ~/.ssh/config

```
Host server_A
    HostName 128.xxx.xxx.xxx 				#跳转用的机器
    Port 8088 											#对应的端口
    User user_sever_A								#username
    IdentityFile ~/.ssh/id_rsa      #根据所用bash改成给对应的格式吧, 指向私钥位置
    
Host server_B
	HostName 196.168.xx.xx
	Port 22
	User user_sever_B
	ProxyCommand ssh user_server_A@server_A -W %h:%p 	# 如果报错使用下一行
	#ProxyCommand C:\Windows\System32\OpenSSH\ssh.exe user_server_A@server_A -W %h:%p
	IdentityFile ~/.ssh/id_rsa
```



### 连接

```
ssh user_sever_B@196.168.xx.xx
```

