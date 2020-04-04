---
title: python算法集合
date: 2019-12-10 19:35:51
categories: python
tag:
- python
- 算法
---

# python算法集合

## 数据结构

### 栈

两种操作：压入/插入、弹出/删除并读取

特征：后进先出

！！调用另一个函数时，当前函数暂停并处于未完成状态

### 队列

两种操作：入队、出队

特征：先进先出

### 数组

python中的list

所有数据在内存中紧靠在一起，必须储存在一起，要是空间不够，要重新分配内存

可以直接读取某个元素

性能：读取速度快

### 链表

元素可以储存在任何地方

每个元素都储存了下个元素的地址，只要有足够的内存空间，就能为链表分配内存

必须从第一个元素访问起，从中获取下一个的地址

性能：插入和删除速度快

### 散列表/hash table

python中的dict

内部机制：实现、冲突、散列函数

数组、链表被直接映射到内存；散列表使用散列函数来确定元素的储存位置

作用：1.  防止重复`dict.get()` 2.  缓存/记住数据 3. 仿真映射关系

冲突：两个key被映射到同个位置

性能：读取、插入、删除都很快

## 二分法

```python
def binary_search(list,item):
    low=0
    high=len(list1)-1
    while low<=high:   
        mid=(low+high)//2
        guess=list1[mid]
        if guess==item:
            print("congratulations!the item=",mid)
            break
        elif guess>item:
            print(guess)
            high=mid-1
        else:
            low=mid+1
            print(guess)
            
list1=list(range(100))
binary_search(list1,47)
```



## 排序算法

min

```python
def findsmall(arr):
    smallest=arr[0]#储存最小的值
    smallest_index=0#储存最小的值的索引
    for i in range(1,len(arr)):
        if arr[i]<smallest:
            smallest=arr[i]
            smallest_index=i
    return smallest_index#返回最小值索引
#测试
#x=[43,5,57,31,4,35,45,45,6,3,42,1]    
#print(findsmall(x))
```

排序

```python
def selection(arr):
    new_arr=[]
    for i in range(len(arr)):
        smallest_index=findsmall(arr)
        new_arr.append(arr[smallest_index])
        arr.pop(smallest_index)
    return new_arr
#x=[5,3,6,2,10]
#print(selection(x))
```

## 递归算法

### 简介

1. base case(基线条件)：函数不再调用自己
2. recursive case(递归条件)：函数调用自己

```python
#x!
def fact(x):
    if x==1:
        return 1;
    else:
        return x*fact(x-1)
#print(fact(3))
```

### D&C

即divide and conquer,分而治之，一种思想方法

工作原理：

1. 找出简单的基线条件
2. 确定如何缩小问题规模，使其符合基线条件

#### eg1:sum(list)

```python
#遍历
def sum1(arr):
    total=0
    for i in arr:
        total+=i
    return total
#print(sum([1,2,3,4]))        
```

思考：

1. 最简单的数组？
   - 空列表：sum=0
   - 一个元素a：sum=a
2. 每次递归调用，都必须离空数组更进一步。

```python
#递归
def sum2(list):
    if list==[]:
        return 0
    else:
        return list[0]+sum2(list[1:])
#print(sum2([1,2,3,4]))
```

#### sg2:quicksort

```python
def quicksort(array):
    if len(array)<2:
        return array#基线条件：为空或者只包含一个元素的数组是有序的
    else:
        pivot=array[0]#基准值
        less=[i for i in array[1:] if i<=pivot]#由小于基准值的数构成的子数列
        more=[i for i in array[1:] if i>pivot]#由大于基准值的数构成的子数列
        return quicksort(less)+[pivot]+quicksort(more)
#print(quicksort([2,5,7,123,87,4]))
```

## 广度优先搜索

适用于非加权图

找出两样东西之间的最短距离

图：1.仿真一组连线，由节点和边组成  2.与节点直接相邻的节点被称为邻居

```python
#找出seller
#建立关系图
graph={}
graph["you"]=["alice","bob","claire"]
graph["bob"]=["anuj","peggy"]
graph["alice"]=["peggy"]
graph["claire"]=["thom","jonny"]
graph["anuj"]=[]
graph["peggy"]=[]
graph["thom"]=[]
graph["jonny"]=[]
#建立双端队列
from collections import deque
#检验是否为seller
def person_is_seller(name):
    return name[-1]=="m"
#搜索
def search(name):
    search_queue=deque()#建立队列
    search_queue+=graph[name]# 把name的邻居加入搜索队列
    searched=[]#建立空表格，以存储一搜索过的元素
    while search_queue:#当搜索列表不为空时
        person=search_queue.popleft()#出一个人
        if person not in searched:#排除已检查过的，免得无限循环
            if person_is_seller(person):
                print("i find",person) 
            else:
                search_queue+=graph[person]#把那人的邻居加入队列
                searched.append(person)#加入已搜索行列
#search("you")
    
```

