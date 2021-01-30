适用于列表的最小堆排序算法

heapq模块实现了适用于python列表的最小堆排序算法，该模块提供heappush和heapify两种方法来构建最小堆

- heappop方法可以删除堆顶元素
- nlargest和nsmallest可以分别查看最大的几个元素和最小的几个元素





假定在数据记录中存在一个能够标识数据记录的数据项，并可依据该数据项对数据进行组织，则称此数据项为关键码（key）：

- 最小堆中，父节点的关键码小于等于它的左右子女的关键码

- 最大堆中，父节点的关键码大于等于左右子女的关键码



## 打印堆的二叉树结构

```python
import math
from io import StringIO

def print_heap(heap, width=36, fill=' '):
    output = StringIO()
    last_row = -1
    for index, item in enumerate(heap):
        # 计算元素在哪一行
        if index:
            row = int(math.floor(math.log(index + 1, 2)))
        else:
            row = 0

        # 换行操作
        if row != last_row:
            output.write('\n')
        columns = 2 ** row   # 计算一行有多少个数据
        col_width = int(math.floor(width / columns))
        output.write(str(item).center(col_width, fill))
        last_row = row
    print(output.getvalue())


print_heap(heap_lst)
'''
                 1                  
        2                 5         
    7        6        10       8    
 9  
'''
```



# 构建最小堆

## 基于heappush

```python
import heapq

random_lst = [5, 7, 2, 1, 6, 10, 8, 9]
heap_lst = []

for item in random_lst:
    heapq.heappush(heap_lst, item)

print(heap_lst)	#[1, 2, 5, 7, 6, 10, 8, 9]
```

使用heappush方法，将混乱的数据==依次放入==到heap_lst中，就会得到一个最小堆





## 基于heapify

使用heapify方法可以直接将一个混乱的列表调整为一个最小堆，使用的是向下调整算法

```python
random_lst = [5, 7, 2, 1, 6, 10, 8, 9]
heapq.heapify(random_lst)
print_heap(random_lst)
```





# 访问堆里的元素

## heappop 删除堆顶元素

堆顶元素就是列表的第一个元素，删除它是一件很容易的事情

但删除后要立即对堆进行调整，使之维持堆的特性，这些都已经被heappop方法实现。

```python
random_lst = [9, 7, 5, 2, 1, 6, 10, 8]
heapq.heapify(random_lst)
print_heap(random_lst)

print("-"*20)

heapq.heappop(random_lst)
print_heap(random_lst)
'''
                 1                  
        2                 5         
    8        7        6        10   
 9  
--------------------

                 2                  
        7                 5         
    8        9        6        10   
'''
```





## heapreplace 替换堆顶元素

heapreplace方法允许使用新的元素替换掉堆顶的元素，这个过程等价于先删除堆顶元素然后将新的元素加入到堆中，先后进行了两次调整

```python
random_lst = [9, 7, 5, 2, 1, 6, 10, 8]
heapq.heapify(random_lst)
print_heap(random_lst)

print("-"*20)

heapq.heapreplace(random_lst, 4)
print_heap(random_lst)
'''
                 1                  
        2                 5         
    8        7        6        10   
 9  
--------------------

                 2                  
        4                 5         
    8        7        6        10   
 9  
'''
```





# Top k问题

- heapq.nlargest()
- heapq.nsmallest()

在==处理大规模数据时==，要从海量数据中寻找最大K个数据，或者最小K个数，这类问题被称之为Top k问题

这么多的数据不可能都载入内存进行排序，使用最小堆就可以完美的解决这个问题

- 构建一个容量为20的最小堆，先放入20个元素
  - 此后所有的数据都和堆顶的元素进行大小比较，如果比堆顶元素大，则替换堆顶元素
  - 否则不做任何处理
- 遍历完这10亿个数据后，最小堆里==剩下的数据==就是最大的20个数

```python
random_lst = [9, 7, 5, 2, 1, 6, 10, 8, 4, 3]
heap_lst = []

for item in random_lst[:3]:
    heapq.heappush(heap_lst, item)

for item in random_lst[3:]:
    if item > heap_lst[0]:
        heapq.heapreplace(heap_lst, item)

print(heap_lst)	#[8, 9, 10]
```





对于一个给定的最小堆，nlargest和nsmallest可以分别查看最大的几个元素和最小的几个元素

```python
random_lst = [9, 7, 5, 2, 1, 6, 10, 8, 4, 3]
heapq.heapify(random_lst)

print("最大的3个元素", heapq.nlargest(3, random_lst))		#最大的3个元素 [10, 9, 8]
print("最小的3个元素", heapq.nsmallest(3, random_lst))	#最小的3个元素 [1, 2, 3]
```





# heapq.merge() 合并多个已排序列表

可以使用归并排序来处理这个问题

当需要合并的列表太多时，归并排序就会变的麻烦，毕竟，我们平时接触到归并排序的问题通常是两个列表

```python
import heapq
import random

lst = []

for i in range(5):
    l = random.sample(range(1, 100), 5)
    l.sort()
    print(l)
    lst.append(l)

print("合并后")
merge = [i for i in heapq.merge(*lst)]
print(merge)	#[3, 10, 13, 20, 21, 43, 48, 53, 54, 56, 60, 62, 63, 63, 69, 70, 75, 77, 80, 81, 83, 89, 93, 95, 97]
```