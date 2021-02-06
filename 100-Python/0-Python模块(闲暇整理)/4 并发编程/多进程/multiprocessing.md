multiprocessing与threading有着相似的API，是一个用于产生进程的包。使用这个模块，可以让你轻松的创建出多进程程序。



# Process类

创建一个Process类的实例，然后调用它的start方法就可以创建出一个子进程

```python
import time
import os
from multiprocessing import Process

def f(seconds):
    pid = os.getpid()		#获取当前进程的pid
    ppid = os.getppid()     #获取当前进程的父进程pid
    print("父进程pid: {ppid}, 子进程pid: {pid}".format(ppid=ppid, pid=pid))
    time.sleep(seconds)

if __name__ == '__main__':
    pid = os.getpid()
    print("主进程pid: {pid}".format(pid=pid))
    p = Process(target=f, args=(3,))
    p.start()
    p.join()
```

输出结果：

```text
主进程pid: 19544
父进程pid: 19544, 子进程pid: 19545
```

一旦提到多进程，必然是一个主进程（父进程）产生出至少一个子进程，至于主进程如何产生出子进程，在不同的系统下有着不同的实现



在创建Process实例时，需要指明子进程需要执行的函数以及需要传入的参数,args的类型是tuple，即便只有一个参数，也要在后面写一个逗号。

==只有执行了start方法后，才正式的创建出子进程==。



## python

python官方文档对于多进程启动方式的介绍：

### spawn

父进程启动一个新的Python解释器进程。

子进程只会继承那些运行进程对象的 run() 方法所需的资源。特别是父进程中非必须的文件描述符和句柄不会被继承。

相对于使用 fork 或者 forkserver，使用这个方法启动进程相当慢。



可在Unix和Windows上使用。 Windows上的默认设置。



### fork

父进程使用 os.fork() 来产生 Python 解释器分叉。子进程在开始时实际上与父进程相同。

父进程的所有资源都由子进程继承。

请注意，安全分叉多线程进程是棘手的。



只存在于Unix。Unix中的默认值。



### forkserver

程序启动并选择forkserver启动方法时，将启动服务器进程。

从那时起，每当需要一个新进程时，父进程就会连接到服务器并请求它分叉一个新进程。分叉服务器进程是单线程的，因此使用 os.fork() 是安全的。没有不必要的资源被继承。



可在Unix平台上使用，支持通过Unix管道传递文件描述符







# 继承 Process类

上述创建子进程时使用的是函数，除了这种方法外，还可以使用类，创建一个类并继承Process，==这个类必须实现run方法==。

```python
import time
import os
from multiprocessing import Process

class MyProcess(Process):
    def __init__(self, seconds):
        super(MyProcess,self).__init__()
        self.seconds = seconds

    def run(self):
        pid = os.getpid()
        ppid = os.getppid()         # 父进程pid
        print("父进程pid: {ppid}, 子进程pid: {pid}".format(ppid=ppid, pid=pid))
        time.sleep(self.seconds)

if __name__ == '__main__':
    pid = os.getpid()
    print("主进程pid: {pid}".format(pid=pid))
    p = MyProcess(3)
    p.start()
    p.join()
```







# 进程池 Pool(n)

进程池内部维护若干个进程，当需要时可以去进程池中获取一个进程，如果进程池中没有可用的进程，程序进入等待状态直到有可用的进程为止。

进程池有两种方法：

- apply_async是异步的
- apply是同步的

一般我们使用异步方法。



已知列表中有10个整数 `lst = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]`

现在要求你计算他们的平方，为了加快计算速度，我决定使用多进程，这样操作仅仅是为了向你展示如何使用进程池进行编程。

```python
from multiprocessing import Pool


def func(num):
    return num**2


lst = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
pool = Pool(4)   # 创建4个进程
results = []

for item in lst:
    results.append(pool.apply_async(func, (item, )))	#异步进程

pool.close()    # 关闭进程池，表示不能再往进程池中添加进程，需要在join之前调用
pool.join()     # 等待进程池中的所有进程执行完毕

print(type(results[0]))
for res in results:
    print(res.get())
```

程序输出结果：

```text
<class 'multiprocessing.pool.ApplyResult'>
1
4
9
16
25
36
49
64
81
100
```

很多人误以为列表results里存储的数据类型是int，但==其实是ApplyResult==，必须通过get方法才能获得真正的结果。

apply（单个任务同步）与apply_async之间的区别我们不必去深究，因为python官方建议废弃apply而使用apply_async，所以，我们只需要了解apply_async就可以了。

在上面的示例中，我启动了拥有4个进程的进程池，当我们==启动子进程进行计算时，主进程并没有被阻塞，也就是说主进程也在执行==

因此我在代码里加了pool.join()这行代码，它的作用是==等待进程池里所有的进程执行结束==，然后再继续执行主进程里的代码，如果没有这行代码，进程池里的进程还没有运行结束呢，可能主进程就已经结束了。





# map 与 map_async

- map是多个任务同步

- map_async是多个任务异步

使用起来差别不大，与apply_async相比，只是改变了编程的方式，其本质是一样的，都是利用多进程并行运算



先来看map版本的并行运算

```python
from multiprocessing import Pool


def func(num):
    return num**2


lst = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
pool = Pool(4)   # 创建4个进程

results = pool.map(func, lst)

pool.close()    # 关闭进程池，表示不能再往进程池中添加进程，需要在join之前调用
pool.join()     # 等待进程池中的所有进程执行完毕

print(results)	#[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
```



再来看map_async版本

```python
from multiprocessing import Pool


def func(num):
    return num**2


lst = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
pool = Pool(4)   # 创建4个进程

results = pool.map_async(func, lst)

pool.close()    # 关闭进程池，表示不能再往进程池中添加进程，需要在join之前调用
pool.join()     # 等待进程池中的所有进程执行完毕

print(results.get())
```



me|小结：套用了map函数的外壳 => 基于多线程，对lst使用func