# 数据I/O | 解析XML 提取字幕

## 一、起：问题背景

考虑到语言学习、使用存在**易学难精**和**遗忘**的规律，笔者想要通过翻译新闻的方式，在训练听力的同时，积累常用的表达素材。

此外，在翻译过程中也可以锻炼理解、语言转化的能力。可谓一举多得，快哉快哉！



选择的素材是德国第二电视台(ZDF)，一款儿童新闻节目——Logo!

(你没看错：听、说方面，我还是个“孩子”……)

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1591951955749-bc95c824-c20c-4ef9-b2f4-d8c2cb2c2cd9.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_10%2Ctext_6K-t6ZuALUREdW5jYW4%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)



起初，笔者边看视频，边记录听到的内容。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1591951615153-8294baa3-af00-4f7f-9926-980ee9859403.png)



在没有字幕的情况下，3分钟的内容大概**花了30分钟**来听、写、记录。

心想装逼可以，装过头了就真的很难受......

为了拯救逐渐崩溃的心态，果断打开了字幕。



随之而来的一个问题是，跟着字幕敲击单词也是一项**繁重的工作**，**容易出错**，而且**重复枯燥低效率**。

作为具有**极简素养**的笔者，秉承着**一切重复枯燥事务自动化**的原则，果断考虑下载字幕。



如上图视频界面所示，提供了下载按钮。

然而，天杀的却不带字幕。

于是，一场知识饕餮的血雨腥风拉开了序幕.........







## 二、承：HTML页面分析

### 1 So easy: HTML爬虫抓取数据 我会！

```
### 方法1：基于requests库

import requests

r = requests.get("https://www.zdf.de/kinder/logo/logo-am-dienstagabend-104.html")

# 网页元素selector
sel = '#zdf-player-jbwoe417 > div.zdfplayer-subtitle-wrapper.zdfplayer-subtitles-n > div > div > div > div > p > span:nth-child(1)'
r.text.find(sel)



### 方法2：基于requests_html库
from requests_html import HTMLSession

session = HTMLSession()
url = 'https://www.zdf.de/kinder/logo/logo-am-dienstagabend-104.html'
r = session.get(url)
print(r.html.text)

sel = '#zdf-player-jbwoe417 > div.zdfplayer-subtitle-wrapper.zdfplayer-subtitles-n > div > div > div > div > p' #> p > span:nth-child(1)
results = r.html.find(sel)
results
```



读取HTML页面，结果是有的，如下图：

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1591952400759-985643da-2453-4c61-9275-e1bb410043e8.png)



但是按照字幕对应的Selector查找，结果屁都没有：直接显示**空列表[]**

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1591952527916-61f83e01-f5ed-4c7a-a10e-bb4282498a61.png)





### 2 Life becomes harder: 分析HTML页面

分析的思路，自然是由近及远，找相关的内容。



#### 1）陌生的属性 xmlns:tt

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1591952848329-1bee9f56-5329-47da-8778-94f187693b28.png)

如上图，又含有begin, end的时间，明摆着就是跟字幕勾搭在一块儿的。

于是，满怀期待地输入xmlns:tt对应的网址，心想：这下还不给我捉奸在网。



输入http://www.w3.org/ns/ttml之后，

一看页面，心就凉了半截，啥玩意儿？就是个普通的网页啊。



#### 2）调整策略：查找相关内容

当时的思路时：这个陌生的**属性xmlns**，可能和某个JavaScript文件挂钩。

应该从**该JavaScript源码**中去寻找绑定的字幕内容。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1591953532962-6cea6791-2991-40c4-a69b-975d0b64a660.png)



就这么，尽一切努力，找啊找啊，一直追溯到最初的关联区域。

期间看到content, embed_content也是一阵高潮，事后证明：假高潮来得快，去得更快。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1591953127367-0f349167-71d3-4ac8-b7d3-7ca9deaa3d7b.png)

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1591953142360-9283a76f-37fc-497d-aacf-eb2a9646e61c.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_10%2Ctext_6K-t6ZuALUREdW5jYW4%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)







## 三、转：定位到XML文件

### 1 Deadline的突破：发现字幕XML

时间一分一秒流逝，头也在慢慢涨大。

突然，电脑桌面弹出 21:45关机休息的提醒。



心一横，不整一些杂七杂八的东西。

**问题在哪，就在哪正面刚！**

要不然怎么说Deadline是最后生产力？！



回到那个陌生的标签属性 xmlns:tt，检索过后，发现是**与XML相关的命名空间**。

猜测是否有XML文件，反正也是死马当活马医了。



正是“山重水复疑无路，柳暗花明又一村”——陆游老人家的话真的很有道理：

输入查找xml：**噌**！(脑补《中华小当家》开盖瞬间)

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1591953706223-b48ee7a6-5b6a-4d8e-992b-5a170884e81c.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_10%2Ctext_6K-t6ZuALUREdW5jYW4%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)



### 2 进一步探索

XML文件有了，之后用Python解析的过程也比较套路化，代码附在文末。



我想：能不能再进一步

——基于Python，输入视频的网址，**直接提取**服务器上的XML文件(貌似这是唯一的XML文件)。

然后解析XML，提取字幕文本输出到Txt文件中。



### 3 进一步折腾

第二天，满怀着生命的憧憬与期待，我终究还是起晚了……

但是所获也颇丰：

#### 1）理解了[基本的XML概念](https://www.runoob.com/xml/xml-tutorial.html)

#### 2）进一步了解了[XML DOM操作](https://www.runoob.com/dom/dom-tutorial.html)

