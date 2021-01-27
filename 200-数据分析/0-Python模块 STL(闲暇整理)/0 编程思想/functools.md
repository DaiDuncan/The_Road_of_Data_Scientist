提供了一些非常神奇的高阶函数， 其中为人熟知的有reduce，partial，wraps， 他们是作用于或返回其他函数的函数

 一般来说任何可调用的对象都可以用这个模块里的函数进行处理



# partial 偏函数

和装饰器对比来理解，装饰器改变了一个函数的行为，而偏函数不能改变一个函数的行为。偏函数只能根据已有的函数生成一个新的函数，这个新的函数完成已有函数相同的功能，但是，==这个新的函数的部分参数已被偏函数确定下来==



## 应用场景

假设我们的程序要在dest目录下新建一些文件夹：

```python
import os
from os import mkdir


mkdir(os.path.join('./dest', 'dir1'))
mkdir(os.path.join('./dest', 'dir2'))
mkdir(os.path.join('./dest', 'dir3'))
```

功能很简单，代码很简洁，但是有个小小的不如意之处，每次都是在dest目录下新建文件夹，既然它这么固定，是不是可以不用传递dest参数呢？

```python
import os
from os import mkdir
from functools import partial


dest_join = partial(os.path.join, './dest')	#

mkdir(dest_join('dir1'))
mkdir(dest_join('dir2'))
mkdir(dest_join('dir3'))
```

dest_join是partial创建出来的一个新的函数

- 函数在执行时会调用os.path.join并将'./dest'作为参数传给join
- 偏函数将预先确定的参数冻结，起到了缓存的作用，在获得了剩余的参数后，连同之前冻结的确定参数一同传给最终的处理函数。

虽然看起来代码没有减少，但如果你实际使用就会发现，用了偏函数，免去了反复输入相同参数的麻烦，尤其当这些参数很多很难记忆时





# cmp_to_key

可以将一个cmp函数变成一个key函数，从而支持自定义排序

python的列表提供了sort方法：

```python
lst = [(9, 4), (2, 10), (4, 3), (3, 6)]
lst.sort(key=lambda item: item[0])
print(lst)
```

sort方法的key参数需要设置一个函数，这个函数返回元素参与大小比较的值，这看起来没有问题，但如果想实现更加复杂的自定义排序，就不那么容易了。

- 目前的排序规则是根据元组里第一个元素的大小进行排序



修改规则：

- 如果元组里第一个元素是奇数，就用元组里第一个元素进行排序
- 如果元组里第一个元素是偶数，则用这个元组里的第二个元素进行大小比较

```python
from functools import cmp_to_key
lst = [(9, 4), (2, 10), (4, 3), (3, 6)]

def cmp(x, y):
    '''举例
    x是(9, 4)	9是奇数，所以用9来代表x这个元组，进行排序
    y是(2, 10)	2是偶数，所以用10来代表y这个元组，进行排序
    '''
    a = x[0] if x[0] %2 == 1 else x[1]	#如果是奇数，赋值为第一个元素；是偶数赋值为第二个元素
    b = y[0] if y[0] %2 == 1 else y[1]

    return 1 if a > b else -1 if a < b else 0	#大于为1，小于为-1，等于为0

lst.sort(key=cmp_to_key(cmp))	#元组各个元素作为cmp函数的输入，比如(9, 4)？？？
print(lst)
```





原理：cmp_to_key的实现非常简单

```python
def cmp_to_key(mycmp):
    """Convert a cmp= function into a key= function"""
    class K(object):
        __slots__ = ['obj']	#限定只有obj属性
        def __init__(self, obj):
            self.obj = obj
        def __lt__(self, other):
            '''自动把函数mycmp传入的实参作为为K类对象的属性值
            值的大小比较：就是K类对象的大小比较@比较关系运算符重载
            '''
            return mycmp(self.obj, other.obj) < 0	#只有当cmp函数返回-1：即a<b时，等价于K类 对象 => K-self小于K-other
        def __gt__(self, other):
            return mycmp(self.obj, other.obj) > 0
        def __eq__(self, other):
            return mycmp(self.obj, other.obj) == 0
        def __le__(self, other):
            return mycmp(self.obj, other.obj) <= 0
        def __ge__(self, other):
            return mycmp(self.obj, other.obj) >= 0
        __hash__ = None
    return K
```

