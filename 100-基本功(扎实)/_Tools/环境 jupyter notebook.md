# 安装环境

在cmd(pip)或者Anaconda中安装jupyter notebook

然后输入jupyter notebook即可打开





通过!来调用Shell的一些命令

- 只能执行ls 、pwd 等简单命令
- 当需要执行tar 或者稍微负责命令时就报错了

magic函数主要包含两大类，一类是行魔法（Line magic）前缀为%，一类是单元魔法(Cell magic)前缀为%%



# 魔术函数@IPython

- 命令后面加问号(?)可以查看函数的参数

```python
### ?查看函数的参数
%time?

### %quickref 或 %magic可以查看所有的命令
%timeit   #多次执行一条语句，并返回平均时间
%%timeit   #多次执行多条语句，并返回平均时间
%time   #返回执行一条语句的时间
%%time  #返回执行多条语句的时间

%reset  #删除当前空间的全部变量
%run *.py   #在IPython中执行Python脚本


###常用的魔术命令
%quickref thon 快速参考
%magic 显示magic command详细文档
%debug 从最新的异常跟踪的底部进入交互式调试器
%hist 打印命令输入历史
%pdb 在发生异常后自动进入调试器
%paste执行剪贴板中的Python代码
%cpaste 打开一个特殊的提示符以便手工粘贴待执行的代码
%reset 删除interactive空间中的全部变量/名称
%run 执行一个python脚本
%page 分页显示一个对象
%time 报告statement执行的时间
%timeit 多次执行statement以计算平均执行时间，用于执行时间非常小的代码。
%who、%who_is、%whos 显示Interactive命名空间的中定义的变量，信息级别/冗余度可变
%xdel 删除变量，并尝试清楚其在IPython中的对象上的一切引用
```







# #参考文献

[Link: Jupyter Notebook 常用魔法命令](https://blog.csdn.net/u011213419/article/details/81299282?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-2.control&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-2.control)