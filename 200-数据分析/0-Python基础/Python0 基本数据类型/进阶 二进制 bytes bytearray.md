# 二进制

二进制中只有两个可能的数：1和0

1kb=2^10=1024个字节



## 计算机中正数和负数表示方式

### 整数：有符号

0是正，1是负（1开头代表负数，0开头代表正数）

- 总共是32位的二进制，其中一位表示正数负数，剩下31位表示数字

- 不够用，引入64位使用，第一位表示正负，剩余63位表示数字





### 用科学计数法表示十进制 => 浮点数

示例：114.9可以写成0.1149* 10 ^ 3

32位浮点数中，第一位表示数字正负。后面八位存指数，剩下23位存有效数字





## 字符编码

@Python0 基本数据类型 `str-关键 字符编码`

字符串转换为字节的方法：

- ASCII
- Unicode
- UTF-8
- gbk



### 字符集

| 对象     |                                                      |
| -------- | ---------------------------------------------------- |
| 字符     | 根据字符编码方案转换为一个二进制数值，存储在计算机中 |
| 字符编码 | 定义在字符集上的映射规则                             |
|          | 字符—>计算机中的实际存储值                           |



# 字节序|大小端

| 模式     |                                                              |
| -------- | ------------------------------------------------------------ |
| 大端模式 | 数据的高字节保存在内存的低地址中                             |
|          | 类似于把数据当作字符串顺序处理：地址由小向大增加，而数据从高位往低位放(和阅读习惯一致) |
| 小端模式 | 将地址的高低和数据位权有效地结合起来                         |
|          | 高地址部分权值高，低地址部分权值低，和我们的逻辑方法一致     |

**位(bit)**：计算机中的最小数据单位，计算机存储的都是二进制0和1

**字节(Byte)**：字节是存储空间的基本计量单位，也是内存的基本单位，也是编址单位

- 例如，一个计算机的内存是4GB，就是该计算机的内存中共有4×1024×1024×1024个字节，意味着它有4G的内存寻址空间



针对一个32位的数值，如**0x1A2B3C4D**，总共四个字节，两个十六进制数表示一个字节

- 高位字节为**0x1A**
- 低位字节为**0x4D**
- 中间两个字节分别为**0x2B**和**0x3C**；



数值**0x1A2B3C4D**想要在计算机中正确使用，就必须要考虑在内存中将其对应的四个字节合理存储。假设内存的地址都是从低到高分配的，那么对于一个数值多个字节顺序存储就有两种存储方式：

![image-20210130105210529](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210130105210.png)



示例：存储 0x12345678到内存中

