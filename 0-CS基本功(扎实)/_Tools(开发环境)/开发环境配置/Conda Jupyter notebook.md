@`_cmd(pip命令行)`

- 查看版本等



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



# 安装环境

在cmd(pip)或者Anaconda中安装jupyter notebook

然后输入jupyter notebook即可打开



通过!来调用Shell的一些命令

```shell
!pip install lightgbm
!pip install xgboost
!pip install catboost
```

- 只能执行ls 、pwd 等简单命令
- 当需要执行tar 或者稍微负责命令时就报错了

magic函数主要包含两大类，一类是行魔法（Line magic）前缀为%，一类是单元魔法(Cell magic)前缀为%%



## 1 专题|kernel

jupyter notebook运行需要的kernel和conda创建的虚拟环境并不能完全互通。利用conda创建了虚拟环境，但是启动jupyter notebook之后却找不到虚拟环境。

实际上是由于在虚拟环境下缺少kernel.json文件。

```python
#==# 查看kernel
jupyter kernelspec list	#查看jupyter所有的kernel
jupyter kernelspec remove "kernelname"	#可以删除指定的kernel

#==# 向Jupyter中添加conda虚拟环境
# 0新建虚拟环境：自带python，可选择ipykernel
conda create -n [env] python=3.5 ipykernel

# 0先安装ipykernel => 一般是在默认的主目录中安装 => 各env通用
conda install ipykernel	

# 1在虚拟环境下创建kernel文件
conda install -n [env] ipykernel

# 2激活虚拟环境
conda activate [env]

# 3将环境写入notebook的kernel中
python -m ipykernel install --user --name [env] --display-name "显示的名称"
```

注意：添加kernel可能需要重启(等待自动更新)



1 kernel所在的地址

![image-20210518204654872](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210518204655.png)



2 pytorch里面有`kernel.json`

内容如下：映射到conda所创建的pytorch环境中

```json
{
 "argv": [
  "D:\\JustPractice\\1_MS_Geek_salute\\PythonAnaconda\\envs\\pytorch\\python.exe",
  "-m",
  "ipykernel_launcher",
  "-f",
  "{connection_file}"
 ],
 "display_name": "pytorch2021",
 "language": "python"
}
```



## 汇总|python3 pytorch tf

| 基本库 + numpy+pandas+matlibplot+seaborn |        |
| ---------------------------------------- | ------ |
| 【env1 python3】                         |        |
| torch                                    |        |
|                                          |        |
| 【env2 pytorch】                         |        |
| torch                                    |        |
|                                          |        |
| 【env3 tensorflow】                      |        |
| tensorflow                               | 1.10.0 |
|                                          |        |









# 问题

## 1 kernel无法加载

1）DLL load failed 找不到指定模块

![img](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210518090418.png)

看到报错代码里面有个zmq文件夹下面的，参考网上的一些做法，然后连猜带蒙重装了pyzmq，问题得以解决。

```shell
pip uninstall pyzmq
pip install pyzmq
```







# #参考文献

[Link: Jupyter Notebook 常用魔法命令](https://blog.csdn.net/u011213419/article/details/81299282?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-2.control&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-2.control)