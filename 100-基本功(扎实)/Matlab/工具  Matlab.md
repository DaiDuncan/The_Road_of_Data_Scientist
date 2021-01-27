# 工具 | Matlab

## 一 搭建开发环境

- 授权：学校官网
- 安装方式：直接安装matlab，需要的模块另外专门安装
- 学校

| postdoctoral     | 博士后        |
| ---------------- | ------------- |
| graduate level   | 研究生水平    |
| undergraduate    | 大学生-本科生 |
| K-12 Pre college | 预科生        |

### 1 已安装组件

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1592839702411-f0a01618-1897-481d-92a8-2f221666f2d0.png)

### 2 许可证

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1592839717375-ca4cfcd4-0c4f-4e7a-805b-968baae3c5c7.png)



## 二 基础 | 变量

变量：用于保存和处理数据

- 定义某种数据类型的变量

编译器会根据**数据类型**分配一定的内存空间，程序通过**变量名访问内存**

- 变量名最多包含63个字符

以字母开头，不可以下划线开头



### 1 局部变量

每个函数都有局部变量，存储在函数的独立的工作区里，和其他函数的局部变量以及**主工作区的变量**分开存储。

函数调用结束，他们就会被删除。



### 2 全局变量

**声明**：global X_val

在一个工作区改变值， 其他工作区也会随之改变。

在**函数开头位置**定义， **用大写字母**。

使用它的目的是： 减少数据传递次数

使用全局变量有风险，：变量耦合，容易造成错误



### 3 永久变量

**声明**：persistent a %用persistent声明

只在m文件中定义和使用， 不能在命令行声明



只允许声明它的函数存取

声明他的函数退出时，不从内存中清理它



### 4 特殊变量

| 特殊变量  | 描述                               |
| --------- | ---------------------------------- |
| ans       | 系统默认：用来保存运算结果         |
| pi        | 圆周率                             |
| eps       | 机器零阈值：MATLAB中的最小数       |
| inf       | 表示无穷大                         |
| NaN或nan  | 表示不定数                         |
| i或j      | 虚数                               |
| nargin    | 函数输入参数的个数                 |
| nargout   | 函数输出参数的个数                 |
| realmin   | 可用的最小正实数                   |
| realmax   | 可用的最大正实数                   |
| intmax    | 可用的最大正整数(双精度格式存储)   |
| varargin  | 可变的函数输入参数的个数           |
| varargout | 可变的函数输出参数的个数           |
| beep      | 计算机发出声音(平时matlab的报错声) |





## 三 基础 | 关键字

\>>>iskeyword   %一共20个

| 关键字              | 描述                                                         |
| ------------------- | ------------------------------------------------------------ |
| **#控制循环**       |                                                              |
| forend              |                                                              |
| while               |                                                              |
| breakcontinue       |                                                              |
| **#控制判断、选择** |                                                              |
| ifelseelseif        |                                                              |
| switchcaseotherwise |                                                              |
| **#函数**           |                                                              |
| functionreturn      |                                                              |
| classdef            | 定义类@面向对象解决变量名冲突和让函数、对象的结构清晰其中，static function可以在不定义类的实例前提下，直接调用类的成员函数  属性(不能与类具有相同的名称)  方法  事件  枚举 |
| **#内存变量的属性** |                                                              |
| global              |                                                              |
| persistent          |                                                              |
| **#并行编程**       |                                                              |
| parfor              | 并行for循环@Parallel Computing Toolbox 软件                  |
| spmd                | SPMD（Single Program/Multiple Data）单程序多任务进行任务并行：并行可分为两种，一种是任务并行（parfor），另一种则数据并行（Spmd）。Single program”方面指的是同一段代码运行在不同的多个lab上。当有大数据量需要同时处理，而单机又无法存储大数据量时，可考虑使用数据并行编程方法。Spmd 结构和分布式数组可实现数据并行处理。 |
| #方便调试           |                                                              |
| try                 | @python                                                      |
| catch               |                                                              |
| #其他               |                                                              |
| inputkeyboard       | value = input('message') %将用户输入的内容赋值给变量valuevalue = input('message', 's') %将用户输入的内容以字符串的形式赋值给变量value @keyboard允许输入任意多个matlab命令，而input命令只允许用户输入赋值给变量的数组、字符串或元胞数组等  只有当keyboard输入return时，控制权收回 |
| pause               | pause(n)暂停n秒                                              |
| errorwarning        | error('message') %显示出错误信息message, 终止程序errortrap %在错误发生后，控制程序继续执行与否的开关lasterr %终止程序并显示matlab系统判断的最新出错原因warning('message') %显示警告信息message, 继续运行程序lastwarn %显示matlab系统给出的最新警告程序，并继续运行 |

