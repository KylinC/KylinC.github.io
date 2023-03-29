---
dlayout:    post
title:      Macbook M1 安装 mujoco 和 mujoco-py
subtitle:   Mac OS 13 安装 mujoco 和 mujoco-py
date:       2023-3-22
author:     Kylin
header-img: img/mujoco.jpg
catalog: true
tags:
    - software
---



[TOC]

### Macbook M1 安装 mujoco 和 mujoco-py

> 环境：
>
> Macbook M1 pro
>
> Mac OS 13.2 (22D49)



#### 安装 homebrew

安装Mac OS最为著名的包管理器 [homebrew](https://brew.sh/)：

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```



#### 安装 miniforge

miniforge是一个基于Conda的Python发行版，重要的是它支持ARM架构。

- 下载 Miniforge3: https://github.com/conda-forge/miniforge

![截屏2023-03-22 23.41.19](http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-03-22%2023.41.19.png)

- 安装

```bash
bash Miniforge3-MacOSX-arm64.sh
source ~/.zshrc  
```



##### * 切换anaconda和miniforge

对于安装了两个环境用户的切换方法

```bash
# 切换miniforge：
/Users/kylinchan/miniforge3/bin/conda init zsh

# 切换anaconda：
/opt/anaconda3/bin/conda init zsh 
/opt/anaconda3/bin/conda init bash 
```



#### 安装 gcc@11

- 查看已经安装的gcc的版本

```bash
ls /opt/homebrew/bin/gcc*
```

- 安装 Apple Command Line Tools

```bash
xcode-select --install
```

- 安装 gcc11

```bash
brew install gcc@11
```



#### 安装mujoco

安装有gui的版本 ([mujoco-2.1.1-macos-universal2.dmg](https://github.com/deepmind/mujoco/releases/download/2.1.1/mujoco-2.1.1-macos-universal2.dmg)): https://github.com/deepmind/mujoco/releases/tag/2.1.1



#### 安装mujoco-py

- 查看当前python环境是否在arm64下

```bash
$ which python3
/Users/$ID/.miniforge3/bin/python3
$ lipo -archs $(which python3)
arm64
```

- 安装

```bash
mkdir -p $HOME/.mujoco/mujoco210         # Remove existing installation if any
ln -sf /Applications/MuJoCo.app/Contents/Frameworks/MuJoCo.framework/Versions/Current/Headers/ $HOME/.mujoco/mujoco210/include
mkdir -p $HOME/.mujoco/mujoco210/bin
ln -sf /Applications/MuJoCo.app/Contents/Frameworks/MuJoCo.framework/Versions/Current/libmujoco.2.*.dylib $HOME/.mujoco/mujoco210/bin/libmujoco210.dylib
ln -sf /Applications/MuJoCo.app/Contents/Frameworks/MuJoCo.framework/Versions/Current/libmujoco.2.*.dylib /usr/local/lib/

# For M1 (arm64) mac users:
# The released binary doesn't ship glfw3, so need to install on your own
brew install glfw
ln -sf /opt/homebrew/lib/libglfw.3.dylib $HOME/.mujoco/mujoco210/bin

# Please make sure /opt/homebrew/bin/gcc-11  exists: install gcc if you haven't already
# brew install gcc
export CC=/opt/homebrew/bin/gcc-11         # see https://github.com/openai/mujoco-py/issues/605

pip install mujoco-py && python -c 'import mujoco_py'
```



















