## 简介|粘包与分包

一个以太网包只能传输1500字节长度的数据，而这其中，IP头和TCP头各占去了20个字节，因此，有效载荷为1460，如果你要发的一段数据的长度超过了1460，假设为2000，那么必然被分成多个以太网包发送过来，对于接收方来说，如果每次接受1024个字节，则需要多次recv才能把整段数据接收，而TCP是不维护数据边界的

==因此对于接收方来说，完全不知道这一段数据什么时候结束==。



粘包则和分包相反，你要发送的数据长度很短，比如只有30个字节左右，如果你以非常快的速度发送，那么有可能一个以太网包里包含了好几段数据，他们是被一起发送过来的，这时接收方recv得到的数据是好几段数据连在一起，无法分开



# 简单的解决方案

如何解决呢？

一种方法就是约定好命令的长度，这样一来，接收方就可以根据提前约定好的数据长度来解析数据了。

- 但这样会产生许多不必要的麻烦了，比如实际发送数据小于约定长度时需要填充，这样也造成了传输上的浪费。



另一种方法就是对要传输的数据进行封装，比如在数据的最前面加上一个长度标识，指明本次要发送的数据长度是多少，这样一来，接收方先获得数据的长度，然后根据数据的长度来获取实际数据。

在数据的前面加一个长度为5的头用来标识数据的长度，假设要发送数据的长度是50，则头就是“00050”，后接实际数据。

- 接收方在获得数据后，先解析前5位，获得数据实际长度后再继续解析后面的数据





# MsgContainer

实现一个MsgContainer类：

- 客户端使用pack_msg方法封装数据
- 服务端使用add_data方法将接收到的数据放到自己维护的缓冲区中，根据协议剥离出客户端实际发送的数据

```python
zero_count = 5

class MsgContainer(object):

    def __init__(self):
        self.msg = []
        self.msgpond = b''
        self.msg_len = 0

    def __add_zero(self, str_len):
        head = (zero_count - len(str_len))*'0' + str_len
        return head.encode(encoding='utf-8')

    def pack_msg(self, data):
        """
        封装数据
        :param data:
        :return:
        """
        bdata = data.encode(encoding='utf-8')
        str_len = str(len(bdata))
        return self.__add_zero(str_len) + bdata

    def __get_msg_len(self):
        self.msg_len = int(self.msgpond[:5])

    def add_data(self, data):
        if len(data) == 0 or data is None:
            return
        self.msgpond += data
        self.__check_head()

    def __check_head(self):
        if len(self.msgpond) > 5:
            self.__get_msg_len()
            self.__get_msg()

    def __get_msg(self):
        if len(self.msgpond)-5 >= self.msg_len:
            msg = self.msgpond[5:5+self.msg_len]
            self.msgpond = self.msgpond[5+self.msg_len:]
            self.msg_len = 0
            msg = msg.decode(encoding='utf-8')
            self.msg.append(msg)
            self.__check_head()

    def get_all_msg(self):
        return self.msg

    def clear_msg(self):
        self.msg = []
```



## 1 封装数据

```python
mc = MsgContainer()
data = mc.pack_msg('123')
print(data)   # 00003123
```

这是一段封装数据的示例，123被封装成b'00003123'， 前5位表示数据长度，'00003' 表示数据长度为3，从第6位开始的到第8位是实际数据



## 2 解析数据

```python
mc = MsgContainer()
mc.add_data(b'00006\xe4\xb8\xad\xe5\x9b\xbd')
mc.add_data(b'00015\xe9\x94\x84\xe7\xa6\xbe\xe6\x97\xa5\xe5\xbd\x93\xe5\x8d\x88')

lst = mc.get_all_msg()
for item in lst:
    print(item)
    
    
### 输出结果
中国
锄禾日当午
```





# 在服务端使用

```python
def start_server(port):
    HOST = '0.0.0.0'
    PORT = port
    s = socket.socket(socket.AF_INET,socket.SOCK_STREAM)   #定义socket类型，网络通信，TCP
    s.bind((HOST, PORT))   #套接字绑定的IP与端口
    s.listen(1)           #开始TCP监听

    mc = MsgContainer()

    while True:
        conn, addr = s.accept()     #接受TCP连接，并返回新的套接字与IP地址
        print('Connected by',addr)    #输出客户端的IP地址
        while True:
            data = conn.recv(2)       # 把接收的数据实例化
            # print(data)
            if len(data) == 0:
                break

            mc.add_data(data)          # 将数据写入缓冲区,每次写入都会尝试剥离实际传输的数据
            msgs = mc.get_all_msg()
            for msg in msgs:
                print(msg)

            mc.clear_msg()

        conn.close()     #关闭连接


if __name__ == '__main__':
    start_server(8801)
```

在服务端使用recv接收数据时，我故意每次调用函数都只接收两个字节的内容以此来验证我的方案是否可行，

启动TCP server, 等待客户端发送数据。 这个服务端使用while循环一直在监听连接请求，客户端的程序可以多次执行。





# 在客户端使用

客户端发送数据前，先使用pack_msg方法封装数据，

```python
import socket

mc = MsgContainer()

def start_client(addr, port):
    s = socket.socket()
    s.connect((addr, port))
    s.send(mc.pack_msg('解决分包问题'))
    s.send(mc.pack_msg('酷python'))
    s.close()

if __name__ == '__main__':
    start_client('127.0.0.1', 8801)
```

