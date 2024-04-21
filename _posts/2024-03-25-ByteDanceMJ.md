---
dlayout:    post
title:      字节跳动2025届算法面筋
subtitle:   interview to ByteDance
date:       2024-03-25
author:     Kylin
header-img: img/bg_NLPseg.jpg
catalog: true
tags:
    - job
---



[TOC]



#### 一面

##### 项目

多模态评论生成的指标，人工准确率是多少

mplug的结构，纯LLM效果怎么样

预训练细节



##### 手撕

输入文件1: 1kw行title，中文，1k

输入文件2 : 10w 个实体词，中文，最大100

输出：统计文件1中，出现频次最高的100个实体词（来自文件2）及其频次

```python
# encoding: utf-8
file1 = []
file2 = []

ma = DefaultDict()

vocab_size = 10000
tokenid = {c:i for i,c in enumerate(vocab)}
class Trie:
    def __init__(self):
        self.child = [None]*vocab_size
        self.isEnd = False

    def insert(self, s):
        root = self
        for c in s:
            index = tokenid[c]
            if not root.child[index]:
                root.child[index] = Trie()  
            root = root.child[index]
        root.isEnd = True

    def find(self, s):
        root = self
        for c in s:
            index = tokenid[c]
            if not root.child[index]: 
                return False
            root = root.child[index]
        return root.isEnd
      
    def count(self,s):
        root = self
      	for i,c in enumerate(s):
            index = tokenid[c]
            if not root.child[index]: 
                return
            root = root.child[index]
            if root.isEnd:
              	ma[s[:i+1]]+=1

tree = tire()
for word in file2:
    tree.insert(word)

for title in file1:
    n = len(title)
    for i in range(n):
        tree.count(title[i:])

# 选取可以用快选
heap = []
for word,fre in ma:
    heapq.heappush(heap,(-fre,word))
res = []
for i in range(100):
  	fre,word = heapq.heappop(heap)
    res.append((word,-fre))
print(res)
```



#### 二面

长文本问题？transformer什么部分是长文本的bottleneck？

pretrain相关技术，rlhf、ppo相关的文章读过吗？

一个开放问题，现在有100条优质创作数据，我们大概有一个70B模型，怎么从模型本身提升模型创作能力？ 

手撕：判断一个字符串能否被给定字典中的字符串划分

```python
s = "catsandog"
wordDict = ["cats", "dog", "sand", "and", "cat"]
class Node:
    def __init__(self) -> None:
        self.children = [None]*26
        self.is_end = False

class Tire:
    def __init__(self) -> None:
        self.root = Node()
    
    def insert(self,word):
        root = self.root
        for c in word:
            if not root.children[ord(c)-ord('a')]:
                root.children[ord(c)-ord('a')]=Node()
            root = root.children[ord(c)-ord('a')]
        root.is_end = True
t = Tire()
for word in wordDict:
    t.insert(word)
n = len(s)
b = [False]*(n+1)
b[-1]=True
for i in range(n-1,-1,-1):
    root = t.root
    for j in range(i,n):
        c = s[j]
        if not root.children[ord(c)-ord('a')]:
            break
        root = root.children[ord(c)-ord('a')]
        if root.is_end:
            b[i] = b[j+1] if b[j+1] else b[i]
print("true" if b[0] else "false")
```

