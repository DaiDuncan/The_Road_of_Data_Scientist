![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210412104111.webp)

Python入门阶段，通常只需掌握list、tuple、set、dict这类数据结构，做到灵活使用即可。

然而，随着学习的深入，平时遇到实际场景变复杂，很有必要去了解Python内置的更加强大的数据结构deque、heapq、Counter、OrderedDict、defaultDict、ChainMap，掌握它们，往往能让你少写一些代码且能更加高效的实现功能。



Python常用的10种数据结构：

- 4种常用的基本结构
- 6种基于它们优化的适应于特定场景的结构



## 1 list

1 [link: 基本用法](http://mp.weixin.qq.com/s?__biz=MzI3NTkyMjA4NA==&mid=2247496130&idx=1&sn=b1ea2d463a04c97d2db09e10c3acfc5c&chksm=eb7fdc09dc08551f35594f21383a8b7a899d2d5586796df6ac35ec0e05557b7df8860f9e5162&scene=21#wechat_redirect)

2 **使用场景** 

list 使用在需要查询、修改的场景，极不擅长需要**频繁插入、删除元素**的场景。

3 **实现原理** 

list对应数据结构的线性表，列表长度在初始状态时无需指定，当插入元素超过初始长度后**再启动动态扩容**，删除时尤其位于列表开始处元素，时间复杂度为O(n)



## 2 tuple

元组是一类不允许添加删除元素的特殊列表，也就是一旦创建后续决不允许增加、删除、修改。

1 **基本用法** 

元组大量使用在打包和解包处，如函数有多个返回值时打包为一个元组，赋值到等号左侧变量时解包。

```python
In [22]: t=1,2,3                                         
In [23]: type(t)                              
Out[23]: tuple
```



2 **使用场景** 

如果非常确定你的对象后面不会被修改，则可以大胆使用元组。

为什么？因为相比于list, tuple实例更加节省内存，这点尤其重要。

```python
In [24]: from sys import getsizeof                                              

In [25]: getsizeof(list())                                                      
Out[25]: 72 # 一个list实例占用72个字节

In [26]: getsizeof(tuple())                                                     
Out[26]: 56 # 一个tuple实例占用56个字节
```

备注：创建100个实例，tuple能节省1K多字节





## 3 set

1 **基本用法** 

set是一种里面不能含有重复元素的数据结构，这种特性天然的使用于列表的去重

```python
In [27]: a=[3,2,5,2,5,3]                                                        

In [28]: set(a)                                                                 
Out[28]: {2, 3, 5}
```

除此之外，还有知道set结构可用于两个set实例的求交集、并集、差集等操作

```python
In [29]: a = {2,3,5}                                                            
In [30]: b = {3,4,6,2}                                                          

In [31]: a.intersection(b) # 求交集                                                      
Out[31]: {2, 3}
```

2 **使用场景** 

如果只是想缓存某些元素值，且要求元素值不能重复时，适合选用此结构。

并且set内允许增删元素，且效率很高。



3 **实现原理** 

set在内部将值哈希为索引，然后按照索引去获取数据，因此删除、增加、查询元素效果都很高。



## 4 dict

1 **基本用法** 

dict 是Python中使用最频繁的数据结构之一，字典创建由通过dict函数、{}写法、==字典生成式==等，增删查元素效率都很高。

```python
d = {'a':1,'b':2} # {}创建字典

# 列表生成式
In [38]: d = {a:b for a,b in zip(['a','b'],[1,2])}                              
In [39]: d                                                                      
Out[39]: {'a': 1, 'b': 2}
```



2 **使用场景** 

字典尤其适合在查询多的场景，时间复杂度为O(1). 如leetcode第一题求解两数之和时，就会使用到dict的O(1)查询时间复杂度。



同时，Python类中属性值等信息也都是缓存在`__dict__`这个字典型数据结构中。

但是值得注意，dict占用字节数是list、tuple的3、4倍，因此对内存要求苛刻的场景要慎重考虑。

```python
In [40]: getsizeof(dict())                                                      
Out[40]: 248
```



3 **实现原理** 

字典是一种哈希表，同时保存了键值对。





## 5 deque

1 **基本用法** 

deque 双端队列，基于list优化了列表两端的增删数据操作。

基本用法：

```python
from collections import deque

In [3]: d = deque([3,2,4,0])                                                    

In [4]: d.popleft() # 左侧移除元素，O(1)时间复杂度                                                            
Out[4]: 3

In [5]: d.appendleft(3) # 左侧添加元素，O(1)时间复杂度                                                       

In [6]: d                                                                       
Out[6]: deque([3, 2, 4, 0])
```



2 **使用场景** 

list左侧添加删除元素的时间复杂度都为O(n)，所以在Python模拟队列时切忌使用list，相反使用deque双端队列非常适合频繁在列表两端操作的场景。

但是，加强版的deque牺牲了空间复杂度，所以嵌套deque就要仔细trade-off:

```python
In [9]: sys.getsizeof(deque())                                                  
Out[9]: 640

In [10]: sys.getsizeof(list())                                                  
Out[10]: 72
```



3 **实现原理** 

cpython实现deque使用默认长度64的数组，每次从左侧移除1个元素，leftindex加1，如果超过64释放原来的内存块，再重新申请64长度的数组，并使用双端链表block管理内存块。





## 6 Counter

1 **基本用法** 

Counter一种继承于dict用于统计元素个数的数据结构，也称为bag 或 multiset. 基本用法：

```python
from collections import Counter
In [14]: c = Counter([1,3,2,3,4,2,2]) # 统计每个元素的出现次数
In [17]: c                                                                      
Out[17]: Counter({1: 1, 3: 2, 2: 3, 4: 1})

# 除此之外，还可以统计最常见的项
# 如统计第1最常见的项，返回元素及其次数的元组
In [16]: c.most_common(1)                                                       
Out[16]: [(2, 3)]
```

2 **使用场景** 

基本的dict能解决的问题就不要用Counter，==但如遇到统计元素出现频次的场景，就不要自己去用dict实现了，果断选用Counter.==

需要注意，Counter统计的元素要求可哈希(hashable)，换句话说如果统计list的出现次数就不可行，不过**list转化为tuple不就可哈希了吗**.



3 **实现原理** 

Counter实现基于dict，它将元素存储于keys上，出现次数为values.



## 7 OrderedDict

1 **基本用法** 

继承于dict，能确保keys值按照顺序取出来的数据结构，基本用法：

```python
In [25]: from collections import OrderedDict                                    

In [26]: od = OrderedDict({'c':3,'a':1,'b':2})                                  

In [27]: for k,v in od.items(): 
    ...:     print(k,v) 
    ...:                                                                        
c 3
a 1
b 2
```



2 **使用场景** 

基本的dict无法保证顺序，keys映射为哈希值，而此值不是按照顺序存储在散列表中的。

所以遇到要确保字典keys有序场景，就要使用OrderedDict.



3 **实现原理** 

你一定会好奇OrderedDict如何确保keys顺序的，**翻看cpython**看到它里面维护着一个==双向链表`self.__root`，它维护着keys的顺序==。



既然使用双向链表，细心的读者可能会有疑问：删除键值对如何保证O(1)时间完成？

cpython使用空间换取时间的做法，==内部维护一个`self.__map`字典，键为key，值为指向双向链表节点的`link`==. 这样在删除某个键值对时，通过__map在O(1)内找到link，然后O(1)内从双向链表__root中摘除。



## 8 heapq

1 **基本用法** 

基于list优化的一个数据结构：堆队列，也称为优先队列。

堆队列特点在于**最小的元素总是在根结点**：heap[0] 基本用法：

```python
import heapq
In [41]: a = [3,1,4,5,2,1]                                                      

In [42]: heapq.heapify(a) # 对a建堆，建堆后完成对a的就地排序
In [43]: a[0] # a[0]一定是最小元素
In [44]: a
Out[44]: [1, 1, 3, 5, 2, 4]

In [46]: heapq.nlargest(3,a) # a的前3个最大元素                                                    
Out[46]: [5, 4, 3]

In [47]: heapq.nsmallest(3,a) # a的前3个最小元素                                                  
Out[47]: [1, 1, 2]
```

2 **使用场景** 

如果想要统计list中前几个最小(大)元素，那么使用heapq很方便，同时它还提供合并多个有序小list为大list的功能。



3 **基本原理** 

堆是一个二叉树，它的每个父节点的值都只会小于或大于所有孩子节点（的值），原理与堆排序极为相似。



## 9 defaultdict⭐

1 **基本用法** 

defaultdict是一种带有默认工厂的dict，如果对设计模式不很了解的读者可能会很疑惑工厂这个词，准确来说工厂全称为对象工厂。下面体会它的基本用法。

基本dict键的值没有一个默认数据类型，如果值为list，必须要手动创建：

```python
words=['book','nice','great','book']
d = {}
for i,word in enumerate(words):
    if word in d:
        d[word].append(i)
    else:
        d[word]=[i] # 显示的创建一个list
```



但是使用defaultdict：

```python
from collections import defaultdict
d = defaultdict(list) # 创建字典值默认为list的字典
for i,word in enumerate(words):
    d[word] = i 
```

省去一层if逻辑判断，代码更加清晰。



上面defaultdict(list)这行代码默认创建值为list的字典，还可以构造defaultdict(set), defaultdict(dict)等等，这种模式就是对象工厂，工厂里能制造各种对象：list,set,dict...



2 **使用场景** 

上面已经说的很清楚，适用于键的值**必须指定一个默认值的场景**，如键的值为list,set,dict等。



3 **实现原理** 

基本原理就是调用工厂函数去提供缺失的键的值。后面设计模式专题再详细探讨。



## 10 ChainMap

1 **基本用法** 

如果有多个dict想要合并为一个大dict，那么ChainMap将是你的选择，它的方便性体现在同步更改。具体来看例子：

```python
In [55]: from collections import ChainMap                                       

In [56]: d1 = {'a':1,'c':3,'b':2}                                               

In [57]: d2 = {'d':1,'e':5}                                                     

In [58]: dm = ChainMap(d1,d2)                                                   

In [59]: dm                                                                     
Out[59]: ChainMap({'a': 1, 'c': 3, 'b': 2}, {'d': 1, 'e': 5})
```



ChainMap后返回一个大dict视图，如果修改其对应键值对，原小dict也会改变：

```python
In [86]: dm.maps  # 返回一个字典list                                                               
Out[86]: [{'a': 2, 'c': 3, 'b': 2, 'd': 10}, {'d': 1, 'e': 5}]

In [87]: dm.maps[0]['d']=20   # 修改第一个dict的键等于'd'的值为20                                                   

In [88]: dm                                                                     
Out[88]: ChainMap({'a': 2, 'c': 3, 'b': 2, 'd': 20}, {'d': 1, 'e': 5})

In [89]: d1 # 原小dict的键值变为20                                                                    
Out[89]: {'a': 2, 'c': 3, 'b': 2, 'd': 20}
```



2 **使用场景** 

具体使用场景是我们有**多个字典或者映射**，想把它们合并成为一个单独的映射，有读者可能说可以用update进行合并，

- 这样做的问题就是新建了一个内存结构，除了浪费空间外，
- 还有一个缺点就是我们对新字典的更改不会同步到原字典上。





3 **实现原理** 

通过maps便能观察出ChainMap联合多个小dict装入list中，实际确实也是这样实现的，内部维护一个lis实例，其元素为小dict.











# #参考文献

[link: 数据结构1](https://mp.weixin.qq.com/s/aA2XPwS5qRRpUBQnbmfB6A)

[link: 数据结构2](https://mp.weixin.qq.com/s/0s7zOHuU7VQF1q0kRXa63w)



