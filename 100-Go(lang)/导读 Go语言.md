go programming language: 搜索引擎区别go，协程golang



[link: Go语言完整教程](https://books.studygolang.com/gopl-zh/)

在上个世纪70年代，贝尔实验室的[Ken Thompson](http://genius.cat-v.org/ken-thompson/)和[Dennis M. Ritchie](http://genius.cat-v.org/dennis-ritchie/)合作发明了[UNIX](http://doc.cat-v.org/unix/)操作系统，同时[Dennis M. Ritchie](http://genius.cat-v.org/dennis-ritchie/)为了解决[UNIX](http://doc.cat-v.org/unix/)系统的移植性问题而发明了C语言，==贝尔实验室的[UNIX](http://doc.cat-v.org/unix/)和C语言两大发明奠定了整个现代IT行业最重要的软件基础==（目前的三大桌面操作系统的中[Linux](http://www.linux.org/)和[Mac OS X](http://www.apple.com/cn/osx/)都是源于[UNIX](https://books.studygolang.com/gopl-zh/)系统，两大移动平台的操作系统iOS和Android也都是源于[UNIX](http://doc.cat-v.org/unix/)系统。C系家族的编程语言占据统治地位达几十年之久）。

在[UNIX](https://books.studygolang.com/gopl-zh/)和C语言发明40年之后，目前已经在Google工作的[Ken Thompson](http://genius.cat-v.org/ken-thompson/)和[Rob Pike](http://genius.cat-v.org/rob-pike/)（他们在贝尔实验室时就是同事）、还有[Robert Griesemer](http://research.google.com/pubs/author96.html)（设计了V8引擎和HotSpot虚拟机）一起合作，为了解决在21世纪==多核和网络化环境下==越来越复杂的编程问题而发明了Go语言。

从Go语言库早期代码库日志可以看出它的演化历程（Git用`git log --before={2008-03-03} --reverse`命令查看）：

![img](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210325102358.png)

从早期提交日志中也可以看出，Go语言是从[Ken Thompson](http://genius.cat-v.org/ken-thompson/)发明的B语言、[Dennis M. Ritchie](http://genius.cat-v.org/dennis-ritchie/)发明的C语言逐步演化过来的，是C语言家族的成员，因此很多人将Go语言称为21世纪的C语言。

纵观这几年来的发展趋势，Go语言已经成为==云计算、云存储时代==最重要的基础编程语言。



在C语言发明之后约5年的时间之后（1978年），[Brian W. Kernighan](http://www.cs.princeton.edu/~bwk/)和[Dennis M. Ritchie](http://genius.cat-v.org/dennis-ritchie/)合作编写出版了C语言方面的经典教材《[The C Programming Language](http://s3-us-west-2.amazonaws.com/belllabs-microsite-dritchie/cbook/index.html)》，该书被誉为C语言程序员的圣经，作者也被大家亲切地称为[K&R](https://en.wikipedia.org/wiki/K%26R)。

同样在Go语言正式发布（2009年）约5年之后（2014年开始写作，2015年出版），由Go语言核心团队成员[Alan A. A. Donovan](https://github.com/adonovan)和[K&R](https://en.wikipedia.org/wiki/K%26R)中的[Brian W. Kernighan](http://www.cs.princeton.edu/~bwk/)合作编写了Go语言方面的经典教材《[The Go Programming Language](http://gopl.io/)》。

Go语言被誉为21世纪的C语言，如果说[K&R](https://en.wikipedia.org/wiki/K%26R)所著的是圣经的旧约，那么D&K所著的必将成为圣经的新约。

==该书介绍了Go语言几乎全部特性，并且随着语言的深入层层递进，对每个细节都解读得非常细致，每一节内容都精彩不容错过，是广大Gopher的必读书目。==大部分Go语言核心团队的成员都参与了该书校对工作，因此该书的质量是可以完全放心的。

同时，单凭阅读和学习其语法结构并不能真正地掌握一门编程语言，必须进行足够多的编程实践——亲自编写一些程序并研究学习别人写的程序。

要从利用Go语言良好的特性使得程序模块化，充分利用Go的标准函数库以Go语言自己的风格来编写程序。

书中包含了上百个精心挑选的习题，希望大家能先用自己的方式尝试完成习题，然后再参考官方给出的解决方案。



该书英文版约从2015年10月开始公开发售，其中日文版本最早参与翻译和审校（参考致谢部分）。

在2015年10月，我们并不知道中文版是否会及时引进、将由哪家出版社引进、引进将由何人来翻译、何时能出版，这些信息都成了一个秘密。

中国的Go语言社区是全球最大的Go语言社区，我们从一开始就始终紧跟着Go语言的发展脚步。

我们应该也完全有能力以中国Go语言社区的力量同步完成Go语言圣经中文版的翻译工作。与此同时，国内有很多Go语言爱好者也在积极关注该书（本人也在第一时间购买了纸质版本，[亚马逊价格314人民币](http://www.amazon.cn/The-Go-Programming-Language-Donovan-Alan-A-A/dp/0134190440/)。补充：国内也即将出版英文版，[价格79元](http://product.china-pub.com/4912464)）。为了Go语言的学习和交流，大家决定合作免费翻译该书。



翻译工作从2015年11月20日前后开始，到2016年1月底初步完成，前后历时约2个月时间（在其它语言版本中，全球第一个完成翻译的，基本做到和原版同步）。其中，[chai2010](https://github.com/chai2010)翻译了前言、第2 ~ 4章、第10 ~ 13章，[Xargin](https://github.com/cch123)翻译了第1章、第6章、第8 ~ 9章，[CrazySssst](https://github.com/CrazySssst)翻译了第5章，[foreversmart](https://github.com/foreversmart)翻译了第7章，大家共同参与了基本的校验工作，还有其他一些朋友提供了积极的反馈建议。如果大家还有任何问题或建议，可以直接到中文版项目页面提交[Issue](https://github.com/golang-china/gopl-zh/issues)，如果发现英文版原文在[勘误](http://www.gopl.io/errata.html)中未提到的任何错误，可以直接去[英文版项目](https://github.com/adonovan/gopl.io/)提交。

最后，希望这本书能够帮助大家用Go语言快乐地编程。

2016年 1月 于 武汉



# 前言

*“Go是一个开源的编程语言，它很容易用于构建简单、可靠和高效的软件。”（摘自Go语言官方网站：[http://golang.org](http://golang.org/) ）*

Go语言由来自Google公司的[Robert Griesemer](http://research.google.com/pubs/author96.html)，[Rob Pike](http://genius.cat-v.org/rob-pike/)和[Ken Thompson](http://genius.cat-v.org/ken-thompson/)三位大牛于2007年9月开始设计和实现，然后于2009年的11月对外正式发布（译注：关于Go语言的创世纪过程请参考 http://talks.golang.org/2015/how-go-was-made.slide ）。

语言及其配套工具的设计目标是具有表达力，高效的编译和执行效率，有效地编写高效和健壮的程序。



Go语言有着和C语言类似的语法外表，和C语言一样是专业程序员的必备工具，可以用最小的代价获得最大的战果。 

但是它不仅仅是一个更新的C语言。==它还从其他语言借鉴了很多好的想法，同时避免引入过度的复杂性==。 

Go语言中和并发编程相关的特性是全新的也是有效的，同时对数据抽象和面向对象编程的支持也很灵活。

Go语言同时还集成了自动垃圾收集技术用于更好地管理内存。



Go语言尤其适合编写网络服务相关基础设施，同时也适合开发一些工具软件和系统软件。 但是Go语言确实是一个通用的编程语言，它也可以用在==图形图像驱动编程、移动应用程序开发 和机器学习==等诸多领域。

目前Go语言已经成为受欢迎的作为无类型的脚本语言的替代者： 因为==Go编写的程序通常比脚本语言运行的更快也更安全==，而且很少会发生意外的类型错误。



Go语言还是一个开源的项目，可以免费获取编译器、库、配套工具的源代码。 

Go语言的贡献者来自一个活跃的全球社区。

Go语言可以运行在类[UNIX](http://doc.cat-v.org/unix/)系统—— 比如[Linux](http://www.linux.org/)、[FreeBSD](https://www.freebsd.org/)、[OpenBSD](http://www.openbsd.org/)、[Mac OSX](http://www.apple.com/cn/osx/)——和[Plan9](http://plan9.bell-labs.com/plan9/)系统和[Microsoft Windows](https://www.microsoft.com/zh-cn/windows/)操作系统之上。 

Go语言编写的程序无需修改就可以运行在上面这些环境。

本书是为了帮助你开始以有效的方式使用Go语言，充分利用语言本身的特性和自带的标准库去编写清晰地道的Go程序。





# 简要对比|Python与Go

[link: 知乎专题|对比Python与Go](https://www.zhihu.com/question/36826836)

做脚本还是麻烦，完成同类工作，go编码工作量是比较高的。

好处是在方方面面几乎都是最不费脑子的那一种。适合做服务端项目。



1 Go的生态链现在比Python还是略差的，但也只是略差，事实上Go的标库和Python的标库一样丰富，还没有Python历史问题遗留的凌乱。

2 没Python顺手，实际感受代码量要比Python多40%左右，但可用，花头不多，出错机率不大，性能出奇的高。

3 不可以，该Python的时候还用Python，结了婚偶尔撸几管还是很爽的。



一些评测结果：

比较 Go 和 Python 在简单的 web 服务器方面的性能，单位为传输量每秒：
原生的 Go http 包要比 [web.py](https://link.zhihu.com/?target=http%3A//web.py/) 快 7 至 8 倍，如果使用 web.go 框架则稍微差点，比 [web.py](https://link.zhihu.com/?target=http%3A//web.py/) 快 6 至 7 倍。

如果是使用Python 中的 tornado 异步服务器和框架开发出的Web应用，那么要比传统的 [web.py](https://link.zhihu.com/?target=http%3A//web.py/) 快很多，此时，Go 大概只比它快 1.2 至 1.5 倍。

Go 和 Python 在一般开发的平均水平测试中，Go 要比 Python 3 快 25 倍左右，少占用三分之二的内存，但比 Python 大概多写一倍的代码，**毫无疑问，开发效率上，python是要技高一筹的**。

根据 Robert Hundt（2011 年 6 月）的文章对 C++、Java、Go 和 Scala，以及 Go 开发团队的反应，可以得出以下结论：

- Go 和 Scala 之间具有更多的可比性（都使用更少的代码），而 C++ 和 Java 都使用非常冗长的代码。
- ==Go 的编译速度要比绝大多数语言都要快==，比 Java 和 C++ 快 5 至 6 倍，比 Scala 快 10 倍。
- Go 的二进制文件体积是最大的（每个可执行文件都包含 runtime）。
- ==在最理想的情况下==，Go 能够和 C++ 一样快，比 Scala 快 2 至 3 倍，比 Java 快 5 至 10 倍。
- Go 在内存管理方面也可以和 C++ 相媲美，几乎只需要 Scala 所使用的一半，比 Java 少 4 倍左右。





以前，我用C++，界面也用Qt。后来，用python，再然后Go，C#。

现在，主要Python，Go，C#。用哪个不重要，关键是熟悉的、方便的。

工具这玩意，当然是多多益善，只不过是某个职业对哪个工具更擅长一些而已。



python和go不冲突，解决的领悟不一样。

- python是应用系统如web，后端服务，数据处理或单纯作为脚本

- go定位是系统级编程语言，二者有重叠的地方，但更多的是可以互为补充

我就同时使用这两个，很爽





Python虽然也是跨平台，它丰富的库反而成了跨平台部署桎梏，我看过一个做银行特色业务的实施过程，为了把引用的三方库的应用程序部署在一台内网Linux服务器上，花了三个多小时，我在看完部署过程后，想了想如果是操作很熟练至少也得半小时左右，如此复杂的部署，又是并不能随时停掉的系统，如果有版本bug或漏洞会让升级维护很麻烦。

如果是go语言部署的话完全不会有这种麻烦，生产机直接上传二进制文件，跨服迁移代码直接copy过去。

- 而且go语言的库现在不仅多，而且库升级兼容性很好。

- 自带的net，http，template完全可以做一个Web应用程序，性能还相当不错。

- 还有各种数据库的连接库，pdf，excel生成库等

当然像Python那种各式各样的库的确不够，毕竟时间轴在那儿，但缺少的自己可以写嘛。

至于学习曲线，go的确要难一点，但==无论是性能还是后期维护==，就会觉得多花一些时间学习真的很值。



如果只是自己用用的脚本，还是就小甜饼python吧。速度慢和部署难这两个python老大难，对你来说都不是问题。

go的代码是很吵的，隔几行就要判断一次err != nil



## 定位|尺有所长，寸有所短

总不能一门语言独大吧，语言的应用场景很多的，写业务用的，连数据库用的，渲染界面用的，做配置用的，教学用的，研究用的，装逼用的。。。



go编译后的文件是二进制文件，这个东西真的很方便。在运维里，你会发现那种不侵入系统环境的代码才是很厉害的，几乎现在很多的agent都是go写的。。



- python的机器学习，图像识别处理，数学和科学计算领域

- 如果是DBA运维那么果断选Ruby（Puppet等自动化运维中的神器全是Ruby）或者Python。
- 如果做Web开发那么PHP、Java或者Ruby。
- 如果是系统级编程以及游戏服务端那么C++、Erlang、Golang。
- 如果是**游戏以及Windows相关客户端**那么C#。
  - 游戏客户端U3D是主流，U3D里以C#为主
- 如果跨平台工具软件那么Java或者QT。
- 如果IOS/MAC，那么Swift， 安卓继续Java，前端继续JS。
- 如果是黑客脚本小子那么JS、PHP、Python（SqlMap可谓神器）、国内还流行易语言，哈哈哈哈。
- 如果像题主这么玩那随便。

