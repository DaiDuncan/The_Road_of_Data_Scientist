特殊变量/函数\__xxx__

- Python的class允许定义许多定制方法，可以让我们非常方便地生成特定的类



# `__new__()` `__init__()`

构造函数创建对象以后才会调用`__init__`为实例的属性赋值：

- `__init__()`是初始化函数
- 真正的构造函数是`__new__`

`__init__`不是构造函数的最直接证据便是self，==self是实例，这说明实例已经被创建==。

```python
class Stu:
    def __init__(self, name, age, score):
        self.name = name
        self.age = age
        self.score = score
        
stu = Stu('小明', 18, 600)
```



一般来说，不建议重写`__new__`，除非是构建str, int或者tuple不可变类型的子类

比如，我现在定义一个正整数类，如果只在`__init__()`方法上下文章是解决不了问题的

```python
class PositiveInteger(int):
    def __init__(self, value):
        print(type(self), self)
        super(PositiveInteger, self).__init__()	#super().__init__()

i = PositiveInteger(-5)	#-5指的是int类，对应实例self => 并没有某个属性来存储-5
print(i)

# Output
<class '__main__.PositiveInteger'> -5
-5
```

当执行print(i)时输出的是i的这个属性。

而实际情况是self这个实例就是-5，并没有某个属性来存储-5。这就意味着我们无法通过重写`__init__`方法来让对象i的值变为5，==在构造i这个对象时，它本身就已经是-5了==



为了创建出正整数类，只能重载`__new__`方法

```python
class PositiveInteger(int):
    def __new__(cls, value):
        return super(PositiveInteger, cls).__new__(cls, abs(value))

i = PositiveInteger(-5)
print(i)
```

构造PositiveInteger实例时，将参数的值修改为abs(value)，这样就可以确保得到的是正整数







# `__call__()`  调用对象触发的操作

一个对象实例可以有自己的属性和方法，当我们调用实例方法时，我们用`instance.method()`来调用。能不能直接在实例本身上调用呢？

python中, 一个类，如果实现了`__call__`方法，那么这个类的实例就是callable对象（可调用对象）



任何类，只需要定义一个`__call__()`方法，就可以直接==对实例进行调用==

```python
class Student(object):
    def __init__(self, name):
        self.name = name

    def __call__(self):
        print('My name is %s.' % self.name)
        
        
        
### 调用call
>>> s = Student('Michael')
>>> s() # self参数不要传入
My name is Michael.
```

`__call__()`还可以定义参数。

对实例进行直接调用就好比对一个函数进行调用一样，所以你完全可以把对象看成函数，把函数看成对象，因为这两者之间本来就没啥根本的区别。



那么，怎么判断一个变量是对象还是函数呢？其实，更多的时候，我们需要==判断一个对象是否能被调用==，能被调用的对象就是一个`Callable`对象，比如函数和我们上面定义的带有`__call__()`的类实例：

```python
### 通过callable()函数，我们就可以判断一个对象是否是“可调用”对象
>>> callable(Student())	#类的实例
True
>>> callable(max)	#函数
True
>>> callable([1, 2, 3])
False
>>> callable(None)
False
>>> callable('str')
False
```



## 应用|类的实例对象伪装成函数

### 1 基于类实现装饰器

```python
from functools import wraps

class SafeAdd:
    def __init__(self, func):
        self.func = func       # func是被装饰的函数

    def __call__(self, *args, **kwargs):
        if len(args) == 2:
            left = args[0]
            right = args[1]         # 获取参数并进行类型转换
            if not isinstance(left, (int, float)):
                left = float(left)

            if not isinstance(right, (int, float)):
                right = float(right)

            return left + right

        return None

@SafeAdd
def add(a, b):
    return a + b


print(add(3, 5))
print(add('3', '6'))
```

换一种写法，虽然我们正式使用装饰器时很少这样写，但更容易理解

```python
def add(a, b):
    return a + b

safe_add = SafeAdd(add)     # add函数作为参数传入SafeAdd的初始化函数\__init__中
# safe_add是类SafeAdd的实例
print(safe_add(3, 5))
print(safe_add('3', '6'))
```

实例对象safe_add由于实现了`__call__`方法，因此可以像使用函数一样通过一对小括号来调用执行



### 2 实现WSGI协议

