# `__slots__`|限制类的属性

假如你的python程序需要创建大量对象，这个数量级达到了百万，那么你可以通过使用__slots__来节省内存

```python
class Student(object):
    __slots__ = ('name', 'age') # 用tuple定义允许绑定的属性名称
```



## 应用|节省内存

一个普通类

对于普通定义的类，每个实例都会创建一个字典来==保存实例属性==，也就是__dict__

为了实现O(1)的查找效率，字典的实现额外的使用一些内存空间，即便是一个空字典也要占用200多字节的空间

如果你创建几百万个Stu实例，内存的消耗就是你不得不考虑的事情，下面的代码执行会占用176M的内存空间

```python
import os
import psutil

class Stu:
    def __init__(self, name, age, score):
        self.name = name
        self.age = age
        self.score = score

lst = []
for i in range(1000000):
    lst.append(Stu('小明', 18, 600))

process = psutil.Process(os.getpid())
print('Used Memory:',process.memory_info().rss / 1024 / 1024,'MB')
```

使用了`__slots__`后，就不再为每个实例创建用于保存属性的字典，由于==属性已经固定，不允许增加==，因此python在内部采取一种更加紧凑的内部表示，这和列表和元组有些相似。

```python
import os
import psutil

class Stu:
    __slots__ = ['name', 'age', 'score']
    def __init__(self, name, age, score):
        self.name = name
        self.age = age
        self.score = score

lst = []
for i in range(1000000):
    lst.append(Stu('小明', 18, 600))

process = psutil.Process(os.getpid())
print('Used Memory:',process.memory_info().rss / 1024 / 1024,'MB')	#使用内存为76M
```







## 绑定实例的属性+方法

当我们定义了一个class，创建了一个class的实例后，我们可以给该实例绑定任何属性和方法，这就是动态语言的灵活性

```python
class Student(object):
    pass


### 给实例绑定一个属性
>>> s = Student()
>>> s.name = 'Michael' # 动态给实例绑定一个属性
>>> print(s.name)
Michael


### 给实例绑定一个方法
>>> def set_age(self, age): # 定义一个函数作为实例方法
...     self.age = age
...
>>> from types import MethodType
>>> s.set_age = MethodType(set_age, s) # 给实例绑定一个方法
>>> s.set_age(25) # 调用实例方法
>>> s.age # 测试结果
25
```

但是，给一个实例绑定的方法，对另一个实例是不起作用的：

```python
>>> s2 = Student() # 创建新的实例
>>> s2.set_age(25) # 尝试调用方法
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Student' object has no attribute 'set_age'
```





为了给所有实例都绑定方法，可以给class绑定方法：

```python
>>> def set_score(self, score):
...     self.score = score
...
>>> Student.set_score = set_score


### 所有实例均可调用
>>> s.set_score(100)
>>> s.score
100
>>> s2.set_score(99)
>>> s2.score
99
```

通常情况下，上面的`set_score`方法可以直接定义在class中，但动态绑定允许我们在程序运行的过程中动态给class加上功能，这在静态语言中很难实现。



## 使用\__slots__

想要限制实例的属性怎么办？比如，只允许对Student实例添加`name`和`age`属性。

为了达到限制的目的，Python允许在定义class的时候，定义一个特殊的`__slots__`变量，来限制该class实例能添加的属性：

```python
class Student(object):
    __slots__ = ('name', 'age') # 用tuple定义允许绑定的属性名称
    
    
>>> s = Student() # 创建新的实例
>>> s.name = 'Michael' # 绑定属性'name'
>>> s.age = 25 # 绑定属性'age'
>>> s.score = 99 # 绑定属性'score'	=> 不在允许绑定的属性之内
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Student' object has no attribute 'score'
```

由于`'score'`没有被放到`__slots__`中，所以不能绑定`score`属性，试图绑定`score`将得到`AttributeError`的错误



使用`__slots__`要注意，`__slots__`定义的属性==仅对当前类实例起作用==，对继承的子类是不起作用的：

```python
>>> class GraduateStudent(Student):
...     pass
...
>>> g = GraduateStudent()	#子类没有绑定属性的限制
>>> g.score = 9999
```

除非在子类中也定义`__slots__`，这样，子类实例允许定义的属性就是==自身的`__slots__`加上父类的`__slots__`==





# `@property`装饰器|限制属性的范围

## 背景

绑定属性时，如果我们直接把属性暴露出去，虽然写起来很简单，但是，没办法检查参数，导致可以把成绩随便改：

```python
s = Student()
s.score = 9999


### 人为限制：get与set方法
class Student(object):

    def get_score(self):
         return self._score

    def set_score(self, value):
        if not isinstance(value, int):
            raise ValueError('score must be an integer!')
        if value < 0 or value > 100:
            raise ValueError('score must between 0 ~ 100!')
        self._score = value
```

