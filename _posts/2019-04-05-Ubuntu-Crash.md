---
layout:     post
title:      Some Solutions for Ubuntu Crash
subtitle:   Ubuntu 死机解决方案
date:       2019-04-05
author:     Kylin
header-img: img/post-crash.jpg
catalog: true
tags:
    - Ubuntu
   
---

## Some Solutions for Ubuntu Crash

- **不能强制关机**


 Ubuntu 强制关机很容易造成驱动崩溃，而且之后在配置这些驱动需要清除卸载残留，其操作繁琐甚至比过重新安装。


- **重新安装**


 Ubuntu 或因为配置的环境不兼容，所以常备 **镜像盘**， **环境包储存盘**。
 当然，这也是实在没有办法了😔
 不过只要有准备，一个小时也就弄好了。
 
 
- **TTY终端注销**


 **Ctrl+Alt+F3**进入TTY1终端字符界面（**Ctrl+Alt+F2** 回到图形界面）, 输入用户名和密码以登录。

 然后执行以下命令注销桌面重新登录。
 
 ```<?
 sudo pkill Xorg
 ```
 或者
 
 ```<?
 sudo restart lightdm
 ```
 
- **reisub 方法**

 >SysRq是一种系统请求, 按住了Ctrl+Alt+SysRq键，这个时候输入的一切都会直接由 Linux 内核来处理，即可进行底层操作。

 在按住**Ctrl+Alt**的同时，依次按下**SysRq**、**R**、**E**、**I**、**S**、**U**、**B**，此时即可安全重启。
