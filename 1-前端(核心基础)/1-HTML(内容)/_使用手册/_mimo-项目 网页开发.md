# 主页设计1

- header
- navigation bar
- content area
- footer: 链接到社交媒体，介绍作者



### index.html

```html
<!DOCTYPE html>
<html>
    <head>
        <title>My blog</title>
    </head>
    
    <body>
        <div id="header">
            <div class="container">
                <a href="index.html">My Blog</a>
                <ul>
                    <li><a href="about.html">About</a></li>
                    <li><a href="contact.html">Contact</a></li>
                </ul>
            </div>
        </div> 

        <div id="content">
            <div class="container">
                <div class="post">
                    <div class="post-author">
                        <img src="me.png">
                        <span>Me</span>
                    </div>
                    <p class="post-date">Today</p>
                    <h3 class="post-title">Becoming a web developer</h3>
                    <div class="post-content">
                        <p>If somebody would have told me a week ago that I would be able to make a website, I would not have believed them.</p>
                        <p>Just a few days later, I came across Mimo and started to work on a real blog.</p>
                    </div>
                </div>
            </div>
        </div> 

        <div id="footer">
            <div class="container">
                <div class="column">
                    <h4>My Links</h4>
                    <p>
                        <a href="https://twitter.com/me">Twitter</a><br>
                        <a href="https://facebook.com/me">Facebook</a>
                    </p>
                </div>
                <div class="column">
                    <h4>My Story</h4>
                    <p>Hi there! I'm an aspiring web developer.</p>
                </div>
            </div>
        </div> 
    </body>
</html>
```



### about.html

```html
<ul class="navbar-links">
    <li><a href="about.html">About</a></li>
    <li><a href="mailto:me@me.com">Contact</a></a></li>
</ul>

<!-- 分割线  -->
		<div id="content">
            <div class="container">
                <div class="about">
                    <div class="about-author">
                        <img src="me.png">
                    </div>

                    <h1 class="about-title">Becoming a web developer</h1>
                    <div class="about-content">
                        <p>Hi there! I'm an aspiring web developer/</p>
                        <p>If you like my blog, please follow me on Facebook or twitter and tell your family and friends about it. If you want to get in touch, please click on the contact link in the navigation bar.</p>
                    </div><!-- about-content  -->
                </div><!-- about  -->
            </div><!-- container  -->
        </div><!-- content  -->
```





# 主页设计2

## HTML

```html
<!DOCTYPE html>
<html>
    <head>
        <title>My blog</title>
        <link rel="stylesheet" type="text/css" href="styles.css">
    </head>

    <body>
        <div id="header">
            <div class="post">
                <a id="header-title" href="index.html">My Blog</a>
                <ul id="header-nav">
                    <li><a href="about.html">About</a></li>
                    <li><a href="mailto:daidinggen@hotmail.com">Contact</a></li>
                </ul>
            </div>
        </div>

        <div id="content">
            <div class="post-container">
                <div class="post">
                    <div class="post-author">
                        <img src="me.png">
                        <span>Me</span>
                    </div>
                    <p class="post-date">Today</p>
                    <h3 class="post-title">Becoming a web developer</h3>
                    <div class="post-content">
                        <p>
                            If somebody would have told me a week ago that I would be able to make a website, I would not have believed them.
                        </p>
                        <p>
                            Just a few days later, I came across Mimo and started to work on a <em>real</em> blog.
                        </p>
                    </div>
                </div>
            </div>
        </div>

        <div id="footer">
            <div class="container">
                <div class="column">
                    <h4>My Links</h4>
                    <p>
                        <a href="https://twitter.com/me">Twitter</a><br>
                        <a href="https://facebook.com/me">Facebook</a>
                    </p>
                </div>

                <div class="column">
                    <h4>My Story</h4>
                    <p>Hi there! I'm an aspiring web developer.</p>
                </div>
            </div>
        </div>
    </body>
</html>

```



## 处理header

- 颜色

- 字体

- 高度、宽度

- 边界

