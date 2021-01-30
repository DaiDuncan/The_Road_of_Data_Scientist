按排序顺序维护列表

python的bisect模块提供了一种算法，该算法可以将元素插入到列表中，同时保持列表是有序的



## 用法

1. bisect ==返回元素在插入lst后的索引位置==，这个时候还没有进行插入操作，只是预先获知应该插入的位置
2. insort 方法将数据插入到列表的合适位置上

```python
import bisect

values = [23, 1, 54, 34, 56]

lst = []
for item in values:
    index = bisect.bisect(lst, item)        # bisect返回item准备插入的索引位置
    bisect.insort(lst, item)                # insort方法将item插入到lst中的合适位置
    print('{:3}  {:3}'.format(item, index), lst)
    
    
'''output
item index	lst
 23    0 	[23]
  1    0 	[1, 23]
 54    2 	[1, 23, 54]
 34    2 	[1, 23, 34, 54]
 56    4 	[1, 23, 34, 54, 56]
'''
```

一次插入数据，列表都保持着有序的特性，虽然你可以在全部插入后进行一次sort排序

但中间过程如果也需要有序的话，使用bisect最为合适



除了insort方法外，bisect还提供了insort_left方法和insort_right方法，他们用来处理重复数据：

- insort_left会在重复数据的左侧插入
- insort_right在重复数据的右侧插入(insort是insort_right的别名)

虽然提供了这两种方法，但实在看不出这两个方法有什么特别的用处，毕竟重复数据的位置对于列表的最终结果没有任何影响啊



## 实现原理

因为列表始终保持有序，因此只需要使用二分查找法，就可以快速定位到需要插入的位置

```python
def insort_right(a, x, lo=0, hi=None):
    '''
    a表示已有的list, x是待插入的元素
    lo表示low index
    hi表示high index
    '''
    if lo < 0:
        raise ValueError('lo must be non-negative')
    if hi is None:
        hi = len(a)
    while lo < hi:
        mid = (lo+hi)//2
        if x < a[mid]: hi = mid
        else: lo = mid+1
    a.insert(lo, x)
```