目前所实现的TCP server服务能力都很差，根本无法同时与多个客户端进行交互，只有处理完一个客户端的交互以后才能使用accept等待下一个客户端的连接。

使用多线程技术，在通过accept获得一个socket文件后，==启动一个多线程==来专门处理这个客户端的数据传输。



# 服务端代码

```python
import socket
import threading


def worker(client):
    while True:
        data=client.recv(1024)    #把接收的数据实例化
        if len(data) == 0 or data == None:
            break
        print(data.decode(encoding='utf-8'))
        client.send('收到数据'.encode(encoding='utf-8'))


def start_server(port):
    HOST = '0.0.0.0'
    PORT = port
    s = socket.socket(socket.AF_INET,socket.SOCK_STREAM)   #定义socket类型，网络通信，TCP
    s.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    s.bind((HOST,PORT))   #套接字绑定的IP与端口
    s.listen(1)           #开始TCP监听
    while True:
        conn, addr = s.accept()   #接受TCP连接，并返回新的套接字与IP地址
        print('Connected by',addr)          #输出客户端的IP地址
        # 给子程序建立多线程：对应新的客户端连接
        t = threading.Thread(target=worker, args=(conn, ))	
        t.start()


if __name__ == '__main__':
    start_server(8801)
```

在服务端，我使用while实现了无限循环，在循环里使用accept接受TCP连接

获得连接后，这个连接何时将全部数据发送给服务端是未知的，如果一直处理这个连接，那么其他的连接就无法处理，因为在服务端，代码无法执行到下一次accept。

使用了多线程以后，情况就不同了，==每个TCP连接使用一个线程来处理==，那么主线程就可以再次执行到accept这行代码上，不受任何操作的阻塞影响。







# 客户端代码

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

客户端不需要做什么改变，在代码里，每次发送完消息后，都会sleep 3秒钟，一共发送20次

这样，就有足够的时间启动多个client来测试服务端是否有能力同时处理多个客户端的请求了。



