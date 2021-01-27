

不用明确的指明base class

更重要的意义是：在多重继承的情境中，==确保正确调用MRO的next方法==

```
A init'ed
C
D
F
E
G
UserDependency init'ed
H: zum Ende
```

```python
print(F.__mro__)  #可以输入继承顺序
```

# #理解|范例

```python
class Base(object):
    def __init__(self):
        print "Base created"

class ChildA(Base):
    def __init__(self):
        Base.__init__(self)

class ChildB(Base):
    def __init__(self):
        super(ChildB, self).__init__()

ChildA() 
ChildB()
```

`super().__init__()`替代了`super(ChildB, self).__init__()`

```python
### Python3
class ChildB(Base):
    def __init__(self):
        super().__init__() 
        
        
### Python2
super(ChildB, self).__init__()
```





# 思考：如果没有了super()

## super()的实现

```python
### 参考在C中的实现
class ChildB(Base):
    def __init__(self):
        mro = type(self).mro()             # Get the Method Resolution Order.
        check_next = mro.index(ChildB) + 1 # Start looking after *this* class.
        while check_next < len(mro):
            next_class = mro[check_next]
            if '__init__' in next_class.__dict__:
                next_class.__init__(self)
                break
            check_next += 1
           
        
### 更接近python的风格
class ChildB(Base):
    def __init__(self):
        mro = type(self).mro()
        for next_class in mro[mro.index(ChildB) + 1:]: # slice to end
            if hasattr(next_class, '__init__'):
                next_class.__init__(self)
                break
```

如果没有super()对象，每一次根据MRO调用方法时，都要重写上述代码



为什么在python3中，不需要明确指出，直接用super()就可以了？

- 因为使用calling stack frame
- 找到class(隐式存储为\__class__)
- 第一个参数：包含MRO信息 => 确保正确调用MRO中的next方法



# 注意：细节

![image-20210119215846803](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210119215846.png)

```python
### 创建类
class Base(object):
    def __init__(self):
        print("Base init'ed")

class ChildA(Base):
    def __init__(self):
        print("ChildA init'ed")
        Base.__init__(self)

class ChildB(Base):
    def __init__(self):
        print("ChildB init'ed")
        super(ChildB, self).__init__()
        

### 创建依赖关系
class UserDependency(Base):
    def __init__(self):
        print("UserDependency init'ed")
        super(UserDependency, self).__init__()
        
        
### 基于上述类创建user
class UserA(ChildA, UserDependency):
    def __init__(self):
        print("UserA init'ed")
        super(UserA, self).__init__()

class UserB(ChildB, UserDependency):
    def __init__(self):
        print("UserB init'ed")
        super(UserB, self).__init__()
        
        
### 调用user
# UserA没有调用UserDependency方法
>>> UserA()
UserA init'ed
ChildA init'ed
Base init'ed
<__main__.UserA object at 0x0000000003403BA8>


>>> UserB()
UserB init'ed
ChildB init'ed
UserDependency init'ed	#注意：在UserA()中不包含UserDependency方法，因为只有ChildB用了super()
Base init'ed
<__main__.UserB object at 0x0000000003403438>
```

注意：

- 以上super(xxx, self) (Python2风格)转换为`super().__init__()`（Python3风格），结果一致
- 注意上述结果的print顺序



## me|理解

UserA()中的ChildA使用`Base.__init__(self)`到头了，相当于区别于MRO之外的断点

但是UserB()中的ChildB使用`super().__init__()`，MRO表格记录了顺序，按照规定的广度优先，先找UserDependency，最后才是回到Base(object)



### test

E不用super()

- D、F、G作为测试点：是否使用super()

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210119230404.png" alt="image-20210119230404827" style="zoom:80%;" />

```python
### 创建类
class H():
    def __init__(self):
        print("H: zum Ende")

class F(H):
    def __init__(self):
        print("F")
        super().__init__()
        #H.__init__(self)

class G(H):
    def __init__(self):
        print("G")
        super().__init__()
        #H.__init__(self)

class D(F):
    def __init__(self):
        print("D")
        super().__init__()
        #F.__init__(self)
        
class E(G):
    def __init__(self):
        print("E")
        super().__init__()
        #G.__init__(self)
        

### 创建依赖关系
class UserDependency(H):
    def __init__(self):
        print("UserDependency")
        super().__init__()
        
        
### 
class C(D, E):
    def __init__(self):
        print("C")
        super().__init__()

class A(C, UserDependency):
    def __init__(self):
        print("Begin")
        super().__init__()
        
        
>>> A()	#调用A()
```

| 测试点：不用super() | 打印结果                                   |
| ------------------- | ------------------------------------------ |
| 都是super()         | Begin C D F E G UserDependency H: zum Ende |
| C：父类D初始化      | Begin C D F E G UserDependency H: zum Ende |
| D                   | Begin C D F E G UserDependency H: zum Ende |
| E、D                | Begin C D F E G UserDependency H: zum Ende |
| E、F                | Begin C D ==F== H: zum Ende                |
| E、G                | Begin C D F E **G** H: zum Ende            |

猜测结论：

由于python解释器的动态编译 => 导致调用super()的有优先权





# #参考文献

[Link: Stackoverflow|如何理解super()](https://stackoverflow.com/questions/576169/understanding-python-super-with-init-methods)

[Link: 参考|知乎博客 super()](https://zhuanlan.zhihu.com/p/38471944)