flask， tornado， django 等web框架，无一例外的遵守WSGI协议，WSGI是一个协议，是一份约定

它规定服务器和应用程序之间如何传递数据，各自应该实现什么样的接口，以便彼此间配合工作。对于应用程序段，它只规定了3个简单的要求：

- 应用程序是一个可调用对象（callable）
- 可调用对象接受两个参数，分别是environ和start_response
- 可调用对象需要返回一个可迭代对象

```python
from flask import Flask
app = Flask(__name__)


@app.route('/')
def hello_world():
    return 'Hello World!'


if __name__ == '__main__':
    app.run()
```

app就是应用程序，它是类Flask的实例，app是可调用对象，深入Flask类的源码可以发现，Flask类实现了`__call__`方法

```python
def __call__(self, environ, start_response):
    """The WSGI server calls the Flask application object as the
    WSGI application. This calls :meth:`wsgi_app` which can be
    wrapped to applying middleware."""
    return self.wsgi_app(environ, start_response)
```



# metaclass元类 控制类的创建行为

在python中，类(class)本身也是一个实例对象, 它的类型则是元类

-  如果没有指明，则自定义类的类型是type
- 我们所定义的普通类都是type的实例对象
  - ==如果一个类继承了type, 那么这个类就是元类==

```python
class A(type):	#A就是一个元类
    pass
```

## 元类的`__new__`方法

在定义一个类时，指定metaclass，就意味着这个类将有所指定的metaclass来创建

```python
class MyMeta(type):
    def __new__(cls, *args, **kwargs):
        _class = super().__new__(cls, *args, **kwargs)	#基于type，创建一个新的类
        print(_class.__name__)
        return _class


class Animal(metaclass=MyMeta):
    def __init__(self, name):
        self.name = name
```

类MyMeta是元类

- 在定义Animal这个类时，我指定了它的元类是MyMeta => 因此，类Animal将由MyMeta的`__new__`方法来创建

类Animal是类MyMeta的实例对象

- 在MyMeta的`__new__`方法中，我使用print语句输出了`__class`的`__name__`属性，理论分析告诉我们，这个值应该是Animal

元类是用来创建普通类（自定义类）的，我们可以利用元类对普通类进行一些限制和要求

- 比如，我们可以要求所有继承Animal的类必须拥有run方法

```python
from inspect import isfunction

class MyMeta(type):
    def __new__(cls, *args, **kwargs):
        _class = super().__new__(cls, *args, **kwargs)
        if _class.__name__ != 'Animal':	#不是Animal类 => Animal的子类
            # 如果没有run方法，就会报错
            if not hasattr(_class, 'run') or not isfunction(getattr(_class, 'run')):
                raise Exception('类{name}没有实现run方法'.format(name=_class.__name__))
        return _class


class Animal(metaclass=MyMeta):
    def __init__(self, name):
        self.name = name


class Cat(Animal):
    def __init__(self, name):
        super().__init__(name)

cat = Cat('加菲猫')
```

类Cat继承了Animal，那么它的元类也是MyMeta

- 在MyMeta的`__new__`方法里将创建出类Cat，创建以后会检查类Cat是否有run属性且该属性是一个函数，如果不满足条件则抛出异常
- 如果类Cat实现了run方法，那么上述代码将正常执行

务必想清楚一点：尽管我们在脚本里使用class定义了类Cat(Animal), 但并不是真正的创建了类Cat,我们所写的代码仅仅是一个定义，创建的过程使用元类MyMeta来完成的。





## 元类的`__init__`方法

元类的`__new__`负责构建普通类，`__init__`负责对这个普通类进行初始化

```python
from inspect import isfunction

class MyMeta(type):
    def __new__(cls, *args, **kwargs):
        _class = super().__new__(cls, *args, **kwargs)
        if _class.__name__ != 'Animal':
            if not hasattr(_class, 'run') or not isfunction(getattr(_class, 'run')):
                raise Exception('类{name}没有实现run方法'.format(name=_class.__name__))
        return _class

    def __init__(self, *args, **kwargs):
        super().__init__(*args, **kwargs)
        self.home = 'earth'


class Animal(metaclass=MyMeta):
    def __init__(self, name):
        self.name = name


class Cat(Animal):
    def __init__(self, name):
        super().__init__(name)

    def run(self):
        print('run')

print(Animal.home)
print(Cat.home)
```

