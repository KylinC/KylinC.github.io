---
layout:     post
title:      How to Use Terminal in Linux
subtitle:   Linux命令行基本命令
date:       2019-04-01
author:     Kylin
header-img: img/post-bg-terminal.jpg
catalog: true
tags:
    - Ubuntu
    - Linux
    - Terminal
   
---

# Linux 命令行初探 
## Terminal 概览

- 快捷键打开Terminal：

```<?
Alt+Ctrl+T
```

- 显示：

```<?
kylin@Thinkstation:~$ 
```
在这里，kylin 是用户名，Thinkstation 是主机名，~ 代表用户 home 目录，其地址为

```<?
/home/kylin/
```

## 基本操作

- **ls 查看当前目录下的文件** 

例如在 /home/kylin/ (home) 中进行操作：

```<?
kylin@Thinkstation:~$ ls
Applications			PycharmProjects
Applications (Parallels)	README.md
```

这就表示 /home/kylin/ 目录下有3个文件夹，和一个 markdown 文本。

参数拓展：

**ls -l 查看当前目录下的文件（完整的格式）** 

```<?
kylin@Thinkstation:~$ ls -l
total 34
drwx------@   5 kylin  staff     160  3 30 16:53 Applications
drwx------@   4 kylin  staff     128  3  5 22:44 Applications (Parallels)
```

**ls -a 查看当前目录下的文件（包括隐藏文件）** 

```<?
kylin@Thinkstation:~$ ls -a
.				.ssh
..				.subversion
```
.ssh公匙在通常情况下是处于隐藏状态的。

- **cd 命令切换目录**

```<?
kylin@Thinkstation:~$ cd PycharmProjects/
kylin@Thinkstation:~/PycharmProjects$ 
```
工作目录由 /home/kylin/ 转到 /home/kylin/PycharmProjects/ 。

参数拓展：
**cd ~/. 回到主目录**

```<?
kylin@Thinkstation:~/_includes$ cd ~/.
kylin@Thinkstation:~$ 
```


- **mkdir 新建工作目录（文件夹）**

```<?
kylin@Thinkstation:~$ mkdir file_new
kylin@Thinkstation:~$ ls
Applications			PycharmProjects
Applications (Parallels)	README.md
file_new
kylin@Thinkstation:~$ cd file_new
kylin@Thinkstation:~/file_new$ 
```

- **touch 创建文件实例**

```<?
kylin@Thinkstation:~/unity$ touch a.txt
kylin@Thinkstation:~/unity$ ls
FPS1_1		FPSME 1.05	a.txt
```
或者加上相对路径的 touch 命令

```<?
kylin@Thinkstation:~$ touch unity/b.txt
kylin@Thinkstation:~$ cd unity
kylin@Thinkstation:~/unity$ ls
FPS1_1		FPSME 1.05	a.txt		b.txt
```

- **pwd 显示当前目录**

```<?
kylin@Thinkstation:~/myblog$ pwd
/Users/kylinchan/myblog
```

- **mv 命令移动文件/文件重命名**

移动格式：mv source destination

```<?
kylin@Thinkstation:~$ mv a.txt unity/
kylin@Thinkstation:~$ cd unity
kylin@Thinkstation:~/unity$ ls
a.txt
```

重命名格式: mv oldname newname

```<?
kylin@Thinkstation:~/unity$ ls
a.txt
kylin@Thinkstation:~/unity$ mv a.txt b.txt
kylin@Thinkstation:~/unity$ ls
b.txt
kylin@Thinkstation:~/unity$ 
```

- **cp 复制文件**

格式：cp source destination

```<?
kylin@Thinkstation:~/unity$ cp b.txt des/
kylin@Thinkstation:~/unity$ cd des
kylin@Thinkstation:~/unity/des$ kylinchan$ ls
b.txt

```
参数拓展：
**cp -i 给出如是否覆盖的information提示，并复制文件**

格式：cp -i source destination

```<?
kylin@Thinkstation:~/unity$ ls
b.txt	des
kylin@Thinkstation:~/unity$ cp -i b.txt des 
overwrite des/b.txt? (y/n [n]) y
b.txt

```

**cp -r 递归复制目录**

格式：cp -r source destination

```<?
kylin@Thinkstation:~/unity$ ls
b.txt	con	des
kylin@Thinkstation:~/unity$ cp -r des con
kylin@Thinkstation:~/unity$ cd con
kylin@Thinkstation:~/unity/con$ ls
des
kylin@Thinkstation:~/unity/con$ cd des
kylin@Thinkstation:~/unity/con/des$ ls
b.txt
```

**cp -ir 信息提示递归复制目录**

格式：cp -ir source destination

- **rm 删除文件**

格式：rm filename

```<?
kylin@Thinkstation:~/unity/con/des$ ls
b.txt
kylin@Thinkstation:~/unity/con/des$ rm b.txt
kylin@Thinkstation:~/unity/con/des$ ls
kylin@Thinkstation:~/unity/con/des$
```

参数拓展：

**rm -r 递归删除文件及其子目录**

