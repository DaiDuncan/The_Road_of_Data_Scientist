线程安全的FIFO实现

python的内置模块queue提供了线程安全的队列数据结构，

- 有基本的先进先出的队列Queue
- 后进先出的队列LifoQueue
- 优先级队列PriorityQueue



# 基本FIFO队列

first in first out, 先进先出队列，将元素从队列的一端放入，从另一端删除

```python
import queue

q = queue.Queue()

for i in range(10):
    q.put(i)

while not q.empty():
    print(q.get())	#0 1 2 3 4 5 6 7 8 9
```





# LIFO队列 => 栈

与FIFO队列相反，LIFO队列是后进先出，这是一种栈结构

```python
import queue

q = queue.LifoQueue()

for i in range(10):
    q.put(i)

while not q.empty():
    print(q.get())
```



栈 数据结构有很广泛的用途，比如下面这个字符串 `((sdf)sdf)(sg()())gsg((sgsg()))`

写算法判断，字符串里的小括号是否是成对出现，使用后进先出这种数据结构，可以非常轻松的实现算法：

```python
import queue

string = '((sdf)sdf)(sg()())gsg((sgsg()))'


def is_pair(string):
    q = queue.LifoQueue()

    for item in string:
        if item == '(':
            q.put(item)
        elif item == ')':	#成对呼应
            if q.empty():
                return False
            q.get()

    return q.empty()
```



# 优先队列

前面介绍的队列，都是根据进入队列的顺序来决定出队列的顺序

除了这两种情况之外，还需要根据优先级来决定出队列的顺序@任务的优先级

```python
import queue


class Task:
    def __init__(self, priority, description):
        self.priority = priority	#优先级是重要属性
        self.description = description

    def __eq__(self, other):
        return self.priority == other.priority

    def __lt__(self, other):
        return self.priority < other.priority

    def __str__(self):
        msg = '{_class} 优先级:{priority} 任务:{task}'
        return msg.format(_class=self.__class__, priority=self.priority, task=self.description)

q = queue.PriorityQueue()	#Task类对象的比较：重载运算符
q.put(Task(3, '打游戏'))
q.put(Task(5, '做饭'))
q.put(Task(1, '洗碗'))

while not q.empty():
    print(q.get())
    
'''
<class '__main__.Task'> 优先级:1 任务:洗碗
<class '__main__.Task'> 优先级:3 任务:打游戏
<class '__main__.Task'> 优先级:5 任务:做饭
'''
```