```css
body {
    color: #555;
    matgin: 0;
}

#header {
    background-color: #1abc9c;
    height: 150px;
    line-height: 150px;
}

#header a {
    color: white;
    text-decoration: none;

    text-transform: uppercase;
    letter-spacing: : 0.1em;
}

/* 移动到链接上面：变成黑色 */
#header a:hover {
    color: #222;
}

#header-title {
    display: block;
    float: left;

    font-size:20px;
    font-weight: bold;
}

#header-nav {
    display: block;
    float: right;
    margin-top: 0;
}

#header-nav li {
    display: inline;
    padding-left: 20px;
}


/* 用来处理元素的水平位置 */
.container {
    max-width: 1000px;
    margin: 0 auto;
}

```



## 处理footer

没有固定的高度

```css
#footer {
    background-color: #2f2f2f;
    padding: 50px 0;
}

#footer h4 {
    color: white;
    text-transform: uppercase;
    letter-spacing: 0.1em;
}

#footer p {
    color: white;
}

.column {
    min-width: 300px;
    display: inline-block;
    vertical-align: top;
}

a {
    color: #1abc9c;
    text-decoration: none;
}

/* 移动到链接上面：变成土黄色 */
a:hover {
    color: #F6A623;
}
```





## 处理content

在css开头`@import url(http://fonts.googleapis.com/css?family=Montserrat:400, 700);`导入web的字体

```css
/* 设置内容最大宽度，位置居中 */
.post {
    max-width: 800px;
    margin: 0 auto;
    padding: 60px 0;
}

.post-author img {
    width: 50px;
    height: 50px;
    vertical-align: middle;
}

.post-author span {
    margin-left: 16px;
}


.post-date {
    color: #D2D2D2;

    font-size: 14px;
    font-weight: bold;

    text-transform: uppercase;
    letter-spacing: 0.1em;
}

h1, h2, h3, h4 {
    color: #333;
}

p {
    line-height: 1.5;
}


body {
    margin: 0;
    color: #555;

    font-family: 'Montserrat', san-serif;
}


.post, .about {
    max-width: 800px;
    margin: 0 auto;
    padding: 60px 0;
}

.about {
    text-align: center;
}

.post-author img, .about-author img {
    border-radius: 50%;
}


.post-container:nth-child(even) {
    background-color:  #f2f2f2;
}
```







# 部署到github

