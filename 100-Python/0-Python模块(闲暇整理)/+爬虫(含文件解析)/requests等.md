# requests

发送HTTP请求的测试

| 请求类型 |                                                              |
| -------- | ------------------------------------------------------------ |
| get请求  | @get请求还可以传递参数     <br>payload =  {'key1': 'value1', 'key2': 'value2'}  <br>r = requests.get("http://httpbin.org/get", **params**=payload)  实际上它构造成了如下网址：  http://httpbin.org/get?key1=value1&key2=value2 |
| POST请求 | @无参数的post请求  <br>r =  requests.post("http://httpbin.org/post") |
|          | @有参数的post请求  payload =  {'key1': 'value1', 'key2': 'value2'}  <br>r =  requests.post("http://httpbin.org/post", data=payload)     post请求多用来**提交表单数据**，即填写一堆输入框，然后提交 |
| 其他请求 | 例如put请求、delete请求、head请求、option请求等其实都是类似的 |



发送http请求获得了网页的HTML内容

```python
import requests

r = requests.get('https://unsplash.com') #向目标url地址发送get请求，返回一个response对象
print(r.text) #r.text是http response的网页HTML
```



# requests_html

基于HTTP协议的接口测试，那么一定会首选Requsts

现在作者Kenneth Reitz 又开发了requests_html 用于做爬虫

基于现有的框架 PyQuery、Requests、lxml、beautifulsoup4等库进行了二次封装，作者将Requests设计的简单强大的优点带到了该项目中。

```python
pip install requests-html

from requests_html import HTMLSession
session = HTMLSession()

r = session.get('https://python.org/')

# 获取页面上的所有链接。
all_links =  r.html.links
print(all_links)

# 获取页面上的所有链接，以绝对路径的方式。
all_absolute_links = r.html.absolute_links

print(all_absolute_links)
```





## 方法

| 方法          | 功能                           |
| ------------- | ------------------------------ |
| r.html.find() | 查找相关元素 @Chrome: selector |
|               |                                |
|               |                                |



```python
### 下载新闻标题和链接

# 通过CSS找到新闻标签
news = r.html.find('h2.news_entry > a')

for new in news:
    print(new.text)  # 获得新闻标题
    print(new.absolute_links)  # 获得新闻链接
    
    
    
### 下载图片
# 查找页面中背景图，找到链接，访问查看大图，并获取大图地址
items_img = r.html.find('ul.clearfix > li > a')
for img in items_img:
    img_url = img.attrs['href']
    if "/wallpaper_detail" in img_url:
        r = session.get(img_url)
        item_img = r.html.find('img.pic-large', first=True)
        url = item_img.attrs['src']
        title = item_img.attrs['title']
        print(url+title)
        save_image(url, title)
```











# BeautifulSoup

从HTML中获得图片