在元类的`__init__`方法里，self参数就是我们所创建的类，Animal和Cat， 我们为他们增加了类属性home

重载`__init__`方法，可以更加优雅的实现单例模式

```python
class Singleton(type):
    def __init__(self, *args, **kwargs):
        self.__instance = None
        super().__init__(*args, **kwargs)

    def __call__(self, *args, **kwargs):
        if self.__instance is None:
            self.__instance = super().__call__(*args, **kwargs)
            return self.__instance
        else:
            return self.__instance

class FileLock(metaclass=Singleton):
    pass

file_lock_1 = FileLock()
file_lock_2 = FileLock()
print(file_lock_1 is file_lock_2)
```







## 元类的`__call__`方法

```python
class MyMeta(type):
    def __call__(self, *args, **kwargs):
        raise TypeError('不能创建实例')


class FileTool(metaclass=MyMeta):
    @staticmethod
    def iter_folder(path):
        print('遍历文件夹')

ft = FileTool()
```

上面的代码执行会报错：

​	类FileTool是元类MyMeta的一个实例，那么当执行FileTool()时，不正是在调用元类MyMeta的`__call__`方法么？而MyMeta的`__call__`方法偏偏抛出一个类型异常，这就导致FileTool不能被实例化，我们只能使用它的静态方法



==重载元类的`__call__`方法==和类cat的`__del__`方法可以让我们控制类的实例化过程

```python
class MyMeta(type):	#继承type的类MyMeta是元类
    def __init__(self, *args, **kwargs):
        self.instance_count = 0
        super().__init__(*args, **kwargs)

    def __call__(self, *args, **kwargs):
        if self.instance_count < 3:
            self.instance_count += 1
            return type.__call__(self, *args, **kwargs)
        else:
            raise Exception("类{name}的实例总数超出限制".format(name=self.__name__))

    def __del__(self):
        self.instance_count -= 1

class Cat(metaclass=MyMeta):
    def __init__(self, name):
        self.name = name

    def __del__(self):
        Cat.instance_count -= 1


c1 = Cat('c1')
c2 = Cat('c2')
c3 = Cat('c3')
c4 = Cat('c4')
```

上面的代码中，当创c4的时候会抛出异常，因为实例的数量已经达到上限

想要创建c4，必须销毁一个之前创建的对象实例

```python
c1 = Cat('c1')
c2 = Cat('c2')
c3 = Cat('c3')
del c1
c4 = Cat('c4')
```

销毁c1时，类属性instance_count执行了减一操作，因此可以创建出c4



以上示例代码，不保证有工程实践意义，纯粹是为了讲解元类的功能作用而人为制造出来的，坦率的讲，在实际工作中，几乎用不到元类



---

以下来源于教程|廖雪峰

网友评价：先看一遍有个大致概念，等遇到具体问题再来分析

metaclass是Python面向对象里最难理解，也是最难使用的魔术代码。

正常情况下，你不会碰到需要使用metaclass的情况，所以，以下内容看不懂也没关系，因为基本上你不会用到。

## type() 运行时/动态创建类

动态语言和静态语言最大的不同，就是函数和类的定义，不是编译时定义的，而是运行时动态创建的。

比方说我们要定义一个`Hello`的class，就写一个`hello.py`模块：

```python
class Hello(object):
    def hello(self, name='world'):
        print('Hello, %s.' % name)
```

当Python解释器**载入`hello`模块时**，==就会依次执行该模块的所有语句==，执行结果就是**动态创建出一个`Hello`的class对象**，测试如下：

```python
>>> from hello import Hello
>>> h = Hello()
>>> h.hello()
Hello, world.

'''me：测试
type(Hello())	#Hello实例：结果是_test.hello.Hello <= 自定义了_test包(hello.py模块)：from _test.hello import Hello
type(Hello)		#Hello名称：结果是type

type(max(1,2))	#结果是int：因为返回值是int
type(max)		#结果是builtin_function_or_method
'''

>>> print(type(Hello))	#Hello是一个class的名称，它的类型就是type
<class 'type'>

>>> print(type(h))	#h是一个class的实例，它的类型就是class Hello
<class 'hello.Hello'>
```

class的定义是运行时动态创建的，而创建class的方法就是使用`type()`函数



