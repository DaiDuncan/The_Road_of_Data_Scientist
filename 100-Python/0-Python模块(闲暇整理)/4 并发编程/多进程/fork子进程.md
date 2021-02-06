fork通过系统调用能够创建出一个==与原来进程一模一样的进程==，子进程可以执行和父进程一样的代码，通过逻辑控制，也可以让父进程和子进程执行完全不同的代码块。

如果你只是会使用multiprocessing模块进行编程，那么并不能说明你真的理解多进程，因为你并不清楚：

- 多进程是如何创建的
- 创建出的子进程与父进程之间的关系是怎样的
- 也不会明白同一个变量在父进程和子进程都进行修改时会发生什么

知其然，更要知其所以然。



创建子进程的过程非常简单

```python
pid = os.fork()
```

返回值有三种

1. pid < 0 表示创建失败
2. pid = 0 此时，处在子进程中
3. pid > 0 此时，处在父进程中 pid 的值就是刚刚创建出来的子进程的pid



# #示例

```python
import os


def create_child():
    pid0 = os.getpid()
    print('主进程', pid0)

    try:
        pid1 = os.fork()	#尝试创建子进程
    except OSError:
        print('你的系统不支持fork')
        exit()

    if pid1 < 0:
        print('创建子进程失败')
    elif pid1 == 0:
        print('子进程 ', pid1, os.getpid(), os.getppid())
    else:
        print('主进程 ', pid1, os.getpid(), os.getppid())

    print('这句话,父进程和子进程都会执行')


if __name__ == '__main__':
    create_child()
```

如果fork函数执行成功，那么从if pid1 < 0 这行代码开始的代码，父进程和子进程都会执行，此时，他们已经是两个完全不相干的进程了。

程序输出结果是

```python
主进程  70522
主进程  70523 70522 366	#pid > 0 刚刚创建出来的子进程：pid 的值就是刚刚创建出来的子进程的pid 
这句话,父进程和子进程都会执行
子进程  0 70523 70522		#处在子进程中
这句话,父进程和子进程都会执行
```





# 父子内存关系: 写时复制

对于fork，有一点必须搞清楚，那就是原来父进程里的那些变量和子进程里的变量是什么关系。

一种普遍的存在误区的理解是子进程完全拷贝了==父进程的数据段、栈和堆上的内容==，但实际情况是linux引入了写时拷贝技术，子进程的页表项只想了与父进程相同的物理内存页，这样只拷贝父进程的页表项就可以了，这些页面被标记为只读，如果父子进程都不去修改内存内容，大家相安无事，一旦父子进程中的某一个尝试修改，就会引发缺页异常。

此时，内核会尝试为该页面==创建一个新的物理页面，并将内容真实的复制到物理页面中==，这样，父子页面就各自拥有了各自的物理内存,看下面这段代码：

```python
import os
import time


class TestFork():
    def __init__(self,age):
        self.age = age

tf = TestFork(10)

def child_work():
    print('我是子进程', os.getpid())
    tf.age = os.getpid()
    print(2, "age的值是{age}, tf对象的内存地址是{tf}\n".format(age=tf.age, tf=id(tf)))

def parent_work():
    print('我是主进程',os.getpid())
    print(1, "age的值是{age}, tf对象的内存地址是{tf}\n".format(age=tf.age, tf=id(tf)))


def fork_many_child(count):
    if count == 0:
        parent_work()
        return

    pid1 = os.fork()
    if pid1 == 0:
        child_work()
    else:
        fork_many_child(count-1)

if __name__ == '__main__':
    fork_many_child(3)
```



程序运行结果

```text
我是主进程 72544
主进程72544, age的值是10, tf对象的内存地址是4357220672

我是子进程 72545
子进程72545中, age的值是72545, tf对象的内存地址是4357220672

我是子进程 72546
子进程72546中, age的值是72546, tf对象的内存地址是4357220672

我是子进程 72547
子进程72547中, age的值是72547, tf对象的内存地址是4357220672
```

在父进程中，我创建了一个TestFork对象，子进程里去修改age的值，然后输出tf.age的值和tf对象的内存地址。

子进程各自对tf对象的age属性进行了修改，因此输出的age值是不同的

