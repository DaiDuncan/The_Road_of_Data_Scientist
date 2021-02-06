# Python 基本数据类型 | tuple

## 场景导入

元素的==不可修改性==



## 特性

- 有序
- 不可修改：因为tuple不可变，所以==代码更安全==(代替list)(内存可以明确知道需要分配多少空间给元组)
- 效率更高/访问速度更快
- 元组的元素不允许删除，但是我们可以删除整个元组
- 元组中的元素可以是不同类型，各元素存在先后关系，可通过索引访问元组中的数据     

一般用于表达固定数据项，函数多返回值等情况



### 理解不可变性

```python
>>> t = ('a', 'b', ['A', 'B'])
>>> t[2][0] = 'X'
>>> t[2][1] = 'Y'
>>> t
('a', 'b', ['X', 'Y'])
```

初始化的tuple：

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210115161141.png" alt="image-20210115161141339" style="zoom:80%;" />

“修改”后的tuple：

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210115161155.png" alt="image-20210115161155645" style="zoom:80%;" />

表面上看，tuple的元素确实变了，但其实变的不是tuple的元素，而是list的元素。tuple一开始指向的list并没有改成别的list，所以，tuple所谓的“不变”是说，tuple的每个元素，指向永远不变。即指向`'a'`，就不能改成指向`'b'`，指向一个list，就不能改成指向其他对象，但指向的这个list本身是可变的！





## 意义

元组在特定场景下可以实现列表无法实现的效果

### 1 函数返回多个结果时，元组可以作为返回值

```python
def func(x, y):
    return x, y, x+y

res = func(2, 3)
print(res)	#结果：(2, 3, 5)
```

列表是可变对象，这意味着函数的返回结果是可修改的，那么函数在使用时就容易出现修改函数返回值的情况

某些情况下，我们不希望函数的返回值被他人修改，元组恰好满足了我们的要求



### 2 元组作为函数的可变参数

```python
def func(*args):
    print(args, type(args))

func(3, 4, 5)
```

如果args被设计成列表，由于列表可以被修改，保不齐某个程序员在实现函数的时候修改了args，传入的参数被修改了，或是增加，或是减少，这样就会引发不可预知的错误

现在，python将其设计成元组，元组无法修改，你在函数内部无法修改传入的参数，这就保证了函数入参的安全性。



### 3 元组可以作为字典的key，可以存储到集合中

想要成为字典的key，或是存储到集合中，必须满足可hash这个条件

所有的可变对象，诸如列表，集合，字典都不能做key，但元组可以，在一些特定情境下，用元组做key，非常实用

```python
### 问题：从两个列表里各取出一个数，他们的和如果为10，则记录下来
lst1 = [3, 5, 6, 1, 2, 4, 7]
lst2 = [6, 5, 4, 7, 3, 8]



### 使用嵌套循环，就可以轻易的完成这个题目，但是这中间过程要去除掉重复的组合，正好可以利用元组，算法实现如下 => 利用集合的不重复性来达到去重的目的
res_set = set()
for i in lst1:
    for j in lst2:
        if i + j == 10:
            if i > j:
                res_set.add((j, i))
            else:
                res_set.add((i, j))

print(res_set)
```





# 操作1|创建

- 元组和列表几乎一样
- 元组的元素不可修改，但是元组**元素的元素**是可以修改的
- tuple(iterable)，可以存放**所有可迭代**的数据类型

```python
ages = (11, 22, 33, 44, 55)
ages = tuple((11, 22, 33, 44, 55))

tuple=(5,)  #只有1个元素：否则和数学中的括号产生歧义
tuple=(5)  #表示int
tup = tuple([])


### 嵌套
t = (11,22,["alex",{"k1":"v1"}])
temp = t[2][1]['k1']
print(temp)
```





# 操作2|查 选/索引 改 增 删

```python
### 选/索引(含切片)
name_tuple = (1,2,3)
print(name_tuple[0])

# 切片
name_tuple = (1,2,3)
print(name_tuple[0:2])



### 删除
del()
```





# 操作3|遍历 排序 统计

```python
### 遍历
for i in name_tuple:
    print(i)


### 统计
len()
max()
min()

name_tuple = (1,2,3)
print(name_tuple[len(name_tuple)-1])
```





# 操作|补充：数据类型转换

更改：先转化为列表



# #源码

```python
class tuple(object):
    """
    tuple() -> empty tuple
    tuple(iterable) -> tuple initialized from iterable's items
    
    If the argument is a tuple, the return value is the same object.
    """
    def count(self, value): 
        """计算元素出现的个数"""
        """ T.count(value) -> integer -- return number of occurrences of value """
        return 0

    def index(self, value, start=None, stop=None): 
        """获取索引"""
        """
        T.index(value, [start, [stop]]) -> integer -- return first index of value.
        Raises ValueError if the value is not present.
        """
        return 0

    def __add__(self, *args, **kwargs): 
        """ Return self+value. """
        pass

    def __contains__(self, *args, **kwargs): 
        """ Return key in self. """
        pass

    def __eq__(self, *args, **kwargs): 
        """ Return self==value. """
        pass

    def __getattribute__(self, *args, **kwargs): 
        """ Return getattr(self, name). """
        pass

    def __getitem__(self, *args, **kwargs): 
        """ Return self[key]. """
        pass

    def __getnewargs__(self, *args, **kwargs): 
        pass

    def __ge__(self, *args, **kwargs): 
        """ Return self>=value. """
        pass

    def __gt__(self, *args, **kwargs): 
        """ Return self>value. """
        pass

    def __hash__(self, *args, **kwargs): 
        """ Return hash(self). """
        pass

    def __init__(self, seq=()): # known special case of tuple.__init__
        """
        tuple() -> empty tuple
        tuple(iterable) -> tuple initialized from iterable's items
        
        If the argument is a tuple, the return value is the same object.
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

    def __rmul__(self, *args, **kwargs): 
        """ Return self*value. """
        pass
```



# #参考文献

[[转摘\]cnblogs | python基本数据类型之tuple](https://www.cnblogs.com/whatisfantasy/p/5956759.html)

[Link: 教程|廖雪峰](https://www.liaoxuefeng.com/wiki/1016959663602400/1017092876846880)

[Link: 酷Python](http://www.coolpython.net/python_primary/data_type/tuple.html)