[Link: pack与unpack](https://www.jb51.net/article/136226.htm)

| address | big-endian | little-endian |
| :------ | :--------- | :------------ |
| 1000    | 0x12       | 0x78          |
| 1001    | 0x34       | 0x56          |
| 1002    | 0x56       | 0x34          |
| 1003    | 0x78       | 0x12          |

#记忆技巧

**大端模式**：低对高(或高对低)，门不当户不对，真令人头大

**小端模式**：低对低，门当户对，你侬我侬



## 意义

**大端模式：**

符号位在所表示的数据的内存的第一个字节中，便于快速判断数据的正负和大小

- CPU做数值运算时从内存中依顺序依次从低位地址到高位地址取数据进行运算，大端就会最先拿到数据的(高字节的)符号位



**小端模式：**

内存的低地址处存放低字节，所以在强制转换数据时不需要调整字节的内容

- 比如，把int---4字节强制转换成short---2字节，就可以直接把int数据存储的前两个字节给short就行，因为其前两个字节刚好就是最低的两个字节，符合转换逻辑
- 另外CPU做数值运算时从内存中依顺序依次从低位地址到高位地址取数据进行运算，开始只管取值，最后刷新最高位地址的符号位就行，这样的运算方式会更高效一些



两种模式各有优点，存在“你有我无，你无我有”的特点，所以造就了不同的硬件厂商基于不同的效率(角度)考虑，有了不同的硬件设计支持，最终形成了计算机各个相关领域目前并没有采用统一的字节序，没有统一标准的现状。



## 常见应用

**常见的CPU：**

- PowerPC、IBM是大端模式
- x86是小端模式

**ARM既可以工作在大端模式，也可以工作在小端模式，一般ARM都默认是小端模式。**

**一般通讯协议都采用的是大端模式。**



常见文件的字节序如下：

| 模式          |              |
| ------------- | ------------ |
| Big Endian    | JPEG         |
|               | Adobe PS     |
| Little Endian | BMP          |
|               | GIF          |
|               | RTF          |
| Variable      | DXF(AutoCAD) |





理解“大端”“小端”，才能在跨平台、跨芯片、跨系统，跨网络通信时，实时对内存字节序进行检查和转换，保证传递内容的正确性



## 如何检测模式？

1 **通过C代码检测当前计算机环境采用的是大端模式还是小端模式**

借助联合体union的特性实现：

- 联合体类型数据所占的内存空间等于其最大的成员所占的空间
- 对联合体内部所有成员的存取都是相对于该联合体基地址的偏移量为 0 处开始，也就都是从该联合体所占内存的首地址位置开始

```c
int main()
{
    union{
      int a;  //4 bytes
      char b; //1 byte
    } data;
  
    data.a = 1; //占4 bytes，十六进制可表示为 0x 00 00 00 01

    //b因为是char型只占1Byte，a因为是int型占4Byte
    //所以，在联合体data所占内存中，b所占内存等于a所占内存的低地址部分 
    if(1 == data.b){ //走该case说明a的低字节，被取给到了b，即a的低字节存在了联合体所占内存的(起始)低地址，符合小端模式特征
      printf("Little_Endian\n");
     } else {
      printf("Big_Endian\n");
     }
   return 0;
}
```



2 将int强制类型转换成char单字节，判断起始存储位置内容实现

- int是4个字节
- char是1个字节

```c
#include <stdio.h>
int main()
{
    int a = 1; //占4 bytes，十六进制可表示为 0x 00 00 00 01
    
     //b相当于取了a的低地址部分 
    char *b =(char *)&a; //占1 byte
    
    if (1 == *b) {//走该case说明a的低字节，被取给到了b，即a的低字节对应a所占内存的低地址，符合小端模式特征
        printf("Little_Endian!\n");
    } else {
        printf("Big_Endian!\n");
    }
    return 0;
}
```



3 **也可以通过MSB/LSB实现大端小端的判断和检测**

**PS:**网上有很多，用MSB和LSB讲大端和小端的描述，我们一定需要注意

- **大端和小端描述的是字节之间的关系**
- **MSB、LSB描述的是Bit位之间的关系**。

字节是存储空间的基本计量单位，所以通过高位字节和低位字节来理解大小端存储是最为直接的



**MSB**: Most Significant Bit ------- 最高有效位(指二进制中最高值的比特)

**LSB:** Least Significant Bit ------- 最低有效位(指二进制中最小值的比特)

```c
#include <stdio.h>

int main(void)
{
    union {
      struct {
        char a:1; //定义位域为 1 bit[不知位域何物，还请先自行查阅一下，后续文章也会专门讲到] 
      } s;
      char b;
    } data;

    data.b = 8;//8(Decimal) == 1000(Binary)，MSB is 1，LSB is 0

    //在联合体data所占内存中，data.s.a所占内存bit等于data.b所占内存低地址部分的bit
    if (1 == data.s.a) {//走该case说明data.b的MSB是被存储在union所占内存的低地址中，符合大端序的特征
        printf("Big_Endian\n");
    } else {
        printf("Little_Endian\n");
    }
    return 0;
}
```





# bytes

bytes类型是python3新增的一种数据类型，用来代表字节串。

- 字符串由多个字符串构成，以字符为单位进行操作
- 字节串由多个字节构成，以字节为单位进行操作。

除了操作单元不同，其他的用法基本相同，bytes类型数据也是不可变对象。



bytes是byte的序列，是一个不可变的数据类型

```python
### bytes()
>>> a = bytes()
>>> a	#b''


### bytes(int):直接定义字节的个数, 但是每个字节里面是空的
>>> a = bytes(3)
>>> a	#b'\x00\x00\x00'  这是ASCII 0 ，不是阿拉伯数字0，阿拉伯数字0是十进制48，十六进制30.


### bytes(iteratable): 创造可迭代对象的元素个数相等的字节，然后把每个元素填充进去
# 注意，必须是整型int 的可迭代对象
>>> a = bytes(range(15))
>>> a	#b'\x00\x01\x02\x03\x04\x05\x06\x07\x08\t\n\x0b\x0c\r\x0e'
# 看ASCII表，9 是\t，10是\n，13是\r。这就是用ASCII表来解码的字节。
# 前面那些 00 01 02 03，也有自己的意思，但是没办法用字符表示出来，所以就用它的十六进制表示法表示出来


### bytes(byte) or bytes(‘string’,encode)
>>> a = bytes(range(3))
>>> a 	#b'\x00\x01\x02'

>>> b = a
>>> b	#b'\x00\x01\x02'

c = bytes(b'fuck')	#等价于 bytes('fuck','utf-8')

### 下述两者
e = b'fuck'
f = b'\x61\x62\x63'
```



## 修改

bytes 和 str 一样，都是不可修改的类型，所以，所谓的修改，都是创造一个新的bytes 和str

```python
# 替换字节
b'fuck'.replace(b'f',b's')
b'suck'


# 查找某个字节的索引号
b'fuck'.find(b'u')
1
```



## hex <=> bytes

用一串由十六进制数字组成的字符串，来表示字节

```python
### hex => bytes
# 注意，在括号内，空格是无效的，相当于没输入，所以想打出空格，就要打ASCII编码表里空格对应的编码，也就是20
>>> bytes.fromhex('53 75 63 6b 20 4d 79 20 44 69 63 6b')
b'Suck My Dick'


### bytes => hex
>>> 'Suck My Dick'.encode().hex()
'5375636b204d79204469636b'


>>> 'Suck My Dick'.encode()[0]	
83	#返回的是十进制的数字，换成十六进制，也就是我们上面输入的S 的编码53
```





@示例|探索

```python
e = b'fuck'
e.hex()		#'6675636b'

f = b'\x66\x75\x63\x6b'

e==f	#True
```





## int <=> bytes 

无论字符串str 还是整型int ，在内存中都是以字节来储存，那么二者之间可以通过bytes 互相转化



`int.from_bytes(bytes,byteorder)`

```python
# byteorder = 'big' 是指大小端模式
>>> int.from_bytes(b'fuck','big')	#把字符串 ‘fuck’，通过解码，以整型int（十进制）表示出来
1718969195
```



`int.to_bytes(length,byteorder)`：把一个整型的值，转换成字节

- length 是指定有多少个字符，只能多不能少，多了话，多余的字符会被十六进制的0填充
- 少了的话会报错。

```python
>>> 1718969195.to_bytes(4,'big')
b'fuck'
```



# bytearray

bytearray是字节数组，是一个有序、可变的数据类型 (范围：range 0<=x<256)

bytearray 类型的操作，和bytes 类型的操作基本差不多

```python
# 参数是字符串: 必须给出编码以及可选的错误参数
b = bytearray(u'未来的兰草说', encoding='utf-8')   # 参数里面进行编码

print(b)		#编码格式：\xe6\x9c\xaa\xe6\x9d\xa5\xe7
print(type(b))

# 空的字节数组
bytearray()

# 参数是int
b = bytearray(23)   # 编码格式：\x00的个数    本代码行是23个
print(b)
print(type(b))

# 参数是list 可迭代
b = bytearray([0, 2, 4, 5])        
print(b)	##结果：bytearray(b'\x00\x02\r\x04\x05'）

# bytearray(‘string’,encode): 把字符串string，按encode 指定的编码来解码，然后把解码之后的十六进制数字，放入字节中，组成字节数组。


# bytearray(bytes_or_buffer): 从一个字节序列或者是buffer中，复制一个新的bytearray
```





## 修改

因为是可变的，所以bytearray 可以就地修改，修改方法和list 的修改方法类似

| 函数                                    |                                                              |
| --------------------------------------- | ------------------------------------------------------------ |
| **bytearray.replace(old,new)**          | 这个替换不是就地修改，返回值是修改之后的，一个新的bytearray  |
| **bytearray.find(sub)**                 | 寻找目标的子字节，然后返回索引号                             |
| **bytearray.fromhex(‘string’)**         | 把一个字符串（这个字符串必须是两个字符的十六进制形式，例如 ‘5375636b204d79204469636b’ ），放在一个bytearray 里 |
| **bytearray(‘string’,encode()).hex()**  | 字符串，以某种编码形式，转换成bytearray 类型的数据，然后再用十六进制表示出来 |
| **bytearray(bytes)[index]**             | 返回索引为index 的字节的十进制数字，类型为int                |
|                                         |                                                              |
| ##类似list的修改##                      |                                                              |
| **bytearray.append(int)**               |                                                              |
| **bytearray.insert(index,int)**         |                                                              |
| **bytearray.extend(iteratable_of_int)** |                                                              |
| **bytearray.pop()**                     |                                                              |
| **bytearray.remove(value)**             |                                                              |
| **bytearray.clear()**                   |                                                              |
| **bytearray.reverse()**                 |                                                              |





# bytes <=> bytearray二进制数组

```python
# bytes和bytearray的转换：因为byte和bytearray都是同一类型（字节），所以不用encode以及decode
s = b'wuhan'
b = bytearray(s)       # 就把bytes类型的wuhan转换成了bytearray
z = bytes(b)           # 就把bytearray类型转换为了bytes
```





# 应用场景

## 计算md5

计算md5值的过程中，有一步要使用update方法，该方法只接受bytes类型数据

```python
import hashlib

string = "123456"

m = hashlib.md5()       # 创建md5对象
str_bytes = string.encode(encoding='utf-8')
print(type(str_bytes))	#<class 'bytes'>
m.update(str_bytes)   # md5()对象的update方法：只接收bytes类型数据作为参数
str_md5 = m.hexdigest()     # 得到散列后的字符串

print('MD5散列前为 ：' + string)		#MD5散列前为 ：123456
print('MD5散列后为 ：' + str_md5)	#MD5散列后为 ：e10adc3949ba59abbe56e057f20f883e
```





## 二进制读写文件

使用二进制方式读写文件时，均要用到bytes类型

- 二进制写文件时，write方法只接受bytes类型数据，因此需要先将字符串转成bytes类型数据
- 读取二进制文件时，read方法返回的是bytes类型数据，使用decode方法可将bytes类型转成字符串。

```python
f = open('data', 'wb')
text = '二进制写文件'
text_bytes = text.encode('utf-8')
f.write(text_bytes)
f.close()

f = open('data', 'rb')
data = f.read()
print(data, type(data))	#b'\xe4\xba\x8c\xe8\xbf\x9b\xe5\x88\xb6\xe5\x86\x99\xe6\x96\x87\xe4\xbb\xb6' <class 'bytes'>
str_data = data.decode('utf-8')
print(str_data)	#二进制写文件
f.close()
```





## socket编程

使用socket时，不论是发送还是接收数据，都需要使用bytes类型数据 => 网络字节流

```python
import socket


url = 'www.zhangdongshengtech.com'
port = 80
# 创建TCP socket
sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
# 连接服务端
sock.connect((url, port))
# 创建请求消息头
request_url = 'GET /article-types/6/ HTTP/1.1\r\nHost: www.zhangdongshengtech.com\r\nConnection: close\r\n\r\n'
print(request_url)	
'''
GET /article-types/6/ HTTP/1.1
Host: www.zhangdongshengtech.com
Connection: close
'''
# 发送请求
sock.send(request_url.encode())
response = b''
# 接收返回的数据
rec = sock.recv(1024)
while rec:
    response += rec
    rec = sock.recv(1024)

print(response.decode())
print(type(response))		#<class 'bytes'>
```





# 字符串与bytes类型数据转换

将字符串转成bytes类型数据需要使用encode方法

将bytes类型数据转换成字符串，需要使用decode方法

```python
string = "爱我中华"
bstr = string.encode(encoding='utf-8')
print(bstr, type(bstr))

string = bstr.decode(encoding='utf-8')
print(string, type(string))
```





# #参考文献

[Link: 知乎|大小端模式](https://zhuanlan.zhihu.com/p/144718837)