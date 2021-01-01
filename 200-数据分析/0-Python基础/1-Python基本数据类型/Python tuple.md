# Python 基本数据类型 | tuple

## 一 创建元组

- 元组和列表几乎一样
- 元组的元素不可修改，但是**元组元素的元素**是可以修改的
- tuple(iterable)，可以存放**所有可迭代**的数据类型

```
ages = (11, 22, 33, 44, 55)
ages = tuple((11, 22, 33, 44, 55))
```





## 二 基本操作

### 索引

### 切片

### 循环

### 长度

### 嵌套

```
## 索引
name_tuple = (1,2,3)
print(name_tuple[0])


## 切片
name_tuple = (1,2,3)
print(name_tuple[0:2])


## 循环
for i in name_tuple:
    print(i)


## 长度
name_tuple = (1,2,3)
print(name_tuple[len(name_tuple)-1])


## 嵌套
t = (11,22,["alex",{"k1":"v1"}])
temp = t[2][1]['k1']
print(temp)
```





## 三 源码

```
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

## 参考文献

[[转摘\]cnblogs | python基本数据类型之tuple](https://www.cnblogs.com/whatisfantasy/p/5956759.html)