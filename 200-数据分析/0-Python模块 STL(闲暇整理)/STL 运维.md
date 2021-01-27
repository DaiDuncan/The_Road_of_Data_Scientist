- logging
- unittest



# subprocess

可以执行shell命令的相关模块和函数有：

- os.system
- os.spawn*
- os.popen*   ——废弃
- popen2.*    ——废弃
- commands.*  ——废弃，3.x中被移除

```python
import commands

result = commands.getoutput('cmd')
result = commands.getstatus('cmd')
result = commands.getstatusoutput('cmd')
```

以上执行shell命令的相关的模块和函数的功能均在 subprocess 模块中实现，并提供了更丰富的功能。

### 1 call

执行命令，**返回状态码**(命令正常执行返回0，报错则返回1)

```python
ret1=subprocess.call("ifconfig")
ret2=subprocess.call("ipconfig")
print(ret1)     #0
print(ret2)     #1
ret = subprocess.call(["ls", "-l"], shell=False)    #shell为False的时候命令必须分开写
ret = subprocess.call("ls -l", shell=True)
```



### 2 check_call

执行命令，如果执行成功则返回状态码0，否则抛异常

```python
subprocess.check_call(["ls", "-l"])
subprocess.check_call("exit 1", shell=True)
```



### 3 check_output

执行命令，如果执行成功则返回执行结果，否则抛异常

```python
subprocess.check_output(["echo", "Hello World!"])
subprocess.check_output("exit 1", shell=True)
```



### 4 subprocess.Popen(...)

用于执行**复杂的系统命令**

| 参数                  | 注释                                                         |
| --------------------- | ------------------------------------------------------------ |
| args                  | shell命令，可以是字符串或者序列类型（如：list，元组）        |
| bufsize               | 指定缓冲。  0 无缓冲  1 行缓冲  其他 缓冲区大小  负值 系统缓冲 |
| stdin, stdout, stderr | 分别表示程序的标准输入、输出、错误句柄                       |
| preexec_fn            | 只在Unix平台下有效，用于指定一个可执行对象（callable object），它将在子进程运行之前被调用 |
| close_sfs             | 在windows平台下，如果close_fds被设置为True，则新创建的子进程将不会继承父进程的输入、输出、错误管道。所以不能将close_fds设置为True同时重定向子进程的标准输入、输出与错误(stdin, stdout, stderr)。 |
| shell                 | 同上                                                         |
| cwd                   | 用于设置子进程的当前目录                                     |
| env                   | 用于指定子进程的环境变量。如果env = None，子进程的环境变量将从父进程中继承。 |
| universal_newlines    | 不同系统的换行符不同，True -> 同意使用 \n                    |
| startupinfo           | 只在windows下有效，将被传递给底层的CreateProcess()函数，用于设置子进程的一些属性，如：主窗口的外观，进程的优先级等等 |
| createionflags        | 同上                                                         |

```python
import subprocess
ret1 = subprocess.Popen(["mkdir","t1"])
ret2 = subprocess.Popen("mkdir t2", shell=True)
```

终端输入的命令分为两种：

- 输入即可得到输出，如：ifconfig
- 输入进行某环境，依赖再输入，如：python

```python
import subprocess
obj = subprocess.Popen("mkdir t3", shell=True, cwd='/home/dev',)     #在cwd目录下执行命令


========================================================
import subprocess

obj = subprocess.Popen(["python"], stdin=subprocess.PIPE, stdout=subprocess.PIPE, stderr=subprocess.PIPE, universal_newlines=True)
obj.stdin.write("print(1)\n")
obj.stdin.write("print(2)")
obj.stdin.close()

cmd_out = obj.stdout.read()
obj.stdout.close()
cmd_error = obj.stderr.read()
obj.stderr.close()

print(cmd_out)
print(cmd_error)


========================================================
import subprocess

obj = subprocess.Popen(["python"], stdin=subprocess.PIPE, stdout=subprocess.PIPE, stderr=subprocess.PIPE, universal_newlines=True)
obj.stdin.write("print(1)\n")
obj.stdin.write("print(2)")

out_error_list = obj.communicate()
print(out_error_list)


========================================================
import subprocess

obj = subprocess.Popen(["python"], stdin=subprocess.PIPE, stdout=subprocess.PIPE, stderr=subprocess.PIPE, universal_newlines=True)
out_error_list = obj.communicate('print("hello")')
print(out_error_list)
```





