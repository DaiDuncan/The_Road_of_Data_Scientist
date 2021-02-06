# Python 基本数据类型|list

很像C++中的vector 和 java 中的ArrayList，是大小可动态变换的数组



## 场景导入

每天给你一个小球，每个小球都一模一样，且不允许你做任何标记，在第100天的时候，我要求你拿出来我第35天给你的那个小球，请你思考，你该如何存放小球，才能保证我说出天数，你拿出天数所对应的小球。

方法：所有的小球都按顺序排放，第1天的小球排第1位，第2天的小球排第2位，以此类推，那么当我要求你找出第35天给你的小球时，你需要做的是从第1个小球开始数数，数到35时，这个小球就是我想要的。

=> 列表是数据的有序集合



## 特性

- 有序的，可变的，支持嵌套

- 列表与字典相反，查找和插入速度随着元素的增加而变慢，但占用的内存较小





### 列表操作符+ *

列表对+和*的操作符与字符串相似：

- +号用于组合列表
- *号用于重复列表

```python
print len([1, 2, 3]); #3
print [1, 2, 3] + [4, 5, 6]; #[1, 2, 3, 4, 5, 6]
print ['Hi!'] * 4; #['Hi!', 'Hi!', 'Hi!', 'Hi!']
print 3 in [1, 2, 3] #True
for x in [1, 2, 3]: print x, #1 2 3
```





## 操作1|创建

- 列表推导式
- 常用：range()

```python
li = []
li = list()

name_list = ['alex', 'seven', 'eric']
name_list ＝ list(['alex', 'seven', 'eric'])


list1 = [1, 2, 3, 4, 5 ]
list2 = [1, 2, '3', True]
list3 = [[1,2,3], True, 1]
```



### 嵌套列表

```python
lst = [1, [1, 5], 3, [9, 8, [1, 3]], 5]
print(lst[3][2][0])
print(lst[1:4])
```

| 索引 | 数据           |
| :--- | :------------- |
| 0    | 1              |
| 1    | [1, 5]         |
| 2    | 3              |
| 3    | [9, 8, [1, 3]] |
| 4    | 5              |

lst\[3]\[2][0]的值是1



### 拷贝

list的拷贝有5种方式

```python
#1
listb = lista[:]

#2
listb = list(lista)

#3
listb = [i for i in lista]

#4
import copy
listb = copy.copy(lista)           #浅拷贝：父对象独立

#5
import copy
 listb = copy.deepcopy(lista)      #深拷贝：父、子对象(比如列表中嵌套列表，即列表子对象)均独立
```





## 操作2|查 选/索引 改 增 删

### 查

```python
list[start: end: step] #切片

in; not in  #查找，返回布尔值

.index() #返回索引，可指定索引范围，查不到也不会报错
```





### 选/索引|含切片

```python
## 索引  [start:end), 左闭右开
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

应用示例|分组：

```python
lst = [2, 3, 4, 1, 5, 2, 7, 1, 8, 9, 10, 31, 22, 34]


lst = [2, 3, 4, 1, 5, 2, 7, 1, 8, 9, 10, 31, 22, 34]

step = 3
sum_lst = []
for i in range(0, len(lst), step):
    tmp = lst[i: i+step]
    sum_lst.append(sum(tmp))

print(sum_lst)
```



### 改

```python
### 修改：直接赋值
L = ['Windrivder', 21, {'name': 'xiaoming'}]; name, age, other = L #赋值小技巧
```



### 增

```python
list1+list2  #合并列表

list1.extend(list2)  #合并列表
list.append()  #添加元素
list.insert(index, ) #指定位置添加元素


### 追加.append()
name_list = ["eirc","morra","tommy"]
name_list.append('456')     #直接操作对象，无返回值
print(name_list)

OUTPUT:
['eirc', 'morra', 'tommy', '456']


### 扩展.extend()
name_list = ["eirc","morra","tommy"]
name_list2 = ["bom"]
name_list.extend(name_list2)       #直接操作对象，无返回值
print(name_list)

OUTPUT:
['eirc', 'morra', 'tommy', 'bom']


### 插入.insert()
name_list = ["eirc","morra","tommy"]
name_list.insert(1,'hello')        #直接操作对象，无返回值
print(name_list)

OUTPUT:
['eirc', 'hello', 'morra', 'tommy']
```





### 删

```python
### pop()：删除尾部元素
# 返回最后一个元素，并删除
name_list = ["eirc","morra","tommy"]
temp = name_list.pop()        #删除尾部元素，并把所删除的元素存到一个新值里
print(name_list)
print(temp)

OUTPUT:
['eirc', 'morra']
tommy


### remove()：删除指定元素，只能有一个参数（匹配从左到右的第一个），不能加index
# 删除第一个匹配的值，没有则抛出异常
name_list = ["eirc","morra","tommy"]
name_list.remove("morra")
print(name_list)

OUTPUT:
['eirc', 'tommy']


### del:删除指定元素，可以使用索引和切片
name_list = ["eirc","morra","tommy","bom"]
del name_list[0]
print(name_list)

del name_list[1:2]      
print(name_list)
OUTPUT：
['morra', 'tommy', 'bom']
['morra', 'bom']


### clear()
```







## 操作3|遍历 排序 统计

### 遍历

- 判断一个数据类型是否可遍历/迭代

```python
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



```python
### for ... in range()  #常用
for i in range(len(lst)):
    print(lst[i])	#通过索引遍历
    
### 迭代器
for item in lst:	#lst本身就是迭代器
    print(item)
    
###for ... in enumerate()  #枚举
for index, item in enumerate(lst):
    print(index, item)
    
    
for ... in zip() #多个列表合并
```



### 排序

```python
.sort(key=)
.reverse()
```





### 统计

```python
len()
.count()  #某个元素的个数

max()
min()
sum()
```





## 补充|数据类型转换

```python
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



## 补充|列表操作符

| 操作符 | 功能作用                             |
| :----- | :----------------------------------- |
| +      | 连接两个列表                         |
| *      | 重复列表内容                         |
| in     | 成员操作符，判断某个数据是否在列表中 |
| not in | 成员操作符，判断某个数据是否在列表中 |

```python
>>> lst1 = [1, 2, 3]
>>> lst2 = [4, 5, 6]
>>> lst1 + lst2
[1, 2, 3, 4, 5, 6]
>>> lst1*3
[1, 2, 3, 1, 2, 3, 1, 2, 3]
>>> 3 in lst1
True
>>> 4 not in lst2
False
```





# #源码

```python
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

    __hash__ = None	#不可哈希
```





# #参考文献

[cnblogs | Python基本数据类型之list](https://www.cnblogs.com/whatisfantasy/p/5956754.html)

[Link: 酷python](http://www.coolpython.net/python_primary/data_type/list_index.html)