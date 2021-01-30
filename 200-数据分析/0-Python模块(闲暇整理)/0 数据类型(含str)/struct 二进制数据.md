Python提供了一个`struct`模块来解决`bytes`等二进制数据类型的转换



Python没有专门处理字节的数据类型。但由于`b'str'`可以表示字节，所以，用字节数组(二进制)来表示字符串。

而在C语言中，我们可以很方便地用struct、union来处理字节，以及字节和int，float的转换。



在Python中，比方说要把一个32位无符号整数变成字节，也就是4个长度的`bytes`，你得配合位运算符这么写：

```python
>>> n = 10240099
>>> b1 = (n & 0xff000000) >> 24
>>> b2 = (n & 0xff0000) >> 16
>>> b3 = (n & 0xff00) >> 8
>>> b4 = n & 0xff
>>> bs = bytes([b1, b2, b3, b4])
>>> bs
b'\x00\x9c@c'
```

非常麻烦。如果换成浮点数就无能为力了。



# struct

`struct`的`pack`函数把任意数据类型变成`bytes`：

```python
>>> import struct
>>> struct.pack('>I', 10240099)
b'\x00\x9c@c'
```

`pack`的第一个参数是处理指令，`'>I'`的意思是：

- `>`表示字节顺序是big-endian，也就是网络序
- `I`表示4字节无符号整数



`unpack`把`bytes`变成相应的数据类型：

```python
>>> struct.unpack('>IH', b'\xf0\xf0\xf0\xf0\x80\x80')
(4042322160, 32896)
```

根据`>IH`的说明，后面的`bytes`依次变为

- `I`：4字节无符号整数
- `H`：2字节无符号整数



所以，尽管Python不适合编写底层操作字节流的代码，但在对性能要求不高的地方，利用`struct`就方便多了。



## 字符对应的格式(官方文档)

| Character | Byte order             | Size     | Alignment |
| :-------- | :--------------------- | :------- | :-------- |
| `@`       | native                 | native   | native    |
| `=`       | native                 | standard | none      |
| `<`       | ==little-endian==      | standard | none      |
| `>`       | ==big-endian==         | standard | none      |
| `!`       | network (= big-endian) | standard | none      |



| Format | C Type               | Python type           | Standard size | Notes    |
| :----- | :------------------- | :-------------------- | :------------ | :------- |
| `x`    | pad byte             | no value              |               |          |
| `c`    | `char`               | ==bytes of length 1== | 1             |          |
| `b`    | `signed char`        | integer               | 1             | (1), (2) |
| `B`    | `unsigned char`      | integer               | 1             | (2)      |
| `?`    | `_Bool`              | bool                  | 1             | (1)      |
| `h`    | `short`              | integer               | 2             | (2)      |
| `H`    | `unsigned short`     | ==integer==           | 2             | (2)      |
| `i`    | `int`                | integer               | 4             | (2)      |
| `I`    | `unsigned int`       | ==integer==           | 4             | (2)      |
| `l`    | `long`               | integer               | 4             | (2)      |
| `L`    | `unsigned long`      | integer               | 4             | (2)      |
| `q`    | `long long`          | integer               | 8             | (2)      |
| `Q`    | `unsigned long long` | integer               | 8             | (2)      |
| `n`    | `ssize_t`            | integer               |               | (3)      |
| `N`    | `size_t`             | integer               |               | (3)      |
| `e`    | (6)                  | float                 | 2             | (4)      |
| `f`    | `float`              | float                 | 4             | (4)      |
| `d`    | `double`             | float                 | 8             | (4)      |
| `s`    | `char[]`             | bytes                 |               |          |
| `p`    | `char[]`             | bytes                 |               |          |
| `P`    | `void *`             | integer               |               | (5)      |



# 应用|bmp位图

Windows的位图文件（.bmp）是一种非常简单的文件格式



BMP格式采用小端方式存储数据，文件头的结构按顺序如下：

