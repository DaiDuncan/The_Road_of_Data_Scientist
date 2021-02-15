# HTML | 属性download

**声明**：以下内容主要来自[cnblogs | 了解HTML/HTML5中的download属性](https://www.zhangxinxu.com/wordpress/2016/04/know-about-html-download-attribute/)，根据理解做了些许调整

## 一 起：目标

实现下载按钮：

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1592771918814-565fd87c-5e8f-49e1-a6aa-d6bbc0b92f8e.png)





## 二 承：实现思路

### 1 一个最简单的方法，按钮添加a标签链接一张图片

类似下面这样：

```
<a href="large.jpg">下载</a>
```

但是，想法虽好，实际效果却不是我们想要的，因为**浏览器可以直接浏览**图片，因此，我们点击下面的“下载”链接，并是不下载图片，而是在新窗口直接浏览图片。

于是，基本上，目前的实现都是放弃HTML策略。



### 2 php等后端语言

通过告知浏览器header信息，来实现下载。

```
header('Content-type: image/jpeg'); 
header("Content-Disposition: attachment; filename='download.jpg'"); 
```

然而，这种前后端都要操心的方式神烦，现在都流行前后端分离。

那有没有什么只需要前端动动指头就能实现下载的方式呢？



### 3 属性 download

例如，我们希望点击“下载”链接下载图片而不是浏览，直接增加一个download属性就可以：

```
<a href="large.jpg" download>下载</a>
```

备注：在Chrome浏览器下直接下载（FireFox浏览器因为跨域限制无效）

不仅如此，我们还可以指定下载图片的文件名(如果后缀名一样，我们还可以缺省)：

```
<a href="index_logo.gif" download="_5332_.gif">下载</a>
<a href="index_logo.gif" download="_5332_">下载</a>
```





## 三 转：浏览器兼容性和跨域策略

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1592772264447-ad6426dd-e20e-4768-9307-2a07b1b4b64e.png)

如果需要下载的资源是跨域的，包括跨子域，在Chrome浏览器下，使用download属性是可以下载的，但是，并不能重置下载的文件的命名；而FireFox浏览器下，则download属性是无效的，也就是FireFox浏览器无论如何都不支持跨域资源的download属性下载。



如果资源是同域名的，则两个浏览器都是畅通无阻的下载，不会出现下载变浏览的情况。



### 问题：是否支持download属性？

要检测当前浏览器是否支持download属性，一行JS代码就可以了，如下：

```
var isSupportDownload = 'download' in document.createElement('a');
```





## 四 合：回顾小结

除了图片资源，我们还可以是PDF资源，或者txt资源等等。尤其Chrome等浏览器可以直接打开PDF文件，使得此文件格式需要download处理的场景越来越普遍。



此HTML属性虽然非常实用和方便，但是兼容性制约了我们的大规模应用。

同时考虑到很多时候，需要进行一些下载的统计，纯前端的方式想要保存下载量数据，还是有些吃紧，需要跟开发的同学配合才行，还不如使用传统方法。



其实使用JS实现download属性的polyfill并不难，但是，考虑到为何不所有浏览器都使用polyfill的方法，又觉得为了技术而技术是不太妥当的。

总之，先放着心上，再观察观察。

## 参考文献

[cnblogs | 了解HTML/HTML5中的download属性](https://www.zhangxinxu.com/wordpress/2016/04/know-about-html-download-attribute/)