## 狄克斯特拉算法

适用于加权的有向无环图，不能有负边权

非加权图：不带权重的图；加权图：带权重的图；环：带圈的；边上的数字：权重
算法：

1. 找出“最便宜”的节点
2. 更新该节点的邻居开销
3. 重复1，2直到对每个节点都这样做了
4. 计算最终路径

```python
# 用散列表实现图的关系
# 创建节点的开销表，开销是指从"起点"到该节点的权重
#graph:图
graph = {}
graph["start"] = {}
graph["start"]["a"] = 6
graph["start"]["b"] = 2

graph["a"] = {}
graph["a"]["end"] = 1

graph["b"] = {}
graph["b"]["a"] = 3
graph["b"]["end"] = 5

graph["end"] = {}

#cost；从父节点到该节点的开销（邻居可以直接得知，对于还不知道的用无穷表示）（不断更新）
infinity = float("inf")# 无穷大
costs = {}
costs["a"] = 6
costs["b"] = 2
costs["end"] = infinity

# parents：父节点散列表
parents = {}
parents["a"] = "start"
parents["b"] = "start"
parents["end"] = None

# 已经处理过的节点，需要记录，因为对于同一个节点，不用多次处理
processed = []

# 找到开销最小的节点
def find_lowest_cost_node(costs):
    # 初始化数据
    lowest_cost = infinity
    lowest_cost_node = None
    # 遍历所有节点,且该节点没有被处理
    for node in costs :#循环key
        # 如果当前节点的开销比已经存在的开销小，,且该节点没有被处理则更新该节点为开销最小的节点
        if costs[node] < lowest_cost and node not in processed:
            lowest_cost = costs[node]
            lowest_cost_node = node
    return lowest_cost_node


# 找到最短路径
def find_shortest_path():
    node = "end"
    shortest_path = ["end"]
    while parents[node] != "start":
        shortest_path.append(parents[node])
        node = parents[node]
    shortest_path.append("start")
    return shortest_path


# 寻找加权的最短路径
def dijkstra():
    # 查询到目前开销最小的节点(note为目前最小开销节点)
    node = find_lowest_cost_node(costs)
    # 只要有开销最小的节点就循环（这个while循环在所有节点都被处理过后结束）
    while node is not None:
        # 获取该节点当前开销
        cost = costs[node]
        # 获取该节点相邻的节点
        neighbors = graph[node]
        # 遍历当前节点的所有邻居
        for n in neighbors.keys():
            # 计算经过当前节点到达相邻结点的开销,即当前节点的开销加上当前节点到相邻节点的开销
            new_cost = cost + neighbors[n]
            # 如果经当前节点前往该邻居更近，就更新该邻居的开销
            if new_cost < costs[n]:
                costs[n] = new_cost
                #同时将该邻居的父节点设置为当前节点
                parents[n] = node
        # 将当前节点标记为处理过
        processed.append(node)
        # 找出接下来要处理的节点，并循环
        node = find_lowest_cost_node(costs)
    # 循环完毕说明所有节点都已经处理完毕
    shortest_path = find_shortest_path()
    shortest_path.reverse()
    print(shortest_path)
# 测试
dijkstra()
```

## 近似算法

用于解决NP完全问题

每步都选择局部最优解，最终得到的就是全局最优解（并不是任何情况下成立）

```python
# 首先，创建一个列表，其中包含要覆盖的州
states_needed = set(["mt", "wa", "or", "id", "nv", "ut", "ca", "az"])
# 还需要有可供选择的广播台清单
stations = {}
stations["kone"] = set(["id", "nv", "ut"])
stations["ktwo"] = set(["wa", "id", "mt"])
stations["kthree"] = set(["or", "nv", "ca"])
stations["kfour"] = set(["nv", "ut"])
stations["kfive"] = set(["ca", "az"])
# 最后，需要使用一个集合来存储最终选择的广播台
final_stations = set()
# 需要遍历所有广播台，从中选择覆盖了最多的未覆盖州的广播台。
while states_needed:
    best_station = None # 覆盖了最多的未覆盖州的广播台
    states_covered = set() # 包含该广播台覆盖的所有未覆盖的州
    for station, states_for_station in stations.items():
        covered = states_needed & states_for_station  # 计算交集
        if len(covered) > len(states_covered):
            best_station = station
            states_covered = covered
    states_needed -= states_covered
    final_stations.add(best_station)
print(final_stations)

```

## 动态规划

网格，单元格中的值通常是要优化的值

要确定:

1. 单元格的值是什么
2. 如何将这个问题划分为子问题
3. 网格的坐标轴是什么

## k最邻近算法

k-nearest-neighbours,KNN

基本工作

1. 分类（边组）
2. 回归（预测数值）
3. 特征抽取（将物品转换成一系列可比较的数字）

应用：机器学习（OCR）