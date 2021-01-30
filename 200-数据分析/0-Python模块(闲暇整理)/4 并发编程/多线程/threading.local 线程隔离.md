@教程|廖雪峰



## 背景

在多线程环境下，每个线程都有自己的数据。

一个线程使用自己的局部变量比使用全局变量好，因为局部变量只有线程自己能看见，不会影响其他线程，而==全局变量的修改必须加锁==。

但是局部变量也有问题，就是在函数调用的时候，传递起来很麻烦：

```python
def process_student(name):	#进程
    std = Student(name)
    # std是局部变量，但是每个函数都要用它，因此必须传进去：
    do_task_1(std)
    do_task_2(std)

def do_task_1(std):		#线程1/任务：多个子任务
    do_subtask_1(std)
    do_subtask_2(std)

def do_task_2(std):		#线程2
    do_subtask_2(std)
    do_subtask_2(std)
```

每个函数一层一层调用都这么传参数那还得了？用全局变量？也不行，因为每个线程处理不同的`Student`对象，不能共享。



如果用一个全局`dict`存放所有的`Student`对象，然后==以`thread`自身作为`key`==获得线程对应的`Student`对象如何？

这种方式理论上是可行的，它最大的优点是消除了`std`对象在每层函数中的传递问题

但是，每个函数获取`std`的代码有点丑。

```python
global_dict = {}

def std_thread(name):
    std = Student(name)
    # 把std放到全局变量global_dict中：
    global_dict[threading.current_thread()] = std
    do_task_1()
    do_task_2()

def do_task_1():
    # 不传入std，而是根据当前线程查找：
    std = global_dict[threading.current_thread()]
    ...

def do_task_2():
    # 任何函数都可以查找出当前线程的std变量：
    std = global_dict[threading.current_thread()]
    ...
```





# ThreadLocal

不用查找`dict`，`ThreadLocal`帮你自动做这件事

```python
import threading
    
# 创建全局ThreadLocal对象:
local_school = threading.local()

def process_student():		#进程
    # 获取当前线程关联的student:
    std = local_school.student
    print('Hello, %s (in %s)' % (std, threading.current_thread().name))

def process_thread(name):	#线程
    # 绑定ThreadLocal的student:
    local_school.student = name
    process_student()

t1 = threading.Thread(target= process_thread, args=('Alice',), name='Thread-A')
t2 = threading.Thread(target= process_thread, args=('Bob',), name='Thread-B')
t1.start()
t2.start()
t1.join()
t2.join()

'''执行结果
Hello, Alice (in Thread-A)
Hello, Bob (in Thread-B)
'''
```

全局变量`local_school`就是一个`ThreadLocal`对象：

- 每个`Thread`对它都可以读写`student`属性，但互不影响
- 你可以把`local_school`看成全局变量，但每个属性如`local_school.student`都是线程的局部变量，可以任意读写而互不干扰，也不用管理锁的问题，`ThreadLocal`内部会处理



可以理解为全局变量`local_school`是一个`dict`，不但可以用`local_school.student`，还可以绑定其他变量，如`local_school.teacher`等等



## 应用

`ThreadLocal`最常用的地方就是为每个线程绑定一个数据库连接，HTTP请求，用户身份信息等，这样一个线程的所有调用到的处理函数都可以非常方便地访问这些资源。



一个`ThreadLocal`变量虽然是全局变量，但==每个线程都只能读写自己线程的独立副本，互不干扰==。`ThreadLocal`解决了==参数在一个进程中各个函数之间互相传递==的问题。



---

@教程|酷python

示例代码：

```python
import threading

mylocal = threading.local()
mylocal.value = 0


def add_one(index):
    mylocal.value = index
    print(mylocal.value)

for i in range(1, 11):
    t = threading.Thread(target=add_one,args=(i,))
    t.start()

print(mylocal.value)
```

如果你有一定的多线程编程经验，你应该会回答说结果不确定，因为mylocal.value的最终值是不确定的，10个线程对它进行写操作，只有最后那个执行的线程才会生效，实际的过程比这还要复杂



对内存中一个变量进行修改，可总结为三个步骤：

1. 将变量读取到寄存器中
2. 在寄存器中修改
3. 写回内存

哪个线程最后执行这第3步，哪个线程的执行是生效的, 你能理解到这一层次，已经非常好了

但是，这不是上面问题的答案



# 线程隔离

实际执行上述代码的结果是0

threading.local() 返回的是一个特殊的对象，它的状态是线程隔离的

- 每个线程的对value赋值，其实是在对不同的值进行赋值

python的对象里，属性值保存在字典中，显然，mylocal里保存了多份字典，区分他们的正是线程ID



web框架flask,基于Werkzeug实现，而Werkzeug 自己封装了werkzeug.local.Local ，其效果和threading.local 基本一致

但也有一些不同，正是由于使用了这样的技术，所以App Context 对象和 Request Context 对象是请求间隔离的，也就是说，在多线程环境下，每个request对象都会准确的找到自己的信息