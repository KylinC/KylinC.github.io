---
dlayout:    post
title:      zsh on Ubuntu
subtitle:   在ubuntu上安装zsh
date:       2023-3-15
author:     Kylin
header-img: img/ubuntu.jpg
catalog: true
tags:
    - Linux
---





## zsh on Ubuntu

#### 检查shell

>  事实证明，有时候漂亮的命令行确实不会提高生产力，but。。。
>
> 热知识：ubuntu下sh命令指的是dash，而不是bash，所以要制定bash运行脚本必须用全称。

```
cat /etc/shells  
```



#### 安装zsh

```
apt install zsh #安装zsh

chsh -s /bin/zsh #将zsh设置成默认shell（不设置的话启动zsh只有直接zsh命令即可）
```



#### 安装oh-my-zsh

```
sh -c "$(curl -fsSL https://gitee.com/shmhlsy/oh-my-zsh-install.sh/raw/master/install.sh)" #国内镜像源
```



#### 安装插件

```
#zsh-autosuggestions 命令行命令键入时的历史命令建议
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
#zsh-syntax-highlighting 命令行语法高亮插件
git clone https://gitee.com/Annihilater/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```

