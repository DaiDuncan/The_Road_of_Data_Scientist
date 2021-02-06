在Python中，面向对象还有很多高级特性：多重继承、定制类、元类等



# 特性|数据封装

隐藏实现的细节，只对外公开我们想让他们使用的属性和方法：就好比把一些东西用一个盒子封装起来，只留一个口，内部让你看不见。



python的面向对象, 并没有严格意义上的私有属性和方法, ==私有只是一种约定==, 隐藏实现的细节，只对外公开我们想让他们使用的属性和方法，这就叫做封装

- 封装的目的在于保护类内部数据结构的完整性：避免了外部对内部数据的影响 => 提高了程序的可维护性



上面的Student类中，每个实例就拥有各自的name和score这些数据。我们可以通过函数来访问这些数据/属性

- `Student`实例本身就拥有这些数据，要访问这些数据，就没有必要从外面的函数去访问
- 可以直接在`Student`类的内部定义访问数据的函数 => 相当于把“数据”给封装起来了
- 这些封装数据的函数是和`Student`类本身是关联起来的，我们称之为==类的方法==

```python
class Student(object):

    def __init__(self, name, score):
        self.name = name
        self.score = score

    def print_score(self):
        print('%s: %s' % (self.name, self.score))
```

这些数据和逻辑被“封装”起来了，调用很容易，但却不用知道内部实现的细节。

封装的另一个好处是可以给`Student`类增加新的方法，比如`get_grade`：

```python
class Student(object):
    ...

    def get_grade(self):
        if self.score >= 90:
            return 'A'
        elif self.score >= 60:
            return 'B'
        else:
            return 'C'
```



注意：和静态语言不同，Python允许对实例变量绑定任何数据，也就是说，对于两个实例变量，虽然它们都是同一个类的不同实例，但拥有的变量名称都可能不同：

```python
>>> bart = Student('Bart Simpson', 59)
>>> lisa = Student('Lisa Simpson', 87)
>>> bart.age = 8	#自由绑定任何数据/属性
>>> bart.age
8
>>> lisa.age
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Student' object has no attribute 'age'
```



## #访问限制|内部/私有属性  \__属性

如果要让内部属性不被外部访问，可以把属性的名称前加上两个下划线`__`，在Python中，实例的变量名如果以`__`开头，就变成了一个私有变量（private），只有内部可以访问，外部不能访问

```python
class Student(object):

    def __init__(self, name, score):
        self.__name = name
        self.__score = score

    def print_score(self):
        print('%s: %s' % (self.__name, self.__score))
```

改完后，对于外部代码来说，没什么变动，但是已经无法从外部访问`实例变量.__name`和`实例变量.__score`了：

```python
>>> bart = Student('Bart Simpson', 59)
>>> bart.__name
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Student' object has no attribute '__name'
```

这样就确保了外部代码不能随意修改对象内部的状态，这样通过访问限制的保护，代码更加健壮。



但是如果外部代码要获取name和score怎么办？可以给Student类增加`get_name`和`get_score`这样的方法：

如果又要允许外部代码修改score怎么办？可以再给Student类增加`set_score`方法：

@Java

```python
### get方法
class Student(object):
    ...

    def get_name(self):
        return self.__name

    def get_score(self):
        return self.__score
    
    
### set方法 
class Student(object):
    ...

    def set_score(self, score):
        self.__score = score
```

你也许会问，原先那种直接通过`bart.score = 99`也可以修改啊，为什么要定义一个方法大费周折？因为在方法中，==可以对参数做检查，避免传入无效的参数==：

```python
class Student(object):
    ...

    def set_score(self, score):	#属性控制
        if 0 <= score <= 100:
            self.__score = score
        else:
            raise ValueError('bad score')
```



### __方法

这样的方法，类的对象无法使用，双下划线开头的属性也同样无法访问，通过这种方法，就可以将不希望外部访问的属性和方法隐藏起来

```python
class Animal:
    def __init__(self, name, age):
        self.__name = name
        self.__age = age

    def __run(self):	#类的私有方法
        print("run")

a = Animal('猫', 2)
a.__run()
print(a.__name, a.__age)
```





