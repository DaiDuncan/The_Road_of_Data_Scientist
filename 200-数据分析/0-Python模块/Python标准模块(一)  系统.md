# Python标准模块(一) | 系统

**声明**：以下内容主要来自[cnblogs | Python标准模块(一)](https://www.cnblogs.com/whatisfantasy/p/5995278.html)，根据理解做了些许调整



模块：多个 .py 文件组成的代码集合，如：os 是系统相关的模块；file是文件操作相关的模块

模块分为三种：

- 内置标准模块（又称标准库）
- 开源模块
- 自定义模块 @PyPI



## 一 time

```
import time

time.sleep(5)      #等待5s
print(time.time())      #打印时间戳
print(time.ctime())      #打印当前系统时间
print(time.ctime(time.time()-86400))      #打印昨天这个时候的系统时间

#OUTPUT#
1592728196.9899387
Sun Jun 21 10:29:57 2020
Sat Jun 20 10:29:57 2020
==========================================================
#以struct_time格式显示时间，输出时间对象
print(time.gmtime())    #显示国际系统时间时间
print(time.localtime())     #显示本地地系统时间（无时差）

#OUTPUT#
time.struct_time(tm_year=2020, tm_mon=6, tm_mday=21, tm_hour=8, tm_min=29, tm_sec=57, tm_wday=6, tm_yday=173, tm_isdst=0)
time.struct_time(tm_year=2020, tm_mon=6, tm_mday=21, tm_hour=10, tm_min=29, tm_sec=57, tm_wday=6, tm_yday=173, tm_isdst=1)
==========================================================
print(time.mktime(time.localtime()))    #把struct_time格式转成时间戳格式，必须传入时间对象，否则报错
print(time.strftime("%Y-%m-%d %H:%M:%S", time.gmtime()))    #把struct_time格式转换成字符串格式
print(time.strptime("2016-01-28", "%Y-%m-%d"))  # 把字符串格式转换成struct_time格式

#OUTPUT#
1592728197.0
2020-06-21 08:29:57
time.struct_time(tm_year=2016, tm_mon=1, tm_mday=28, tm_hour=0, tm_min=0, tm_sec=0, tm_wday=3, tm_yday=28, tm_isdst=-1)
==========================================================
#格式化输出时间
object= time.gmtime()
print('%s-%s-%s %s:%s:%s'%(object.tm_year,object.tm_mon,object.tm_mday,object.tm_hour,object.tm_min,object.tm_sec))

#OUTPUT#    #UTC±00:00：格林威治标准时间(伦敦)
2020-6-21 8:29:57
```





## 二 datetime

```
import datetime

print(datetime.datetime.now())      #2016-10-23 16:28:38.389144
print(datetime.datetime.today())    #2016-10-23 16:28:38.389144
print(datetime.date.today())    #2016-10-23

print(datetime.date.fromtimestamp(time.time()) )  # 时间戳直接转成日期格式 2016-08-19

#时间的加减
print(datetime.datetime.now() + datetime.timedelta(3)) #当前时间+3天
print(datetime.datetime.now() + datetime.timedelta(days=-3)) #当前时间-3天
print(datetime.datetime.now() + datetime.timedelta(hours=3)) #当前时间+3小时
print(datetime.datetime.now() + datetime.timedelta(minutes=30)) #当前时间+30分

# c_time  = datetime.datetime.now()
# print(c_time.replace(minute=3,hour=2)) #时间替换
```





## 三 sys

sys模块里存放的都是和解释器相关的功能

### 1 sys.argv

```
import sys
 
print(sys.argv)
 
#OUTPUT#
$ python test.py helo world
['test.py', 'helo', 'world']  #把执行脚本时传递的参数获取到了
```



### 2 sys.path

```
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



### 3 sys.exit()

等同于exit()，正常退出时exit(0)

```
import sys

choice = input("wanna quit?")
if choice == 'y' or choice == 'Y':
    sys.exit("Good Bye！")        #等同于exit("Good Bye！")

#OUTPUT#
wanna quit?y
Good Bye！

Process finished with exit code 1
```



### 4 sys.platform

返回操作系统平台名称

```
import sys

#MacOS
print(sys.platform) #darwin

#Win7
print(sys.platform) #win32
```



### 5 sys.stdout.write()

在屏幕上输出

```
#进度条的简单实现

import sys
import time

for i in range(101):
    sys.stdout.write("\r"+"%s%%\t"%(i)+"#"*i)
    sys.stdout.flush()
    time.sleep(0.05)
```



## 四 os

os模块存放和系统相关的功能

| 方法：使用时带上()                      | 备注                                                         |
| --------------------------------------- | ------------------------------------------------------------ |
| os.getcwd()                             | 获取当前工作目录，即当前python脚本工作的目录路径             |
| os.getuid()                             | 返回当前进程的有效用户 id                                    |
| os.getpid()                             | 函数返回当前进程的 id                                        |
| os.getppid                              | 返回父进程的 id                                              |
| os.uname                                | 函数返回识别操作系统的不同信息                               |
| os.name                                 | 字符串指示当前使用平台。win->'nt'; Linux->'posix'            |
| os.chdir("dirname")                     | 改变当前脚本工作目录；相当于shell下cd                        |
| os.curdir                               | 返回当前目录: ('.')                                          |
| os.pardir                               | 获取当前目录的父目录字符串名：('..')                         |
| os.makedirs('dir1/dir2')                | 可生成多层递归目录                                           |
| os.removedirs('dirname1')               | 若目录为空，则删除，并递归到上一级目录，如若也为空，则删除，依此类推 |
| os.mkdir('dirname')                     | 生成单级目录；相当于shell中mkdir dirname                     |
| os.rmdir('dirname')                     | 删除单级空目录，若目录不为空则无法删除，报错；相当于shell中rmdir dirname |
| os.listdir('dirname')                   | 列出指定目录下的所有文件和子目录，包括隐藏文件，并以列表方式打印 |
| os.remove()                             | 删除一个文件                                                 |
| os.rename("oldname","new")              | 重命名文件/目录                                              |
| **os.stat('path/filename')**            | 获取文件/目录信息                                            |
| os.sep                                  | 操作系统特定的路径分隔符，win下为"\",Linux下为"/"            |
| os.linesep                              | 当前平台使用的行终止符，win下为"\t\n",Linux下为"\n"          |
| os.pathsep                              | 用于分割文件路径的字符串                                     |
| os.system("bash command")               | 运行shell命令，直接显示                                      |
| os.environ                              | 获取系统环境变量                                             |
| os.path.abspath(path)                   | 返回path规范化的绝对路径                                     |
| os.path.split(path)                     | 将path分割成目录和文件名二元组返回                           |
| **os.path.dirname(path)**               | 返回path的目录。其实就是os.path.split(path)的第一个元素      |
| os.path.basename(path)                  | 返回path最后的文件名。如何path以／或\结尾，那么就会返回空值。即os.path.split(path)的第二个元素 |
| **os.path.exists(path)**                | 如果path存在，返回True；如果path不存在，返回False            |
| **os.path.isabs(path)**                 | 如果path是绝对路径，返回True                                 |
| **os.path.isfile(path)**                | 如果path是一个存在的文件，返回True。否则返回False            |
| **os.path.isdir(path)**                 | 如果path是一个存在的目录，则返回True。否则返回False          |
| **os.path.join(path1[, path2[, ...]])** | 将多个路径组合后返回，第一个绝对路径之前的参数将被忽略       |
| os.path.getatime(path)                  | 返回path所指向的文件或者目录的最后存取时间                   |
| os.path.getmtime(path)                  | 返回path所指向的文件或者目录的最后修改时间                   |



```
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





## 五 random

### 1 随机数

```
import random
print random.random()
print random.randint(1,2)
print random.randrange(1,10)
```



### 2 生成随机验证码

```
import random
checkcode = ''
for i in range(4):
    current = random.randrange(0,4)
    if current != i:
        temp = chr(random.randint(65,90))
    else:
        temp = random.randint(0,9)
    checkcode += str(temp)
print checkcode
```



## 六 re

详见[re | Python正则表达式](https://www.yuque.com/docs/share/7dcf9a16-d72c-4f71-96f0-23160d093e51?#)





## 七 hashlib

用于加密相关的操作，代替了md5模块和sha模块，主要提供 SHA1, SHA224, SHA256, SHA384, SHA512 ，MD5 算法

```
import hashlib
 
# ######## md5 ########
hash = hashlib.md5()
# help(hash.update)
hash.update(bytes('admin', encoding='utf-8'))
print(hash.hexdigest())
print(hash.digest())
 
 
######## sha1 ########
 
hash = hashlib.sha1()
hash.update(bytes('admin', encoding='utf-8'))
print(hash.hexdigest())
 
# ######## sha256 ########
 
hash = hashlib.sha256()
hash.update(bytes('admin', encoding='utf-8'))
print(hash.hexdigest())
 
 
######### sha384 ########
 
hash = hashlib.sha384()
hash.update(bytes('admin', encoding='utf-8'))
print(hash.hexdigest())
 
######### sha512 ########
 
hash = hashlib.sha512()
hash.update(bytes('admin', encoding='utf-8'))
print(hash.hexdigest())
```

以上加密算法虽然依然非常厉害，但时候存在缺陷，即：通过撞库可以反解。

所以，有必要对加密算法中添加**自定义key**再来做加密。

```
import hashlib
 
# ######## md5 ########
 
hash = hashlib.md5(bytes('898oaFs09f',encoding="utf-8"))
hash.update(bytes('admin',encoding="utf-8"))
print(hash.hexdigest())
```

python内置还有一个 hmac 模块，它内部对我们**创建 key 和 内容** 进行进一步的处理，然后再加密

```
import hmac
 
h = hmac.new(bytes('898oaFs09f',encoding="utf-8"))
h.update(bytes('admin',encoding="utf-8"))
print(h.hexdigest())
```





## 参考文献

[cnblogs | Python标准模块(一)](https://www.cnblogs.com/whatisfantasy/p/5995278.html)