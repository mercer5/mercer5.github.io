---
title: python-一行代码
date: 2020-02-13 20:39:45
categories: python
tags:
- python
---
# 一行代码python

## 1 - 100 的和

```python
sum(range(1,101))
```

## 数值交换

```python
a,b=b,a
```

## 奇偶数

```python
[x for x in range(10) if x%2==0]
#[0, 2, 4, 6, 8]
[x for x in range(10) if x%2==1]
#[1, 3, 5, 7, 9]
```

## 展开列表

```python
lst=[[1,2,3],[4,5,6],[7,8,9]]
[x for y in lst for x in y]
#[1, 2, 3, 4, 5, 6, 7, 8, 9]
```

## 打乱列表

```python
import random
lst=list(range(10))
random.shuffle(lst)
lst
```

## 反转字符串

```python
s="asdfghjkl"
s[::-1]
#'lkjhgfdsa'
```

## **查看目录下所有文件**

```python
import os
os.listdir()
```

## **去除字符串间的空格**

```python
s="my name is mercer"
s.replace(" ","")
```

```python
s="my name is mercer"
"".join(s.split())

```

## **字符串整数列表变成整数列表**

```python
a=["1","2","3"]
list(map(lambda x: int(x),a))
#[1, 2, 3]

```

## **删除列表中重复的值**

```python
lst=[1,1,2,2,3,3,4,5,6]
list(set(lst))
#[1, 2, 3, 4, 5, 6]

```

## **9 \* 9 乘法表**

```python
print('\n'.join(['\t'.join(["%2s*%2s=%2s"%(j,i,i*j) for j in range(1,i+1)]) for i in range(1,10)]))

```

## 两个列表中相同的元素

```python
a=[1,2,2,3,4]
b=[3,4,4,5,6]
set(a)&set(b)
#{3, 4}

```

## **两个列表中不同的元素**

```python
a=[1,2,2,3,4]
b=[3,4,4,5,6]
set(a)^set(b)
#{1, 2, 5, 6}

```

## **合并两个字典**

```python
a={"name":"mercer"}
b={"age":"100"}
a.update(b)
a
#{'name': 'mercer', 'age': '100'}

```

## **字典键从小到大排序**

```python
a={"name":"mercer","age":100,"like":"python"}
sorted(a.items(),key=lambda x:x[0])
#[('age', 100), ('like', 'python'), ('name', 'mercer')]

```

