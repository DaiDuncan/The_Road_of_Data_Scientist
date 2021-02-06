平时我们使用浏览器浏览web资源，写爬虫的时候，我们会使用封装好的库，比如requests，或者使用爬虫框架。

工欲善其事必先利其器，顶层封装好的东西，是为了我们使用着方便，节省开发时间，尽管各种http库功能强大，但学习底层的技术仍然有着实践意义，==只有了解底层，才能真正理解顶层的封装和设计，遇到那些艰难的问题时，才会有思路，有方案==。



## 用socket发送http请求

浏览器也好，爬虫框架也罢，在最底层，都是在使用socket发送http请求，然后接收服务端返回的数据，浏览器会对返回的数据进行渲染，最终呈现在我们眼前，==爬虫框架相比于浏览器，只是少了一个渲染的过程==。



用socket发送http请求，首先要建立一个TCP socket，然后连接到服务端socket，http请求的3次握手，本质上是TCP socket建立连接的3次握手。

连接建立好了以后，就要发送数据了，这里的数据，可不是随意发送的，而是要==遵照http协议==

下面的代码，演示了socket发送http请求的过程

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

# 发送请求：遵循HTTP协议
sock.send(request_url.encode())

response = b''
# 接收返回的数据
rec = sock.recv(1024)
while rec:
    response += rec
    rec = sock.recv(1024)
print(response.decode())
```



重点要了解的是http协议，request_url的内容是

```text
GET /article-types/6/ HTTP/1.1
Host: www.zhangdongshengtech.com
Connection: close
```

请求的消息头，每一行都有各自的作用，在消息头和消息体之间，有两次换行

由于我们发送的是==GET请求，没有消息体==，因此两个换行后就结束了。

第一行的内容，包含了三个重要信息：

- GET 指明本次请求所使用的method，这是一次GET请求
- /article-types/6/ 指明了==要请求的资源地址==
- HTTP/1.1 指明http协议的版本，更早以前是1.0，现在大家都在用1.1

第二行的内容，指明了host

​	一台服务器上，也许不只是部署了一个web服务，而是多个，他们都是80端口，url = '[www.zhangdongshengtech.com](http://www.zhangdongshengtech.com/)' 只是告诉socket去哪里建立连接，这仅仅是个域名而已。程序根据域名找到IP地址，如果服务端部署多个服务，为了在服务端区分一个请求是指向哪个服务，==客户端需要在请求头中指明host，服务端会根据这个host来做请求的转发，这就是常说的nginx反向代理==。

第三行，定义了Connection的值是close

​	如果不定义，默认是keep-alive， 如果是keep-alive，那么服务端在返回数据后不会断开连接，而是允许客户端继续使用这个连接发送请求，我故意设置成close，目的就是让服务端主动断开。这样，当程序在使用while循环时，接收完所有的数据后，sock.recv(1024) 返回的就是None，这样，就可以停止程序了。

如果是keep-alive，无法通过连接断开来判断数据是否已经全部接收，那么就==只能通过返回数据的消息头来获取数据的长度，进而决定本次请求返回的数据到哪里结束==。



### 返回的消息体

程序输出了服务端返回的数据，由于数据量很大，我们只截取消息头Header的部分进行讲解，消息体Body只是网页源码而已，没什么可说的。

```text
HTTP/1.1 200 OK
Server: openresty/1.11.2.1
Date: Sun, 05 May 2019 03:11:05 GMT
Content-Type: text/html; charset=utf-8
Content-Length: 29492
Connection: close
Set-Cookie: session=eyJjc3JmX3Rva2VuIjp7IiBiIjoiTn
prd1pqZGhaamd6T1dObFlUQTRZVFJqTkRJeU9USmtNalU0TldOaU1UQXdNamsxTkdSaVpRPT0ifX0.D6_lyQ.
4EqkK8taszUkPtMsol-8pzF_LQM; HttpOnly; Path=/

