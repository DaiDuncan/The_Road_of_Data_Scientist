# HTTP抓取文本 & 文本处理

## 一 requests_html库

```python
from requests_html import HTMLSession

# 建立一个会话session: 即让Python作为一个客户端，和远端服务器交谈
session = HTMLSession() 

# 网址
url = 'https://www.cnblogs.com/sesshoumaru/p/6140987.html'
# 把这个链接对应的网页整个儿取回来
r = session.get(url)

# 抓取文字部分
print(r.html.text)

# 抓取所有的链接：HTML href标签
r.html.absolute_links
```



### #爬虫抓取网页特定内容

目标：寻找正文中**全部可以点击的蓝色文字链接**，拷贝文字到Excel表格。然后右键复制对应的链接，也拷贝到Excel表格。每个链接在Excel占一行，文字和链接各占一个单元格。

```
# 测试网页selector
sel = '#cnblogs_post_body > blockquote > ul:nth-child(9) > li:nth-child(1) > a > strong'
results = r.html.find(sel)  #[<Element 'strong' >]
results[0].text  #'abs：求数值的绝对值'

# 模块化：函数
def get_text_link_from_sel(sel):
    mylist = []
    try:
        results = r.html.find(sel)
        for result in results: #遍历
            mytext = result.text
            mylist.append(mytext)
            #mylink = list(result.absolute_links)[0] 不需要链接
            #mylist.append((mytext, mylink))
        return mylist
    except:
        return None

sel_test = '#cnblogs_post_body > blockquote > ul > li > a > strong'
tmp1 = get_text_link_from_sel(sel_test)
    
sel_test = '#cnblogs_post_body > blockquote > ul > li > strong > a'
tmp2 = get_text_link_from_sel(sel_test)

lst = tmp1+tmp2
len(lst)    #68个内置函数
```





## 二 requests库

```
import requests
r = requests.get("https://www.zdf.de/kinder/logo/logo-am-dienstagabend-104.html")
type(r) #requests.models.Response

sel = '#zdf-player-jbwoe417 > div.zdfplayer-subtitle-wrapper.zdfplayer-subtitles-n > div > div > div > div > p > span:nth-child(1)'
r.text.find('show-sel-large')  #一共有37925个

r.text
```

### #对比：requests_html库

```
from requests_html import HTMLSession

session = HTMLSession()
url = 'https://www.zdf.de/kinder/logo/logo-am-dienstagabend-104.html'
r = session.get(url)

print(r.html.text)
sel = '#zdf-player-jbwoe417 > div.zdfplayer-subtitle-wrapper.zdfplayer-subtitles-n > div > div > div > div > p' #> p > span:nth-child(1)

results = r.html.find(sel)
results #返回空列表：字幕绑定XML，动态读取
```





## 三 文本处理 | split分割

数据源：上述HTTP通信抓取的lst

```
t1, t2 = lst[0].split("：")
#t1: 'abs'
#t2: '求数值的绝对值'


# 格式化为.md的表格
for text in lst:
    func, descrip = text.split("：")
    print("| `{}()` | {} |".format(func, descrip))
```

### #补充：Python68个内置函数

| 方法             | 描述                                                         |
| ---------------- | ------------------------------------------------------------ |
| `abs()`          | 求数值的绝对值                                               |
| `divmod()`       | 返回两个数值的商和余数                                       |
| `max()`          | 返回可迭代对象中的元素中的最大值或者所有参数的最大值         |
| `min()`          | 返回可迭代对象中的元素中的最小值或者所有参数的最小值         |
| `pow()`          | 返回两个数值的幂运算值或其与指定整数的模值                   |
| `round()`        | 对浮点数进行四舍五入求值                                     |
| `sum()`          | 对元素类型是数值的可迭代对象中的每个元素求和                 |
| `bool()`         | 根据传入的参数的逻辑值创建一个新的布尔值                     |
| `int()`          | 根据传入的参数创建一个新的整数                               |
| `float()`        | 根据传入的参数创建一个新的浮点数                             |
| `complex()`      | 根据传入参数创建一个新的复数                                 |
| `str()`          | 返回一个对象的字符串表现形式(给用户)                         |
| `bytearray()`    | 根据传入的参数创建一个新的字节数组                           |
| `bytes()`        | 根据传入的参数创建一个新的不可变字节数组                     |
| `memoryview()`   | 根据传入的参数创建一个新的内存查看对象                       |
| `ord()`          | 返回Unicode字符对应的整数                                    |
| `chr()`          | 返回整数所对应的Unicode字符                                  |
| `bin()`          | 将整数转换成2进制字符串                                      |
| `oct()`          | 将整数转化成8进制数字符串                                    |
| `hex()`          | 将整数转换成16进制字符串                                     |
| `tuple()`        | 根据传入的参数创建一个新的元组                               |
| `list()`         | 根据传入的参数创建一个新的列表                               |
| `dict()`         | 根据传入的参数创建一个新的字典                               |
| `set()`          | 根据传入的参数创建一个新的集合                               |
| `frozenset()`    | 根据传入的参数创建一个新的不可变集合                         |
| `enumerate()`    | 根据可迭代对象创建枚举对象                                   |
| `range()`        | 根据传入的参数创建一个新的range对象                          |
| `iter()`         | 根据传入的参数创建一个新的可迭代对象                         |
| `slice()`        | 根据传入的参数创建一个新的切片对象                           |
| `super()`        | 根据传入的参数创建一个新的子类和父类关系的代理对象           |
| `object()`       | 创建一个新的object对象                                       |
| `all()`          | 判断可迭代对象的每个元素是否都为True值                       |
| `any()`          | 判断可迭代对象的元素是否有为True值的元素                     |
| `ilter()`        | 使用指定方法过滤可迭代对象的元素                             |
| `map()`          | 使用指定方法去作用传入的每个可迭代对象的元素，生成新的可迭代对象 |
| `next()`         | 返回可迭代对象中的下一个元素值                               |
| `reversed()`     | 反转序列生成新的可迭代对象                                   |
| `sorted()`       | 对可迭代对象进行排序，返回一个新的列表                       |
| `zip()`          | 聚合传入的每个迭代器中相同位置的元素，返回一个新的元组类型迭代器 |
| `help()`         | 返回对象的帮助信息                                           |
| `dir()`          | 返回对象或者当前作用域内的属性列表                           |
| `id()`           | 返回对象的唯一标识符                                         |
| `hash()`         | 获取对象的哈希值                                             |
| `type()`         | 返回对象的类型，或者根据传入的参数创建一个新的类型           |
| `len()`          | 返回对象的长度                                               |
| `ascii()`        | 返回对象的可打印表字符串表现方式                             |
| `format()`       | 格式化显示值                                                 |
| `vars()`         | 返回当前作用域内的局部变量和其值组成的字典，或者返回对象的属性列表 |
| `__import__()`   | 动态导入模块                                                 |
| `isinstance()`   | 判断对象是否是类或者类型元组中任意类元素的实例               |
| `issubclass()`   | 判断类是否是另外一个类或者类型元组中任意类元素的子类         |
| `hasattr()`      | 检查对象是否含有属性                                         |
| `getattr()`      | 获取对象的属性值                                             |
| `setattr()`      | 设置对象的属性值                                             |
| `delattr()`      | 删除对象的属性                                               |
| `callable()`     | 检测对象是否可被调用                                         |
| `compile()`      | 将字符串编译为代码或者AST对象，使之能够通过exec语句来执行或者eval进行求值 |
| `globals()`      | 返回当前作用域内的全局变量和其值组成的字典                   |
| `locals()`       | 返回当前作用域内的局部变量和其值组成的字典                   |
| `print()`        | 向标准输出对象打印输出                                       |
| `input()`        | 读取用户输入值                                               |
| `open()`         | 使用指定的模式和编码打开文件，返回文件读写对象               |
| `eval()`         | 执行动态表达式求值                                           |
| `exec()`         | 执行动态语句块                                               |
| `repr()`         | 返回一个对象的字符串表现形式(给解释器)                       |
| `property()`     | 标示属性的装饰器                                             |
| `classmethod()`  | 标示方法为类方法的装饰器                                     |
| `staticmethod()` | 标示方法为静态方法的装饰器                                   |







