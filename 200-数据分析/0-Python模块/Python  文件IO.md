# Python | 文件I/O

## 一 [文件函数 open()](https://www.runoob.com/python/python-func-open.html)

open(**file**, **mode**='r', buffering=-1, **encoding**=None, errors=None, newline=None, closefd=True, opener=None)

打开一个文件，并返回文件对象。如果该文件无法被打开，会抛出 OSError。

- file: 必需，文件路径（相对或者绝对路径）。
- mode: 决定了打开文件的模式：只读，写入，追加等。默认文件访问模式为只读(r)。
- buffering: 设置缓冲。如果 buffering 的值被设为 0，就不会有寄存。如果 buffering 的值取 1，访问文件时会寄存行。如果将 buffering 的值设为大于 1 的整数，表明了这就是的寄存区的缓冲大小。如果取负值，寄存区的缓冲大小则为系统默认。
- encoding: 一般使用utf8
- errors: 报错级别
- newline: 区分换行符
- closefd: 传入的file参数类型
- opener:



注意：

- 编码变换 encoding
- 使用 open() 方法一定要保证关闭文件对象，即调用 close() 方法。



```
with open(txt_file, 'w', encoding="utf-8") as f:
    f.write(txt)
```

### 基本操作

#### 1）打开文件

文件在open的时候是不会被加到内存中的，只有read或write的时候才会加到内存中

#### 2）操作文件

#### 3）关闭文件

使用open打开文件后一定要记得调用文件对象的close()方法。

比如可以用try/finally语句来确保最后能关闭文件。

```
#open()是python的内置函数，而read()、close()等都是属于TextIOWrapper类的，详情参见源码。

f = open('1.txt', 'r')       #打开文件
data = f.read()           #操作文件
f.close()                #关闭文件
print(data)


## 关闭文件
file_object = open('thefile.txt')
try:
     all_the_text = file_object.read( )
finally:
     file_object.close( )

注：不能把open语句放在try块里，因为当打开文件出现异常时，文件对象file_object无法执行close()方法。
```

### 1 文件打开模式

- r ，只读模式【默认】
- w，只写模式【不可读；不存在则创建；存在则清空内容；】
- x， 只写模式【不可读；不存在则创建，**存在则报错**】
- a， 追加模式【可读； 不存在则创建；存在则只追加内容；】

- - "+" 表示可以**同时读写**某个文件

- - - r+ 从开始向后读，写和追加的时候指针会调到最后
    - w+ x+ a+  先清空数据，从开始向后读，写和追加的时候指针会调到最后

- - "b"表示以字节的方式操作

- - - rb 或 r+b
    - wb 或 w+b
    - xb 或 w+b
    - ab 或 a+b

注：以b方式打开时，读取到的内容是**字节类型**，写入时也需要提供**字节**类型

| 模式 | 描述                                                         |
| ---- | ------------------------------------------------------------ |
| t    | 文本模式 (默认)。                                            |
| x    | 写模式，新建一个文件，如果该文件已存在则会报错。             |
| b    | 二进制模式。                                                 |
| +    | 打开一个文件进行更新(可读可写)。                             |
| U    | 通用换行模式（不推荐）。                                     |
| w    | 打开一个文件只用于写入。如果该文件已存在则打开文件，并从开头开始编辑，即**原有内容会被删除**。如果该文件不存在，创建新文件。 |
| wb   | 以二进制格式打开一个文件只用于写入。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。如果该文件**不存在，创建新文件**。 |
| w+   | 打开一个文件用于读写。如果该文件已存在则打开文件，并从开头开始编辑，即**原有内容会被删除**。如果该文件不存在，创建新文件。 |
| wb+  | 以二进制格式打开一个文件用于读写。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。如果该文件不存在，创建新文件。 |
| a    | 打开一个文件用于追加。如果该文件已存在，文件指针将会放在**文件的结尾**。也就是说，新的内容将会被写入到已有内容之后。如果该文件不存在，创建新文件进行写入。 |
| ab   | 以二进制格式打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。也就是说，新的内容将会被写入到已有内容之后。如果该文件不存在，创建新文件进行写入。 |
| a+   | 打开一个文件**用于读写**。如果该文件已存在，文件指针将会放在文件的结尾。文件打开时会是追加模式。如果该文件不存在，创建新文件用于读写。 |
| ab+  | 以**二进制格式**打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。如果该文件不存在，创建新文件用于读写。 |

