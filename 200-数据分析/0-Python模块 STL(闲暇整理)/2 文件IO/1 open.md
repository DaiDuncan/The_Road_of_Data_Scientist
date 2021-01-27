# [文件函数 open()](https://www.runoob.com/python/python-func-open.html)

文件读写：不需要使用任何数据库，而是就地取材，方便快捷。但文件存储只适合存储小规模数据，没有复杂的数据间的关联关系

```python
open(**file**, **mode**='r', buffering=-1, **encoding**=None, errors=None, newline=None, closefd=True, opener=None)
'''打开一个文件，并返回文件对象。如果该文件无法被打开，会抛出 OSError
- file: 必需，文件路径（相对或者绝对路径）。
- mode: 决定了打开文件的模式：只读，写入，追加等。默认文件访问模式为只读(r)。
- buffering: 设置缓冲。如果 buffering 的值被设为 0，就不会有寄存。如果 buffering 的值取 1，访问文件时会寄存行。如果将 buffering 的值设为大于 1 的整数，表明了这就是的寄存区的缓冲大小。如果取负值，寄存区的缓冲大小则为系统默认。
- encoding: 一般使用utf8
- errors: 报错级别
- newline: 区分换行符
- closefd: 传入的file参数类型
- opener:
'''
```

注意：

- 编码变换 encoding
- 使用 open() 方法一定要保证关闭文件对象，即调用 close() 方法。

```python
with open(txt_file, 'w', encoding="utf-8") as f:
    f.write(txt)
```





## 基本操作

### 1）打开文件

文件在open的时候是不会被加到内存中的，只有read或write的时候才会加到内存中

读取文件时，首先要打开文件，获得==文件句柄==

```python
f = open('学生信息.txt', 'r')
```



#### 文件打开模式⭐

最常用的有r, w, a+ 

| 模式代号   |                                                              |
| ---------- | ------------------------------------------------------------ |
| t          | 文本模式 (默认)                                              |
| U          | 通用换行模式（不推荐）                                       |
| r          | 只读模式【默认】                                             |
| w          | 只写模式【不可读】<br>不存在则创建<br>存在则**清空内容**     |
| x          | 只写模式【不可读】<br>不存在则创建<br>**存在则报错**         |
| a          | 追加模式【可读】<br>不存在则创建<br>存在则只==追加内容==     |
| 补充后缀 + | 表示可以==同时读写==某个文件                                 |
|            | r+ 从开始向后读，写和追加的时候==指针会调到最后==            |
|            | w+ x+ a+  先清空数据，从开始向后读，写和追加的时候==指针会调到最后== |
| 补充后缀 b | 二进制模式，以字节的方式操作                                 |
|            | rb 或 r+b                                                    |
|            | wb 或 w+b                                                    |
|            | xb 或 w+b                                                    |
|            | ab 或 a+b                                                    |

注：以b方式打开时，读取到的内容是**字节类型**，写入时也需要提供**字节**类型





- w 和 wb模式

w是以文本文件格式打开文件进行写

wb模式是以二进制格式打开文件进行写操作

```python
text = '学习python'

f = open('data', 'w')
f.write(text)
f.close()

f = open('data_byte', 'wb')
b_text = text.encode(encoding='utf-8')
f.write(b_text)
f.close()
```

如果是以wb模式打开，使用write方法时，只能传入bytes类型的数据，因此需要使用encode方法先将str转成bytes类型数据。



他们内容上没有区别，何必弄出两个来呢? 原来有些数据，本身就是二进制的，而且不能也务必要转成字符串类型，那么这种数据写入文件就需要以wb的模式打开文件，下面就是一个具体的例子

```python
import pickle

class Stu:
    def __init__(self, name, age):
        self.name = name
        self.age = age

stu = Stu('小明', 15)

f = open('stu_obj', 'wb')
pickle.dump(stu, f)
f.close()
```

上面的代码里，将stu对象序列化到文件中，这种场景下，就需要以wb模式打开文件





