HTTP请求：

- urllib
- Requests

发送http请求后，服务器一般会返回json、xml、html这几种格式的数据



在涉及到http的编程过程中，很容易遇到url编码问题和url解析的需求：url编码和url解码总是成对出现

- 对于url的解析，可以使用urllib.parse模块的urlparse函数

## url编码与解码

在浏览器里打开下面这个网址

```text
https://baike.baidu.com/item/URL%E7%BC%96%E7%A0%81/3703727?fr=aladdin
```

在浏览器网址输入栏里看到的url是这样的：

![image-20210201185140080](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210201185140.png)

中文的部分在浏览器里可以正常显示，但是如果你把它复制出来粘贴到文本编辑器中，中文部分就会变成 %E7%BC%96%E7%A0%81



在URL里，任何特殊的字符，即==不是ASCII的字符，包括汉字都会被编码，比如空格，在URL里用%20来代替==

在网络编程中，经常会使用到url编码：(quote: 引号)

```python
from urllib.parse import quote, unquote


url = 'https://baike.baidu.com/item/URL%E7%BC%96%E7%A0%81/3703727?fr=aladdin'
decode_url = unquote(url)
print(decode_url)	#https://baike.baidu.com/item/URL编码/3703727?fr=aladdin

encode_url = quote(decode_url)
print(encode_url)	#https%3A//baike.baidu.com/item/URL%E7%BC%96%E7%A0%81/3703727%3Ffr%3Daladdin
```



## url解析

对url解析使用urllib.parse模块的urlparse函数，解析十分方便

```python
from urllib.parse import urlparse

url = 'https://www.baidu.com/s?wd=url%20%E7%BC%96%E7%A0%81'
result = urlparse(url)

print(result)	#ParseResult(scheme='https', netloc='www.baidu.com', path='/s', params='', query='wd=url%20%E7%BC%96%E7%A0%81', fragment='')
print(result.scheme, result.netloc)	#https www.baidu.com
```





# urllib

urllib提供了一系列用于操作URL的功能。



Python标准库中提供了：urllib等模块以供HTTP请求，但是，它的 API 太渣了。

它是为另一个时代、另一个互联网所创建的。它需要巨量的工作，甚至包括各种方法覆盖，来完成最简单的任务。

```python
import urllib.request

req = urllib.request.Request('http://www.example.com/')
req.add_header('Referer', 'http://www.python.org/')
f = urllib.request.urlopen(req)

result = f.read().decode('utf-8')

发送携带请求头的GET请求
```



## get

urllib的`request`模块可以非常方便地抓取URL内容，也就是发送一个GET请求到指定的页面，然后返回HTTP的响应

- 例如，对豆瓣的一个URL`https://api.douban.com/v2/book/2129650`进行抓取，并返回响应：

```python
from urllib import request

with request.urlopen('https://api.douban.com/v2/book/2129650') as f:
    data = f.read()
    print('Status:', f.status, f.reason)
    for k, v in f.getheaders():
        print('%s: %s' % (k, v))
    print('Data:', data.decode('utf-8'))
```

可以看到HTTP响应的头和JSON数据：

```python
Status: 200 OK	#f.status, f.reason

### f.getheaders():
Server: nginx
Date: Tue, 26 May 2015 10:02:27 GMT
Content-Type: application/json; charset=utf-8
Content-Length: 2049
Connection: close
Expires: Sun, 1 Jan 2006 01:00:00 GMT
Pragma: no-cache
Cache-Control: must-revalidate, no-cache, private
X-DAE-Node: pidl1
    
Data: {"rating":{"max":10,"numRaters":16,"average":"7.4","min":0},"subtitle":"","author":["廖雪峰编著"],"pubdate":"2007-6",...}
```



如果我们要想==模拟浏览器发送GET请求，就需要使用`Request`对象==，通过往`Request`对象添加HTTP头，我们就可以==把请求伪装成浏览器==。例如，模拟iPhone 6去请求豆瓣首页：

