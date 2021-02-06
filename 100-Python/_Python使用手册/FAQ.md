## 如何获取当前路径

当前路径可以用`'.'`表示，再用`os.path.abspath()`将其转换为绝对路径：

```python
# -*- coding:utf-8 -*-
# test.py

import os

print(os.path.abspath('.'))
```





## 如何获取当前模块的文件名

需要在终端运行.py文件(Jupyter notebook报错)

```python
# -*- coding:utf-8 -*-
# test.py

print(__file__)
```



## 如何获取命令行参数

`argv`的第一个元素永远是命令行执行的`.py`文件名。

```python
# -*- coding:utf-8 -*-
# test.py

import sys

print(sys.argv)

'''output
$ python3 test.py -a -s "Hello world"
['test.py', '-a', '-s', 'Hello world']
'''
```





## 如何获取当前Python命令的可执行文件路径

`sys`模块的`executable`变量就是Python命令可执行文件的路径：

```python
# -*- coding:utf-8 -*-
# test.py

import sys

print(sys.executable)
```

