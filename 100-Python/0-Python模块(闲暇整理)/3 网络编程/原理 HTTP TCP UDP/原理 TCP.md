TCP协议全称是Transmission Control Protocol，传输控制协议

是一种面向连接的、可靠的、==基于字节流==的==传输层==通信协议，由IETF的RFC793定义

进行TCP通信一般由3个步骤组成：

1. 创建连接
2. 数据传送
3. 终止连接



TCP3次握手就发生在建立连接的阶段

TCP客户端与服务端进行通信的示意图：

![image-20210201134955181](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210201134955.png)

# TCP Server

理解每一行代码背后的意义和原理, 涉及到的知识点包括：

- 绑定ip
- 绑定端口
- 监听
- 接受一个客户端的连接请求
- 接收数据
- 关闭连接

```python
import socket

# 指定协议
server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# 让端口可以重复使用
server.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
# 绑定ip和端口
server.bind(('0.0.0.0', 8080))
# 监听
server.listen(1)
# 等待消息
clientsocket, address = server.accept()
# 接收消息
data = clientsocket.recv(1024)
# 关闭socket
clientsocket.close()
server.close()
```



## SO_REUSEADDR

如果没有下面这行代码`server.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)`

一旦server意外死亡或人为干掉，那么大约两分钟内，无法重新启动该server

这是因为，建立socket连接后，主动断开连接的一方所用的端口会进入到TIME_WAIT状态， 如果短时间内再次启动server，就会引发异常 OSError: [Errno 48] Address already in use





## 绑定IP|接受的IP地址

`server.bind(('0.0.0.0', 8080))`表示==接受所有ip发起的tcp连接==

如果写成127.0.0.1，那么就只接受本机上发出的socket连接了





## 绑定端口

一台主机，可以开放的端口范围是从0~65535，这个范围，是由TCP/IP协议决定的

在该协议中，TCP的头结构如下

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210201135402.png" alt="image-20210201135401916" style="zoom:80%;" />

在基于IPv4的网络中，源端口号和目标端口号都是16位的，因此最大只能是65535。

端口0在网络编程中有着特殊作用，尤其在unix系统中，如果你申请打开0端口号，0更像是一个统配符，==系统会寻找一个合适的端口供你使用，而不是按照你的要求打开端口0==。

在TCP/IP 协议中，0端口号是保留的，在TCP和UDP中都不应该使用，1-1023为系统端口，1024-65535为用户端口。



## 监听

`server.listen(1)`

listen方法开始监听客户端连接请求，listen的参数设置为n，并不是表示最多可以建立n个连接，而是==在服务器拒绝连接之前，操作系统可以挂起的最大连接数量==，我这里设置为1，但可以有多于1个客户端向服务端发起连接。

对于这个参数：

- tornado默认设置为128，我觉得有点小了

- nginx一般推荐大一些，搞个2048也没问题，它是用多个进程进行accept操作，一个进程处理不过来了，会换另一个进程，因此处理的非常快





## accept⭐(三次握手)

accept()接受一个客户端的连接请求，并返回一个新的套接字，这样的解释正确但还不够深入。

作为服务端，系统会维护一个syn队列和一个accept队列：

- 客户端发起连接=> 发送syn包时，系统会把这个连接信息放入到syn队列里，然后返回syn+ack包
- 等收到客户端最后发回来的ack包时，会从syn队列里把连接信息放入到accept队列里
- accept会从accept队列里取连接， 这里的连接都已经完成了3次握手。







## 接收数据

`data = server.recv(1024)`

recv负责接收套接字的数据， 参数设置成1024，并不意味着一定会接收到1024字节数据，即便对方发的数据超过1024

recv执行一次究竟能收到多少数据，取决接收缓冲区里有多少数据以及TCP/IP协议，如果已知对方会发送5000字节的数据，那么我们==需要自己统计已经接收了多少，还剩余多少没有收到==





## 关闭连接

我这里用了close(),也可以用shutdown()，但你千万不要以为关闭socket连接有两种方法，其实他们很不同。

- shutdown破坏了连接
- 而close只是关闭本进程的socket id,不破坏连接，此时，其他进程仍然可以在该连接上发送和接收消息。

前面提到过，nginx使用多个进程进行accept操作，这些进程都是fork出来的，他们共同使用同一个socket连接

如果某一个进程执行close,并不影响其他进程继续使用，每进行一次close,计数器就会减一，等到为0时，就是所有进程都执行close了，此时，套接字资源才会被回收。





# TCP Client

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210201155117.png" alt="image-20210201155117267" style="zoom:80%;" />