### \__变量__

注意：在Python中，变量名类似`__xxx__`的，也就是以双下划线开头，并且以双下划线结尾的，是特殊变量，特殊变量是可以直接访问的，不是private变量，所以，不能用`__name__`、`__score__`这样的变量名。



### \__变量  <==>  \_\_属性

双下划线开头的实例变量是不是一定不能从外部访问呢？其实也不是。

不能直接访问`__name`是因为Python解释器对外把`__name`变量改成了`_Student__name`，所以，仍然可以通过`_Student__name`来访问`__name`变量：

```python
>>> bart._Student__name
'Bart Simpson'
```

但是强烈建议你不要这么干，因为不同版本的Python解释器可能会把`__name`改成不同的变量名。



### \_变量

这样的实例变量外部是可以访问的，但是，按照约定俗成的规定，当你看到这样的变量时，意思就是，“虽然我可以被访问，但是，请把我视为私有变量，不要随意访问”。



### #小结

总的来说就是，Python本身没有任何机制阻止你干坏事，一切全靠自觉。

注意下面的这种错误写法：

```python
>>> bart = Student('Bart Simpson', 59)
>>> bart.get_name()
'Bart Simpson'
>>> bart.__name = 'New Name' # 设置__name变量！
>>> bart.__name
'New Name'
```

表面上看，外部代码“成功”地设置了`__name`变量，但实际上这个`__name`变量和class内部的`__name`变量**不是**一个变量！

内部的`__name`变量已经被Python解释器自动改成了`_Student__name`，而外部代码给`bart`新增了一个`__name`变量。

```python
>>> bart.get_name() # get_name()内部返回self.__name
'Bart Simpson'
```



### #练习题

```python
### 把下面的Student对象的gender字段对外隐藏起来，用get_gender()和set_gender()代替，并检查参数有效性
class Student(object):
    def __init__(self, name, gender):
        self.name = name
        self.gender = gender
        
        
### 思路
class Student(object):
    def __init__(self, name, gender):
        self.__name = name
        self.__gender = gender

    def get_gender(self):
        if self.__gender in ['male', 'female', 'others']:
            return self.__gender
        else:
            return "invalid gender"

    def set_gender(self, gen):
        if isinstance(gen, str):
            self.__gender = gen
            
            
# 测试代码：
bart = Student('Bart', 'male')
if bart.get_gender() != 'male':
    print('测试失败!')
else:
    bart.set_gender('female')
    if bart.get_gender() != 'female':
        print('测试失败!')
    else:
        print('测试成功!')
```







# 特性|继承 super()

最大的好处是子类获得了父类的全部功能：属性和方法

- 当然，也可以新增子类的属性和方法



## 本质原理

```python
class Base():
    def print(self):
        print("base")


class A(Base):
    pass


print(id(Base.print))
print(id(A.print))
```

Base.print 和 A.print 的内存地址是相同的，这说明他们是同一个方法。执行A.print时，python会寻找print方法，它会先从A类的定义中寻找，没有找到，然后去A的父类里寻找print方法，如果还是找不到，就继续向上寻找。





## 超类与子类superclass subclass

新的class称为子类（Subclass）、派生类，而被继承的class称为基类、父类或超类（Base class、Super class）

- Sandwich是Food的子类
- Food是Sandwich的超类/父类/基类



super()指向父类实例：`super().__init__([attr,])`

```python
### 1 继承父类的.get_flavour()方法
class Food:
    def __init__(self, ingredients):
        self.ingredients = ingredients
    def get_flavor(self):
        return self.ingredients[0]
    
class Sandwich(Food):
    def __init__(self, ingredients):
        Food.__init__(self, ingredients)	#注意在子类中根据需要初始化超类/父类 => 可用super().__init__(self, ingredients)替代
        
pbj = Sandwich(["bread", "peanut butter", "jelly", "bread"])
print(pbj.get_flavor())



### 2 重写override
class Sandwich(Food):
    def __init__(self, ingredients):
        Food.__init__(self, ingredients)	#注意在子类中需要初始化超类/父类
    def get_flavor():
        return ",".join(self.ingredients[1:-1])
        
pbj = Sandwich(["bread", "peanut butter", "jelly", "bread"])
print(pbj.get_flavor())	#[1:-1]表示从1开始，不显示bread
```



