# (ing...)Python-积累 | 实用技巧

## 一 查看状态

### 1 函数的所有方法

#### 1）内置函数

```
help(list)  #结果更加详细
?list   #弹出Docstring：编写函数文档

dir(list)

#OUTPUT#
['__add__',
 '__class__',
 '__contains__',
 '__delattr__',
 '__delitem__',
 '__dir__',
 '__doc__',
 '__eq__',
 '__format__',
 '__ge__',
 '__getattribute__',
 '__getitem__',
 '__gt__',
 '__hash__',
 '__iadd__',
 '__imul__',
 '__init__',
 '__init_subclass__',
 '__iter__',
 '__le__',
 '__len__',
 '__lt__',
 '__mul__',
 '__ne__',
 '__new__',
 '__reduce__',
 '__reduce_ex__',
 '__repr__',
 '__reversed__',
 '__rmul__',
 '__setattr__',
 '__setitem__',
 '__sizeof__',
 '__str__',
 '__subclasshook__',
 'append',
 'clear',
 'copy',
 'count',
 'extend',
 'index',
 'insert',
 'pop',
 'remove',
 'reverse',
 'sort']
```



### 2 提醒任务状态

#### 1）听觉：系统蜂鸣声提醒

```
import winsound
duration = 100  # millisecond
freq = 440  # Hz
for i in range(5):
    winsound.Beep(freq, duration*i)
```



## 二 CMD命令

以!开头(Jupyter Notebook)

```
# 以!开头
!python --version   #Python 3.7.6
```