现在，对任意的Student实例进行操作，就不能随心所欲地设置score了：

```python
>>> s = Student()
>>> s.set_score(60) # ok!
>>> s.get_score()
60
>>> s.set_score(9999)
Traceback (most recent call last):
  ...
ValueError: score must between 0 ~ 100!
```

但是，上面的调用方法又略显复杂，没有直接用属性这么直接简单。

有没有==既能检查参数==，又可以用类似属性这样简单的方式来访问类的变量呢？



## `@property`装饰器

可以给函数动态加上功能

- 对于类的方法，装饰器一样起作用

Python内置的`@property`装饰器就是负责把一个方法变成属性调用的：

```python
class Student(object):

    @property
    def score(self):
        return self._score

    @score.setter
    def score(self, value):
        if not isinstance(value, int):
            raise ValueError('score must be an integer!')	#显示的提示
        if value < 0 or value > 100:
            raise ValueError('score must between 0 ~ 100!')
        self._score = value
```

`@property`的实现比较复杂，我们先考察如何使用。

- 把一个getter方法变成属性，只需要加上`@property`就可以了：此时，`@property`本身又创建了另一个装饰器`@score.setter`，负责把一个setter方法变成属性赋值

```python
>>> s = Student()
>>> s.score = 60 # OK，实际转化为s.set_score(60)
>>> s.score # OK，实际转化为s.get_score()
60
>>> s.score = 9999
Traceback (most recent call last):
  ...
ValueError: score must between 0 ~ 100!
```

注意到这个神奇的`@property`，我们在对实例属性操作的时候，就知道该属性很可能不是直接暴露的，而是通过getter和setter方法来实现的。



还可以定义只读属性，只定义getter方法，不定义setter方法就是一个==只读属性==：

```python
class Student(object):

    @property	#get birth
    def birth(self):
        return self._birth

    @birth.setter	#set birth
    def birth(self, value):
        self._birth = value

    @property	#只有get age
    def age(self):
        return 2015 - self._birth
```

上面的`birth`是可读写属性，而`age`就是一个*只读*属性，因为`age`可以根据`birth`和当前时间计算出来。





## 示例|符合封装特性的类定义

```python
class Book:
    def __init__(self, name, price):
        self._name = name
        self._price = price

    def get_name(self):
        return self._name

    def set_name(self, name):
        self._name = name

    def get_price(self):
        return self._price

    def set_price(self, price):
        self._price = price

book = Book('python进阶', 58.5)
print(book.get_name())
print(book.get_price())

book.set_price(50.0)
print(book.get_price())
```

上面这种代码，在C++中非常的常见，这种定义类的初衷概括为两点：

1. 确保==属性是私有的==，不可随意修改
2. 提供一个获取属性的getter方法和一个修改属性的setter方法

set_price方法里可以判断传入的price参数的值是否大于0，小于等于0的数肯定是有问题的。如果直接将属性暴露出来，你无法约束使用者合理的使用这个类。



我最早转型python时，就喜欢这样定义类，但逐渐发现，python提供的property可以让我们以一种更加优雅的方式定义类

```python
class Book:
    def __init__(self, name, price):
        self._name = name
        self._price = price

    @property
    def name(self):
        return self._name

    @name.setter
    def name(self, name):
        self._name = name

    @property
    def price(self):
        return self._price

    @price.setter
    def price(self, price):
        if price > 0:
            self._price = price
        else:
            raise 'price must bigger than zero'

book = Book('python进阶', 58.5)
print(book.name)
print(book.price)

book.price = 50.0
print(book.price)
```

使用@property装饰器修饰price方法后

- 你就可以像使用属性一样使用price

- 如果你希望可以对price进行赋值，那么需要用@price.setter装饰器再装饰一个方法，该方法完成对_price属性的赋值操作



我们以为自己直接操作了对象的属性，但其实我们是在==使用类的方法==，而且关键的是省去了调用方法时的那对小括号



## 应用|利用property重构代码

```python
class Rectangle:
    def __init__(self, length, width):
        self.length = length
        self.width = width
        self.area = length * width

rectangle = Rectangle(5, 2)

print(rectangle.length)
print(rectangle.width)
print(rectangle.area)
```

上面这段代码定义了一个Rectangle类，创建对象时初始化参数为length和width

一个严重的问题，area属性被暴露出来了，这导致你可以使用下面的语句对这个属性进行修改：

```python
rectangle.area = 20
```

长方形的面积被修改为20，但面积本应该由长和宽来决定，不应该被随意修改，为了避免面积被修改，一个可能的解决办法是修改类的定义

```python
class Rectangle:
    def __init__(self, length, width):
        self.length = length
        self.width = width

    def get_area(self):
        return self.length * self.width
```

