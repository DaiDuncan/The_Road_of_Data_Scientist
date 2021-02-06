在Web应用中，服务器把网页传给浏览器，实际上就是把==网页的HTML代码==发送给浏览器，让浏览器显示出来。而==浏览器和服务器之间的传输协议是HTTP==，所以：

- HTML是一种用来定义网页的文本，会HTML，就可以编写网页；
- HTTP是在网络上传输HTML的协议，用于==浏览器和服务器的通信==。



为什么要使用Chrome浏览器而不是IE呢？因为IE实在是太慢了，并且，IE对于开发和调试Web应用程序完全是一点用也没有。

我们需要在浏览器很方便地调试我们的Web应用，而==Chrome提供了一套完整的调试工具==，非常适合Web开发。





安装好Chrome浏览器后，打开Chrome，在菜单中选择“更多工具”，“开发者工具”，就可以显示开发者工具：

![image-20210131182023390](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210131182023.png)



`Elements`显示网页的结构

`Network`显示浏览器和服务器的通信。

我们点`Network`，确保第一个小红灯亮着，Chrome就会记录所有浏览器和服务器之间的通信(recording)：

![image-20210131182140326](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210131182140.png)

当我们在地址栏输入`www.sina.com.cn`时，浏览器将显示新浪的首页。

在这个过程中，浏览器都干了哪些事情呢？通过`Network`的记录，我们就可以知道。

- 在`Network`中，定位到第一条记录点击，右侧将显示`Request Headers`
- 点击右侧的`view source`，我们就可以看到浏览器发给新浪服务器的请求：

![image-20210131182620810](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210131182621.png)

![image-20210131182706602](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210131182706.png)

最主要的头两行分析如下，第一行：`GET / HTTP/1.1`

- `GET`表示一个读取请求，将==从服务器获得网页数据==
- `/`表示URL的路径，URL总是以`/`开头，`/`就表示首页

最后的`HTTP/1.1`指示采用的HTTP协议版本是1.1

目前HTTP协议的版本就是1.1，但是大部分服务器也支持1.0版本，主要区别在于1.1版本允许==多个HTTP请求复用一个TCP连接，以加快传输速度==。



从第二行开始，每一行都类似于`Xxx: xxxxx`：比如`Host: www.sina.com.cn`

- 表示请求的域名是`www.sina.com.cn`。
- 如果一台服务器有多个网站，服务器就需要通过`Host`来区分浏览器请求的是哪个网站。



继续往下找到`Response Headers`，点击`view source`，显示服务器返回的原始响应数据：

HTTP响应分为Header和Body两部分（Body是可选项），我们在`Network`中看到的Header最重要的几行如下：

```python
200 OK
```

`200`表示一个成功的响应，后面的`OK`是说明。

失败的响应有

- `404 Not Found`：网页不存在
- `500 Internal Server Error`：服务器内部出错，等等。



```python
Content-Type: text/html
```

`Content-Type`指示响应的内容，这里是`text/html`表示HTML网页。

请注意，浏览器就是依靠`Content-Type`来判断响应的内容是网页还是图片，是视频还是音乐。

浏览器并不靠URL来判断响应的内容，所以，==即使URL是`http://example.com/abc.jpg`，它也不一定就是图片。==





HTTP响应的Body就是HTML源码

- 网页上点击右键 => 查看源代码

当浏览器读取到新浪首页的HTML源码后，它会==解析HTML，显示页面==，然后，==根据HTML里面的各种链接==，再发送HTTP请求给新浪服务器，拿到相应的图片、视频、Flash、JavaScript脚本、CSS等各种资源，最终显示出一个完整的页面。

所以我们在`Network`下面能看到很多额外的HTTP请求。



# HTTP请求

跟踪了新浪的首页，我们来总结一下HTTP请求的流程：

## 1 浏览器首先向服务器发送HTTP请求

请求包括：

- 方法：`GET`还是`POST`，`GET`仅请求资源，`POST`会==附带用户数据==；
  - 如果是POST，那么请求还包括一个Body，包含用户数据。

- 路径：`/full/url/path`；

- 域名：由Host头指定：`Host: www.sina.com.cn`

- 以及其他相关的Header；





## 2 服务器向浏览器返回HTTP响应

响应包括：

- 响应代码：
  - `200`表示成功
  - `3xx`表示重定向
  - `4xx`表示客户端发送的请求有错误
  - `5xx`表示服务器端处理时发生了错误；
- 响应类型：由`Content-Type`指定，
  - 例如：`Content-Type: text/html;charset=utf-8`表示响应类型是HTML文本，并且编码是`UTF-8`
  - `Content-Type: image/jpeg`表示响应类型是JPEG格式的图片；
- 以及其他相关的Header；

通常服务器的HTTP响应会携带内容，也就是有一个Body，包含响应的内容，网页的HTML源码就在Body中。



## 3 补充|其他请求：再次发出HTTP请求

如果浏览器还需要继续向服务器请求其他资源，比如图片，就再次发出HTTP请求，重复步骤1、2。

Web采用的HTTP协议采用了非常简单的请求-响应模式，从而大大简化了开发。



当我们编写一个页面时，我们==只需要在HTTP响应中把HTML发送出去==，不需要考虑如何附带图片、视频等，浏览器如果需要请求图片和视频，它会发送另一个HTTP请求，==因此，一个HTTP请求只处理一个资源==。



HTTP协议同时具备极强的扩展性，虽然浏览器请求的是`http://www.sina.com.cn/`的首页，但是新浪在HTML中可以链入其他服务器的资源，比如`<img src="http://i1.sinaimg.cn/home/2013/1008/U8455P30DT20131008135420.png">`，从而==将请求压力分散到各个服务器上==

并且，==一个站点可以链接到其他站点==，无数个站点互相链接起来，就形成了World Wide Web，简称“三达不溜”（WWW）。





# HTTP格式

每个HTTP请求和响应都遵循相同的格式，一个HTTP包含Header和Body两部分，其中Body是可选的。

HTTP协议是一种文本协议，所以，它的格式也非常简单。



HTTP GET请求的格式：

- 每个Header一行一个，换行符是`\r\n`

```python
GET /path HTTP/1.1
Header1: Value1
Header2: Value2
Header3: Value3
```





HTTP POST请求的格式：

```python
POST /path HTTP/1.1
Header1: Value1
Header2: Value2
Header3: Value3

body data goes here...
```

当遇到连续两个`\r\n`时，Header部分结束，后面的数据全部是Body。





HTTP响应的格式：

```python
200 OK
Header1: Value1
Header2: Value2
Header3: Value3

body data goes here...
```

HTTP响应如果包含body，也是通过`\r\n\r\n`来分隔的。

请再次注意，Body的数据类型由`Content-Type`头来确定：

- 如果是网页，Body就是文本
- 如果是图片，Body就是图片的二进制数据。



当存在`Content-Encoding`时，Body数据是被压缩的，最常见的压缩方式是gzip

所以，看到`Content-Encoding: gzip`时，需要将Body数据==先解压缩，才能得到真正的数据==。

压缩的目的在于减少Body的大小，加快网络传输。



# #参考文献

[Link: 教程|廖雪峰](https://www.liaoxuefeng.com/wiki/1016959663602400/1017804782304672)

要详细了解HTTP协议，推荐“[HTTP: The Definitive Guide](http://shop.oreilly.com/product/9781565925090.do)”一书，非常不错，有中文译本：[HTTP权威指南](http://t.cn/R7FguRq)