## 四 文本处理 | 去除格式控制：空行 标点等

```
# coding = utf-8
# 测试
with open('tmp.txt', 'r') as f1:
        fnew=open('newfile.txt','w')  
        try:
            for line in f1.readlines():
                #方式1 二进制读rb  空行内容为 b'\r\n'
                #方式2 字符读取r  空行内容为 '\n'
                if line == '\n': 
                    continue
                #测试用 print(line)
                if len(line)!=0:
                   fnew.write(line)
        finally:
            print('end')
            fnew.close() #缺少该语句，文件没有输出改变

# 模块化
def clearBlankLine():
    with open('tmp.txt', 'rb') as f1:
        fnew=open('newfile.txt','wb')  
        try:
            for line in f1.readlines():
                # 如果是空行
                if line == '\n':
                    line = line.strip()
                # 如果不是空行
                if len(line)!=0:
                    fnew.write(line)
        finally:
            fnew.close()
            

# 清除特殊符号
with open('newfile.txt', 'r') as file:
    for line in file.readlines():
        line = line.replace("）", " ")
        line = line.replace("：", " ")
        text = line.split()
        print("| `{}` | {} |".format(text[1], text[2]))
```

### #补充：关键字

```
import keyword
print(keyword.kwlist)

len(keyword.kwlist) #35个关键字
```



['False', 'None', 'True', 'and', 'as', 'assert', 'async', 'await', 'break', 'class', 'continue', 'def', 'del', 'elif', 'else', 'except', 'finally', 'for', 'from', 'global', 'if', 'import', 'in', 'is', 'lambda', 'nonlocal', 'not', 'or', 'pass', 'raise', 'return', 'try', 'while', 'with', 'yield']





## 五 文本处理 | 解析XML

```
import xml.etree.ElementTree as ET
file = r"logo_090620.xml"

tree = ET.parse(file)

root = tree.getroot()
root.tag    #'{http://www.w3.org/ns/ttml}tt'
root.attrib #{'{http://www.w3.org/ns/ttml#parameter}timeBase': 'media', \
            #'{http://www.w3.org/ns/ttml#parameter}cellResolution': '50 30', \
            #'{http://www.w3.org/XML/1998/namespace}lang': 'de-DE'}

for child in root:
    print(child.tag, child.attrib)

# root[0]表示head, [1]表示body
root[1][0][0][0].text   #'Es ist Dienstag, der 9. Juni,'
# 最后的[1]是换行符
root[1][0][0][2].text   #'ihr seid bei "logo!".'

subtitles = root.findall(".//{http://www.w3.org/ns/ttml}span")
for subtitle in subtitles:
    print(subtitle.text)
```

### #基于URL的XML文件

```
import xml.etree.ElementTree as ET
import urllib.request

opener = urllib.request.build_opener()

url = 'https://utstreaming.zdf.de/mtt/zdf/20/06/200609_di_neu_lot/2/logo_090620.xml'
tree = ET.parse(opener.open(url))

root = tree.getroot()
txt = ""
for subtitle in root.iter("{http://www.w3.org/ns/ttml}span"):
    txt = txt + subtitle.text + '\n'
    print(subtitle.text)
```