[Link: github部署指南](https://docs.github.com/en/github/working-with-github-pages/creating-a-github-pages-site)

最终的blog网页：`https://github.com/DaiDuncan/DaiDuncan.github.io`

# CSS

- interneal style sheet
- external style sheet：分离HTML内容和CSS格式

```html
<!doctype html>
<html>
    <head>
        <!-- interneal style sheet -->
        <style>		
            body{
                backgroud-color: #00cd6f;
                color: #ffffff;
            }
            p{
                color: blue;
                font-size: 14px;
            }
        </style>
    </head>
    <body>
        I'm back!
    </body>
</html>
```



```html
<!-- 链接到外部的css文件 -->
<!doctype html>
<html>
    <head>
        <link rel="stylesheet" type="text/css" href="https://getmimo.com/r/styles.css">	<!-- link也是empty tag:建立网页和外部源的链接 -->
    </head>
    <body>
        I could be a part-time model.
    </body>
</html>
```

注意：在head中直接添加external style sheet

## style.css

组成元素：

- selectors：
  - types => 也就是tags
    - body
    - p
    - span
  - IDs => 属性id：是独一无二的
  - classes
- declarations => `<property>: <value>;`
  - color
  - font-family
  - text-align





### 针对types/tags

```css
/* style.css文件 */

p{
    color: blue;
    font-size: 14px;
}

span{
    color: brown;
}
```





### 针对ids

```css
/* style.css文件 */

#<id>{
    color: blue;
    font-size: 14px;
}
```



```html
<p id="topParagraph">xxx</p>
```





### 针对class

```css
/* style.css文件 */

.<class>{
    color: blue;
    font-size: 14px;
}


/* 多种class */
.box {
    border-style: solid
}
.primary-text {
    font-family: "Lucida Grande"
}
```



```html
<div class="box primary-text">xxx</div>
```





# CSS|box模型

是一个矩形的box：包裹每一个HTML元素：

- margins
- borders
- padding
- 具体content

```css
p {
    backgroud-color: #44b3c2;
    width: 150px;
    height: 25px;
    padding: 25px;
    border: 25px solid #f1a94e;
    margin: 25px;
    display: inline; /*默认值为block：inline元素没有width和height*/
    visibility: None;	/*即便隐藏，但仍然占据空间*/
}

/* margin: 边界之外的空白 */
p {
    border: 2px solid #44b3c2;
    margin-top: 10px;
    margin-right: 25px;
    margin-bottom: 10px;
    margin-left: 25px;
}

p {
    border: 2px solid #44b3c2;
    margin: 10px 25px 10px 25px;
}


/* padding: 边界内部的空白 */
p {
    border: 2px solid #44b3c2;
    padding: 25px;
}


/*  */
p {
    box-sizing: border-box;
}


/* 下述box的宽度为150px == width + 2*(padding+border+margin) */
.box {
    width: 100px;
    height: 25px;
    padding: 10px;
    border: 5px solid black;
    margin: 10px
}
```

特性display：

- inline：没有width和height
- inlune-block
- none



box-sizing：

- 默认值是content-box
- border-box



# CSS|位置layout

```css
img {
    float: right;	/* 图像在文字的右边 */
    margin: 10px;
}
```



```css
.box {
    width: 200px;
    height: 200px;
}

.blue { background: #44accf;}
.green { background: #b7d84b;}
.floated { float: left;}
.cleared { clear: left;}	/* 表示在所有元素的最下层 */
```

clear：

- left
- right
- both：表示位于任何floated元素下层



```css
.box {
    width: 200px;
    height: 200px;
    position: relative
}
```

position：

- relative
- absolut：不考虑其他元素的位置
- fixed：滚动过程中，保持在原来的位置

absolut和fixed都是脱离正常flow，使用offset的属性(比如top和bottom)





margin：

- auto：上下都是0px，左右占据最大的空位置
- margin: -25px auto 0px auto;
- margin: 0px auto
  - 两个元素：第一个0px表示top和bottom，第二个auto表示left和right

`text-align: center;`只表示文字居中，而`margin: 0px auto`是整个元素水平居中





# CSS|特殊的selectors

## universal selector

```css
* {
    border: 1px solid black;
    box-sizing: border-box;
}

*.box {backgroud: #44b3c2;}
```



## descendant(后裔)元素

```css
p span {
    font-weight: bold;
}
```

p中的span元素



```css
/* 紧接着第一个子元素 */
div > p {
    font-weight: bold;
}
```

紧接着div的p元素：注意p不是div的子元素

```html
<div></div>
<p>这是div > p选择的元素</p>
```





## pseudo class类

```css
a {
    
}

a:active {
    background: white;
    color: black;
}
```

针对被点击的元素：比如链接经过点击后，改变颜色

与`:hover`类似



`:first-child`：第一个子元素





## pseudo element元素

```css
p::first-letter {
    font-size: 2em;
}
```

`::first-line`





## <属性=value>作为selector

```css
a[href="https://getmimo.com"] {
    font-weight: bold;
}
```





## 选择一组selectors：拥有共同特性

用逗号分隔开

```css
div, .trailer, #<id> {
    border: 2px solid #44accf;
}
```





## 组合selectors：特性相结合

```css
.box{}
.blue.bordered {}
.pink.bordered {}

.blue {}
.pink {}
```



```html
<!doctype html>
...
<div class="blue bordered box"></div>
...
```





## specificity特异性

顺序：IDs > classes > types

```css
#<id> {}
.<class> {}
div
```



```html
<div class="box" id="box1"></div>
```



```html
<!doctype html>
<html>
    <head>
        <!-- interneal style sheet -->
        <style>		
            <!-- 这是box最终的样式 cyan -->
            #box1 {background: cyan;}	
            .box {background: magenta;}
            body div {background: yellow;}
            div.box {background: black;}
        </style>
    </head>
    <body>
        <div class="box" id="box1"> </div>
    </body>
</html>
```

