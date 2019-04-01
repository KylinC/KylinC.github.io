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
kylin@Thinkstation:_includes kylinchan$ cd ~/.
kylin@Thinkstation:~ kylinchan$ 
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
kylin@Thinkstation:unity kylinchan$ touch a.txt
kylin@Thinkstation:unity kylinchan$ ls
FPS1_1		FPSME 1.05	a.txt
```
或者加上相对路径的 touch 命令

```<?
kylin@Thinkstation:~ kylinchan$ touch unity/b.txt
kylin@Thinkstation:~ kylinchan$ cd unity
kylin@Thinkstation:unity kylinchan$ ls
FPS1_1		FPSME 1.05	a.txt		b.txt
```

- **pwd 显示当前目录**

- **mv 命令移动文件**

- **cp 复制文件**

- **rm 删除文件**
