OOP是一种程序设计思想，把对象作为程序的基本单元，一个对象包含了==数据/属性==和==操作数据的函数==

- 封装：类是一种封装技术
- 继承
- 多态



对象：

- id
- type
- value



object是class的实例：

- 拥有attriibute => variable
- method

# #面向过程 vs 面向对象

面向过程的程序设计把计算机程序视为一系列的命令集合，即一组函数的顺序执行。为了简化程序设计，面向过程把函数继续切分为子函数，即把大块函数通过切割成小块函数来降低系统的复杂度



面向对象的程序设计把计算机程序视为一组对象的集合，而每个对象都可以接收其他对象发过来的消息，并处理这些消息，计算机程序的执行就是==一系列消息在各个对象之间传递==

面向过程编程从代码的组织形式来看就是根据业务从上至下的的垒代码，典型的特征就是写出一大堆的函数，还要定义很多全局变量和局部变量，一切的行为都是以函数为基础的。而面向对象编程将函数与变量进一步封装成类，一切的行为都是以类为基础的。类==将数据和函数紧密的联系在一起==并且保证数据不会被随意修改。



在Python中，所有数据类型都可以视为对象，当然也可以自定义对象。自定义的对象数据类型就是面向对象中的类（Class）的概念



## 示例|处理学生的成绩表

### 面向过程

```python
# 数据
std1 = { 'name': 'Michael', 'score': 98 }
std2 = { 'name': 'Bob', 'score': 81 }

# 操作函数：比如打印学生成绩
def print_score(std):
    print('%s: %s' % (std['name'], std['score']))
```





### 面向对象

首选思考的不是程序的执行流程，而是`Student`这种数据类型应该被视为一个对象

- 这个对象拥有`name`和`score`这两个属性（Property）

如果要打印一个学生的成绩，首先必须创建出这个学生对应的对象，然后，给对象发一个`print_score`消息，让对象自己把自己的数据打印出来

```python
class Student(object):

    def __init__(self, name, score):
        self.name = name
        self.score = score

    def print_score(self):
        print('%s: %s' % (self.name, self.score))
        
        
### 应用
bart = Student('Bart Simpson', 59)
lisa = Student('Lisa Simpson', 87)
bart.print_score()
lisa.print_score()
```

给对象发消息实际上就是调用对象对应的关联函数，我们称之为对象的方法（Method）

仅仅从代码量上来看，使用类并没有减少代码，但是从代码的可阅读性上看，使用类组织数据和方法显然更具有优势，而且，==类这种概念，更加符合我们人类的思维==。

- 使用类组织数据和方法，扩展性更好，可以随时添加新的属性和方法



面向对象的设计思想是从自然界中来的

在自然界中，类（Class）和实例（Instance）的概念是很自然的。Class是一种抽象概念，比如我们定义的Class——Student，是指学生这个概念，而实例（Instance）则是一个个具体的Student，比如，Bart Simpson和Lisa Simpson是两个具体的Student。

> 有趣例子
>
> class 类 (人) instance 实例 (你,我,他) 
>
> 你会有些属性(身高,年龄,体重) 你会有些技能(吃饭,泡妞)
>
> - `__init__` 方法的主要作用,就是初始化你的属性,这些属性,在上帝初始化你的时候就要赋予给你,比如`zhangsan = Person(170,29,50)`这时上帝就把你创造出来了，也就是实例化了你
>
> - 然后，你到底有哪些技能呢，这就看有没有在类里面定义了，如果有定义泡妞的技能，那么你就可以调用泡妞的技能来泡妞





面向对象的设计思想：

- 抽象出Class
- 根据Class创建Instance

面向对象的抽象程度又比函数要高，因为一个Class既包含数据，又包含操作数据的方法。







# 概念|类和实例

类Class是抽象的模板，比如Student类，而实例是根据类创建出来的一个个具体的“对象”

每个对象都拥有相同的方法，但各自的数据可能不同

- 类名通常是大写开头的单词
- 通常，如果没有合适的继承类，就使用`object`类，这是所有类最终都会继承的类。
- 和普通的函数相比，在类中定义的函数只有一点不同，就是第一个参数永远是实例变量self

