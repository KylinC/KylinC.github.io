---
dlayout:    post
title:      美团2025届算法策略笔试
subtitle:   coding to Meituan
date:       2024-03-09
author:     Kylin
header-img: img/bg_NLPseg.jpg
catalog: true
tags:
    - job
---



[TOC]

### 3.9 算法策略

#### Q1

MT 是美团的缩写，因此小美很喜欢这两个字母。现在小美拿到了一个仅由大写字母组成字符串，她可以最多操作k次，每次可以修改任意一个字符。小美想知道，操作结束后最多共有多少个'M' 和'T' 字符?
**输入**
输入两个正整数n和k，代表字符串长度和操作次数第二行输入一个长度为n的、仅由大写字母组成的字符串。

**约束条件**

1≤k≤n≤10^5

**输出描述**

输出操作结束后最多共有多少个'M' 和'T'字符

**示例 1**

**输入**

```
5 2
MTUAN
```

**输出**

```
4
```

**说明**

修改第三个和第五个字符，形成的字符串为 MTTAM，这样共有4个'M'和'T'



##### A1

```python
import sys

n,k = map(int,sys.stdin.readline().split())
s = str(sys.stdin.readline())

count = 0
for c in s:
    if c=='M' or c=='T':
        count += 1

print(min(count+k,n))
```



#### Q2

**题目描述**
小美拿到了一个由正整数组成的数组，但其中有一些元素是未知的(用0来表示)。现在小美想知道，如果那些未知的元素在区间[l,r]范围内随机取值的话，数组所有元素之和的最小值和最大值分别是多少?共有q次询问。

**输入描述**
第一行输入两个正整数n和 q，代表数组大小和询问次数。
第二行输入 n个整数 ai，其中如果输入的ai为0，那么说明ai是未知的。
接下来的 q行，每行输入两个正整数 l 和r，代表一次询问。

**约束条件**
1≤n,q≤10^5
0≤a_i≤10^9
1≤l≤r≤10^9

**输出描述**
输出 q行，每行输出两个正整数，代表所有元素之和的最小值和最大值。

**示例 1**

**输入**

```
3 2
1 0 3
1 2
4 4
```

**输出**

```
5 6
8 8
```

**说明**

只有第二个元素是未知的。第一次询问，数组最小的和是1+1=3=5，最大的和是1+2+3=6

第二次询问，显然数组的元素和必然为 8



##### A2

```python
import sys

n,q = map(int,sys.stdin.readline().split())
array = list(map(int,sys.stdin.readline().split()))
count = 0
for a in array:
    if not a:
        count += 1
s = sum(array)
for i in range(q):
    l,r = map(int,sys.stdin.readline().split())
    print(s+count*l,s+count*r)
```



#### Q3

**题目描述**
小美拿到了一个 nxn 的矩阵，其中每个元素是0或者1。小美认为一个矩形区域是完美的，当且仅当该区域内0的数量恰好等于1的数量。现在，小美希望你回答有多少个ixi的完美矩形区域。你需要回答1≤i≤n的所有答案。
**输入描述**
第一行输入一个正整数 n，代表矩阵大小。
接下来的 n行，每行输入一个长度为n的 01串，用来表示矩阵
**约束条件**
1≤n≤200
**输出描述**
输出n行，第i行输出 ixi的完美矩形区域的数量。
**示例 1**
**输入**

```
4
1010
0101
1100
0011
```

**输出**

```
0
7
0
1
```



##### A3

```python
import sys

n = int(sys.stdin.readline())

dp = [[0]*(n+1) for _ in range(n+1)]

for i in range(1,n+1):
    s = list(map(int,list(sys.stdin.readline().strip())))
    su = 0
    for j in range(1,n+1):
        su += s[j-1]
        dp[i][j] = dp[i-1][j]+su

for i in range(1,n+1):
    res = 0
    if i%2:
        print(0)
    else:
        for j in range(1,n+1):
            for k in range(1,n+1):
                num = dp[j][k]-dp[j][k-i]-dp[j-i][k]+dp[j-i][k-i]
                if num == i*i//2:
                    res += 1
        print(res)
```



#### Q4

**题目描述**
小美拿到了一个大小为n的数组，她希望删除一个区间后，使得剩余所有元素的乘积未尾至少有k个0。小美想知道，一共有多少种不同的删除方案?

