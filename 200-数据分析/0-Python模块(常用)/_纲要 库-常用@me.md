python的发展得益于开源社区的开源精神，人们热衷于将自己的成果展示在互联网上并希望能够帮助到别人，你可以在github和pypi上找到大量python开源库

![Python库](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210109222030.png)

| 库(官方文档) | 教程 |
| ------------ | ---- |
|              |      |
|              |      |
|              |      |
|              |      |



# 常用第三方库

## 如何学习使用第三方库

得益于python强大的开源社区，我们在使用python开发项目时，可以几乎可以做到全程拿来主义。

虽然你不必重复造轮子，但理解掌握别人已经早好的轮子有时候也并非易事

### 1 搜索第三方库文档

可以直接找到第三方库的官方网址，或者找到国内已经翻译好的教程

- 讲解详细
- 示例多



### 2 阅读源码示例， 测试脚本

有些第三方库的作者并没有提供官方的文档，或者还没有人将英文的文档翻译成中文，或者文档讲解不细致

考虑从源码里寻找可用的资源，例如源码中的代码示例或者测试脚本。

- 作者认为自己所开发的库或者模块比较简单，无需提供详细的使用文档，但他会在源码里写一些示例代码
- 有些作者自己会写测试脚本以验证自己的代码是健壮的，通过示例代码和测试脚本，你仍然可以快速学习第三方库的使用方法



很多作者都会把开源项目放在github上，因此想要找源码，github最合适不过

