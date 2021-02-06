不论是==web后端开发，还是爬虫开发==，都需要对http相关的知识有一定的了解和掌握。

- 浏览器发出一次http请求，直至收到响应后显示内容，这个中间过程究竟都经历了什么？
- 如何用socket直接发送http请求，服务端响应的本质有是什么。
- 一段url里的中文，为什么都是乱码？怎么样使用python下载图片？





# 浏览器是如何工作的

浏览器是一个软件，它是如何工作的？

## 普通人眼里的浏览器

浏览器就是用来上网的，它的工作模式如下图所示

![image-20210201163917674](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210201163917.png)

## 初级程序员眼里的浏览器

浏览器器发送请求，服务器返回请求，浏览器处理请求这三个动作上

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210201163938.png" alt="image-20210201163938071" style="zoom:80%;" />

## 高级程序员眼里的浏览器

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210201164012.png" alt="image-20210201164012508" style="zoom:80%;" />

在高级程序员眼里，对浏览器行为了解的更为透彻，对技术的关注更倾向于底层实现





# 建立TCP连接

## TCP 服务端与客户端

web服务的本质，是一个TCP server

浏览器则是一个TCP client

只有先建立连接，浏览器才能通过这个连接发送http请求



### 示例|TCP服务端

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

### 客户端

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
# 收到的数据都是bytes类型
print(revcdata.decode(encoding='utf-8'))
time.sleep(1)
client.close()
```



## TCP3次握手

客户端与服务端在建立是在连接之前，要互相试探着询问对方是否愿意与自己建立连接

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210201183010.png" alt="image-20210201183010241" style="zoom:80%;" />



想要从技术层面上做出解释，则必须先了解TCP头结构

### TCP头结构

客户端与服务端发送数据时，必须遵守TCP/IP协议，按照指定的格式发送数据，TCP的头结构携带了许多关键信息，比如源端口号与目的端口号

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210201135402.png" alt="image-20210201135401916" style="zoom: 67%;" />

上图中，中间白色的那一块区域，找到SYN和ACK，这两个各站1个bit位，是3次握手的关键：

1. 客户端向服务端发送syn包，所谓的syn包，是指SYN为1
2. 服务端收到syn包以后，发送syn+ack包，这时，SYN和ACK都是1
3. 客户端向服务端发送ACK包，ACK标识位是1

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210201183204.png" alt="image-20210201183204820" style="zoom:67%;" />



### 为什么是3次握手，而不是2次？

看上去，似乎两次握手就可以了，服务端在收到SYN包后回复ACK确认可以建立连接不就行了么，为何非要让服务端也发SYN，客户端再回ACK呢？

之所要求3次握手，是==为了防止已经失效的SYN包意外地到达服务端==(第一次握手)，造成双方的不一致。



考虑这种情况，客户端向服务端发出了SYN包S1， 可好巧不巧的是，S1由于网络阻塞或其他原因在网络中滞留，毕竟从客户端到服务器隔着十万八千里呢，中间是好多个交换机路由器等网络设备，网络也会堵车。

发生网络堵车后，客户端以为S1丢包了，于是又发出一个SYN包S2，S2顺利的到达服务端

如果握手只需要两次，那么服务端回一个ACK就建立好连接了。



可别忘了网络中还有一个S1,S1并不知道客户端又发出了S2，它恪尽职守的穿越重重障碍，在S2之后到达服务端，如果这个时候S2所建立的连接还存在，那么服务端会无视S1。

但如果S2所建立的连接已经断开了呢？服务端会以以为S1是一个新的连接请求，服务端回ACK，建立连接，客户端收到ACK后，就有点懵逼了，它完全不知道是怎么回事，也不知道该如何处理这个突然到达的ACK包，这样一来，==两边都会浪费很多资源==。



2次握手建立连接，会发生上述的意外，但是3次握手就不会

服务端与客户端都要发送SYN包并==回复对方ACK包==，只有这样才会建立连接，服务端在回复ACK的时候，顺带着把SYN设置为1，这样就免去了单独发送SYN包的步骤，3次握手即可建立连接。





# DNS解析

诡异的事情：电脑无法上网，但是QQ是可以登录的。这种情况，是由于DNS解析异常导致的。



浏览器在底层本质上是一个socket客户端，当你想要在浏览器里打开百度首页时，你输入网址www.baidu.com并点击回车后，==浏览器需要与百度服务器建立TCP连接==，这就需要知道百度服务器的IP 和 端口号， 端口号默认是80， 但是IP是多少呢， 所谓DNS解析，就是根据网站域名找到IP。



## 域名与IP

一个网站，可能会有多个域名，多个IP，在终端里执行下面的命令

```shell
ping coolpython.net

# 结果
PING coolpython.net (101.201.37.248): 56 data bytes
64 bytes from 101.201.37.248: icmp_seq=0 ttl=51 time=11.260 ms
64 bytes from 101.201.37.248: icmp_seq=1 ttl=51 time=11.138 ms
64 bytes from 101.201.37.248: icmp_seq=2 ttl=51 time=11.209 ms
```

在浏览器里直接输入ip，就打开了我的个人技术博客，我这只是一个小网站，只用一个域名就可以了，不需通过DNS做负载均衡，因此只需要一个ip就可以了。

在网站域名coolpython.net 和 服务器IP 101.201.37.248之间存在一个映射关系，这个关系保存在DNS服务器中。我们人类的记忆更适合记录那些==看起来有点意义的东西==，记录一个域名要要比记录一个IP地址要容易的多。





## DNS解析过程

1. 本地：hosts文件(系统缓存：常用的域名)
2. 本地：DNS服务器(网络服务提供商：连通 电信)
3. 根域名服务器：
4. 顶级域名服务器
5. 如果找不到，可能是由于这个域名有问题了

当我们在浏览器里输入一个网站域名后，会先在本地查找这个域名所对应的IP，在hosts文件中，如果有这个域名的记录，那么就完成查找，这部分是==系统缓存的数据==。

如果在hosts文件中找不到，则向本地DNS服务器发起请求，请求解析这个域名的ip，所谓本地DNS服务器是指你的网络服务提供商，联通，电信。如果他们可以找到映射关系就会把对应的IP返回给你。

如果本地DNS服务器找不到，则向根域名服务器继续发请求，根域名服务器还是找不到，就继续向上，向顶级域名服务器发送请求，直至找到为止。

那么会不会出现最终也找不到的情况呢？ 如果找不到，可能是由于这个域名有问题了，服务商已经停止了对它的解析。



平时所遇到的无法上网但是可以登录QQ的情况是由于你电脑里DNS服务器所配置的IP出问题了

- 百度一个==可用的DNS服务器IP==替换一下就好

QQ走的UDP，不是TCP，因此不受DNS解析的影响。





# #参考文献

《http权威指南》