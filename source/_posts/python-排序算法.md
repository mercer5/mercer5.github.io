---
title: python排序算法集合
date: 2019-12-20 19:35:51
categories: python
tag:
- python
- 算法

---


# python 排序算法

## 0.相关知识

**内部排序**

若整个排序过程不需要访问外存便能完成，则称此类排序问题为内部排序。

**外部排序**

若参加排序的记录数量很大，整个序列的排序过程不可能在内存中完成，则称此类排序问题为外部排序。

**就地排序**

若排序算法所需的辅助空间并不依赖于问题的规模n，即辅助空间为O（1），称为就地排序。

**稳定排序**

假定在待排序的记录序列中，存在多个具有相同的关键字的记录，若经过排序后，这些记录的相对次序保持不变，即在原序列中 ri=rj， ri 在 rj 之前，而在排序后的序列中，ri 仍在 rj 之前，则称这种排序算法是稳定的；否则称为不稳定的。

**排序序列分布**

排序需要考虑待排序关键字的分布情况，这会影响对排序算法的选择，通常我们在分析下列算法时都考虑关键字分布是随机分布的，不是按照某种规律分布的，比如正态分布等。

**待排序序列**

排序序列中，剩余即将要排序的序列部分。

**已排序序列**

排序序列中，已经排序好的序列部分。

**哨兵**

一切为简化边界条件而引入的附加结点(元素)均可称为哨兵

## 1.冒泡排序

**步骤**

1. 比较相邻的元素。如果第一个比第二个大，就交换他们两个。
2. 对每一对相邻元素作同样的工作，从开始第一对到结尾的最后一对。这步做完后，最后的元素会是最大的数。
3. 针对所有的元素重复以上的步骤，除了最后一个。
4. 持续每次对越来越少的元素重复上面的步骤，直到没有任何一对数字需要比较。

**比较次数**

n * (n-1) / 2 

**算法评价**

优点：如果你不是故意去交换相等的关键码的话，这个算法是绝对稳定的排序算法。

缺点：比较次数也就是所谓的时间复杂度 为O(n^2^)，最好的情况和最坏的情况都是O(n^2^)。

**代码**

```python
def bubbleSort(arr):
    for i in range(1,len(arr)):
        for j in range(len(arr)-i):
            if arr[j]<arr[j+1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
    return arr
```

**优化代码1**

```python
def bubble_sort1(arr):
    """
    如果没有元素交换，说明数据在排序过程中已经有序，直接退出循环
    """
    for i in range(1,len(arr)):
        swapped = False
        for j in range(len(arr)-i):
            if arr[j]<arr[j+1]:
                swapped = True
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
        if not swapped: 
            break  # Stop iteration if the collection is sorted.
    return collection
```

**优化代码2**

```python
def bubble_sort2(arr):
    """
    bubble_sort1的基础上再优化。
    优化思路：在排序的过程中，数据可以从中间分为两段，一段是无序状态，另一段是有序状态。
    每一次循环的过程中，记录最后一个交换元素的index，它便是有序和无序状态的边界
    下一次仅循环到边界即可，从而减少循环次数，达到优化。
    """
    compare_count=0
    last_change_index = 0 #最后一个交换的位置
    border = len(arr)-1 #有序和无序的分界线
    for i in range(1,len(arr)):
        swapped = False
        for j in range(border):
            if arr[j]<arr[j+1]:
                swapped = True
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
                last_change_index = j
        if not swapped: 
            break  # Stop iteration if the collection is sorted.
        border = last_change_index # 最后一个交换的位置就是边界
    return collection
```

## 2.快速排序

**简介**

快速排序是由东尼·霍尔所发展的一种排序算法。在平均状况下，排序 n 个项目要 Ο(nlogn) 次比较。在最坏状况下则需要 Ο(n2) 次比较，但这种状况并不常见。事实上，快速排序通常明显比其他 Ο(nlogn) 算法更快，因为它的内部循环（inner loop）可以在大部分的架构上很有效率地被实现出来。