```
% 演示：switch-case-otherwise
n = input('Enter a number:');

switch n
    case -1
    disp('negetive one')
    case 0
    disp('zero')
  case 1
    disp('positive one')
  otherwise
    disp('other value')
end


% 演示：并行编程
% 使用三个工作线程或核的三次大特征值计算
parpool(3)
parfor i=1:3, c(:,i) = eig(rand(1000)); end


% classdef
classdef tools < handle
    methods (Static = true)
    function a = test(b, c)
        a = b + c;
    end
  end
end

% 此时，可直接通过a = tools.test(b, c)调用函数
```



## 四 基础 | 基本语句

| 主体     | 变量函数表达式程序控制语句 |
| -------- | -------------------------- |
| 句终符号 | ，；回车                   |
| 注释     | %                          |



### 1 顺序语句

```
a = 1
b = 5
c = a/b
```

### 2 分支语句

```
% if-else-end

t = 1:5
if 0    %条件恒为假
    t1 = 6 - t
elseif 0
    t1 = t - 2
else
    t1 = t
end


% switch-case
num = 3;
switch num
    case 1
    data = '差'
  case 2
    data = '次'
  case 3
    data = '中'
  case 4
    data = '良'
  case 5
    data = '优'
    otherwise
    data = '请确认输入'
end


% try-catch
n = 16;
mat = magic(3)  %生成3阶魔法矩阵
try
    mat_num = mat(n)    %取mat的第n个元素
catch
    mat_end = mat(end)  %若mat没有第n个元素，则取mat的最后一个元素
  reason = lasterr  %显示出错原因：lasterr查询出错原因
end

%% case后的值为数组：不同的缴税级别
switch floor(price/1000)
    case {0,1}
        taxes=0; 
    case {2,3,4}
        taxes=(price-2000)*0.02; 
    otherwise
        taxes=(5000-2000)*0.02+(price-5000)*0.05;
        fee=60;        
end        
%%
```

- 判断条件的变量

- - 标量：用逻辑判断==
  - 字符串：用strcmp

- case后的值可以是标量，字符串，元胞数组

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1592841874282-fa6723d0-f602-498a-a7d8-00337e14ed91.png)



### 3 循环语句

```
%% for
for i = 1:4
    for j = 1:4
    if i >= j
        mat(i, j) = i^2;
    else
        mat(i, j) = j^2;
    end
  end
end


%% for循环变量
% 分配矩阵值
s = 10;
H = zeros(s);

for c = 1:s
    for r = 1:s
        H(r,c) = 1/(r+c-1);
    end
end


% 递减值
for v = 1.0:-0.2:0.0
   disp(v)
end


% 执行指定值的语句
for v = [1 5 8 17]
   disp(v)
end


% 对每个矩阵列重复执行语句
for I = eye(4,3)
    disp('Current unit vector:')
    disp(I)  %%显示该列
end


======================================================
%% while 循环体被执行的次数不确定
sum = 0;
i = 0;
while i <= 100
    sum = sum + i;
    i = i + 2;
end


% continue跳至下一循环迭代
fid = fopen('magic.m','r');
count = 0;
while ~feof(fid)    %%涵盖判断条件
    line = fgetl(fid);
    if isempty(line) || strncmp(line,'%',1) || ~ischar(line)
        continue
    end
    count = count + 1;
end
count

fclose(fid);


% break在表达式为 false 之前退出循环
limit = 0.8;
s = 0;

while 1
    tmp = rand;
    if tmp > limit
        break   %%用break 满足条件即退出
    end
    s = s + tmp;
end
```

#### 1）表达式

表达式可以包含**关系**运算符（例如 < 或 ==）和**逻辑**运算符（例如 &&、|| 或 ~）

使用逻辑运算符 and 和 or 创建复合表达式。

MATLAB按照运算符优先级规则**从左至右**计算复合表达式。





在 while...end 块的条件表达式中，**逻辑运算符 & 和 |** 的行为方式和**短路运算符**一样。此行为分别相当于 && 和 ||。

由于 && 和 || 在条件表达式和语句中**一致短路**，因此，建议在该表达式中**使用 && 和 ||**，而不是 & 和 |。

例如：

```
x = 42;
while exist('myfunction.m','file') && (myfunction(x) >= pi)
    disp('Expressions are true')
    break
end
```

表达式的第一部分的计算结果为 false。

因此，MATLAB 不需要计算表达式的第二部分，否则会导致**未定义的函数**错误。





#### 2）注意