增加子类的方法：

```python
class Animal(object):
    def run(self):
        print('Animal is running...')
        
        
### 子类
class Dog(Animal):
    pass

class Cat(Animal):
    pass


# 增加子类的方法
class Dog(Animal):

    def run(self):
        print('Dog is running...')

    def eat(self):
        print('Eating meat...')
   

# 重写父类的方法
class Dog(Animal):

    def run(self):
        print('Dog is running...')

class Cat(Animal):

    def run(self):
        print('Cat is running...')
```





# 特性|多态

多态：同一类事物有多种形态

python面向对象的多态依赖于继承，因为继承，使得子类拥有了父类的方法

- 子类的方法与父类方法重名时是重写 => 那么同一类事物，但是不同实例，就有不同的形态/方法

多态使得不同的子类对象调用相同的 类方法，产生不同的执行结果，可以增加代码的外部调用灵活度。



当子类和父类都存在相同的`run()`方法时，我们说，子类的`run()`覆盖了父类的`run()`，在代码运行的时候，总是会调用子类的`run()`。这样，我们就获得了继承的另一个好处：多态。



首先要对数据类型再作一点说明。当我们定义一个class的时候，我们实际上就定义了一种数据类型。我们定义的数据类型和Python自带的数据类型，比如str、list、dict没什么两样

```python
a = list() # a是list类型
b = Animal() # b是Animal类型
c = Dog() # c是Dog类型


### 用isinstance()判断数据类型
>>> isinstance(a, list)
True
>>> isinstance(b, Animal)
True
>>> isinstance(c, Dog)
True


### 多态：c不仅仅是Dog，c还是Animal
>>> isinstance(c, Animal)
True
```



新增一个`Animal`的子类，不必对`run_twice()`做任何修改，实际上，任何依赖`Animal`作为参数的函数或者方法都可以不加修改地正常运行，原因就在于多态



多态的好处就是，当我们需要传入`Dog`、`Cat`、`Tortoise`……时，我们只需要接收`Animal`类型就可以了，因为`Dog`、`Cat`、`Tortoise`……都是`Animal`类型，然后，按照`Animal`类型进行操作即可。由于`Animal`类型有`run()`方法，因此，传入的任意类型，只要是`Animal`类或者子类，就会自动调用实际类型的`run()`方法



对于一个变量，我们只需要知道它是`Animal`类型，无需确切地知道它的子类型，就可以放心地调用`run()`方法，而具体调用的`run()`方法是作用在`Animal`、`Dog`、`Cat`还是`Tortoise`对象上，由运行时该对象的确切类型决定，这就是多态真正的威力：

调用方只管调用，不管细节，而当我们新增一种`Animal`的子类时，只要确保`run()`方法编写正确，不用管原来的代码是如何调用的。这就是著名的“开闭”原则：

- 对扩展开放：允许新增`Animal`子类
- 对修改封闭：不需要修改依赖`Animal`类型的`run_twice()`等函数

```python
def run_twice(animal):
    animal.run()
    animal.run()
    
    
# 传入Animal()实例
>>> run_twice(Animal())
Animal is running...
Animal is running...


# 传入Dog()实例
>>> run_twice(Dog())	#Dog(Animal)继承了Animal，而run_twice是一个通用模板/函数，指向公共函数.run() => 调用不同Dog类，呈现不同的效果
Dog is running...
Dog is running...


# 传入Cat()实例
>>> run_twice(Cat())
Cat is running...
Cat is running...


### 如果我们再定义一个Tortoise类型，也从Animal派生
class Tortoise(Animal):
    def run(self):
        print('Tortoise is running slowly...')
        
        
# 传入Tortoise()实例
>>> run_twice(Tortoise())
Tortoise is running slowly...
Tortoise is running slowly...
```





## #继承树