这样修改可能会带来一些麻烦，如果系统里**已经有很多处代码使用 rectangle.area 这种方式获取面积**，那么你需要将所有这种代码都修改为rectangle.get_area()，这绝对是一个糟糕的修改过程。



面对这种情况，你可以使用@property修饰符：一个类里定义的方法一旦被@property修饰，你可以**像使用属性一样去使用这个方法**

```python
class Rectangle:
    def __init__(self, length, width):
        self.length = length
        self.width = width

    @property
    def area(self):	#area 虽然是一个方法，但是你可以把它当成属性来使用
        return self.length * self.width

rectangle = Rectangle(5, 2)

print(rectangle.length)
print(rectangle.width)
print(rectangle.area)
```

调用area方法时不需要写后面的小括号，而且不必担心有人对其进行赋值操作，rectangle.area = 20 这种代码一定会报错的

==用@property重构了属性area，之前使用的rectangle.area代码依然可以使用，不需要进行任何修改==，这就是property的强大之处，兵不血刃的重构了类的定义却不影响外部的使用。





## #小结

`@property`广泛应用在类的定义中，可以让调用者写出简短的代码，==同时保证对参数进行必要的检查==。

这样，程序运行时就减少了出错的可能性。





## #练习题

```python
### 利用@property给一个Screen对象加上width和height属性，以及一个只读属性resolution

class Screen(object):

    @property
    def width(self):
        return self._width  #默认私有

    @width.setter
    def width(self, value):
        self._width = value

    ### height
    @property
    def height(self):
        return self._height  #默认私有

    @height.setter
    def height(self, value):
        self._height = value

    ### resolution
    @property
    def resolution(self):
        return self._width * self._height #默认私有


# 测试代码:
s = Screen()
s.width = 1024
s.height = 768
print('resolution =', s.resolution)
if s.resolution == 786432:
    print('测试通过!')
else:
    print('测试失败!')
```

注意：内部定义变量带了\_，但是外部访问不需要下划线















# `__len__()` => len()



# `__str__()`|print(obj)        `__repr__()`|obj

```python
### 先定义一个Student类，打印一个实例
>>> class Student(object):
...     def __init__(self, name):
...         self.name = name
...
>>> print(Student('Michael'))
<__main__.Student object at 0x109afb190>	#打印出一堆<__main__.Student object at 0x109afb190>，不好看

# 等价于：	
>>> s = Student('Michael')	
>>> s	#直接显示变量调用的不是__str__()，而是__repr__()
<__main__.Student object at 0x109afb310>

### 定义好__str__()方法
>>> class Student(object):
...     def __init__(self, name):
...         self.name = name
...     def __str__(self):	#打印出来的实例，不但好看，而且容易看出实例内部重要的数据
...         return 'Student object (name: %s)' % self.name
...
>>> print(Student('Michael'))
Student object (name: Michael)



```

`__str__()`返回用户看到的字符串

而`__repr__()`返回程序开发者看到的字符串，也就是说，`__repr__()`是为调试服务的。



解决办法是再定义一个`__repr__()`。但是通常`__str__()`和`__repr__()`代码都是一样的

```python
class Student(object):
    def __init__(self, name):
        self.name = name
    def __str__(self):
        return 'Student object (name=%s)' % self.name
    __repr__ = __str__
```





# `__iter__()`    `__next__()`生成迭代对象

一个类想被用于`for ... in`循环，类似list或tuple那样 => 必须实现一个__iter__()方法，该方法返回一个迭代对象

然后，Python的for循环就会不断调用该迭代对象的`__next__()`方法拿到循环的下一个值，直到遇到`StopIteration`错误时退出循环

```python
### 以斐波那契数列为例
class Fib(object):
    def __init__(self):
        self.a, self.b = 0, 1 # 初始化两个计数器a，b

    def __iter__(self):	#返回迭代对象
        return self # 实例本身就是迭代对象，故返回自己

    def __next__(self):
        self.a, self.b = self.b, self.a + self.b # 计算下一个值
        if self.a > 100000: # 退出循环的条件
            raise StopIteration()
        return self.a # 返回下一个值
    
    
    
# 把Fib实例作用于for循环
>>> for n in Fib():
...     print(n)
...
1
1
2
3
5
...
46368
75025
```





# `__getitem__()`    ` __setitem__()`     `__delitem()__` 模拟基本数据类型

Fib实例虽然能作用于for循环，看起来和list有点像，但是，把它当成list来使用还是不行，比如，取第5个元素：

```python
>>> Fib()[5]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'Fib' object does not support indexing
```



要表现得像list那样按照下标取出元素，需要实现`__getitem__()`方法：