- 以web框架[bottle](https://github.com/bottlepy/bottle)为例，在git仓库里有一个==test文件夹==，这里就保留了大量作者用以测试框架的代码，你完全可以通过阅读这些测试代码来学习该框架
- 以excel操作库[xlrd](https://github.com/python-excel/xlrd)为例，源码里不仅有tests文件夹，还有一个==examples文件夹==，提供了示例代码





### 3 编译项目使用说明文档

有些作者会在源码里提供项目使用说明文档，以[xlwt](https://github.com/python-excel/xlwt)为例，==项目文档存放在docs中==

将源码下载到本地，并进入到docs目录中，你会看到很多以.rst结尾的文件，rst于Python类似Javadoc与Java,我们可以将其转为html后用浏览器打开。

在转换之前，你需要先安装sphinx

```python
pip3 install sphinx
```

编译说明文档有两种方式：

1. 进入到源码，执行命

   ```shell
   sudo sphinx-build -b html docs build
   # docs就是保存rst文件的文件夹
   # 上面的命令会生成一个build文件，在这个文件夹里有一个index.html文件，用浏览器打开这个文件，就可以看到帮助说明文档
   ```

2.  进入docs文件夹，执行命令

   ```shell
   make html
   # 会在docs文件夹下生成一个_build目录，和方法1里生成的build一样，保存了一些html文件，使用浏览器打开index.html，就可以查看说明文档了。
   ```



# 科学计算

| 库(官方文档) | 教程                                                         |
| ------------ | ------------------------------------------------------------ |
|              | [numpy](https://link.zhihu.com/?target=http%3A//www.numpy.org/) |
|              | [scipy](https://link.zhihu.com/?target=http%3A//www.scipy.org/) |
|              | [pandas](https://link.zhihu.com/?target=http%3A//pandas.pydata.org/) |
|              |                                                              |

blaze： [blaze](https://link.zhihu.com/?target=http%3A//blaze.readthedocs.io/en/latest/index.html)





# Web框架

| 库(官方文档) | 教程                                                         |
| ------------ | ------------------------------------------------------------ |
|              | [django](https://link.zhihu.com/?target=https%3A//www.djangoproject.com/) |
|              | [flask](https://link.zhihu.com/?target=http%3A//flask.pocoo.org/) |
|              |                                                              |
|              |                                                              |



web2py：[web2py](https://link.zhihu.com/?target=http%3A//web2py.com/)

bottle： [bottle](https://link.zhihu.com/?target=http%3A//www.bottlepy.org/docs/dev/index.html)

tornadoweb ：[tornadoweb](https://link.zhihu.com/?target=http%3A//www.tornadoweb.org/en/stable/)

webpy： [webpy](https://link.zhihu.com/?target=http%3A//webpy.org/)

cherrypy： [cherrypy](https://link.zhihu.com/?target=http%3A//www.cherrypy.org/)

jinjs： [jinja](https://link.zhihu.com/?target=http%3A//docs.jinkan.org/docs/jinja2/)





# 爬虫

| 库(官方文档) | 教程                                                         |
| ------------ | ------------------------------------------------------------ |
|              |                                                              |
|              | [BeautifulSoup](https://link.zhihu.com/?target=https%3A//www.crummy.com/software/BeautifulSoup/) |
|              |                                                              |
|              |                                                              |



urllib 、urllib2 、requests

scrapy： [scrapy](https://link.zhihu.com/?target=http%3A//scrapy.org/)

pyspider： [pyspider](https://link.zhihu.com/?target=https%3A//github.com/binux/pyspider)

portia： [portia](https://link.zhihu.com/?target=https%3A//github.com/scrapinghub/portia)

html2text： [html2text](https://link.zhihu.com/?target=https%3A//github.com/Alir3z4/html2text)



lxml： [lxml](https://link.zhihu.com/?target=http%3A//lxml.de/)

selenium：[selenium](https://link.zhihu.com/?target=http%3A//docs.seleniumhq.org/)

mechanize： [mechanize](https://link.zhihu.com/?target=https%3A//pypi.python.org/pypi/mechanize)

PyQuery： [pyquery](https://link.zhihu.com/?target=https%3A//pypi.python.org/pypi/pyquery/)

creepy： [creepy](https://link.zhihu.com/?target=https%3A//pypi.python.org/pypi/creepy)





# 图像处理

bigmoyan：[scikit-image](https://link.zhihu.com/?target=http%3A//scikit-image.org/) 

Python Imaging Library (PIL)：[pil](https://link.zhihu.com/?target=http%3A//www.pythonware.com/products/pil/)

pillow： [pillow](https://link.zhihu.com/?target=http%3A//pillow.readthedocs.io/en/latest/)

python-qrcode： [python-qrcode](https://link.zhihu.com/?target=https%3A//github.com/lincolnloop/python-qrcode)





# 自然语言处理

nltk： [nltk ](https://link.zhihu.com/?target=http%3A//www.nltk.org/)

snownlp： [snownlp](https://link.zhihu.com/?target=https%3A//github.com/isnowfy/snownlp)

Pattern：[pattern](https://link.zhihu.com/?target=https%3A//github.com/clips/pattern)

TextBlob：[textblob](https://link.zhihu.com/?target=http%3A//textblob.readthedocs.org/en/dev/)

Polyglot：[polyglot](https://link.zhihu.com/?target=https%3A//pypi.python.org/pypi/polyglot)

jieba： [jieba](https://link.zhihu.com/?target=https%3A//github.com/fxsjy/jieba)







# GUI 图形界面

Tkinter : [Tkinter](https://link.zhihu.com/?target=https%3A//wiki.python.org/moin/TkInter/)

wxPython: [wxPython](https://link.zhihu.com/?target=https%3A//www.wxpython.org/)

PyGTK: [PyGTK](https://link.zhihu.com/?target=http%3A//www.pygtk.org/)

PyQt: [PyQt](https://link.zhihu.com/?target=https%3A//sourceforge.net/projects/pyqt/)

PySide: [PySide](https://link.zhihu.com/?target=http%3A//wiki.qt.io/Category%3ALanguageBindings%3A%3APySide)





# 密码学

cryptography：[cryptography](https://link.zhihu.com/?target=https%3A//pypi.python.org/pypi/cryptography/)

hashids：[hashids](https://link.zhihu.com/?target=http%3A//www.oschina.net/p/hashids)

Paramiko：[Paramiko](https://link.zhihu.com/?target=http%3A//www.paramiko.org/)

Passlib：[Passlib](https://link.zhihu.com/?target=https%3A//pythonhosted.org/passlib/)

PyCrypto：[PyCrypto](https://link.zhihu.com/?target=https%3A//pypi.python.org/pypi/pycrypto)

PyNacl：[PyNacl](https://link.zhihu.com/?target=http%3A//pynacl.readthedocs.io/en/latest/)





# 数据库驱动

mysql-python： [mysql-python](https://link.zhihu.com/?target=https%3A//sourceforge.net/projects/mysql-python/)

PyMySQL： [PyMySQL](https://link.zhihu.com/?target=https%3A//github.com/PyMySQL/PyMySQL)

PyMongo： [PyMongo](https://link.zhihu.com/?target=https%3A//docs.mongodb.com/ecosystem/drivers/python/)

