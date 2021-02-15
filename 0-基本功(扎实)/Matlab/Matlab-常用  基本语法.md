# Matlab-常用 | 基本语法

## 一 常见操作

### 1 切片

比如，10:2:20



### 2 常量

- pi
- j, i
- eps：最小的数
- inf
- nan：比如0/0



3 常见操作

- clear：清除所有内存
- close all：关闭所有窗口
- clc：清空所有显示
- clock：记录时间

- - etime(t1, t2)
  - tic
  - toc





## 二 运算：元素级 VS 矩阵级

### 0 基本运算

- \+ - * / \ ^ '
- 左除\

- - X=A\B=A-1•B

- 右除/

- - X=A/B=A•B-1

### 1 元素级运算

- .* .\ ./ .^ .'

### 2 矩阵运算

- ' 转置
- diag()
- inv()
- det()





## 三 模块 | 设定初始值

- randn()
- randi(range, m, n)
- zeros(m, n)
- ones(m, n)





## 四 模块 | 图表可视化

- plot

- - grid
  - hold

- subplot()
- title
- xlabel
- ylabel
- legend



## 参考文献

[训练 | 基本语句](https://blog.csdn.net/gsls200808/article/details/45267969)