`type()`函数既可以返回一个对象的类型，==又可以创建出新的类型==，比如，我们可以通过`type()`函数创建出`Hello`类，而无需通过`class Hello(object)...`的定义：

```python
>>> def fn(self, name='world'): # 先定义函数
...     print('Hello, %s.' % name)
...
### type()函数：创建Hello class
'''
第一个参数'Hello'是class的名称
第三个参数基于key-value：定义了class中的方法hello
'''
>>> Hello = type('Hello', (object,), dict(hello=fn)) 

>>> h = Hello()	#调用class Hello
>>> h.hello()	#调用Hello.hello()方法
Hello, world.

>>> print(type(Hello))
<class 'type'>
>>> print(type(h))
<class '__main__.Hello'>
```



要创建一个class对象，`type()`函数依次传入3个参数：

| type()的参数   |                                                              |
| -------------- | ------------------------------------------------------------ |
| 'Hello'        | class的名称                                                  |
| (object,)      | 继承的父类集合，注意Python支持多重继承，如果只有一个父类，别忘了==tuple的单元素写法== |
| dict(hello=fn) | class的方法名称与函数绑定，这里我们把函数`fn`绑定到方法名`hello`上 |



### #小结|本质联系

通过`type()`函数创建的类和直接写class是完全一样的，因为==Python解释器遇到class定义时，仅仅是扫描一下class定义的语法，然后调用`type()`函数创建出class==



正常情况下，我们都用`class Xxx...`来定义类

但是，`type()`函数也允许我们动态创建出类来，也就是说，动态语言本身支持==运行时期动态创建类==，这和静态语言有非常大的不同

要在静态语言运行时期创建类，必须构造==源代码字符串==再==调用编译器==，或者借助一些工具生成字节码实现，本质上都是动态编译，会非常复杂。





## metaclass元类

metaclass是Python中非常具有魔术性的对象，它==可以改变类创建时的行为==。这种强大的功能使用起来务必小心。



定义了类以后，就可以根据这个类创建出实例，所以：先定义类，然后创建实例。

如果我们想创建出类呢？那就必须根据metaclass创建出类，所以：先定义metaclass，然后创建类。

1. 创建类的模版：先定义metaclass
2. type(cls, (super_obj, ), methods_dict )创建类
3. `__init_()`初始化创建实例

metaclass允许你创建类或者修改类。换句话说，你可以==把类看成是metaclass创建出来的“实例”==



### `__new__()` 创建类的方法 => `type.__new__()`

```python
### metaclass可以给我们自定义的MyList增加一个add方法
# 定义ListMetaclass

# metaclass是类的模板，所以必须从type类型派生：
class ListMetaclass(type):
    def __new__(cls, name, bases, attrs):
        attrs['add'] = lambda self, value: self.append(value)	#添加add方法
        return type.__new__(cls, name, bases, attrs)	#从type类型派生
    
    
### 有了ListMetaclass，在定义类的时候还要指示使用ListMetaclass来定制类，传入关键字参数metaclass：
class MyList(list, metaclass=ListMetaclass):
    pass
```

当我们传入关键字参数`metaclass`时，魔术就生效了：

- ==指示Python解释器在创建`MyList`时，要通过`ListMetaclass.__new__()`来创建==



在此，我们可以修改类的定义，比如，加上新的方法，然后，返回修改后的定义

`__new__()`方法接收到的参数依次是：

- cls：当前准备创建的类的==对象==； => `__new__`接收的第一个参数cls（类对象）其实就是相当于`__init__`的self（实例对象）
- name：类的名字；
- bases：类继承的父类集合；
- attrs：类的方法集合。

```python
>>> L = MyList()
>>> L.add(1)
>> L
[1]


### 对比：而普通的list没有add()方法
>>> L2 = list()
>>> L2.add(1)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'list' object has no attribute 'add'
```



### #意义|编写ORM框架

动态修改有什么意义？直接在`MyList`定义中写上`add()`方法不是更简单吗？正常情况下，确实应该直接写，通过metaclass修改纯属变态。

但是，总会遇到需要通过metaclass修改类定义的。ORM就是一个典型的例子。

ORM全称“Object Relational Mapping”，即对象-关系==映射==：

- 把关系数据库的==一行映射为一个对象==，也就是==一个类对应一个表==

这样，写代码更简单，不用直接操作SQL语句。



