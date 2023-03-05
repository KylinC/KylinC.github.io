---
layout:     post
title:      Hello World with Android Virtual Device(AVD) in Ubuntu
subtitle:   使用 Android Virtual Device(AVD) 运行 HelloWorld.c
date:       2019-04-06
author:     Kylin
header-img: img/post-android.jpg
catalog: true
tags:
    - Linux
   
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

> 文件尾插入

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

- 添加 tools 到环境变量

```<?
export PATH=/home/kylin/java/android-sdk-linux/tools:$PATH
```

这样我们就可以调用 tools 内的常用工具

>打开 AVD 图形界面

```<?
android avd
```

>自定义内核打开已有 AVD

```<?
emulator -avd test1 -kernel /home/kylin/new_kernel/android-kernel/kernel/goldfish/arch/arm/boot/zImage -show-kernel
```

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
# Android ndk
export PATH=/home/kylin/java/android-ndk-r11-linux-x86_64/android-ndk-linux:$PATH

# ndk-pre_build source
export PATH=/toolchains/arm-linux-androideabi-4.9/prebuilt/linux-x86_64/bin:$PATH
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

### File structure:
- hello 
 - jni
     - hello.c 
     - hello.h 
     - Android.mk

### File content:
- hello.c

```<?
#include "hello.h"
int main(int argc, char *argv[]){
	printf("Hello World!\n");
	return 0;
}
```

- hello.h

```<?
#ifndef HELLOHEADER_H_
#define HELLOHEADER_H_
#include <stdio.h>
#endif /*HELLOHEADER_H_*/
```

- Android.mk

```<?
LOCAL_PATH := $(call my-dir)

include $(CLEAR_VARS)
LOCAL_SRC_FILES := hello.c				# your source code	
LOCAL_MODULE := helloARM				# output file name
LOCAL_CFLAGS += -pie -fPIE	 			# These two line mustn’t be 
LOCAL_LDFLAGS += -pie -fPIE			# change.
LOCAL_FORCE_STATIC_EXECUTABLE := true

include $(BUILD_EXECUTABLE)
```

## HelloWorld 编译与Push

- 编译文件

>在 jni 文件夹下

```<?
ndk-build
```
>在hello/libs/armeabi 目录下产生文件 helloARM

- 导入模拟器

```<?
adb push /home/kylin/OS_project/hello/libs/armeabi/helloARM /data/misc
```
>模拟器路径可以自定义，而且甚至可以在这步中使用 chmod 使可执行之后再 push（见后）。

## .ko 文件 编写

>module 文件相当于 AVD 环境中 ARM 文件的调用头文件, 这里给的 Sample 并没有给出system call，仅仅只是为了示范 module 的上传与加载

### File structure:
 - Prob1
     - hello.c 
     - Makefile

### File content:
- sys_ptree.c 

```<?
#include<linux/module.h>
#include<linux/kernel.h>
#include<linux/init.h>
#include<linux/sched.h>
#include<linux/unistd.h>

MODULE_LICENSE("Dual BSD/GPL");
#define __NR_hellocall 356

static int (*oldcall)(void);
static int sys_hellocall(int n, char* str)
{
    printk("this is my system second call!\n the uid = %d\n str: %s\n",n,str);
    return n;
}
static int addsyscall_init(void)
{
	long *syscall = (long*)0xc000d8c4;
	oldcall=(int(*)(void))(syscall[__NR_hellocall]);
	syscall[__NR_hellocall]=(unsigned long)sys_hellocall;
	printk(KERN_INFO "module load!\n");
	return 0;
}

static void addsyscall_exit(void)
{
	long *syscall=(long*)0xc000d8c4;
	syscall[__NR_hellocall]=(unsigned long)oldcall;
	printk(KERN_INFO "module exit!\n");
}

module_init(addsyscall_init);
module_exit(addsyscall_exit);

```

- Makefile

```<?
obj-m := hello.o
KID := ~/kernel/goldfish
CROSS_COMPILE=arm-linux-androideabi-
CC=$(CROSS_COMPILE)gcc
LD=$(CROSS_COMPILE)ld

all:
	make -C $(KID) ARCH=arm CROSS_COMPILE=$(CROSS_COMPILE) M=$(shell pwd) modules

clean:
	rm -rf *.ko *.o *.mod.c *.order *.symvers
```


## .ko 文件 编译与Push

- 编译文件

>在 Prob1 文件夹下

```<?
make
```
>在当前目录下产生文件组 及 module文件 **hello.ko**

- 导入模拟器

```<?
adb push /home/kylin/OS_project/Prob1/hello.ko /data/misc
```
>模拟器路径可以自定义


## Module 模拟器内上传

>进入 ADB shell

```<?
adb shell
```
>在 /data/misc 文件目录下

```<?
insmod *.ko            #Install mod
```
> 验证 mod 已经加载

```<?
lsmod *.ko            #list mod
```
此时应该返回刚刚加载的 mod

>> tips:

1、Remove mod:

```<?
  rmmod *.ko
```
2、List mod:

```<?
  lsmod
```
3、Delete you .ko file before you want to update it.

4、Remove the mod installed before you delete .ko file.

## HelloWorld 模拟器内执行

- 使可执行文件并执行

>进入 ADB shell

```<?
adb shell
```
>在 /data/misc 文件目录下使可执行

```<?
chmod +x helloARM
chmod 777 helloARM
```

>执行

```<?
./helloARM
```

>ADB shell 返回

```<?
HelloWorld！
```

## 支持文件下载

>> [网盘链接](https://pan.baidu.com/s/1a00SH72hKPgrCDAPo5gQCA)

