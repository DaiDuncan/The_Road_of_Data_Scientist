# Python3 vs Python2

| Python3                                  | Python2 |
| ---------------------------------------- | ------- |
| str        针对文本数据     => 用于显示  | unicode |
| bytes   针对二进制数据 => 用于存储和传输 | str     |

Python2里面的str和unicode是可以混用的，在==都是英文字母的时候str和unicode没有区别==

Python3 严格区分文本（str）和二进制数据（bytes）：

- 文本总是unicode，用str类型
- 二进制数据则用bytes类型



Python3中，严格区分了str和bytes，不同类型之间操作就会抛出TypeError的异常

```python
In [1]: 'a' == b'a'
Out[1]: False

In [2]: 'a' in b'a'
---------------------------------------------------------------------------
TypeError Traceback (most recent call last)
<ipython-input-10-ca907fd8856f> in <module>()
----> 1 'a' in b'a'

TypeError: a bytes-like object is required, not 'str'
```



# str bytes bytearray

str是unicode的序列

```python
### str类型：                       
a = u'吴某'         
print(type(a))  	#输出为<class'str'>               
```



```python
### bytes类型：
l = b'zhangxinyi'   #b''表示二进制字符串类型（作用：存储图片，音频，电影），序列类型，不可变类型
print(type(l)) 		#输出为<class'bytes'>
```





## str <=> bytes 

bytes一般来自：

- 网络读取的数据
- 从二进制文件（图片等）读取的数据
- 以二进制模式读取的文本文件(.txt, .html, .py, .cpp等)



str可以encode为bytes，但是bytes不一定可以decode为str

- bytes.decode(‘latin1’)可以称为str，也就是说decode使用的编码决定了decode()的成败
- UTF-8编码的bytes字符串用GBK去decode()也会出错

![image-20210130093212617](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210130093212.png)

```python
### encoding 指的是具体的编码规则的名称 对于中文来说，可以是这些值：‘utf-8’, ‘gb2312’, ‘gbk’, ‘big5’ 等
str.encode(‘encoding’) -> bytes

bytes.decode(‘encoding’) -> str
```

表示同样的内容，str的长度要小于或等于bytes的长度(参考Unicode、UTF-8的编码规则)



```python
In [16]: a = 'T恤'
In [17]: a
Out[17]: 'T恤'

In [18]: len(a)
Out[18]: 2

    
In [19]: b = a.encode('utf8')	#指明str需要utf8的编码
In [20]: b
Out[20]: b'T\xe6\x81\xa4'

In [21]: a == b
Out[21]: False

    
In [22]: c = a.encode('gbk')
In [23]: c
Out[23]: b'T\xd0\xf4'

    
In [24]: b == c
Out[24]: False

In [25]: a == c
Out[25]: False
```

上面str和bytes之间的转换是==针对文本内容==的

要是其它二进制内容（比如图片）时，bytes就不能decode成str了

- 因为图片中的二进制数据不符合文本数据的UTF-8编码规则

```python
In [29]: img = open('str-bytes.jpg', 'rb').read()

In [30]: type(img)
Out[30]: bytes

In [31]: img.decode('utf8')
---------------------------------------------------------------------------
UnicodeDecodeError Traceback (most recent call last)
<ipython-input-31-c9e28f45be95> in <module>()
----> 1 img.decode('utf8')

UnicodeDecodeError: 'utf-8' codec can't decode byte 0xff in position 0: invalid start byte
```





上面获得图片数据时，我们用到了open()来读取文件，文件存储的无非是文本和二进制这两种格式，读写文件时也有分清楚编码：

```python
In [32]: open('z.txt', 'w').write('T恤')
Out[32]: 2

In [33]: open('z.txt', 'w').write(img)
---------------------------------------------------------------------------
TypeError Traceback (most recent call last)
<ipython-input-33-4a88980b3a54> in <module>()
----> 1 open('z.txt', 'w').write(img)

TypeError: write() argument must be str, not bytes
---------------------------------------------------------------------------

In [34]: open('z.txt', 'wb').write(img)
Out[34]: 12147
```

读写二进制数据（如图片）时，要加’b’参数，b代码binary（二进制）

读写文本数据时，一般加’b’, open()会自动呈现str







## str <=> bytearray 

```python
#bytearray和str的转化：
s = 'wuhan'
b = bytearray(s,encoding='utf-8')	#把str类型转换为bytearray类型

z = b.decode(encoding='utf-8')      #把bytearray类型转换为str类型   等价于 c = b.decode('utf-8')  或 c = str(b, encoding='utf-8')
```



