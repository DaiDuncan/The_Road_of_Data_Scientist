仅适用于linux环境



# CPU亲和性

CPU亲和性是指==进程在某个给定的CPU上长时间运行==，尽可能少的迁移到其他处理器的倾向性。

linux内核的进程调度器天生就具有这样的特性，它尽可能保证一个进程不在处理器之间频繁的迁移，频繁的迁移意味着会增加CPU缓存miss的概率，增加从主存到CPU缓存的复制时间。

2.6版本linux内核新增了一个机制，它允许开发人员可以编程实现==硬CPU亲和性==，即程序可以显示的指定进程在哪个CPU上执行。



## CPU亲和性应用场景

1. 有大量计算
2. 系统对时间敏感，对性能要求高



最常见的例子就是nginx，在配置nginx子进程的数量时，一般设置成CPU的核心数，同时通过设置worker_cpu_affinity，将不同的子进程绑定到不同的CPU核上，来增强nginx的响应能力。

如果你的服务器只有一个核，那么是否绑定也就无所谓了。





# python程序如何绑定CPU

Linux 内核 API 提供了一些方法，让用户可以修改位掩码或查看当前的位掩码：

1. sched_set_affinity() （用来修改位掩码）
2. sched_get_affinity() （用来查看当前的位掩码）

python开发人员不需要使用这么底层的API，第三方库psutil就可以完成这些操作

```python
import psutil

count = psutil.cpu_count()
p = psutil.Process()

cpu_lst = p.cpu_affinity()  # [0, 1, 2, 3]
p.cpu_affinity([0, 1])     # 将进程绑定到0和1这两个cpu核上
```



