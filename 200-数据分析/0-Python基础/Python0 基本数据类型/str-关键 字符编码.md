**声明**：以下内容主要基于[cnblogs | python编码](https://www.cnblogs.com/whatisfantasy/p/5951294.html)





在最新的Python 3版本中，字符串是以Unicode编码的，直接支持多语言

## 背景

计算机只能处理数字，如果要处理文本，就必须先把文本转换为数字才能处理。最早的计算机在设计时采用8个比特（bit）作为一个字节（byte），所以，一个字节能表示的最大的整数就是255（二进制11111111=十进制255），如果要表示更大的整数，就必须用更多的字节。比如两个字节可以表示的最大整数是`65535`，4个字节可以表示的最大整数是`4294967295`

- ASCII 127个字符
- GB2312 中文
- Shift_JIS 日文
- Euc-kr 韩文
- Unicode
- UTF-8
  - 如果统一成Unicode编码，乱码问题从此消失了
  - 但是，如果你写的文本基本上全部是英文的话，用Unicode编码比ASCII编码需要多一倍的存储空间，在存储和传输上就十分不划算

UTF-8编码把一个Unicode字符根据不同的数字大小编码成1-6个字节：常用的英文字母被编码成1个字节，汉字通常是3个字节，只有很生僻的字符才会被编码成4-6个字节

| 字符 | ASCII    | Unicode           | UTF-8                      |
| :--- | :------- | :---------------- | :------------------------- |
| A    | 01000001 | 00000000 01000001 | 01000001                   |
| 中   | x        | 01001110 00101101 | 11100100 10111000 10101101 |

UTF-8编码有一个额外的好处，就是ASCII编码实际上可以被看成是UTF-8编码的一部分，所以，大量只支持ASCII编码的历史遗留软件可以在UTF-8编码下继续工作。





### ASCII和Unicode的区别

ASCII编码是1个字节，而Unicode编码通常是2个字节

字母`A`用ASCII编码是十进制的`65`，二进制的`01000001`；

字符`0`用ASCII编码是十进制的`48`，二进制的`00110000`，注意字符`'0'`和整数`0`是不同的；



### 应用场景|记事本

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210115155108.png" alt="image-20210115155108034" style="zoom:80%;" />

### 应用场景|浏览网页

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210115155132.png" alt="image-20210115155132091" style="zoom:67%;" />

很多网页的源码上会有类似`<meta charset="UTF-8" />`的信息，表示该网页正是用的UTF-8编码





## 特性

字符串属于序列具有序列的通用操作

- 索引(indexing)
- 切片(slicing)
- 迭代(iteration)
- 加(adding)
- 乘(multiplying)



# 1 ASCII编码⭐

![image-20210121103644979](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210121103645.png)

例如一个字符a,你看到的是a,但在计算机中，一切都是以二进制存储的，ascii规定了每一个字符在计算机的二进制存储方式，a在计算机中的二进制存储方式是"01100001"，转成成10进制数就是97

- 数字0\~9在ascii表里的编码范围是48~57
- 大写字母在ascii表里的编码范围是65~90
- 小写字母在ascii表里的编码范围是97~122

```python
en_str = 'a'
en_ascii = ord(en_str)	#输出ASCII编码的十进制数
print(en_ascii, type(en_ascii))

print(chr(97))	#输出97对应的ASCII编码

# Output
97 <class 'int'>
a
```



# 2 unicode

搞一个大点的字符集，把这个世界上所有的字符都进行编码

规定了不同的字符在二进制上的表示形式，比如“升”这个汉字，它的unicode编码是 \u5347，5347是16进制，转换成成10进制是21319，转成二进制是101 0011 0100 0111，这一个汉字，至少需要2个字节来表示。

```python
# 1 转成unicode
ch = '升'
ch_unicode = ch.encode('unicode_escape')
print(ch_unicode)	#结果：b'\\u5347'

# 2 转成16进制形式
ch_hex = "0x" + str(ch_unicode,encoding='utf-8')[2:]
print(ch_hex)	#结果：0x5347

# 3 转成10进制
ch_int = eval(ch_hex)
print(ch_int)	#结果：21319

# 4 转成二进制
print(bin(ch_int))	#结果：0b101001101000111
```

unicode并没有规定这些字符所对应的二进制代码，但是并没有规定这些二进制代码该如何存储。这个汉字两个字节就能存储，但有些字符需要三个字节，像a这种字符，以前大家用ascii码的时候，用一个字节就能表示，在unicode里如果用两个或者更多字节表示，那么不是很浪费么，而且也与之前的ascii不兼容。



## Python3与unicode

python3中，字符串是以unicode编码的，当你想把一个字符串写入到磁盘上时，就==必须指定用哪种编码方式进行存储==，否则，就容易出错，比如下面的这段代码

```python
with open('city', 'w') as f:
    f.write('北京')
    
    
UnicodeEncodeError: 'ascii' codec can't encode characters in position 0-1: ordinal not in range(128)
```

字符串采用的是unicode字符集，但是==文件保存的时候，默认采用ascii编码==



- 解决办法1 指定utf-8编码
- 解决办法2 以二进制的形式写入文件

```python
with open('city', 'w', encoding='utf-8') as f:
    f.write('北京')
    

### 这种方法虽然也行，但并不常用，因为这需要每次写入都对字符串进行utf-8编码，不如第一种方法简单高效
with open('city', 'wb') as f:
    f.write('北京'.encode('utf-8'))
```







# 3 utf-8

一种变长的编码方式，ascii码表里的字符仍然用一个字节来存储，一个汉字用三个字节来存储

```python
ascii_a = 'a'
ascii_a_utf8 = ascii_a.encode(encoding='utf-8')
print(ascii_a_utf8, len(ascii_a_utf8))	#b'a' 1

ch = '升'
ch_utf8 = ch.encode(encoding='utf-8')
print(ch_utf8, len(ch_utf8))	#b'\xe5\x8d\x87' 3
```













# Python3 字符串的编码

| 编码          | 说明                                          |
| ------------- | --------------------------------------------- |
| ASCII 编码    | 只有大小写英文字母、数字和一些符号等127个字符 |
| (专用)        | 中文的GB2312编码 日文的Shift_JIS编码          |
| Unicode(统一) | u'字符串'  \u四位十六进制数                   |

- Python 不支持单字符类型(char)，单字符也在Python也是作为一个字符串使用
- `byt = b'Python'`：bytes数据，只接受 ASCII 码这个范围的字符



```python
### 针对单个字符
>>> ord('A')
65
>>> ord('中')
20013
>>> chr(66)
'B'
>>> chr(25991)
'文'


### 如果知道字符的整数编码
>>> '\u4e2d\u6587'
'中文'
```





## 编码转换

需要将文件保存到外设或进行网络传输时，就要进行编码转换，将**字符转换为字节**，以提高效率@二进制文件(b' ')

- 在网络上传输，或者保存到磁盘上，就需要把`str`变为以字节为单位的`bytes`
- 如果我们从网络或磁盘上读取了字节流，那么读到的数据就是`bytes`。要把`bytes`变为`str`，就需要用`decode()`方法

注意区分`'ABC'`和`b'ABC'`，前者是`str`，后者虽然内容显示得和前者一样，但`bytes`的每个字符都只占用一个字节

```python
### 转化为bytes
.encode(encoding='UTF-8',errors='strict')  #将字符转换为字节 (默认编码是 UTF-8)

### 转化为str
.decode(encoding='UTF-8',errors='strict')  #将字节转换为字符

# 说明：出错默认报ValueError,除非errors是ignore或replace
```

- 纯英文的`str`可以用`ASCII`编码为`bytes`
- 含有中文的`str`可以用`UTF-8`编码为`bytes`

如果bytes中只有一小部分无效的字节，可以传入errors='ignore'忽略错误的字节



=> 为了避免乱码问题，应当始终坚持使用UTF-8编码对`str`和`bytes`进行转换

- 通常在文件开头写上这两行

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
```

申明了UTF-8编码并不意味着你的`.py`文件就是UTF-8编码的，必须并且要确保==文本编辑器正在使用UTF-8 without BOM编码==

如果`.py`文件本身使用UTF-8编码，并且也申明了`# -*- coding: utf-8 -*-`，打开命令提示符测试就可以正常显示中文



### 函数len()

`len()`函数计算的是`str`的字符数

如果换成`bytes`，`len()`函数就计算字节数



## 输出方式

```python
## python2.7默认以字节的方式输出
s = "你好"
for i in s:
    print i

OUTPUT:      
�
�
�
�
�
�


## python3.5默认以字符的方式输出
s = "你好"
for i in s:
    print (i)

OUTPUT:        
你
好
```





## 基础|常见编码

- ascii：1个字节
- unicode：2个字节
- utf-8：英文1个字节，汉字3个字节

### 1 Python 2.X

- str(用于8位文本和二进制数据)
- unicode(用于宽字符文本)

在Python2中，通用的str类型填补了二进制数据的这一角色（==特指python3中的bytes类型==），因为字符串也只是**字节的序列**(单独的unicode类型处理宽字符串)。

在Python2中，为了兼容性而使用b'xxx'，但是它与'xxx'是相同的，并且产生一个str，并且，bytes只是str的同义词。

在Python3中，这二者都解决了bytes类型之间的差异。Python2中的u'xxx'和 U'xxx' Unicode字符串常量形式在Python3中已经**取消**了，而是使用'xxx'替代，因为所有的字符串都是Unicode，即便它们包含所有的ASCII字符。



### 2 Python 3.X

- str(用于**Unicode文本**，**包括ASCII**)
- bytes(用 于带有绝对**字节值**的二进制数据)
- bytearray(bytes的一种可变的形式)

bytes是一个**不可改变的字符序列**。

Python 3.0 bytes对象是较小整数的一个序列，其中每个整数都在0到255之间。在python3中bytes主要用于处理那些没有针对每个任意文本格式都编码的raw字节数据（图像和声音文件，以及用来与设备接口的打包数据，或者你想要用python的struct模块处理的C程序）。

Python3的bytes类型支持几乎str类型所做的所有相同操作：包括字符串方法、序列操作，甚至re模块模式匹配。



bytearray是bytes类型的一个变体，**它是可变的**并且支持原处修改。

它支持str和bytes所支持的常见的字符串操作，以及和列表相同的很多**原处修改操作**(例如，append和extend方法， 以及向索引赋值)。





### 3 文件分类

python3中的文件I/O一般分为两类

- ==文本文件str==：适用于文本内容
- 二进制文件Byte：适用于图像、数据流



使用建议：

1. 针对图像文件；其他程序创建的，而且必须解压的打包数据；或者一些==设备数据流==，则使用**bytes和二进制**模式文件处理它更合适。如果想要更新数据而**不在内存中产生其副本**，也可以选择使用bytearray。
2. 针对**实质是文本的内容**，例如程序输出、HTML、国际化文本或CSV或XML文件，可能要使用**str和文本模式**文件。





### 4 类型转换

Python 3.0下的类型转换：

1. str.encode()和bytes(S, encoding)把一个字符串转换为其**raw bytes形式**，并且在此过程中根据一个str创建一个bytes。
2. bytes.decode()和str(B, encoding)把raw bytes**转换为其字符串形式**，并且在此过程中根据一个bytes创建一个str。

```python
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

```python
# -*- coding: latin-1 -*-
```







## 基础|字符串与字节

### 1 bytes类型（和int,str类似的数据类型）

```python
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

```python
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











## 应用|不同场景下的字符编码方式

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







## 实例|编码问题

问：如下代码设置了在代码中添加了coding: utf-8，但是在cmd下面运行的时候还是输出乱码，这是什么情况？

```python
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

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

temp = "你好"
temp_unicode = temp.decode('utf-8')        #把utf-8解码成unicode格式
temp_gbk = temp_unicode.encode('gbk')      #把unicode编码成gbk格式
print temp_gbk
```

- Python3风格

utf-8编码能直接转成gbk格式的编码

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

temp = "你好"
temp_gbk = temp.encode('gbk')       #在python3中utf-8编码能直接转成gbk格式的编码
print temp_gbk
```



### 1 查看系统编码

```python
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

```python
UnicodeEncodeError: 'ascii' codec can't encode characters in position 0-1....
# Python2的系统编码改为utf-8，一般放在文件头
import sys
reload(sys)
sys.setdefaultencoding('utf-8')
```





## 库chardet|编码识别

chardet是python的一个第三方库，常用于**编码识别**。

### 1 网页编码判断

```python
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

```python
import chardet

with open('text.txt', 'rb') as f:
    data = f.readline()

tmp = chardet.detect(data)  # 识别文件的编码
print(tmp)
"""
{'encoding': 'ascii', 'confidence': 1.0}
"""
```





## 库pickle|序列化与编码

pickle模块的Python3版本==总是创建一个bytes对象==

```python
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







## 库sys/locale|获取**当前环境**的编码

提供了一些获取**当前环境**下的默认编码的方法

```python
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





# #参考文献

[cnblogs | python编码](https://www.cnblogs.com/whatisfantasy/p/5951294.html)

[cnblogs | python编码问题在此终结](https://www.cnblogs.com/whatisfantasy/p/6422028.html)