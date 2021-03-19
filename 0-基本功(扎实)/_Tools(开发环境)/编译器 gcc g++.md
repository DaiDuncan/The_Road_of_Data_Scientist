[link: 在windows XP上安装C++编译器](https://cs.calvin.edu/courses/cs/112/resources/installingEclipse/cygwin/)(版本旧，但是最新的也能使用)

- `g++`, the GNU C++ compiler,
- `gdb`, the GNU debugger, and
- `make`, a utility for compiling and linking multi-file projects.



# 环境|IDE

## 使用 Visual Studio (Graphical Interface) 直接编译

[link: 菜鸟教程](https://www.runoob.com/cplusplus/cpp-environment-setup.html)



# 环境|[菜鸟教程](https://www.runoob.com/cplusplus/cpp-environment-setup.html)

## 文本编辑器

文本编辑器包括 Windows Notepad、OS Edit command、Brief、Epsilon、EMACS 和 vim/vi。

- Notepad 通常用于 Windows 操作系统上
- vim/vi 可用于 Windows 和 Linux/UNIX 操作系统上

通过编辑器创建的文件通常称为源文件，源文件包含程序源代码。

C++ 程序的源文件通常使用扩展名 .cpp、.cp 或 .c



## C++ 编译器

"编译"，转为机器语言，这样 CPU 可以按给定指令执行程序

大多数的 C++ 编译器并不在乎源文件的扩展名，但是如果您未指定扩展名，则默认使用 .cpp

最常用的免费可用的编译器是 GNU 的 C/C++ 编译器，如果您使用的是 HP 或 Solaris，则可以使用各自操作系统上的编译器

-  GNU 的 gcc 编译器适合于 C 和 C++ 编程语言



### UNIX/Linux 上的安装

### Mac OS X 上的安装

### Windows 上的安装

[link: MinGW主页](http://mingw-w64.org/doku.php)

安装 MinGW 时，您至少要安装 gcc-core、gcc-g++、binutils 和 MinGW runtime，但是一般情况下都会安装更多其他的项。

添加您安装的 MinGW 的 bin 子目录到您的 **PATH** 环境变量中，这样您就可以在命令行中通过简单的名称来指定这些工具

完成安装时，您可以从 Windows 命令行上运行 gcc、g++、ar、ranlib、dlltool 和其他一些 GNU 工具。







# 环境|[知乎专栏](https://zhuanlan.zhihu.com/p/47935258)

Windows具有良好的界面和丰富的工具，所以目前linux开发的流程是

- windows下完成编码工作
- linux上实现编译工作



为了提高工作效率，有必要在windows环境下搭建一套gcc,gdb,make环境

[MinGW就是windows下gcc的版本](https://sourceforge.net/projects/mingw/files/MinGW/)

- 进入网址后点击下载mingw-get-setup.exe安装包

![image-20210303222100510](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210303222100.png)

- 在MinGW-Installation-Manager中选择gcc，gdb，make相关软件包即可
- 要正常使用MinGW，还需要设置环境变量
  - – 将C:\MinGW\bin加入PATH　　
    – 将C:\MinGW\include加入INCLUDE
    – 将C:\MinGW\lib加入LIB

**打开CMD在命令提示符下输入gcc –v,看到gcc版本信息，gcc安装OK**
**打开CMD在命令提示符下输入gdb –v,看到gdb版本信息，gdb安装OK**
**打开CMD在命令提示符下输入make –v,看到make版本信息，make安装OK**



## make命令

[link: 没有make命令](https://stackoverflow.com/questions/36770716/mingw64-make-build-error-bash-make-command-not-found)

先安装mingw-get，然后输入mingw-get install msys-make



## 测试

```c++
//test.c
#include <stdio.h>

void main() {
        printf("Hello World!");
}
```



```c++
//编译
gcc test.c -o test
    
//得到test.exe
//在windows平台上
.\test.exe
```



# GCC与g++

- GCC原名为GNU C语言编译器(GNU C Compiler)
- 现在GCC一般指的是GNU编译器套件(GNU Complier Collection)，它是由GNU开发的编程语言编译器。GNU编译器套件包括C、C++等语言以及这些语言的库

[link: 博客|GCC](http://blog.fpliu.com/it/software/GNU/gcc/)

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210303095347.png" alt="image-20210303095347354" style="zoom:67%;" />





## g++

- gcc与g++都是GNU的一个编译器
- gcc与g++都可以编译c和c++代码
  - 不同之处在于后缀为.c的文件，gcc将其当作c程序，g++将其当作时c++程序
  - 后缀为cpp的文件，两者都会认为是c++程序
- 在编译阶段，g++会调用gcc，所以==对于c++代码，两者是等价的==
- 在使用gcc命令生成可执行程序时，在链接阶段不会自动调用c++的库进行链接
  - 所以一般使用g++来对c++源程序生成可执行程序
  - 也可以通过gcc -lstdc++来手动说明C++库进行链接，此时与g++作用一致





## g++应用

 g++ 是将 gcc 默认语言设为 C++ 的一个特殊的版本

链接时它自动使用 C++ 标准库而不用 C 标准库。通过遵循源码的命名规范并指定对应库的名字，用 gcc 来编译链接 C++ 程序是可行的

```shell
$ gcc main.cpp -lstdc++ -o main
```



```c++
// helloworld.cpp
#include <iostream>
using namespace std;
int main()
{
    cout << "Hello, world!" << endl;
    return 0;
}
```

```sh
$ g++ helloworld.cpp

# 由于命令行中未指定可执行程序的文件名，编译器采用默认的 a.out。程序可以这样来运行：
$ ./a.out
Hello, world!


# 使用 -o 选项指定可执行程序的文件名
$ g++ helloworld.cpp -o helloworld
$ ./helloworld
Hello, world!


# 多个 C++ 代码文件
$ g++ runoob1.cpp runoob2.cpp -o runoob


# g++ 有些系统默认是使用 C++98，我们可以指定使用 C++11 来编译 main.cpp 文件
g++ -g -Wall -std=c++11 main.cpp
```



## g++常用命令选项

| 选项         | 解释                                                         |
| :----------- | :----------------------------------------------------------- |
| -ansi        | 只支持 ANSI 标准的 C 语法。这一选项将禁止 GNU C 的某些特色， 例如 asm 或 typeof 关键词。 |
| ==-c==       | 只编译并生成目标文件。                                       |
| -DMACRO      | 以字符串"1"定义 MACRO 宏。                                   |
| -DMACRO=DEFN | 以字符串"DEFN"定义 MACRO 宏。                                |
| -E           | 只运行 C 预编译器。                                          |
| -g           | 生成调试信息。GNU 调试器可利用该信息。                       |
| -IDIRECTORY  | 指定额外的头文件搜索路径DIRECTORY。                          |
| -LDIRECTORY  | 指定额外的函数库搜索路径DIRECTORY。                          |
| -lLIBRARY    | 连接时搜索指定的函数库LIBRARY。                              |
| -m486        | 针对 486 进行代码优化。                                      |
| ==-o==       | FILE 生成指定的输出文件。用在生成可执行文件时。              |
| -O0          | 不进行优化处理。                                             |
| -O           | 或 -O1 优化生成代码。                                        |
| -O2          | 进一步优化。                                                 |
| -O3          | 比 -O2 更进一步优化，包括 inline 函数。                      |
| -shared      | 生成共享目标文件。通常用在建立共享库时。                     |
| -static      | 禁止使用共享连接。                                           |
| -UMACRO      | 取消对 MACRO 宏的定义。                                      |
| -w           | 不生成任何警告信息。                                         |
| -Wall        | 生成所有警告信息。                                           |



### gcc常用参数选项

- gcc -E (指定源文件)

预处理指定的源文件，不进行编译

- gcc -S (指定源文件)

编译指定的源文件(当然也会先进行预处理)，但是不进行汇编

- gcc -c (指定源文件)

汇编指定的源文件(当然也会先进行预处理和编译)，但是不进行链接

- gcc (指定源文件) -o (生成的文件名称)

由指定源文件生成可执行文件

- gcc -I directory

指定include包含文件的搜索目录directory

- gcc -g

在编译的时候，生成调试信息

- gcc -D 宏内容

在程序编译的时候，定义一个宏(常用来和#ifdef #endif中的调试代码结合使用)

- gcc -w

不生成任何警告信息

- gcc -Wall

生成所有警告信息

- gcc -On

n取0，1，2，3。选择编译器的优化级别，默认为O1

- gcc -l(库名称)

在程序编译时，指定使用的库

- gcc -L (路径)

指定编译的时候，搜索的库的路径

- gcc -fPIC/fpic

生成位置无关的代码，用来生成共享目标文件

- gcc -shared

生成共享目标文件 ，用来建立共享库

- gcc -std=(标准名)

指定c标准，默认为GNU C





## 常见问题

1 调试时界面一闪而过解决办法：

一闪而过是因为你的程序没有输入，只有固定的输出。程序会在运行到 return 语句时退出程序。



这种采用了输入方法来不让程序终止，他会在读入到数据后退出程序（cin.get）

```c++
cin.clear();  // 清空缓存
cin.sync();   // 清空缓存
cin.get();    // 接收键盘输入
```



这种是采用了输入方法，但不同于上一种的是，这次是使用 getchar 函数获取一个 char 类型，但不将读入的数据存放于任何变量

```c++
#include <stdio.h>
int main()
{
  getchar();
  return 0;
}
```



采用 system() 函数中的 pause 命令进行程序的暂停

```c++
#include <stdlib.h>
int main()
{
  system("pause"); //注意：“system("pause")”;语句会显示“请按任意键继续……”
  return 0;
}
```





# 补充|GNU计划：GCC等 + Linux内核

Unix 系统被发明之后，大家用的很爽。但是后来==开始收费和商业闭源==了。

一个叫 RMS 的大叔觉得很不爽，于是发起 <u>GNU 计划，模仿 Unix 的界面和使用方式，从头做一个开源的版本</u>。



然后他自己做了编辑器 Emacs 和编译器 GCC。

接下来大家纷纷在 GNU 计划下做了很多的工作和项目，基本实现了当初的计划。包括核心的 gcc 和 glibc。

但是 GNU 系统缺少操作系统内核。原定的内核叫 HURD，一直完不成。

同时 BSD（一种 UNIX 发行版）陷入版权纠纷，x86 平台开发暂停。



然后一个叫 Linus 的同学为了在 PC 上运行 Unix，在 Minix 的启发下，开发了 Linux。

注意，==Linux 只是一个系统内核==，系统启动之后使用的仍然是 gcc 和 bash 等软件。Linus 在发布 Linux 的时候选择了 GPL，因此符合 GNU 的宗旨。



最后，大家突然发现，这玩意不正好是 GNU 计划缺的么。于是==合在一起打包发布叫 GNU / Linux==。

然后大家念着念着省掉了前面部分，变成了 Linux 系统。



实际上 Debian，RedHat 等 Linux 发行版中内核只占了很小一部分容量。



## GNU计划

[link: CSDN](https://blog.csdn.net/Shangxingya/article/details/106197053)

**Linux操作系统是GNU计划的结晶，而且到目前依旧是GNU提供大多软件、Linux提供内核**

- **1969**年研发出了Unix操作系统，是一个主要支持多用户操作系统、多任务、支持多种处理器架构 shell、==大部分使用C语言开发==。
- 在**1983**年提出GNU计划 支持自由软件运动（自由获取、自由改变、自由分发、自由使用）。它的目的就是为了创建一个==完全自由的操作系统==。
- 并且产生两种认证软件格式GPL与LGPL
  - GPL：**通用公共许可，支持自由使用，提供源代码、可以自由修改，但必须保证修改后也是自由软件**
  - LGPL：**以库的形式调用，不容许修改现有程序，研发的新软件也可以闭源，可以商业化**



GNU计划为Linux做的最终稳定铺垫便是GCC（功能非常强大）（将程序源码编译为需要的二进制文件）

最终在1991年 Linus 发布了 Linux内核。



GNU 计划自己的内核 Hurd 依然在开发中，但直到 2013 年为止，都还没有稳定版本发布

GNU 工程十几年以来已经成为一个对软件开发主要的影响力量，创造了无数的重要的工具，例如：强健的编译器，有力的文本编辑器，甚至一个全功能的操作系统。

这个工程是从 1984 年麻省理工学院的程序员理查德·斯托曼的想法得来的，他想要创建一个自由的、和 UNIX 类似的操作环境。从那时开始，许多程序员聚集起来开始开发一个自由的、高质量、易理解的软件。



GNU 计划采用了部分当时已经可自由使用的软件，例如 TeX 排版系统和 X Window 视窗系统等。不过 GNU 计划也开发了大批其他的自由软件

这些软件也被移植到其他操作系统平台上，例如 Microsoft Windows、BSD 家族、Solaris 及 Mac OS。
许多 UNIX 系统上也安装了 GNU 软件，因为 GNU 软件的质量比之前 UNIX 的软件还要好。

所以，GNU 计划中的许多软件目前在所有的操作系统中都应用广泛(Unix，mac，linux，windows，bsd…)，最出名的就是 GCC 了。







### POSIX

**可移植操作系统接口，定义了操作系统要为应用程序提供的接口标准**

- 而且必须使用 API 规范 （应用程序接口规范）
- 使用 ABI （应用程序二进制接口）



IEEE（电气与电子工程师协会）定义的POSIX：GNU计划产生的软件可以与Linux操作系统完美对接

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210303095745.png" alt="image-20210303095744937" style="zoom: 50%;" />







# 补充|编译过程

## 编译过程

主要通过我们的编译器做了以下任务：**扫描、语法分析、语义分析、源代码优化、代码生成和目标代码优化**



`.i` 文件、`.s` 文件、`.o` 文件可以认为是中间文件或临时文件，如果使用 GCC 一次性完成 C 语言程序的编译，那么只能看到最终的可执行文件，这些中间文件都是看不到的，因为 GCC 已经经它们删除了。

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210303102040.png" alt="image-20210303102039982" style="zoom:67%;" />

### 1 预编译过程：

```shell
$ gcc -E hello.c -o hello.i

# 另一种方式
$ cpp hello.c > hello.i
```



1. 将所有的 `#define` 删除，并展开所有的宏定义
2. 处理所有条件预编译指令，比如 `#if`，`#ifdef`，`#elif`，`#else`，`#endif`
3. 处理 `#include` 预编译指令，将被包含的文件插入到该预编译指令的位置。
4. 删除所有的注释 `//` 和 `/**/`
5. 添加行号和文件名标识，比如 `# 1 "hello.c" 2`。
6. 保留所有的 `#pragma` 编译器指令



### 2 编译 => 汇编代码

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210303102642.png" alt="image-20210303102642572" style="zoom:67%;" />

```shell
gcc -S hello.i -o hello.s
```

**词法分析**、**语法分析**、**语义分析及优化后**生产==相应的**汇编代码文件**==，此过程是整个程序构建的核心部分，也是最复杂的部分之一





### 3 汇编 => 机器码

```sh
as hello.s -o hello.o

# 使用 gcc 命令从 C 源代码文件开始
gcc -c hello.s -o hello.o
```

将汇编代码转变成机器可以执行的指令，每一个汇编语句几乎对应一条机器令

只是根据汇编指令和机器指令的对照表一一翻译就可以了





### 4 链接

```shell
# 类似这样的鬼东西
ld -o hello /lib/crt0.o hello.o -lc

# 使用 gcc 的方式
gcc -o hello hello.c
```









## Apple 的编译器

因为 GCC 的编译器已经慢慢无法满足苹果的需求，因此，苹果开发了 Clang 与 LLVM 来完全取代 GCC，Xcode4 之后，苹果的默认编译器已经是 LLVM 了。

- Clang 作为编译器前端
- LLVM 作为编译器后端



LLVM 采用三相设计：

- 前端 Clang 负责解析，验证和诊断输入代码中的错误
- 然后将解析的代码转换为 LLVM IR
- 后端 LLVM 编译把 IR 通过一系列改进代码的分析和优化过程提供，然后被发送到代码生成器以生成本机机器代码。

![image-20210303102810908](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210303102811.png)



编译器前端的任务是进行：

- 语法分析
- 语义分析
- 生成中间代码(intermediate representation )

在这个过程中，会==进行类型检查==，如果发现错误或者警告会标注出来在哪一行。





## 小结|所有的步骤

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210303102901.png" alt="image-20210303102901388" style="zoom:50%;" />





# 初步探索|Udacity

## 前提条件：

1. 安装C++环境
2. 设置PATH变量

```shell
### 在终端中查看
g++ -help

#我的windows系统
G++ --help
```



## 终端操作|目标文件的目录

注意|Unix环境 目录不该包含空格：**Unix folder and file names work best if there are no spaces in the name.**



注意：检测工作环境(所有的目标文件都在==当前目录==)

```shell
Print Working Directory: pwd
Change Directory: cd
List Directory: ls
```





## 终端|编译 + 执行

假设目标文件是：assignment2.cpp

- 编译命令：g++ is the command to compile
- 待编译的目标文件：assignment2.cpp is the file to be compiled
- 设置参数：给执行文件命名 -o is the command that says you want name of the executable file
- 编译后的执行文件：a2 is the name I want to give the executable file.

```shell
g++ filename.cpp -o nameOfExecutableCode


# For example:
g++ assignment2.cpp -o a2
```



```shell
#执行目标文件(可执行文件)
./a2
```







# #参考文献

[link: GNU & GCC & CLANG & LLVM](https://rcyw.github.io/2020/01/08/compiler/gcc-clang-gun-llvm/)