继承还可以一级一级地继承下来，就好比从爷爷到爸爸、再到儿子这样的关系。而任何类，最终都可以追溯到根类object，这些继承关系看上去就像一颗倒着的树。比如如下的继承树：

![image-20210119112339054](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210119112339.png)











# 多重继承

对于单继承，直接用类名调用父类方法没有问题

但对于多继承， 会涉及到查找顺序（MRO）、重复调用（菱形继承）等问题

- 为了解决python菱形继承导致基类方法重复调用的问题，引入了super()， super() 函数可用于调用父类(超类)的一个方法

## 普通继承

```python
class A:
    def __init__(self):
        self.attr_a = 1
        print('执行A的初始化函数')


class B(A):
    def __init__(self):
        A.__init__(self)	#继承父类
        self.attr_b = 2

b = B()
print(b.attr_a, b.attr_b)
'''output
执行A的初始化函数
1 2
'''
```



## 菱形继承

```python
class A:
    def __init__(self):
        self.attr_a = 1
        print('执行A的初始化函数')


class B(A):
    def __init__(self):
        A.__init__(self)
        self.attr_b = 2
        print('执行B的初始化函数')

class C(A):
    def __init__(self):
        A.__init__(self)
        self.attr_c = 3
        print('执行C的初始化函数')

class D(B, C):
    def __init__(self):
        B.__init__(self)
        C.__init__(self)
        self.attr_d = 4
        print('执行D的初始化函数')


d = D()
print(d.attr_a, d.attr_b, d.attr_c, d.attr_d)
'''
执行A的初始化函数
执行B的初始化函数
执行A的初始化函数
执行C的初始化函数
执行D的初始化函数
1 2 3 4
'''
```

注意到，类A的初始化函数被执行了两次，这是一个非常危险的行为。

如果A的初始化函数执行了一些==一个进程中只能执行一次的代码==，这样的多进程就会导致严重的问题



 super的引入就是为了解决这种问题

```python
class A:
    def __init__(self):
        self.attr_a = 1
        print('执行A的初始化函数')


class B(A):
    def __init__(self):
        super().__init__()
        self.attr_b = 2
        print('执行B的初始化函数')

class C(A):
    def __init__(self):
        super().__init__()
        self.attr_c = 3
        print('执行C的初始化函数')

class D(B, C):
    def __init__(self):
        super().__init__()	#D的初始化函数中，只使用了一行代码super().__init__(),就将两个父类B和C的初始化函数都执行了， 而且不会重复执行A的初始化函数
        self.attr_d = 4
        print('执行D的初始化函数')


d = D()
print(d.attr_a, d.attr_b, d.attr_c, d.attr_d)
'''
执行A的初始化函数
执行C的初始化函数
执行B的初始化函数
执行D的初始化函数
1 2 3 4
'''
```



## MRO

在我们定义类时，python会计算出一个方法解析顺序列表：就是一个简单的所有基类的线性顺序表

```python
print(D.mro())	#[<class '__main__.D'>, <class '__main__.B'>, <class '__main__.C'>, <class '__main__.A'>, <class 'object'>]
```

按照顺序，先执行B的初始化函数，在执行C的初始化函数，最后执行A的初始化函数



## 带参数的super方法

```python
class A:
    def __init__(self):
        self.attr_a = 1

    def add(self, a, b):
        return a + b

class B(A):
    def __init__(self):
        super().__init__()
        self.attr_b = 2

    def add(self, a, b):
        print('执行B的add')
        return a + b + 1


class C(A):
    def __init__(self):
        super().__init__()
        self.attr_c = 3

    def add(self, a, b):
        print('执行C的add')
        return a + b + 2

class D(B, C):
    def __init__(self):
        super().__init__()
        self.attr_d = 4

    def add(self, a, b):
        return super().add(a, b)

d = D()
print(d.add(1, 2))
'''output
执行B的add
4
'''
```

D的父类B和C都有add方法，在D的add方法时，super().add()会根据mro来决定调用哪个父类的add方法，根据顺序，应该执行B的add方法， 如果你希望执行C的add方法, 那么可以这样来实现add方法

