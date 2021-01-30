@回顾 python-STL 基于Pycharm的调式



很多场景下，却不具备这样的条件。比如所使用的编辑器是Sublime Text,或者Vim，一些身经百战的开发人员会使用print语句通过输出变量内容的方式进行调试。



# 

在那些无法使用编辑器内置调试器的场景下，建议你使用ipdb来调试你的代码。ipdb可以看做是python内置调试器pdb的增强版本，依赖于IPtyhon,提供了补全、语法高亮等功能，使用pip来安装

```text
pip install ipdb
```



# 设置断点

可以直接在程序里设置断点

```python
import ipdb
import random

ipdb.set_trace()	#直接在程序里设置断点


lst = []
for i in range(10):
    a = random.randint(0, 100)
    lst.append(a)

res = sum(lst)
print(res)
```

执行程序，会在断点出停下来，进入到调试界面

在代码中通过ipdb.set_trace()来设置断点并不是唯一的方式，而且我不建议你这样做，原因在于调试完成以后，你不得不再去删除这行代码，推荐你使用交互式的命令式调试， 将上面代码中和ipdb有关的代码去掉

```python
import random

lst = []
for i in range(10):
    a = random.randint(0, 100)
    lst.append(a)

res = sum(lst)
print(res)


# 在终端里执行命令
python3 -m ipdb demo.py

# 进入调试界面后输入
----> 1 import random
      2
      3 lst = []

ipdb> b 8
Breakpoint 1 at /Users/kwsy/kwsy/coolpython/demo.py:8
ipdb> c
ipdb> lst
[76, 36, 83, 43, 94, 33, 28, 30, 80, 77]
ipdb> lst=[1, 2, 3]
ipdb> lst
[1, 2, 3]
```

命令b是设置断点，b 8 表示在第8行设置断点，c命令一直执行至断点处

- 在断点处，你可以随意的查看变量的值，甚至可以修改变量的值.





# 命令介绍

##1 h 帮助命令

h是帮助命令，可以列出所有的命令

```python
ipdb> h

Documented commands (type help <topic>):
========================================
EOF    cl         disable  interact  next    psource  rv         unt
a      clear      display  j         p       q        s          until
alias  commands   down     jump      pdef    quit     source     up
args   condition  enable   l         pdoc    r        step       w
b      cont       exit     list      pfile   restart  tbreak     whatis
break  continue   h        ll        pinfo   return   u          where
bt     d          help     longlist  pinfo2  retval   unalias
c      debug      ignore   n         pp      run      undisplay

Miscellaneous help topics:
==========================
exec  pdb
```



想知道某个具体命令的作用，可以使用h命令来查看

```text
ipdb> h cl
cl(ear) filename:lineno
cl(ear) [bpnumber [bpnumber...]]
        With a space separated list of breakpoint numbers, clear
        those breakpoints.  Without argument, clear all breaks (but
        first ask confirmation).  With a filename:lineno argument,
        clear all breaks at that line in that file.
```







##2 b 断点相关命令

该命令==默认在当前脚本==里执行，如果你想在别的脚本里设置断点，则需要提供脚本的名称

```text
b file_name:line_number
```

file_name必须在sys.path中，当前目录默认在sys.path， 你也可以通过..来引用上一层目录

已经设置过的断点在重新运行debug程序时仍然保留，你可以使用disable关闭这些断点，使用enable打开这些断点，如果想清楚，可以使用clear或者cl命令





## 3 执行代码

c命令会执行到下一处断点

s(step) 和 n(next)会逐行执行代码

- 遇到函数时，s会进入到函数内部，而n命令不会。

进入到函数以后：

- a命令可以查看函数的入参
- r可以直接执行到return语句









## 4 j 忽略某个代码段

j line_number 可以忽略某段代码，下一步直接从 line_number 开始执行



## 5 查看源码

调试界面里默认是当前往后 11 行，这样可能会影响你的调试，使用l或者ll命令可以查看源码

- l是查看整个源码，你可以指定某一行代码，也可以指定一个范围 l 5, 10，这样就只看5到10行的代码





## 6 重启或退出

调试过程中，如果你想重新开始，可以使用restart命令或者run命令，但是要注意，此前设置的断点都生效

如果你想要一个全新的调试，使用命令 q、quit 或 exit 退出，然后重新启动debugger