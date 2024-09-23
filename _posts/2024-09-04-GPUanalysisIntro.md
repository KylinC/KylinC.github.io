---
dlayout:    post
title:      GPU Analysis 入门
subtitle:   GPU Analysis from zero to zero
date:       2024-09-04
author:     Kylin
header-img: img/bg-owncloud.jpg
catalog: true
tags:
    - llmsys
---



[TOC]

### Basics

#### pytorch自定义算子



#### numba



#### triton





### Analysis Tools

#### Pytorch.autograd.profiler

```python
with torch.autograd.profiler.profile(use_cuda=True) as prof:
    torch.square(b)
prof.export_chrome_trace("logs/square.json")
print(prof.key_averages().table(sort_by="cuda_time_total", row_limit=10))
```

1) 命令行结果：

```text
-------------------------  ------------  ------------  ------------  ------------  ------------  ------------  ------------  ------------  ------------  ------------  
                     Name    Self CPU %      Self CPU   CPU total %     CPU total  CPU time avg     Self CUDA   Self CUDA %    CUDA total  CUDA time avg    # of Calls  
-------------------------  ------------  ------------  ------------  ------------  ------------  ------------  ------------  ------------  ------------  ------------  
                aten::mul         8.36%      23.000us        12.73%      35.000us      35.000us     298.000us       100.00%     298.000us     298.000us             1  
          cudaEventRecord         2.91%       8.000us         2.91%       8.000us       4.000us       0.000us         0.00%       0.000us       0.000us             2  
         cudaLaunchKernel         4.36%      12.000us         4.36%      12.000us      12.000us       0.000us         0.00%       0.000us       0.000us             1  
    cudaDeviceSynchronize        84.36%     232.000us        84.36%     232.000us     232.000us       0.000us         0.00%       0.000us       0.000us             1  
-------------------------  ------------  ------------  ------------  ------------  ------------  ------------  ------------  ------------  ------------  ------------  
Self CPU time total: 275.000us
Self CUDA time total: 298.000us
```

2. 可以在`chrome://tracing/`中查看profile文件

![v2-93f150a1ac8525db9690448a82f6ba31_1440w](http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/v2-93f150a1ac8525db9690448a82f6ba31_1440w.webp)

3. 使用torch.profiler

```python
import torch
from torch.profiler import profile, record_function, ProfilerActivity


# ## Default way to use profiler
with profile(activities=[ProfilerActivity.CPU, ProfilerActivity.CUDA]) as prof:
    for _ in range(10):
        a = torch.square(torch.randn(10000, 10000).cuda())

prof.export_chrome_trace("logs/trace.json")
```

![v2-42f34973e0ba1cf2ac745feafe38956e_1440w](http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/v2-42f34973e0ba1cf2ac745feafe38956e_1440w.webp)



#### NCU

> Download: https://developer.nvidia.com/tools-overview/nsight-compute/get-started

在正常情况下，大多数参数并不需要使用，通常使用以下命令即可

```text
ncu --set full -o *** python3 xxx.py
```

完成后会在服务器上产生一个 `***.ncu-rep` 文件, 可以在本地用 Nsight Compute 打开。



### Reference

[^1]: 
