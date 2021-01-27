bytes类型是python3新增的一种数据类型，用来代表字节串。

- 字符串由多个字符串构成，以字符为单位进行操作
- 字节串由多个字节构成，以字节为单位进行操作。

除了操作单元不同，其他的用法基本相同，bytes类型数据也是不可变对象。



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