要编写一个ORM框架，所有的类都只能动态定义，因为只有使用者才能根据表的结构定义出对应的类来。

==编写底层模块的第一步，就是先把调用接口写出来==。比如，使用者如果使用这个ORM框架，想定义一个`User`类来操作对应的数据库表`User`，我们期待他写出这样的代码：

- 父类`Model`和属性类型`StringField`、`IntegerField`是由ORM框架提供的
- 剩下的魔术方法比如`save()`全部由metaclass自动完成

虽然metaclass的编写会比较复杂，但ORM的使用者用起来却异常简单。

1 首先来定义`Field`类，它==负责保存数据库表的字段名和字段类型==：

```python
class Field(object):

    def __init__(self, name, column_type):
        self.name = name
        self.column_type = column_type

    def __str__(self):
        return '<%s:%s>' % (self.__class__.__name__, self.name)
```

2 进一步定义各种类型的`Field`，比如`StringField`，`IntegerField`等等：

```python
class StringField(Field):	#继承Field

    def __init__(self, name):
        super(StringField, self).__init__(name, 'varchar(100)')	#子类中：对超类的初始化

class IntegerField(Field):

    def __init__(self, name):
        super(IntegerField, self).__init__(name, 'bigint')
```

3 编写最复杂的`ModelMetaclass`：

```python
class ModelMetaclass(type):	#根源是type类型：创建类
	# # attrs {属性名：属性类型,...}
    def __new__(cls, name, bases, attrs):	#cls可能与self类似	attrs包含所有传入的参数：包括属性和方法 此处为SQL的数据
        '''
        关键过程：进行属性映射 => attrs
        '''
         # 父类不处理
        if name=='Model':	#如果类的名称是Model，那么就用type创建该类
            return type.__new__(cls, name, bases, attrs)	#此处的attrs是Model的方法 save()
        print('Found model: %s' % name)	#如果是其他的类
        # 声明一个mapper
        mappings = dict()
        
        # k: ['id', 'name', 'email', 'password']
        # v: ['Michael', ...]
        for k, v in attrs.items():	#此处的attrs是User的参数 **kw
            # 判断是不是指定的Filed类：如果对象的属性值是Field类的实例对象(如StringField('username'))，则将这些属性放入dict格式的mappings变量中
            if isinstance(v, Field):
                print('Found mapping: %s ==> %s' % (k, v))
                mappings[k] = v	#mappings作为中转，最后保存到__mappings__属性中
                # 其实上面的这几行可以简化为一行 mappings = {k:v for k, v in attrs.items() if isinstance(v, Field)} 吧？
                
        # 移除类属性，防止同名类属性和实例属性冲突  
        for k in mappings.keys():
            attrs.pop(k)
        
        # 把dict格式的mappings变量添加到类对象User的属性__mapping__中
        attrs['__mappings__'] = mappings #添加__mappings__属性：保存属性和列的映射关系 => 该属性是一个字典
        # 添加__table__属性：假设表名和类名一致
        attrs['__table__'] = name 
        return type.__new__(cls, name, bases, attrs)
```

基类`Model`：

```python
class Model(dict, metaclass=ModelMetaclass):
    '''在运行__init__来生成实例对象前，先调用元函数ModelMetaclass来生成类对象，用这个类对象再去生成实例对象
    super()继承dict类：所以可以使用self[key]
    
    另一种观点：不是继承dict的关系,而是**kw传进去就是一个dict
    '''

    def __init__(self, **kw):	#参数基于超类dict，解析为**kw
        super(Model, self).__init__(**kw)

    def __getattr__(self, key):
        try:
            return self[key]	#key如果已经存在(说明__setattr__中已经建立属性)就返回属性value
        except KeyError:
            raise AttributeError(r"'Model' object has no attribute '%s'" % key)

    def __setattr__(self, key, value):
        self[key] = value

    def save(self):
        fields = []
        params = []
        args = []
        for k, v in self.__mappings__.items():	#self.__mappings__是一个属性字典
            # v是一个Field对象，有属性.name
            fields.append(v.name)	#field: [id,username,email,password] 
            params.append('?')		#params: [?,?,?,?]
            # get key对应的value
            args.append(getattr(self, k, None))	#args的值: [None, 'Michael', 'test@orm.org', 'my-pwd']
        sql = 'insert into %s (%s) values (%s)' % (self.__table__, ','.join(fields), ','.join(params))	#fields和params都是字符串的列表
        print('SQL: %s' % sql)
        print('ARGS: %s' % str(args))	#args是初始化后的实例属性的值
```

