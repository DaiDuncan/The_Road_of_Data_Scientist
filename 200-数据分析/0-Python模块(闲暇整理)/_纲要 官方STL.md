备注：笔记命名

- 单独一个库：直接用库名(英文，比如re)
- 多个库形成一个功能：用中文(比如文件IO)

---

Python是一个依赖强大的组件库完成对应功能的语言

- 前辈大牛们打造了多种多样的工具库公开提供给大众使用
- 而越来越多的库已经因为使用的广泛和普遍及其功能的强大，已经成为Python的标准库

标准库：

- 包含了多个内置模块 (以 C 编写) => 依靠它们来实现系统级功能
  - 例如文件 I/O
- 大量以 Python 编写的模块，提供了日常编程中许多问题的标准解决方案
  - 其中有些模块经过专门设计，通过将特定平台功能抽象化为平台中立的 API 来鼓励和加强 Python 程序的可移植性



在这个标准库以外还存在成千上万并且不断增加的其他组件 (从单独的程序、模块、软件包直到完整的应用开发框架)，均可以在网络上搜索到并下载使用。

- 图形界面开发
- 3D游戏开发
- Web应用开发
- 网络编程
- 系统网络 运维
- 科学计算
- 人工智能



# [怎样系统的学习Python标准库？](https://www.zhihu.com/question/22100190)

看官方文档：

- The Python Standard Library
- Python HOWTOs

学习资料：

