如果我们要操作文件、目录，可以在命令行下面输入操作系统提供的各种命令来完成。比如`dir`、`cp`等命令。

- 本质：操作系统提供的命令只是简单地调用了操作系统提供的接口函数

如果要在Python程序中执行这些目录和文件的操作怎么办？



os模块存放和系统相关的功能



| 方法：使用时带上()                      | 备注                                                         |
| --------------------------------------- | ------------------------------------------------------------ |
| ==os.getcwd()==                         | 获取当前工作目录，即当前python脚本工作的目录路径             |
| os.getuid()                             | 返回当前进程的有效用户 id                                    |
| os.getpid()                             | 函数返回当前进程的 id                                        |
| os.getppid                              | 返回父进程的 id                                              |
| ==os.getenv()==                         | 读取环境变量                                                 |
| os.uname                                | 函数返回识别操作系统的不同信息                               |
| ==os.name==                             | 字符串指示当前使用平台。win->'nt'; Linux->'posix'            |
| ==os.chdir("dirname")==                 | 改变当前脚本工作目录；相当于shell下cd                        |
| os.curdir                               | 返回当前目录: ('.')                                          |
| os.pardir                               | 获取当前目录的父目录字符串名：('..')                         |
| ==os.makedirs('dir1/dir2')==            | 可生成多层递归目录                                           |
| ==os.removedirs('dirname1')==           | 若目录为空，则删除，并递归到上一级目录，如若也为空，则删除，依此类推 |
| ==os.mkdir('dirname')==                 | 生成单级目录；相当于shell中mkdir dirname                     |
| ==os.rmdir('dirname')==                 | 删除单级空目录，若目录不为空则无法删除，报错；相当于shell中rmdir dirname |
| ==os.listdir('dirname')==               | 列出指定目录下的所有文件和子目录，包括隐藏文件，并以列表方式打印 |
| os.remove()                             | 删除一个文件                                                 |
| ==os.rename("oldname","new")==          | 重命名文件/目录                                              |
| **os.stat('path/filename')**            | 获取文件/目录信息                                            |
| ==os.sep==                              | 操作系统特定的路径分隔符，win下为"\\\\",Linux下为"/"         |
| os.linesep                              | 当前平台使用的行终止符，win下为"\t\n",Linux下为"\n"          |
| ==os.pathsep==                          | 用于分割文件路径的字符串                                     |
| os.system("bash command")               | 运行shell命令，直接显示                                      |
| os.environ                              | 获取系统环境变量                                             |
| os.path.abspath(path)                   | 返回path规范化的绝对路径                                     |
| os.path.split(path)                     | 将path分割成==目录==和==文件名==二元组返回                   |
| **os.path.dirname(path)**               | 返回path的目录。其实就是os.path.split(path)的第一个元素      |
| os.path.basename(path)                  | 返回path最后的文件名。如何path以／或\结尾，那么就会返回空值。即os.path.split(path)的第二个元素 |
| **os.path.exists(path)**                | 如果path存在，返回True；如果path不存在，返回False            |
| **os.path.isabs(path)**                 | 如果path是绝对路径，返回True                                 |
| **os.path.isfile(path)**                | 如果path是一个存在的文件，返回True。否则返回False            |
| **os.path.isdir(path)**                 | 如果path是一个存在的目录，则返回True。否则返回False          |
| **os.path.join(path1[, path2[, ...]])** | 将多个路径组合后返回，第一个绝对路径之前的参数将被忽略       |
| os.path.getatime(path)                  | 返回path所指向的文件或者目录的最后存取时间                   |
| os.path.getmtime(path)                  | 返回path所指向的文件或者目录的最后修改时间                   |



# 系统操作相关