当用户定义一个`class User(Model)`时：

​	Python解释器首先在当前类`User`的定义中查找`metaclass`

​	如果没有找到，就继续在父类`Model`中查找`metaclass`

找到了，就使用`Model`中定义的`metaclass`的`ModelMetaclass`==来创建`User`类==，也就是说，metaclass可以隐式地继承到子类，但子类自己却感觉不到。

- 这也就是为什么`class User(Model)`中没有初始化，因为直接在Model中，传递了实例的参数 **kw
- 并且用Model对应的`ModelMetaclass类`来创建了类
  - 先创立了Model类：顺序是User => Model => 元类创建
  - 再创建了User类

在`ModelMetaclass`中，一共做了几件事情：

1. if分支：==排除掉==对`Model`类的修改；
2. 在当前类（比如`User`）中查找定义的类的==所有属性==，如果找到一个Field属性，**就把它保存到一个`__mappings__`的dict中**，同时==从类属性中删除该Field属性，否则，容易造成运行时错误==（==实例的属性==会遮盖类的同名属性）；
3. 把表名保存到`__table__`中，这里简化为表名默认为类名。



在`Model`类中，就可以定义各种操作数据库的方法，比如`save()`，`delete()`，`find()`，`update`等等。



#### 具体操作

我们实现了`save()`方法，把一个实例保存到数据库中。因为有表名，属性到字段的映射和属性值的集合，就可以构造出`INSERT`语句。

```python
class User(Model):
    # 定义类的属性到列的映射：
    id = IntegerField('id')
    name = StringField('username')
    email = StringField('email')
    password = StringField('password')
    
    '''按照我的理解：补充下述代码 => 也可以：莫非当父类Model继承元类时，这些初始化可以省略？
    def __init__(self, **kw):
    	super()._init__(**kw)
    '''

# 创建一个实例：
u = User(id=12345, name='Michael', email='test@orm.org', password='my-pwd')
# 保存到数据库：
u.save()
```



输出如下：

```python
Found model: User	#class ModelMetaclass(type)
Found mapping: email ==> <StringField:email>		#class ModelMetaclass(type)	
Found mapping: password ==> <StringField:password>	#class ModelMetaclass(type)	
Found mapping: id ==> <IntegerField:uid>			#class ModelMetaclass(type)	
Found mapping: name ==> <StringField:username>		#class ModelMetaclass(type)
SQL: insert into User (password,email,username,id) values (?,?,?,?)	#def save(self)
ARGS: ['my-pwd', 'test@orm.org', 'Michael', 12345]					#def save(self)
```

可以看到，`save()`方法已经打印出了可执行的SQL语句，以及参数列表，只需要真正连接到数据库，执行该SQL语句，就可以完成真正的功能。



#### 拓展SQL

```python
# 定义一个class派生自dict,定义一个方法生成SQL语句
@unique
class SQLType(Enum):
    NoneType = 0
    Create = 1
    Insert = 2
    Delete = 3
    Select = 4

class Model(dict, metaclass=ModelMetaClass):
    def __init__(self, *args, **kwargs):
        super().__init__(*args, **kwargs)

    # 重写setter和getter方法，添加属性自动存储到list中
    def __getattr__(self, key):
        try:
            return self[key]
        except:
            raise AttributeError('Read "Model" Object has no attribute "%s"' % key)

    def __setattr__(self, key, value):
        self[key] = value

    # 生成SQL方法
    def getSQL(self,sqlType=SQLType.NoneType):
        fileds, params, args, typeDict = ([], [], [], [])
        if not isinstance(sqlType, SQLType):
            raise TypeError('Error SQL Type, Please Use SQLType')

        # 读取元类中mapping属性
        for k, v in self.__mapping__.items():
            fileds.append(k)
            params.append('?')
            args.append(getattr(self, k))
            typeDict.append('%s %s' % (v, k))

        if SQLType.Create == sqlType:
            return 'CREATE TABLE %s (%s)' % (self.__table__, ','.join(typeDict))
        elif SQLType.Insert == sqlType:
            return 'INSERT INTO %s (%s) VALUES (%s)' % (self.__table__, ','.join(fileds), ','.join(params))
        elif SQLType.Delete == sqlType:
            return 'DELETE FROM %s' % self.__table__
        elif SQLType.Select == sqlType:
            return 'SELECT %s FROM %s' % (','.join(fileds), self.__table__)
        else:
            return 'ARGS:%s' % str(args)

class User(Model):
    id = IntegerFiled('id')
    name = StringFiled('username')
    email = StringFiled('email')
    # password = StringFiled('password')

    

user = User(id = 12345, name = 'gitkong', email = 'xxx.163.com', password = '123')
print(user.getSQL(SQLType.Create))
print(user.getSQL(SQLType.Delete))
print(user.getSQL(SQLType.Insert))
print(user.getSQL(SQLType.Select))
print(user.getSQL())
```



