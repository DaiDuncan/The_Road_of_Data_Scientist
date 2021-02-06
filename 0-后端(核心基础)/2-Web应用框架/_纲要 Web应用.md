了解了WSGI框架，我们发现：其实一个Web App，就是==写一个WSGI的处理函数，针对每个HTTP请求进行响应==。

如何处理HTTP请求不是问题，问题是如何处理100个不同的URL。

每一个URL可以对应GET和POST请求，当然还有PUT、DELETE等请求：

- 但是我们通常只考虑最常见的GET和POST请求。



一个最简单的想法是从`environ`变量里取出HTTP请求的信息，然后逐个判断：

```python
def application(environ, start_response):
    method = environ['REQUEST_METHOD']
    path = environ['PATH_INFO']
    if method=='GET' and path=='/':
        return handle_home(environ, start_response)
    if method=='POST' and path='/signin':
        return handle_signin(environ, start_response)
    ...
```

只是这么写下去代码是肯定没法维护了。

代码这么写没法维护的原因是因为WSGI提供的接口虽然比HTTP接口高级了不少，但和Web App的处理逻辑比，还是比较低级，我们需要==在WSGI接口之上能进一步抽象，让我们专注于用一个函数处理一个URL==，至于URL到函数的映射，就交给Web框架来做。



用Python开发一个Web框架十分容易，所以Python有上百个开源的Web框架。

选择一个比较流行的Web框架——[Flask](http://flask.pocoo.org/)来使用。





# B/S 浏览器-服务器

运行在网络上，以浏览器作为通用客户端的应用程序，在许多地方又被称为B/S（Browser/Server，浏览器-服务器）模式的应用

- 当使用IE或者FireFox在网易、新浪等门户网站上冲浪时，使用的就是Web应用

一个Web应用程序是由完成特定任务的各种Web组件（web components)构成的并通过Web将服务展示给外界

- 多个Servlet、JSP页面、HTML文件以及图像文件等
- 核心是对数据库的相关操作和处理

面向表现的Web应用程序

- 包含了很多种标记语言和动态内容的交互的web页面作为对请求的响应

面向服务的Web应用

- 实现了Web服务的端点(endpoint)

一般来说，一个Web应用可以看成是一组安装在服务器URL名称空间的特定子集下面的Servlet的集合



## servlet|Java的API(规范操作)

Java Servlet 是运行在 Web 服务器或应用服务器上的程序，它是作为来自 Web 浏览器或其他 HTTP 客户端的请求和 HTTP 服务器上的数据库或应用程序之间的中间层

使用 Servlet，您可以收集来自网页表单的用户输入，呈现来自数据库或者其他源的记录，还可以动态创建网页。

Java Servlet 通常情况下与使用 CGI（Common Gateway Interface，公共网关接口）实现的程序可以达到异曲同工的效果

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210108104139.png" alt="image-20210108104139739" style="zoom:80%;" />







# C/S 客户端-服务器

还存在一种所谓的C/S（Client/Server，客户端-服务器）模式的应用。

- 例如，常用的聊天工具QQ或MSN、下载工具迅雷等就是典型的C/S模式应用



Web应用是基于标准的应用层协议HTTP的，而C/S模式的应用是基于网络层协议TCP/UDP的

- 应用层协议是定义在网络层协议之上的
- 正是由于Web应用采用的是一种==更高层次的标准化通信模式==，是Web应用能够获得更快的普及和推广。