在内部定义了一个类K， 并使用我传入的cmp函数完成了==比较关系运算符的重载 => 类与类之间可以进行大小的比较==

函数返回的是一个类，而sort函数的key需要的是一个函数，看起来矛盾，但==在python中，这样做完全可行，因为类和函数都是callable的==，这里把类当成了函数来用。



一开始lambda表达式生成的匿名函数返回的是元组的第一个元素进行大小比较

现在，==cmp_to_key返回的是类K==，参与比较的是K的对象，由于K已经实现了比较关系运算符重载，且算法就是我刚刚实现的cmp函数，这样就最终实现了自定义排序。



me|理解思路：调用lst.sort(key=cmp_to_key(cmp))时：==注意：额外的参数也就是列表的元素item==

1. 每一个item，也就是列表元素：即元组 => 基于cmp_to_key返回的是类K，变成了类K(属性值obj也就是该元组)
2. 由于.sort()函数  => 自动触发类K对象的比较
3. 类的对象的比较 => 自动触发比较关系运算符重载
4. 最终比较的结果：也就是函数cmp(x, y)
   - x是第一个类K的属性值obj，也就是第一个元组
   - y是第二个类K的属性值obj，也就是第二个元组





# .singledispatch

singledispatch是一个装饰器，将普通函数转为泛型函数

这种泛型函数将根据==第一个参数类型的不同==表现出不同的行为，被装饰的函数是默认实现，如果有其他功能可以使用register进行注册。

## 对比|使用if 语句处理逻辑分支

下面的代码：

1. 定义了Stu类，Police类
2. 定义了函数wake_up
3. 调用wake_up函数唤醒stu,police，和一个字符串

```python
class Stu(object):
    def __init__(self, name):
        self.name = name

    def wake_up(self):
        print('起床')

class Police(object):
    def __init__(self, name):
        self.name = name

    def wake_up(self):
        print('起床')

stu = Stu('小明')
police = Police('小明爸爸')

def wake_up(obj):
    if isinstance(obj, Stu):
        print('今天周末休息,让孩子们再睡一会')
    elif isinstance(obj, Police):
        print('警察很辛苦,又要起床了')
        obj.wake_up()
    else:
        print('不处理')

wake_up(stu)
wake_up(police)
wake_up('一个字符串')
```

在函数wake_up里，传入的参数类型是不一样的，函数要根据obj类型的不同来决定自己的行为，假设obj可能有10种类型，那么我就得在wake_up函数里用if ... elif 写10个逻辑分支来处理，这是一件非常痛苦的事情



## 对比|singledispatch

```python
from functools import singledispatch

@singledispatch
def wake_up(obj):
    print('不处理')


@wake_up.register(Stu)
def wake_stu(obj):
    print('今天周末休息,让孩子们再睡一会')


@wake_up.register(Police)
def wake_police(obj):
    print('警察很辛苦,又要起床了')
    obj.wake_up()


stu = Stu('小明')
police = Police('小明爸爸')

wake_up(stu)			#调用@wake_up.register(Stu)装饰的函数wake_stu(obj)  等价于wake_stu(stu)
wake_police(police)		#调用@wake_up.register(Police)装饰的函数wake_police(obj) 等价于wake_up(police)
wake_up('一个字符串')
```

wake_up被singledispatch装饰以后，变成了泛型函数

- 使用@wake_up.register注册一个类型来装饰一个函数
- 这里没有注册字符串，当第一个参数是没有被注册的类型时，==默认使用wake_up==来处理。





# .reduce

