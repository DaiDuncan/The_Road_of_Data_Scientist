# Python 基本数据类型 | dict

## 一 创建字典

```
d = {
    "name": "morra",        #字典是无序的
    "age": 99,
    "gender": 'm'
}

a = dict()
b = dict(k1=123,k2="morra")
```





## 二 基本操作

### 键值对

- keys()：获取所有键
- values()：获取所有值
- items()：获取所有键、值对

```
d = {
    "name": "morra",
    "age": 99,
    "gender": 'm'
}

print(d.keys())
print(type(d.keys()))

print(d.values())
print(type(d.values()))

print(d.items())
print(type(d.items()))

OUTPUT:
dict_keys(['gender', 'age', 'name'])
<class 'dict_keys'>

dict_values(['m', 99, 'morra'])
<class 'dict_values'>

dict_items([('gender', 'm'), ('age', 99), ('name', 'morra')])
<class 'dict_items'>
```

### 循环

```
for a in d:         #字典在for中默认只能迭代输出key
    print(a)        

OUTPUT:
name
gender
age

#------------------

for i in d.keys():      #迭代输出key
    print(i)

OUTPUT:
name
gender
age

#------------------

for j in d.values():    #迭代输出value
    print(j)

OUTPUT:
morra
m
99

#------------------

for k,v in d.items():   #迭代输出键值对
    print(k,v)

OUTPUT:
name morra
gender m
age 99
```

###  

### 查：索引

### 查：取值

实际上，在使用字典取值的时候使用最多的方法是get()而不是dict[key]，因为如果当key值不存在时，使用get()可以调用一个默认值，但是在dict[key]中则会报错。

```
## dict[key]
d = {
    "name": "morra",
    "age": 99,
    "gender": 'm'
}
print(d["name"])


## get()
d = {
    "name": "morra",
    "age": 99,
    "gender": 'm'
}
val1 = d.get('age')
val2 = d.get('get','123')    #为key设置默认值
print(val1,val2)

OUTPUT:
99 123
```

### 改：批量更新

update()方法只会更新与原来不同的键值对：

- 如果key一样，value不一样，则会把原来的**value覆盖掉**。
- 如果key不一样，则会把两个键值对都添加进来。
- 如果有一个键值对的key、value和原来一样，则**不会更新**。

```
d = {
    "name": "morra",
    "age": 99,
    "gender": 'm'
}

d1 = {
    "name": "morra",
    "age1": 991,
    "gender1": 'm1'
}

d.update(d1)
for k,v in d.items():
    print(k,v)

OUTPUT:
name morra
age1 991
gender1 m1
gender m
age 99
```

### 删：删除

- pop(k)：**根据key值**移除指定的键值对
- popitem()：从当前尾部移除键值对，由于**字典是无序的**，因此被移除的键值对也是随机的。
- del：删除指定索引的键值对，和pop()用法一样

```
## dict.pop(key)
d = {
    "name": "morra",
    "age": 99,
    "gender": 'm'
}

d.pop("name")
print(d)

OUTPUT：
{'gender': 'm', 'age': 99}

==========================================
## dict.popitem()
d = {
    "name": "morra",
    "age": 99,
    "gender": 'm'
}

d.popitem()        
print(d)

==========================================
## del dict[key]
d = {
    "name": "morra",
    "age": 99,
    "gender": 'm'
}

del d["name"]
print(d)

OUTPUT：
{'age': 99, 'gender': 'm'}
```

### 判断

用in就可以了,但是在python2.7中可以使用has_key()

### 补充

- fromkeys()：

由于list在迭代的时候没有只有一个值，所以不能直接使用dict(li)把列表转换为字典。在这里我们需要使用字典里的fromkeys()方法。

fromkeys() ：函数用于创建一个新字典，列表中的元素当做key，并为每个key设置一个固定值（value是可选的，如果没有默认为None）。

```
seq = ("name","key")
a = dict.fromkeys(seq)
b = dict.fromkeys(seq,1)

print(a)
print(b)

OUTPUT：
{'key': None, 'name': None}
{'key': 1, 'name': 1}
```

- @staticmethod

一般来说，要使用某个类的方法，需要先实例化一个对象再调用方法，或直接用类调用。而使用staticmethod装饰器之后，就可以不需要实例化，直接类名.方法名()来调用。

这有利于组织代码，把某些应该属于某个类的函数给放到那个类里去，同时有利于命名空间的整洁。

```
@staticmethod 
def fromkeys(*args, **kwargs):
    """ 
    Returns a new dict with keys from iterable and values equal to value. 
    """
    pass
```





## 三 源码

```
class dict(object):
    """
    dict() -> new empty dictionary
    dict(mapping) -> new dictionary initialized from a mapping object's
        (key, value) pairs
    dict(iterable) -> new dictionary initialized as if via:
        d = {}
        for k, v in iterable:
            d[k] = v
    dict(**kwargs) -> new dictionary initialized with the name=value pairs
        in the keyword argument list.  For example:  dict(one=1, two=2)
    """

    def clear(self): 
        """清空字典"""
        """ D.clear() -> None.  Remove all items from D. """
        pass

    def copy(self): 
        """ D.copy() -> a shallow copy of D """
        pass

    @staticmethod # known case
    def fromkeys(*args, **kwargs): 
        """ Returns a new dict with keys from iterable and values equal to value. """
        pass

    def get(self, k, d=None): 
        """ D.get(k[,d]) -> D[k] if k in D, else d.  d defaults to None. """
        pass

    def items(self): 
        """ D.items() -> a set-like object providing a view on D's items """
        pass

    def keys(self): 
        """ D.keys() -> a set-like object providing a view on D's keys """
        pass

    def pop(self, k, d=None): 
        """
        D.pop(k[,d]) -> v, remove specified key and return the corresponding value.
        If key is not found, d is returned if given, otherwise KeyError is raised
        """
        pass

    def popitem(self): 
        """
        D.popitem() -> (k, v), remove and return some (key, value) pair as a
        2-tuple; but raise KeyError if D is empty.
        """
        pass

    def setdefault(self, k, d=None): 
        """ D.setdefault(k[,d]) -> D.get(k,d), also set D[k]=d if k not in D """
        pass

    def update(self, E=None, **F): # known special case of dict.update
        """
        D.update([E, ]**F) -> None.  Update D from dict/iterable E and F.
        If E is present and has a .keys() method, then does:  for k in E: D[k] = E[k]
        If E is present and lacks a .keys() method, then does:  for k, v in E: D[k] = v
        In either case, this is followed by: for k in F:  D[k] = F[k]
        """
        pass

    def values(self): 
        """ D.values() -> an object providing a view on D's values """
        pass

    def __contains__(self, *args, **kwargs): 
        """ True if D has a key k, else False. """
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

    def __init__(self, seq=None, **kwargs): # known special case of dict.__init__
        """
        dict() -> new empty dictionary
        dict(mapping) -> new dictionary initialized from a mapping object's
            (key, value) pairs
        dict(iterable) -> new dictionary initialized as if via:
            d = {}
            for k, v in iterable:
                d[k] = v
        dict(**kwargs) -> new dictionary initialized with the name=value pairs
            in the keyword argument list.  For example:  dict(one=1, two=2)
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

    def __setitem__(self, *args, **kwargs): 
        """ Set self[key] to value. """
        pass

    def __sizeof__(self): 
        """ D.__sizeof__() -> size of D in memory, in bytes """
        pass

    __hash__ = None
```



## 参考文献

[[转摘\]cnblogs | python基本数据类型dict](https://www.cnblogs.com/whatisfantasy/p/5956761.html)