- 两个字节：`'BM'`表示Windows位图，`'BA'`表示OS/2位图； 
- 一个4字节整数：表示位图大小； 
- 一个4字节整数：保留位，始终为0； 
- 一个4字节整数：实际图像的偏移量； 
- 一个4字节整数：Header的字节数； 
- 一个4字节整数：图像宽度； 
- 一个4字节整数：图像高度；
-  一个2字节整数：始终为1； 
- 一个2字节整数：颜色数。



组合起来用`unpack`读取：

- 读入==前30个字节==

```python
>>> s = b'\x42\x4d\x38\x8c\x0a\x00\x00\x00\x00\x00\x36\x00\x00\x00\x28\x00\x00\x00\x80\x02\x00\x00\x68\x01\x00\x00\x01\x00\x18\x00'

>>> struct.unpack('<ccIIIIIIHH', s)	#第一个参数是解码的格式：<表示小端
(b'B', b'M', 691256, 0, 54, 40, 640, 360, 1, 24)
```

结果显示，`b'B'`、`b'M'`说明是Windows位图，位图大小为640x360，颜色数为24。



## 示例|检查文件是否是位图文件

```python
# -*- coding: utf-8 -*-
import base64, struct
bmp_data = base64.b64decode('Qk1oAgAAAAAAADYAAAAoAAAAHAAAAAoAAAABABAAAAAAADICAAASCwAAEgsAA' +
                   'AAAAAAAAAAA/3//f/9//3//f/9//3//f/9//3//f/9//3//f/9//3//f/9//3//f/9//3/' +
                   '/f/9//3//f/9//3//f/9/AHwAfAB8AHwAfAB8AHwAfP9//3//fwB8AHwAfAB8/3//f/9/A' +
                   'HwAfAB8AHz/f/9//3//f/9//38AfAB8AHwAfAB8AHwAfAB8AHz/f/9//38AfAB8/3//f/9' +
                   '//3//fwB8AHz/f/9//3//f/9//3//f/9/AHwAfP9//3//f/9/AHwAfP9//3//fwB8AHz/f' +
                   '/9//3//f/9/AHwAfP9//3//f/9//3//f/9//38AfAB8AHwAfAB8AHwAfP9//3//f/9/AHw' +
                   'AfP9//3//f/9//38AfAB8/3//f/9//3//f/9//3//fwB8AHwAfAB8AHwAfAB8/3//f/9//' +
                   '38AfAB8/3//f/9//3//fwB8AHz/f/9//3//f/9//3//f/9/AHwAfP9//3//f/9/AHwAfP9' +
                   '//3//fwB8AHz/f/9/AHz/f/9/AHwAfP9//38AfP9//3//f/9/AHwAfAB8AHwAfAB8AHwAf' +
                   'AB8/3//f/9/AHwAfP9//38AfAB8AHwAfAB8AHwAfAB8/3//f/9//38AfAB8AHwAfAB8AHw' +
                   'AfAB8/3//f/9/AHwAfAB8AHz/fwB8AHwAfAB8AHwAfAB8AHz/f/9//3//f/9//3//f/9//' +
                   '3//f/9//3//f/9//3//f/9//3//f/9//3//f/9//3//f/9//3//f/9//38AAA==')


def bmp_info(data):
    # 取了前面的30 bytes(左闭右开：0-29)
    data_img = data[:30]
    res = struct.unpack('<ccIIIIIIHH', data_img)	#一定要恰好匹配30个bytes，因为前面的format对应30个bytes
    flag_bmp = False
    if res[0]==b'B' and res[1]==b'M':	#if bytes.decode(bm[0]+bm[1]) == 'BM':
        flag_bmp = True
        width = res[6]
        height = res[7]
        color = res[-1]

    return {
        'width': width,
        'height': height,
        'color': color
    }


# 测试
bi = bmp_info(bmp_data)
assert bi['width'] == 28
assert bi['height'] == 10
assert bi['color'] == 16
print('ok')
```







# #参考文献

[Link: 教程|廖雪峰](https://www.liaoxuefeng.com/wiki/1016959663602400/1017685387246080)

[Link: 官方文档](https://docs.python.org/3/library/struct.html#format-characters)

