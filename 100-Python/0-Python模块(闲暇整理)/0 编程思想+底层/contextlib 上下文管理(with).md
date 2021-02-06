读写文件这样的资源要特别注意，必须在使用完毕后正确关闭它们：

- try...finally

```python
try:
    f = open('/path/to/file', 'r')
    f.read()
finally:
    if f:
        f.close()
```

- with

```python
with open('/path/to/file', 'r') as f:
    f.read()
```

并不是只有`open()`函数返回的fp对象才能使用`with`语句。

实际上，任何对象，==只要正确实现了上下文管理==，就可以用于`with`语句



# 上下文管理 __`__enter__`和`__exit__`__

```python
class Query(object):

    def __init__(self, name):
        self.name = name

    def __enter__(self):
        print('Begin')
        return self
    
    def __exit__(self, exc_type, exc_value, traceback):
        if exc_type:
            print('Error')
        else:
            print('End')
    
    def query(self):
        print('Query info about %s...' % self.name)
```

这样我们就可以把自己写的资源对象用于`with`语句：

```python
with Query('Bob') as q:
    q.query()
```





# @contextmanager

编写`__enter__`和`__exit__`仍然很繁琐，因此Python的标准库`contextlib`提供了更简单的写法，上面的代码可以改写如下：

```python
from contextlib import contextmanager

class Query(object):

    def __init__(self, name):
        self.name = name

    def query(self):
        print('Query info about %s...' % self.name)

@contextmanager
def create_query(name):
    print('Begin')
    q = Query(name)
    yield q
    print('End')
```



`@contextmanager`这个decorator接受一个generator(函数create_query是一个生成器)，用`yield`语句把`with ... as var`把变量输出出去，然后，`with`语句就可以正常地工作了：

- 所以核心是`q = Query(name)`和`yield q`

```python
with create_query('Bob') as q:
    q.query()
```







很多时候，我们希望在某段==代码执行前后自动执行特定代码==，也可以用`@contextmanager`实现。例如：

```python
@contextmanager
def tag(name):
    print("<%s>" % name)	#目标代码执行前
    yield
    print("</%s>" % name)	#目标代码执行后

with tag("h1"):
    print("hello")
    print("world")
    
    
'''output
<h1>
hello
world
</h1>
'''
```

代码的执行顺序是：

1. `with`语句首先执行`yield`之前的语句，因此打印出`<h1>`；
2. `yield`调用==会执行`with`语句内部的所有语句==，因此打印出`hello`和`world`；
3. 最后执行`yield`之后的语句，打印出`</h1>`。

因此，`@contextmanager`让我们通过编写generator来简化上下文管理。





# @closing

如果一个对象没有实现上下文，我们就不能把它用于`with`语句。这个时候，可以用`closing()`来把该对象变为上下文对象。例如，用`with`语句使用`urlopen()`：

```python
from contextlib import closing
from urllib.request import urlopen

with closing(urlopen('https://www.python.org')) as page:
    for line in page:
        print(line)
```

`closing`也是一个经过@contextmanager装饰的generator，这个generator编写起来其实非常简单：

```python
@contextmanager
def closing(thing):
    try:
        yield thing
    finally:
        thing.close()
```

它的作用就是把任意对象变为上下文对象，并支持`with`语句。

---



`@contextlib`还有一些其他decorator，便于我们编写更简洁的代码。