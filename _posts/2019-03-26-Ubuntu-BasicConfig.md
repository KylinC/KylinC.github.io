---
layout:     post
title:      Basic Configuration for Ubuntu 
subtitle:   Ubuntu18.04基本配置
date:       2019-04-03
author:     Kylin
header-img: img/basic_config.jpg
catalog: true
tags:
    - Pycharm
    - Ubuntu Driver
    - Gurobi
    - Pytorch
    - Anaconda
---

# Basic Configuration for Device  
## 显卡驱动更新/拓展屏幕配置

显卡驱动更新

```<?
sudo apt-get update

sudo ubuntu-drivers autoinstall

reboot
```
双（多）屏幕配置

 **xrandr 显示设备设置**

- 了解设备名

```<?
xrandr
```

- 设置主屏幕

（ DP-3为设备接口名指代设备，具体情况由 xrandr 了解 ）

```<?
xrandr --output DP-3 --auto --primary
```

- 设置位置关系

设置 DVI-I-0 在 DP-3 的右边

```<?
xrandr --output DVI-I-0 --right-of DP-3 --auto
```

（位置关系参数：right-of ，left-of , below）


# Configuration for Software

## Anaconda 配置

- 在 Anaconda 网站下载需要版本的 Linux 安装包

（或者在清华大学开源软件镜像站下载）

- 在 Terminal 中进入 Downloads 目录

## Pytorch 配置

## Pycharm-Anaconda 配置

## Anaconda-Gurobi 配置