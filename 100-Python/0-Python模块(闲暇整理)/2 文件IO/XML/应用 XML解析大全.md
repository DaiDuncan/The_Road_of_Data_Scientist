| 上传本地XML文件到Chrome                    | https://www.youtube.com/watch?v=Z74WBwD_5Lw                  |
| ------------------------------------------ | ------------------------------------------------------------ |
| 基于jQuery和JavaScript上传XML后，调用XML   | Web data tutorial: **Retrieving** and displaying XML  data  https://www.youtube.com/watch?v=ppzYGd0wi_c |
| 基于Silverlight下载和读取XML文件——通过HTTP | https://www.youtube.com/watch?v=k4IY1M3xMaA                  |



# URL为XML文件

[Javscript](https://stackoverflow.com/questions/11778074/parsing-xml-data-from-a-remote-website)

[Java](https://stackoverflow.com/questions/15851700/parsing-xml-from-webpage/28449971)

Python

[minidom](https://stackoverflow.com/questions/16441823/python-xml-parsing-from-website)

[lxml](https://lxml.de/parsing.html)

[BeautifulSoup](https://www.youtube.com/watch?v=pKz1faPVNMA)

[ElementTree:针对xml文件](https://towardsdatascience.com/processing-xml-in-python-elementtree-c8992941efd2)

[urllib先将url转化为xml文件](https://stackoverflow.com/questions/21179272/parsing-a-url-xml-with-the-elementtree-xml-api/21179544)

```python
import urllib.request

opener = urllib.request.build_opener()
tree = ET.parse(opener.open(url))
```

 

@拓展：多个格式化的URL

http://www.trion1.com:6060/stat.xml,

http://www.trion2.com:6060/stat.xml,

http://www.trion3.com:6060/stat.xml

 

```python
import xml.etree.cElementTree as ET
import urllib2

for i in range(3):
  tree = ET.ElementTree(file=urllib2.urlopen('http://www.trion%i.com:6060/stat.xml' % i ))

  root = tree.getroot()
  root.tag, root.attrib
```



@注意 python3中没有urllib2——>urllib

```python
from urllib.request import urlopen

html = urlopen("http://www.google.com/").read()
print(html)
```



minidom VS lxml VS **ElementTree**(Python风格 操作简单-效率低)

https://stackoverflow.com/questions/8022477/xml-parsing-element-tree-etree-vs-minidom





# 补充

@需求@检索关键词

Retrieving und downloding the XML data in a website site:youtube.com

grab an XML file from the network



| 区分概念                   |      |
| -------------------------- | ---- |
| load 上传                  |      |
| retreving 取回             |      |
| **download**               |      |
| extract 从文件中提取       |      |
| parsing 从提取的文件中解析 |      |