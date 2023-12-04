---
dlayout:    post
title:      MacOS制作Ubuntu系统盘
subtitle:   MacOS制作Ubuntu系统盘
date:       2023-12-04
author:     Kylin
header-img: img/ubuntu.jpg
catalog: true
tags:
    - Linux
---



- 转换镜像

```
hdiutil convert -format UDRW -o ubuntu-20.04-desktop-amd64.dmg ubuntu-20.04-desktop-amd64.iso
```

- 查看u盘序号

```
diskutil list
```

- 卸载u盘

```
diskutil unmountDisk /dev/diskN # N换成u盘序号
```

- 写入镜像

```
sudo dd if=./ubuntu-20.04-desktop-amd64.dmg of=/dev/rdiskN bs=1m # N换成u盘序号
```

- eject

```
diskutil eject /dev/diskN  # N换成u盘序号
```