格式：rm -r foldername

**rm -r 信息提示递归删除文件及其子目录**

格式：rm -ir foldername

- **Tab补齐**

在文件夹名字记不清的时候，可以通过 Tab 键自动补齐。 当有多个满足条件的文件时，会检索出列表作为提示。

```<?
kylin@Thinkstation:~$ cd my
myblog/ mypage/ 
kylin@Thinkstation:~$ cd myblog/
```

## 环境变量

>用于路径缺省的调用

- **打开配置文件**

 ```<?
sudo gedit ~/.bashrc
```
 ```<?
/etc/profile
```

-**添加形式**
（最好写上#注释以便将来修改）

```<?
export PATH=#l absolute path you want#:$PATH
```

```<?
export PATH="#l absolute path you want#:$PATH"
```

- **bashrc Missing 的解决方案**

```<?
export PATH=/bin:/usr/bin:/usr/local/bin:/sbin:/usr/sbin
```

## 归档命令

- **ZIP压缩文件**

格式：zip target.zip filename

- **ZIP递归压缩文件夹**

格式：zip -r target.zip foldername -r

- **ZIP解压包**

格式：unzip target.zip

- **归档数据（Linux标准归档）**

格式：tar function [options] object1 object2

参数拓展：

**-x 已有的归档文件提取文件**

**-v 处理文件时显示文件**

**-f 输出结果到文件**

实例：tar -xvf target.tar

## 网络命令

- **ping 命令检查网络连接**

```<?
kylin@Thinkstation:~$ ping www.being.com
PING being.com (139.198.13.29): 56 data bytes
64 bytes from 139.198.13.29: icmp_seq=0 ttl=45 time=35.805 ms
64 bytes from 139.198.13.29: icmp_seq=1 ttl=45 time=36.788 ms
64 bytes from 139.198.13.29: icmp_seq=2 ttl=45 time=36.857 ms
64 bytes from 139.198.13.29: icmp_seq=3 ttl=45 time=37.029 ms
64 bytes from 139.198.13.29: icmp_seq=4 ttl=45 time=38.204 ms
64 bytes from 139.198.13.29: icmp_seq=5 ttl=45 time=37.285 ms
```

- **ifconfig 查看本机网路**

该命令向目标主机发送 ICMP 协议（Internet Control Message Protocol） 的echo request 数据包。如果目标主机在线且允许接受ping 请求，那么目标主机将回复 ICMP echo reply 数据包。


```<?
kylin@Thinkstation:~$ ifconfig
lo0: flags=8049<UP,LOOPBACK,RUNNING,MULTICAST> mtu 16384
······
```


- **wget 下载文件到当前目录**

```<?
kylin@Thinkstation:~$ cd unity
kylin@Thinkstation:~/unity$ wget http://cn.wordpress.org/wordpress-3.1-zh_CN.zip
······
```

参数拓展：

**wget –limit -rate URL 限速下载**


**wget -c URL 断点续传**


**wget –spider URL 测试下载链接**


**wget –tries URL 增加断点重试次数**


**wget –user-agent="user-agent" URL**


```<?
wget –user-agent="Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.162 Safari/537.36" URL
```

## 软件操作

- **更新软件列表（只会检测更新，不会安装更新）**

```<?
sudo apt-get update
```

- **更新软件（会安装更新）**

```<?
sudo apt-get upgrade
```

- **从网络安装软件**

```<?
sudo apt-get install zip
sudo apt-get install unzip
```
或者
```<?
sudo apt-get install zip unzip
```

- **从本地安装软件**

>xxx.sh 文件安装
 
```<?
bash xxx.sh
```
或者直接运行

```<?
./ xxx.sh
```

>xxx.whl 文件安装

```<?
pip install xxx.whl
```

- **卸载软件**

```<?
sudo apt-get remove zip unzip
```
- **自动修复依赖项**

```<?
sudo apt-get install -f
```

- **其它**

.deb文件安装

```<?
sudo dpkg -i filename.deb
```

## VIM 编辑器

vi 是 Unix 系统最初的编辑器，之后 GNU 项目将 vi 移植到了开源世界，在此过程中，GNU 对最初的 vi 做了一些改进，于是就有了 vim（vi improved），vim 可能是现在最繁琐也最强大的编辑器。

- VIM 安装

```<?
sudo apt-get install vim
```

- 打开文件

```<?
vim filename
```


- 输入

```<?
i + Enter     
```

- 退出

```<?
ESC + ：+ w + Enter     保存

ESC + ：+ q + Enter     退出

ESC + ：+ wq + Enter       保存并退出
```

- 查找

在非输入模式下，查找方式：

```<?
/stringToFind + Enter 

```

在非输入模式下，‘下一个’方式：

```<?
n

```

- 撤销

```<?
ESC + ：+ u + Enter      撤销

```

## Gedit 图形编辑器
Gedit 是Ubuntu中的图形化编辑器。除此之外，还有atom与sublime推荐下载安装。

```<?
kylin@Thinkstation:~/unity$ Gedit
```


