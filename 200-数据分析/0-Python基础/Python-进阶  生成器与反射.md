# Python-进阶 | 生成器与反射

## 一 生成器 yield

```
def f():        #生成器函数
    print(11)
    yield 1

    print(22)
    yield 2

    print(33)
    yield 3

    print(44)
    yield 4

r = f()  # 这里只获取了一个生成器
ret = r.__next__()  #11,进入生成器函数f()，直到返回第一个yield的值
print(ret)          #1

ret = r.__next__()  #22，根据上一次的位置接着执行，直到返回第二个yield的值
print(ret)          #2
```



## 二 反射

反射就是根据**字符串的形式**去对象（某个模块）中操作其成员。

```
#index.py
inp = input("输入模块：")
print(inp, type(inp))   #inp是字符串类型

dd = __import__(inp)
inp2 = input("输入函数：")
func = getattr(dd,inp2)

ret = func()
print(ret)


# 注意：当模块与当前文件不在同一目录下的时候，需要添加fromlist=True，否则python就找不到commens模块
dd = __import__('lib.test.commens',fromlist=True)

============================================
# commens.py
def f1():
   return "F1"
```

### 1 getattr：获取

以字符串的形式，获取模块中的某函数

- 模块：commens
- 函数：f1111

```
import commens

target_func = getattr(commens, 'f1111', None)  # 加上None之后，如果模块中的函数不存在则直接返回None
print(target_func)
```

### 2 hasattr：判断

以字符串的形式，判断模块中是否存在某函数

```
import commens

r = hasattr(commens, 'f22')
print(r)    #False
```

### 3 setattr：创建

在内存中创建一个**函数**或**全局变量**，不会影响文件

```
import commens

r = hasattr(commens, 'ABC')
print(r)    #False

setattr(commens,'ABC',18)   在内存中创建一个全局变量
#setattr(commens,'ABC',lambda a : a+1)，也可以使用lambda创建函数
r = hasattr(commens, 'ABC')
print(r)    #True
```

### 4 delattr：删除

```
r = hasattr(commens, 'ABC')
print(r)    #True

delattr(commens,'ABC')

r = hasattr(commens, 'ABC')
print(r)    #False
```

### 示例：web访问的简单模拟

```
#index.py
from lib import account

url = input("输入url：")

inp = url.split('/')[-1]    #-1：获取url最后面的值

if hasattr(account, inp):
    target_func = getattr(account, inp)
    r = target_func()
    print(r)
else:
    print("404")


#account.py
def login():
    return "login"
    
def logout():
    return "logout"

=======================================
#简单的web框架逻辑
url = input('请输入"模块/函数":')

target_model,target_func = url.split('/')
m = __import__("lib."+ target_model,fromlist=True)

if hasattr(m, target_func):
    func = getattr(m,target_func)
    r = func()
    print(r)
else:
    print("404")
```

## 参考文献

[[转摘\]cnblogs | 生成器与反射](https://www.cnblogs.com/whatisfantasy/p/6024640.html)