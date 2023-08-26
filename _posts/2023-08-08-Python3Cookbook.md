---
dlayout:    post
title:      Python3 Cookbook
subtitle:   fast introduction to python3
date:       2023-8-8
author:     Kylin
header-img: img/python.jpg
catalog: true
tags:
    - coding
---



[TOC]



> 本文总结了三个教程，原教程链接地址：
>
> - _Python tutorial_：http://www.pythondoc.com/pythontutorial3/index.html
> - _Python for you and me_：http://pymbook.readthedocs.io/en/latest/
> - _The Python Standard Library_：https://docs.python.org/3/library/index.html



### 代码风格建议

#### shebang

脚本第一行的前两个字符 `#!` 称为 *Shebang* ，目的是告诉 shell 使用 Python 解释器执行其下面的代码。这样就可以使用`./python_script.py`执行脚本而系统不会当成bash脚本执行。

1. `#!/usr/bin/python` - 使用系统默认的Python版本。
2. `#!/usr/bin/python3` - 使用Python 3。
3. `#!/usr/bin/env python3` - 使用`env`工具找到`python3`，这样脚本可以在使用不同路径的系统上工作。

> "Shebang" 这个词的起源很有趣。它来源于两个字符：`#` 和 `!`。在编程和其他上下文中，`#` 通常被称为 "hash"（或在某些情况下为 "pound"），而 `!` 被称为 "bang"。因此，将这两个词组合起来就形成了 "hash-bang"，但通常简化为 "shebang"。
>
> 读作：/ʃiːˈbæŋ/，其中 "shee" 的发音与 "she" 相同，而 "bang" 的发音与 "band" 中的 "ban" 相似，但是以 'g' 结尾。
>
> "Shebang" 还有另一个常见的称呼，即 "hashbang"，它更直接地描述了这两个字符的名称。但是，在许多开发者和Unix用户中，“shebang”是更常用和更受欢迎的术语。



#### 空格vs制表符

- 使用 4 个空格来缩进，如果是用空格，就一直用空格缩进，不要使用制表符。
- 永远不要混用空格和制表符
- 在函数之间空一行
- 在类之间空两行
- 字典，列表，元组以及参数列表中，在 `,` 后添加一个空格。对于字典，`:` 后面也添加一个空格
- 在赋值运算符和比较运算符周围要有空格（参数列表中除外），但是括号里则不加空格：`a = f(1, 2) + g(3, 4)`



#### 单行定义多个变量

```
a , b = 45, 54
```

这是使用tuple实现的，可以用于两个值进行交换



#### 格式化输出

```python
>>> data
{'Kushal': 'Fedora', 'Jace': 'Mac', 'kart_': 'Debian', 'parthan': 'Ubuntu'}
>>> for x, y in data.items():
...     print("{} uses {}".format(x, y))
```



### 数据结构

more data type: https://docs.python.org/3/library/datatypes.html

#### 列表

- 方法 `a.append(45)` 添加元素 `45` 到列表末尾。

```python
>>> a = [23, 45, 1, -3434, 43624356, 234] 
>>> a.append(45) 
>>> a 
[23, 45, 1, -3434, 43624356, 234, 45] 
```

- 将数据插入到列表的任何位置，使用列表的 `insert()` 方法。

```python
>>> a.insert(0, 1) # 在列表索引 0 位置添加元素 1 
>>> a 
[1, 23, 45, 1, -3434, 43624356, 234, 45] 
>>> a.insert(0, 111) # 在列表索引 0 位置添加元素 111 
>>> a 
[111, 1, 23, 45, 1, -3434, 43624356, 234, 45] 
```

列表方法 `count(s)` 会返回列表元素中 `s` 的数量。我们来检查一下 `45` 这个元素在列表中出现了多少次。

```python
>>> a.count(45) 
2 
```

- 移除任意指定值，使用 `remove()` 方法。

```python
>>> a.remove(234) 
>>> a 
[111, 1, 23, 45, 1, -3434, 43624356, 45] 
```

- 反转整个列表。

```python
>>> a.reverse() 
>>> a 
[45, 43624356, -3434, 1, 45, 23, 1, 111] 
```