```python
from urllib import request

req = request.Request('http://www.douban.com/')
req.add_header('User-Agent', 'Mozilla/6.0 (iPhone; CPU iPhone OS 8_0 like Mac OS X) AppleWebKit/536.26 (KHTML, like Gecko) Version/8.0 Mobile/10A5376e Safari/8536.25')

with request.urlopen(req) as f:
    print('Status:', f.status, f.reason)
    for k, v in f.getheaders():
        print('%s: %s' % (k, v))
    print('Data:', f.read().decode('utf-8'))
```

这样豆瓣会返回==适合iPhone==的移动版网页：

```python
...
    <meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, minimum-scale=1.0, maximum-scale=1.0">
    <meta name="format-detection" content="telephone=no">
    <link rel="apple-touch-icon" sizes="57x57" href="http://img4.douban.com/pics/cardkit/launcher/57.png" />
...
```





## Post

如果要以POST发送一个请求，只需要==把参数`data`以bytes形式传入==。

我们模拟一个微博登录，先读取登录的邮箱和口令，然后按照weibo.cn的登录页的格式以`username=xxx&password=xxx`的编码传入：

```python
from urllib import request, parse

print('Login to weibo.cn...')
email = input('Email: ')
passwd = input('Password: ')
login_data = parse.urlencode([
    ('username', email),
    ('password', passwd),
    ('entry', 'mweibo'),
    ('client_id', ''),
    ('savestate', '1'),
    ('ec', ''),
    ('pagerefer', 'https://passport.weibo.cn/signin/welcome?entry=mweibo&r=http%3A%2F%2Fm.weibo.cn%2F')
])

req = request.Request('https://passport.weibo.cn/sso/login')
req.add_header('Origin', 'https://passport.weibo.cn')
req.add_header('User-Agent', 'Mozilla/6.0 (iPhone; CPU iPhone OS 8_0 like Mac OS X) AppleWebKit/536.26 (KHTML, like Gecko) Version/8.0 Mobile/10A5376e Safari/8536.25')
req.add_header('Referer', 'https://passport.weibo.cn/signin/login?entry=mweibo&res=wel&wm=3349&r=http%3A%2F%2Fm.weibo.cn%2F')

with request.urlopen(req, data=login_data.encode('utf-8')) as f:
    print('Status:', f.status, f.reason)
    for k, v in f.getheaders():
        print('%s: %s' % (k, v))
    print('Data:', f.read().decode('utf-8'))
```

如果登录成功，我们获得的响应如下：

```python
Status: 200 OK
Server: nginx/1.2.0
...
Set-Cookie: SSOLoginState=1432620126; path=/; domain=weibo.cn
...
Data: {"retcode":20000000,"msg":"","data":{...,"uid":"1658384301"}}
```

如果登录失败，我们获得的响应如下：

```python
...
Data: {"retcode":50011015,"msg":"\u7528\u6237\u540d\u6216\u5bc6\u7801\u9519\u8bef","data":{"username":"example@python.org","errline":536}}
```



## Handler

如果还需要更复杂的控制，比如通过一个Proxy去访问网站，我们需要利用`ProxyHandler`来处理，示例代码如下：

```python
proxy_handler = urllib.request.ProxyHandler({'http': 'http://www.example.com:3128/'})
proxy_auth_handler = urllib.request.ProxyBasicAuthHandler()
proxy_auth_handler.add_password('realm', 'host', 'username', 'password')
opener = urllib.request.build_opener(proxy_handler, proxy_auth_handler)

with opener.open('http://www.example.com/login.html') as f:
    pass
```



urllib提供的功能就是利用程序去执行各种HTTP请求。

如果要模拟浏览器完成特定功能，需要把请求伪装成浏览器。伪装的方法是==先监控浏览器发出的请求，再根据浏览器的请求头来伪装==，`User-Agent`头就是用来标识浏览器的。



## 应用|urllib读取JSON

```python
# -*- coding: utf-8 -*-
from urllib import request

def fetch_data(url):
    req = request.Request(url)
    
    with request.urlopen(req) as f:
        print('Status:', f.status, f.reason)
        json_data = f.read().decode('utf-8') 	#二进制数据变成字符串，显示
       
    return json_data
```

注意：请求的结果视url而定

- 有的url已经失效
- 有的url并不是jason数据













