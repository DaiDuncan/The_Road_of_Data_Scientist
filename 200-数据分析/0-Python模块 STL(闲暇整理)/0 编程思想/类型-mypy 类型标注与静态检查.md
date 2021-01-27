# 动态类型与静态类型

python作为一种动态类型语言，这使得程序不需要指定变量的类型

但python创始人Guido van Rossum在python3.5中引入了一个类型系统，它允许开发人员指定变量类型，主要作用是便于开发维护代码，==供IDE和开发工具使用==，对代码运行不产生任何影响，运行时会过滤类型信息。



python在构建大型项目上一直遭人诟病，除了自身性能在个别领域不尽如人意外，动态类型的语言特点也使得python并不适合构建大型项目



其他强类型语言诸如C++, java 在定义变量时必须指定其类型，因此在维护代码方面，即使缺少注释也仍然可以通过阅读代码来获取变量的信息

而python不可以，如果python开发人员不注意变量命名规则，也不喜欢写注释 => python的代码真的很难理解和维护，尤其在使用了一大堆诸如装饰器等特性之后，理解起来更是难上加难





# 类型标注

```python
def add(x: int, y: int)->int:
    return x + y


print(add(3, 4.3))
```

类型标注的使用并不复杂，只有两条规则：

1. 使用：语句将信息附加到==变量或函数参数==中
2. ->运算符用于将信息附加到==函数/方法的返回值==中



通过类型标注，你很容易就了解了使用add函数时的参数要求以及函数返回值的类型，不需要添加其他注释，而这并没有增加太多工作量。尽管类型标注注明了参数y是int类型，但实际调用函数时仍然可以传入float数据，因为类型标注仅仅被当做注释来处理，而不是强制的类型要求

python并没有据此进行类型推断和验证，也没有使用类型信息来优化生成的字节码以获得安全性或性能，一言以蔽之，==类型标注就是一个另类的代码注释==。





## 意义

1  易于理解代码

在代码旁边陈述了所有参数的类型和返回类型，可以大大加快了理解这个代码片段的过程。



2 易于重构

我所使用的DIE是pycharm，在阅读函数out_stu时，由于标注了stu的类型是Stu，我可以通过command + 左键跟踪代码到类Stu，这就为我理解函数内部对stu处理增加了帮助



换做以前，你只能通过代码注释来了解stu是什么类型的数据，如果不幸，类的定义不在这个脚本里，也没有代码注释，你将无从下手



pycharm一直有这样的代码定位功能，但此前由于缺少类型说明，函数的参数无法使用该功能定位到这个参数的类型。有了类型标注，可以让IDE具有100%的检测准确率。

```python
class Stu:
    def __init__(self, name: str, age: int):
        self.name = name
        self.age = age


def out_stu(stu: Stu):
    print(stu.name, stu.age)

stu = Stu('小明', 14)
out_stu(stu)
```





3 易于使用库

大多数编辑器都提供了代码提示功能，由于python是动态类型语言，有些代码，无法确定变量的类型，这时他们就无法提供代码提示，这一点在编写函数尤为明显

由于函数的参数具体是什么类型的，只有在执行阶段才能确定，因此编辑无法在你编写函数时为你提供代码提示功能，但加了类型标注之后就变成了可能



![image-20210125164359554](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210125164359.png)

由于已经加了类型标注，编辑器知道参数string是str类型 => 因此在编写代码时就可以为我们提供字符串可用的方法列表，这样可以加快代码编写的速度





# mypy: 静态类型检查

mypy就是一个利用类型注解对python代码进行静态类型检查的工具

## 示例|有问题的代码

下述代码在执行时不会报任何错误，但严格来讲是存在问题的，在创建Stu实例时，age参数应该传入int类型数据。

但由于python是动态类型语言，因此，传入float或者字符串都不会引发错误，除非在后续的属性使用中对类型有明确要求。这样的代码是不安全的，在程序运行前，可以通过静态类型检查来发现问题，这需要类型标注的帮助

```python
class Stu:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def __str__(self):
        return '{name} {age}'.format(name=self.name, age=self.age)


stu1 = Stu('小明', 16.5)
stu2 = Stu('小刚', '17')

print(stu1, stu2)
```



## 添加类型标注并使用mypy检查

```python
class Stu:
    def __init__(self, name: str, age: int):
        self.name = name
        self.age = age

    def __str__(self):
        return '{name} {age}'.format(name=self.name, age=self.age)


stu1 = Stu('小明', 16.5)	##编辑器提示潜在问题
stu2 = Stu('小刚', '17')	#编辑器提示潜在问题

print(stu1, stu2)
```

仅仅是添加了类型标注，我所使用的pycharm就已经提示我创建Stu实例时的age参数有问题



```shell
mypy demo.py

demo.py:10: error: Argument 2 to "Stu" has incompatible type "float"; expected "int"
demo.py:11: error: Argument 2 to "Stu" has incompatible type "str"; expected "int"
Found 2 errors in 1 file (checked 1 source file)
```