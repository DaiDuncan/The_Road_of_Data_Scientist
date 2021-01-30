如果我们要编写一个搜索引擎：

- 第一步是用爬虫把目标网站的页面抓下来
- 第二步就是==解析该HTML页面==，看看里面的内容到底是新闻、图片还是视频。



HTML本质上是XML的子集，但是HTML的语法没有XML那么严格，所以不能用标准的DOM或SAX来解析HTML。

Python提供了HTMLParser来非常方便地解析HTML，只需简单几行代码：

```python
from html.parser import HTMLParser
from html.entities import name2codepoint

class MyHTMLParser(HTMLParser):

    def handle_starttag(self, tag, attrs):
        print('<%s>' % tag)

    def handle_endtag(self, tag):
        print('</%s>' % tag)

    def handle_startendtag(self, tag, attrs):
        print('<%s/>' % tag)

    def handle_data(self, data):
        print(data)

    def handle_comment(self, data):
        print('<!--', data, '-->')

    def handle_entityref(self, name):
        print('&%s;' % name)

    def handle_charref(self, name):
        print('&#%s;' % name)

parser = MyHTMLParser()
parser.feed('''<html>
<head></head>
<body>
<!-- test html parser -->
    <p>Some <a href=\"#\">html</a> HTML&nbsp;tutorial...<br>END</p>
</body></html>''')
```

`feed()`方法可以多次调用，也就是不一定一次把整个HTML字符串都塞进去，可以一部分一部分塞进去。



特殊字符有两种：这两种字符都可以通过Parser解析出来。

- 一种是英文表示的
- 一种是数字表示的`Ӓ`



# #示例

找一个网页，例如https://www.python.org/events/python-events/，用浏览器查看源码并复制

然后尝试解析一下HTML，输出Python官网发布的会议时间、名称和地点

```python
# -*-coding:UTF-8-*-

from html.parser import HTMLParser
from urllib.request import Request,urlopen
import re

def get_data(url):
   '''
   GET请求到指定的页面
   :return: HTTP响应
   '''

   headers = {
      'User-Agent': 'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.96 Safari/537.36'
      }
    
   # 提取网页内容，返回为str，请求方式为GET
   req = Request(url, headers=headers)
   with urlopen(req, timeout=25) as f:
      data = f.read()
      print(f'Status: {f.status} {f.reason}')
      print()
   return data.decode("utf-8")	# 转码后data为str类型

class MyHTMLParser(HTMLParser):
   '''由于继承了HTMLParser，所以handle_函数都进行了重写？？？(因为后面么有看到调用，想必是parser = MyHTMLParser()解析过程中直接利用了被重写的函数)'''
   def __init__(self):
      super().__init__()
      self.__parsedata='' # 设置一个空的标志位
      self.info = []

   def handle_starttag(self, tag, attrs):
      if ('class', 'event-title') in attrs:
         self.__parsedata = 'name'  # 通过属性判断如果该标签是我们要找的标签，设置标志位
      if tag == 'time':
         self.__parsedata = 'time'
      if ('class', 'say-no-more') in attrs:
         self.__parsedata = 'year'
      if ('class', 'event-location') in attrs:
         self.__parsedata = 'location'

   def handle_endtag(self, tag):
      self.__parsedata = ''# 在HTML 标签结束时，把标志位清空

   def handle_data(self, data):

      if self.__parsedata == 'name':
         # 通过标志位判断，输出打印标签内容
         self.info.append(f'会议名称:{data}')

      if self.__parsedata == 'time':
         self.info.append(f'会议时间:{data}')

      if self.__parsedata == 'year':
         if re.match(r'\s\d{4}', data): # 因为后面还有两组 say-no-more 后面的data却不是年份信息,所以用正则检测一下
            self.info.append(f'会议年份:{data.strip()}')

      if self.__parsedata == 'location':
         self.info.append(f'会议地点:{data} \n')

def main():
   parser = MyHTMLParser()
   URL = 'https://www.python.org/events/python-events/'
   data = get_data(URL)
   parser.feed(data)
   for s in parser.info:
      print(s)

if __name__ == '__main__':
   main()


### 结果
运行结果：

D:\Python37\python.exe D:/Python37/Code/html_parser_官网会议_v2.py
Status: 200 OK

会议名称:PyCon AU 2019
会议时间:02 Aug. – 06 Aug. 
会议年份:2019
会议地点:Sydney, Australia

会议名称:DjangoCon AU 2019
会议时间:02 Aug.
会议年份:2019
会议地点:Sydney, Australia

会议名称:PyBay
会议时间:15 Aug. – 18 Aug. 
会议年份:2019
会议地点:Mission Bay, San Francisco, CA, USA

会议名称:PyCon Korea 2019
会议时间:15 Aug. – 18 Aug. 
会议年份:2019
会议地点:Seoul

会议名称:Kiwi PyCon X
会议时间:23 Aug. – 25 Aug. 
会议年份:2019
会议地点:Wellington, New Zealand
```



补充：匹配正则表达式



---

利用HTMLParser，可以把网页中的==文本、图像==等解析出来