- r 和 rb模式

想要将上面的例子中写入到文件中的stu对象读取出来

```python
f = open('stu_obj', 'rb')
stu = pickle.load(f)
f.close()

print(stu)
```

如果你用r模式打开并读取数据，程序不会报错，但是你无法还原stu对象 => 呈现乱码





- a 模式，追加模式

希望文件二次打开写入数据时，数据能从文件的末尾开始写，这样原有的内容可以保留下来，那么你需要用a的模式打开





### 2）操作文件

每次操作完后不用再次手动关闭

能同时打开两个文件（2.7之后才有的功能）

- 批量操作

```python
with open('1.txt', 'r') as f1, open('2.txt'，'r') as f2:
    pass
```

- 模拟**文件复制**的过程（针对大文件比较有效）

```python
with open('1.txt', 'r') as f1, open('2.txt'，'w') as f2:
    for line in f1:         #使用迭代：读取原文件内容，然后一行一行写到新文件中
        f2.write(line)
```



注意：write并不自动写入换行，因此需要你自己添加换行

```python
with open('test', 'w') as f:
    for i in range(10):
        f.write(str(i) + "\n")
```



### 3）关闭文件

使用open打开文件后一定要记得调用文件对象的close()方法。

比如可以用try/finally语句来确保最后能关闭文件。

```python
# open()是python的内置函数，而read()、close()等都是属于TextIOWrapper类的，详情参见源码。

f = open('1.txt', 'r')       #打开文件
data = f.read()           #操作文件
f.close()                #关闭文件
print(data)


### 关闭文件
file_object = open('thefile.txt')
try:
     all_the_text = file_object.read( )
finally:
     file_object.close( )

注：不能把open语句放在try块里，因为当打开文件出现异常时，文件对象file_object无法执行close()方法。
```





## file对象

