# 背景

## 介绍+历史+特性

NumPy(Numerical Python) 是 Python 语言的一个扩展程序库，支持大量的**维度数组与矩阵运算**，此外也针对数组运算提供大量的数学函数库。



NumPy 的前身 Numeric 最早是由 Jim Hugunin 与其它协作者共同开发，2005 年，Travis Oliphant 在 Numeric 中结合了另一个同性质的程序库 Numarray 的特色，并加入了其它扩展而开发了 NumPy。NumPy 为开放源代码并且由许多协作者共同维护开发。



NumPy 是一个**运行速度非常快的数学库，主要用于数组计算**，包含：

- 一个强大的N维数组对象 ndarray
- 广播功能函数
- 整合 C/C++/Fortran 代码的工具
- 线性代数、傅里叶变换、随机数生成等功能



## 应用

NumPy 通常与 SciPy（Scientific Python）和 Matplotlib（绘图库）一起使用， **这种组合广泛用于替代 MatLab**，是一个强大的科学计算环境，有助于我们通过 Python 学习数据科学或者机器学习。



SciPy 是一个开源的 Python 算法库和数学工具包。

- SciPy 包含的模块有**最优化**、**线性代数**、积分、插值、特殊函数、快速傅里叶变换、信号处理和图像处理、常微分方程求解和其他科学与工程中常用的计算。



Matplotlib 是 Python 编程语言及其数值数学扩展包 NumPy 的可视化操作界面。

- 它为利用通用的图形用户界面工具包，如 Tkinter, wxPython, Qt 或 GTK+ 向应用程序嵌入式绘图提供了应用程序接口（API）。





# 安装

## Windows

1 已有的发行版本

包含了所有的关键包（包括 NumPy，SciPy，matplotlib，IPython，SymPy 以及 Python 核心自带的其它包）



2 pip安装

```shell
pip3 install --user numpy scipy matplotlib	#默认情况使用国外线路，国外太慢
pip3 install numpy scipy matplotlib -i https://pypi.tuna.tsinghua.edu.cn/simple	#清华的镜像
```



## Linux

```shell
#=# Ubuntu & Debian
sudo apt-get install python-numpy python-scipy python-matplotlib ipython ipython-notebook python-pandas python-sympy python-nose

#=# CentOS/Fedora
sudo dnf install numpy scipy python-matplotlib ipython python-pandas sympy python-nose atlas-devel

#=# Mac 系统：Homebrew 不包含 NumPy 或其他一些科学计算包
pip3 install numpy scipy matplotlib -i https://pypi.tuna.tsinghua.edu.cn/simple
```



测试是否安装成功：

```python
print(np.__version__)

>>> from numpy import *
>>> eye(4)
array([[1., 0., 0., 0.],
       [0., 1., 0., 0.],
       [0., 0., 1., 0.],
       [0., 0., 0., 1.]])
```





# help命令

## Finding help

| [`lookfor`](https://numpy.org/doc/stable/reference/generated/numpy.lookfor.html#numpy.lookfor)(what[, module, import_modules, …]) | Do a keyword search on docstrings. |
| ------------------------------------------------------------ | ---------------------------------- |

## Reading help

| [`info`](https://numpy.org/doc/stable/reference/generated/numpy.info.html#numpy.info)([object, maxwidth, output, toplevel]) | Get help information for a function, class, or module.       |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [`source`](https://numpy.org/doc/stable/reference/generated/numpy.source.html#numpy.source)(object[, output]) | Print or write to a file the source code for a NumPy object. |

