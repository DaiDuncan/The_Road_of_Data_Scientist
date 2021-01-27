常规方法编写的装饰器：

- 很多人被装饰器的闭包概念搞的死去活来

```python
def cost(func):
    def warpper(*args, **kwargs):
        t1 = time.time()
        res = func(*args, **kwargs)
        t2 = time.time()
        print(func.__name__ + "执行耗时" +  str(t2-t1))
        return res
    return warpper

@cost
def test_cost(sleep):
    time.sleep(sleep)


test_cost(2)
```





# 基于decorator库

cost函数被decorator装饰，cost就变成了一个装饰器

- 第一个参数必须传入函数，就是你希望被cost装饰的函数
  - *args 和 **kw 是用来传给func的参数(func带参数)

```python
import time
from decorator import decorator


@decorator
def cost(func, *args, **kw):
    t1 = time.time()
    res = func(*args, **kw)
    t2 = time.time()

    print(func.__name__ + "执行耗时", t2-t1)
    return res

@cost
def test_cost(sleep):
    time.sleep(sleep)


test_cost(2)
```



如果想写一个带参数的装饰器(装饰器本身带参数)：

```python
import time
from decorator import decorator


@decorator	#说明cost()是装饰器
def cost(func, timelimit=3, *args, **kw):
    t1 = time.time()
    res = func(*args, **kw)
    t2 = time.time()
    # 只打印运行时长超过timelimit的函数
    if (t2-t1) > timelimit:
        print(func.__name__ + "执行耗时", t2-t1)
    return res

@cost
def test_cost(sleep):	#被cost装饰器所装饰
    time.sleep(sleep)


test_cost(3)
```



## me|理解

装饰器的格式比较固定，所以直接用@decorator来装饰也很容易理解



# #参考文献

[Link: 酷python|轻松编写装饰器](http://www.coolpython.net/python_senior/function/decorator_decorator.html)