但是请注意，子进程中tf对象的内存地址和父进程里的是相同的，这是因为程序只是修改了age属性，引发age属性的写时复制，但==变量tf仍然指向之前的对象，因此不会引发写时复制==，tf的内存地址不会变化。





# 收尸

如果父进程还存在，而子进程退出了，那么子进程会变成一个僵尸进程，父进程必须为他收尸。

如果父进程先结束了，而子进程还没有结束，此时，子进程的父进程就变成了init进程，由它来负责为子进程退出后收尸。

收尸有两种方法：

- 一个是wait，wait是阻塞的
- 一个是os.waitpid，而os.waitpid可以设置为非阻塞的



重点讲解waitpid，waitpid函数定义为  def waitpid(pid, options)，第一个参数取值有以下几种情况：

1. pid > 0  等待进程ID为pid的子进程，此时是精确打击
2. pid = 0  等待与调用进程同一个进程组的任意子进程
3. pid = -1 等待任意子进程，此时和wait等价
4. pid < -1 等待进程组ID与pid 绝对值相等的所有子进程



options 是以下几个标志位的组合：

1. os.WNOHANG     如果子进程没有发生变化，则立刻返回，不会阻塞
2. os.WUNTRACED  除了关心终止进程的信息，也关心因信号而停止的子进程信息
3. os.WCONTINUED 除了关心终止进程的信息，也关心因受到信号而恢复执行的子进程信息



函数的返回值有两个，分别为pid 和 status：

1. pid = 0 表示子进程没有发生变化, status不需要理会
2. pid = -1 表示waitpid调用失败，此时要关心status的值
   - status 为 ECHLD，表示没有发现有子进程需要等待
   - status 为EINTR，表示函数被信号中断
3. pid >0 pid是发生变化的子进程的pid，具体子进程何种状态，因为什么退出，需要根据status来判断
   - 如果子进程是正常退出，status就是0；
   - 如果子进程是被信号杀死的，status记录的就是终止的信号；
   - 如果子进程被停止，或者恢复执行，status记录对应的值这里要重点说明一下，假设你是用SIGSTOP信号停止了子进程的运行，这个status的值可不是SIGSTOP所对应的常量值，具体是多少，取决于系统，mac下的和linux下的值是不一样的

那怎么==根据status的值来判断是停止还是恢复亦或是被信号杀死==呢还好？系统提供了跨平台的判断方法，具体看下面的例子

```python
#coding=utf-8
import os
import time
import errno

def child_work2():
    # 子进程
    print('我是子进程',os.getpid())
    i = 0
    while True:
        print('子进程{i}'.format(i=i))
        i += 1
        time.sleep(3)

def test_wait():
    pid = os.fork()
    if pid == 0:
        child_work2()
    else:
        print('子进程',pid)
        while True:
            try:
                time.sleep(4)
                p, status = os.waitpid(pid,os.WNOHANG|os.WUNTRACED|os.WCONTINUED)
            except OSError:
                print('没有子进程需要等待')
                break

            print(p,status)
            if p == 0:
                pass
                #print u'子进程没有退出'
            elif p < 0:
                if status == errno.EINTR:
                    print('被信号中断')
                elif status == errno.ECHILD:
                    print('该pid不可等待')
            else:
                if os.WIFSTOPPED(status):
                    print('子进程并没有退出,只是停止工作')
                elif os.WIFCONTINUED(status):
                    print('子进程恢复了运行')
                else:
                    print('子进程结束了')
                    break

            time.sleep(5)

if __name__ == '__main__':
    test_wait()
```

实验步骤如下:

1. 程序启动后，父进程创建一个子进程，然后开始进行waitpid操作，子进程则每隔3秒做一次输出。
2. 终端执行`kill -17 <子进程的PID>`，子进程会停止打印，同时父进程会提示子进程没有退出，只是停止工作
3. 一段时间后，再执行`kill -19 <子进程PID>`，子进程被唤醒，继续打印，而父进程也会提示子进程恢复了运行，

强调一点，我刚才讲述的是在mac环境下，mac和linux环境下的信号所对应的常量值是不一样的，执行kill -l，可以查看信号与常量值之间的关系

如果你是linux环境下实验：

- 发送停止信号时应该是kill -19
- 发送恢复信号时，应该是kill -18