```python
class Student(object):
    pass


### 创建实例
>>> bart = Student()
>>> bart
<__main__.Student object at 0x10a67a590>	#内存地址
>>> Student
<class '__main__.Student'>

# 可以自由地给一个实例变量绑定属性
>>> bart.name = 'Bart Simpson'
>>> bart.name
'Bart Simpson'


# 在创建实例的时候，把一些我们认为必须绑定的属性强制填写进去
class Student(object):

    def __init__(self, name, score):	#第一个参数永远是self，表示创建的实例本身
        self.name = name
        self.score = score
        
        
>>> bart = Student('Bart Simpson', 59)
>>> bart.name
'Bart Simpson'
>>> bart.score
59
```

在类里面，同样用def关键字来定义函数，但是我们管==类里的函数叫方法==

属性就是数据

在类的方法里，self就是实例，python中的self等同于其他编程语言里的this。



self的内存地址，和实例的内存地址是一致的：

```python
class Stu:
    def __init__(self, name, age):
        print("在__init__方法中", id(self))
        self.name = name
        self.age = age

    def run(self):
        print("在run方法中", id(self))
        print("{name} is running".format(name=self.name))


s = Stu('小明', 18)
print("s的内存地址", id(s))
s.run()
'''Output
在__init__方法中 4336221600
s的内存地址 4336221600
在run方法中 4336221600
小明 is running
'''
```

在`__init__`方法被调用时，s这个对象就已经被创建好了，这可以证明它不是构造方法，真正的构造方法是`__new__`，在没有调用构造方法前，对象是不存在的



## 要点：方法属于类，属性属于实例

大多数面向对象语言都符合这个不成文的理论，方法属于类，属性属于实例

- 看到实例，应该意识到内存中存在一个实例对象，实例属性也随之存在 => 实例属性必须依附于实例

- 实例方法则不然，不论实例是否存在，实例方法都客观存在，实例方法是在定义类的时候创建的，只要这个类在，那么这些实例方法就存在

```python
class Stu:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def run(self):
        print("{name} is running".format(name=self.name))

    def print(self):
        print("ok")

Stu.print(None)
s = Stu("小刚", 18)
Stu.run(s)
```

通过类调用类里所定义的print方法，我这里直接传入None,在执行过程中，self就是None

- 即便如此，这个方法也是可以被正常执行的，因为在函数里没有使用实例属性
  - 如果在类的print()方法中使用了实例属性呢？就会报错
- self是None，不是类Stu的实例，因此没有实例属性。



Stu这个类可以直接调用方法，因为方法是属于类的，但属性属于对象，你无法通过类直接访问对象的属性

下面的写法是错误的：

- 原因在于，对象还没有被创建，那么name，age根本就不存在
- 即便是创建了，name和age属性，也是==属于对象==的。

```python
class Stu:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def run(self):
        print("{name} is running".format(name=self.name))

    def print(self):
        print("ok")

print(Stu.name)
```



## 操作|创建

```python
class Car:
    wheels = 4
    def start_engine(self):
        pring("Vroom!")
        
my_car = Car()	#调用class的instance


### attributes在所有的instance中是共享的 => 所以要用self: 用来表示所属的实例instance
class Car:
    wheels = 4
    def start_engine(self):
        self.running = True
        pring("Vroom!")

my_car = Car()
my_car.start_engine()
```





```python
### def __init__(self)	#当创建一个class的实例时：会自动运行__init__(self)初始化
# class Car后面的()：可加也可不添加
class Car:
    wheels = 4
    def __init__(self, color):
        self.color = color
        self.running = False
        
    def start_engine(self):
        self.running = True
        pring("Vroom!")

my_car = Car("red")
pring(my_car.color)
```





# 概念|实例属性 类属性

由于Python是动态语言，根据类创建的实例可以任意绑定属性。