<!DOCTYPE html>
<html lang="en">
<head>
```

返回的数据中，消息头和消息体之间，也是两个换行。

请求的消息体和服务端响应的消息体中，除了第一行外，其他的长的很像字典形式的key-value对，叫首部

第一行：

- HTTP/1.1 200 OK 指明了http协议的版本，已经本次请求返回的状态码，200表示成功响应，比较常见的还有404，500，302

首部：

- Content-Length的值是29492，这表明，消息体的长度是29492
- 如果Connection的值是keep-alive，客户端就得根据这个值来读取消息体。





### 从消息头解析出content_length

在获取返回数据时，当消息体的长度满足要求时，停止获取数据，并关闭连接

```python
import socket


url = 'www.zhangdongshengtech.com'
port = 80
# 创建TCP socket
sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
# 连接服务端
sock.connect((url, port))
# 创建请求消息头
request_url = 'GET /article-types/6/ HTTP/1.1\r\nHost: www.zhangdongshengtech.com\r\n\r\n'
print(request_url)
# 发送请求
sock.send(request_url.encode())
body = ''
# 接收返回的数据
rec = sock.recv(1024)


index = rec.find(b'\r\n\r\n')       # 找到消息头与消息体分割的地方
head = rec[:index]
body = rec[index+4:]

# 获取Content-Length
headers = head.split(b'\r\n')
for header in headers:
    # 在首部查找指定的字符串
    if header.startswith(b'Content-Length'):
        content_length = int(header.split(b' ')[1])	#根据空格分隔字符串

length = len(body)

while length < content_length:
    rec = sock.recv(1024)
    length += len(rec)
    body += rec

sock.close()
print(length)
print(head.decode())
print(body.decode())
```





# 示例|最简单的web服务器

http协议是基于TCP实现的， 我们所熟知的nginx， tomcat, apache等服务器，本质上就是一个tcp server

只要你对http协议有少许的了解，就可以自己实现一个简单的web服务器，当然，它只能是一个你用来验证http协议和提升自己编程能力的小玩意。

## TCP server

```python
import socket

# 创建socket
server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
# 绑定ip 和端口
server.bind(('0.0.0.0', 8080))
# 开始监听
server.listen(1)

html = """
<html>
<head>
<title>simple server</title>
</head>
<body>
<p>hello world</p>
</body>
</html>
"""
length = len(html.encode())

# 这里也可以不设置Content-Length， 因为Connection 是close
# 具体缘由可以参考文章《来一波原生的http请求》
head = "HTTP/1.1 200 OK\r\nServer: simple server\r\n" \
       "Content-Type: text/html; charset=utf-8\r\n" \
       "Content-Length: {length}\r\nConnection: close\r\n\r\n"

head = head.format(length=length)

while True:
    # 等待客户端连接
    clientsocket, address = server.accept()
    # 接收客户端的数据
    data = clientsocket.recv(1024).decode()
    msg = head + html	#数据：header + body
    # 向客户端发送数据
    clientsocket.send(msg.encode())
    # 关闭连接
    clientsocket.close()

server.close()
```

#### 消息头

headrt的定义完全遵守http协议：

- Content-Length 是可以不用设置的，因为Connection被设置成了close，如果是keep-alive，那么就必须设置Content-Length

#### 消息体

- 消息体就是一段html 



## 浏览器访问 + 修改server

启动程序后，就可以在浏览器里输入url http://localhost:8080/ 得到的页面内容很简单，只有一个hello world

此时，只要url前面是http://localhost:8080/ ，后面不管跟什么path，得到的结果都是相同的，这是因为我们的server太简单了，没有根据path返回对应的内容



为了让你更进一步的了解http请求时都发生了什么，接下来，我要修改这个server

```python
import socket

# 创建socket
server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
# 绑定ip 和端口
server.bind(('0.0.0.0', 8080))
# 开始监听
server.listen(1)


def get_path(data):
    index = data.find("\r\n")
    if index == -1:
        return ""
    first_line = data[:index]
    arrs = first_line.split()
    if len(arrs) != 3:
        return ""
    path = arrs[1]
    return path


