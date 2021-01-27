# 简介|STL哲学-Wiki

[Link: Wiki](https://zh.wikipedia.org/wiki/%E6%A0%87%E5%87%86%E5%BA%93)

## 要素

- [子程序](https://zh.wikipedia.org/wiki/子程序)
- [宏](https://zh.wikipedia.org/wiki/巨集)定义
- [全局变量](https://zh.wikipedia.org/wiki/全局变量)
- [类别](https://zh.wikipedia.org/wiki/类_(计算机科学))定义
- [模板](https://zh.wikipedia.org/wiki/模板)

大多数标准库都至少含有如下常用组件的定义：

- [算法](https://zh.wikipedia.org/wiki/算法)（例如[排序算法](https://zh.wikipedia.org/wiki/排序算法)）
- [数据结构](https://zh.wikipedia.org/wiki/数据结构)（例如[ 表](https://zh.wikipedia.org/w/index.php?title=表_(数据结构)&action=edit&redlink=1)、[ 树](https://zh.wikipedia.org/wiki/树_(数据结构))、[哈希表](https://zh.wikipedia.org/wiki/哈希表)）
- 与宿主平台的交互，包括[输入输出](https://zh.wikipedia.org/wiki/输入输出)和操作系统调用



## 哲学

标准库设计的哲学多种多样

### C++|[Bjarne Stroustrup](https://zh.wikipedia.org/wiki/比雅尼·斯特劳斯特鲁普) 

> C++标准库应该是什么？程序员的一个理想是在库中找到所有有趣、重要、适度通用的类、函数、模板等等。然而，这里我们问的不是“某个库里应该有什么？”，而是“标准库里应该有什么”
>
> 回答“所有！”对前者来说是一个合理的答案，而对后者不然。标准库是每一个实现者都必须提供的东西，==以便让每一个程序员能够依赖于它==。

=> 相对较小的标准库，只包含“每一个程序员”在构建多种软件时都实际可能需要的要素



### Python|Guido van Rossum

更倾向于包容

>Python 有“已含电池”的哲学，这从它的庞大软件包复杂而又可靠的能力中就可以看出端倪

列举了处理 [XML](https://zh.wikipedia.org/wiki/XML)、[XML-RPC](https://zh.wikipedia.org/wiki/XML-RPC)、电子邮件信息、和本地化的库，这些都是被 C++ 标准库所忽略的。

这种哲学经常可以在[脚本语言](https://zh.wikipedia.org/wiki/脚本语言)（如 [Python](https://zh.wikipedia.org/wiki/Python) 和 [Ruby](https://zh.wikipedia.org/wiki/Ruby)）和使用[虚拟机](https://zh.wikipedia.org/wiki/虚拟机)的语言（如 [Java](https://zh.wikipedia.org/wiki/Java) 和 [.NET框架](https://zh.wikipedia.org/wiki/.NET框架) 语言）中找到





---

C++标准库，包括了STL容器，算法和函数等 => 可以分为两部分：标准函数库，面向对象类库

- [C++ Standard Library](http://en.wikipedia.org/wiki/C%2B%2B_Standard_Library)：是一系列类和函数的集合，使用核心语言编写，也是C++ ISO自身标准的一部分。
- [Standard Template Library](http://en.wikipedia.org/wiki/Standard_Template_Library)：标准模板库
- [C POSIX library](http://en.wikipedia.org/wiki/C_POSIX_library)： POSIX系统的C标准库规范
- [ISO C++ Standards Committee](https://github.com/cplusplus)：C++标准委员会





C++ STL 是一套功能强大的 C++ 模板类，提供了通用的模板类和函数，这些模板类和函数可以实现多种流行和常用的算法和数据结构，如==向量、链表、队列、栈==。

核心包括以下三个组件：

| 组件                | 描述                                                         |
| :------------------ | :----------------------------------------------------------- |
| 容器（Containers）  | 容器是用来管理某一类对象的集合。C++ 提供了各种不同类型的容器，比如 deque、list、vector、map 等。 |
| 算法（Algorithms）  | 算法作用于容器。它们提供了执行各种操作的方式，包括对容器内容执行初始化、排序、搜索和转换等操作。 |
| 迭代器（iterators） | 迭代器用于遍历对象集合的元素。这些集合可能是容器，也可能是容器的子集。 |

三个组件都带有丰富的预定义函数，帮助我们通过简单的方式处理复杂的任务

```c++
// 示例|<vector>容器
/*
.push_back( ) 成员函数在向量的末尾插入值，如果有必要会扩展向量的大小
.size( ) 函数显示向量的大小。
.begin( ) 函数返回一个指向向量开头的迭代器。
.end( ) 函数返回一个指向向量末尾的迭代器。

*/
#include <iostream>
#include <vector>
using namespace std;
 
int main()
{
   // 创建一个向量存储 int
   vector<int> vec; 
   int i;
 
   // 显示 vec 的原始大小
   cout << "vector size = " << vec.size() << endl;
 
   // 推入 5 个值到向量中
   for(i = 0; i < 5; i++){
      vec.push_back(i);
   }
 
   // 显示 vec 扩展后的大小
   cout << "extended vector size = " << vec.size() << endl;
 
   // 访问向量中的 5 个值
   for(i = 0; i < 5; i++){
      cout << "value of vec [" << i << "] = " << vec[i] << endl;
   }
 
   // 使用迭代器 iterator 访问值
   vector<int>::iterator v = vec.begin();
   while( v != vec.end()) {
      cout << "value of v = " << *v << endl;
      v++;
   }
 
   return 0;
}
```



# 1标准函数库

这个库是由通用的、独立的、不属于任何类的函数组成的。函数库继承自 C 语言

C++ 标准库包含了所有的 C 标准库，为了支持类型安全，做了一定的添加和修改

- 输入/输出 I/O
- 字符串和字符处理
- 数学
- 时间、日期和本地化
- 动态分配
- 其他
- 宽字符函数





# 2面向对象类库

标准的 C++ 面向对象类库定义了大量支持一些常见操作的类，比如输入/输出 I/O、字符串处理、数值处理。

- 标准的 C++ I/O 类
- String 类
- 数值类
- STL 容器类
- STL 算法
- STL 函数对象
- STL 迭代器
- STL 分配器
- 本地化库
- 异常处理类
- 杂项支持库



| 类别                                                         | 标头(标准头文件)                                             |
| :----------------------------------------------------------- | ------------------------------------------------------------ |
| [算法](https://docs.microsoft.com/zh-cn/cpp/standard-library/algorithms?view=msvc-160) | ==<algorithm>== 针对 STL 容器的算法<br/><cstdlib><br/><numeric> |
| 原子操作                                                     | <atomic>                                                     |
| ==C 库包装==                                                 | <cassert><br/><ccomplex><br/><cctype><br/><cerrno><br/><cfenv><br/><cfloat><br/><cinttypes><br/><ciso646><br/><climits><br/><clocale><br/><cmath><br/><csetjmp><br/><csignal><br/><cstdalign><br/><cstdarg><br/><cstdbool><br/><cstddef><br/>==<cstdint>==<br/>==<cstdio>==<br/>==<cstdlib>==<br/>==<cstring>==<br/><ctgmath><br/><ctime><br/><cuchar><br/><cwchar><br/><cwctype> |
| 概念                                                         | <concepts>                                                   |
| [==容器 Containers==](https://docs.microsoft.com/zh-cn/cpp/standard-library/stl-containers?view=msvc-160) |                                                              |
| 序列容器                                                     | <array> 固定大小数组<br/><deque> 双向队列<br/><forward_list> 单向链表<br/><list> 双向链表<br/><vector> 动态数组 |
| 排序关联容器                                                 | <map> 有序的一组偶对<br/><multimap>在map的基础上，允许键重复<br><set><br/><multiset> |
| 无序关联容器                                                 | <unordered_map><br/><unordered_set><br><unordered_multiset>  |
| 容器适配器                                                   | <queue> 单向队列<br/><priority_queue> 优先队列<br><stack> 基于deque包装后的容器类：栈 |
| 容器视图                                                     | <span>                                                       |
| [错误和异常处理](https://docs.microsoft.com/zh-cn/cpp/cpp/errors-and-exception-handling-modern-cpp?view=msvc-160) | <cassert><br/><exception> 提供了一些与异常处理有关的类型和函数<br/><stdexcept> 有一些表示异常的类，例如 std::exception，std::logic_error，std::runtime_error<br/><system_error> |
| 常规实用工具                                                 | <any><br/><bit><br/><bitset> 专门用来存放位组(一系列 bit)<br/><cstdlib><br/><execution><br/><functional> 提供了一些函数对象（如仿函数）<br/><memory><br/><memory_resource><br/><optional><br/><ratio><br/><scoped_allocator><br/><tuple> 提供了元组<br/><type_traits><br/><typeindex><br/><utility> 提供了类模板偶对 std::pair  所谓偶对，就是放在一起的、有序的两个东西 (a,b)。提供了 namespacestd::rel_ops，用来更简便地使用运算符重载<br/><variant> |
| [I/o 和格式设置](https://docs.microsoft.com/zh-cn/cpp/text/string-and-i-o-formatting-modern-cpp?view=msvc-160) | <cinttypes><br/><cstdio><br/>==<filesystem>==<br/><fstream> 提供了对文件的输入输出设施<br/><iomanip> 提供了控制输出格式的功能。例如，基于特定的进制数格式化整数，或者控制浮点数的精度<br/><ios> 提供了 iostream 需要的一些类型和函数<br/><iosfwd> 提供了一些与 I/O 有关的转发操作<br/>==<iostream>== 提供了 C++ 输入与输出的基础层<br/><istream> 提供了类模板 std::istream 和其他与输入相关的辅助类<br/><ostream> 提供了类模板 std::ostream 和其他与输出相关的辅助类<br/><sstream> 提供了类模板 std::stringstream 和其他用来处理字符串流的类<br/><streambuf> 提供了对字符序列（文件或字符串）进行读写的基础设施<br/><strstream><br/><syncstream> |
| 迭代器                                                       | ==<iterator>==                                               |
| 语言支持                                                     | <cfloat><br/><climits><br/><codecvt><br/><compare><br/><contract><br/><coroutine><br/><csetjmp><br/><csignal><br/><cstdarg><br/><cstddef><br/><cstdint><br/><cstdlib><br/><exception><br/><initializer_list><br/><limits><br/><new> 与 new 和 delete 运算符有关的一些类型和函数<br/><typeinfo> C++ 运行时的类型信息的一些工具<br/><version> |
| 本地化                                                       | <clocale> 以类和函数的形式封装了与区域有关的操作<br/><codecvt> 提供了不同代码页（code page）之间字符编码的转换功能<br/><cvt/wbuffer> <br><cvt/wstring><br/><locale> |
| 数学和数字                                                   | <bit><br/><cfenv><br/><cmath><br/><complex> 复数相关<br/><cstdlib><br/><limits> 提供了类模板 std::numeric_limits，用来描述基本数字类型的最值<br/><numeric> 一些通用的数学运算<br/><random> （伪）随机数相关<br/><ratio><br/><valarray> 定义了五个类模板 valarray，slice_array，gslice_array，mask_array，indirect_array，两个类slice，gslice，还有一系列相关的函数模板 |
| [内存管理](https://docs.microsoft.com/zh-cn/cpp/cpp/smart-pointers-modern-cpp?view=msvc-160) | ==<allocators>==<br/><memory> 提供了 C++ 内存管理的工具，例如 std::unique_ptr，std::shared_ptr，std::weak_ptr<br/><memory_resource><br/><new><br/><scoped_allocator> |
| 多线程处理                                                   | <atomic><br/><condition_variable> 条件变量。一个条件变量代表一个条件，当一个条件变量等待时，它会阻塞当前线程，直到它代表的条件为真<br/><future> 可以用它来进行便捷的异步操作，以免代码中出现一大堆回调函数<br/><mutex> 里面有互斥对象（mutex），锁（lock），还有 once（一种用来保证某函数在某进程中只执行一次的对象）<br/><shared_mutex><br/><thread> 提供了使用线程所需要的类和 namespace |
| 范围                                                         | <ranges>                                                     |
| ==正则表达式==                                               | <regex>                                                      |
| 字符串和字符数据                                             | <charconv><br/><cctype><br/><cstdlib><br/><cstring><br/><cuchar><br/><cwchar><br/><cwctype><br/>==<regex>== 提供了用正则表达式进行字符串匹配的功能<br/>==<string>== 提供了 C++ 标准的字符串类和字符串模板<br/><string_view> |
| 时间                                                         | <chrono> 提供了诸如 std::chrono::duration 和 std::chrono::time_point 等的时间元素，还有时钟<br/>==<ctime>== |





# 标准库使用概述

- 可按任何顺序包括标准标头，可多次包括一个标准标头，或包括用于定义相同宏或相同类型的两个或多个标准标头
- 声明内不能包括标准标头
- 包含标准标头之前，不能定义具有与关键字相同名称的宏

```c++
#include <iostream>// include I/O facilities
using namespace std;  //会将所有库名称引入当前命名空间
```

如果 C++ 库标头包括任何其他 C++ 库标头，则需要定义所需的类型

标准 C 标头决不包含其他标准标头。

标准标头仅==声明或定义==此文档中描述的相关实体。

- 库中的每个函数都在标准标头中声明



C + + 库标头中除运算符 delete 和 运算符 new 以外的所有名称，均在 std 命名空间或命名空间中嵌套的命名空间中定义 std 

- 例如cin => std::cin

注意，该宏名称不受命名空间限定，因此你始终需要写入不带命名空间限定符的 __STD_COMPLEX

在某些转换环境中（包括 c + + 库标头），可以将命名空间中声明的外部名称提升 std 到全局命名空间，其中 using 每个名称都有单独的声明。 否则，标头不能将任何库名称引入当前命名空间中。



## C++库 约定

C++ 库中声明类型和函数时存在某些限制：[Link: 微软官网](https://docs.microsoft.com/zh-cn/cpp/standard-library/cpp-library-conventions?view=msvc-160)





## C++ 程序启动和终止

在目标环境调用函数 main 之前，以及将任何指定的常量初始值存储在具有静态对象中后，程序将为这些静态对象执行任何剩余的构造函数

控制文本流：

- [cin](https://docs.microsoft.com/zh-cn/cpp/standard-library/iostream?view=msvc-160#cin) — 针对标准输入。
- [cout](https://docs.microsoft.com/zh-cn/cpp/standard-library/iostream?view=msvc-160#cout) — 针对标准输出。
- [cerr](https://docs.microsoft.com/zh-cn/cpp/standard-library/iostream?view=msvc-160#cerr) — 针对未缓冲的标准错误输出。
- [clog](https://docs.microsoft.com/zh-cn/cpp/standard-library/iostream?view=msvc-160#clog) — 针对已缓冲的标准错误输出。

此外，还可以在程序终止期间，在为静态对象调用的析构函数中使用这些对象

与 C 程序一样，从 main 返回或调用 exit 时，它会按相反顺序的注册表调用在 atexit 注册的所有函数。 从已注册函数引发的异常会调用 terminate



## 安全库

已将 C++ 标准库中的多个方法确定为具有潜在的不安全性，因为它们可能导致缓冲区溢出或其他代码缺陷

建议不要使用这些方法，已创建了更安全的新方法来替代这些方法。 

- 这些新方法均以 _s结尾

还实施了多项功能增强以提升迭代器和算法的安全性

- [Checked Iterators](https://docs.microsoft.com/zh-cn/cpp/standard-library/checked-iterators?view=msvc-160)
- [Debug Iterator Support](https://docs.microsoft.com/zh-cn/cpp/standard-library/debug-iterator-support?view=msvc-160)
- [_ITERATOR_DEBUG_LEVEL](https://docs.microsoft.com/zh-cn/cpp/standard-library/iterator-debug-level?view=msvc-160)：宏控制是否启用[检查迭代](https://docs.microsoft.com/zh-cn/cpp/standard-library/checked-iterators?view=msvc-160)[器和调试迭代器支持](https://docs.microsoft.com/zh-cn/cpp/standard-library/debug-iterator-support?view=msvc-160)

| 编译模式 | ==_ITERATOR_DEBUG_LEVEL宏==的可能值 | 描述                                     |
| :------- | :---------------------------------- | :--------------------------------------- |
| **调试** |                                     |                                          |
|          | 0                                   | 禁用经过检查的迭代器，并禁用迭代器调试。 |
|          | 1                                   | 启用经过检查的迭代器，禁用迭代器调试。   |
|          | 2（默认值）                         | 启用迭代器调试；经过检查的迭代器不相关。 |
| **版本** |                                     |                                          |
|          | 0（默认值）                         | 禁用经过检查的迭代器。                   |
|          | 1                                   | 启用经过检查的迭代器；迭代器调试不相关。 |





下表列出了可能不安全的 C++ 标准库方法，以及更安全的等效方法：

| 可能不安全的方法                                             | 更安全的等效方法                                             |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [copy](https://docs.microsoft.com/zh-cn/cpp/standard-library/basic-string-class?view=msvc-160#copy) | [basic_string::_Copy_s](https://docs.microsoft.com/zh-cn/cpp/standard-library/basic-string-class?view=msvc-160#copy_s) |
| [copy](https://docs.microsoft.com/zh-cn/cpp/standard-library/char-traits-struct?view=msvc-160#copy) | [char_traits::_Copy_s](https://docs.microsoft.com/zh-cn/cpp/standard-library/char-traits-struct?view=msvc-160#copy_s) |





## 线程安全

以下线程安全规则应用到标准 C++ 库中的所有类，这也包括 shared_ptr

从多个线程读取某个对象时，该对象是线程安全的。 

- 例如，给定对象 A，可安全地同时从线程 1 和线程 2 读取 A。



如果要通过某个线程写入到对象，则必须保护相同线程或其他线程上所有对该对象的读取和写入。 

- 例如，给定对象 A，如果线程 1 将写入到 A，则必须阻止线程 2 读取或写入 A。



即使另一个线程正在读取或写入同一类型的其他实例，也可以安全地读取和写入该类型的某个实例。 

- 例如，给定同一类型的对象 A 和 B，在线程 1 中写入 A 的同时可以安全地在线程 2 中读取 B



### shared_ptr

即使对象是共享所有权的副本，多个线程也可以同时读取和写入不同的 [shared_ptr](https://docs.microsoft.com/zh-cn/cpp/standard-library/shared-ptr-class?view=msvc-160) 对象





### iostream

标准 iostream 对象 cin、cout、cerr、clog、wcin、wcout、wcerr 和 wclog 遵循与其他类相同的规则，但存在此例外：可以安全地从多个线程写入一个对象

- 例如，可以将线程 1 和线程 2 同时写入 [cout](https://docs.microsoft.com/zh-cn/cpp/standard-library/iostream?view=msvc-160#cout)。 但是，此操作可能会导致两个线程的输出相混合。



注意：不会将读取流缓冲区视为读取操作。 反而会将它视为写入操作，因为类的状态发生了更改。





## stdext 命名空间

<hash_map>和 <hash_set> 标头文件的成员当前不是 ISO c + + 标准的一部分。 因此，这些类型和成员已从 `std` 命名空间移动到命名空间 `stdext`以保持符合 C++ 标准。



若要让编译器生成错误以将 std <hash_map> 和 <hash_set> 标头文件的成员用于 /ze，请在 #include 任何 c + + 标准库标头文件之前添加以下指令。

```c++
#define _DEFINE_DEPRECATED_HASH_CLASSES 0
```





# 概念|函数对象

函数对象（也称 函子）是实现 operator() 的任何类型

此运算符被称为 调用运算符 （有时称为 应用程序运算符）

C++ 标准库主要使用函数对象作为容器和算法内的排序条件。



相对于直接函数调用，函数对象有两个优势：

- 函数对象可包含状态
- 函数对象是一个类型，因此可用作模板参数



## 创建函数对象

要创建函数对象，请创建一个类型并实现 operator()

```c++
class Functor
{
public:
    int operator()(int a, int b)
    {
        return a < b;
    }
};

int main()
{
    Functor f;
    int a = 5;
    int b = 7;
    int ans = f(a, b);  //函数对象的调用方式  => 实际上调用的是函子类型
}
```





## 函数对象和容器

C + + 标准库在标头文件中包含若干函数对象 <functional> 

这些函数对象的一个用途是用作容器的排序条件

```c++
//set 容器声明：
template <class Key,
    class Traits=less<Key>,
    class Allocator=allocator<Key>>
class set
```

第二个模板函数是函数对象 less





## 函数对象和算法

```c++
template <class ForwardIterator, class Predicate>
ForwardIterator remove_if(
    ForwardIterator first,
    ForwardIterator last,
    Predicate pred);
```

remove_if 的最后一个参数是返回布尔值（一个 谓词）的函数对象





# 补充|注意：屏蔽宏？？

# #参考文献

[Link: 思否|常用库](https://segmentfault.com/a/1190000011483340)

[Link: C++标准库 列表大全](https://en.cppreference.com/w/cpp/header)⭐

[Link: 微软|C++标准库头文件](https://docs.microsoft.com/zh-cn/cpp/standard-library/algorithm?view=msvc-160)