```python
### 给实例绑定属性的方法是通过实例变量，或者通过self变量：
class Student(object):
    def __init__(self, name):
        self.name = name

s = Student('Bob')
s.score = 90



### Student类本身需要绑定一个属性
# 直接在class中定义属性，这种属性是类属性，归Student类所有
class Student(object):
    '''
    这个属性虽然归类所有，但类的所有实例都可以访问到
    '''
    name = 'Student'
    

    
>>> class Student(object):
...    	name = 'Student'
...
>>> s = Student() # 创建实例s
>>> print(s.name) # 打印name属性，因为实例并没有name属性，所以会继续查找class的name属性
Student
>>> print(Student.name) # 打印类的name属性
Student

>>> s.name = 'Michael' # 给实例绑定name属性
>>> print(s.name) # 由于实例属性优先级比类属性高，因此，它会屏蔽掉类的name属性
Michael
>>> print(Student.name) # 但是类属性并未消失，用Student.name仍然可以访问
Student

>>> del s.name # 如果删除实例的name属性
>>> print(s.name) # 再次调用s.name，由于实例的name属性没有找到，类的name属性就显示出来了
Student
```

从上面的例子可以看出，在编写程序的时候，千万不要对实例属性和类属性使用相同的名字，因为相同名称的实例属性将屏蔽掉类属性，但是当你删除实例属性后，再使用相同的名称，访问到的将是类属性。

- 实例属性属于各个实例所有，互不干扰；
- 类属性属于类所有，所有实例共享一个属性；



## #练习题

```python
### 为了统计学生人数，可以给Student类增加一个类属性，每创建一个实例，该属性自动增加
class Student(object):
    count = 0

    def __init__(self, name):
        self.name = name
        Student.count += 1	#调用类的属性
        
        
# 测试代码
if Student.count != 0:
    print('测试失败!')
else:
    bart = Student('Bart')
    if Student.count != 1:
        print('测试失败!')
    else:
        lisa = Student('Bart')
        if Student.count != 2:
            print('测试失败!')
        else:
            print('Students:', Student.count)
            print('测试通过!')
```





# 概念|实例方法，类方法，静态方法

一个方法不访问示例的属性，但会访问类的属性和方法时，就可以设置成==类方法==

某些功能不一定非得访问什么属性啊，它只需要完成我们期望的事情就好了 => ==静态方法==

| 名称     | 定义方法                                                     | 权限                                               | 调用方法                       |
| :------- | :----------------------------------------------------------- | :------------------------------------------------- | :----------------------------- |
| 实例方法 | 第一个参数必须是实例，一般命名为self                         | 可以访问实例的属性和方法，也可以访问类的实例和方法 | 一般通过实例调用，类也可以调用 |
| 类方法   | 使用装饰器@classmethod修饰，第一个参数必须是当前的类对象，一般命名为cls | 可以访问类的实例和方法                             | 实例对象和类对象都可以调用     |
| 静态方法 | 使用装饰器@staticmethod修饰，参数随意，没有self和cls         | 不可以访问类和实例的属性和方法                     | 实例对象和类对象都可以调用     |

修改类Stu的定义来向你展示这3种方法的区别：

```python
class Stu:
    school = '湘北高中'   # 类属性
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def play_basketball(self):	#实例方法
        print("{name} 正在打篮球".format(name=self.name))

    @classmethod	#类方法
    def sport(cls):
        print("{school}的同学都喜欢篮球".format(school=cls.school))

    @staticmethod	#静态方法
    def clean():
        print("打扫教室卫生")


stu = Stu("樱木花道", 17)
stu.play_basketball()       # 通过实例调用实例方法
Stu.play_basketball(stu)    # 通过类调用实例方法

Stu.sport()     # 通过类调用类方法
stu.sport()     # 通过实例调用类方法

Stu.clean()     # 通过类调用静态方法
stu.clean()     # 通过实例调用静态方法

print(stu.school)       # 通过实例访问类属性
stu.school = '山王工业'     # 只是增加了一个实例属性,类属性不会被修改
print(Stu.school)       # 通过类访问类属性

### output
'''
# 实例方法
樱木花道 正在打篮球
樱木花道 正在打篮球

# 类方法
湘北高中的同学都喜欢篮球
湘北高中的同学都喜欢篮球

# 静态方法：和类、实例无关
打扫教室卫生
打扫教室卫生

# 访问类属性
湘北高中
湘北高中
'''
```