从中看到了一个XMLHttpRequest 对象，可以用来幕后与服务器交换数据。

一开始是有期待的，后来感觉这个在Web开发中，可以用来通信，但我只是下载一个XML文件啊。

(不知道这里能不能用，存疑：**有懂行的大佬，请不吝赐教**~)



#### 3）惊！众里寻他千百度，那人却……

心生哀叹之际，总会回到伤心故地，回到最初的地方，也就是XML文件。

到这里，我才明白“众里寻他千百度，那人却在灯火阑珊处”的百般滋味，如下图

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1591954499974-60159201-a000-4a87-a0ad-cb732fe61855.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_10%2Ctext_6K-t6ZuALUREdW5jYW4%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)



在XML文件的Headers应答中，已经有了**XML文件的URL地址**。

真的是“妙哇”！







## 四、合：梳理思路

### 1 流程：

1）打开视频：在浏览器工具，network选项卡下面，查找XML文件，复制**该文件的URL地址**

2）URL作为脚本的输入，运行Python脚本

3）输出Text (注意德语含Unicode字符，需要把编码改为utf-8)



### 2 Python脚本：

```
import sys

import xml.etree.ElementTree as ET
import urllib.request


def get_subtitle(url, txt_file):
    '''Get the subtitle of URL represented for XML into a Text file'''
    opener = urllib.request.build_opener()

    tree = ET.parse(opener.open(url))
    root = tree.getroot()
    txt = ""

    # Parse the XML file: get subtile text
    for subtitle in root.iter("{http://www.w3.org/ns/ttml}span"):
        txt = txt + subtitle.text + '\n'
        print(subtitle.text)

    # Output as Text file
    with open(txt_file, 'w', encoding="utf-8") as f:
        f.write(txt)

    print('Subtitle already to {}'.format(txt_file))


def main():
    # run python with .py URL Text
    if len(sys.argv) == 3:
        url, txt_file = sys.argv[1:]  #input is already String

        get_subtitle(url, txt_file)

    else:
        print('Please provide the url of XML file and the output txt file as the two arguments'\
        '\n\nExample: python get_zdf_subtitle.py https://utstreaming.zdf.de/mtt/zdf/20/06/200609_di_neu_lot/2/logo_090620.xml zdf_subtitle.txt')

if __name__ == '__main__':
    main()
```



### 3 脚本运行：

**打开命令行**，如下图依次输入

- python
- python脚本名称
- XML的URL地址
- 输出Text文件的名称

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1591957185757-8596de2e-316c-439e-b8ec-84e8da425983.png)

输出Text文件(部分)：

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1591960704090-5873042f-856c-428c-bd0a-6461a73599e5.png)



正文完！



## 五、总结与展望

### 1 重点：定位问题比解决问题更重要——卡鲁戴斯基

爱因斯坦曾经说过：XXXXXXXXXX

今天，卡鲁**戴斯基**想说：一旦对问题的对象做出了**清晰准确的定位**，那么解决问题可以顺势而为，事半功倍，水到而渠成。



### 2 知识储备：JavaScript, XML等

如果有相关的背景知识，**问题定位**也许更快。

但**学海无涯，回头是岸**。

**主业毕竟不在此**，能够遇到问题，分析并且解决该问题即可。



### 3 心态：平和

急躁难成事，宁静而致远。

(题外话——当然遇到**恶意暴力**事件，适当急躁还是要的：靠**吼&杀气**先震住对方)



### 4 遗留问题：

Q1 如何在网站上直接提取XML文件？

https://stackoverflow.com/questions/10232774/how-to-find-sitemap-xml-path-on-websites

Q1+1 robots.txt

https://support.google.com/webmasters/answer/6062596?hl=zh-Hans

Q2 HTTP通信中：GET, POST过程的理解？



## 补充：文本格式化-换行符

### 0 回顾初始思路

```
txt = txt + line + '\n'
'''
# 思路旅顺过程
1）每一行添加换行 '\n'
但是有的行，只是因为字幕框大小的限制，没有显示在同一行
对目标-翻译来说：最好呈现连续完整的句子

2）提取整个文本(不添加符号)：没有换行符，而且第一句Linda和und写在一起(在xml文件中是两行，但读取时没有添加换行符)
而在更改的过程当中，发现句子
Ich bin Linda
und...
会直接连在一起Ich bin Lindaund...
'''
```

### 1 解决思路

```
line = subtitle.text

str_lst = [':', '.', '?', '!', '#']
if line[-1] in str_lst:
    line = line + '\n'
else:
    line = line + ' '
txt = txt + line
'''
- 提取行line
    - 如果line最后一个字符是结尾字符，那么就添加换行 '\n'
    - 除此之外，添加空格，隔开两个单词
    - 最后：将格式化的line，整合到txt中
'''
```



有任何想法，欢迎评论区讨论啊 :)

## 六、参考文献

[[1\]XML DOM教程](https://www.runoob.com/dom/dom-http.html)

[[2\]XML教程](https://www.runoob.com/xml/xml-tutorial.html)

[[3\]Logo! 2020.06.09](https://www.zdf.de/kinder/logo/logo-am-dienstagabend-104.html)

[[4\]Python解析XML ElementTree](https://docs.python.org/2/library/xml.etree.elementtree.html#module-xml.etree.ElementTree)

[[5\]Python-urllib读取URL](https://stackoverflow.com/questions/28238713/python-xml-parsing-lxml-urllib-request)

[[6\]Python I/O](https://www.cnblogs.com/ymjyqsx/p/6554817.html)