```python
>>> import os
>>> os.sep
'/'
>>> os.name
'''
如果是posix，说明系统是Linux、Unix或Mac OS X
如果是nt，就是Windows系统
'''
'posix'	

>>> os.uname()	#获取详细的系统信息：在Windows上不提供，也就是说，os模块的某些函数是跟操作系统相关的

>>> os.getenv('PATH')
'/Library/Frameworks/Python.framework/Versions/3.6/bin:/usr/local/var/pyenv/shims:/opt/local/bin:/opt/local/sbin:/opt/local/sbin:/opt/local/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/opt/X11/bin:/Users/kwsy/Qt5.5.0/5.5/clang_64/bin:/usr//bin/ChromeDriver:/Users/kwsy/.rvm/bin:/Users/kwsy/.rvm/bin:/usr/bin/:/Users/kwsy/rc_tool/rd_tools/arcanist/bin'
>>> os.getcwd()
'/Users/kwsy/PycharmProjects/pythonclass/mytest'
```

```python
# 注意：os.path.dirname(filename) + os.path.basename(filename) == filename

# __file__是当前脚本运行的路径@CMD命令脚本 \
# 但是也会分不同的情况：如果执行命令时使用绝对路径，__file__就是脚本的绝对路径 \
# 如果使用的是相对路径，__file__就是脚本的相对路径 \
# 如果在交互式环境中，则会爆出异常。因为此时__file__并未生成

import os

print(os.path.dirname(__file__))    #/Users/morra/Desktop/python/lib
print(os.path.basename(__file__))   #index.py


tmp = os.path.dirname(__file__)

print(os.path.dirname(tmp))         #/Users/morra/Desktop/python/
print(os.path.basename(tmp))        #lib，返回index.py的上一级目录


#1 新建文件夹 
if not os.path.isdir(path_out):
   os.makedirs(path_out)

#2 遍历所有文件和子文件夹
for a, b, filenames in os.walk(path_data):
   for filename in filenames:

#3 只遍历当前文件，不包含子文件夹
for a, b, filenames in os.walk(path_data):
   for filename in filenames:
       if a == path_data:
```



# 环境变量

```python
>>> os.environ
environ({'VERSIONER_PYTHON_PREFER_32_BIT': 'no', 'TERM_PROGRAM_VERSION': '326', 'LOGNAME': 'michael', 'USER': 'michael', 'PATH': '/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin:/opt/X11/bin:/usr/local/mysql/bin', ...})
```



要获取某个环境变量的值，可以调用`os.environ.get('key')`：

```pascal
>>> os.environ.get('PATH')
'/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin:/opt/X11/bin:/usr/local/mysql/bin'

>>> os.environ.get('x', 'default')
'default'
```











# 文件/目录操作

备注：目录和文件夹是同一个意思

对文件夹进行增删改查操作

- 一部分放在`os`模块中
- 一部分放在`os.path`模块中



@查看、创建和删除目录：

```python
# 查看当前目录的绝对路径:
>>> os.path.abspath('.')
'/Users/michael'

# 在某个目录下创建一个新目录，首先把新目录的完整路径表示出来:
>>> os.path.join('/Users/michael', 'testdir')
'/Users/michael/testdir'
# 然后创建一个目录:
>>> os.mkdir('/Users/michael/testdir')

# 删掉一个目录:
>>> os.rmdir('/Users/michael/testdir')
```



## os模块

```python
import os

### 获取指定目录下的所有文件和目录名，如果不指定目录，则目录默认为当前所在目录
os.listdir('.')				#当前路径
os.listdir('/Users/kwsy/')


### 创建一个目录（文件夹）
os.mkdir('test')


### 生成多层递归目录，这个方法与mkdir一样，都可以创建目录，不同之处在于mkdir只能创建一层目录，而makedirs可以创建多层。比如你想在目录A下创建一个目录B，用着两个方法都可以，但如果想在A目录下创建一个多层目录C/D/E， 就只能用os.makedirs().它会先创建C，然后在C下面创建D，在D下面创建E
os.makedirs('test/demo/demo2')


### 删除文件
os.remove('test.py')

### 删除一个目录，如果目录里有文件，则无法删除
os.rmdir('test')


### 递归删除多层目录，但若干目录中有文件则无法删除
os.removedirs('test/demo/demo2')


### 改变当前目录，切换到其他目录，和linux系统下的cd 命令是同样的效果
os.chdir()


### os.rename()对文件重命名
os.rename('demo.py', 'demo2.py')	#将文件demo.py修改为demo2.py
```