```python
def add(self, a, b):
    return super(B, self).add(a, b)
```

在mro列表里，B的后面是C

- super的==第一个函数指定为B==， 第二个参数设置为self，就会执行C的add方法





## 实例|Animal类的设计

`Animal`类层次的设计，假设我们要实现以下4种动物：

- Dog - 狗狗；
- Bat - 蝙蝠；
- Parrot - 鹦鹉；
- Ostrich - 鸵鸟。

按照哺乳动物和鸟类归类：

![image-20210119160100168](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210119160100.png)

如果按照“能跑”和“能飞”来归类：

![image-20210119160130287](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210119160130.png)

如果要把上面的两种分类都包含进来，我们就得设计更多的层次：

- 哺乳类：能跑的哺乳类，能飞的哺乳类；
- 鸟类：能跑的鸟类，能飞的鸟类。

![image-20210119160148990](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210119160149.png)

如果要再增加“宠物类”和“非宠物类”，这么搞下去，类的数量会呈指数增长，很明显这样设计是不行的。

正确的做法是采用多重继承。首先，主要的类层次仍按照哺乳类和鸟类设计：

```python
class Animal(object):
    pass

# 大类:
class Mammal(Animal):
    pass

class Bird(Animal):
    pass

# 各种动物:
class Dog(Mammal):
    pass

class Bat(Mammal):
    pass

class Parrot(Bird):
    pass

class Ostrich(Bird):
    pass
```

![image-20210119212311524](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210119212311.png)

给动物再加上`Runnable`和`Flyable`的功能，只需要先定义好`Runnable`和`Flyable`的类

```python
class Runnable(object):
    def run(self):
        print('Running...')

class Flyable(object):
    def fly(self):
        print('Flying...')
```

对于需要针对功能的动物，就多继承一个功能类：

```python
class Dog(Mammal, Runnable):
    pass


class Bat(Mammal, Flyable):
    pass
```

通过多重继承，一个子类就可以同时获得多个父类的所有功能



## 示例：狼人(狼 + 人)

```python
class Person():
    def person_walk(self):
        print("走路")


class Wolf():
    def wolf_run(self):
        print("奔跑")


class WolfMan(Person, Wolf):
    def __init__(self, state):
        # 1表示狼形态,2表示人形态
        self.state = state
    def change(self):
        if self.state == 1:
            self.state = 2
        else:
            self.state = 1

    def run(self):
        if self.state == 1:
            self.wolf_run()
        else:
            self.person_walk()

wolf_man = WolfMan(1)

wolf_man.run()
wolf_man.change()
wolf_man.run()
```



## 多重继承原则|MixIn

设计类的继承关系时，通常，主线都是单一继承下来的

- 例如，`Ostrich`继承自`Bird`
- 如果需要“混入”额外的功能，通过多重继承就可以实现

比如，让`Ostrich`除了继承自`Bird`外，再同时继承`Runnable`。这种设计通常称之为MixIn。



为了更好地看出继承关系，我们把`Runnable`和`Flyable`改为`RunnableMixIn`和`FlyableMixIn`

类似的，你还可以定义出肉食动物`CarnivorousMixIn`和植食动物`HerbivoresMixIn`，让某个动物同时拥有好几个MixIn：

```python
class Dog(Mammal, RunnableMixIn, CarnivorousMixIn):
    pass
```

MixIn的目的就是给一个类增加多个功能，这样，在设计类的时候，我们优先考虑通过多重继承来组合多个MixIn的功能，而不是设计多层次的复杂的继承关系。



## #Python自带的很多库也使用了MixIn

举个例子，Python自带了`TCPServer`和`UDPServer`这两类网络服务，而要同时服务多个用户就必须使用多进程或多线程模型，这两种模型由`ForkingMixIn`和`ThreadingMixIn`提供。

通过组合，我们就可以创造出合适的服务来。

```python
### 编写一个多进程模式的TCP服务
class MyTCPServer(TCPServer, ForkingMixIn):
    pass

### 编写一个多线程模式的UDP服务
class MyUDPServer(UDPServer, ThreadingMixIn):
    pass


### 更先进的协程模型
class MyTCPServer(TCPServer, CoroutineMixIn):
    pass
```

