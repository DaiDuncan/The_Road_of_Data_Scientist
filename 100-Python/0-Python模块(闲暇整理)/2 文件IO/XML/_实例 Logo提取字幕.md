## 网页selector

\#zdf-player-jbwoe417 > div.zdfplayer-subtitle-wrapper.zdfplayer-subtitles-n > div > div > div > div > p

![image-20210220170423137](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210220170423.png)

# 目标|找到字幕文件的src

| 过程               |                                            |
| ------------------ | ------------------------------------------ |
| 1 动态页面         | 爬虫无法提取流动字幕：静态提取requests失败 |
| 2 封闭API          | @对比  TED可直接下载字幕：开放API          |
| 3 查看HTML相关代码 | 找出关键要素：xmlns——> XML                 |
| 4 XML解析          |                                            |

![image-20210220170410689](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210220170410.png)

@附加问题

视频播放和字母时间是如何联系起来的？



## 补充|探索过程

### 猜测：<head>

head中含有大量的script: 可能是相关的JS文件



### 从关键属性入手

```html
<p xmlns:tt="http://www.w3.org/ns/ttml" xml:id="sub0" data-style="textCenter" region="bottom" begin="00:00:06.680" end="00:00:09.720">
        <span data-style="textWhite" style="background-color: rgba(0, 0, 0, 0.76); color: rgb(255, 255, 255);">Es ist Dienstag, der 9. Juni,</span>
        <br>
        <span data-style="textWhite" style="background-color: rgba(0, 0, 0, 0.76); color: rgb(255, 255, 255);">ihr seid bei "logo!".</span>
      </p>
```





http://www.w3.org/ns/ttml   xmlns默认的命名空间namespace

|                            |                                                |
| -------------------------- | ---------------------------------------------- |
| <div> 属性\|格式控制 class |                                                |
| <p> 特别属性               | xmlns:tt  <br />xml:id  <br />begin  <br />end |





## 最终实现：退而求其次

XML文件中有对应的URL

![image-20210220170626487](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210220170626.png)