注意：没有复制文件的函数 => 原因是复制文件并非由操作系统提供的系统调用

理论上讲，我们通过上一节的读写文件可以完成文件复制，只不过要多写很多代码。



### shutil模块：os模块的补充

`shutil`模块提供了`copyfile()`的函数，你还可以在`shutil`模块中找到很多实用函数，它们可以看做是`os`模块的补充。



## os.path模块

os.path提供了对文件和目录更加强大的操作

| 方法名                 | 功能作用                                                     |
| :--------------------- | :----------------------------------------------------------- |
| os.path.exists(path)   | 判断path是否存在，path既可以是目录，也是是文件，如果存在返回True，反之返回False |
| os.path.isfile(path)   | 判断path是否是文件，如果是返回True                           |
| os.path.isdir(path)    | 判断path是否是目录，如果是返回True                           |
| os.path.isabs(path)    | 判断路径是否是绝对路径                                       |
| os.path.basename(path) | 获取文件名                                                   |
| os.path.dirname(path)  | 获取路径名                                                   |
| os.path.getsize(path)  | 获取文件大小                                                 |
| os.path.getatime(path) | 获取最近访问时间                                             |
| os.path.getctime(path) | unix是最新的元数据更改的时间,windows系统下是文件创建时间     |
| os.path.getmtime(path) | 获取文件内容最近修改时间                                     |
| os.path.abspath(path)  | 获取绝对路径                                                 |
| os.path.split(path)    | 分割路径                                                     |
| os.path.splitext(path) | 分割文件名，返回由文件名和==扩展名==组成的元组               |
| os.path.join()         | 拼接路径                                                     |

```python
import os

path = '/Users/kwsy/kwsy/coolpython/test.py'

print(os.path.basename(path))       # 获取文件名
print(os.path.dirname(path))        # 获取路径名
print(os.path.getsize(path))        # 获取文件大小
print(os.path.getatime(path))       # 获取最近访问时间
print(os.path.getctime(path))       # unix是最新的元数据更改的时间,windows系统下是文件创建时间
print(os.path.getmtime(path))       # 获取文件内容最近修改时间
print(os.path.abspath(path))        # 获取绝对路径
print(os.path.split(path))          # 分割路径
print(os.path.splitext(path))       # 分割文件名，返回由文件名和扩展名组成的元组
print(os.path.join('/Users/kwsy/kwsy/coolpython', 'test.py'))


### output
test.py
/Users/kwsy/kwsy/coolpython
707
1575863105.0
1575862477.0
1575862477.0
/Users/kwsy/kwsy/coolpython/test.py
('/Users/kwsy/kwsy/coolpython', 'test.py')
('/Users/kwsy/kwsy/coolpython/test', '.py')
/Users/kwsy/kwsy/coolpython/test.py
```





把两个路径合成一个时，不要直接拼字符串，而要通过`os.path.join()`函数，这样可以正确处理不同操作系统的路径分隔符

在Linux/Unix/Mac下，`os.path.join()`返回这样的字符串：

```
part-1/part-2
```

而Windows下会返回这样的字符串：

```
part-1\part-2
```





同样的道理，要拆分路径时，也不要直接去拆字符串，而要通过`os.path.split()`函数，这样可以把一个路径拆分为两部分，后一部分总是==最后级别的目录或文件名==：

```python
>>> os.path.split('/Users/michael/testdir/file.txt')
('/Users/michael/testdir', 'file.txt')
```



`os.path.splitext()`可以直接让你得到文件扩展名

