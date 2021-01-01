# Python 基本数据类型 | set

set是一个**无序且不重复**的元素集合。

集合对象是一组无序排列的**不****可哈希**的值，集合成员可以做字典中的键。

集合支持：

- 用in和not in操作符检查成员
- 由len()内建函数得到集合的基数(大小)
- 用 for 循环迭代集合的成员。
- 但是因为集合本身是**无序**的，不可以为集合创建索引或执行切片(slice)操作，也没有键(keys)可用来获取集合中元素的值。



set和dict一样，只是没有value，**相当于dict的key集合**，由于dict的key是**不重复**的，且key是**不可变对象**因此set也有如下特性：

- 不重复
- 元素为不可变对象





## 概念：可哈希 VS 不可哈希

### 1 不严谨但易懂的解释

一个对象在其生命周期内，如果保持不变，就是hashable（可哈希的）。

hashable ≈ imutable   可哈希 ≈ 不可变



在Python中：

list、set和dictionary都是可改变的，比如可以通过list.append()，set.remove()，dict['key'] = value对其进行修改，所以它们都是不可哈希的；

而tuple和string是不可变的，只可以做**复制或者切片**等操作，所以它们就是可哈希的。

### 2 官方解释

> An object is hashable if it has a hash value which never changes during its lifetime (it needs a __hash__() method), and can be compared to other objects (it needs an __eq__() or __cmp__() method). Hashable objects which compare equal must have the same hash value.
>
> 
>
> Hashability makes an object usable as a dictionary key and a set member, because these data structures use the hash value internally.
>
> 
>
> All of Python’s immutable built-in objects are hashable, while no mutable containers (such as lists or dictionaries) are. Objects which are instances of user-defined classes are hashable by default; they all compare unequal, and their hash value is their id().

如果一个对象在其生命周期内，其哈希值从未改变(这需要一个__hash__()方法)，并且可以与其他对象进行比较(这需要一个__eq__()或__cmp__()方法)，那么这个对象就是可哈希的。哈希对象的相等意味着其**哈希值的相等**。

Python 的某些链接库在内部需要使用hash值，例如往集合中添加对象时会用__hash__() 方法来获取hash值，看它是否与集合中现有对象的hash值相同，如果相同则会舍去不加入，如果不同，则使用__eq__() 方法**比较是否相等**，以确定是否需要加入其中。



哈希性使得对象可以用作dictionary键和set成员，因为这些数据结构在内部使用了哈希值。



Python的所有不可变的内置对象都是可hashable的，但可变容器列表、字典、集合，他们在改变值的同时却没有改变id,无法由地址定位值的唯一性,因而**无法哈希**。

对于用户定义的类的实例，默认情况下是可哈希的；它们都是不相等的，并且它们的哈希值都是id()。





## 一 创建

```
s = set()
s = {11,22,33,44}  #注意在创建空集合的时候只能使用s=set()，因为s={}创建的是空字典


a=set('boy')
b=set(['y', 'b', 'o','o'])
c=set({"k1":'v1','k2':'v2'})
d={'k1','k2','k2'}
e={('k1', 'k2','k2')}
print(a,type(a))
print(b,type(b))
print(c,type(c))
print(d,type(d))
print(e,type(e))

OUTPUT:
{'o', 'b', 'y'} <class 'set'>
{'o', 'b', 'y'} <class 'set'>
{'k1', 'k2'} <class 'set'>
{'k1', 'k2'} <class 'set'>
{('k1', 'k2', 'k2')} <class 'set'>
```



## 二 基本操作

### 集合的特性(集合运算)

#### 1）差集：比较

#### 2）交集

#### 3）并集

#### 4）合并

