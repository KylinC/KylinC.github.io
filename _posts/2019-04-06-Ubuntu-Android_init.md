---
layout:     post
title:      Hello World with Android Virtual Device(AVD) in Ubuntu
subtitle:   使用 Android Virtual Device(AVD) 运行 HelloWorld.c
date:       2019-04-06
author:     Kylin
header-img: img/post-android.jpg
catalog: true
tags:
    - Ubuntu
    - Android
   
---

# Hello World with Android Virtual Device(AVD) in Ubuntu

## 环境配置

- **基本所需环境**

 JDK，Android SDK，NDK
 
- **可选环境**

 Android Studio
 
 
### JDK 安装

- 在压缩包目录下解压

```<?
sudo tar zxvf jdk-8u201-linux-x64.tar.gz
```

- 选择一个目录统一存放

```<?
sudo mv jdk1.8.0_201 /home/kylin/java
```

- 配置环境变量

```<?
sudo gedit ~/.bashrc
```

 文件尾插入

```<?
#JDK
export JAVA_HOME=/home/kylin/java/jdk1.8.0_201:$PATH
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
export PATH=${JAVA_HOME}/bin:${JRE_HOME}/bin:$PATH
```
- 非reboot应用

```<?
source ~/.bashrc
```

- 测试

```<?
java -version
```
>此时若不报错而显示版本信息则说明成功

### Android SDK 安装

- 在压缩包目录下解压

```<?
sudo tar zxvf android-sdk-linux.tgz
```

- 选择一个目录统一存放

```<?
sudo mv android-sdk-linux /home/kylin/java
```

- 测试（在 SDK 解压文件内）

```<?
./tools/android avd
```
>此时若不报错而显示图形界面说明成功

### NDK 安装

- 在压缩包目录下解压

```<?
sudo tar zxvf android-ndk-r11-linux-x86_64.tar.gz
```

- 选择一个目录统一存放

```<?
sudo mv android-ndk-r11-linux-x86_64 /home/kylin/java
```

- 配置环境变量

```<?
sudo gedit ~/.bashrc
```

 文件尾插入

```<?
#Android ndk
export PATH=/home/kylin/java/android-ndk-r11-linux-x86_64/android-ndk-linux:$PATH
```
- 非reboot应用

```<?
source ~/.bashrc
```

- 测试

```<?
ndk-build -v
```
>此时若不报错而显示版本信息则说明成功


### SDK-ADB 配置

>ADB means multi-purpose Android Debug Bridge utility in SDK.


- 检查 SDK 地址

 \#your sdk location\#/platform-tools/
 
- 如上添加环境变量并应用

```<?
#Android adb
export PATH=/home/kylin/java/android-sdk-linux/platform-tools:$PATH
```

- 测试

```<?
adb devices
```
>返回以下则成功

```<?
List of devices attached
```

## HelloWorld 文件编写

###File structure:
- hello 
 - jni
     - hello.c 
     - hello.h 
     - Android.mk

###File contains:
- hello.c

```<?
#include "hello.h"int main(int argc, char *argv[]){	printf("Hello World!\n");	return 0;}
```

- hello.h

```<?
#ifndef HELLOHEADER_H_#define HELLOHEADER_H_#include <stdio.h>#endif /*HELLOHEADER_H_*/
```

- Android.mk

```<?
LOCAL_PATH := $(call my-dir)include $(CLEAR_VARS)LOCAL_SRC_FILES := hello.c				# your source code	LOCAL_MODULE := helloARM				# output file nameLOCAL_CFLAGS += -pie -fPIE	 			# These two line mustn’t be LOCAL_LDFLAGS += -pie -fPIE			# change.LOCAL_FORCE_STATIC_EXECUTABLE := trueinclude $(BUILD_EXECUTABLE)
```

## HelloWorld 文件执行

- 编译文件

在 jni 文件夹下

```<?
ndk-build
```
在hello/libs/armeabi 目录下产生文件 helloARM

- 导入模拟器

```<?
adb push /home/kylin/OS_project/hello/libs/armeabi/helloARM /data/misc
```
模拟器路径可以自定义

- 使可执行文件并执行

进入 ADB shell

```<?
adb shell
```

```<?
chmod +x helloARMchmod 777 helloARM
```

执行

```<?
./helloARM
```

ADB shell 返回

```<?
HelloWorld！
```

