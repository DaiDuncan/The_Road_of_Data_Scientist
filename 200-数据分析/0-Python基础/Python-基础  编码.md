# Python-基础 | 编码

**声明**：以下内容主要基于[cnblogs | python编码](https://www.cnblogs.com/whatisfantasy/p/5951294.html)



## 一 常见编码

- ascii：1个字节
- unicode：2个字节
- utf-8：英文1个字节，汉字3个字节

### 1 Python 2.X

- str(用于8位文本和二进制数据)
- unicode(用于宽字符文本)

在Python2中，通用的str类型填补了二进制数据的这一角色（特指python3中的bytes类型），因为字符串也只是**字节的序列**(单独的unicode类型处理宽字符串)。

在Python2中，为了兼容性而使用b'xxx'，但是它与'xxx'是相同的，并且产生一个str，并且，bytes只是str的同义词。

在Python3中，这二者都解决了bytes类型之间的差异。Python2中的u'xxx'和 U'xxx' Unicode字符串常量形式在Python3中已经**取消**了，而是使用'xxx'替代，因为所有的字符串都是Unicode，即便它们包含所有的ASCII字符。



### 2 Python 3.X

- str(用于**Unicode文本**，**包括ASCII**)
- bytes(用 于带有绝对**字节值**的二进制数据)
- bytearray(bytes的一种可变的形式)

bytes是一个**不可改变的字符序列**。

Python 3.0 bytes对象是较小整数的一个序列，其中每个整数都在0到255之间。在python3中bytes主要用于处理那些没有针对每个任意文本格式都编码的raw字节数据（图像和声音文件，以及用来与设备接口的打包数据，或者你想要用python的struct模块处理的C程序）。

Python3的bytes类型支持几乎str类型所做的所有相同操作：包括字符串方法、序列操作，甚至re模块模式匹配。



bytearray是bytes类型的一个变体，它是可变的并且支持原处修改。

它支持str和bytes所支持的常见的字符串操作，以及和列表相同的很多**原处修改操作**(例如，append和extend方法， 以及向索引赋值)。



### 3 文件分类

python3中的文件I/O一般分为两类

- 文本文件str：适用于文本内容
- 二进制文件Byte：适用于图像、数据流



使用建议：

1. 针对图像文件；其他程序创建的，而且必须解压的打包数据；或者一些设备数据流，则使用**bytes和二进制**模式文件处理它更合适。如果想要更新数据而**不在内存中产生其副本**，也可以选择使用bytearray。
2. 针对实质是文本的内容，例如程序输出、HTML、国际化文本或CSV或XML文件，可能要使用**str和文本模式**文件。



### 4 类型转换

Python 3.0下的类型转换：

1. str.encode()和bytes(S, encoding)把一个字符串转换为其**raw bytes形式**，并且在此过程中根据一个str创建一个bytes。
2. bytes.decode()和str(B, encoding)把raw bytes**转换为其字符串形式**，并且在此过程中根据一个bytes创建一个str。

```
## 字符串转换为其raw bytes形式 
>>> S = 'eggs' 
>>> S.encode()  
b'eggs'
>>> bytes(S, encoding='ascii') 
b'eggs'


## raw bytes转换为其字符串形式
>>> B = b'spam' 
>>> B.decode() 
'spam'
>>> str(B, encoding='ascii') 
'spam'
```



### 5 源文件字符集编码声明

对于在脚本文件中编码的字符串，python默认地使用UTF-8编码。

但是，它允许我们通过包含一个注释来指明想要的编码，从而将**默认值修改为支持任意的字符集**。这个注释必须拥有如下的形式，并且在Python 2.6或Python 3.0中必须作为**脚本的第一行或第二行**出现:

```
# -*- coding: latin-1 -*-
```



## 二 实例：编码问题

问：如下代码设置了在代码中添加了coding: utf-8，但是在cmd下面运行的时候还是输出乱码，这是什么情况？

```
test.py:
#!/usr/bin/env python
# -*- coding: utf-8 -*-
print “你好”
```

答： cmd**默认的编码是GBK格式**的，所以只在代码里写了coding: utf-8也是不行的。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1592924662873-d6bd926b-424a-4b9e-bf58-dafdb377c216.png)



### #解决方案

#### 方案1）[修改cmd的编码为utf-8格式](http://blog.useasp.net/archive/2012/04/24/how_to_use_UTF8_encoding_in_Windows_CMD.aspx)

#### 方案2）使代码以gbk的格式输出

- Python2风格

```
#!/usr/bin/env python
# -*- coding: utf-8 -*-

temp = "你好"
temp_unicode = temp.decode('utf-8')        #把utf-8解码成unicode格式
temp_gbk = temp_unicode.encode('gbk')      #把unicode编码成gbk格式
print temp_gbk
```

- Python3风格

utf-8编码能直接转成gbk格式的编码

```
#!/usr/bin/env python
# -*- coding: utf-8 -*-

temp = "你好"
temp_gbk = temp.encode('gbk')       #在python3中utf-8编码能直接转成gbk格式的编码
print temp_gbk
```



### 1 查看系统编码

