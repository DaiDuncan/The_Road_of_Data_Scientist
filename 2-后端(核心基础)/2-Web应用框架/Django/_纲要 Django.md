网站服务器：一个用来监控接收邮件（请求）的邮箱（端口）

- 网站服务器读这封信，然后将响应发送给网页
- 当你想发送一些东西的时候，你必须要有一些内容。 而Django就是可以帮助您创建内容的工具。



服务器请求：

当一个请求到达网站服务器，它会被传递到Django，试图找到实际上什么是被请求的

它首先会拿到一个网页的地址，然后试图去弄清该做什么：

- Django的urlresolver（url解析器）
- 并不十分聪明，接受一个模式列表，然后试图去匹配 URL。 Django从顶到底检查模式，如果有匹配上的，那么Django会将请求传递给相关的函数（这被称作视图）
- 想象一个邮递员拿着一封信。 她沿着街区走下去，检查每一个房号与信件地址是否对应。 如果匹配上了，她就把信投在那里。 这也是url解析器的工作方式！



视图函数：

- 用户要求修改数据呢？
  - *视图* 将检查是否你允许它这么干，然后更新工作描述并发回一个消息：“做完了”
  - 视图生成响应，并且Django能够发送给用户的web浏览器



## 1 安装Python

| 平台    |                    |
| ------- | ------------------ |
| Windows | .msi文件：添加PATH |
| Linux   | 先安装Python       |
| OS X    |                    |



## 2 配置虚拟环境 virtualenv

让你计算机上的编码环境保持整洁

虚拟环境会以项目为单位将你的 Python/Django 安装隔离开。 这意味着对一个网站的修改不会影响到你同时在开发的其他任何一个网站。

```shell
mkdir djangogirls
cd djangogirls

### 虚拟环境 命名
python3 -m venv myvenv
```



### 使用虚拟环境

```shell
C:\Users\Name\djangogirls> myvenv\Scripts\activate
```



## 3 安装Django

```shell
(myvenv) ~$ pip install django==1.8
```



### +代码编辑器

### +git



# 理论|互联网如何工作？

是一个网站只是一堆保存在硬盘上的文件。 就像你的电影、 音乐或图片一样。 然而，网站的唯一的不同之处是： 网站包含一种称为 HTML 的代码。

我们需要把HTML文件存储在硬盘的某个位置。 对于互联网，我们使用特定而功能强大的电脑，我们称之为服务器

- 它们没有屏幕、鼠标或者键盘，因为它们的主要目的是存储数据，并用它来提供服务

我们不太可能用电缆去连接任何两台上网的电脑。 我们需要将请求通过很多很多不同的机器传递，以便连接一台机器 

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210108105337.png" alt="image-20210108105337315" style="zoom:80%;" />

唯一的事情是，如果你将许多信件 (数据包) 发送到同一个地方，他们可以通过完全不同邮政局 (路由器)。 这取决于每个办公室的分布情况

- 使用的地址叫做IP地址
- 首先要求DNS（域名服务器）去解析djangogirls.org成为IP地址
  - 工作起来有点像老式的电话簿，在那里你可以找到你想联系的人的名字，并且找到他们的电话号码和地址
- 使用一种接收者同样能够理解的语言：一个名为 HTTP (Hypertext Transfer Protocol：超文本传输协议) 的协议

基本上，当你有一个网站你需要有一台服务器用于网站的托管。 当服务器接收到一个访问请求（一封信），它把网站内容发送回去（回信）



# 理论|基本概念

## URL

 在 Django 中，我们使用一种叫做 URLconf （URL 配置）的机制



### 正则表达式匹配

Django 使用了 `regex`



## 视图view

view是存放应用逻辑的地方

- 从之前创建的 模型中获取数据
- 并将它传递给 模板

视图就是Python方法

一个最简单的view就长得这个样子：

```python
def post_list(request):
    return render(request, 'blog/post_list.html', {})
```











# 项目及其部署

## 文件目录

```shell
djangogirls
├───manage.py
└───mysite
        settings.py
        urls.py
        wsgi.py
        __init__.py
```





## 更改设置

```python
### 时区
TIME_ZONE = 'Europe/Berlin'


### 添加静态文件的路径
STATIC_URL = '/static/'
STATIC_ROOT = os.path.join(BASE_DIR, 'static')


### 数据库
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    }
}


### 开启 web 服务器
(myvenv) ~/djangogirls$ python manage.py runserver
# 如果Windows系统遇到UnicodeDecodeError这个错误
(myvenv) ~/djangogirls$ python manage.py runserver 0:8000


    
### 检测站点的服务器是否已经在运行
http://127.0.0.1:8000/
```



## Django模型

- 面向对象
- 创建应用程序
- 创建一个博客文章模型



## Django管理|from django.contrib import admin





## 项目部署

部署是在互联网上发布你的应用程序的一系列过程

在互联网上你可以找到很多的服务器供应商

- Heroku
- [PythonAnywhere](https://www.pythonanywhere.com/)
  -  对于一些没有太多访问者的小应用是免费的

外部服务是[GitHub](https://www.github.com/)

使用 GitHub 作为基石，以和 PythonAnywhere 互相传输我们的代码

- 方法1）git push到PythonAnywhere 

```shell
### git
$ git status
$ git add --all .
$ git status
$ git commit -m "Added view and template for detailed blog post as well as CSS for the site."
$ git push


### PythonAnywhere 的 Bash 终端
$ cd my-first-blog
$ source myvenv/bin/activate
(myvenv)$ git pull
[...]
(myvenv)$ python manage.py collectstatic
[...]
```



- 方法2）git clone: 拉取一份你的代码副本到 PythonAnywhere 上

```shell
$ tree my-first-blog
my-first-blog/
├── blog
│   ├── __init__.py
│   ├── admin.py
│   ├── migrations
│   │   ├── 0001_initial.py
│   │   └── __init__.py
│   ├── models.py
│   ├── tests.py
│   └── views.py
├── manage.py
└── mysite
    ├── __init__.py
    ├── settings.py
    ├── urls.py
    └── wsgi.py
```



### 在 PythonAnywhere 上创建 virtualenv

```shell
$ cd my-first-blog

$ virtualenv --python=python3.4 myvenv
Running virtualenv with interpreter /usr/bin/python3.4
[...]
Installing setuptools, pip...done.

$ source myvenv/bin/activate

(mvenv) $  pip install django whitenoise
Collecting django
[...]
Successfully installed django-1.8.2 whitenoise-2.0
```



### 收集静态文件

静态文件是很少改动或者并非可运行的程序代码的那些文件，比如 HTML 或 CSS 文件





### 在 PythonAnywhere 上创建数据库

服务器与你自己的计算机不同的另外一点是：它使用不同的数据库。因此用户账户以及文章和你电脑上的可能会有不同





# 发布Web应用程序

## 配置 WSGI 文件

Django 使用 “WSGI 协议”，它是用来服务 Python 网站的一个标准









# #参考文献

[Link: 教程|djangogirls](https://tutorial.djangogirls.org/zh/installation/)

[Link: 演讲|互联网如何工作？](http://web.mit.edu/jesstess/www/)