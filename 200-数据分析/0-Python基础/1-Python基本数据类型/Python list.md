# Python 基本数据类型 | list

**声明**：以下内容主要基于[cnblogs | Python基本数据类型之list](https://www.cnblogs.com/whatisfantasy/p/5956754.html)



## 一 创建列表

```
li = []
li = list()
name_list = ['alex', 'seven', 'eric']

name_list ＝ list(['alex', 'seven', 'eric'])
```





## 二 基本操作

### 索引

### 索引的特殊用法

### 切片

```
## 索引
name_list = ["eirc","morra","tommy"]
print(name_list[0])    #eirc
print (name_list[2])     #'tommy'
print (name_list[-1])    #'tommy'
print (name_list[-2])    #'morra'
print (name_list[1:])    #['morra', 'tommy']
print(name_list.index('eirc'))     #输出eric的索引
print(type(name_list[2]))        #<class 'str'>索引取出来后，元素的类型


## 索引的特殊用法
>>> li=[1,2,3,4,5,6,7,8]
>>> li[::-1]        #倒叙
[8, 7, 6, 5, 4, 3, 2, 1]
>>> li[::-2]        #元素之间间隔2
[8, 6, 4, 2]
>>> li[::-3]
[8, 5, 2]


## 切片
name_list = ["eirc","morra","tommy"]
print(name_list[0:2])       #['eirc', 'morra']
print(name_list[2:len(name_list)])       #['tommy']
print(type(name_list[1:2]))        #<class 'list'>切片取出之后是列表类型
```



### 增：追加与扩展

### 增：插入



```
## 追加
name_list = ["eirc","morra","tommy"]
name_list.append('456')     #直接操作对象，无返回值
print(name_list)

OUTPUT:
['eirc', 'morra', 'tommy', '456']


## 扩展
name_list = ["eirc","morra","tommy"]
name_list2 = ["bom"]
name_list.extend(name_list2)       #直接操作对象，无返回值
print(name_list)

OUTPUT:
['eirc', 'morra', 'tommy', 'bom']


## 插入
name_list = ["eirc","morra","tommy"]
name_list.insert(1,'hello')        #直接操作对象，无返回值
print(name_list)

OUTPUT:
['eirc', 'hello', 'morra', 'tommy']
```

删：删除

```
## pop()：删除尾部元素
name_list = ["eirc","morra","tommy"]
temp = name_list.pop()        #删除尾部元素，并把所删除的元素存到一个新值里
print(name_list)
print(temp)

OUTPUT:
['eirc', 'morra']
tommy


## remove()：删除指定元素，只能有一个参数（匹配从左到右的第一个），不能加index
name_list = ["eirc","morra","tommy"]
name_list.remove("morra")
print(name_list)

OUTPUT:
['eirc', 'tommy']


## del:删除指定元素，可以使用索引和切片
name_list = ["eirc","morra","tommy","bom"]
del name_list[0]
print(name_list)

del name_list[1:2]      
print(name_list)
OUTPUT：
['morra', 'tommy', 'bom']
['morra', 'bom']
```



### 列表脚本操作符

列表对+和*的操作符与字符串相似：

- +号用于组合列表
- *号用于重复列表

```
print len([1, 2, 3]); #3
print [1, 2, 3] + [4, 5, 6]; #[1, 2, 3, 4, 5, 6]
print ['Hi!'] * 4; #['Hi!', 'Hi!', 'Hi!', 'Hi!']
print 3 in [1, 2, 3] #True
for x in [1, 2, 3]: print x, #1 2 3
```

### 拷贝

list的拷贝有5种方式

```
#1
listb = lista[:]

#2
listb = list(lista)

#3
listb = [i for i in lista]

#4
import copy
listb = copy.copy(lista)           #浅拷贝

#5
import copy
 listb = copy.deepcopy(lista)      #深拷贝
```



### 补充：业务代码(特定框架)

注意：在业务代码里，一般会把列表写成 a = [1,2,3,]的形式，在使用特定框架时会用到





## 三 遍历迭代：for循环

- 判断一个数据类型是否可迭代

```
class list(object):
    """
    list() -> new empty list
    list(iterable) -> new list initialized from iterable's items
    """
    
    
## 判断一个数据类型是否可迭代：
from collections import Iterable    #使用collections模块的Iterable类型来判断

li = [1,2,3,4]
ret = isinstance(li, Iterable)
print(ret)    #True
```

### 字符串转列表

### 元组转列表

### 字典转列表：默认循环key

```
## 字符串转列表
s = "你好morra"
li = list(s)
print(li)

OUTPUT:
['你', '好', 'm', 'o', 'r', 'r', 'a']


## 元组转列表
tu = ("你好","alex")
li = list(tu)
print(li)

OUTPUT:
['你好', 'alex']


## 字典转列表
dic = {'k1':'hello','k2':'morra'}
l3 = list(dic)              #字典在循环的时候默认只循环key
print(l3)

l4 = list(dic.values())
print(l4)

l5 = list(dic.items())
print(l5)

OUTPUT:
['k2', 'k1']    #字典无序
['morra', 'hello']
[('k2', 'morra'), ('k1', 'hello')]
```





## 四 源码

```
class list(object):
    """
    list() -> new empty list
    list(iterable) -> new list initialized from iterable's items
    """
    def append(self, p_object): 
        """追加，直接操作对象，无返回值"""
        """ L.append(object) -> None -- append object to end """
        pass

    def clear(self): 
        """ L.clear() -> None -- remove all items from L """
        pass

    def copy(self): 
        
        """ L.copy() -> list -- a shallow copy of L """
        return []

    def count(self, value): 
        """ L.count(value) -> integer -- return number of occurrences of value """
        return 0

    def extend(self, iterable): 
        """ L.extend(iterable) -> None -- extend list by appending elements from the iterable """
        pass

    def index(self, value, start=None, stop=None): 
        """
        L.index(value, [start, [stop]]) -> integer -- return first index of value.
        Raises ValueError if the value is not present.
        """
        return 0

    def insert(self, index, p_object): 
        """ L.insert(index, object) -- insert object before index """
        pass

    def pop(self, index=None): 
        """
        L.pop([index]) -> item -- remove and return item at index (default last).
        Raises IndexError if list is empty or index is out of range.
        """
        pass

    def remove(self, value): 
        """
        L.remove(value) -> None -- remove first occurrence of value.
        Raises ValueError if the value is not present.
        """
        pass

    def reverse(self): 
        """列表倒序"""
        """ L.reverse() -- reverse *IN PLACE* """
        pass

    def sort(self, key=None, reverse=False): 
        """"快速排序，问：包含字母数字中文的列表该怎么排序？"""
        """ L.sort(key=None, reverse=False) -> None -- stable sort *IN PLACE* """
        pass

    def __add__(self, *args, **kwargs): 
        """ Return self+value. """
        pass

    def __contains__(self, *args, **kwargs): 
        """ Return key in self. """
        pass

    def __delitem__(self, *args, **kwargs): 
        """ Delete self[key]. """
        pass

    def __eq__(self, *args, **kwargs): 
        """ Return self==value. """
        pass

    def __getattribute__(self, *args, **kwargs): 
        """ Return getattr(self, name). """
        pass

    def __getitem__(self, y): 
        """ x.__getitem__(y) <==> x[y] """
        pass

    def __ge__(self, *args, **kwargs): 
        """ Return self>=value. """
        pass

    def __gt__(self, *args, **kwargs): 
        """ Return self>value. """
        pass

    def __iadd__(self, *args, **kwargs): 
        """ Implement self+=value. """
        pass

    def __imul__(self, *args, **kwargs): 
        """ Implement self*=value. """
        pass

    def __init__(self, seq=()): # known special case of list.__init__
        """
        list() -> new empty list
        list(iterable) -> new list initialized from iterable's items
        # (copied from class doc)
        """
        pass

    def __iter__(self, *args, **kwargs): 
        """ Implement iter(self). """
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

    def __mul__(self, *args, **kwargs): 
        """ Return self*value.n """
        pass

    @staticmethod # known case of __new__
    def __new__(*args, **kwargs): 
        """ Create and return a new object.  See help(type) for accurate signature. """
        pass

    def __ne__(self, *args, **kwargs): 
        """ Return self!=value. """
        pass

    def __repr__(self, *args, **kwargs): 
        """ Return repr(self). """
        pass

    def __reversed__(self): 
        """ L.__reversed__() -- return a reverse iterator over the list """
        pass

    def __rmul__(self, *args, **kwargs): 
        """ Return self*value. """
        pass

    def __setitem__(self, *args, **kwargs): 
        """ Set self[key] to value. """
        pass

    def __sizeof__(self): 
        """ L.__sizeof__() -- size of L in memory, in bytes """
        pass

    __hash__ = None
```

## 参考文献

[cnblogs | Python基本数据类型之list](https://www.cnblogs.com/whatisfantasy/p/5956754.html)