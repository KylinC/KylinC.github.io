---
dlayout:    post
title:      高德2025届算法笔试
subtitle:   coding to Meituan
date:       2024-03-12
author:     Kylin
header-img: img/bg_NLPseg.jpg
catalog: true
tags:
    - job
---



[TOC]

### 3.12 算法

#### 选择

（1）Batch Normalization 的细节

```python
import torch.nn as nn

class MyModel(nn.Module):
    def __init__(self):
        super(MyModel, self).__init__()
        self.fc1 = nn.Linear(784, 256)
        self.bn1 = nn.BatchNorm1d(256)
        self.relu1 = nn.ReLU()
        self.fc2 = nn.Linear(256, 10)
    
    def forward(self, x):
        x = self.fc1(x)
        x = self.bn1(x)
        x = self.relu1(x)
        x = self.fc2(x)
        return x

# 训练模式
model.train()
# 在训练过程中，Batch Normalization使用每个mini-batch的均值和方差，
# 并更新全局的移动平均值

# 评估模式
model.eval()
# 在评估过程中，Batch Normalization使用训练过程中计算得到的
# 均值和方差的移动平均值，而不是在测试数据上重新计算
```

（2）AVL在插入一个序列时，平衡因子的变化

（3）循环有序数组做二分查找时候第一个比较的数

（4）多任务学习的损失函数

（5）多头注意力是怎么合并的

#### OJ

给你一个整数数组 nums 和一个整数 k ，编写一个函数来判断该数组是否含有同时满足下述条件的连续子数组：

子数组大小至少为 2，且子数组元素总和为 k 的倍数。
如果存在，返回 true ；否则，返回 false 。

如果存在一个整数 n ，令整数 x 符合 x = n * k ，则称 x 是 k 的一个倍数。0 始终视为 k 的一个倍数。

#### 大题

实现一个简单的文本编辑器，支持以下操作：
	1	append(s: str)：在文本末尾追加字符串 s。
	2	delete(k: int)：删除文本末尾的 k 个字符。
	3	print_text()：打印当前文本。
	4	undo()：撤销上一次的操作（只需撤销一步）。
编写一个类 TextEditor 实现上述功能：

    class TextEditor:
        def __init__(self):
            # Your code here
            pass
    def append(self, s: str):
        # Your code here
        pass
    
    def delete(self, k: int):
        # Your code here
        pass
    
    def print_text(self):
        # Your code here
        pass
    
    def undo(self):
        # Your code here
        pass

**示例**

editor = TextEditor()
editor.append("Hello")
editor.append(" World")
editor.print_text()  # 输出 "Hello World"
editor.delete(5)
editor.print_text()  # 输出 "Hello"
editor.undo()
editor.print_text()  # 输出 "Hello World"