**输入描述**
第一行输入两个正整数n和 k。
第二行输入n个正整数 ai，代表小美拿到的数组。

**约束条件**
1≤n,k≤10^5
1≤ai≤10^9

**输出描述**
一个整数，代表删除的方案数。

**示例 1**
**输入**

```
5 2
2 5 3 4 20
```

**输出**

```
4
```

**说明**

第一个方案，删除 [3]。
第二个方案，删除 [4]。
第三个方案，删除 [3,4]。
第四个方案，删除[2]。



##### A4

```python
import sys

n,k = map(int,sys.stdin.readline().split())
array = list(map(int,sys.stdin.readline().split()))

num_2,num_5 = [0]*(n+1),[0]*(n+1)
for i,a in enumerate(array):
    num_2[i+1],num_5[i+1]=num_2[i],num_5[i]
    while a%2==0:
        num_2[i+1]+=1
        a//=2
    while a%5==0:
        num_5[i+1]+=1
        a//=5

left,res = 0,0
for right in range(n):
    while left<right and (num_2[n]-(num_2[right]-num_2[left])<k or num_5[n]-(num_5[right]-num_5[left])<k):
        left+=1
    res+=right-left
print(res)
```



#### Q5

**题目描述**

小美认为，在人际交往中，随着时间的流逝，朋友的关系也会慢慢变淡，最终朋友关系就会淡忘。现在初始有一些朋友关系，存在一些事件会导致两个人淡忘了他们的朋友关系。小美想知道某一时刻中，某两人是否可以通过朋友介绍互相认识。
事件共有 2 种:
1.1uv:代表编号u的人和编号v的人淡忘了他们的朋友关系
2.2uv:代表小美查询编号u的人和编号v的人是否能通过朋友介绍互相认识。
注:介绍可以有多层，比如2号把1号介绍给3号，然后3号再把1号介绍给4号，这样1号和4号就认识了。

**输入描述**
第一行输入三个正整数 n,m,q，代表总人数，初始的朋友关系数量，发生的事件数量。
接下来的 m 行，每行输入两个正整数 u,v，代表初始编号u的人和编号v的人是朋友关系。
接下来的 q行，每行输入三个正整数 op,u,v，含义如题目描述所述。

**约束条件**
1≤n≤10^9
1≤m,q≤10^5
1≤u,v≤n
1≤op≤2
保证至少存在一次查询操作。

**输出描述**
对于每次2号操作，输出一行字符串代表查询的答案。如果编号u的人和编号心的人器涵过朋友介绍互治认识,则输出"Yes"。否则输出"No"。

**示例 1**
**输入**

```
5 3 5
1 2
2 3
4 5
1 1 5
2 1 3
2 1 4
1 1 2
2 1 3
```

**输出**

```
Yes
NO
NO
```

**说明**
第一次事件，1号和5号本来就不是朋友，所以无事发生第二次事件是询问，1号和3号可以通过2号的介绍认识。第三次事件是询问，显然1号和4号无法互相认识。第四次事件，1号和2号淡忘了。
第五次事件，此时1号无法再经过 2号和3号互相认识了。



##### A5

```python
import sys

n,m,q = map(int,sys.stdin.readline().split())

s = set()
for i in range(m):
    u,v = map(int,sys.stdin.readline().split())
    s.add((u,v))

s_construct = s.copy()
query = []
for i in range(q):
    op,u,v = map(int,sys.stdin.readline().split())
    query.append((op,u,v))
    if op == 1:
        if (u,v) in s_construct:
            s_construct.remove((u,v))
        if (v,u) in s_construct:
            s_construct.remove((v,u))
            
fa = [-1]*(n+1)
csize = [1]*(n+1)

def find(x):
    if fa[x] == -1:
        return x
    fa[x] = find(fa[x])
    return fa[x]

def union(x,y):
    x = find(x)
    y = find(y)
    if x == y:
        return
    if csize[x] < csize[y]:
        x,y = y,x
    fa[y] = x
    csize[x] += csize[y]
    
for u,v in s_construct:
    union(u,v)

res = []
for op,u,v in query[::-1]:
    if op == 1:
        if (u,v) in s or (v,u) in s:
            union(u,v)
    else:
        res.append('YES' if find(u) == find(v) else 'NO')
print('\n'.join(res[::-1]))
```