- 要以编程方式退出循环，请使用 **break 语句**
- 要跳过循环中的其余指令，并开始下一次迭代，请使用 **continue 语句**
- 避免在**循环语句内**对 index 变量**赋值**。for 语句会覆盖循环中对 index 所做的任何更改
- 要对单列向量的值进行**迭代**，首先将其转置，以创建一个**行向量**。
- 如果意外创建了一个无限循环（即永远不会自行结束的循环），请按下 Ctrl+C 停止执行循环
- 如果条件表达式的计算结果是一个矩阵，则仅当该矩阵中的所有元素都为 true（非零）时，MATLAB 才会计算这些语句。要在**任何元素为 true** 时执行语句，请在 **any 函数**中对表达式换行。
- 嵌套许多 while 语句时，每个 while 语句都需要一个 end 关键字



## 五 函数与脚本

### 1 脚本：.m文件

### 2 函数

#### 1）一般函数(末尾没有end)

- lookfor命令查询
- help在线帮助

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1592847715748-b54c02c3-f330-48ab-a2d5-c1b7a6187186.png)

#### 2）匿名函数

在命令行窗口中创建函数句柄 f = @(input1, input2, ...) expression

- f为创建的函数句柄
- input1等为输入变量
- expression：函数的表达式

如果匿名函数没有输入参数：则在调用函数时，需要用空格来替换input，否则将不执行该程序

```
sqr = @(x) x.^2 %创建匿名函数句柄
t = sqr([1.25 2])
whos

#OUTPUT#
Name  Size  Bytes  Class Attributes
sqr   1x1    16    function_handle
t     1x2    16         double
```



#### 3）子函数

```
function F = ex4_19(n)
    A = 1; w = 2; phi = pi/2;
  signal = createsig(A, w, phi);
  F = signal.^n

% 子函数
function signal = createsig(A, w, phi)
x = 0:pi/100:pi*2;
signal = A * sin(w*x + phi)
```

#### 4）私有函数

只能被private**直接父目录下的M文件**所调用，不能被**其他目录下**的任何M文件**或命令行**调用



#### 5）重载函数

功能类似，但变量属性不同：双精度浮点 vs 整型



#### 6）eval与feval



#### 7）内置函数：MATLAB底层函数

一般**不是用MATLAB语言写成的**、无法看到其源代码（只能看到注释）、执行效率相对较高。

用exist函数判断该函数是否为builtin函数。将确定为builtin函数的函数名称写入一个table中，最后去重排序。

- MATLAB R2019a (至少)有595个built-in函数
- 尽量避免使用内置函数名称作为自己的函数/变量名称



#### 8）符号函数

```
% 示例：符号函数f(x1, x2)
syms x1 x2;
f=(x1-2)^2+2*(x2-1)^2;
x=[1;3];
e=10^(-20);
[k ender]=steepest(f,x,e)


% steepest函数
function [k ender]=steepest(f,x,e)
% 梯度下降法,f为目标函数（两变量x1和x2），x为初始点,如[3;4]

syms x1 x2 m; %m为学习率
d=-[diff(f,x1);diff(f,x2)];  %分别求x1和x2的偏导数，即下降的方向

flag=1;  %循环标志
k=0; %迭代次数

while(flag)
    d_temp = subs(d,x1,x(1));      %将起始点代入，求得当次下降x1梯度值
    d_temp = subs(d_temp,x2,x(2)); %将起始点代入，求得当次下降x2梯度值
    nor = norm(d_temp); %范数
    if(nor >= e)
        x_temp = x + m*d_temp;            %改变初始点x的值
        f_temp = subs(f,x1,x_temp(1));  %将改变后的x1和x2代入目标函数
        f_temp = subs(f_temp,x2,x_temp(2));
        h = diff(f_temp,m);  %对m求导，找出最佳学习率
        m_temp = solve(h);   %求方程，得到当次m
        x = x + m_temp*d_temp; %更新起始点x
        k = k + 1;
    else
        flag = 0;
    end
end

ender = double(x);  %终点
end
```

**
**

**
**

## 六 补充

### 1 注意点

#### 1）matlab中默认列向量(numpy中默认行向量)

- x=[x1 ; x2]中间为分号



#### 2）内存优化技术

@文档 R2016a  S.150

- 加减法 优于乘除法
- load/save 优于文件I/O操作
- 代码**向量化**

**
**

**
**

## 参考文献

[已下载]MATLAB R2016a完全自学一本通

[Matlab论坛](https://www.ilovematlab.cn/thread-22239-1-1.html)

[MATLAB动态神经网络做时间序列预测教学视频](https://www.ilovematlab.cn/thread-113431-1-1.html)

[《了解卡尔曼滤波器（中文字幕）》系列视频](https://www.ilovematlab.cn/thread-567568-1-1.html)

[关键字 | spmd-数据并行](http://www.voidcn.com/article/p-kvbjozdn-bcw.html)