### 2 file对象

| [file.close()](https://www.runoob.com/python/file-close.html) | 关闭文件。关闭后文件不能再进行读写操作。                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
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

####  

#### #源码

```
class TextIOWrapper(_TextIOBase):
    """
    Character and line based layer over a BufferedIOBase object, buffer.
    
    encoding gives the name of the encoding that the stream will be
    decoded or encoded with. It defaults to locale.getpreferredencoding(False).
    
    errors determines the strictness of encoding and decoding (see
    help(codecs.Codec) or the documentation for codecs.register) and
    defaults to "strict".
    
    newline controls how line endings are handled. It can be None, '',
    '\n', '\r', and '\r\n'.  It works as follows:
    
    * On input, if newline is None, universal newlines mode is
      enabled. Lines in the input can end in '\n', '\r', or '\r\n', and
      these are translated into '\n' before being returned to the
      caller. If it is '', universal newline mode is enabled, but line
      endings are returned to the caller untranslated. If it has any of
      the other legal values, input lines are only terminated by the given
      string, and the line ending is returned to the caller untranslated.
    
    * On output, if newline is None, any '\n' characters written are
      translated to the system default line separator, os.linesep. If
      newline is '' or '\n', no translation takes place. If newline is any
      of the other legal values, any '\n' characters written are translated
      to the given string.
    
    If line_buffering is True, a call to flush is implied when a call to
    write contains a newline character.
    """
    def close(self, *args, **kwargs): 
        """关闭文件"""      
        pass

    def detach(self, *args, **kwargs): 
        pass

    def fileno(self, *args, **kwargs): 
        """文件描述符"""  
        pass

    def flush(self, *args, **kwargs): 
        """把缓冲区的内容写入硬盘"""
        pass

    def isatty(self, *args, **kwargs): 
        pass

    def read(self, *args, **kwargs): 
        """读取指定字节数据"""
        pass

    def readable(self, *args, **kwargs): 
        pass

    def readline(self, *args, **kwargs): 
        """只读取一行数据"""
        pass

    def seek(self, *args, **kwargs): 
        """
        指定文件中指针位置
        f.seek(0) 把指针归零
        """
        pass

    def seekable(self, *args, **kwargs): 
        pass

    def tell(self, *args, **kwargs): 
        """获取当前指针位置"""
        pass

    def truncate(self, *args, **kwargs): 
        pass

    def writable(self, *args, **kwargs): 
        pass

    def write(self, *args, **kwargs): 
        pass

    def __getstate__(self, *args, **kwargs): 
        pass

    def __init__(self, *args, **kwargs): 
        pass

    @staticmethod # known case of __new__
    def __new__(*args, **kwargs): 
        """ Create and return a new object.  See help(type) for accurate signature. """
        pass

    def __next__(self, *args, **kwargs): 
        """ Implement next(self). """
        pass

    def __repr__(self, *args, **kwargs): 
        """ Return repr(self). """
        pass
```





## 二 功能代码块

- 每次操作完后不用再次手动关闭
- 能同时打开两个文件（2.7之后才有的功能）

### 1 批量操作

```
with open('1.txt', 'r') as f1, open('2.txt'，'r') as f2:
    pass
```

### 2 模拟**文件复制**的过程（针对大文件比较有效）

```
with open('1.txt', 'r') as f1, open('2.txt'，'w') as f2:
    for line in f1:         #读取原文件内容，然后一行一行写到新文件中
        f2.write(line)
```

## 参考文献

[Python 文件I/O](https://www.runoob.com/python/python-files-io.html)

[[转摘\]cnblogs | 文件操作(初阶)](https://www.cnblogs.com/whatisfantasy/p/5967288.html)