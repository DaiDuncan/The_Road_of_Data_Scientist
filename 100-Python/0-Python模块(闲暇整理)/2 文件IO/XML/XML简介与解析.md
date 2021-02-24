| 资源       |                                                              |
| ---------- | ------------------------------------------------------------ |
| python文档 | https://docs.python.org/2/library/xml.etree.elementtree.html#parsing-xml-with-namespaces |

XML 的设计宗旨是==传输数据==，而不是显示数据。

- HTML 旨在显示信息，而 XML 旨在传输信息。

XML 标签没有被预定义。您需要==自行定义标签==。

XML 是 W3C 的推荐标准。

XML 是各种应用程序之间进行**数据传输的最常用的工具**。



| 语法           | https://www.cnblogs.com/weiyouqing/p/10662177.html           |
| -------------- | ------------------------------------------------------------ |
| 一般语法规则   | XML文档必须有根元素  <br />XML文档必须有关闭标签  <br />XML标签对大小写敏感  <br />XML元素必须被正确的嵌套  <br />XML属性必须加引号 |
| 特性：命名空间 | xlmns:XXX                                                    |



# HTML与XML

[Link: 在 HTML 页面中显示 XML 数据](https://www.runoob.com/xml/xml-to-html.html)



# XML简介

如果您需要在 HTML 文档中==显示动态数据==，那么每当数据改变时将花费大量的时间来编辑 HTML。

通过 XML，数据能够存储在独立的 XML 文件中。这样您就可以专注于使用 HTML/CSS 进行显示和布局，并确保修改底层数据不再需要对 HTML 进行任何的改变。

 

通过使用几行 JavaScript 代码，您就可以读取一个外部 XML 文件，并更新您的网页的数据内容。



## 数据结构

|        |                                                              |
| ------ | ------------------------------------------------------------ |
| 树结构 | 必须包含根元素。<br>该元素是所有其他元素的父元素  <br><root>  <br /><child>  <br /><subchild>.....</subchild>  <br /></child>  <br /></root> |
|        | ![image-20210220114855302](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210220114855.png) |
|        | 实例中的根元素是  <bookstore>。<br />文档中的所有 <book> 元素都被包含在 <bookstore> 中。     <br /><book>  元素有 4 个子元素：<title>、<author>、<year>、<price>。 |

```XML
<bookstore>
    <book category="COOKING">
        <title lang="en">Everyday Italian</title>
        <author>Giada De Laurentiis</author>
        <year>2005</year>
        <price>30.00</price>
    </book>
    <book category="CHILDREN">
        <title lang="en">Harry Potter</title>
        <author>J K. Rowling</author>
        <year>2005</year>
        <price>29.99</price>
    </book>
    <book category="WEB">
        <title lang="en">Learning XML</title>
        <author>Erik T. Ray</author>
        <year>2003</year>
        <price>39.95</price>
    </book>
</bookstore>
```



@实体引用：某些字符的“转义”

|        |      |                 |
| ------ | ---- | --------------- |
| &lt;   | <    | less than       |
| &gt;   | >    | greater  than   |
| &amp;  | &    | ampersand       |
| &apos; | '    | apostrophe      |
| &quot; | "    | quotation  mark |

注释：在 XML 中，只有字符 "<" 和 "&" 确实是非法的。大于号是合法的，但是用实体引用来代替它是一个好习惯。



@格式：换行

在 Windows 应用程序中，换行通常以一对字符来存储：回车符（CR）和换行符（LF）。

在 Unix 和 Mac OSX 中，使用 LF 来存储新行。

在旧的 Mac 系统中，使用 CR 来存储新行。

XML 以 LF 存储换行。



@理念

理念是：元数据（有关数据的数据）应当存储为属性，而数据本身应当存储为元素

 





## 结合CSS

使用 CSS 格式化 XML 不是常用的方法。

W3C 推荐使用 XSLT，请看下一章。

![image-20210220115208088](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210220115208.png)





# XML解析

## XMLHttpRequest 对象

| XMLHttpRequest 对象       | 用于在后台与服务器交换数据                                   |
| ------------------------- | ------------------------------------------------------------ |
| 是开发者的梦想            | 在**不重新加载**页面的情况下更新网页  <br />在页面**已加载后**从服务器请求数据  <br />在页面已加载后从服务器接收数据  <br />在后台向服务器发送数据 |
| @实例                     | 在下面的输入字段中键入一个字符，一个  **XMLHttpRequest** 发送到服务器 -  返回名称的建议（从服务器上的文件）：    <br />![image-20210220135122612](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210220135122.png) |
| @创建 XMLHttpRequest 对象 | 所有现代浏览器（IE7+、Firefox、Chrome、Safari  和 Opera）都有内建的 XMLHttpRequest 对象 |

```xml
<!-- 创建  XMLHttpRequest 对象的语法：   -->
xmlhttp=new XMLHttpRequest();     

<!-- 旧版本的Internet  Explorer（IE5和IE6）中使用 ActiveX 对象：   -->
xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");  
```

 

## XML Parser解析器

所有现代浏览器都有内建的 XML 解析器。

把 XML 文档转换为 XML DOM 对象 - 可通过 JavaScript 操作的对象

```javascript
// 解析 XML 文档
if (window.XMLHttpRequest)
{// code for IE7+, Firefox, Chrome, Opera, Safari
xmlhttp=new XMLHttpRequest();
}
else
{// code for IE6, IE5
xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
}
xmlhttp.open("GET","books.xml",false);
xmlhttp.send();
xmlDoc=xmlhttp.responseXML;




// 解析 XML 字符串 
// 把 XML 字符串解析到 XML DOM 对象中
txt="<bookstore><book>";
txt=txt+"<title>Everyday Italian</title>";
txt=txt+"<author>Giada De Laurentiis</author>";
txt=txt+"<year>2005</year>";
txt=txt+"</book></bookstore>";

if (window.DOMParser)
{
parser=new DOMParser();
xmlDoc=parser.parseFromString(txt,"text/xml");
}
else // Internet Explorer
{
xmlDoc=new ActiveXObject("Microsoft.XMLDOM");
xmlDoc.async=false;
xmlDoc.loadXML(txt);
}


```

注释：Internet Explorer 使用 loadXML() 方法来解析 XML 字符串，而其他浏览器使用 DOMParser 对象。

 

\#跨域访问

出于安全方面的原因，现代的浏览器不允许跨域的访问。

这意味着，网页以及它试图加载的 XML 文件，都必须位于相同的服务器上。





## 命名空间

当在 XML 中使用前缀时，一个所谓的==用于前缀的命名空间==必须被定义。

命名空间是在元素的开始标签的 xmlns 属性中定义的。

命名空间声明的语法如下。xmlns:前缀="URI"。

![image-20210220135516064](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210220135516.png)



命名空间，可以在他们被使用的元素中或者在 XML 根元素中声明：

![image-20210220135542779](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210220135542.png)

注释：命名空间 URI 不会被解析器用于查找信息。

其目的是赋予命名空间一个惟一的名称。不过，很多公司常常会==作为指针来使用命名空间指向实际存在的网页==，这个网页包含关于命名空间的信息。







### 实际使用中的命名空间

XSLT 是一种用于把 XML 文档转换为其他格式的 XML 语言，比如 HTML。



在下面的 XSLT 文档中，您可以看到，大多数的标签是 HTML 标签。

非 HTML 的标签==都有前缀 xsl==，并由此命名空间标识：xmlns:xsl="http://www.w3.org/1999/XSL/Transform"：

![image-20210220135653035](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210220135653.png)





### 应用

[Link: 解析含有命名空间的元素](https://blog.csdn.net/hanchaoqi/article/details/41701443)





## 示例|解析

### 数据源文件

```xml
<?xml version="1.0"?>
<data>
    <country name="Liechtenstein">
        <rank>1</rank>
        <year>2008</year>
        <gdppc>141100</gdppc>
        <neighbor name="Austria" direction="E"/>
        <neighbor name="Switzerland" direction="W"/>
    </country>
    <country name="Singapore">
        <rank>4</rank>
        <year>2011</year>
        <gdppc>59900</gdppc>
        <neighbor name="Malaysia" direction="N"/>
    </country>
    <country name="Panama">
        <rank>68</rank>
        <year>2011</year>
        <gdppc>13600</gdppc>
        <neighbor name="Costa Rica" direction="W"/>
        <neighbor name="Colombia" direction="E"/>
    </country>
</data>
```



[Link: 示例代码](https://docs.python.org/2/library/xml.etree.elementtree.html#parsing-xml-with-namespaces)

```python
###来自文件
import xml.etree.ElementTree as ET
tree = ET.parse('country_data.xml')
root = tree.getroot()



# 来自字符串
root = ET.fromstring(country_data_as_string)
```





```python
### 解析XML
root.tag
root.attrib

for child in root:
	print child.tag, child.attrib
    
root[0][1].text
    
    
    
# 查找目标元素Element ==>标签tag
# Element.iter()  针对具体的元素：iter遍历
>>> for neighbor in root.iter('neighbor'):
...     print neighbor.attrib
...
{'name': 'Austria', 'direction': 'E'}
{'name': 'Switzerland', 'direction': 'W'}
{'name': 'Malaysia', 'direction': 'N'}
{'name': 'Costa Rica', 'direction': 'W'}
{'name': 'Colombia', 'direction': 'E'}


# Element.findall()  #针对
>>> for country in root.findall('country'):
...     rank = country.find('rank').text
...     name = country.get('name')
...     print name, rank
...
Liechtenstein 1
Singapore 4
Panama 68



```

| 其他方法       |                        |
| -------------- | ---------------------- |
| Element.find() | 找第一个符合要求的对象 |
| Element.text   |                        |
| Element.get()  | 包含元素的属性         |



### [Link: 基于XPath](https://docs.python.org/2/library/xml.etree.elementtree.html#xpath-support)

将XML种标签的组织关系看作文件路径的关系

```python
import xml.etree.ElementTree as ET

root = ET.fromstring(countrydata)

# Top-level elements
root.findall(".")

# All 'neighbor' grand-children of 'country' children of the top-level
# elements
root.findall("./country/neighbor")

# Nodes with name='Singapore' that have a 'year' child
root.findall(".//year/..[@name='Singapore']")

# 'year' nodes that are children of nodes with name='Singapore'
root.findall(".//*[@name='Singapore']/year")

# All 'neighbor' nodes that are the second child of their parent
root.findall(".//neighbor[2]")
```

| 符号标记          |                                                              |
| ----------------- | ------------------------------------------------------------ |
| tag               | 选择所有相关的tag  比如spam选择所有名为spam的子元素：那么spam/egg选择所有spam的子元素egg(全局上看是孙子元素) |
| *                 | 选择所有的子元素  */egg表示所有的孙子元素egg                 |
| .                 | 当前节点                                                     |
| //                | 所有的子元素  .//egg表示所有的egg——>iter()     root.findall() #表示root节点为当前节点 |
| ..                | 父元素                                                       |
| [@attrib]         | 条件筛选：具有该属性attrib                                   |
| [@attrib='value'] | 条件筛选                                                     |
| [tag]             | 条件筛选                                                     |
| [tag='text']      | 条件筛选                                                     |
| [position]        | 1表示起始位置  last()表示最后的位置  last()-1表示相对于最后位置的前一个位置 |





# 总结

XML 可用于交换、共享和存储数据。

XML 文档形成 树状结构，在"根"和"叶子"的分支机构开始的。

XML 有非常简单的 语法规则。带有正确语法的 XML 是"形式良好"的。有效的 XML 是针对 DTD 进行验证的。





==所有现代的浏览器有一个内建的 XML 解析器，可读取和操作XML==。

- XSLT 用于把 XML 转换为其他格式，比如 HTML。

- DOM（Document Object Model）定义了一个访问 XML 的标准方式。

- XMLHttpRequest 对象提供了一个网页加载后与服务器进行通信的方式。

- XML 命名空间提供了一种**避免元素命名冲突**的方法。

- CDATA 区域内的文本会被解析器忽略。



 

\#下一步学习什么呢？

我们推荐学习 XML DOM 和 XSLT。

如果您想要学习有关验证 XML 的知识，我们推荐学习 DTD 和 XML Schema。

|                                   |                                                              |
| --------------------------------- | ------------------------------------------------------------ |
| XML  DOM（Document Object Model） | 定义了一种访问和处理  <br />XML 文档的标准方式 是平台和语言独立的，可用于任何编程语言，如  Java、JavaScript 和 VBScript <br />https://www.runoob.com/dom/dom-tutorial.html |
| XSLT（XML  样式表语言转换）       | XSLT 是 XML  文件的样式表语言。@HTML与CSS  <br />通过使用  XSLT，可以把 XML 文档转换为其他格式，比如 XHTML  <br />https://www.runoob.com/xsl/xsl-tutorial.html |
| XML  DTD（文档类型定义）          | 目的是定义 XML  文档中合法的元素、属性和实体。  <br />通过使用 DTD，每个  XML 文件可以随身携带它自己的格式的描述。  DTD  可以被用来确认您收到的数据和您自己的数据是否有效。  <br />https://www.runoob.com/dtd/dtd-tutorial.html |
| XML Schema                        | 是一种基于 XML 的  DTD 替代。  <br />不像 DTD，XML  Schema 支持数据类型，且使用 XML 语法。  https://www.runoob.com/schema/schema-tutorial.html |