```
## 比较：差集
se = {11, 22, 33}
be = {22, 55}
temp1 = se.difference(be)        #找到se中存在，be中不存在的集合，返回新值
print(temp1)        #{33, 11}
print(se)        #{33, 11, 22}

temp2 = se.difference_update(be) #找到se中存在，be中不存在的集合，覆盖掉se
print(temp2)        #None
print(se)           #{33, 11},


## 交集
se = {11, 22, 33}
be = {22, 55}

temp1 = se.intersection(be)             #取交集，赋给新值
print(temp1)  # 22
print(se)  # {11, 22, 33}

temp2 = se.intersection_update(be)      #取交集并更新自己
print(temp2)  # None
print(se)  # 22


## 并集
se = {11, 22, 33}
be = {22,44,55}

temp=se.union(be)   #取并集，并赋新值
print(se)       #{33, 11, 22} 集合无序
print(temp)     #{33, 22, 55, 11, 44}


## 合并
se = {11, 22, 33}
be = {22}

temp1 = se.symmetric_difference(be)  # 合并不同项，并赋新值
print(temp1)    #{33, 11}
print(se)       #{33, 11, 22}

temp2 = se.symmetric_difference_update(be)  # 合并不同项，并更新自己
print(temp2)    #None
print(se)             #{33, 11}
```

### 改：更新

### 删：删除

```
## 改：更新
se = {11, 22, 33}
be = {22,44,55}

se.update(be)  # 把se和be合并，得出的值覆盖se
print(se)
se.update([66, 77])  # 可增加迭代项
print(se)


## 删：删除
se = {11, 22, 33}
se.discard(11)
se.discard(44)  # 移除不存的元素不会报错
print(se)

se = {11, 22, 33}
se.remove(11)
se.remove(44)  # 移除不存的元素会报错
print(se)

se = {11, 22, 33}  # 移除末尾元素并把移除的元素赋给新值
temp = se.pop()
print(temp)  # 33
print(se) # {11, 22}
```

### 判断

```
se = {11, 22, 33}
be = {22}

print(se.isdisjoint(be))        #False，判断是否不存在交集（有交集False，无交集True）
print(se.issubset(be))          #False，判断se是否是be的子集合
print(se.issuperset(be))        #True，判断se是否是be的父集合
```

### 集合的类型转换

```
se = set(range(4))
li = list(se)
tu = tuple(se)
st = str(se)
print(li,type(li))
print(tu,type(tu))
print(st,type(st))

OUTPUT:
[0, 1, 2, 3] <class 'list'>
(0, 1, 2, 3) <class 'tuple'>
{0, 1, 2, 3} <class 'str'>
```



## 三 源码

