sys模块里存放的都是==和解释器相关的功能==



# sys.version

获取python的版本

```python
>>> import sys
>>> sys.version
'3.6.6 (v3.6.6:4cf1f54eb7, Jun 26 2018, 19:50:54) \n[GCC 4.2.1 Compatible Apple LLVM 6.0 (clang-600.0.57)]'
```



# sys.stdin, sys.stdin, sys.stderr

与解释器的标准输入，输出和错误流相对应的文件对象

这三个文件对象足以专门写一篇教程

```python
import sys

sys.stdout = open('test', 'a')
print('ok')
```

sys.stdout是标准输出流，上面的代码将标准输出流重定向到一个打开的文件中，这样==print函数在执行时，就不会在终端输出内容==，而是在将内容写入到文件中



# sys.modules

一个全局字典，该字典是python启动后就加载在内存中。

每当程序员导入新的模块，sys.modules将自动记录该模块。

当第二次再导入该模块时，python会直接到字典中查找，从而加快了程序运行的速度





# sys.path

获取指定模块搜索路径的列表，当你在程序中使用import关键字引入一个模块时，解释器会按照一定的顺序来查找这个模块

- 1 当前目录
- 2 如果不在当前目录，Python 则搜索在 shell 变量 PYTHONPATH 下的每个目录。
- 3 如果都找不到，Python会察看默认路径。UNIX下，默认路径一般为/usr/local/lib/python/

模块搜索路径存储在 system 模块的 sys.path 变量中。变量里包含当前目录，PYTHONPATH和由安装过程决定的默认目录

```python
import sys

for i in sys.path:        
    print(i)            #查看python环境变量列表

#OUTPUT#
D:\D\test_file\jupyter
D:\JustPractice\1_MS_Geek_salute\PythonAnaconda\python37.zip
D:\JustPractice\1_MS_Geek_salute\PythonAnaconda\DLLs
D:\JustPractice\1_MS_Geek_salute\PythonAnaconda\lib
D:\JustPractice\1_MS_Geek_salute\PythonAnaconda

D:\JustPractice\1_MS_Geek_salute\PythonAnaconda\lib\site-packages
D:\JustPractice\1_MS_Geek_salute\PythonAnaconda\lib\site-packages\win32
D:\JustPractice\1_MS_Geek_salute\PythonAnaconda\lib\site-packages\win32\lib
D:\JustPractice\1_MS_Geek_salute\PythonAnaconda\lib\site-packages\Pythonwin
D:\JustPractice\1_MS_Geek_salute\PythonAnaconda\lib\site-packages\IPython\extensions
C:\Users\Stein_2\.ipython
```





# sys.exit()

程序执行到代码末尾，解释器会自动退出，如果你希望中途退出程序，可以使用sys.exit(n)方法

n可以传入一个整数，你可以在主程序中捕获对sys.exit()的调用并获得这个n

一般0表示正常退出，==其他为异常==

```python
import sys

def test():
    sys.exit(3)

try:
    test()
except SystemExit as e:
    print(e)	#程序输出结果是 3
```

```python
import sys

choice = input("wanna quit?")
if choice == 'y' or choice == 'Y':
    sys.exit("Good Bye！")        #等同于exit("Good Bye！")

#OUTPUT#
wanna quit?y
Good Bye！

Process finished with exit code 1
```





# sys.argv⭐

```python
import sys
print(sys.argv) #执行.py脚本时传递的参数获取到了
 
#OUTPUT#
$ python test.py helo world
['test.py', 'helo', 'world']  
```







# sys.platform

返回操作系统平台名称

```python
import sys

#MacOS
print(sys.platform) #darwin

#Win7
print(sys.platform) #win32
```





# sys.stdout.write()⭐

在屏幕上输出 进度条

```python
#进度条的简单实现
import sys
import time

for i in range(101):
    sys.stdout.write("\r"+"%s%%\t"%(i)+"#"*i)
    sys.stdout.flush()
    time.sleep(0.05)
```