快速排序使用分治法（Divide and conquer）策略来把一个串行（list）分为两个子串行（sub-lists）。

快速排序又是一种分而治之思想在排序算法上的典型应用。本质上来看，快速排序应该算是在冒泡排序基础上的递归分治法。

**步骤**

1. 从数列中挑出一个元素，称为 “基准”（pivot）;
2. 重新排序数列，所有元素比基准值小的摆放在基准前面，所有元素比基准值大的摆在基准的后面（相同的数可以到任一边）。在这个分区退出之后，该基准就处于数列的中间位置。这个称为分区（partition）操作；
3. 递归地（recursive）把小于基准值元素的子数列和大于基准值元素的子数列排序；

递归的最底部情形，是数列的大小是零或一，也就是永远都已经被排序好了。虽然一直递归下去，但是这个算法总会退出，因为在每次的迭代（iteration）中，它至少会把一个元素摆到它最后的位置去。

**代码**

```pyhton
def quicksort(array):
    if len(array)<2:
        return array#基线条件：为空或者只包含一个元素的数组是有序的
    else:
        pivot=array[0]#基准值
        less=[i for i in array[1:] if i<=pivot]#由小于基准值的数构成的子数列
        more=[i for i in array[1:] if i>pivot]#由大于基准值的数构成的子数列
        return quicksort(less)+[pivot]+quicksort(more)
```

**简单评价**

1. 最坏情况

快速排序的最坏情况，实际上就退化为了冒泡排序的情况，想想冒泡排序，每一轮比较后，都将原来的排序好的区间增加了一个长度，也就是说快速排序每次选择的pivot也正好达成了冒泡排序的作用，那么最坏情况就此发生。简单来说，最坏情况发生在每次划分过程产生的两个区间分别包含n-1个元素和1个元素的时候。那么不难知道，最坏情况的复杂度也为 O(n^2)。

2. 最好情况

如果每次划分过程产生的区间大小都为n/2，则快速排序法运行就快得多了，此时的时间复杂度为 **O（nlogn）**，logn表示以2为底，n的对数。因为每轮比较都会平均分成2个区间，共经过趋向于n轮的比较。

3. 平均情况

平均情况和最好情况的时间复杂度都为**O（nlogn）**，只不过平均情况的常数因子可能大一些，有关详细分析，请查阅相关资料。

## 3.选择排序

**步骤**

1. 首先在未排序序列中找到最小（大）元素，存放到排序序列的起始位置
2. 再从剩余未排序元素中继续寻找最小（大）元素，然后放到已排序序列的末尾。
3. 重复第二步，直到所有元素均排序完毕。

**代码**

```python
def selectionSort(arr):
    for i in range(len(arr) - 1):
        # 记录最小数的索引
        minIndex = i
        for j in range(i + 1, len(arr)):
            if arr[j] < arr[minIndex]:
                minIndex = j
        # i 不是最小数时，将 i 和最小数进行交换
        if i != minIndex:
            arr[i], arr[minIndex] = arr[minIndex], arr[i]
    return arr
```

**算法评价**

在直接选择排序中，共需要进行 n-1 轮，每轮必发生一次交换，每轮需要进行 n-i 次比较 (1<=i<=n-1)，总的比较次数等于 

(n-1) + (n-2) + ... + ( n-(n-1) ) 

化简后等于 n + (n-1)(n-2)/2

由此可知，直接选择排序的时间复杂度为 O(n^2) ，空间复杂度为 O(1) 。注意到，直接选择排序在最好和最坏情况下都是 O(n^2) 。

一般地，排序算法的时间复杂度为 O(n^2)是不令人满意的排序算法，在选择排序算法的思想下，有一种选择排序算法提升了时间性能，它就是==堆排序==

## 4.堆排序