# logging

用于便捷**记录日志**且**线程安全**的模块，不会允许多个人同时操作，不会存在脏数据

### 1 简单日志输出

```python
import logging

logging.debug('This is debug message')
logging.info('This is info message')
logging.warning('This is warning message')
 
OUTPUT:
WARNING:root:This is warning message
```

默认的日志级别为WARNING，只有【当前写等级】>=【日志等级】时，日志文件才被记录。

由于info和debug的日志等级都比warning小，所以上面的代码输出为：“WARNING:root:This is warning message”

日志级别大小关系如下，当然也可以自己定义日志级别。

- **CRITICAL** = 50
- FATAL = CRITICAL
- ERROR = 40
- WARNING = 30
- WARN = WARNING
- INFO = 20
- DEBUG = 10
- NOTSET = 0



### 2 单文件日志

```python
import logging
  
logging.basicConfig(filename='log.log',
                    format='%(asctime)s - %(name)s - %(levelname)s -%(module)s:  %(message)s',
                    datefmt='%Y-%m-%d %H:%M:%S %p',
                    level=10) 
#只有【当前写等级】大于10时，日志文件才被记录。

logging.debug('debug')
logging.info('info')
logging.warning('warning')
logging.error('error')
logging.critical('critical')
logging.log(10,'log')    
```

logging.basicConfig函数各参数：

| 函数以及参数   | 注释                                                         |
| -------------- | ------------------------------------------------------------ |
| filename       | 指定日志文件名                                               |
| filemode       | 和file函数意义相同，指定日志文件的打开模式，'w'或'a'         |
| format         | 指定输出的格式和内容，format可以输出很多有用信息，如上例所示 |
| %(levelno)s    | 打印日志级别的数值                                           |
| %(levelname)s  | 打印日志级别名称                                             |
| %(pathname)s   | 打印当前执行程序的路径，其实就是sys.argv[0]                  |
| %(filename)s   | 打印**当前执行程序名**                                       |
| %(funcName)s   | 打印日志的当前函数                                           |
| %(lineno)d     | 打印日志的当前行号                                           |
| %(asctime)s    | 打印日志的时间                                               |
| %(thread)d     | 打印**线程ID**                                               |
| %(threadName)s | 打印线程名称                                                 |
| %(process)d    | 打印**进程ID**                                               |
| %(message)s    | 打印日志信息                                                 |
| datefmt        | 指定时间格式，同time.strftime()                              |
| level          | 设置日志级别，默认为logging.WARNING                          |
| stream         | 指定将日志的输出流，可以指定输出到sys.stderr,sys.stdout或者文件，默认输出到sys.stderr，当stream和filename同时指定时，stream被忽略 |



### 3 多文件日志

对于上述记录日志的功能，只能将日志记录在单文件中，如果想要设置多个日志文件，logging.basicConfig将无法完成，需要自定义文件和日志操作对象。

日志一：

```python
# 定义文件
file_1_1 = logging.FileHandler('l1_1.log', 'a', encoding='utf-8')
fmt = logging.Formatter(fmt="%(asctime)s - %(name)s - %(levelname)s -%(module)s:  %(message)s")
file_1_1.setFormatter(fmt)

file_1_2 = logging.FileHandler('l1_2.log', 'a', encoding='utf-8')
fmt = logging.Formatter()
file_1_2.setFormatter(fmt)

# 定义日志
logger1 = logging.Logger('s1', level=logging.ERROR)
logger1.addHandler(file_1_1)
logger1.addHandler(file_1_2)

# 写日志
logger1.critical('1111')
```

日志二：

```python
# 定义文件
file_2_1 = logging.FileHandler('l2_1.log', 'a')
fmt = logging.Formatter()
file_2_1.setFormatter(fmt)

# 定义日志
logger2 = logging.Logger('s2', level=logging.INFO)
logger2.addHandler(file_2_1)
```

如上述创建的两个日志对象

当使用【logger1】写日志时，会将相应的内容写入 l1_1.log 和 l1_2.log 文件中

当使用【logger2】写日志时，会将相应的内容写入 l2_1.log 文件中





# #参考文献

**声明**：内容主要来自[cnblogs | Python标准模块(三)](https://www.cnblogs.com/whatisfantasy/p/6014515.html)，根据理解做了些许调整