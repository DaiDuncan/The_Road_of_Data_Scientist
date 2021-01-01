# Python-进阶 | 异常处理

## 一 异常

### 1 异常处理

```
# 没有异常处理
num = int('abc')
print(num)          #报错

# 异常处理
try:
    num = int('abc')      #try里的代码是受保护的
    print(num)
except Exception as e:
    print(e)            #输出invalid literal for int() with base 10: 'abc'，程序正常运行
```

e是由**Exception类 实例化**的一个对象

```
class Exception(BaseException):
    """ Common base class for all non-exit exceptions. """
    def __init__(self, *args, **kwargs): # real signature unknown
        pass

    @staticmethod # known case of __new__
    def __new__(*args, **kwargs): # real signature unknown
        """ Create and return a new object.  See help(type) for accurate signature. """
        pass
```



### 2 异常分类

Exception是万能的异常捕捉方法，可以捕捉到任何错误。

```
li = []
inp = input('请输入内容：')
try:
    li = []
    li[999]

except IndexError as e:  # 特殊异常需要优先匹配
    print('索引错误')
except Exception as e:
    print(e)
```

| 常见异常          |                                                              |
| ----------------- | ------------------------------------------------------------ |
| AttributeError    | 试图访问一个对象没有的树形，比如foo.x，但是foo**没有属性x**  |
| IOError           | 输入/输出异常；基本上是无法打开文件                          |
| ImportError       | 无法引入模块或包；基本上是路径问题或名称错误                 |
| IndentationError  | 语法错误（的子类） ；代码没有正确对齐                        |
| IndexError        | 下标索引超出序列边界，比如当x只有三个元素，却试图访问x[5]    |
| KeyError          | 试图访问**字典里不存在的键**                                 |
| KeyboardInterrupt | Ctrl+C被按下                                                 |
| NameError         | 使用一个还未被赋予对象的变量                                 |
| SyntaxError       | Python代码非法，代码不能编译(个人认为这是语法错误，写错了）  |
| TypeError         | 传入对象类型与要求的不符合                                   |
| UnboundLocalError | 试图访问一个**还未被设置的局部变量**，基本上是由于另有一个同名的全局变量，导致你以为正在访问它 |
| ValueError        | 传入一个调用者不期望的值，即使值的类型是正确的               |

Python的错误其实也是class，**所有的错误类型都继承自BaseException**，所以在使用except时需要注意的是，它不但捕获该类型的错误，还把其子类也“一网打尽”。比如：

```
try:
    foo()
except ValueError as e:
    print('ValueError')
except UnicodeError as e:
    print('UnicodeError')
```

第二个except永远也捕获不到UnicodeError，因为UnicodeError是ValueError的子类，如果有，也被第一个except给捕获了。



使用try...except捕获错误还有一个巨大的好处，就是可以跨越多层调用，比如函数main()调用foo()，foo()调用bar()，结果bar()出错了，这时，**只要main()捕获到了**，就可以处理。

也就是说，不需要在每个可能出错的地方去捕获错误，只要**在合适的层次去捕获错误**就可以了。

这样一来，就大大减少了写try...except...finally的麻烦。

```
def foo(s):
    return 10 / int(s)

def bar(s):
    return foo(s) * 2

def main():
    try:
        bar('0')
    except Exception as e:
        print('Error:', e)
    finally:
        print('finally...')
```



### 3 异常实例

```
#IndexError
dic = ["wupeiqi", 'alex']
try:
    dic[10] #下标越界
except IndexError, e:
    print e

#KeyError
dic = {'k1':'v1'}
try:
    dic['k20']  #不存在的key
except KeyError, e:
    print e
    
#ValueError
s1 = 'hello'
try:
    int(s1) #字符串无法转化为int
except ValueError, e:
    print e
```

### 4 实用 | 异常处理的完整结构

```
try:
    # 主代码块
    pass
except KeyError as e:
    # 异常时，执行该块
    pass
else:
    # 主代码块执行完，执行该块
    pass
finally:
    # 无论异常与否，最终执行该块
    pass
```

### 5 实用 | 主动触发异常 raise

```
try:
    raise Exception('错误了。。。')
except Exception as e:
    print e
```

### 6 实用 | 自定义异常

```
class WupeiqiException(Exception):
 
    def __init__(self, msg):
        self.message = msg
 
    def __str__(self):
        return self.message
 
try:
    raise WupeiqiException('我的异常')
except WupeiqiException,e:
    print e
```

### 7 实用 | 断言assert：单元测试

```
# assert 条件
assert 1 == 1   #条件成立程序正常运行
assert 1 == 2   #不成里就会报错，程序就异常
```

## 参考文献

[[转摘\]cnblogs | python异常处理](https://www.cnblogs.com/whatisfantasy/p/6038360.html)

[官方文档 | 常见的错误类型和继承关系](https://docs.python.org/3/library/exceptions.html#exception-hierarchy)