这3种方法的核心在于他们的权限

面向对象的设计理念中

- ==方法必定属于某个类==，如果这个方法不属于某个类，那么它就是函数了，就回归到了面向过程编程。

- 方法属于某个类，但这个方法可能不会访问实例和类的任何属性或其他方法，对于这种方法，我们就应该把它设计成静态方法。

- 一个方法会访问到类的属性，就像本示例中的sport方法，整个湘北高中的学生都喜欢篮球，这个方法就不是某个学生所特有的，而是整个类所拥有的一个方法，那么就需要把它设计成类方法

=> 类属性/公共属性：school是类属性，既然类拥有这个属性，那么类实例化出来的对象也自然拥有这个属性，通过类和示例都可以访问到这个属性，但是要注意，这个属性是类的，因此不能通过实例对它进行修改。





# 类的内部管理

python如何存储属性以及如何查找这些属性

## 示例|一个简单的类

- category是类属性
- name是实例属性

```python
class AirVehicle(object):
    category = '飞机'

    def __init__(self, name):
        self.name = name

    def fly(self):
        print('{name} is flying'.format(name=self.name))
```



1 通过类修改category

```python
p1 = AirVehicle('歼20')
p2 = AirVehicle('歼15')

print(p1.category, p1.category)	#飞机 飞机
AirVehicle.category = '战斗机'
print(p1.category, p2.category)	#战斗机 战斗机
```

通过类修改category以后，==对所有实例都生效了==，那么通过实例进行修改是否也可以呢？



2 通过实例修改category

```python
p1 = AirVehicle('歼20')
p2 = AirVehicle('歼15')

print(p1.category, p1.category)	#飞机 飞机
p1.category = '战斗机'
print(p1.category, p2.category)	#战斗机 飞机
```

通过实例p1修改category属性，只对p1生效了



## 原理|如何存储实例属性

python中，实例属性存储在一个字典(`__dict__`)中，对于属性的操作，都是在操作这个字典

```python
p1 = AirVehicle('歼20')
print(p1.__dict__)	#{'name': '歼20'}


### 可以直接操作这个字典
p1 = AirVehicle('歼20')
p1.__dict__['speed'] = '2马赫'
print(p1.speed)   # 2马赫
```





## 原理|如何存储类属性

同样是存储在字典中，只是这个字典是类的字典

```python
print(AirVehicle.__dict__)
'''output
{'__module__': '__main__', 'category': '飞机', '__init__': <function AirVehicle.__init__ at 0x103eefd90>, 
'fly': <function AirVehicle.fly at 0x103f0d268>, '__dict__': <attribute '__dict__' of 'AirVehicle' objects>, '__weakref__': <attribute '__weakref__' of 'AirVehicle' objects>, '__doc__': None}
'''
```

只关注category和fly，类的属性和方法存在在类的`__dict__`之中



## 属性寻找规则

在实例p1中，是没有category这个属性的

- 当解释器在p1的`__dict__`找不到category时，就会去类AirVehicle的`__dict__`查找，找到后返回



当你执行p1.category = '战斗机'时，修改的是p1的`__dict__`， 而类AirVehicle的`__dict__`则并没有被修改，因此再次执行print(p1.category, p2.category)时，p1的category是战斗机， 而p2的category还是飞机

因为p2的`__dict__`中仍然没有category这个key，还是要到AirVehicle中寻找



# 追问|类的意义

面向对象是一种编程技术，也是一种编程思想，它最贴近人类的普通思维



背景：小明和小红是同班同学，现在，需要你用python代码来存储他们两个人最基本的信息，姓名，和年龄

1 最普通的存储方式

```python
# 小明的信息
name1 = '小明'
age1 = 14

# 小红的信息
name2 = '小红'
age2 = 14
```