def get_html_by_path(path):
    if path == '/':
        return get_index()
    elif path == "/name":
        return get_name()
    else:
        return get_404()

html_string = """
    <html>
    <head>
    <title>simple server</title>
    </head>
    <body>
    <p>{content}</p>
    </body>
    </html>
"""

head_string = "HTTP/1.1 {status}\r\nServer: simple server\r\n" \
           "Content-Type: text/html; charset=utf-8\r\n" \
           "Content-Length: {length}\r\nConnection: close\r\n\r\n"

def get_html(status, content):
    html = html_string.format(content=content)
    length = len(html.encode())
    head = head_string.format(status=status, length=length)
    return head + html

def get_404():
    return get_html('404 NOT FOUND', "你访问的资源不存在")

def get_name():
    return get_html("200 OK", "my name is sheng")

def get_index():
    return get_html("200 OK", "hello world")

while True:
    # 等待客户端连接
    clientsocket, address = server.accept()
    # 接收客户端的数据
    data = clientsocket.recv(1024).decode()
    path = get_path(data)

    msg = get_html_by_path(path)
    # 向客户端发送数据
    clientsocket.send(msg.encode())
    # 关闭连接
    clientsocket.close()

server.close()
```

升级后的server，可以对http://localhost:8080/name 和 http://localhost:8080/ 做出正确的响应，访问其他的资源则会返回404





这里实现的服务器，是最最简单的服务器，但它和那些普遍使用的服务器，诸如nginx,tomcat在原理上是一致的，不同的是他们可以实现多进程，多线程，使用的epoll模型。

你用flask写一个web程序，最终部署的时候：

- 用的可能是nginx，中间件用uwsgi，结构更加复杂，但本质上就是TCP server



# select模型

在TCP服务端使用多线程技术能够提高响应的能力，但这种提高是有限的，因为你不可能无限制的创建多线程，更何况python的度线程还受到GIL锁的限制。

想要更稳定的提高服务端性能，可以使用select模型。



==select模型是多路复用模型的一种==，windows和linux下都可以使用

还有更厉害的epoll模型，下一篇将会介绍。

select模型允许进程指示内核等待多个事件的任何一个发生，并在只有一个或者多个事件发生或者经历一段事件后select函数才返回。

select模型其实很好理解，我们给它三个数组，数组里存放的是socket文件，每一次执行select，模型会从这三个数组中==分别挑出来可读的，可写的，发生异常的socket，并分别放入到三个数组中==，这样，应用层遍历这三个数组，做相应的操作。



## 服务端示例

```python
import select
import socket


def start_server(port):
    HOST = '0.0.0.0'
    PORT = port

    server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    server.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    server.bind((HOST, PORT))   # 套接字绑定的IP与端口
    server.listen(10)           # 开始TCP监听

    inputs = [server]           # 存放需要被检测可读的socket
    outputs = []                # 存放需要被检测可写的socket

    while inputs:
        readable, writable, exceptional = select.select(inputs, outputs, inputs)
        # 可读
        for s in readable:
            if s is server:     # 可读的是server,说明有连接进入
                connection, client_address = s.accept()
                inputs.append(connection)
            else:
                data = s.recv(1024)        # 故意设置的这么小
                if data:
                    # 已经从这个socket当中收到数据, 如果你想写, 那么就将其加入到outputs中, 等到select模型检查它是否可写
                    print(data.decode(encoding='utf-8'))
                    if s not in outputs:
                        outputs.append(s)
                else:
                    # 收到为空的数据,意味着对方已经断开连接,需要做清理工作
                    if s in outputs:
                        outputs.remove(s)
                    inputs.remove(s)
                    s.close()

        # 可写
        for w in writable:
            w.send('收到数据'.encode(encoding='utf-8'))
            outputs.remove(w)

        # 异常
        for s in exceptional:
            inputs.remove(s)
            if s in outputs:
                outputs.remove(s)
            s.close()

if __name__ == '__main__':
    start_server(8801)