- 《每周一个 Python 3 模块》 [Python 3 Module of the Week](https://link.zhihu.com/?target=https%3A//pymotw.com/3/index.html)

技巧：

- dir()和help()很好用



工具：[Link: Mac|Dash+Alfred](https://www.jianshu.com/p/77d2bf8df81f)

- 查看API文档的神器



注意：很多第三方库比标准库更好用，迭代也更快

- ujson就比json效率高十倍以上，处理大包特别有效
- sqlalchemy，zmq，tornado，mqtt，lxml，requests等等都不是标准库，但是比标准库的对应版本好用或者标准库就没有



说明：多看别人写的优秀代码有助于提高个人写代码的姿势，不鼓励重复造轮子但鼓励重复拆轮子，研究轮子然后造全新的轮子

根据经验总结出来的我认为最好的做法是：在使用任何一个模块之前，先通读一遍其官方文档，了解其为我们提供了哪些 API，之后在实际编程的过程中遇到了相关需求，就针对这个需求再去查阅文档，如果文档说的不够详细，再 google 网上的一些代码示例。





# 数据类型⭐

| 分类       |                                      |
| ---------- | ------------------------------------ |
| 数据类型   | ==datetime==：基于日期与时间工具     |
|            | ==calendar==：通用月份函数           |
|            | ==collections==：容器数据类型        |
|            | collections.abc：容器虚基类          |
|            | ==heapq==：堆、队列算法              |
|            | bisect：数组二分算法                 |
|            | array：高效数值数组                  |
|            | weakref：弱引用                      |
|            | types：内置类型的动态创建与命名      |
|            | ==copy==：浅拷贝与深拷贝             |
|            | pprint：格式化输出                   |
|            | reprlib：交替repr()的实现            |
| 文本       | ==string==：通用字符串操作           |
|            | ==re==：正则表达式操作               |
|            | difflib：差异计算工具                |
|            | textwrap：文本填充                   |
|            | unicodedata：Unicode字符数据库       |
|            | stringprep：互联网字符串准备工具     |
|            | readline：GNU按行读取接口            |
|            | rlcompleter：GNU按行读取的实现函数   |
| 二进制数据 | struct：将字节解析为打包的二进制数据 |
|            | codecs：注册表与基类的编解码器       |







# 数学⭐

| 分类 |                             |
| ---- | --------------------------- |
| 数学 | numbers：数值的虚基类       |
|      | ==math==：数学函数          |
|      | cmath：复数的数学函数       |
|      | decimal：定点数与浮点数计算 |
|      | fractions：有理数           |
|      | ==random==：生成伪随机数    |







# 文件I/O⭐

| 分类       |                                        |
| ---------- | -------------------------------------- |
| 文件与目录 | ==os.path==：通用路径名控制            |
|            | fileinput：从多输入流中遍历行          |
|            | stat：解释stat()的结果                 |
|            | filecmp：文件与目录的比较函数          |
|            | tempfile：生成临时文件与目录           |
|            | glob：Unix风格路径名格式的扩展         |
|            | ==fnmatch==：Unix风格路径名格式的比对  |
|            | linecache：文本行的随机存储            |
|            | shutil：高级文件操作                   |
|            | macpath：MacOS 9路径控制函数           |
| 加载、存储 | ==pickle==：Python对象序列化           |
|            | @互联网 ==json==：JSON编码与解码       |
|            | copyreg：注册机对pickle的支持函数      |
|            | shelve：Python对象持久化               |
|            | marshal：内部Python对象序列化          |
|            | dbm：Unix“数据库”接口                  |
|            | ==sqlite3==：针对SQLite数据库的API2.0  |
| 压缩       | zlib：兼容gzip的压缩                   |
|            | gzip：对gzip文件的支持                 |
|            | bz2：对bzip2压缩的支持                 |
|            | lzma：使用LZMA算法的压缩               |
|            | ==zipfile==：操作ZIP存档               |
|            | ==tarfile==：读写tar存档文件           |
| 文件格式化 | ==csv==：读写CSV文件                   |
|            | configparser：配置文件解析器           |
|            | netrc：netrc文件处理器                 |
|            | xdrlib：XDR数据编码与解码              |
|            | plistlib：生成和解析Mac OS X.plist文件 |







# 操作系统

| 分类           |                                                           |
| -------------- | --------------------------------------------------------- |
| 操作系统工具   | ==os==：多方面的操作系统接口                              |
|                | ==io==：流核心工具                                        |
|                | ==time==：时间的查询与转化                                |
|                | argparser：命令行选项、参数和子命令的解析器               |
|                | optparser：命令行选项解析器                               |
|                | getopt：C风格的命令行选项解析器                           |
|                | logging：Python日志工具                                   |
|                | logging.config：日志配置                                  |
|                | logging.handlers：日志处理器                              |
|                | getpass：简易密码输入                                     |
|                | curses：字符显示的终端处理                                |
|                | curses.textpad：curses程序的文本输入域                    |
|                | curses.ascii：ASCII字符集工具                             |
|                | curses.panel：curses的控件栈扩展                          |
|                | platform：访问底层平台认证数据                            |
|                | errno：标准错误记号                                       |
|                | ctypes：Python外部函数库                                  |
| ==并发==       | ==threading==：基于线程的并行                             |
|                | ==multiprocessing==：基于进程的并行                       |
|                | ==concurrent==：并发包                                    |
|                | concurrent.futures：启动并行任务                          |
|                | subprocess：子进程管理                                    |
|                | sched：事件调度                                           |
|                | queue：同步队列                                           |
|                | select：等待I / O完成                                     |
|                | dummy_threading：threading模块的替代（当_thread不可用时） |
|                | _thread：底层的线程API（threading基于其上）               |
|                | \_dummy_thread：\_thread模块的替代（当_thread不可用时）   |
| ==进程间通信== | socket：底层网络接口                                      |
|                | ssl：socket对象的TLS / SSL填充器                          |
|                | asyncore：异步套接字处理器                                |
|                | asynchat：异步套接字命令 / 响应处理器                     |
|                | signal：异步事务信号处理器                                |
|                | mmap：内存映射文件支持                                    |
| Windows相关    | msilib：读写Windows的Installer文件                        |
|                | msvcrt：MS VC + + Runtime的有用程序                       |
|                | winreg：Windows注册表访问                                 |
|                | ==winsound==：Windows声音播放接口                         |
| Unix相关       | posix：最常用的POSIX调用                                  |
|                | pwd：密码数据库                                           |
|                | spwd：影子密码数据库                                      |
|                | grp：组数据库                                             |
|                | crypt：Unix密码验证                                       |
|                | termios：POSIX风格的tty控制                               |
|                | tty：终端控制函数                                         |
|                | pty：伪终端工具                                           |
|                | fcntl：系统调用fcntl()和ioctl()                           |
|                | pipes：shell管道接口                                      |
|                | resource：资源可用信息                                    |
|                | nis：Sun的NIS的接口                                       |
|                | syslog：Unix 日志服务                                     |





# 网络⭐

| 分类                 |                                              |
| -------------------- | -------------------------------------------- |
| 互联网               | email：邮件与MIME处理包                      |
|                      | ==json==：JSON编码与解码                     |
|                      | mailcap：mailcap文件处理                     |
|                      | mailbox：多种格式控制邮箱                    |
|                      | mimetypes：文件名与MIME类型映射              |
|                      | base64：RFC                                  |
|                      | 3548：Base16、Base32、Base64编码             |
|                      | binhex：binhex4文件编码与解码                |
|                      | binascii：二进制码与ASCII码间的转化          |
|                      | quopri：MIME                                 |
|                      | quoted - printable数据的编码与解码           |
|                      | uu：uuencode文件的编码与解码                 |
| ==HTML与XML==        | html：HTML支持                               |
|                      | html.parser：简单HTML与XHTML解析器           |
|                      | html.entities：HTML通用实体的定义            |
|                      | xml：XML处理模块                             |
|                      | xml.etree.ElementTree：树形XML元素API        |
|                      | xml.dom：XML DOM API                         |
|                      | xml.dom.minidom：XML DOM最小生成树           |
|                      | xml.dom.pulldom：构建部分DOM树的支持         |
|                      | xml.sax：SAX2解析的支持                      |
|                      | xml.sax.handler：SAX处理器基类               |
|                      | xml.sax.saxutils：SAX工具                    |
|                      | xml.sax.xmlreader：SAX解析器接口             |
|                      | xml.parsers.expat：运用Expat快速解析XML      |
| ==互联网协议与支持== | webbrowser：简易Web浏览器控制器              |
|                      | cgi：CGI支持                                 |
|                      | cgitb：CGI脚本反向追踪管理器                 |
|                      | wsgiref：WSGI工具与引用实现                  |
|                      | ==urllib==：URL处理模块                      |
|                      | ==urllib.request==：打开URL连接的扩展库      |
|                      | ==urllib.response==：urllib模块的响应类      |
|                      | ==urllib.parse==：将URL解析成组件            |
|                      | ==urllib.error==：urllib.request引发的异常类 |
|                      | urllib.robotparser：robots.txt的解析器       |
|                      | http：HTTP模块                               |
|                      | http.client：HTTP协议客户端                  |
|                      | ftplib：FTP协议客户端                        |
|                      | poplib：POP协议客户端                        |
|                      | imaplib：IMAP4协议客户端                     |
|                      | nntplib：NNTP协议客户端                      |
|                      | smtplib：SMTP协议客户端                      |
|                      | smtpd：SMTP服务器                            |
|                      | telnetlib：Telnet客户端                      |
|                      | uuid：RFC4122的UUID对象                      |
|                      | socketserver：网络服务器框架                 |
|                      | http.server：HTTP服务器                      |
|                      | ==http.cookies==：HTTPCookie状态管理器       |
|                      | ==http.cookiejar==：HTTP客户端的Cookie处理   |
|                      | xmlrpc：XML - RPC服务器和客户端模块          |
|                      | xmlrpc.client：XML - RPC客户端访问           |
|                      | xmlrpc.server：XML - RPC服务器基础           |
|                      | ipaddress：IPv4 / IPv6控制库                 |







# 多媒体

| 分类   |                                    |
| ------ | ---------------------------------- |
| 多媒体 | audioop：处理原始音频数据          |
|        | aifc：读写AIFF和AIFC文件           |
|        | sunau：读写Sun AU文件              |
|        | ==wave==：读写WAV文件              |
|        | chunk：读取IFF大文件               |
|        | colorsys：颜色系统间转化           |
|        | imghdr：指定图像类型               |
|        | sndhdr：指定声音文件类型           |
|        | ossaudiodev：访问兼容OSS的音频设备 |



 



# 编程思想⭐

| 分类           |                                             |
| -------------- | ------------------------------------------- |
| ==函数式编程== | ==itertools==：为高效循环生成迭代器         |
|                | ==functools==：可调用对象上的高阶函数与操作 |
|                | ==operator==：针对函数的标准操作            |
| 编程框架       | ==turtle==：Turtle图形库                    |
|                | cmd：基于行的命令解释器支持                 |
|                | shlex：简单词典分析                         |
| 导入模块       | imp：访问import模块的内部                   |
|                | zipimport：从ZIP归档中导入模块              |
|                | pkgutil：包扩展工具                         |
|                | modulefinder：通过脚本查找模块              |
|                | runpy：定位并执行Python模块                 |
|                | importlib：import的一种实施                 |
| Python语言     | parser：访问Python解析树                    |
|                | ast：抽象句法树                             |
|                | symtable：访问编译器符号表                  |
|                | symbol：Python解析树中的常量                |
|                | token：Python解析树中的常量                 |
|                | keyword：Python关键字测试                   |
|                | tokenize：Python源文件分词                  |
|                | tabnany：模糊缩进检测                       |
|                | pyclbr：Python类浏览支持                    |
|                | py_compile：编译Python源文件                |
|                | compileall：按字节编译Python库              |
|                | dis：Python字节码的反汇编器                 |
|                | pickletools：序列化开发工具                 |



 

# GUI

| 分类           |                                    |
| -------------- | ---------------------------------- |
| Tk图形用户接口 | tkinter：Tcl / Tk接口              |
|                | tkinter.ttk：Tk主题控件            |
|                | tkinter.tix：Tk扩展控件            |
|                | tkinter.scrolledtext：滚轴文本控件 |



 



# 开发

| 分类     |                                  |
| -------- | -------------------------------- |
| 开发工具 | pydoc：文档生成器和在线帮助系统  |
|          | doctest：交互式Python示例        |
|          | ==unittest==：单元测试框架       |
|          | unittest.mock：模拟对象库        |
|          | test：Python回归测试包           |
|          | test.support：Python测试工具套件 |
|          | ==venv==：虚拟环境搭建           |



 

# 运维

| 分类   |                                       |
| ------ | ------------------------------------- |
| 调试   | bdb：调试框架                         |
|        | faulthandler：Python反向追踪库        |
|        | pdb：Python调试器                     |
|        | ==timeit==：小段代码执行时间测算      |
|        | trace：Python执行状态追踪             |
| 运行时 | ==sys==：系统相关的参数与函数         |
|        | sysconfig：访问Python配置信息         |
|        | builtins：内置对象                    |
|        | main：顶层脚本环境                    |
|        | warnings：警告控制                    |
|        | contextlib：with状态的上下文工具      |
|        | abc：虚基类                           |
|        | atexit：出口处理器                    |
|        | traceback：打印或读取一条栈的反向追踪 |
|        | future：未来状态定义                  |
|        | gc：垃圾回收接口                      |
|        | inspect：检查存活的对象               |
|        | site：址相关的配置钩子（hook）        |
|        | fpectl：浮点数异常控制                |
|        | distutils：生成和安装Python模块       |
| 解释器 | code：基类解释器                      |
|        | codeop：编译Python代码                |



 # 安全|加密

| 分类 |                             |
| ---- | --------------------------- |
| 加密 | hashlib：安全散列与消息摘要 |
|      | hmac：针对消息认证的键散列  |



 

# 其他

| 分类   |                             |
| ------ | --------------------------- |
| 国际化 | gettext：多语言的国际化服务 |
|        | locale：国际化服务          |
| 其它   | formatter：通用格式化输出   |
|        |                             |



 

# #参考文献

[Link: 官方-中文|标准库](https://docs.python.org/zh-cn/3/library/index.html)

[Link: 汇总|一句话简介](https://jishuin.proginn.com/p/763bfbd348c4)  2020.12.18