这样一来，我们不需要复杂而庞大的继承链，只要选择组合不同的类的功能，就可以快速构造出所需的子类。



由于Python允许使用多重继承，因此，MixIn就是一种常见的设计。

只允许单一继承的语言（如Java）不能使用MixIn的设计。



## 补充|多重的继承顺序

- Method Resolution Order (MRO)

class B(A)

class C(A)

class D(B)

class E(C)

class F(D,E)

f=F()

f.func()  查找顺序： F -->D-->B-->E-->C--A

![image-20210119211939651](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210119211939.png)

```python
print(F.__mro__)  #可以输入继承顺序
```

继承顺序遵循MRO（==广度优先==）：

  1，优先找实例对应的类F

  2，自身找不到，再找上层父类，如有多个父类，先找左边D。

  3，仍未找，继续找左边父类的父类B，还是先找左边。直到找到第二层父类B

  4，再继续找右边父类E，直到顶层父类A。

  5，如果顶层父类A也找不到，最后会找object





# 特殊类|枚举Enum

当我们需要定义常量时，一个办法是用大写变量通过整数来定义，例如月份：

```
JAN = 1
FEB = 2
MAR = 3
...
NOV = 11
DEC = 12
```

好处是简单，缺点是类型是`int`，并且仍然是变量。

更好的方法是为这样的枚举类型定义一个class类型，然后，每个常量都是class的一个唯一实例。Python提供了`Enum`类来实现这个功能：

```python
from enum import Enum

Month = Enum('Month', ('Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'))
```

可以直接使用`Month.Jan`来引用一个常量，或者枚举它的所有成员：

```python
for name, member in Month.__members__.items():
    print(name, '=>', member, ',', member.value)
```

`value`属性则是自动赋给成员的`int`常量，默认从`1`开始计数



如果需要更精确地控制枚举类型，可以从`Enum`派生出自定义类：

```python
from enum import Enum, unique

@unique
class Weekday(Enum):
    Sun = 0 # Sun的value被设定为0
    Mon = 1
    Tue = 2
    Wed = 3
    Thu = 4
    Fri = 5
    Sat = 6
```

`@unique`装饰器可以帮助我们检查保证没有重复值



访问这些枚举类型可以有若干种方法：

- 既可以用==成员名称==引用枚举常量
- 又可以直接==根据value的值==获得枚举常量

```python
>>> day1 = Weekday.Mon
>>> print(day1)
Weekday.Mon
>>> print(Weekday.Tue)
Weekday.Tue
>>> print(Weekday['Tue'])
Weekday.Tue
>>> print(Weekday.Tue.value)
2

>>> print(day1 == Weekday.Mon)
True
>>> print(day1 == Weekday.Tue)
False
>>> print(Weekday(1))
Weekday.Mon
>>> print(day1 == Weekday(1))
True

>>> Weekday(7)
Traceback (most recent call last):
  ...
ValueError: 7 is not a valid Weekday
    
    
>>> for name, member in Weekday.__members__.items():
...     print(name, '=>', member)
...
Sun => Weekday.Sun
Mon => Weekday.Mon
Tue => Weekday.Tue
Wed => Weekday.Wed
Thu => Weekday.Thu
Fri => Weekday.Fri
Sat => Weekday.Sat
```





## #练习题

`Enum`可以把一组相关常量定义在一个class中，且class不可变，而且成员可以直接比较。

```python
### 把Student的gender属性改造为枚举类型，可以避免使用字符串
from enum import Enum, unique

@unique #检查枚举过程，没有重复值
class Gender(Enum):
    Male = 0
    Female = 1

class Student(object):
    def __init__(self, name, gender):
        self.name = name
        self.gender = gender
```









# #参考文献

[Link: 教程|廖雪峰](![](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210119160100.png))

[Link: 官网|定制类](https://docs.python.org/3/reference/datamodel.html#special-method-names)

[Link: 类创建|继承dict](http://www.coolpython.net/python_senior/pytip/special_dict.html)