| 方法                                                         |                                                              |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [file.close()](https://www.runoob.com/python/file-close.html) | 关闭文件。关闭后文件不能再进行读写操作。                     |
| [file.flush()](https://www.runoob.com/python/file-flush.html) | 刷新文件内部缓冲。直接把内部缓冲区的数据立刻写入文件, 而不是被动的等待输出缓冲区写入。 |
| [file.fileno()](https://www.runoob.com/python/file-fileno.html) | 返回一个整型的文件描述符(file descriptor FD 整型), 可以用在如os模块的read方法等一些底层操作上。 |
| [file.isatty()](https://www.runoob.com/python/file-isatty.html) | 如果文件连接到一个终端设备返回 True，否则返回 False。        |
| [file.next()](https://www.runoob.com/python/file-next.html)  | 返回文件下一行。                                             |
| [file.read([size\])](https://www.runoob.com/python/python-file-read.html) | 从文件读取指定的字节数，如果未给定或为负则读取所有。如果文件大小 >2 倍内存则有问题，f.read()读到文件尾时返回""(空字串)。 |
| [file.readline([size\])](https://www.runoob.com/python/file-readline.html) | 读取整行，包括 "\n" 字符。                                   |
| [file.readlines([sizeint\])](https://www.runoob.com/python/file-readlines.html) | 读取所有行并返回列表若给定sizeint>0，则是设置一次读**多少行**，这是为了减轻读取压力。 |
| [file.seek(offset[, whence\])](https://www.runoob.com/python/file-seek.html) | 设置文件当前位置：用来移动文件指针  **偏移量:** 单位为比特，可正可负  **起始位置**:   0 - 文件头, 默认值;   1 - 当前位置;   2 - 文件尾 |
| [file.tell()](https://www.runoob.com/python/file-tell.html)  | 返回文件当前位置。                                           |
| [file.truncate([size\])](https://www.runoob.com/python/file-truncate.html) | 截取文件，截取的字节通过size指定，默认为当前文件位置。       |
| [file.write(str)](https://www.runoob.com/python/python-file-write.html) | 将字符串写入文件，返回的是写入的字符长度。                   |
| [file.writelines(sequence)](https://www.runoob.com/python/file-writelines.html) | 向文件写入一个序列字符串列表，如果需要换行则**要自己加入每行的换行符**。 |





###  readlines

以列表的形式返回文件里的所有数据，文件有多少行，列表里就有多少个字符串，==每一行的换行符==也会被读取

```python
f = open('学生信息.txt', 'r')
lines = f.readlines()
print(lines)
```





### readline

一次只读取一行，如果文件特别大，readlines会一次性把数据读取到内存中，这样会非常耗费内存，而readline就不存在这样的问题，但由于一次只读取一行，所以，想要读取全部数据需要使用while循环

```python
f = open('学生信息.txt', 'r')
line = f.readline()
while line:
    print(line)
    line = f.readline()
```



### 使用迭代

open返回的文件句柄，是一个可迭代对象

```python
f = open('学生信息.txt', 'r')
for line in f:
    print(line)
```





# #练习题

## 结构化存储

```python
小明  15  170
小刚  14  168
小红  15  165

### 最好的办法是将一行数据里的一个人的信息存储到字典里，然后将这些字典存储到列表里
lst = []
with open('学生信息.txt', 'r') as f:
    for line in f:
        arrs = line.strip().split()
        info = {
            'name': arrs[0],
            'age': int(arrs[1]),
            'height': int(arrs[2])
        }
        lst.append(info)


print(lst)
```





## 附加函数

把数据读取和数据分析的代码分开

- 第一步，从文件里读取数据，就像2.3中的示例代码那样，将每个学生的数据存储为字典，将所有学生的数据存储到列表中

- 第二步，遍历列表，计算身高与年龄的平均值



读取文件专门写成一个函数，文件地址作为参数传入，这样就可以读取其他相同存储方式的文件。

将数据分析单独封装成一个函数，与数据读取分离，这样当有新的数据分析的需求时

- 比如计算学生身高的中位数，就可以再封装一个函数，当你想获取学生身高均值时，就可以将数据读取函数与计算身高均值函数结合在一起使用

```python
def read_info(filename):
    """
    从文件中读取学生信息
    :param filename:
    :return:
    """
    lst = []
    with open(filename, 'r') as f:
        for line in f:
            arrs = line.strip().split()
            info = {
                'name': arrs[0],
                'age': int(arrs[1]),
                'height': int(arrs[2])
            }

            lst.append(info)
    return lst


def avg_age_avg_height(lst):
    """
    解析lst,计算平均年龄和身高
    :param lst:
    :return:
    """
    age_sum = 0
    height_sum = 0
    for stu in lst:
        age_sum += stu['age']
        height_sum += stu['height']

    avg_age = age_sum/(len(lst))
    avg_height = height_sum/(len(lst))

    return avg_age, avg_height


def avg(filename):
    """
    计算文件filename里学生的平均年龄和身高
    :param filename:
    :return:
    """
    stu_lst = read_info(filename)
    avg_age, avg_height = avg_age_avg_height(stu_lst)
    print(avg_age, avg_height)

avg('学生信息.txt')

```



## 找出文件中的相同IP

将数据读取与数据分析的代码分开来写

考虑读取文件数据时将数据以何种方式保存

- IP存储到集合中，两个集合求交集

```python
### data1
192.168.13.112
192.168.13.113
192.168.13.114
192.168.13.115
192.168.13.116

### data2
192.168.14.112
192.168.14.113
192.168.14.114
192.168.14.115
192.168.14.116
192.168.13.114
192.168.13.115
192.168.13.116



def read_ip(filename):
    """
    从文件filename当中读取ip,保存到集合中,并返回
    :param filename:
    :return:
    """
    f = open(filename, 'r')
    lines = f.readlines()
    ip_set = set()

    for line in lines:
        ip_set.add(line.strip())

    return ip_set


def same_ip():
    ip_set_1 = read_ip('data1')
    ip_set_2 = read_ip('data2')

    same_ip_set = ip_set_1.intersection(ip_set_2)
    print(same_ip_set)


if __name__ == '__main__':
    same_ip()
```





## 统计代码行数

py_path目录下可能还会有文件夹，文件夹的嵌套层次事先是不知道的，因此你现在需要一个函数

- 这个函数可以获取一个文件夹下的所有指定文件，不管这个文件夹下有多少层文件夹



第二个函数：统计代码行数

- 对于一个python脚本而言，空行不算代码
- 一行内容去掉空格后是以# 开头的也不算代码

```python
import os


def get_file_lst(py_path, suffix):
    """
    遍历指定文件目录下的所有指定类型文件
    :param py_path:
    :return:
    """
    file_lst = []
    for root, dirs, files in os.walk(py_path):
        for file in files:
            file_name = os.path.join(root, file)
            if file_name.endswith(suffix):
                file_lst.append(file_name)

    return file_lst


def get_program_line_count(file_name):
    """
    返回单个py脚本的代码行数
    :param filename:
    :return:
    """
    # 存储代码行数
    count = 0
    with open(file_name, 'r', encoding='utf-8')as f:
        for line in f:
            line = line.strip()
            if not line:
                continue

            if line.startswith("#"):
                continue
            count += 1
    return count



def get_py_program_count(py_path):
    # 获取指定目录下所有的python脚本
    file_lst = get_file_lst(py_path, '.py')
    count = 0
    # 遍历列表,获取单个python脚本的代码行数
    for file_name in file_lst:
        count += get_program_line_count(file_name)  # 累加代码函数

    return count   # 返回代码总函数


if __name__ == '__main__':
    py_path = '/Users/kwsy/PycharmProjects/pythonclass'
    print(get_py_program_count(py_path))
```





# 补充|os.getcwd()

程序的工作目录，也就是前面所提到的执行程序的命令所发生的的目录



如果你的程序中自带了一些数据，需要在程序运行期间读取，那么千万不要用相对路径，因为你可能并不知道你的程序是在哪里被启动的，这就意味着你并不清楚程序的工作目录，这个时候，你要先获取脚本的绝对路径，然后根据脚本与数据时间的目录关系进行查找

```python
test_path
├── __init__.py
└── mypath
    ├── __init__.py
    ├── data
    └── test.py
    
##########################################
### os.path.abspath(<file name>)
import os 
  
# file name    
file_name = 'GFG.txt'
  
# change the current working 
# directory 
os.chdir("/home/geeks/") 
  
# prints the absolute path of current 
# working directory with  file name 
print(os.path.abspath(file_name)) 
##########################################
import os

file_path = os.path.abspath(__file__)  # 先通过abspath方法获取当前脚本的绝对路径
print(file_path)
file_path = os.path.dirname(file_path)  # 获取当前路径
print(file_path)

with open(os.path.join(file_path, 'data')) as f:
    print(f.read())
```



```python
import os

os.getcwd()	#'D:\\D\\test_file\\jupyter'

file_path = os.path.abspath('ml-bugs.csv')  # 先通过abspath方法获取当前脚本的绝对路径
print(file_path)
file_path = os.path.dirname(file_path)  # 获取当前路径
print(file_path)


### output
D:\D\test_file\jupyter\ml-bugs.csv
D:\D\test_file\jupyter
```







# #参考文献

[Python 文件I/O](https://www.runoob.com/python/python-files-io.html)

[[转摘\]cnblogs | 文件操作(初阶)](https://www.cnblogs.com/whatisfantasy/p/5967288.html)

[cnblogs | Python标准模块(二)](https://www.cnblogs.com/whatisfantasy/p/6011586.html)

[Python库 | urllib.request](https://docs.python.org/3.5/library/urllib.request.html#module-urllib.request)

[Python第三方库 | Requests](https://requests.readthedocs.io/zh_CN/latest/)

[XML命名空间](https://www.w3school.com.cn/xml/xml_namespaces.asp)