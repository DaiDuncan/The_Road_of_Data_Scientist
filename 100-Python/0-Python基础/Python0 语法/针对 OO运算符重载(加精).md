常见的运算符重载方法说明

| 方法名                                          | 重载说明           | 运算符==调用方式==                                           |
| ----------------------------------------------- | ------------------ | ------------------------------------------------------------ |
| ✅`__new__`                                      | 创建               | 在`__init__`初始化之前创建对象                               |
| ✅`__init__`                                     | 构造函数           | 对象创建: X = Class(args)                                    |
| ✅`__del__`                                      | 析构函数           | X对象收回                                                    |
| `__add__` `__sub__`                             | 加减运算           | X+Y， X+=Y/X-Y， X-=Y                                        |
| `__or__`                                        | 运算符\|           | X\|Y, X\|=Y                                                  |
| ✅`__repr__`<br>`__str__`                        | 打印／转换         | print(X)、repr(X)／str(X)                                    |
| ✅`__call__`                                     | 函数调用           | X(*args, **kwargs)                                           |
| ✅`__get__, __set__,__delete__`                  | 描述符属性         | X.attr, X.attr=value, del X.attr                             |
| ✅`__getattr__`                                  | 属性引用           | X.undefined                                                  |
| ✅`__setattr__`                                  | 属性赋值           | X.any=value                                                  |
| ✅`__delattr__`                                  | 属性删除           | del X.any                                                    |
| `__getattribute__`                              | 属性获取           | X.any                                                        |
| `__getitem__`                                   | ==索引运算==       | X[key]，X[i:j]                                               |
| `__setitem__`                                   | 索引赋值           | X[key]，X[i:j]=sequence                                      |
| `__delitem__`                                   | 索引和分片删除     | del X[key]，del X[i:j]                                       |
| `__len__`                                       | 长度               | len(X)                                                       |
| `__bool__`                                      | 布尔测试           | bool(X)                                                      |
| ✅`__lt__, __gt__,__le__, __ge__,__eq__, __ne__` | ==特定的比较==     | 依次为X<Y，X>Y，X<=Y，X>=Y，X==Y，X!=Y注释：（lt: less than, gt: greater than,le: less equal, ge: greater equal,eq: equal, ne: not equal） |
| `__radd__`                                      | 右侧加法           | other+X                                                      |
| `__iadd__`                                      | 实地（增强的）加法 | X+=Y(or else `__add__`)                                      |
| ✅`__iter__`<br>`__next__`                       | ==迭代==           | I=iter(X), next()                                            |
| `__contains__`                                  | 成员关系测试       | item in X(X为任何可迭代对象)                                 |
| `__index__`                                     | 整数值             | hex(X), bin(X), oct(X)                                       |
| ✅`__enter__`<br>`__exit__`                      | 环境管理器         | with obj as var:                                             |



# 比较运算符

## 示例|美元与人民币的比较

```python
### 定义两个类
class Dollar:
    def __init__(self, value):
        self.value = value
        self.rate = 1       # 与美元的汇率


class Rmb:
    def __init__(self, value):
        self.value = value
        self.rate = 0.1431      # 1元人民币等于0.1431美元


d = Dollar(0.95)
r = Rmb(6.7)


### 写代码来判断d是否比r大
d = Dollar(0.95)
r = Rmb(6.7)

def ge(dollar, rmb):
    return dollar.value > rmb.value*rmb.rate

print(ge(d, r))
```

不可以直接用表达式 d > r来进行判断，因为这样比较的是两个对象的内存地址

编写一个函数来判断两个货币的大小虽然在技术上可行，但从工程上看，着实是一种丑陋的办法。



通过重载__gt__方法来实现两个对象的比较，为了完成这个事情，还需要做一些额外的操作，让这两种货币拥有共同的父类

```python
class Money:
    def __init__(self, value):
        self.value = value

    def __gt__(self, other):	#两个实例self和other的比较
        return self.value*self.rate > other.value*other.rate

    
### 基于共同的父类
class Dollar(Money):
    def __init__(self, value):
        super().__init__(value)
        self.rate = 1       # 与美元的汇率


class Rmb(Money):
    def __init__(self, value):
        super().__init__(value)
        self.rate = 0.1431      # 1元人民币等于0.1431美元


d = Dollar(0.95)
r = Rmb(6.7)

print(d > r)	
```

只要父类中重载了`__gt__`方法，就==可以直接用>==，来比较子类的实例





# #参考文献

[Link: 酷python博客](http://www.coolpython.net/python_senior/senior_oop/overloading.html)