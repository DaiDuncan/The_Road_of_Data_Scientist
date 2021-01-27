HTTP请求：

- urllib
- Requests

发送http请求后，服务器一般会返回json、xml、html这几种格式的数据



# urllib

Python标准库中提供了：urllib等模块以供HTTP请求，但是，它的 API 太渣了。

它是为另一个时代、另一个互联网所创建的。它需要巨量的工作，甚至包括各种方法覆盖，来完成最简单的任务。

```python
import urllib.request

req = urllib.request.Request('http://www.example.com/')
req.add_header('Referer', 'http://www.python.org/')
r = urllib.request.urlopen(req)

result = f.read().decode('utf-8')

发送携带请求头的GET请求
```







# Requests (第三方库)

发起HTTP请求，并获取请求的返回值

Requests 是使用 **Apache2** Licensed 许可证的，基于Python开发的HTTP 库。

其在Python内置模块的基础上进行了高度的封装，使用Requests可以轻而易举的完成**浏览器可有的任何操作**。

```python
# 安装模块
pip3 install requests

# 1无参数实例
import requests
 
ret = requests.get('https://github.com/timeline.json')
print(ret.url)
print(ret.text)
 
# 2有参数实例
#GET请求
import requests
 
payload = {'key1': 'value1', 'key2': 'value2'}
ret = requests.get("http://httpbin.org/get", params=payload)
 
print(ret.url)
print(ret.text)

==============================================
# 基本POST实例
import requests
 
payload = {'key1': 'value1', 'key2': 'value2'}
ret = requests.post("http://httpbin.org/post", data=payload)
 
print(ret.text)
 
    
# 发送请求头和数据实例
#POST请求
import requests
import json
 
url = 'https://api.github.com/some/endpoint'
payload = {'some': 'data'}
headers = {'content-type': 'application/json'}
 
ret = requests.post(url, data=json.dumps(payload), headers=headers)
 
print(ret.text)
print(ret.cookies)

==============================================
#其他请求
requests.get(url, params=None, **kwargs)
requests.post(url, data=None, json=None, **kwargs)
requests.put(url, data=None, **kwargs)
requests.head(url, **kwargs)
requests.delete(url, **kwargs)
requests.patch(url, data=None, **kwargs)
requests.options(url, **kwargs)
 
# 以上方法均是在此方法的基础上构建
requests.request(method, url, **kwargs)
```