```

### 1 为什么要将server放入到inputs中

在select模型中，将server放入到inputs中

当执行select时就会去检查server是否可读，就说明在缓冲区里有数据，对于server来说，有连接进入。

使用accept获得客户端socket文件后，首先要放入到inputs当中，等待其发送消息。



### readable

select会将所有可读的socket返回，包括server在内，假设一个客户端socket的缓冲区里有2000字节的内容，而这一次你只是读取了1024个字节，没有关系，下一次执行select模型时，由于缓冲区里还有数据，这个客户端socket还会被放入到readable列表中。

因此，在读取数据时，不必再像之前那样使用一个while循环一直读取。





### writable

在每一次写操作执行后，都从socket从writable中删除，这样做的原因很简单，该写的数据已经写完了，如果不删除，下一次select操作时，又会把他放入到writable中，可是现在已经没有数据需要写了啊，这样做没有意义，只会浪费select操作的时间，因为它要遍历outputs中的每一个socket，判断他们是否可写以决定是否将其放入到writtable中





### 异常

在exceptional中，是发生错误和异常的socket，有了这个数组，就在也不用操心错误和异常了，不然程序写起来非常的复杂，有了统一的管理，发生错误后的清理工作将变得非常简单



## 客户端

没有变化

启动服务端后，你可以启动多个客户端来检查效果

```python
import os
import time
import socket

def start_client(addr, port):
    PLC_ADDR = addr
    PLC_PORT = port
    s = socket.socket()
    s.connect((PLC_ADDR, PLC_PORT))
    count = 0
    while True:
        msg = '进程{pid}发送数据'.format(pid=os.getpid())
        msg = msg.encode(encoding='utf-8')
        s.send(msg)
        recv_data = s.recv(1024)
        print(recv_data.decode(encoding='utf-8'))
        time.sleep(3)
        count += 1
        if count > 20:
            break

    s.close()

if __name__ == '__main__':
    start_client('127.0.0.1', 8801)
```





# epoll模型

select模型虽好，却有一个缺陷，只能对1024个文件描述符进行监视，虽然可以通过重新编译内核获得更大的监视数量，但这样做还不如将目光投向更高级的epoll模型。

select模型中，每一次都需要遍历所有处于监视中的文件描述符，判断他们哪个可写，哪个可读，这样一来，你监视的越多，速度越慢，而在epoll模型中，==所有添加到epoll中的事件都会网卡驱动程序建立起回调关系==，简言之，如果有一个连接可写，那么这个可写的事件就会报告给你，而你不需要挨个询问他们哪个连接可写，哪个连接可读， ==tornado框架就使用了epoll模型==。

强调一点，本文所用代码只能在linux环境下执行，因为==只有linux系统支持epoll模型==。



## 服务端

```python
import socket
import select


def start_server(port):
    serversocket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    serversocket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    serversocket.bind(('0.0.0.0', port))
    #accept队列大小为100
    serversocket.listen(100)
    serversocket.setblocking(0)

    epoll = select.epoll()
    #注册一个in事件，等待有数据可读
    epoll.register(serversocket.fileno(), select.EPOLLIN)

    try:
        #保存连接，请求，和响应信息
        connections = {}
        while True:
            #最多等待1秒钟时间，有事件返回事件列表
            events = epoll.poll(1)
            for fileno, event in events:
                #事件的句柄是server
                if fileno == serversocket.fileno():
                    connection, address = serversocket.accept()
                    #设置为非阻塞的
                    connection.setblocking(0)
                    #新建的连接也注册读事件
                    epoll.register(connection.fileno(), select.EPOLLIN)
                    connections[connection.fileno()] = connection
                    #不是server，那就是建立的连接，现在连接可读
                elif event & select.EPOLLIN:
                    data = connections[fileno].recv(1024)
                    if data:
                        epoll.modify(fileno, select.EPOLLOUT)
                    else:
                        epoll.modify(fileno, 0)
                        connections[fileno].shutdown(socket.SHUT_RDWR)
                        del connections[fileno]

                elif event & select.EPOLLOUT:
                    #可写的事件被触发
                    clientsocket = connections[fileno]
                    clientsocket.send('收到数据'.encode(encoding='utf-8'))

                    #需要回写的数据已经写完了,再次注册读事件
                    epoll.modify(fileno, select.EPOLLIN)
                elif event & select.EPOLLHUP:
                    #被挂起了，注销句柄，关闭连接，这时候，是客户端主动断开了连接
                    epoll.unregister(fileno)
                    if fileno in connections:
                        connections[fileno].close()
                        del connections[fileno]
    finally:
        epoll.unregister(serversocket.fileno())
        epoll.close()
        serversocket.close()