正常人一眼就能看出代码中隐藏的问题，如果有10个学生信息需要存储，难道要写20行代码么？



2 使用字典存储学生信息

```python
xiaoming = {'name': '小明', 'age': 14}
xiaohong = {'name': '小红', 'age': 14}
```

这样做也存在问题，虽然很隐蔽，但不可忽视。学生的信息是完全暴露的，可以在任意位置为修改

- `xiaoming['age'] = 15`

对于一个简单的只有50行的代码，你这样写或许是没问题的，但工作中的项目，都是需要多人合作的，你以为没有人会乱动你的数据，但你无法保证它不被修改，在未知情况下被人修改了数据，一旦出了问题，是很难追查的。



面向对象三大特性之一的封装，其目的之一，就是为了避免数据随意被修改

```python
class Stu:
    def __init__(self, name, age):
        self.__name = name
        self.__age = age

    def get_name(self):
        return self.__name

    def set_name(self, name):
        self.__name = name

    def get_age(self):
        return self.__age

    def set_age(self, age):
        self.__age = age

stu1 = Stu('小明', 14)
stu2 = Stu('小红', 14)

print(stu1.get_name(), stu1.get_age())
print(stu2.get_name(), stu2.get_age())
```

现在，如果想要修改age或者name属性，name必须通过set_age或者set_name方法：

- 如果你想把age修改成一个不合理的数值，我就可以在set_age方法里进行检查来阻止你这种不合理的操作



通过使用类，可以将数据保护起来，不受意外操作的破坏，这只是面向对象编程技术众多好处中的一个。

python中，使用一个下划线就等于向使用者宣布这个属性是私有的，但你仍然可以直接对其修改，单个下划线更像是一种约定，而非技术上的强硬限制。即便使用了双下划线，也仍然有办法直接对其修改，但这已经不是类所要解决的问题了，一个人执意破坏数据，他总是能找到办法。



# 拓展|静态语言 vs 动态语言

对于静态语言（例如Java）来说，如果需要传入`Animal`类型，则传入的对象必须是`Animal`类型或者它的子类，否则，将无法调用`run()`方法。

对于Python这样的动态语言来说，则不一定需要传入`Animal`类型。我们只需要保证传入的对象有一个`run()`方法就可以了：

```python
class Timer(object):
    def run(self):
        print('Start...')
   

### 应用到函数run_twice()
def run_twice(animal):
    animal.run()
    animal.run()
    
    
# 保证传入的对象有一个run()方法就可以
run_twice(Timer())
```

这就是动态语言的“鸭子类型”，它并不要求严格的继承体系，一个对象只要“看起来像鸭子，走起路来像鸭子”，那它就可以被看做是鸭子。



Python的“file-like object“就是一种鸭子类型。对真正的文件对象，它有一个`read()`方法，返回其内容。但是，许多对象，只要有`read()`方法，都被视为“file-like object“。许多函数接收的参数就是“file-like object“，你不一定要传入真正的文件对象，完全==可以传入任何实现了`read()`方法的对象==。



# #小结及示例：银行账号管理

继承可以把父类的所有功能都直接拿过来，这样就不必重零做起，子类只需要新增自己特有的方法，也可以把父类不适合的方法覆盖重写。

动态语言的鸭子类型特点决定了继承不像静态语言那样是必须的。

## 1 属性与方法

属性：

- 用户姓名（username）
- 账号(card_no)
- 余额(balance)

方法：

- 存钱（deposit）
- 取钱（withdrawal）
- 转账（transfer）
- 查看操作记录（history）



## 2 业务分析

在取钱时，如果账户余额小于所取金额，那么要提示用户余额不足，在转账的时候同样如此。

在存钱，取钱，转账时，都必须将业务操作记录保存到一个列表中，查看历史记录时，遍历这个列表，这里我不考虑查询历史记录的时间范围。



## 3 代码实现

### step1 定义类

