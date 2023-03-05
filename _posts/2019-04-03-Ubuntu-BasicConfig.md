---
layout:     post
title:      Basic Configuration for Ubuntu 
subtitle:   Ubuntu18.04几种常见环境的配置
date:       2019-04-03
author:     Kylin
header-img: img/basic_config.jpg
catalog: true
tags:
    - Linux
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

 （  DP-3为设备接口名指代设备，具体情况由 xrandr 了解 ）

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

- 在 [Anaconda 官网](https://www.anaconda.com/) 下载需要版本的 Linux 安装包

 （或者在  [清华大学开源软件镜像站](https://mirrors.tuna.tsinghua.edu.cn/help/anaconda/)   下载）

- 在 Terminal 中进入 Downloads 目录

```<?
cd Downloads/
```

- 运行 .sh 文件

```<?
bash Anaconda3-xxxx.sh
```
- 修改环境变量

```<?
sudo gedit ~/.bashrc
export PATH="/home/kylin/anaconda3/bin:$PATH"
```

- 非reboot生效

```<?
source ~/.bashrc
```

## CLion\Pycharm-Anaconda 配置

- 在 Terminal 中进入安装包解压目录

```<?
cd Jet/clion/bin
```

- 运行安装程序

```<?
sh ./clion.sh
```

- 同理可以安装pycharm

```<?
cd Jet/clion/bin
sh ./clion.sh
```

- 配置 Pycharm-Anaconda

 点击初始界面 config 选择 Project Interpreter

 点击 Add 在 Add Python Interpreter 界面下勾选Existing environment（Make available to all project）添加 python 路径

```<?
/home/kylin/anaconda3/bin/python
```

  Pycharm-Anaconda 的配置可以使得之后配置的gurobi在 pycharm 中运行。

## Anaconda-Gurobi 配置

### Gurobi 安装

- 下载安装包与申请密匙

     进入 [Gurobi官网](http://www.gurobi.com/) 下载安装包

 - Academic 用户可以申请为期一年的密匙 xxxxxxxxxx 字符码用于联网激活

- 进入Downloads提取文件

```<?
tar xvfz gurobi8.1.1_linux64.tar.gz
```

- 修改环境变量

```<?
sudo gedit ~/.bashrc
```
文件尾添加

```<?
export GUROBI_HOME=/home/kylin/Downloads/gurobi811/linux64
export PATH=${PATH}:${GUROBI_HOME}/bin
export LD_LIBRARY_PATH=${LD_LIBRARY_PATH}:${GUROBI_HOME}/lib
```
- 在 Downloads 目录下用密匙激活

```<?
./gurobi811/linux64/bin/grbgetkey xxxxxxx
```

- 环境变量非reboot生效

```<?
source ~/.bashrc
```

- 进入终端Gurobi

```<?
gurobi.sh
```

- Anaconda 配置

 在 Anaconda 中，只需要安装 gurobipy 包即可完成在 anaconda-python中使用 gurobi

```<?
conda config --add channels http://conda.anaconda.org/gurobi

conda install gurobi
```

- 测试

terminal内测试
（先配置默认 anaconda-python 运行， 见上文）

```<?
python
   >>import gurobipy
   >>
```

pycharm(.py文件头添加)内测试

```<?
import gurobipy

```
若无报错信息出现，则均证明配置成功

## Pytorch（GPU）配置

### CUDA 与 CUDNN 安装

- 查看显卡型号

```<?
ubuntu-drivers devices
```
 （ model之后的即是显卡型号 ）

  在 [NAVIDIA官网](https://developer.nvidia.com/cuda-gpus) 核实是否支持GPU运算

- gcc 降级（确保 CUDA 支持，caffe 需 gcc 5.0+）

```<?
sudo apt-get install gcc-4.8

sudo apt-get install g++-4.8
```

之后进入以下目录修改

```<?
cd /usr/bin

ls -l gcc*
```

修改 gcc 链接

```<?
sudo mv gcc gcc.bak 
sudo ln -s gcc-4.8 gcc 
```

对 g++ 进行同样的操作

```<?
ls -l g++*

sudo mv g++ g++.bak
sudo ln -s g++-4.8 g++
```

查看版本号核实

```<?
gcc --version

g++ --version
```

- 安装 CUDA

在 Terminal 安装包目录下

```<?
sudo sh cuda_9.0.176_384.81_linux.run
```

按照以下命令执行 (只有 Nvidia 驱动无需更新)

```<?
Do you accept the previously read EULA? (accept/decline/quit): accept 
You are attemptingto install on an unsupported configuration. Do you wish to continue? ((y)es/(n)o) [ default is no ]: y 
Install NVIDIA Accelerated Graphics Driver for Linux-x86_64 352.39? ((y)es/(n)o/(q)uit): n 
Install the CUDA 8.0 Toolkit? ((y)es/(n)o/(q)uit): y 
Enter Toolkit Location [ default is /usr/local/cuda-8.0 ]: 
Do you want to install a symbolic link at /usr/local/cuda? ((y)es/(n)o/(q)uit): y
Install the CUDA 8.0 Samples? ((y)es/(n)o/(q)uit): y Enter CUDA Samples Location [ default is /home/kyle ]:
```
配置环境变量

```<?
sudo gedit ~/.bashrc
```

在文件尾插入

```<?
export PATH="/usr/local/cuda-9.0/bin:$PATH" 
export LD_LIBRARY_PATH="/usr/local/cuda-9.0/lib64:$LD_LIBRARY_PATH"
```

环境变量非reboot应用

```<?
source ~/.bashrc
```

测试

>Terminal 输入

```<?
nvcc -V
```
>Terminal 正常返回

```<?
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2017 NVIDIA Corporation
Built on Fri_Sep__1_21:08:03_CDT_2017
Cuda compilation tools, release 9.0, V9.0.176
```

主目录下进入 Sample 测试

>进入 sample 目录

```<?
cd NVIDIA_CUDA-9.0_Samples/0_Simple/vectorAdd
```

>编译

```<?
make
```
>编译正常返回

```<?
"/usr/local/cuda-9.0"/bin/nvcc -ccbin g++ -I../../common/inc  -m64    -gencode arch=compute_30,code=sm_30 -gencode arch=compute_35,code=sm_35 -gencode arch=compute_37,code=sm_37 -gencode arch=compute_50,code=sm_50 -gencode arch=compute_52,code=sm_52 -gencode arch=compute_60,code=sm_60 -gencode arch=compute_70,code=sm_70 -gencode arch=compute_70,code=compute_70 -o vectorAdd.o -c vectorAdd.cu
"/usr/local/cuda-9.0"/bin/nvcc -ccbin g++   -m64      -gencode arch=compute_30,code=sm_30 -gencode arch=compute_35,code=sm_35 -gencode arch=compute_37,code=sm_37 -gencode arch=compute_50,code=sm_50 -gencode arch=compute_52,code=sm_52 -gencode arch=compute_60,code=sm_60 -gencode arch=compute_70,code=sm_70 -gencode arch=compute_70,code=compute_70 -o vectorAdd vectorAdd.o 
mkdir -p ../../bin/x86_64/linux/release
cp vectorAdd ../../bin/x86_64/linux/release
```

>执行

```<?
./vectorAdd
```

>执行正常返回

```<?
[Vector addition of 50000 elements]
Copy input data from the host memory to the CUDA device
CUDA kernel launch with 196 blocks of 256 threads
Copy output data from the CUDA device to the host memory
Test PASSED
Done
```

- 安装 CUDNN

进入 CUDNN 解压包目录 cuda

```<?
cd cuda
```

执行以下命令完成 CUDNN 安装

```<?
sudo cp lib64/* /usr/local/cuda/lib64/
sudo cp include/* /usr/local/cuda/include/
sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
```

- 安装 Pytorch

进入安装包所在目录（未解压）

```<?
pip install torch-0.3.0.post4-cp36-cp36m-linux_x86_64.whl 
```

>成功则会返回

```<?
Successfully installed torch-0.3.0.post4
```

再执行

```<?
pip install torchvision
```

>成功则会返回

```<?
Installing collected packages: torchvision
Successfully installed torchvision-0.2.2.post3
```

- Pytorch 测试

>Terminal 测试

```<?
python
>>import torch
>>
```
（无报错则成功）

>pycharm 测试

新建项目添加 pytorch_test.py 文件加入以下内容

```<?
import torch as t
x = t.rand(5,3)
y = t.rand(5,3)
if t.cuda.is_available():
    x = x.cuda()
    y = y.cuda()
    print(x+y)
```

若返回以下内容则安装成功

```<?
/home/kylin/anaconda3/bin/python /home/kylin/PycharmProjects/torch_test/test.py

 0.7126  1.1742  0.6479
 1.1892  0.7152  1.1412
 1.1193  1.2094  1.5306
 1.1129  1.2118  1.1972
 0.9273  1.0505  0.7236
[torch.cuda.FloatTensor of size 5x3 (GPU 0)]

Process finished with exit code 0

```

- 最后附上我使用的 [CUDA/CUDNN/Pytorch安装包](https://pan.baidu.com/s/1ennvO6_G38ugSEveOk_HdA) 。

测试机型是 Thinkstation-P500，显卡为 Quadro K2200， 可自行在 Nvidia 显卡支持上下载需要的 CUDA 及对应 CUDNN 版本。









