XMLHttpRequest 对象用于==幕后与服务器交换数据==。

XMLHttpRequest 对象是开发者的梦想

## 应用场景

Web开发：与服务器交互通信



## 特性

在不重新加载页面的情况下更新网页

在页面已加载后从服务器请求数据

在页面已加载后从服务器接收数据

在后台向服务器发送数据





# 1 创建 XMLHttpRequest 对象

所有现代的浏览器（IE7+、Firefox、Chrome、Safari 和 Opera）都有一个内建的 XMLHttpRequest 对象

```javascript
if (window.XMLHttpRequest)
{
  // IE7+, Firefox, Chrome, Opera, Safari 浏览器执行代码
  xmlhttp=new XMLHttpRequest();
}
else
{
  // IE6, IE5 浏览器执行代码
  xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
}
```



# 2 发送一个请求到服务器

使用 XMLHttpRequest 对象的 open() 和 send() 方法

```javascript
xmlhttp.open("GET","xmlhttp_info.txt",true);
xmlhttp.send();
```

| 方法                     | 描述                                                         |
| :----------------------- | :----------------------------------------------------------- |
| open(*method,url,async*) | 规定请求的类型，URL，请求是否应该进行异步处理。  *method*：请求的类型：GET 或 POST <br />*url*：文件在服务器上的位置 <br />*async*：true（异步）或 false（同步） |
| send(*string*)           | 发送请求到服务器。  <br />*string*：仅用于 POST 请求         |

1）GET 或 POST？

GET 比 POST 简单并且快速，可用于大多数情况下。

然而，下面的情况下请始终使用 POST 请求：

1 缓存的文件不是一个选项（更新**服务器上的文件**或数据库）

2 发送到服务器的**数据量较大**（==POST 没有大小的限制==）

3 发送用户输入（可以包含未知字符），POST 比 GET 更强大更安全





2）URL - 服务器上的文件

open() 方法的 url 参数，是一个**在服务器上的文件**的地址

`xmlhttp.open("GET","xmlhttp_info.txt",true);`

该文件可以是**任何类型的文件（如 .txt 和 .xml）**，或服务器脚本文件（如.html 和 .php，可在发送回响应之前在服务器上执行动作）。





3）异步 - True 或 False?

异步发送请求，open() 方法的 async 参数必需设置为 true

`xmlhttp.open("GET","xmlhttp_info.txt",true);`

发送异步请求对于 Web 开发人员是一个巨大的进步。在服务器上执行的许多任务非常费时。

通过异步发送，JavaScript 不需要等待服务器的响应，但可以替换为：

- 等待服务器的响应时，执行其他脚本
- 响应准备时处理响应





@Async=true

在 onreadystatechange 事件中**响应准备时**规定一个要执行的函数：

```javascript
xmlhttp.onreadystatechange=function()
{
  if (xmlhttp.readyState==4 && xmlhttp.status==200)
  {
    document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
  }
}
xmlhttp.open("GET","xmlhttp_info.txt",true);
xmlhttp.send();
```



@Async=false

如需使用 async=false，请更改 open() 方法的第三个参数为 false：

`xmlhttp.open("GET","xmlhttp_info.txt",false);`

不推荐使用 async=false，但如果处理几个小的请求还是可以的。

请记住，JavaScript 在服务器响应准备之前不会继续执行。如果服务器正忙或缓慢，应用程序将挂起或停止。

**注意：**当您使用 async=false 时，不要编写 onreadystatechange 函数 - 只需要把代码放置在 send() 语句之后即可：

```javascript
xmlhttp.open("GET","xmlhttp_info.txt",false);
xmlhttp.send();
document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
```





# 3 服务器响应

使用 XMLHttpRequest 对象的 responseText 或 responseXML 属性

| 属性         | 描述                                                         |
| :----------- | :----------------------------------------------------------- |
| responseText | 获取响应数据作为字符串                                       |
| responseXML  | ResponseXML 属性返回 XML 文档对象，可使用 DOM 节点树的方法和属性来检查和解析该对象。 |

## responseText

获取响应数据作为字符串

```javascript
document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
```



## responseXML

ResponseXML 属性返回 XML 文档对象，可使用 DOM 节点树的方法和属性来检查和解析该对象。

- txt在循环遍历中：不断累加

```javascript
xmlDoc=xmlhttp.responseXML;
var txt="";
x=xmlDoc.getElementsByTagName("ARTIST");
for (i=0;i<x.length;i++)
{
  txt=txt + x[i].childNodes[0].nodeValue + "";
}
document.getElementById("myDiv").innerHTML=txt;
```



@onreadystatechange 事件

当请求被发送到服务器，我们要根据响应执行某些动作。@XMLHttpRequest 对象的三个重要的属性

| 属性               | 描述                                                         |
| :----------------- | :----------------------------------------------------------- |
| onreadystatechange | 存储函数（或函数的名称）在每次 readyState 属性变化时被自动调用 |
| readyState         | 存放了 XMLHttpRequest 的状态。从 0 到 4 变化： <br />0：请求未初始化 <br />1：服务器建立连接 <br />2：收到的请求 <br />3：处理请求 <br />4：请求完成和响应准备就绪 |
| status             | 200："OK" <br />404：找不到页面                              |

在 onreadystatechange 事件中，我们规定当服务器的响应准备处理时会发生什么。

当 readyState 是 4 或状态是 200 时，响应准备：

```javascript
xmlhttp.onreadystatechange=function()
{
  if (xmlhttp.readyState==4 && xmlhttp.status==200)
  {
    document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
  }
}
```

注意：onreadystatechange 事件在每次 readyState 发生变化时被触发，总共触发了四次。





# 对象

## 属性

![image-20210220165415312](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210220165415.png)

## 方法

![image-20210220165412682](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210220165412.png)



# #参考文献

[Link: 实例|菜鸟教程](https://www.runoob.com/dom/dom-httprequest.html)