```python
class Fib(object):
    def __getitem__(self, n):
        a, b = 1, 1
        for x in range(n):
            a, b = b, a + b	#实现了斐波那契数列的
        return a
    
    
# 调用
>>> f = Fib()
>>> f[0]
1
>>> f[1]
1
>>> f[2]
2
>>> f[3]
3
>>> f[10]
89
>>> f[100]
573147844013817084101
```



注意：list有个神奇的切片方法：

对于Fib却报错。原因是`__getitem__()`传入的参数可能是一个int，也可能是一个切片对象`slice`，所以要做判断：

```python
class Fib(object):
    def __getitem__(self, n):
        if isinstance(n, int): # n是索引
            a, b = 1, 1
            for x in range(n):
                a, b = b, a + b
            return a
        if isinstance(n, slice): # n是切片
            start = n.start	#切片的属性.start .stop
            stop = n.stop
            if start is None:
                start = 0
            a, b = 1, 1
            L = []
            for x in range(stop):
                if x >= start:
                    L.append(a)
                a, b = b, a + b
            return L
        
        
###
>>> f = Fib()
>>> f[0:5]
[1, 1, 2, 3, 5]
>>> f[:10]
[1, 1, 2, 3, 5, 8, 13, 21, 34, 55]
```

但是没有对step参数作处理：

```python
>>> f[:10:2]
[1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89]
```

也没有对负数作处理，所以，要正确实现一个`__getitem__()`还是有很多工作要做的。



此外，如果把对象看成`dict`，`__getitem__()`的参数也可能是一个可以作key的object，例如`str`。



与之对应的是`__setitem__()`方法，把对象视作list或dict来对集合赋值。

最后，还有一个`__delitem__()`方法，用于删除某个元素。

总之，通过上面的方法，我们自己定义的类表现得和Python自带的list、tuple、dict没什么区别，这完全归功于动态语言的“鸭子类型”，不需要强制继承某个接口。





# `__getattr__()` 动态实现属性、方法

正常情况下，当我们调用类的方法或属性时，如果不存在，就会报错。比如定义`Student`类：

```python
class Student(object):
    
    def __init__(self):
        self.name = 'Michael'
        
# 调用name属性，没问题，但是，调用不存在的score属性，就有问题了：
>>> s = Student()
>>> print(s.name)
Michael
>>> print(s.score)
Traceback (most recent call last):
  ...
AttributeError: 'Student' object has no attribute 'score'
```

要避免这个错误，除了可以加上一个`score`属性外，Python还有另一个机制，那就是写一个`__getattr__()`方法，动态返回一个属性

```python
class Student(object):

    def __init__(self):
        self.name = 'Michael'

    def __getattr__(self, attr):
        '''
        当调用不存在的属性时，比如score，Python解释器会试图调用__getattr__(self, 'score')来尝试获得属性
        '''
        if attr=='score':
            return 99
        
        
# 调用
>>> s = Student()
>>> s.name
'Michael'
>>> s.score
99



### 变式：返回函数
class Student(object):

    def __getattr__(self, attr):
        if attr=='age':
            return lambda: 25
# 调用      
>>> s.age()
25
```

注意，只有在没有找到属性的情况下，才调用`__getattr__`，已有的属性，比如`name`，不会在`__getattr__`中查找。



注意到任意调用如`s.abc`都会返回`None`，这是因为我们定义的`__getattr__`默认返回就是`None`。要让class只响应特定的几个属性，我们就要==按照约定，抛出`AttributeError`的错误==：

```python
class Student(object):

    def __getattr__(self, attr):
        if attr=='age':
            return lambda: 25
        raise AttributeError('\'Student\' object has no attribute \'%s\'' % attr)
```

实际上可以把一个类的所有属性和方法调用全部动态化处理了 => 可以针对完全动态的情况作调用



### #实例

现在很多网站都搞REST API，比如新浪微博、豆瓣啥的，调用API的URL类似：

- http://api.server/user/friends
- http://api.server/user/timeline/list

如果要写SDK，给每个URL对应的API都写一个方法，那得累死，而且，API一旦改动，SDK也要改。

利用完全动态的`__getattr__`，我们可以写出一个链式调用：

```python
class Chain(object):

    def __init__(self, path=''):
        self._path = path

    def __getattr__(self, path):
        return Chain('%s/%s' % (self._path, path))

    def __str__(self):
        return self._path

    __repr__ = __str__
    
    
#
>>> Chain().status.user.timeline.list
'/status/user/timeline/list'
```

这样，无论API怎么变，SDK都可以根据URL实现完全动态的调用，而且，不随API的增加而改变！

还有些REST API会把参数放到URL中，比如GitHub的API：

```python
GET /users/:user/repos
    
### 调用时，需要把:user替换为实际用户名，就可以非常方便地调用API了
Chain().users('michael').repos
```