创建TCP 客户端与 TCP server通信：

```python
import socket
import time


host = '127.0.0.1'
port = 8081
addr = (host, port)
client = socket.socket(socket.AF_INET,socket.SOCK_STREAM)

# 连接server
client.connect(addr)
# 向server发送数据
client.send(b'I am client')
# 接收server返回的数据
revcdata = client.recv(1024)
# 收到的数据都是bytes类型：转化为字符串 便于显示
print(revcdata.decode(encoding='utf-8'))
time.sleep(1)
client.close()
```





为了配合客户端测试，同时编写服务端代码：

```python
import socket

# 指定协议
server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
# 让端口可以重复使用
server.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
# 绑定ip和端口
server.bind(('0.0.0.0', 8081))
# 监听
server.listen(1)
# 等待连接：等待client的连接
clientsocket, address = server.accept()
# 接收消息
data = clientsocket.recv(1024)
clientsocket.send('我已经收到'.encode(encoding='utf-8'))	#服务端发送信息
clientsocket.close()
server.close()
```

先启动服务端，然后在另一个终端里启动客户端，客户端与服务端完成一次通信，随后都关闭



## B与C端的比较

1. 客户端代码没有bind
2. 客户端代码没有listen
3. 客户端代码没有accept
4. ==客户端需要connect==





## connect

服务端不需要去connect谁，它只需要等待客户端来连接它就可以了

所以客户端必须bind，指定监听的IP和端口

因此，在socket应用程序中，==必须先启动服务端==

- 启动后服务端进行listen，客户端发起connect，服务端accept





## bind

客户端其实也能进行bind操作，指明用哪个端口与服务端通信



但通常来说不会这么做，原因很简单，服务端一般来说要一直提供服务，轻易不会停下来，而==客户端却可能经常停下来==，想想上一篇讲的time_wait

再有，如果客户端的程序是多线程的，多个线程里总不能同时去bind同一个端口吧。

所以不bind是最好的，==让操作系统来随机分配端口==







## send

send方法用于发送数据，数据类型必须是bytes类型，字符串与bytes类型之间的相互转换可以参考[bytes 字节串](http://www.coolpython.net/python_senior/data_type/bytes.html), send函数执行完了，不代表数据已经被发送出去了，函数返回，只是表示这些数据==已经被放入到发送缓冲区==了，==至于何时被发送到网络上，由socket协议自己来决定==。



send方法的返回值是本次发送数据的长度，假设你send的数据的长度是1000，send函数执行完了，其返回值可能小于1000，比如返回值是900，这就意味着只有前900个字节的数据被放入到发送缓冲区了

因此在send时，你必须检查send函数的返回值并和你要发送的数据长度做对比，遇到我刚才说的情况，你只==好重新发送失败的那部分==,可靠的写法类似这样

```python
data = b'I am client'
sendlen = 0
while sendlen < len(data):
    successlen = client.send(data[sendlen:])
    sendlen += successlen
```





## recv

关于recv,虽然参数是1024，但这不代表你就能接收到这么多的数据，这个参数的意思是我想接收1024个字节的数据，但socket协议最终给你的可能比你要求的少

想想也合理，因为==TCP是不维护数据边界的==，一次给你多少不影响你的分析，但绝不会超过你所要求的，如果缓冲区里有数据了，也没必要非得等到缓冲区的数据长度达到1024再给你。

如果recv得到的数据长度是0，那就表示对方已经断开了连接





## 分包与粘包

一个tcp包最多可以装1460个字节的数据，所以如果你要发送的数据长度是2000个字节，那么这些数据最少要分成两份，也就是两个TCP包发过去，如果你频繁的发送长度只有10个字节的数据，那么为了不浪费网络带宽，这些==大量的长度只有10的数据会被装进同一个TCP包中==一起发送过去，这就是TCP的分包与粘包。



这个对于接收方来说就是个麻烦，由于TCP自己不维护数据边界，因此应用程序本身必须对此进行处理，以便应对：

- 一段完整数据被分多个包发送（分包）
- 多个小段数据被一起发送（粘包）的情况







# 三次握手

TCP提供了一种可靠、面向连接、字节流、传输层的服务(单播协议)

- 采用三次握手建立一个连接
- 采用4次挥手来关闭一个连接





@概念|连接

客户端和服务器的内存里保存的一份关于对方的信息，如ip地址、端口号等

@概念|字节流

处理IP层或以下的层的丢包、重复以及错误问题

在连接的建立过程中，双方需要交换一些连接的参数。这些参数可以放在TCP头部











# 四次挥手