```
class set(object):
    """
    set() -> new empty set object
    set(iterable) -> new set object
    
    Build an unordered collection of unique elements.
    """
    def add(self, *args, **kwargs): 
        """添加"""
        """
        Add an element to a set.
        
        This has no effect if the element is already present.
        """
        pass

    def clear(self, *args, **kwargs): 
        """清除"""
        """ Remove all elements from this set. """
        pass

    def copy(self, *args, **kwargs): 
        """浅拷贝"""
        """ Return a shallow copy of a set. """
        pass

    def difference(self, *args, **kwargs): 
        """比较"""
        """
        Return the difference of two or more sets as a new set.
        
        (i.e. all elements that are in this set but not the others.)
        """
        pass

    def difference_update(self, *args, **kwargs): 
        """ Remove all elements of another set from this set. """
        pass

    def discard(self, *args, **kwargs): 
        """删除"""
        """
        Remove an element from a set if it is a member.
        
        If the element is not a member, do nothing.
        """
        pass

    def intersection(self, *args, **kwargs): 
        """
        Return the intersection of two sets as a new set.
        
        (i.e. all elements that are in both sets.)
        """
        pass

    def intersection_update(self, *args, **kwargs): 
        """ Update a set with the intersection of itself and another. """
        pass

    def isdisjoint(self, *args, **kwargs): 
        """ Return True if two sets have a null intersection. """
        pass

    def issubset(self, *args, **kwargs): 
        """ Report whether another set contains this set. """
        pass

    def issuperset(self, *args, **kwargs): 
        """ Report whether this set contains another set. """
        pass

    def pop(self, *args, **kwargs): 
        """
        Remove and return an arbitrary set element.
        Raises KeyError if the set is empty.
        """
        pass

    def remove(self, *args, **kwargs): 
        """
        Remove an element from a set; it must be a member.
        
        If the element is not a member, raise a KeyError.
        """
        pass

    def symmetric_difference(self, *args, **kwargs): 
        """
        Return the symmetric difference of two sets as a new set.
        
        (i.e. all elements that are in exactly one of the sets.)
        """
        pass

    def symmetric_difference_update(self, *args, **kwargs): 
        """ Update a set with the symmetric difference of itself and another. """
        pass

    def union(self, *args, **kwargs): 
        """
        Return the union of sets as a new set.
        
        (i.e. all elements that are in either set.)
        """
        pass

    def update(self, *args, **kwargs): 
        """ Update a set with the union of itself and others. """
        pass

    def __and__(self, *args, **kwargs): 
        """ Return self&value. """
        pass

    def __contains__(self, y): 
        """ x.__contains__(y) <==> y in x. """
        pass

    def __eq__(self, *args, **kwargs): 
        """ Return self==value. """
        pass

    def __getattribute__(self, *args, **kwargs): 
        """ Return getattr(self, name). """
        pass

    def __ge__(self, *args, **kwargs): 
        """ Return self>=value. """
        pass

    def __gt__(self, *args, **kwargs): 
        """ Return self>value. """
        pass

    def __iand__(self, *args, **kwargs): 
        """ Return self&=value. """
        pass

    def __init__(self, seq=()): # known special case of set.__init__
        """
        set() -> new empty set object
        set(iterable) -> new set object
        
        Build an unordered collection of unique elements.
        # (copied from class doc)
        """
        pass

    def __ior__(self, *args, **kwargs): 
        """ Return self|=value. """
        pass

    def __isub__(self, *args, **kwargs): 
        """ Return self-=value. """
        pass

    def __iter__(self, *args, **kwargs): 
        """ Implement iter(self). """
        pass

    def __ixor__(self, *args, **kwargs): 
        """ Return self^=value. """
        pass

    def __len__(self, *args, **kwargs): 
        """ Return len(self). """
        pass

    def __le__(self, *args, **kwargs): 
        """ Return self<=value. """
        pass

    def __lt__(self, *args, **kwargs): 
        """ Return self<value. """
        pass

    @staticmethod 
    def __new__(*args, **kwargs): 
        """ Create and return a new object.  See help(type) for accurate signature. """
        pass

    def __ne__(self, *args, **kwargs): 
        """ Return self!=value. """
        pass

    def __or__(self, *args, **kwargs): 
        """ Return self|value. """
        pass

    def __rand__(self, *args, **kwargs): 
        """ Return value&self. """
        pass

    def __reduce__(self, *args, **kwargs): 
        """ Return state information for pickling. """
        pass

    def __repr__(self, *args, **kwargs): 
        """ Return repr(self). """
        pass

    def __ror__(self, *args, **kwargs): 
        """ Return value|self. """
        pass

    def __rsub__(self, *args, **kwargs): 
        """ Return value-self. """
        pass

    def __rxor__(self, *args, **kwargs): 
        """ Return value^self. """
        pass

    def __sizeof__(self): 
        """ S.__sizeof__() -> size of S in memory, in bytes """
        pass

    def __sub__(self, *args, **kwargs): 
        """ Return self-value. """
        pass

    def __xor__(self, *args, **kwargs): 
        """ Return self^value. """
        pass

    __hash__ = None #不可哈希
```



## 参考文献

[[转摘\]cnblogs | python基本数据类型之set](https://www.cnblogs.com/whatisfantasy/p/5956775.html)

[概念 CSDN | 哈希 VS 不可哈希](https://blog.csdn.net/qq_17753903/java/article/details/85345996)

[概念 简书 | 哈希 VS 不可哈希](https://www.jianshu.com/p/bc5195c8c9cb)