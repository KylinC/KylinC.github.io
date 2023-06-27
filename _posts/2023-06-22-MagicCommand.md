---
dlayout:    post
title:      Jupyter Notebook 中的 Magic Commands
subtitle:   魔术命令 Cheatsheet
date:       2023-6-22
author:     Kylin
header-img: img/compiler.jpg
catalog: true
tags:
    - software
---



[TOC]

### 单命令模式

##### %run 运行外部Python脚本或Jupyter Notebook文件

```python
%run my_script.py
```

##### %time %timeit 测量代码块的执行时间

```python
%time sum(range(1000))
%timeit sum(range(1000))
```

##### %load  加载外部Python脚本或文本文件的内容到一个代码单元格。

```python
%load my_script.py
```

##### %reset 重置交互式命名空间中的名称定义

```python
%reset -f
#### 清空notebook执行的所有命令，清除变量
```

##### %whos 显示当前会话中定义的所有变量和它们的详细信息。

```python
%whos
```

##### %debug 进入交互式调试器来调试代码

```python
%debug
```

##### %history -n 显示执行历史记录，包括输入和输出。

```python
%history -n
```



### 单元格模式

##### %%writefile 将代码单元格的内容写入文件

```python
%%writefile my_script.py
print("Hello, World!")
```

##### %%bash %%sh 在代码单元格中运行Bash或Shell命令

```python
%%bash
ls -l
```

##### %%html 在代码单元格中呈现HTML内容。

```python
%%html
<h1>This is a heading</h1>
<p>This is a paragraph.</p>
```

##### %%latex 在代码单元格中呈现LaTex数学公式

```python
%%latex
\begin{align}
E = mc^2
\end{align}
```

##### %%latex 在代码单元格中呈现LaTex数学公式

```python
%%latex
\begin{align}
E = mc^2
\end{align}
```

##### %%time 测量整个单元格的执行时间

```python
%%time
for i in range(1000):
    pass
```

##### %%capture 捕获并禁止单元格的输出

```python
%%capture captured_output
print("This output is captured.")
```

##### %%matplotlib 设置Matplotlib 的工作方式

```python
%matplotlib inline
import matplotlib.pyplot as plt
plt.plot([1, 2, 3, 4])
plt.show()
```
