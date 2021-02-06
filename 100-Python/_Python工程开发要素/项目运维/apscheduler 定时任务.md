一个很常见的需求，在某天的固定时刻，需要执行某个python脚本来完成特定的任务，面对这样需求，你可能想到的最简单的实现方法是在程序里使用while循环，每次循环使用time.sleep(1)，等时间来到规定的时间点就执行某个函数来完成任务，这样的方法可行么？当然可以，只不过太丑陋了。



在linux系统下，你可以使用crontab命令设置定时任务，不过这个命令对于初学者来说用起来不那么简单直观



# 定时任务框架APScheduler

```text
pip install apscheduler
```





该框架的接口定义十分友好，比如你希望你的爬虫函数run_spider 在每天的10点15分能够准时启动：

```python
from apscheduler.schedulers.blocking import BlockingScheduler

def run_spider():
    print("启动爬虫")


sched = BlockingScheduler()
sched.add_job(run_spider, 'cron', hour=10, minute=15)
sched.start()
```

在add_job方法里，关于时间的设置，可以参考 CronTrigger类的初始化函数





如果你希望自己的爬虫每3分钟就执行一次爬取：

```python
from apscheduler.schedulers.blocking import BlockingScheduler

def run_spider():
    print("启动爬虫")


sched = BlockingScheduler()
sched.add_job(run_spider, 'interval', minutes=3)
sched.start()
```

关于时间的设置，可以参考IntervalTrigger类的初始化函数



掌握上面两种定时任务设置方法，搞定绝大多数定时任务已经绰绰有余了