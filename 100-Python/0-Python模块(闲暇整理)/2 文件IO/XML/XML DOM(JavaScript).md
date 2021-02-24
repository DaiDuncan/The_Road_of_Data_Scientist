DOM（**Document Object Model** 文档对象模型）定义了==访问和操作文档的标准方法==。

XML DOM 把 XML 文档==作为树结构==来查看。

所有元素可以通过 DOM 树来访问。可以修改或删除它们的内容，并创建新的元素。元素，它们的文本，以及它们的属性，都被认为是节点。



| 基础知识 | HTML  <br />XML <br />JavaScript                             |
| -------- | ------------------------------------------------------------ |
| DOM      | 核心 DOM - 用于任何**结构化文档**的标准模型  <br />XML DOM - 用于 XML 文档的标准模型  <br />HTML DOM - 用于 HTML 文档的标准模型 |
|          | 定义了所有文档元素的**对象**和**属性**，以及访问**它们的方法（接口）** |
| XML DOM  | ==>用于获取、更改、添加或删除 XML 元素的标准     <br /><br />用于 XML  的标准对象模型  <br />用于 XML  的标准编程接口  <br />中立于平台和语言  <br />W3C 标准 |

## 数据源

[Link: 菜鸟教程](https://www.runoob.com/xml/xml-dom.html)

1 加载一个 XML 文件

跨浏览器实例

```xml
<html>
<body>
<h1>W3Schools Internal Note</h1>
<div>
<b>To:</b> <span id="to"></span><br />
<b>From:</b> <span id="from"></span><br />
<b>Message:</b> <span id="message"></span>
</div>

<script>
if (window.XMLHttpRequest)
{// code for IE7+, Firefox, Chrome, Opera, Safari
xmlhttp=new XMLHttpRequest();
}
else
{// code for IE6, IE5
xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
}
xmlhttp.open("GET","note.xml",false);
xmlhttp.send();
xmlDoc=xmlhttp.responseXML;

document.getElementById("to").innerHTML=
xmlDoc.getElementsByTagName("to")[0].childNodes[0].nodeValue;
document.getElementById("from").innerHTML=
xmlDoc.getElementsByTagName("from")[0].childNodes[0].nodeValue;
document.getElementById("message").innerHTML=
xmlDoc.getElementsByTagName("body")[0].childNodes[0].nodeValue;
</script>

</body>
</html>
```

如需从上面的 XML 文件（"note.xml"）的 <to> 元素中提取文本 "Tove"，语法是：

`getElementsByTagName("to")[0].childNodes[0].nodeValue`

请注意，即使 XML 文件只包含一个 <to> 元素，您仍然必须指定数组索引 [0]。这是因为 getElementsByTagName() 方法返回一个数组。





2 加载一个 XML 字符串

@跨浏览器实例

```xml
<html>
<body>
<h1>W3Schools Internal Note</h1>
<div>
<b>To:</b> <span id="to"></span><br />
<b>From:</b> <span id="from"></span><br />
<b>Message:</b> <span id="message"></span>
</div>

<script>
txt="<note>";
txt=txt+"<to>Tove</to>";
txt=txt+"<from>Jani</from>";
txt=txt+"<heading>Reminder</heading>";
txt=txt+"<body>Don't forget me this weekend!</body>";
txt=txt+"</note>";

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

document.getElementById("to").innerHTML=
xmlDoc.getElementsByTagName("to")[0].childNodes[0].nodeValue;
document.getElementById("from").innerHTML=
xmlDoc.getElementsByTagName("from")[0].childNodes[0].nodeValue;
document.getElementById("message").innerHTML=
xmlDoc.getElementsByTagName("body")[0].childNodes[0].nodeValue;
</script>
</body>
</html>
```





# DOM节点(含books.xml)

在 DOM 中，XML 文档中的每个成分都是一个节点。

- 整个文档是一个文档节点
- 每个 XML 元素是一个元素节点
- 包含在 XML 元素中的文本是文本节点
- 每一个 XML 属性是一个属性节点
- 注释是注释节点

![image-20210220150438791](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210220150438.png)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<bookstore>
    <book category="cooking">
        <title lang="en">Everyday Italian</title>
        <author>Giada De Laurentiis</author>
        <year>2005</year>
        <price>30.00</price>
    </book>
    <book category="children">
        <title lang="en">Harry Potter</title>
        <author>J K. Rowling</author>
        <year>2005</year>
        <price>29.99</price>
    </book>
    <book category="web">
        <title lang="en">XQuery Kick Start</title>
        <author>James McGovern</author>
        <author>Per Bothner</author>
        <author>Kurt Cagle</author>
        <author>James Linn</author>
        <author>Vaidyanathan Nagarajan</author>
        <year>2003</year>
        <price>49.99</price>
    </book>
    <book category="web" cover="paperback">
        <title lang="en">Learning XML</title>
        <author>Erik T. Ray</author>
        <year>2003</year>
        <price>39.95</price>
    </book>
</bookstore>
```

在上面的 XML 中，根节点是 <bookstore>。

文档中的所有其他节点都被包含在 <bookstore> 中。

 

根节点 <bookstore> 有四个 <book> 节点。

第一个 <book> 节点有四个节点：<title>、<author>、<year> 和 <price>，其中每个节点都包含一个文本节点，"Everyday Italian"、"Giada De Laurentiis"、"2005" 和 "30.00"。



@文本总是存储在文本节点中

在 DOM 处理中一个普遍的错误是，认为元素节点包含文本。

不过，元素节点的文本是存储在文本节点中的。

在这个实例中：<year>2005</year>，元素节点 <year>，拥有一个值为 "2005" 的文本节点。

"2005" **不是 <year> 元素的值**！





## 属性和方法

属性和方法向 XML DOM 定义了编程接口

DOM 把 XML 模拟为一系列节点对象。可通过 JavaScript 或其他编程语言来访问节点。

在本教程中，我们**使用 JavaScript**。

对 DOM 的编程接口是通过一套标准的属性和方法来定义的。





在 XML DOM 中，每个节点都是一个对象。

对象拥有方法和属性，并可通过 JavaScript 进行访问和操作。

| 属性 | 某事物是什么  例如节点名是 "book"    |
| ---- | ------------------------------------ |
| 方法 | 对某事物做什么  例如删除 "book" 节点 |



| 对象            |                                                              |
| --------------- | ------------------------------------------------------------ |
| 节点的属性      | nodeName  nodeValue  nodeType                                |
| nodeName 属性   | nodeName  是只读的  元素节点的 nodeName 与**标签名**相同  属性节点的  nodeName 是属性的名称  文本节点的  nodeName 永远是 #text  文档节点的  nodeName 永远是 #document |
| nodeValue  属性 | 元素节点的  nodeValue 是 undefined  文本节点的  nodeValue 是文本本身  属性节点的  nodeValue 是属性的值 |
|                 |                                                              |
|                 |                                                              |

@获取元素的值

- 使用 loadXMLDoc() 把 "books.xml" 载入 xmlDoc 中
- 获取第一个 <title> 元素节点的文本节点
- 把 txt 变量设置为文本节点的值

```javascript
xmlDoc=loadXMLDoc("books.xml");
 
x=xmlDoc.getElementsByTagName("title")[0].childNodes[0];
txt=x.nodeValue;


// 结果：
txt = "Everyday Italian"
```



@更改元素的值

1 使用 loadXMLDoc() 把 "books.xml" 载入 xmlDoc 中

2 获取第一个 <title> 元素节点的文本节点

3 更改文本节点的值为 "Easy Cooking"

```javascript
xmlDoc=loadXMLDoc("books.xml");
 
x=xmlDoc.getElementsByTagName("title")[0].childNodes[0];
x.nodeValue="Easy Cooking";
```



@nodeType 属性

nodeType 属性规定节点的类型。

nodeType 是只读的。

| 节点类型 | NodeType |
| :------- | :------- |
| 元素     | 1        |
| 属性     | 2        |
| 文本     | 3        |
| 注释     | 8        |
| 文档     | 9        |







### XML DOM 属性

```javascript
// x是一个节点对象
x.nodeName - x 的名称
x.nodeValue - x 的值
x.parentNode - x 的父节点
x.childNodes - x 的子节点

x.attributes - x 的属性节点
```



### XML DOM方法

```javascript
x.getElementsByTagName(name) //获取带有指定标签名称的所有元素
x.appendChild(node) //向 x 插入子节点
x.removeChild(node) //从 x 删除子节点
```

 



### 实例

- xmlDoc - 由解析器创建的 XML DOM 对象
- getElementsByTagName("title")[0] - 第一个 <title> 元素
- childNodes[0] - <title> 元素的第一个子节点（文本节点）
- nodeValue - 节点的值（文本本身）

```javascript
// 源文件：books.xml
// 从 books.xml 中的 <title> 元素获取文本的 JavaScript 代码：
txt=xmlDoc.getElementsByTagName("title")[0].childNodes[0].nodeValue	// 在该语句执行后，txt 保存的值是 "Everyday Italian"。
```





## 访问节点

通过 DOM，您能够访问 XML 文档中的每个节点。

### 1 通过使用 getElementsByTagName() 

```javascript
node.getElementsByTagName("tagname");	//返回拥有指定标签名的所有元素


// 下面的实例返回 x 元素下的所有 <title> 元素：
x.getElementsByTagName("title");

// 请注意，上面的实例仅返回 x 节点下的 <title> 元素。如需返回 XML 文档中的所有 <title> 元素，请使用：
xmlDoc.getElementsByTagName("title");	//xmlDoc 就是文档本身（文档节点）
```

使用 loadXMLDoc() 把 "books.xml" 载入 xmlDoc 中，然后在变量 x 中存储 <title> 节点的一个列表

xmlDoc=loadXMLDoc("books.xml");

x=xmlDoc.getElementsByTagName("title");

 

通过索引号访问 x 中的 <title> 元素。如需访问第三个 <title>，您可以编写：

y=x[2];

注意：该索引从 0 开始。



### 2 通过循环（遍历）节点树

@节点列表长度（Node List Length）==（即节点的数量）

1 使用 loadXMLDoc() 把 "books.xml" 载入 xmlDoc 中

2 获取所有 <title> 元素节点

3 输出每个 <title> 元素的文本节点的值

```javascript
xmlDoc=loadXMLDoc("books.xml");
 
x=xmlDoc.getElementsByTagName("title");
 
for (i=0;i<x.length;i++)
{
  document.write(x[i].childNodes[0].nodeValue);
  document.write("");
}
```

@节点类型（Node Types）

XML 文档的 documentElement 属性是根节点。

节点的 nodeName 属性是节点的名称。

节点的 nodeType 属性是节点的类型。



1 使用 loadXMLDoc() 把 "books.xml" 载入 xmlDoc 中

2 获取根元素的子节点

3 检查每个子节点的节点类型。如果节点类型是 "1"，则是元素节点

4 如果是元素节点，则输出节点的名称

```javascript
xmlDoc=loadXMLDoc("books.xml");
 
x=xmlDoc.documentElement.childNodes;
 
for (i=0;i<x.length;i++)
{
  if (x[i].nodeType==1)
  {
    // 执行一次
    document.write(x[i].nodeName);
    document.write("");
  }
}
```



### 3 通过利用节点的关系在节点树中导航

1 使用 loadXMLDoc() 把 "books.xml" 载入 xmlDoc 中

2 获取第一个 book 元素的子节点

3 把 "y" 变量设置为第一个 book 元素的第一个子节点

4 对于每个子节点（第一个子节点从 "y" 开始），检查节点类型，如果节点类型为 "1"，则是元素节点

5 如果是元素节点，则输出该节点的名称

6 把 "y" 变量设置为下一个同级节点，并再次运行循环

```javascript
xmlDoc=loadXMLDoc("books.xml");
 
x=xmlDoc.getElementsByTagName("book")[0].childNodes;
y=xmlDoc.getElementsByTagName("book")[0].firstChild;
 
for (i=0;i<x.length;i++)
{
  if (y.nodeType==1)
  {
  // 输出节点名
  document.write(y.nodeName + "");
  }
  y=y.nextSibling;
}
```





### 实例|访问节点

[Link: 菜鸟教程](https://www.runoob.com/dom/dom-nodes-access.html)

#### 0 XML文档

```xml
<bookstore>
<book category="cooking">
<title lang="en">Everyday Italian</title>
<author>Giada De Laurentiis</author>
<year>2005</year>
<price>30.00</price>
</book>
<book category="children">
<title lang="en">Harry Potter</title>
<author>J K. Rowling</author>
<year>2005</year>
<price>29.99</price>
</book>
<book category="web">
<title lang="en">XQuery Kick Start</title>
<author>James McGovern</author>
<author>Per Bothner</author>
<author>Kurt Cagle</author>
<author>James Linn</author>
<author>Vaidyanathan Nagarajan</author>
<year>2003</year>
<price>49.99</price>
</book>
<book category="web" cover="paperback">
<title lang="en">Learning XML</title>
<author>Erik T. Ray</author>
<year>2003</year>
<price>39.95</price>
</book>
</bookstore>
```





#### 1 加载XML文件

函数 loadXMLDoc()，位于**外部** **JavaScript** 中，用于加载 XML 文件

```javascript
function loadXMLDoc(dname)
{
    if (window.XMLHttpRequest)
    {
        xhttp=new XMLHttpRequest();
    }
    else
    {
        xhttp=new ActiveXObject("Microsoft.XMLHTTP");
    }
    xhttp.open("GET",dname,false);
    xhttp.send();
    return xhttp.responseXML;
}
```

为了使上述代码更容易维护，以确保在所有页面中使用相同的代码，我们把函数存储在一个外部文件中。

文件名为 "loadxmldoc.js"，且在 HTML 页面中的 head 部分被加载。然后，页面中的脚本调用 loadXMLDoc() 函数。

```javascript
<html>
<head>
<script src="loadxmldoc.js">
</script>
</head>
<body>
 
<script>
xmlDoc=loadXMLDoc("books.xml");
 
code goes here.....
 
</script>
 
</body>
</html>
```





#### 2 访问节点

可以通过三种方式来访问节点：

1. 通过使用 getElementsByTagName() 方法。

2. 通过循环（遍历）节点树。

3. 通过利用节点的关系在节点树中导航。

```javascript
//格式：node.getElementsByTagName("tagname");

<!DOCTYPE html>
<html>
<head>
<script src="loadxmldoc.js"></script>
</head>
<body>

<script>

xmlDoc=loadXMLDoc("books.xml");
x=xmlDoc.getElementsByTagName("title");
document.write(x[2].childNodes[0].nodeValue);

</script>
</body>
</html>

//结果
XQuery Kick Start
```





#### 3 使用 length 属性来遍历节点

使用 **length 属性**来遍历 "books.xml" 中的所有 <title> 元素

```javascript
<!DOCTYPE html>
<html>
<head>
<script src="loadxmldoc.js"></script>
</head>
<body>

<script>
xmlDoc=loadXMLDoc("books.xml");

x=xmlDoc.getElementsByTagName("title");
for (i=0;i<x.length;i++)
{ 
	document.write(x[i].childNodes[0].nodeValue);
	document.write("<br>");
}
</script>
</body>
</html>


//输出结果
Everyday Italian
Harry Potter
XQuery Kick Start
Learning XML
```





#### 4 查看元素的节点类型

使用 nodeType 属性来获取 "books.xml" 中根元素的节点类型

```javascript
<!DOCTYPE html>
<html>
<head>
<script src="loadxmldoc.js"></script>
</head>
<body>

<script>
xmlDoc=loadXMLDoc("books.xml");

document.write(xmlDoc.documentElement.nodeName);
document.write("<br>");
document.write(xmlDoc.documentElement.nodeType);
</script>
</body>
</html>

//结果
bookstore
1
```



#### 5 遍历元素节点

```javascript
<!DOCTYPE html>
<html>
<head>
<script src="loadxmldoc.js"></script>
</head>
<body>

<script>
xmlDoc=loadXMLDoc("books.xml");

x=xmlDoc.documentElement.childNodes;
for (i=0;i<x.length;i++)
{ 
	if (x[i].nodeType==1)
	{
	    //Process only element nodes (type 1) 
	    document.write(x[i].nodeName);
	    document.write("<br>");
	} 
}
</script>
</body>
</html>

//结果
book
book
book
book
```





#### 6 使用节点的关系来遍历元素节点

使用 nodeType 属性和 nextSibling 属性来处理 "books.xml" 中的元素节点

```javascript
<!DOCTYPE html>
<html>
<head>
<script src="loadxmldoc.js"></script>
</head>
<body>

<script>
xmlDoc=loadXMLDoc("books.xml");

x=xmlDoc.getElementsByTagName("book")[0].childNodes;
y=xmlDoc.getElementsByTagName("book")[0].firstChild;
for (i=0;i<x.length;i++)
{
    if (y.nodeType==1)
    {
        // 输出节点名
        document.write(y.nodeName + "<br>");
    }
    y=y.nextSibling;
}
</script>
</body>
</html>

//结果
title
author
year
price
```





# DOM解析

大多数浏览器都内建了供读取和操作 XML 的 XML 解析器。

解析器把 XML 转换为 JavaScript 可存取的对象（XML DOM）。

 

XML DOM 包含了遍历 XML 树，访问、插入及删除节点的方法（函数）。

然而，在访问和操作 XML 文档之前，它必须**加载到 XML DOM 对象**。

 

XML 解析器读取 XML，并把它转换为 XML DOM 对象，这样才可以使用 JavaScript 访问它。

大多数浏览器有一个**内建的 XML 解析器**。



## 1 加载 XML 文档

@JavaScript 片段

- 创建一个 XMLHTTP 对象
- 打开 XMLHTTP 对象：.xml文件
- 发送一个 XML HTTP 请求到服务器
- 设置响应为 XML DOM 对象

```javascript
if (window.XMLHttpRequest)
{
  // IE7+, Firefox, Chrome, Opera, Safari 浏览器执行代码
  xhttp=new XMLHttpRequest();
}
else
{
  // IE6, IE5 浏览器执行代码
  xhttp=new ActiveXObject("Microsoft.XMLHTTP");
}
xhttp.open("GET","books.xml",false);
xhttp.send();
xmlDoc=xhttp.responseXML;
```



## 2 加载 XML 字符串

```javascript
if (window.DOMParser)
{
  parser=new DOMParser();
  xmlDoc=parser.parseFromString(text,"text/xml");
}
else
{
   // Internet Explorer
  xmlDoc=new ActiveXObject("Microsoft.XMLDOM");
  xmlDoc.async=false;
  xmlDoc.loadXML(text); 
}
```

注意：Internet Explorer 使用 loadXML() 方法来解析 XML 字符串，而其他浏览器使用 DOMParser 对象。



注意：

出于安全原因，现代的浏览器不允许跨域访问。

这意味着，网页以及 XML 文件，它必须==位于同一台服务器上尝试加载==。

菜鸟教程上的实例中所有打开的 XML 文件都是位于菜鸟教程域上的。

如果您想要在您的网页上使用上面的实例，您加载的 XML 文件必须位于您自己的服务器上。





# DOM加载函数

加载 XML 文档中的代码

```javascript
function loadXMLDoc(dname)	//loadxmldoc.js文件
{
    if (window.XMLHttpRequest)
    {
        xhttp=new XMLHttpRequest();
    }
    else
    {
        xhttp=new ActiveXObject("Microsoft.XMLHTTP");
    }
    xhttp.open("GET",dname,false);
    xhttp.send();
    return xhttp.responseXML;
}
```

上面的函数**可以存储在 HTML 页面的 <head> 部分**，并从页面中的脚本调用。



```javascript
function loadXMLString(txt) 	//loadxmlstring.js文件
{
    if (window.DOMParser)
    {
        parser=new DOMParser();
        xmlDoc=parser.parseFromString(txt,"text/xml");
    }
    else 
    {
        // Internet Explorer
        xmlDoc=new ActiveXObject("Microsoft.XMLDOM");
        xmlDoc.async=false;
        xmlDoc.loadXML(txt); 
    }
    return xmlDoc;
}
```

上面的函数可以存储在 HTML 页面的 <head> 部分，并从页面中的脚本调用。





@loadXMLString() 的**外部** JavaScript

已经把 loadXMLString() 函数存储在名为 "loadxmlstring.js" 文件中

```javascript
<html>
<head>
<script src="loadxmlstring.js"></script>
</head>
<body>
<script>
text="<bookstore>"
text=text+"<book>";
text=text+"<title>Everyday Italian</title>";
text=text+"<author>Giada De Laurentiis</author>";
text=text+"<year>2005</year>";
text=text+"</book>";
text=text+"</bookstore>";
 
xmlDoc=loadXMLString(text);
 
code goes here.....
</script>
</body>
</html>
```





# 总结

1 XML DOM 定义了访问和操作 XML 的标准。

2 根据 DOM，XML 文档中的一切是一个节点。

3 元素节点中的**文本存储在一个文本节点**中。

4 XML DOM 把 XML 文档视为树结构。树结构被称为节点树。

5 在节点树中，父级、子级和同级是用来描述关系。

6 所有现代的浏览器都有内建的 XML 解析器，可用于读取和操作 XML。

7 通过 XML DOM 属性和方法，您可以访问 XML 文档中的每个节点。

8 ==重要节点属性==：nodeName、nodeValue 和 nodeType。

9 当使用像 childNodes 或 getElementsByTagName() 的属性或方法时，返回节点列表对象。

10 不同的浏览器处理节点之间的**换行或空格字符**时是不同的。

11 如需忽略元素节点间的空文本字节，您可以检查节点类型。

12 节点可以使用节点的关系进行导航。

13 我们的 XML DOM 实例也表示了 XML DOM 教程的一个总结。



[Link: DOM实例](https://www.runoob.com/dom/xml-dom-examples.html)

[Link: DOM验证语法有效性](https://www.runoob.com/dom/dom-validate.html)

# #参考文献

[Link: XML DOM 教程](https://www.runoob.com/dom/dom-tutorial.html  )

[Link: HTML DOM教程](https://www.runoob.com/htmldom/htmldom-tutorial.html   )