**简介**

堆排序，英文名称 Heapsort，利用二叉树（堆）这种数据结构所设计的一种排序算法，是一种对直接选择排序的一种改建算法。在逻辑结构上是按照二叉树存储结构，正是这种结构优化了选择排序的性能，在物理存储上是连续的数组存储，它利用了数组的特点快速定位指定索引的元素。

**堆排序的基本概念**

n个关键字序列 Kl，K2，…，Kn 称为堆（Heap），当且仅当该序列满足如下性质：

- Ki <= K( 2i + 1 ）且 Ki <= K( 2i + 2 ) ( 0≤i≤ (n/2)-1），称为小根堆。每个节点的值都小于或等于其子节点的值，在堆排序算法中用于降序排列

- Ki >= K( 2i + 1） 且 Ki >= K( 2i +2 ) ( 1≤i≤ (n/2)-1）， 称为大根堆。每个节点的值都大于或等于其子节点的值，在堆排序算法中用于升序排列

**堆排序是如何工作的**

以大根堆排序为例，即要得到非降序序列。

1. 先将初始文件R[0..n-1]建成一个大根堆，此堆为初始的无序区。
2. 再将关键字最大的记录R[0]（即堆顶）和无序区的最后一个记录R[n-1]交换，由此得到新的无序区 R[0..n-2] 和有序区 R[n-1]，且满足 R[0..n-2] ≤ R[n-1]
3. 由于交换后新的根R[0]可能违反堆性质，故应将当前无序区R[0..n-2]调整为堆。然后再次将R[0..n-2]中关键字最大的记录R[0]和该区间的最后一个记录R[n-2]交换，由此得到新的无序区R[0..n-3] 和 有序区R[n-2..n-1]，且仍满足关系R[0..n-3] ≤ R[n-2..n-1]。
4. 重复步骤2和步骤3，直到无序区只有一个元素为止。

**步骤**

1. 创建一个堆 H[0……n-1]；
2. 把堆首（最大值）和堆尾互换；
3. 把堆的尺寸缩小 1，并调用 shift_down(0)，目的是把新的数组顶端数据调整到相应位置；
4. 重复步骤 2，直到堆的尺寸为 1。

**例子:如何构建大根堆**

3,2,5,9,2

1. 待排序列的物理存储结构和逻辑存储结构的示意图如下所示，

<img src="http://ww1.sinaimg.cn/large/007UR4Zcly1ga3a8xflnkj30do08hq30.jpg" alt="1.webp" style="zoom:67%;" />

2. 构建初始堆是从length/2 - 1，即从索引1处关键码等于2开始构建，2的左右孩子等于9, 2，它们三个比较后，父节点2与左孩子9交换，如下图所示：

   <img src="http://ww1.sinaimg.cn/large/007UR4Zcly1ga3a8xf5h1j307h07ft8k.jpg" alt="2.webp" style="zoom:67%;" />

3. 接下来从索引1减1等于0处，即元素3开始与其左右孩子比较，比较后父节点3与左孩子节点9交换，如下所示：

   <img src="http://ww1.sinaimg.cn/large/007UR4Zcly1ga3a8xfg03j306e07ct8k.jpg" alt="3.webp" style="zoom:67%;" />

4. 因为索引等于 0 了，所以构建堆结束，得到大根堆，第一步工作结束，下面开始第二步调整堆，也就是不断地交换堆顶节点和未排序区的最后一个元素，然后再构建大根堆，下面开始这步操作，交换栈顶元素9（如上图所示）和未排序区的最后一个元素2，如下图所示，现在排序区9成为了第一个归位的，

   <img src="http://ww1.sinaimg.cn/large/007UR4Zcly1ga3a8xfixnj307k086dfq.jpg" alt="4.webp" style="zoom:67%;" />

5. 接下来拿掉元素9，未排序区变成了2,3,5,2，然后从堆顶2开始进行堆的再构建，比较父节点2与左右子节点3和5，父节点2和右孩子5交换位置，如下图所示，这样就再次得到了大根堆，

   <img src="http://ww1.sinaimg.cn/large/007UR4Zcly1ga3a8xfj31j307w088dfp.jpg" alt="5.webp" style="zoom:67%;" />

6. 再交换堆顶5和未排序区的最后一个元素2，这样5又就位了，这样未排序区变为了2,3,2，已排序区为 5,9，交换后的位置又破坏了大根堆，已经不再是大根堆了，如下图所示，

   <img src="http://ww1.sinaimg.cn/large/007UR4Zcly1ga3a8xfjxjj308608pmx1.jpg" alt="6.webp" style="zoom:67%;" />

7. 所以需要再次调整，然后堆顶2和左孩子3交换，交换后的位置如下图所示，这样二叉树又重新变为了大根堆，再把堆顶3和此时最后一个元素也就是右孩子2交换，

   <img src="http://ww1.sinaimg.cn/large/007UR4Zcly1ga3a8xh9omj3086074mx0.jpg" alt="7.webp" style="zoom:67%;" />

8. 接下来再构建堆，不再赘述，见下图。

<img src="http://ww1.sinaimg.cn/large/007UR4Zcly1ga3a8xhq52j30dp0d4q2w.jpg" alt="8.webp" style="zoom: 67%;" />

**代码**

```python
def buildMaxHeap(arr):
    import math
    for i in range(math.floor(len(arr)/2),-1,-1):
        heapify(arr,i)

def heapify(arr, i):
    left = 2*i+1
    right = 2*i+2
    largest = i
    if left < arrLen and arr[left] > arr[largest]:
        largest = left
    if right < arrLen and arr[right] > arr[largest]:
        largest = right

    if largest != i:
        swap(arr, i, largest)
        heapify(arr, largest)

def swap(arr, i, j):
    arr[i], arr[j] = arr[j], arr[i]

def heapSort(arr):
    global arrLen
    arrLen = len(arr)
    buildMaxHeap(arr)
    for i in range(len(arr)-1,0,-1):
        swap(arr,0,i)
        arrLen -=1
        heapify(arr, 0)
    return arr
```

**算法评价**

堆排序的时间，主要由建立初始堆和反复重建堆这两部分的时间开销构成，堆排序的平均时间复杂度是O(nlogn) 。堆排序是就地排序，空间复杂度为O(1）。

通过上面的例子，可以看到两个关键码2的相对位置会发生变化，所以堆排序是不稳定的排序方法。

同样是选择排序的算法，直接选择和堆选择时间差别还是不小，但是堆排序算法不大适宜数据量较少的情况，因为光构建初始堆就要进行很多次比较。

## 3.插入排序

**简介**

直接插入排序，英文名称 straight insertion sort，它是一种依次将无序区的元素在有序区内找到合适位置依次插入的算法。

想象你在打扑克的情景:-)

**步骤**

1. 将第一待排序序列第一个元素看做一个有序序列，把第二个元素到最后一个元素当成是未排序序列。
2. 从无序表中取出第一个元素
3. 在当前有序区R[0..i-1]中查找R[i]的正确插入位置 **k**(0≤k≤i-1)；（如果待插入的元素与有序序列中的某个元素相等，则将待插入元素插入到相等元素的后面。）
4. 将R[k．．i-1]中的记录均后移一个位置，腾出 **k** 位置上的空间插入R[i]。

**代码**

```python
def insertionSort(arr):
    for i in range(len(arr)):
        preIndex = i-1
        current = arr[i]
        while preIndex >= 0 and arr[preIndex] > current:
            arr[preIndex+1] = arr[preIndex]
            preIndex-=1
        arr[preIndex+1] = current
    return arr
```

**优化代码**

当有序区间数据量很大时，查找数据的插入位置就会显得非常耗时，插入排序算法每次都是从有序区间查找插入位置，以此为切入点，我们可以使用二分查找法来快速确认待插入的位置，于是就有了优化版的插入排序算法，也叫**二分查找插入算法**或**拆半插入**。

```python
def binaryInsert(arr):
# 折半插入排序: 小->大
# 在直接插入排序的基础上使用了折半查找的方法 
    for i in range(1, len(arr)):
        index = arr[i]
        low = 0
        hight = i - 1
        while low <= hight:
            mid = (low + hight) / 2
            if index > arr[mid]:
                low = mid + 1
            else:
                hight = mid - 1
        # 跳出循环后 low, mid 都是一样的, hight = low - 1
        for j in range(i, low, -1):
            arr[j] = arr[j - 1]
        arr[low] = index
    return arr
```

**算法评价**

如果目标是把n个元素的序列升序排列，那么采用插入排序存在最好情况和最坏情况。

- 最好情况就是，序列已经是升序排列了，在这种情况下，需要进行的比较操作需（n-1）次即可。

- 最坏情况就是，序列是降序排列，那么此时需要进行的比较共有n(n-1)/2次。

插入排序从上个演示中可以看到直接插入排序是稳定的排序算法，每次找到的插入点位置定下一个规则，要么统一放在相等关键码的前面或后面。

插入排序算法平均来说时间复杂度为O(n^2），比较次数越多，插入点后的数据移动越多,特别是当数据总量庞大的时候，但是可以用链表解决数据移动的问题。

因而，插入排序不适合对于数据量比较大的排序应用。直接插入排序在n不大时，插入排序的效果会很好，但是，如果需要排序的数据量很大直接插入排序的性能大幅下降，那么有没有优化的方法呢？由此诞生了插入思想下的希尔排序。

## 4.希尔排序

 **简介**

希尔排序(shell sort)，也称**缩小增量排序**，是插入排序的一种更高效的改进版本。但希尔排序是非稳定排序算法。

希尔排序是基于插入排序的以下两点性质而提出改进方法的：

- 插入排序在对几乎已经排好序的数据操作时，效率高，即可以达到线性排序的效率；
- 但插入排序一般来说是低效的，因为插入排序每次只能将数据移动一位；

希尔排序的基本思想是：先将整个待排序的记录序列分割成为若干子序列分别进行直接插入排序，待整个序列中的记录“基本有序”时，再对全体记录进行依次直接插入排序。

**希尔排序的关键概念---增量序列**

是指在待排序序列中提取关键码所用的序号间隔.

比如初始序列包含5个元素 [3 2 5 9 2]， 如果增量序列为2，那么在一轮排序中，分为两组： [3 5 2] ，[2 9] ，在这两组中分别做直接插入排序。

<img src="http://ww1.sinaimg.cn/large/007UR4Zcly1ga3a8xldiej30b90f3jrg.jpg" alt="shell1.webp" style="zoom:67%;" />

**步骤**

1. 先取一个正整数 `d1<n`，把所有序号相隔d1的数组元素放一组，组内进行直接插入排序;

- 一般的初次取序列的一半为增量，以后每次减半，直到增量为1

2. 然后取 d2< d1，重复上述分组和直接插入排序操作；

3. 直至di = 1，即所有记录放进一个组中排序为止。

**代码**

```python
def shellSort(arr):
    import math
    gap=1
    while(gap < len(arr)/3):
        gap = gap*3+1
    while gap > 0:
        for i in range(gap,len(arr)):
            temp = arr[i]
            j = i-gap
            while j >=0 and arr[j] > temp:
                arr[j+gap]=arr[j]
                j-=gap
            arr[j+gap] = temp
        gap = math.floor(gap/3)
    return arr
```

**算法评价**

希尔排序的时间复杂度为 `O(logn * logn * n)`， 没有快速排序算法`O(n*logn)`快 ，因此中等大小规模表现良好，对规模非常大的数据排序不是最优选择。

但是比直接插入排序 O(n^2）复杂度算法快得多，并且希尔排序非常容易实现，算法代码短而简单。 

此外，希尔算法在最坏的情况下和平均情况下执行效率相差不是很多，与此同时快速排序在最坏 的情况下执行的效率会非常差。

专家们提倡，几乎任何排序工作在开始时都可以用希尔排序，若在实际使用中证明它不够快， 再改成快速排序这样更高级的排序算法。

## 5.归并算法

**简介**

归并排序（Merge sort）是建立在归并操作上的一种有效的排序算法。该算法是采用分治法（Divide and Conquer）的一个非常典型的应用。

作为一种典型的分而治之思想的算法应用，将已有序的子序列合并，得到完全有序的序列.

即先使每个子序列有序，再使子序列段间有序。

归并排序的实现由两种方法：

- 自上而下的递归（所有递归的方法都可以用迭代重写，所以就有了第 2 种方法）；
- 自下而上的迭代；

和选择排序一样，归并排序的性能不受输入数据的影响，但表现比选择排序好的多，因为始终都是 O(nlogn) 的时间复杂度。代价是需要额外的内存空间。

**算法的核心概念---二路归并**

若将两个有序表合并成一个有序表，称为二路归并。

**归并过程**

1. 申请空间，使其大小为两个已经排序序列之和，该空间用来存放合并后的序列；
2. 设定两个指针，最初位置分别为两个已经排序序列的起始位置；
3. 比较两个指针所指向的元素，选择相对小的元素放入到合并空间，并移动指针到下一位置；
4. 重复步骤 3 直到某一指针达到序列尾；
5. 将另一序列剩下的所有元素直接复制到合并序列尾。

**二路归并例子演示**

如下图所示，初始状态时，a序列[2,3,5]和b序列[2,9]为已排序好的子序列，现在利用二路归并，将a和b合并为有序序列 r，初始时，i指向a的第一个元素，j指向b的第一个元素，k初始值等于0。

==说明==:r中最后一个元素起到哨兵的作用，灰色显示。

<img src="http://ww1.sinaimg.cn/large/007UR4Zcly1ga3a8xi5loj30b109j3yc.jpg" alt="merge1.webp" style="zoom:67%;" />

第一步，比较a[i]和b[j]，发现相等，如果规定相等时，a的先进入r，则如下图所示，i, k分别加1，为了形象化，归并后的元素不再绘制。

<img src="http://ww1.sinaimg.cn/large/007UR4Zcly1ga3a8xhx0zj30bf0a80sk.jpg" alt="merge2.webp" style="zoom:67%;" />

第二步，继续比较，此时b[j]小，所以b的元素2进入r，则如下图所示，j, k分别加1，

<img src="http://ww1.sinaimg.cn/large/007UR4Zcly1ga3a8xi05xj30av09p0sk.jpg" alt="merge3.webp" style="zoom:67%;" />

第三步，继续比较，此时a[i]小，所以a的元素3进入r，则如下图所示，i, k分别加1，

<img src="http://ww1.sinaimg.cn/large/007UR4Zcly1ga3a8xjxxlj30ar0am3yc.jpg" alt="merge4.webp" style="zoom:67%;" />

第四步，继续比较，此时a[i]小，所以a的元素5进入r，则如下图所示，i, k分别加1，此时序列a的3个元素已经归并完，b中还剩下一个，这个可以通过k可以看出，它还没有到达个数5。

<img src="http://ww1.sinaimg.cn/large/007UR4Zcly1ga3a8xl4h2j30bi0aaq2r.jpg" alt="merge5.webp" style="zoom: 67%;" />

第五步，将序列b中的所有剩余元素直接放入r中即可，不用做任何比较了，直至b变空，二路归并结束。

<img src="http://ww1.sinaimg.cn/large/007UR4Zcly1ga3a8xjxdwj30b80badfn.jpg" alt="merge6.webp" style="zoom:67%;" />

**步骤**

1. 先把待排序区间 [s,t] 以中点二分；
2. 接着把左边子区间排序；
3. 再把右边子区间排序；
4. 最后把左区间和右区间用一次归并操作合并成有序的区间 [s,t] 。

**代码**

```python
def mergeSort(arr):
    import math
    if(len(arr)<2):
        return arr
    middle = math.floor(len(arr)/2)
    left, right = arr[0:middle], arr[middle:]
    return merge(mergeSort(left), mergeSort(right))

def merge(left,right):
    result = []
    while left and right:
        if left[0] <= right[0]:
            result.append(left.pop(0));
        else:
            result.append(right.pop(0));
    while left:
        result.append(left.pop(0));
    while right:
        result.append(right.pop(0));
    return result
```

**算法评价**

归并排序的时间复杂度为O(nlogn) ，因为递归每次按照一半分区，并且merge需要线性时间。最重要的是该算法中最好、最坏和平均的时间性能都是O(nlogn)。

归并排序的空间复杂度为O(n)，会占用内存。

总之，归并排序虽然比较占用内存，但却是一种效率高且稳定的算法。

## 8.计数排序

**简介**

计数排序的核心在于将输入的数据值转化为键存储在额外开辟的数组空间中。作为一种线性时间复杂度的排序，计数排序要求输入的数据必须是有确定范围的整数。

**代码**

```python
def countingSort(arr, maxValue):
    bucketLen = maxValue+1
    bucket = [0]*bucketLen
    sortedIndex =0
    arrLen = len(arr)
    for i in range(arrLen):
        if not bucket[arr[i]]:
            bucket[arr[i]]=0
        bucket[arr[i]]+=1
    for j in range(bucketLen):
        while bucket[j]>0:
            arr[sortedIndex] = j
            sortedIndex+=1
            bucket[j]-=1
    return arr
```

## 9.桶排序

**简介**

桶排序是计数排序的升级版。它利用了函数的映射关系，高效与否的关键就在于这个映射函数的确定。为了使桶排序更加高效，我们需要做到这两点：

1. 在额外空间充足的情况下，尽量增大桶的数量
2. 使用的映射函数能够将输入的 N 个数据均匀的分配到 K 个桶中

同时，对于桶中元素的排序，选择何种比较排序算法对于性能的影响至关重要。

**什么时候最快**

当输入的数据可以均匀的分配到每一个桶中。

**什么时候最慢**

当输入的数据被分配到了同一个桶中。

**代码**

```python
def bucket_sort(s):
    """桶排序"""
    min_num = min(s)
    max_num = max(s)
    # 桶的大小
    bucket_range = (max_num-min_num) / len(s)
    # 桶数组
    count_list = [ [] for i in range(len(s) + 1)]
    # 向桶数组填数
    for i in s:
        count_list[int((i-min_num)//bucket_range)].append(i)
    s.clear()
    # 回填，这里桶内部排序直接调用了sorted
    for i in count_list:
        for j in sorted(i):
            s.append(j)
```

## 10.基数排序

基数排序是一种非比较型整数排序算法，其原理是将整数按位数切割成不同的数字，然后按每个位数分别比较。由于整数也可以表达字符串（比如名字或日期）和特定格式的浮点数，所以基数排序也不是只能使用于整数。

**相关概念**

- 关键码位数

待排序序列中最大数的位数，例如序列 [2, 10, 8, 234]，关键码为：0,1,2,3,4,8，关键码位数 d 为6 。如果关键码是数值型的那么上限为 10个。

- radix

==关键码==(就是每个数字的个位数)的取值范围，例如序列 [2, 10, 8, 234]，按照从右数的顺序第一位d1=1时的关键码的取值为 4,8,0,2，即范围为0~8 。
- 记录数

待排序的个数

- 桶

基数排序中，桶的编号为关键码的取值。若关键码为数值型，则桶的编号为0~9，共10个不同的桶。
- 分配

将记录按照某位（比如从右往左数第1位）将记录分配到编号为0~10的桶中的过程。比如 [2, 10, 8, 234]第一次分配（第一次分配定义为按照从右往左数的第1位）后，桶0中有10，桶2中有2，桶4中有234，桶8中有8，其他桶，比如1,3,5,6,7,9桶中都没有记录。
- 收集

分配后需要对桶中的记录再串起来，这个过程叫做收集。比如，上面的序列收集后的结果为（按照从桶0到桶9的顺序收集）10, 2，234，8 。

**简介**

基数排序（**radix sort**），属于“分配式排序”（distribution sort）。

基数排序算法先要求计算出待排序序列的最大位数，将记录切割成不同的数字，按照最高位优先或者最低位优先的规则遍历；

每次遍历中：

1. 分配。首先要将待排序序列中的当前位上的数字找到对应的桶；
2. 收集。分配后需要对桶中的记录再串起来，形成一个新的排序序列，供下一次分配用。

直至遍历完成，得到排序好的序列。

- **最高位优先** (Most Significant Digit first)法，简称MSD法：先按key = 1 排序分组，再对各组按k = 2 排序分成子组，对后面的关键码继续这样的排序分组，直到按最右位关键码 k = d对各子组排序后。

- **最低位优先** (Least Significant Digit first)法，简称LSD法：先从k = d开始排序，再对k = d-1进行排序，依次重复，直到对k = 1排序后便得到一个有序序列。

**代码**

```python
def RadixSort(list):
    i = 0                                    #初始为个位排序
    n = 1                                     #最小的位数置为1（包含0）
    max_num = max(list) #得到带排序数组中最大数
    while max_num > 10**n: #得到最大数是几位数
        n += 1
    while i < n:
        bucket = {} #用字典构建桶
        for x in range(10):
            bucket.setdefault(x, []) #将每个桶置空
        for x in list: #对每一位进行排序
            radix =int((x / (10**i)) % 10) #得到每位的基数
            bucket[radix].append(x) #将对应的数组元素加入到相 #应位基数的桶中
        j = 0
        for k in range(10):
            if len(bucket[k]) != 0: #若桶不为空
                for y in bucket[k]: #将该桶中每个元素
                    list[j] = y #放回到数组中
                    j += 1
        i += 1
return  list
```

**算法评价**

**基数排序是稳定的排序算法**。

因为每个桶内的元素个数是未知的，所以需要借助链表结构来实施分配时向桶内仍记录的过程。

借助桶编号（键）经过多次分配和采集，最终得到一个有序序列，在这个算法排序过程中，没有经过任何记录的比较，因此基数排序是很独特的排序算法。

待排序列为n个记录，d个关键码，关键码的取值范围为radix，其中，一趟分配

时间复杂度为 O(n)，一趟收集时间复杂度为O(radix)，共进行d趟分配和收集，所以链式基数排序的时间复杂度为 O(d · (n+radix) ) 。

注意这不是说这个时间复杂度一定优于O(n·log(n))，因为 d 的大小一般会受到 n 的影响。 

采用链表或线性数组存储n个记录，自然地每个记录在每趟分配的时候需要临时申请一个内存空间记录下来，此时需要的空间复杂度为O(n)；并且，每次分配时，每个桶中可能含有多条记录，每个桶再形成一个链表，再占用额外的内存空间 。

**算法比较**

基数排序 vs 计数排序 vs 桶排序

这三种排序算法都利用了桶的概念，但对桶的使用方法上有明显差异：

- 基数排序：根据键值的每位数字来分配桶；
- 计数排序：每个桶只存储单一键值；
- 桶排序：每个桶存储一定范围的数值;