```
## Python2

Python 2.7.10 (default, Jul 30 2016, 19:40:32)
[GCC 4.2.1 Compatible Apple LLVM 8.0.0 (clang-800.0.34)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>>
>>> import sys
>>> sys.getdefaultencoding()
'ascii'


## Python3
Python 3.5.2 (v3.5.2:4def2a2901a5, Jun 26 2016, 10:47:25)
[GCC 4.2.1 (Apple Inc. build 5666) (dot 3)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import sys
>>> sys.getdefaultencoding()
'utf-8'
```

### 2 修改系统编码

如果程序执行的过程中，遇到下面的报错信息时，可以把Python2的系统编码改为utf-8。

```
UnicodeEncodeError: 'ascii' codec can't encode characters in position 0-1....
# Python2的系统编码改为utf-8，一般放在文件头
import sys
reload(sys)
sys.setdefaultencoding('utf-8')
```





## 三 不同场景下的字符编码方式

现在计算机中通用的字符编码工作方式：

在**内存中统一使用unicode**，需要**保存到硬盘**或要**传输**的时候就会转为utf-8。



### 1 使用记事本

编辑的时候，文件读取的utf-8字符被转换为unicode字符到**内存**里

编辑完成后，保存的时候再把unicode转换为UTF-8保存到文件里

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1592924834707-5084aeb0-8dd8-4f35-aafa-3008bb6cf416.png)



### 2 浏览网页

服务器会把动态生成的unicode转换为utf-8，再传输到浏览器。

因此会在网页**源码上有类似的信息**，表示该网页用的就是utf-8编码。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1592924870951-1570d9ca-cc76-493f-b4f0-54a877ed94d4.png)





## 四 字符串与字节

### 1 bytes类型（和int,str类似的数据类型）

```
s = "你好"
for i in s:
    bytes_list = bytes(i,encoding='utf-8')        #bytes可以把字符串转换为字节
    print(bytes_list)

    for j in bytes_list:    #在迭代输出16进制的字节时，默认以10进制方式输出
        print(j)

OUTPUT:
b'\xe4\xbd\xa0'  #"你"
228
189
160
b'\xe5\xa5\xbd'  #"好"
229
165
189
```



### 2 字符串与字节的转换

```
a ="你好"

b = bytes(a,encoding='utf-8')       #把字符串转换为字节类型
c = str(b,encoding='utf-8')       #把字节转换为字符串类型

print(b)
print(c)

OUTPUT:
b'\xe4\xbd\xa0\xe5\xa5\xbd'
你好
```



### 3 应用场景

两个server互相通信，如果使用utf-8编码，则通信过程如下：

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1592939836038-3dfedb9a-9bbe-4804-99e2-c0d493d3d8eb.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_10%2Ctext_6K-t6ZuALUREdW5jYW4%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)





## 五 模块 | chardet

chardet是python的一个第三方库，常用于**编码识别**。

### 1 网页编码判断

```
from urllib import request
import chardet

rawdata = request.urlopen('https://www.baidu.com/').read()

tmp = chardet.detect(rawdata)   #识别网页的编码
print(tmp)

"""
{'encoding': 'ascii', 'confidence': 1.0}
confidence:置信水平
encoding:编码形式
"""
```

### 2 文件编码判断

```
import chardet

with open('text.txt', 'rb') as f:
    data = f.readline()

tmp = chardet.detect(data)  # 识别文件的编码
print(tmp)
"""
{'encoding': 'ascii', 'confidence': 1.0}
"""
```





## 六 模块 | pickle序列化与编码

pickle模块的Python3版本总是创建一个bytes对象

```
>>> import pickle
>>> pickle.dumps([1, 2, 3])     #pickle序列化
b'\x80\x03]q\x00(K\x01K\x02K\x03e.'

>>> pickle.dumps([1, 2, 3], protocol=0)
b'(lp0\nL1L\naL2L\naL3L\na.'


## 序列化与反序列化（在python2与python3中都生效）：
>>> import pickle
>>> pickle.dump([1, 2, 3], open('temp', 'wb'))  #对[1,2,3]进行pickle序列化
>>> pickle.load(open('temp', 'rb'))     #pickle反序列化
[1, 2, 3]
```



## 七 补充：编码相关的其他方法

### sys/locale模块

提供了一些获取**当前环境**下的默认编码的方法

```
# coding:gbk

import sys
import locale
 
def p(f):
    print '%s.%s(): %s' % (f.__module__, f.__name__, f())
 
# 返回当前系统所使用的默认字符编码
p(sys.getdefaultencoding)
 
# 返回用于转换Unicode文件名至系统文件名所使用的编码
p(sys.getfilesystemencoding)
 
# 获取默认的区域设置并返回元祖(语言, 编码)
p(locale.getdefaultlocale)
 
# 返回用户设定的文本数据编码
# 文档提到this function only returns a guess
p(locale.getpreferredencoding)
```



## 参考文献

[cnblogs | python编码](https://www.cnblogs.com/whatisfantasy/p/5951294.html)

[cnblogs | python编码问题在此终结](https://www.cnblogs.com/whatisfantasy/p/6422028.html)