### #小结|评论

总结User实例对象的创建步骤：

1. User类的创建1: User类继承了Model类，而Model类由元类ModelMetaclass定义类的生成

   所以User类对象首先通过ModelMetaclass的`type.__new__()`构造生成

2. User类的创建2: 在ModelMetaclass的`type.__new__()`构造函数中，name和bases已经确认

   而属性attr是根据输入的属性内容，由`__new__`构造出

3. User实例的创建1：User类继承了Model类，==所以User通过Model的构造函数`__init__`生成属性的时候会把输入的(id=12345, name='Michael')转化为dict格式{'id': 12345, 'name': 'Michael'}==

   - User继承了Model的属性 **kw
   - 而Model又继承了dict的属性 => 解析**kw

4. User实例的创建2: User实例的生成中要按照元类ModelMetaclass的定义生成，即生成`__mappings`__和`__Table__`等属性





### #小结|me

![image-20210120193400835](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210120193401.png)

#### User类的创建

1. User类继承了Model类

- Model类由元类ModelMetaclass定义类的生成
  - ModelMetaclass最终基于type：所以才被称为创建类的模板
- User类对象通过ModelMetaclass的`type.__new__()`构造生成
  - 其中：属性attr是根据==输入的属性内容==，由`__new__`构造出



2. User类的属性

- 继承了Model类：那么也就继承了**kw的属性
  - 相对应的：传递的参数既是User类的属性，也是Model类的属性
  - 所以User通过Model的构造函数`__init__`生成属性(继承Model的属性)的时候：会把输入的(id=12345, name='Michael')转化为dict格式{'id': 12345, 'name': 'Michael'}
- 此外：在ModelMetaclass中，还定义了`__mappings__`和`__table__`的属性



#### User类的初始化/实例化

1. 类属性

- id = IntegerField('id')

- name = StringField('username')

- email = StringField('email')
- password = StringField('password')
- 以上类属性是Field对象：当实参id='12345'时，该'12345'已经对应初始化为Field对象 => @Python：万物皆对象

​    

2. 实参**kw初始化为：User类的基本属性 

- 实参传入Model
  - 基于`__getattr__`进行解析 => 相当于==实参都是命名参数==，必须保证命名的一致性，否则User类的属性值会变成None



3. **kw传递给Model类

- 基于超类dict进行解析



```python
class User(Model):
    # 定义类的属性到列的映射：
    password = StringField('password')
    name = StringField('username')
    id = IntegerField('id')    
    email = StringField('email')
    
    
    def __init__(self, **kw):
        super().__init__(**kw)
        
###test 1: 改变User类属性的顺序 => 影响ModelMetaclass中__mappings__属性
'''输出结果(最初是id在第一个位置)
Found model: User
Found mapping: password ==> <StringField:password>
Found mapping: name ==> <StringField:username>
Found mapping: id ==> <IntegerField:id>
Found mapping: email ==> <StringField:email>
SQL: insert into User (password,username,id,email) values (?,?,?,?)
'''

        
### test2: 打乱实参的顺序（包括故意命名错误：比如passward命名为pwd，输出结果同下）
'''输出结果：基于上述User类属性的顺序
	实参中passward没有值，对应None
ARGS: [None, 'Michael', '13', 'test@orm.org']
'''
#u = User(id=12345,email='test@orm.org', name='Michael' , de='my-pwd')
u = User(email='test@orm.org', id='13', name='Michael')
u.save()
```