在2.x中是内置函数，到了python3以后移动到了functools 模块

reduce的作用是从左到右对一个序列的项累计地应用有两个参数的函数，以此合并序列到一个单一值

```python
from functools import reduce


lst = [2, 3, 4]
res = reduce(lambda x, y: x*y, lst)
print(res)
```

对reduce功能理解的关键在于==累计==，第一次计算时，传入到lambda函数中的两个参数是2和3，计算的结果是6，第二次传入lambda的是6和4，6是上一次的计算结果，会作为下一次计算的参数传入。

reduce的第一个参数可以是lambda函数，也可以是自定义函数。





# .wraps

可以让被装饰器装饰以后的函数保留原有的函数信息，包括函数的名称和函数的注释doc信息



## 背景|自省信息丢失

函数被装饰以后，一些原本属于自己的自省信息会丢失，先来看装饰前的样子

```python
def test(sleep_time):
    """
    测试装饰器
    :param sleep_time:
    :return:
    """
    time.sleep(sleep_time)


print(test.__name__)	#test
print(test.__doc__)
'''
    测试装饰器
    :param sleep_time:
    :return:
'''
```

被装饰后：变成了装饰函数的name和doc

- 比如一般的装饰函数名：warpper



## @wraps(func) 修复自省信息

```python
import time
from functools import wraps


def cost(func):
    @wraps(func)
    def warpper(*args, **kwargs):
        t1 = time.time()
        res = func(*args, **kwargs)
        t2 = time.time()
        print(func.__name__ + "执行耗时" +  str(t2-t1))
        return res
    return warpper

@cost
def test(sleep_time):
    """
    测试装饰器
    :param sleep_time:
    :return:
    """
    time.sleep(sleep_time)


print(test.__name__)
print(test.__doc__)
```





# 内置LRU缓存

lru_cache装饰器实现了LRU缓存算法

在软件或系统开发中，缓存总是必不可少，这是一种空间换时间的技术，==通过将频繁访问的数据缓存起来==，下一次访问时就可以快速获得期望的结果。

一个缓存系统，关键核心的指标就是缓存命中率，如果命中率很低，那么这个缓存除了浪费了空间，对性能的提升毫无帮助。



==LRU是一种常用的缓存算法==，即最近最少使用，如果一个数据在最近一段时间没有被访问到，那么在将来它被访问的可能性也很小， LRU算法选择将最近最少使用的数据淘汰，保留那些经常被命中的数据。

## functools.lru_cache

从python 3.2开始，就已经将LRU作为标准库的一部分发布，functools模块中的lru_cache装饰器实现了LRU缓存算法

以计算斐波那契数列为例，对比有缓存的算法效率和无缓存的算法效率：

```python
import time
from functools import lru_cache


@lru_cache()        # 测试无缓存时将本行注释掉
def fib_memoization(number: int) -> int:
    if number == 0: return 0
    if number == 1: return 1

    return fib_memoization(number-1) + fib_memoization(number-2)

start = time.time()
res = fib_memoization(33)
print(res)
print(f'耗时: {time.time() - start}s')
```

在有缓存的情况下，计算33的斐波那契数列耗时仅为0.00008秒

去掉装饰器后，耗时则为2.9秒

不过要强调一点，之所以有如此巨大的性能差距，只要原因是由于斐波那契算法的特殊性导致的。





lru_cache装饰器定义如下：

- 第一个参数规定缓存的数量
- 第二个参数如果设置为True，则严格检查被装饰函数的参数类型，默认为False

```python
def lru_cache(maxsize=128, typed=False):
    pass



from functools import lru_cache

@lru_cache(typed=False)
def add(x, y):
    print('add')
    return x + y


print(add(3, 4))
print(add(3, 4.0))
```

第二次调用add函数时参数是4.0

- 如果你认为这种情况可以使用缓存命中上一次3+4的结果，就将typed设置为False

- 如果你严格要求只有函数的参数完全一致时才能命中，那么将typed设置为True