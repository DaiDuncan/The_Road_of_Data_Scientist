web开发人员会面临一些共同的问题

- 开始构建一个web站点时，你总需要一些相似的组件：处理用户认证（注册、登录、登出）的方式、一个管理站点的面板、表单、上传文件的方式，等等
- 由于框架的存在，你无需重新发明轮子就能建立新的站点



服务器

- 需要知道的第一件事就是你希望它为你的网页做什么。



框架（Framework）让程序员的生活更容易，常用的功能和方法都打包进了框架里，直接从库里拿出来修改一下就用

- URL分发（路由）
- 数据库读写（ORM）
- 表单
- admin后台管理等

<img src="https://firebasestorage.googleapis.com/v0/b/firescript-577a2.appspot.com/o/imgs%2Fapp%2Ftest_101_goals%2FD68F5fW0We.png?alt=media&token=0e1574ef-8582-4064-8678-376d40f65a2d" style="zoom: 80%;" />



# 1 全功能框架/重型框架Full-Stack

包括创建web应用所需要的大部分功能，通常可以用于构架需要完整功能和复杂设计的大型网站应用

## **Django**⭐

[官网](https://www.djangoproject.com/) | [GitHub](https://github.com/django) | [PyPI](https://pypi.python.org/pypi/Django/1.11.1) | [Awesome](https://github.com/rosarior/awesome-django)

比如Django集成了安全认证，URL Routing，模板引擎，ORM以及数据库Scheme映射。这使得Django非常强大，有很好的可扩展性，性能也非常好

Django支持 [PostgreSQL](https://www.postgresql.org/), [MySQL](https://www.mysql.com/), [SQLite](https://www.sqlite.org//index.html), [Oracle](https://www.oracle.com/index.html)和其他第三方数据库。其ORM功能支持多数据库之间的转换



提供的功能非常全面，比如(cache、session、登陆、auth授权等等)，和它强大的中间件，提供全方案Web开发支持

当然功能强大和全面的反面就是有点复杂（相对的），有点臃肿，不太灵活。所以Django上手要慢一点，自己造一个轮子替换Django某些内置功能或者使用第三方功能时不太灵活。



在2020年，Django会加入对机器学习的支持，同时携Python迅猛发展势头，很有可能会成为今年使用者增长最快的Web框架



### 特征

采用了[MVT的框架模式，即模型M，视图V和模版T](https://link.zhihu.com/?target=http%3A//mp.weixin.qq.com/s%3F__biz%3DMjM5OTMyODA4Nw%3D%3D%26mid%3D2247483665%26idx%3D1%26sn%3D19875184517f6513e53bec3b91c10202%26chksm%3Da73c6129904be83f13ff0501c1159b4e3b776f086613ce4e3fdff2b525aa361cee819f5ebd77%26scene%3D21%23wechat_redirect)

自带大量的常用工具和组件（比如数据库ORM组件、用户认证、权限管理、分页、缓存), 甚至还自带了管理后台Admin，适合快速开发功能完善的企业级网站。

Django自带免费的数据SQLite，同时支持MySQL与PostgreSQL等多种数据库。



#### 项目结构

Django项目的结构布局是刚性的，每个人写的项目结构最后都差不多，你可以清楚地知道在哪个APP的哪个文件夹里找到哪个文件(media目录, static目录, template目录，views.py, models.py, forms.py, etc)



### 缺点

在urls.py里配置URL路由有点麻烦；

模板不能像php一样在模板插代码；

数据库ORM组装出来的sql语句性能较差；





### 开发案例

利用Django开发的著名网站包括Pinterest, Disqus, Eventbrite, Instagram and Bitbucket

不过最近Pinterest改用Flask开发它的API了



开发项目目标明确，就是要开发包含各种功能的传统企业级网站（比如电商，新闻内容管理，社交网站，办公OA)，使用Django能帮你节省不少寻找或开发第三方扩展的精力

开发企业级网站通常由一个团队来进行，Django可插拔式的APP设计思想和刚性的项目结构便于团队后期维护项目代码

## Pyramid

[官网](https://trypyramid.com/) | [GitHub](https://github.com/Pylons/pyramid) | [PyPI](https://pypi.python.org/pypi/pyramid) | [Awesome](https://github.com/uralbash/awesome-pyramid)

在2010年就诞生，其目标是简化web开发的复杂性。

这个框架可以用于复杂的应用开发，也适用轻量级应用。Pyraid的开发社区还是比较活跃的。版本更新频繁，各技术群的讨论也是非常热烈的





## TurboGears

[官网](http://www.turbogears.org/) | [GitHub](https://github.com/TurboGears/tg2/) | [PyPI](https://pypi.python.org/pypi/TurboGears2/2.3.11)

一个开源和数据驱动的程序框架，它是建构在很多不同的中间件和库的基础上，实际上这个框架试图把其他的Python框架中最好的组件融入其中

TurboGear允许开发者能够快速搭建数据驱动的网站应用。它有非常好用的模板引擎，对聚合的支持，功能强大而灵活的ORM工具，而且自带了大量的小代码片段，可以让开发更容易

现在TurboGear的社区正在致力于开发一个简化（瘦身）版的TurboGear框架，这将会给大家带来一个更加简单易用的框架。





## Web2py

[官网](http://www.web2py.com/) | [GitHub](https://github.com/web2py) | [PyPI](https://pypi.python.org/pypi/web2py)

原先是作为一个教学用的项目被开发出来，自带IDE工具，为了简化使用，其没有项目级的配置文件

Web2Py有一个内置的ticket系统，只要出现错误就会生出ticket，用来追踪运行时的问题

这个项目的社区和邮件列表并不活跃





# 2 轻量级框架Non Full-Stack

提供比较简单的网站构建功能，通常用于简单的，或者是小型的网站应用

## Flask⭐

[官网](http://flask.pocoo.org/) | [GitHub](https://github.com/pallets/flask/) | [PyPI](https://pypi.python.org/pypi/Flask/0.12) | [Awesome](https://github.com/humiaozuzu/awesome-flask)

Flask是最流行的Python轻量级Web框架。这个框架是受到Sinatra Ruby的启发而开发出来的。 Flask基于[Werkzeug WSGI toolkit](http://werkzeug.pocoo.org/)以及 [Jinja2](http://quintagroup.com/cms/python/jinja2) 模板

目的是要建立一个非常稳定和可靠的Web应用的基础系统，我们可以使用Flack再加上各种插件，扩展和其他模块，能够构建功能强大的网站和应用



事实上，如果Django不适合的应用类型，使用Flask基本上是Python Web开发的默认选择

Flask也是一个在2010年启动的开源项目，到目前为止已经更新了27个版本，由于历史比较长，有些早期的扩展已经不被支持，文档也不再更新。需要在网络上找到最新的文档和功能



使用Restful request的风格，很适合开发web api，Flask也更加pythonic



### 特征

默认情况下，Flask 只相当于一个内核，不包含数据库抽象层(ORM)、用户认证、表单验证、发送邮件等其它Web框架经常包含的功能

Flask依赖用各种灵活的扩展（比如邮件Flask-Mail，用户认证Flask-Login，数据库Flask-SQLAlchemy）来给Web应用添加额外功能

Flask没有指定的数据库，可以用MySQL，也可以用 NoSQL。



#### 项目结构

Flask是很灵活的，你可以随意地组织自己的代码，1000个APP说不定就有有1000种组织代码的方式。不同的人之间因为习惯不同可能导致最后项目结构布局差异很大, 造成后期代码难以阅读和维护。

如果大家都严格遵循Flask推荐的代码布局，那么你会发现最后将得到和Django类似的项目结构布局



### 开发案例

Twilio, Netflix, Uber和LinkedIn

Django似乎更多用来开发常规网站，而Flask经常用来开发API（比如Pinterest和Twilio)





## Bottle

[官网](http://bottlepy.org/docs/dev/index.html) | [GitHub](https://github.com/bottlepy/bottle) | [PyPI](https://pypi.python.org/pypi/bottle/0.12.13)

最初是设计为一个API框架，整个Bottle框架是在一个源文件上。没有引用任何Python标准库。建议是如果使用Bottle，最好是非常小的程序，最好小于500行代码并且没有特殊的需求。



**CherryPy， Falcon， Hug，** **FastAPI -** 极为小众





# 3 异步框架Asynchronous

异步框架的I/O性能相对就高一些。当然异步编程的理解难度要大一点



以下两个开源框架用于==处理高并发的应用==，可以用于需要解决C10K问题（10000+并发的场景）

## Sanic

[官网](http://sanic.readthedocs.io/en/latest/) | [GitHub](https://github.com/channelcat/sanic) | [PyPI](https://pypi.python.org/pypi/Sanic)

Sanic是基于[uvloop](https://github.com/MagicStack/uvloop)开发的，用于创建高并发异步Http请求的应用，必须使用Python3.5+，兼容Python3.5+的async/await方法，提供非阻塞的异步访问功能。

Sanic是一个非常流行的异步框架。提供了routing, middleware, cookies, versioning, static files, blueprints, class-based views, 以及sockets的功能。

不过比较可惜的是并没有提供模板引擎，也没有内置的数据库支持功能

在一个Benchmark测试中，Sanic单进程和100连接的情况下，==最高每秒同时并发处理33542个请求，平均时延2.96ms==



## **Tornado**⭐

[官网](http://www.tornadoweb.org/en/latest/) | [GitHub](https://github.com/tornadoweb/tornado) | [PyPI](https://pypi.python.org/pypi/tornado)

Tornado是一个Python web框架加上异步网络处理库，用于大流量的网络应用开发。使用非阻塞I/O,目标能够处理C10K网站。

如果配置合理，Tornado框架的网站应用能够轻松应对10000+并发的流量

Tornado的流行程度介于Django和Flask之间，如果你需要一个web应用，同时也要支持高并发，那Tornado是最好的选择





我对Tornado的印象不太好。16年的时候公司收购了一个项目，然后我去杭州接手这个项目的技术部分，系统是用Tornado开发的。团队是从杭州大厂出来的，设计得非常繁复，说是要支持4万并发的业务。但是到我们买的那天，业务连4千并发都不到。接下来的一年公司为这个项目付出了很大代价。





# 小结

如果你要开发一个大型项目，比如电商系统，需要各种各样的功能都具备，那么使用Full-Stack Web框架是第一选择

如果是一个像内容系统，==功能有限==，不需要面面俱到，那么用Non Full-Stack是第一选择



@评论

话说现在都是前后端分离 一般不会在后端渲染模版啥的 ==基本就是实现api== 用flask这种还是比较合适的



=> 能熟练用其中一种做开发，应该可以找到个web开发工作

对一个刚入门Web开发的新手而言，世界上最悲催的事莫过于花大力气学了一门语言或框架却发现它已经要过气了