```python
>>> os.path.splitext('/path/to/file.txt')
('/path/to/file', '.txt')
```

这些合并、拆分路径的函数并不要求目录和文件要真实存在，它们只对字符串进行操作。







## 应用|基于python特性操作文件

要列出当前目录下的所有目录，只需要一行代码：

```python
>>> [x for x in os.listdir('.') if os.path.isdir(x)]
['.lein', '.local', '.m2', '.npm', '.ssh', '.Trash', '.vim', 'Applications', 'Desktop', ...]
```



要列出所有的`.py`文件，也只需一行代码：

```python
>>> [x for x in os.listdir('.') if os.path.isfile(x) and os.path.splitext(x)[1]=='.py']
['apis.py', 'config.py', 'models.py', 'pymonitor.py', 'test_db.py', 'urls.py', 'wsgiapp.py']
```





## 实用技巧|提取相对路径

原理：用绝对路径，截断根目录的路径，就得到了相对路径

### 方法1：字符串替换（用字符串函数）

```python
import os
 
print('==========1===========')
abspath = os.getcwd()  				# 获取当前路径
rootpath = os.path.abspath('..')  	# 获取上级路径
print(abspath)
print(rootpath)
 
print('==========2===========')
ret = abspath.replace(rootpath, '.', 1)
print(ret)
print('此路径是否为文件夹：%s' % os.path.isdir('../' + ret))
```





### 方法2：字符串替换（用正则）

```python
import os
import re
 
print('==========1===========')
abspath = os.getcwd()  				# 获取当前路径
rootpath = os.path.abspath('..') 	# 获取上级路径
print(abspath)
print(rootpath)
 
print('==========2===========')
print(repr(repr(rootpath).strip("'")).strip("'"))  # 转义路径
print(repr(abspath).strip("'"))
print(str(abspath))
 
print('==========3===========')
ret_list = re.sub(repr(repr(rootpath).strip("'")).strip("'"), '.', repr(abspath).strip("'"))  # 获取相对路径
print('获取到的相对路径: %s' % ret_list)
 
print('../' + ret_list)
print('此路径是否为文件夹：%s' % os.path.isdir('../' + ret_list))
```





## [os.walk()](https://www.tutorialspoint.com/python/os_walk.htm)

通过在目录树中游走，输出在目录中的文件名，以及子目录名

```python
os.walk(top[, topdown=True[, onerror=None[, followlinks=False]]])
'''
top  是要遍历的目录。
topdown  是代表要从目录顶层到底层遍历，还是从底层向上遍历。
onerror  可以用来设置当便利出现错误的处理函数(该函数接受一个OSError的实例作为参数)，设置为空则不作处理
followlinks  表示是否要跟随目录下的链接去继续遍历，要注意的是，os.walk不会记录已经遍历的目录，所以跟随链接遍历的话有可能一直循环调用下去。


return:
	一个3个元素的元组 (root, dirs, files) ，分别表示遍历的路径名，该路径下的目录列表和该路径下文件列表。注意目录列表和文件列表不是具体路径，需要具体路径(从root开始的路径)的话可以用 os.path.join(root,dirs)和 os.path.join(root,files)
'''
```



```python
# !/usr/bin/python

import os
for root, dirs, files in os.walk(".", topdown=False):
   for name in files:
      print(os.path.join(root, name))
   for name in dirs:
      print(os.path.join(root, name))
    
    
        
### bottom-to-up
./tmp/test.py
./.bash_logout
./amrood.tar.gz
./.emacs
./httpd.conf
./www.tar.gz
./mysql.tar.gz
./test.py
./.bashrc
./.bash_history
./.bash_profile
./tmp


### topdown=True
./.bash_logout
./amrood.tar.gz
./.emacs
./httpd.conf
./www.tar.gz
./mysql.tar.gz
./test.py
./.bashrc
./.bash_history
./.bash_profile
./tmp
./tmp/test.py
```