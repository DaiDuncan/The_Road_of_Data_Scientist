实现了许多特定容器，这些容器在某些情况下可以替代内置容器 dict, list, tuple, set

1. [Counter--统计对象的个数](http://www.coolpython.net/python_senior/standard_module/collections_counter.html)
2. [ChainMap---合并多个字典](http://www.coolpython.net/python_senior/standard_module/collections_chainmap.html)
3. [OrderedDict---有序字典](http://www.coolpython.net/python_senior/standard_module/collections_ordereddict.html)
4. [defaultdict---不会引发KeyError的字典](http://www.coolpython.net/python_senior/standard_module/collections_defaultdict.html)
5. [deque---一个类似列表的容器](http://www.coolpython.net/python_senior/standard_module/collections_deque.html)
6. [namedtuple---有属性名称的元组](http://www.coolpython.net/python_senior/standard_module/collections_namedtuple.html)



# Counter 统计对象的个数

Counter类可以统计对象的个数， 它是字典的子类

## 操作1|创建

```python
from collections import Counter

c1 = Counter()      		# 创建一个空的Counter对象
c2 = Counter('hello world') # 从一个可迭代对象(列表,元组,字典,字符串)创建
c3 = Counter(a=3, b=4)      # 从一组键值对创建

print(c2)
print(c3)

''' output
Counter({'l': 3, 'o': 2, 'h': 1, 'e': 1, ' ': 1, 'w': 1, 'r': 1, 'd': 1})
Counter({'b': 4, 'a': 3})
'''


### 字符串方法.count()
word = 'hello world'
print(word.count('h'))  # 1
print(word.count('l'))  # 3
```

虽然列表，字符串都提供了count方法，但是想要==查看所有元素的数量==，却需要你逐个调用才能获取，而Counter类则要简单便捷的多。





## 操作2|查 选/索引(含切片) 改 增 删

### 访问缺失的键

虽然是字典的子类，但访问缺失的键时，不会引发KeyError, 而是返回0

```python
from collections import Counter

c1 = Counter()      # 创建一个空的Counter对象
print(c1['apple'])  # 0
```





### 计数器更新

- update方法用来新增计数

```python
from collections import Counter

c1 = Counter('hello world')
c1.update('hello')  # 使用另一个iterable对象更新
print(c1['o'])      # 3

c2 = Counter('world')
c1.update(c2)       # 使用另一个Counter对象更新
print(c1['o'])      # 4
```

- subtract方法用来减少计数

```python
from collections import Counter

c1 = Counter('hello world')
c1.subtract('hello')  # 使用另一个iterable对象更新
print(c1['o'])      # 1

c2 = Counter('world')
c1.subtract(c2)       # 使用另一个Counter对象更新
print(c1['o'])      # 0
```





### 键的删除

```python
from collections import Counter

c1 = Counter('hello world')
del c1['o']
print(c1['o'])      # 0
```





## 操作3|遍历 排序 统计

### elements()

返回一个迭代器，一个元素的计数是多少，在迭代器中就会有多少

```python
from collections import Counter

c1 = Counter('hello world')
lst = list(c1.elements())
print(lst)      # ['h', 'e', 'l', 'l', 'l', 'o', 'o', ' ', 'w', 'r', 'd']
```



### most_common([n])

返回top N的列表，列表里的元素是元组，如果计数相同，==排列无指定顺序==

如果不指定n， 则返回所有元素

```python
from collections import Counter

c1 = Counter('hello world')
print(c1.most_common(2))        # [('l', 3), ('o', 2)]
```



### 算术和集合操作

还支持+、-、&、|操作：

-  &和|操作分别返回两个Counter对象各元素的最小值和最大值

注意：通过算数和集合操作得到Counter对象将删除计数值小于1的元素

```python
from collections import Counter

c = Counter(a=1, b=3)
d = Counter(a=2, b=2)
print(c + d)  # Counter({'b': 5, 'a': 3})
print(c - d)  # Counter({'b': 1})
print(c & d)  # Counter({'b': 2, 'a': 1})
print(c | d)  # Counter({'b': 3, 'a': 2})
```







# ChainMap 合并多个字典

可以==将多个字典组合==成一个可更新的视图：在使用时, 允许你将多个字典视为一个字典, 这正是ChainMap的作用

- `ChainMap`本身也是一个dict，但是查找的时候，会按照顺序在内部的dict依次查找





### 遍历多个字典

```python
dic1 = {'python': 100}
dic2 = {'c++': 99}
```

假如要求你对他们进行遍历，你该如何操作呢？最粗暴的方法是用两个for循环分别对他们进行遍历，但这样做未免过于繁琐

```python
dic1 = {'python': 100}
dic2 = {'c++': 99}

for key, value in dic1.items():
    print(key, value)

for key, value in dic2.items():
    print(key, value)
```



ChainMap 并不是真的将字典合并在一起，那样的话会产生一个新的字典，使用ChainMap可以让我们以更简洁的代码遍历多个字典

```python
from collections import ChainMap

dic1 = {'python': 100}
dic2 = {'c++': 99}

dic = ChainMap(dic1, dic2)
print(type(dic))				#<class 'collections.ChainMap'>：需要注意的是ChainMap返回的对象类型并不是字典

for key, value in dic.items():
    print(key, value)			
```



原理：ChainMap 将多个字典保存到列表里，当你遍历dic时，它在内部循环这个列表，对每一个字典进行遍历

我们可以利用生成器实现类似的效果：

```python
dic1 = {'python': 100}
dic2 = {'c++': 99}


def pair_chain(*args):
    for dic in args:
        for key in dic:
            yield key, dic[key]	#生成器


for key, value in pair_chain(dic1, dic2):
    print(key, value)
```



### 应用|查找参数

应用程序往往都需要传入参数，参数可以通过命令行传入，可以通过环境变量传入，还可以有默认参数

可以用`ChainMap`实现参数的优先级查找：

- 先查命令行参数
- 如果没有传入，再查环境变量
- 如果没有，就使用默认参数

示例|如何查找`user`和`color`这两个参数：

```python
from collections import ChainMap
import os, argparse

# 构造缺省参数:
defaults = {
    'color': 'red',
    'user': 'guest'
}

# 构造命令行参数:
parser = argparse.ArgumentParser()
parser.add_argument('-u', '--user')
parser.add_argument('-c', '--color')
namespace = parser.parse_args()
command_line_args = { k: v for k, v in vars(namespace).items() if v }

# 组合成ChainMap:
combined = ChainMap(command_line_args, os.environ, defaults)

# 打印参数:
print('color=%s' % combined['color'])
print('user=%s' % combined['user'])
```



没有任何参数时，打印出默认参数：

```python
$ python3 use_chainmap.py 
color=red
user=guest
```

当传入命令行参数时，优先使用命令行参数：

```python
$ python3 use_chainmap.py -u bob
color=red
user=bob
```

同时传入命令行参数和环境变量，命令行参数的优先级较高：

```python
$ user=admin color=green python3 use_chainmap.py -u bob
color=green
user=bob
```





# OrderedDict 有序字典

在python3.6 之前，字典都是无序的

从python3.6开始，字典已经是有序的了，这里所说的有序，并不是按照key的大小进行排序，而是指插入的顺序

特别强调一点，这个特性不是python这门语言的特性，而是CPython的特性。



在python3.6之前，想要实现有序字典，就必须依靠OrderedDict

在3.6以后，则不再是必须,如果还有什么理由使用OrderedDict的话

- OrderedDict提供了move_to_end方法，可以将一个key-value移动到末尾
- 使用popitem方法可以==按照先进后出的原则(queue)==，删除最后加入的key-value对

```python
from collections import OrderedDict

order_dict = OrderedDict()

order_dict[1] = 1
order_dict['a'] = 2
order_dict['0'] = 3

for key, value in order_dict.items():
    print(key, value)

print("*"*20)
order_dict.move_to_end(1)

for key, value in order_dict.items():
    print(key, value)
```



`OrderedDict`可以实现一个FIFO（先进先出）的dict，当容量超出限制时，先删除最早添加的Key：

```python
from collections import OrderedDict

class LastUpdatedOrderedDict(OrderedDict):

    def __init__(self, capacity):
        super(LastUpdatedOrderedDict, self).__init__()
        self._capacity = capacity

    def __setitem__(self, key, value):
        containsKey = 1 if key in self else 0
        if len(self) - containsKey >= self._capacity:	#超过容量：删除旧key
            last = self.popitem(last=False)
            print('remove:', last)
        if containsKey:
            del self[key]
            print('set:', (key, value))
        else:
            print('add:', (key, value))
        OrderedDict.__setitem__(self, key, value)
```





# defaultdict 不会引发KeyError的字典

使用python标准模块提供的字典时,如果所用的key不存在, 就会引发KeyError异常

defaultdict是字典的子类

- 在创建这种字典时，必须传入一个工厂函数，比如int, float, list, set



defaultdict内部有一个`__missing__(key)`方法，当key不存在时，defaultdict会调用`__missing__`方法根据你所传入的工厂函数返回一个默认值

- 你传入的工厂函数是int，那么返回的就是0
- 如果传入的是list，返回的就是空列表。



两个特点：

- 不会引发KeyError
- 而且还能获取默认值

```python
### 统计列表里各个值出现的次数
lst = [1, 3, 4, 2, 1, 3, 5]


# 用普通的dict类来实现
count_dict = {}

for item in lst:
    if item not in count_dict:	#多了一个判断
        count_dict[item] = 0

    count_dict[item] += 1

print(count_dict)


# 使用defaultdict
from collections import defaultdict

count_dict = defaultdict(int)

for item in lst:
    count_dict[item] += 1

print(count_dict)
```





# deque 一个类似列表的容器

使用`list`存储数据时，按索引访问元素很快，但是插入和删除元素就很慢了，因为`list`是线性存储，数据量大的时候，插入和删除效率很低

- deque是为了高效实现插入和删除操作的双向列表，适合用于队列和栈

- `deque`除了实现list的`append()`和`pop()`外，还支持`appendleft()`和`popleft()`，这样就可以非常高效地往头部添加或删除元素





很多人把collections模块下的deque说成是双端队列，这是极不准确的，标准模块里对它的定义如下：list-like container with fast appends and pops on either end => 类似列表的容器：支持在两端快速的追加和删除元素

- append （在末尾追加数据）
- appendleft （在头部追加数据）
- pop （删除并返回指定索引的数据，默认是末尾数据）
- popleft（删除并返回头部数据）
- insert（在指定位置插入数据）
- remove（删除指定数据）

我个人对deque是这样理解的，它提供了及其灵活的功能，自身并不属于你所熟悉的任何一种数据结构，但我们可以在它的基础上非常轻松的实现数据结构，比如单向队列，双端队列，栈。

鉴于python标准库里已经实现了单向队列，所以本篇文章以deque为基础实现双端队列和栈这两种结构

## 实现双端队列

![image-20210126105633721](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210126105633.png)

```python
from collections import deque


class DoubleQueue():
    def __init__(self):
        self.queue = deque()

    def is_empty(self):
        """
        判断是否为空队列
        :return:
        """
        return len(self.queue) == 0

    def insert_front(self, data):
        """
        从队首插入数据
        :param data:
        :return:
        """
        self.queue.appendleft(data)

    def insert_rear(self, data):
        """
        从队尾插入数据
        :param data:
        :return:
        """
        self.queue.append(data)

    def delete_front(self):
        """
        从队首删除数据
        :return:
        """
        return self.queue.popleft()

    def delete_rear(self):
        """
        从队尾删除数据
        :return:
        """
        return self.queue.pop()

    def size(self):
        """
        返回队列长度
        :return:
        """
        return len(self.queue)


dq = DoubleQueue()

print(dq.is_empty())
dq.insert_front(4)
dq.insert_rear(5)
dq.insert_rear(6)

print(dq.delete_front())
print(dq.delete_rear())

print(dq.size())
```







## 实现栈

![image-20210126105705259](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210126105705.png)

```python
from collections import deque


class Stack:
    def __init__(self):
        self.stack = deque()

    def push(self, data):
        """
        入栈
        :param data:
        :return:
        """
        self.stack.append(data)

    def pop(self):
        """
        出栈
        :return:
        """
        return self.stack.pop()

    def size(self):
        """
        栈大小
        :return:
        """
        return len(self.stack)

    def is_empty(self):
        return self.size() == 0


stack = Stack()

stack.push(4)
stack.push(3)
stack.push(1)

print(stack.pop())
print(stack.size())
```





# namedtuple 有属性名称的元组

对元组里数据的访问==只能通过索引==来进行

假设我们想用元组来表示一个平面上的坐标：

```python
point = (3, 5)

# 输出x,y坐标
print(point[0], point[1])
```

namedtuple 允许我们创建有属性名称的元组，这样就可以通过属性名称来获取数据

```python
from collections import namedtuple

Point = namedtuple('Point', ['x_coord', 'y_coord'])
print(issubclass(Point, tuple))		#True：Point是tuple的子类
point = Point(3, 5)

# 输出x,y坐标：用属性来访问
print(point.x_coord, point.y_coord)	#3 5


### 验证：创建的Point对象是tuple的一种子类
>>> isinstance(p, Point)
True
>>> isinstance(p, tuple)
True
```



示例：圆

```python
# namedtuple('名称', [属性list]):
Circle = namedtuple('Circle', ['x', 'y', 'r'])
```



为什么不使用类？

```python
class Point():
    def __init__(self, x, y):
        self.x_coord = x
        self.y_coord = y

point = Point(3, 5)
# 输出x,y坐标
print(point.x_coord, point.y_coord)
```

注意：

1. 创建一个namedtuple对象，不比创建一个类简单
2. namedtuple 返回的对象本身就是一个类，这和直接创建类没有区别



真正的原因是元组，元组是不可变对象，因此元组可以做字典的key，可以存储到集合中

如果我们用自定义的普通类来替代namedtuple，==一旦需要做字典的key==，那么普通的类创建出的对象就无能为力了

而namedtuple创建的是tuple的子类，因此具有tuple的一切属性。

