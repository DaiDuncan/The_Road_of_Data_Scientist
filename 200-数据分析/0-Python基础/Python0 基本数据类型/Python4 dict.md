在其他语言中也称为map，使用键-值（key-value）存储，具有极快的查找速度

# Python 基本数据类型 | dict

## 场景导入

《新华字典》

手机通讯录

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210121114839.png" alt="image-20210121114839337" style="zoom:80%;" />



## 特性

- ==无序==：无法用数值索引

- 可变：但组成字典的键必须是不可变的数据类型(因为它的 value 的位置是根据 key 计算出来的)

- **占用大量的内存**而获得极快的查找和插入速度 => 用空间来换取时间



字典类型数据通过映射/Hash查找数据项

而list是通过遍历



- key键不可重复且必须是可hash的
  - bool
  - int
  - float
  - 字符串
  - 元组
- value值可以是任意数据且可重复

bool类型的数据只有True和False两个值，虽然他们可以做字典的key，但实践时，你最好不要这样做，会导致古怪的问题，比如下面的代码

```python
int_dict = {
    1: '1做key',
    True: 'True做key'
}

print(int_dict)	#结果：{1: 'True做key'}

print(1==True)      # 判断1是否与True相等：True
print(issubclass(bool, int))  # 判断bool类型是否为int类型的子类：True
```

1 和 True是相等的，0和False是相等的，bool类型是int类型的子类，这就是原因，在定义字典时，第二个键值对覆盖了前面的键值对，在字典中，key是不会出现重复的，==后加入的总是会覆盖前面的==。





# 操作1|创建

创建时如果同一个键被赋值两次，后一个值会被记住

```python
d = {
    "name": "morra",        #字典是无序的
    "age": 99,
    "gender": 'm'
}

a = dict()
b = dict(k1=123,k2="morra")


### int也可以作为key
d[3] = 'good'	#3是key，即{3: 'good'}
```

## fromkeys()

以指定key创建一个新的字典

```python
stu_dict = dict.fromkeys(['小明', '小刚'], 90)
print(stu_dict)	#{'小明': 90, '小刚': 90}
```

fromkeys方法接受两个参数：

- 第一个参数是序列，可以是列表，也可以是元组，方法将以这个序列里的元素做key，生成新的字典
- value由第二个参数来决定，我在代码里传入参数90，所有key所对应的value就都是90，如果不传这个参数，==默认value为None==

## setdefault()

和get有些类似，如果key不存在，则增加新的键值对，如果key已经存在，则不做任何操作

```python
score_dict = {
    '小明': 96,
    '小刚': 98,
    '小红': 94
}

score_dict.setdefault('小明', 100)    # 小明这个key已经存在，因此这行语句不产生任何影响
score_dict.setdefault('小丽', 97)     # 小丽这个key不存在，增加新的键值对，key为小丽，value为97

print(score_dict)	#结果：{'小明': 96, '小刚': 98, '小红': 94, '小丽': 97}
```



## 嵌套字典

```python
stu_dict = {
    'name': '小明',
    'age': 12,
    'score': {
        '语文': 90,
        '数学': 98
    }
}
```

# 操作2|查 选/索引 改 增 删

## 查|基于键

```python
dict['key']  #通过键来得到值

dict.get('key')  #安全的获取value的方法：找不到对象返回default，默认为None
```

实际上，在使用字典取值的时候使用最多的方法是get()而不是dict[key]

因为如果当key值不存在时，使用get()可以调用一个默认值，但是在dict[key]中则会报错。

```python
## dict[key]
d = {
    "name": "morra",
    "age": 99,
    "gender": 'm'
}
print(d["name"])


## dict.get()
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



### 判断成员|基于键

```python
in; not in  #查找，返回布尔值

dict.has_keys('key')
setdefault(k, v)   #如果键在字典中，则返回这个键的值，如果不在字典中，则向字典中插入这个键，并返回value，默认value为None
```





## 改/增

添加/修改：类似列表，==直接赋值== dict['new key']=value

update()方法只会更新与原来不同的键值对：

- 如果key一样，value不一样，则会==把原来的**value覆盖掉**==。
- 如果key不一样，则会把两个键值对都添加进来。
- 如果有一个键值对的key、value和原来一样，则**不会更新**。

```python
dict.update(dict2)  #将新的字典添加到原来的字典，若原来的字典包含了这个键则覆盖原来的值
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



由于list不只有一个值，所以不能直接使用dict(li)把列表转换为字典。(字典的key唯一)

在这里我们需要使用字典里的fromkeys()方法。

fromkeys() ：函数用于创建一个新字典，列表中的元素当做key，并为每个key设置一个固定值（value是可选的，如果没有默认为None）。

```python
fromkeys()  #从列表转化为字典：keys默认共用相同的value

seq = ("name","key")
a = dict.fromkeys(seq)
b = dict.fromkeys(seq,1)

print(a)
print(b)

OUTPUT：
{'key': None, 'name': None}
{'key': 1, 'name': 1}
```



一般来说，要使用某个类的方法，需要先实例化一个对象再调用方法，或直接用类调用。

而使用staticmethod装饰器之后，就可以不需要实例化，直接==类名.方法名()==来调用。

这有利于组织代码，把某些应该属于某个类的函数给放到那个类里去，同时有利于命名空间的整洁。

```python
@staticmethod 
def fromkeys(*args, **kwargs):
    """ 
    Returns a new dict with keys from iterable and values equal to value. 
    """
    pass
```







## 删

- pop(‘key’)：**根据key值**移除指定的键值对
- popitem()：从当前尾部移除键值对，由于**字典是无序的**，因此被移除的键值对也是随机的。
- del：删除指定索引的键值对，和pop()用法一样
- `dict.clear()`  ==删除所有==

```python
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



### 区别clear与赋值{}

```python
### clear
dic = {
    '小明': 98
}
dic.clear()
print(dic)



### 赋值{}
dic = {
    '小明': 98
}
dic = {}
print(dic)
```

使用clear方法，字典在内存中的地址没有发生变化

但是第二种方法，变量dic指向了一个新的空字典(只是修改了变量的引用而已)，原来的字典被垃圾回收机制回收了，我们可以通过输出变量的内存地址来验证

```python
dic1 = {
    '小明': 98
}
print("使用clear方法前，dic1内存地址为", id(dic1))	#4352796640
dic1.clear()
print(dic1)
print("使用clear方法后，dic1内存地址为", id(dic1))	#4352796640

print("\n\n分割线"+"*"*30 + "\n"*2)

dic2 = {
    '小明': 98
}
print("赋值空字典之前，dic1内存地址为", id(dic2))	#4729716312
dic2 = {}
print(dic2)
print("赋值空字典之后，dic1内存地址为", id(dic2))	#4729716168
```







# 操作3|遍历 排序 统计

## 遍历

`for ... in enumerate()` 枚举

`for ... in zip()` 多个列表合并



- keys()：获取所有键
- values()：获取所有值
- items()：获取所有键、值对

```python
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



```python
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



## 统计

```python
max(dict.values())  #针对int型的key
len()
```





# 操作|补充

`str()`  输出字典中可以打印的字符串标识





# #源码

```python
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



# #参考文献

[[转摘\]cnblogs | python基本数据类型dict](https://www.cnblogs.com/whatisfantasy/p/5956761.html)

[Link: 酷python](http://www.coolpython.net/python_primary/data_type/dict_index.html)