- 使用列表的 `extend()` 方法: 将一个列表的所有元素添加到另一个列表的末尾。

```python
>>> b = [45, 56, 90] 
>>> a.extend(b) # 添加 b 的元素而不是 b 本身 
>>> a 
[45, 43624356, -3434, 1, 45, 23, 1, 111, 45, 56, 90] 
```

- 使用列表的 `sort()` 方法进行排序，排序的前提是列表的元素是可比较的。

```python
>>> a.sort() 
>>> a 
[-3434, 1, 1, 23, 45, 45, 45, 56, 90, 111, 43624356] 
```

- 使用 `del` 关键字删除指定位置的列表元素。

```python
>>> del a[-1]
>>> a
[-3434, 1, 1, 23, 45, 45, 45, 56, 90, 111]
```

- 推导式

```python
list(map(lambda x: x**2, range(10)))
# 等价于
[x**2 for x in range(10)]
```

```python
# 嵌套
>>> [(x, y) for x in [1,2,3] for y in [3,1,4] if x != y]
[(1, 3), (1, 4), (2, 3), (2, 1), (2, 4), (3, 1), (3, 4)]
```

- 索引遍历

```python
>>> for index, value in enumerate(['a', 'b', 'c']):
...     pass
```

- 同时遍历

```python
>>> a = ['Pradeepto', 'Kushal']
>>> b = ['OpenSUSE', 'Fedora']
>>> for x, y in zip(a, b):
...     print("{} uses {}".format(x, y))
```



#### 列表做栈

```python
>>> a = [1,2,3,4,5]
>>> a.append(6)  # push
>>> a.pop()      # pop
6
>>> a
[1,2,3,4,5]
```

> 传入一个参数 i 即 `pop(i)` 会将索引为 i 的元素弹出。



#### 列表做队列

```python
>>> a = [1,2,3,4,5]
>>> a.append(6)  # push
>>> a.pop(0)      # pop
1
>>> a
[2,3,4,5,6]
```



#### 元组

> 元组是不可变类型，这意味着你不能在元组内删除或添加或编辑任何值。
>
> ! 要创建只含有一个元素的元组，在值后面跟一个逗号。

- 解包 unpacking

```python
x, y = (15,2)
```



#### 集合

> 集合是一个无序不重复元素的集。
>
> ! 要创建空空集合，你必须使用 set() 而不是 {}。后者用于创建空字典。

基本功能包括关系测试和消除重复元素。基本操作： union（联合），intersection（交），difference（差）和 symmetric difference（对称差集）等数学运算。

```python
>>> a = set('abracadabra')
>>> b = set('alacazam')
>>> a                                  # a 去重后的字母
{'a', 'r', 'b', 'c', 'd'}
>>> a - b                              # a 有而 b 没有的字母
{'r', 'd', 'b'}
>>> a | b                              # 存在于 a 或 b 的字母
{'a', 'c', 'r', 'd', 'b', 'm', 'z', 'l'}
>>> a & b                              # a 和 b 都有的字母
{'a', 'c'}
>>> a ^ b                              # 存在于 a 或 b 但不同时存在的字母
{'r', 'd', 'b', 'm', 'z', 'l'}

>>> a = {'a','e','h','g'}
>>> a.pop()  # pop 方法随机删除一个元素并打印
'h'
>>> a.add('c')
>>> a
{'c', 'e', 'g', 'a'}
```



#### 字典

> 字典是无序的键值对（`key:value`）集合，同一个字典内的键必须是互不相同的。
>
> ! 一对大括号 `{}` 创建一个空字典。
>
> ! 字典中的键必须是不可变类型，比如你不能使用列表作为键。

- 删除键值对

```python
>>> del data['kushal']
```

- 查询键存在

```python
>> 'ShiYanLou' in data
```

- 遍历

```python
for x, y in data.items():
    pass
```

- setdefault方法：无需判断存在性进行添加

```python
>>> data = {}
>>> data.setdefault('names', []).append('Ruby')
>>> data
{'names': ['Ruby']}
>>> data.setdefault('names', []).append('Python')
>>> data
{'names': ['Ruby', 'Python']}
>>> data.setdefault('names', []).append('C')
>>> data
{'names': ['Ruby', 'Python', 'C']}
```

