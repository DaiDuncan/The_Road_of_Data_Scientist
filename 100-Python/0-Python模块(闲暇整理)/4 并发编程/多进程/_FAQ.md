## 给进程命名

可以给进程起一个名字，这样在debug的时候能够提供一些帮助

```python
import multiprocessing
from multiprocessing import Process


def func():
    print(multiprocessing.current_process().name)   # 当前进程的名字


p1 = Process(name='test', target=func)
p1.start()

p2 = Process(target=func)
p2.start()
```

程序输出结果：

```text
test
Process-2
```

如果程序没有主动为进程起一个名字，那么会默认为进程分配一个名字。





## 杀掉一个进程

杀掉进程可以使用terminate方法

假设有这样的需求场景，进程启动后，我们希望5秒钟后不论程序执行结果如何都将其杀死，那么就可以这样来设计：

- 使用Process创建一个子进程p，==随后启动一个线程来检查时间和进程p的状态==
- 5秒钟过后，在线程中调用进程的terminate方法将其杀死



示例代码如下

```python
import multiprocessing
import threading
import time

def monitor(p):
    t1 = time.time()
    while True:
        t2 = time.time()
        if t2 - t1 > 5:
            if p.is_alive():
                p.terminate()
            break

        time.sleep(1)


def func():
    while True:
        print('子进程在运行')
        time.sleep(1)


p = multiprocessing.Process(name='测试', target=func)
p.start()

t = threading.Thread(target=monitor, args=(p, ))
t.start()

p.join()
print('状态码:', p.exitcode)
```

1. is_alive可以检查进程是否存活
2. exitcode 是进程退出的状态码：
   - 如果是0表示正常退出
   - 大于0表示进程有错误
   - 小于0表示被对应的信号杀死



## 子进程在后台运行

明确一点，后台进程不是守护进程或者服务，如果需要处理的任务比较大同时又不需要人工干预，那么可以将其设置为后台进程

```python
import multiprocessing
import time


def func():
    t1 = time.time()
    while True:
        t2 = time.time()
        if t2 - t1 > 10:
            break
        print('子进程')
        time.sleep(1)

p = multiprocessing.Process(target=func)
p.daemon = True
p.start()

p.join()
```

只需要将daemon设置为True，进程就变为后台进程，与非后台进程相比有以下几点不同:

1. 后台进程不会在终端输出任何信息，非后端进程则会输出print里的内容
2. ==后台进程不允许再启动子进程==，因为后台进程会在主进程结束退出，非后台进程则没有这样的限制





## 使用队列交换对象

进程间通信可以使用队列，Queue返回一个进程共享的队列，它既是线程安全的也是进程安全的。

任何可以序列化的对象都可以通过它来传输。

在下面的示例中，主进程中有一个列表lst,里面存储了10个任务，启动两个子进程，主进程与子进程通过队列来交换数据

```python
import multiprocessing
import time

def func(q):
    while True:
        if q.empty():
            break
        else:
            task = q.get()
            name = multiprocessing.current_process().name
            print("进程{name}处理任务{task}".format(name=name, task=task))
            time.sleep(1)


if __name__ == '__main__':
    lst = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
    q = multiprocessing.Queue()	#进程队列
    for item in lst:
        q.put(item)

    p_lst = [multiprocessing.Process(target=func, args=(q, )) for i in range(2)]	#两个子进程
    for p in p_lst:
        p.start()

    for p in p_lst:
        p.join()
```

输出结果：

```text
进程Process-1处理任务1
进程Process-2处理任务2
进程Process-1处理任务3
进程Process-2处理任务4
进程Process-1处理任务5
进程Process-2处理任务6
进程Process-1处理任务7
进程Process-2处理任务8
进程Process-1处理任务9
进程Process-2处理任务10
```





# #参考文献

[Link: 酷python](http://www.coolpython.net/python_senior/concurrent/talk_about_process_by_python.html)