if __name__ == '__main__':
    start_server(8801)
```

本文所采用的是epoll模型的边缘触发，除此以外，还有一个水平触发。



### events

epoll.poll()返回的是所有可操作的文件描述符和事件类型，具体是哪个事件，需要你自己使用if语句逐个进行判断，此外，还需要你自己来保存文件描述符与socket文件之间的映射关系。





### 注册

获得客户端连接后，需要将这个socket注册读事件

这样，当这个socket发送数据后，下一次调用epoll.poll()就会获得该socket。



## 客户端

客户端示例代码没有变化

启动服务端后，同时启动多个客户端，观察实验效果。

```python
import os
import time
import socket

def start_client(addr, port):
    PLC_ADDR = addr
    PLC_PORT = port
    s = socket.socket()
    s.connect((PLC_ADDR, PLC_PORT))
    count = 0
    while True:
        msg = '进程{pid}发送数据'.format(pid=os.getpid())
        msg = msg.encode(encoding='utf-8')
        s.send(msg)
        recv_data = s.recv(1024)
        print(recv_data.decode(encoding='utf-8'))
        time.sleep(3)
        count += 1
        if count > 20:
            break

    s.close()

if __name__ == '__main__':
    start_client('127.0.0.1', 8801)
```





# unix socket

有一个与docker相关的文件，目录为/var/run/docker.sock， docker通过它与其他进程通信，提供了可以操作docker的API接口

这种技术，就是unix Domain Socket， 又称 unix域套接口 ， 用于 同一台机器（操作系统）的进程间的通信。

从编程实现上看，它与TCP/IP的socket非常接近，近乎相同

我非常好奇他们之间的区别之处，google了一番，得到下面还算令人满意的答案

```text
Unix套接字：是机器上运行的服务器之间的内部通信过程

IP套接字：更多是外部的，意味着网络上的进程之间进行通信。即使您也可以在内部使用此类型，它通常位于本地主机和远程主机之间。

对于Unix套接字，最好使用它们，因为可以完全避免诸如路由之类的某些操作，因为域套接字知道它们在同一台计算机上执行，这使它们更快，因此如果在同一主机上进行通信，则使其成为更好的选择。


您可以使用以下命令检出计算机的本地unix套接字：
netstat -a -p --unix
```



## unix socket 服务端

TCP/IP 在创建服务端socket时，需要指定ip和端口号，unix socket 用于同一台机器之间的通信，只需要一个本地文件就可以了，但这个文件在创建socket之前不能存在，否则会报错，因此，需要事先删除。

在创建socket套接字时，要使用socket.AF_UNIX，其余的代码，与创建TCP/IP socket几乎相同

```python
import os
import socket

server_address = './uds_socket'
 
# 必须先删除
try:
    os.unlink(server_address)
except OSError:
    if os.path.exists(server_address):
        raise

# 指定协议
server = socket.socket(socket.AF_UNIX, socket.SOCK_STREAM)
server.bind(server_address)
# 监听
server.listen(1)
clientsocket, address = server.accept()
# 接收消息
data = clientsocket.recv(1024)
print(data)
# 关闭socket
clientsocket.close()
server.close()
```





## unix socket 客户端

```python
import os
import socket

server_address = './uds_socket'
sock = socket.socket(socket.AF_UNIX, socket.SOCK_STREAM)
try:
    sock.connect(server_address)
except socket.error as msg:
    print(msg)
    sys.exit(1)


sock.send(b'hello world')
sock.close()
```