- get方法：试图索引一个不存在的键将会抛出一个 *keyError* 错误。我们可以使用 `dict.get(key, default)` 来索引键，如果键不存在，那么返回指定的 default 值。

```python
>>> data['foo']
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
KeyError: 'foo'
>>> data.get('foo', 0)
0
```



#### 字符串

##### 字符串表示

```python
>>> s = "I am Chinese"
>>> s
'I am Chinese'
>>> s = 'I am Chinese'
>>> s = "Here is a line \
... split in two lines"
>>> s
'Here is a line split in two lines'
>>> s = "Here is a line \n split in two lines"
>>> s
'Here is a line \n split in two lines'
>>> print(s)
Here is a line
 split in two lines
# 分几行输入字符串，并且希望行尾的换行符自动包含到字符串当中，可以使用三对引号："""...""" 或 '''...''' 。
>>> print("""\
... Usage: thingy [OPTIONS]
...      -h                        Display this usage message
...      -H hostname               Hostname to connect to
... """)
Usage: thingy [OPTIONS]
     -h                        Display this usage message
     -H hostname               Hostname to connect to
```

##### 内建方法

- `title` \ `upper`\ `lower` \ `swapcase`(大小写交换)

```python
>>> s = "shi yan lou"
>>> s.title()
'Shi Yan Lou'
>>> z = s.upper()
>>> z
'SHI YAN LOU'
>>> z.lower()
'shi yan lou'
>>> s = "I am A pRoGraMMer"
>> s.swapcase()
'i AM a PrOgRAmmER'
```

- `isalnum`(检查所有字符是否只有字母和数字)/`isalpha` (检查字符串之中是否只有字母)

  > 其他is方法

```python
>>> s = "jdwb 2323bjb"
>>> s.isalnum()
False
>>> s = "jdwb2323bjb"
>>> s.isalnum()
True
>>> s = "1234"
>>> s.isdigit() # 检查字符串是否所有字符为数字
True
>>> s = "ShiYanLou is coming"
>>> s.islower() # 检查字符串是否所有字符为小写
False
>>> s = "Shiyanlou Is Coming"
>>> s.istitle() # To 检查字符串是否为标题样式
True
>>> s = "CHINA"
>>> s.isupper() # 检查字符串是否所有字符为大写
True
```

- split() 分割字符串，默认为 `" "`，返回一个包含所有分割后的字符串的列表

```python
>>> s = "We all love Python"
>>> s.split()
['We', 'all', 'love', 'Python']
>>> x = "shiyanlou:is:waiting"
>>> x.split(':')
['shiyanlou', 'is', 'waiting']
```

- join连接字符串

```python
>>> "-".join("GNU/Linux is great".split())
'GNU/Linux-is-great'
```

- (l\r)strip（左右）剥离

> 剥离字符串首尾中指定的字符，它允许有一个字符串参数为剥离哪些字符。不指定参数则默认剥离掉首尾的空格和换行符。

```python
>>> s = "  a bc\n "
>>> s.strip()
'a bc'
>>> s = "www.foss.in"
>>> s.lstrip("cwsd.") #删除在字符串左边出现的'c','w','s','d','.'字符
'foss.in'
>>> s.rstrip("cnwdi.") #删除在字符串右边出现的'c','n','w','d','i','.'字符
'www.foss'
```

- find查找

```python
>>> s = "faulty for a reason"
>>> s.find("for")
7
>>> s.find("fora")
-1
>>> s.startswith("fa") # 检查字符串是否以 fa 开头
True
>>> s.endswith("reason") # 检查字符串是否以 reason 结尾
True
```

- 回文检查

```python
s == s[::-1]
```

- 格式化输出

```python
print("my name is %s.I am %d years old" % ('Shixiaolou',4))
```

> - `%s` 字符串（用 str() 函数进行字符串转换）
> - `%r` 字符串（用 repr() 函数进行字符串转换，`repr()`函数是Python的内建函数，用于获取对象的"官方"字符串表示。这个表示通常是能够使用`eval()`函数重新创建对象的形式）
> - `%d` 十进制整数
> - `%f` 浮点数
> - `%%` 字符 `%`

- 单词计数

```
len(s.split(" "))
```