```python
class BankAccount(object):

    def __init__(self, username, card_no, balance):
        self.username = username     # 用户姓名
        self.card_no = card_no       # 账号
        self.balance = balance       # 余额
        self.history_lst = []        # 历史操作记录

    def deposit(self, amount):
        '''
        存钱
        :param amount:
        :return:
        '''
        pass

    def withdrawal(self, amount):
        '''
        取钱
        :param amount:
        :return:
        '''
        pass

    def transfer(self, another, amount):
        '''
        转账
        :param another:
        :param amount:
        :return:
        '''
        pass

    def history(self):
        '''
        历史操作记录
        :return:
        '''
        pass
```



### step2 实现存钱方法

不论进行何种操作，金额都必须是大于0的 => 需要一个判断金额是否合法的方法@编程范式：函数单一功能

```python
    @classmethod
    def is_amount_legitimate(amount):
        '''
        判断金额是否合法
        :return:
        '''
        if not isinstance(amount, (float, int)):
            return False

        if amount <= 0:
            return False

        return True
```

这个方法无需访问实例属性，因此我把它定义为类方法



每一次操作，都需要记录，记录里必然包括操作发生的时间，因此还需要一个获取当前时间的方法

```python
    @classmethod
    def _get_current_time(cls):
        now = datetime.now()
        current_time = now.strftime('%Y-%m-%d %H:%M:%S')
        return current_time
```



实现存钱方法

```python
    def deposit(self, amount):
        '''
        存钱
        :param amount:
        :return:
        '''
        if not self.is_amount_legitimate(amount):
            print('金额不合法')
            return

        self.balance += amount
```



### step3 实现取钱

转账，都需要判断操作的金额是否小于等于账户余额 => 单独函数

```python
    def withdrawal(self, amount):
        '''
        取钱
        :param amount:
        :return:
        '''
        if not self.is_amount_legitimate(amount):
            print('金额不合法')
            return

        self.balance -= amount
        log = "{operate_time}取出金额{amount}".format(operate_time=self._get_current_time(), amount=amount)
        self.history_lst.append(log)
```





### step4 实现转账

此前，我只定义了转账函数，还缺少一个接收转账的函数

```python
    def transfer(self, another, amount):
        '''
        转账
        :param another:
        :param amount:
        :return:
        '''
        self.balance -= amount
        another.accept_transfer(self, amount)
        log = '{operate_time}向{username}转账{amount}'.format(operate_time=self._get_current_time(),
                                                           username=another.username,
                                                           amount=amount)
        self.history_lst.append(log)

    def accept_transfer(self, another, amount):
        '''
        接收转账
        :param another:
        :param amount:
        :return:
        '''
        self.balance += amount
        log = '{operate_time}收到{username}转来的{amount}'.format(operate_time=self._get_current_time(),
                                                           username=another.username,
                                                           amount=amount)
        self.history_lst.append(log)
```





### step5 查看历史操作记录

```python
    def history(self):
        '''
        历史操作记录
        :return:
        '''
        for log in self.history_lst:
            print(log)
```



### 测试

```python
def test():
    account_1 = BankAccount('小明', '248252543', 4932.13)
    account_2 = BankAccount('小红', '429845363', 3211.9)

    account_1.deposit(400)      # 存钱
    account_1.withdrawal(300)   # 取钱

    account_1.transfer(account_2, 1024)     # 转账
    account_1.history()                     # 查询历史操作记录

    account_2.history()

if __name__ == '__main__':
    test()
    
'''Output
2020-01-09 19:48:01 存入金额400
2020-01-09 19:48:01取出金额300
2020-01-09 19:48:01向小红转账1024
2020-01-09 19:48:01收到小明转来的1024
'''
```



# #日常对象

| 父类    | 子类     |
| ------- | -------- |
| Vehicle | Car      |
| Food    | Sandwich |
|         |          |
|         |          |
|         |          |



# #参考文献

[Link: 教程|廖雪峰](https://www.liaoxuefeng.com/wiki/1016959663602400/1017495723838528)

[Link: 超类与super().__init__()](https://www.programiz.com/python-programming/methods/built-in/super#:~:text=Join-,Python%20super(),Working%20with%20Multiple%20Inheritance)