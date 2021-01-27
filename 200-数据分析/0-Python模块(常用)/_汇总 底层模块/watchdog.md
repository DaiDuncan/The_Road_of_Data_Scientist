在python中创建看门狗，监控文件系统变化

看门狗是一款小软件，可以监控文件和目录是否发生变化

当被监视的区域发生文件或目录的创建，修改，或者删除时，就可以引发特定的事件，我们只需要编写针对这些事件的函数即可处理这些变化。@STM32 定时器中断



## 安装和导入

```python
pip install watchdog

import time
from watchdog.observers import Observer
from watchdog.events import PatternMatchingEventHandler
```



# 创建事件处理对象

有很多有用的参数需要进行设置：

1. watch_patterns 设置监控文件的模式，如果你想监控所有文件，那么设置成"*" 即可，我所设置的模式只会监控以.py 和 .txt结尾的文件
2. ignore_patterns 设置忽略的文件模式，我这里没有忽略任何文件
3. ignore_directories 设置为True==表示忽略文件夹的变化==
4. case_sensitive 设置大小写是否敏感，如果设置为True，那么修改文件名称时，如果只是大小写发生变化，那么则不会被监控

```python
watch_patterns = "*.py;*.txt"    # 监控文件的模式
ignore_patterns = ""     # 设置忽略的文件模式
ignore_directories = False      # 是否忽略文件夹变化
case_sensitive = True           # 是否对大小写敏感
event_handler = PatternMatchingEventHandler(watch_patterns, ignore_patterns, ignore_directories, case_sensitive)
```





# 处理事件

一共有四种事件需要处理，对应的，编写4个函数

```python
def on_created(event):
    print(f"{event.src_path}被创建")

def on_deleted(event):
    print(f"{event.src_path}被删除")

def on_modified(event):
    print(f"{event.src_path} 被修改")

def on_moved(event):
    print(f"{event.src_path}被移动到{event.dest_path}")

event_handler.on_created = on_created
event_handler.on_deleted = on_deleted
event_handler.on_modified = on_modified
event_handler.on_moved = on_moved
```



# 创建观察者

最终，我们需要创建一个观察者，来负责启动监控任务：

- watch_path 设置监控的目录

- go_recursively 设置为True表示监控子文件夹

```python
watch_path = "/Users/kwsy/data/test"  # 监控目录
go_recursively = True    # 是否监控子文件夹
my_observer = Observer()
my_observer.schedule(event_handler, watch_path, recursive=go_recursively)


my_observer.start()
try:
    while True:
        time.sleep(1)
except KeyboardInterrupt:
    my_observer.stop()
    my_observer.join()
```

一个观察者可以多次使用schedule方法来监控文件系统，这样，我们可以一次监控多个区域，每个区域使用不同的监控策略。



现在，在所监控的目录下执行新建文件的操作，就会被这段程序所监控到，并使用on_created函数做相应的处理。

