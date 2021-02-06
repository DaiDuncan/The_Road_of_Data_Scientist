python之所以强大，在于有许许多多的人贡献自己的力量，他们将自己开发的项目打包上传至pypi

工作中，你也可以将自己编写的库打包，分享给其他同事，或者在生产环境进行安装部署



如何制作setup.py文件用以打包python库？



# setuptools

setuptools是增强版的distutils

distutils则是python标准库的一部分，于2000年发布，它能够进行python库的安装和发布

除了setuptools和distutils之外，还有一个distribute，它是setuptools的一个fork分支，弱水三千，只取一瓢，咱们掌握setuptools就可以了。



## setup.py

python打包分发的关键在于编写setup.py文件，咱们来看一下flask的setup.py是如何编写的

```python
import io
import re

from setuptools import find_packages
from setuptools import setup

with io.open("README.rst", "rt", encoding="utf8") as f:
    readme = f.read()

with io.open("src/flask/__init__.py", "rt", encoding="utf8") as f:
    version = re.search(r'__version__ = "(.*?)"', f.read()).group(1)

setup(
    name="Flask",
    version=version,
    url="https://palletsprojects.com/p/flask/",
    project_urls={
        "Documentation": "https://flask.palletsprojects.com/",
        "Code": "https://github.com/pallets/flask",
        "Issue tracker": "https://github.com/pallets/flask/issues",
    },
    license="BSD-3-Clause",
    author="Armin Ronacher",
    author_email="armin.ronacher@active-4.com",
    maintainer="Pallets",
    maintainer_email="contact@palletsprojects.com",
    description="A simple framework for building complex web applications.",
    long_description=readme,
    classifiers=[
        "Development Status :: 5 - Production/Stable",
        "Environment :: Web Environment",
        "Framework :: Flask",
        "Intended Audience :: Developers",
        "License :: OSI Approved :: BSD License",
        "Operating System :: OS Independent",
        "Programming Language :: Python",
        "Programming Language :: Python :: 2",
        "Programming Language :: Python :: 2.7",
        "Programming Language :: Python :: 3",
        "Programming Language :: Python :: 3.5",
        "Programming Language :: Python :: 3.6",
        "Programming Language :: Python :: 3.7",
        "Topic :: Internet :: WWW/HTTP :: Dynamic Content",
        "Topic :: Internet :: WWW/HTTP :: WSGI :: Application",
        "Topic :: Software Development :: Libraries :: Application Frameworks",
        "Topic :: Software Development :: Libraries :: Python Modules",
    ],
    packages=find_packages("src"),
    package_dir={"": "src"},
    include_package_data=True,
    python_requires=">=2.7, !=3.0.*, !=3.1.*, !=3.2.*, !=3.3.*, !=3.4.*",
    install_requires=[
        "Werkzeug>=0.15",
        "Jinja2>=2.10.1",
        "itsdangerous>=0.24",
        "click>=5.1",
    ],
    extras_require={
        "dotenv": ["python-dotenv"],
        "dev": [
            "pytest",
            "coverage",
            "tox",
            "sphinx",
            "pallets-sphinx-themes",
            "sphinxcontrib-log-cabinet",
            "sphinx-issues",
        ],
        "docs": [
            "sphinx",
            "pallets-sphinx-themes",
            "sphinxcontrib-log-cabinet",
            "sphinx-issues",
        ],
    },
    entry_points={"console_scripts": ["flask = flask.cli:main"]},
)
```

直击重点，setup是从setuptools模块引入的一个函数，这个函数有许多参数，正是这些参数对python打包和库安装做出了约定，下表是常用的参数说明：

| 编号 | 参数                 | 说明                                                     |
| :--- | :------------------- | :------------------------------------------------------- |
| 1    | name                 | 包名称                                                   |
| 2    | version              | 包版本                                                   |
| 3    | author               | 作者                                                     |
| 4    | author_email         | 作者的邮箱                                               |
| 5    | maintainer           | 维护者                                                   |
| 6    | maintainer_email     | 维护者的邮箱                                             |
| 7    | url                  | 程序的官网地址                                           |
| 8    | license              | 授权信息                                                 |
| 9    | description          | 程序的简单描述                                           |
| 10   | long_description     | 程序的详细描述                                           |
| 11   | platforms            | 程序适用的软件平台列表                                   |
| 12   | classifiers          | 程序的所属分类列表                                       |
| 13   | keywords             | 程序的关键字列表                                         |
| 14   | packages             | 需要打包的包目录(通常为包含 __init__.py 的文件夹)        |
| 15   | py_modules           | 需要打包的 Python 单文件列表                             |
| 16   | download_url         | 程序的下载地址                                           |
| 17   | cmdclass             | 添加自定义命令                                           |
| 18   | package_data         | 指定包内需要包含的数据文件                               |
| 19   | include_package_data | 自动包含包内所有受版本控制(cvs/svn/git)的数据文件        |
| 20   | exclude_package_data | 当 include_package_data 为 True 时该选项用于排除部分文件 |
| 21   | data_files           | 打包时需要打包的数据文件，如图片，配置文件等             |
| 22   | ext_modules          | 指定扩展模块                                             |
| 23   | scripts              | 指定可执行脚本,安装时脚本会被安装到系统 PATH 路径下      |
| 24   | package_dir          | 指定哪些目录下的文件被映射到哪个源码包                   |
| 25   | requires             | 指定依赖的其他包                                         |
| 26   | provides             | 指定可以为哪些模块提供依赖                               |
| 27   | install_requires     | 安装时需要安装的依赖包                                   |
| 28   | entry_points         | 动态发现服务和插件                                       |
| 29   | setup_requires       | 指定运行 setup.py 文件本身所依赖的包                     |
| 30   | dependency_links     | 指定依赖包的下载地址                                     |
| 31   | extras_require       | 当前包的高级/额外特性需要依赖的分发包                    |
| 32   | zip_safe             | 不压缩包，而是以目录的形式安装                           |
| 33   | python_requires      | 需要的python版本                                         |



### find_packages

find_packages默认在setup.py所在的目录下搜索包含`__init__.py`文件的目录作为要添加的包

```python
@classmethod
def find(cls, where='.', exclude=(), include=('*',)):
```

- where指定在哪个目录下搜索
- exclude设置需要排除的包
- include设置要包含的包

在flask的setup.py文件中，find_packages函数第一个参数设置为src,这是因为作者将flask的源码放在了src目录下，src目录下没有`__init__.py`文件，而src/flask目录下有该文件。





### name

name是一个容易错误理解的参数，在flask的setup.py文件中，name的值是Flask

Flask是库的名字，安装以后在使用时，我们需要使用flask，而非Flask

```python
from flask import request
```

这里的库的名称flask的由来是src/flask这个目录





### python_requires

python一直处于发展变化中，尽管做了最大的努力来保证向下兼容，但不同版本间的区别仍然导致第三方库无法在所有版本上运行

因此打包时需指明这个库可以在哪写python版本上运行，当你使用pip安装第三方库时，pip会做版本兼容性检查





### install_requires

安装时需要安装的依赖包

如果你的程序依赖于其他的第三方库，那么你需要在这里指明，当使用pip进行安装时，pip会自动将这些一来的第三方库一起安装



### license

程序的授权信息，开源社区有6种常用的授权协议

![image-20210128120754090](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210128120754.png)





# 示例|打包分发

## 1 新建项目packaging_demo

新建一个项目，项目结构如下：

![image-20210128120834470](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210128120834.png)

```text
├── hibiscus
│   ├── __init__.py
│   ├── decorator
│   │   ├── __init__.py
│   │   ├── __pycache__
│   │   └── func_decorator.py
│   └── func_utils
│       ├── __init__.py
│       └── utils.py
└── setup.py
```



- `packaging_demo/hibiscus/__init__.py`

```python
from .decorator import *
from .func_utils import *
```

- `packaging_demo/hibiscus/decorator/func_decorator.py`

```python
import time


class FuncTimeDecorator(object):
    def __init__(self, func):
        self.func = func

    def __call__(self, *args, **kwargs):
        t1 = time.time()
        res = self.func(*args, **kwargs)
        t2 = time.time()
        print("函数执行时长:"+ str(t2 - t1))
```

- `packaging_demo/hibiscus/decorator/__init__.py`

```python
from .func_decorator import FuncTimeDecorator
```

- `setup.py`

```python
from setuptools import setup, find_packages

setup(
    name='Hibiscus',
    version='0.0.1',
    author='酷python',
    author_email='pythonlinks@163.com',
    description='打包示例',
    url='http://www.coolpython.net',
    packages=find_packages(),
    classifiers=[
        "Programming Language :: Python :: 3",
        "License :: OSI Approved :: MIT License",
        "Operating System :: OS Independent",
    ],
    python_requires='>=3.6',
)
```





## 2 打包

打包之前，先确认所需要的工具是否齐全，主要是setuptools和wheel

```text
python3 -m pip install --user --upgrade setuptools wheel
```



之后在setup.py所在目录执行下面的命令

```text
python3 setup.py sdist bdist_wheel
```



打包之后会生成几个重要文件夹，主要关注dist，在这个文件夹下生成了两个文件

```text
Hibiscus-0.0.1-py3-none-any.whl
Hibiscus-0.0.1.tar.gz
```

- .whl可以理解为二进制安装包

- .tar.gz可以理解为源码安装包,解压后可以看到程序的源码





## 3 安装

```text
python3 setup.py sdist bdist_wheel
```



### 方法1）在自己的程序源码里安装

```python
python3 setup.py install
```



### 方法2）使用.whl文件

也可以进入dist目录，执行下面的命令：

```text
pip3 install Hibiscus-0.0.1-py3-none-any.whl
```



### 方法3）使用tar.gz文件

```text
pip3 install Hibiscus-0.0.1.tar.gz
```

这种方法先编译出wheel文件，然后再进行安装，如下是其安装过程

```text
Processing ./Hibiscus-0.0.1.tar.gz
Building wheels for collected packages: Hibiscus
  Building wheel for Hibiscus (setup.py) ... done
  Created wheel for Hibiscus: filename=Hibiscus-0.0.1-py3-none-any.whl size=2382 sha256=e4f46223ac138f0b30c3c7fdad3f3284f964144d35e31188fb3d58761aced350
  Stored in directory: /Users/kwsy/Library/Caches/pip/wheels/6f/9f/44/951064df5b5bba5bc41bf713804006c271b2a6946e139dd79c
Successfully built Hibiscus
Installing collected packages: Hibiscus
Successfully installed Hibiscus-0.0.1
```





### 方法4）解压tar.gz文件，然后使用setup.py安装

对tar.gz解压，解压后的文件目录如下

```text
├── Hibiscus.egg-info
│   ├── PKG-INFO
│   ├── SOURCES.txt
│   ├── dependency_links.txt
│   └── top_level.txt
├── PKG-INFO
├── build
│   ├── bdist.macosx-10.9-x86_64
│   └── lib
│       └── hibiscus
│           ├── __init__.py
│           ├── decorator
│           │   ├── __init__.py
│           │   └── func_decorator.py
│           └── func_utils
│               ├── __init__.py
│               └── utils.py
├── dist
│   └── Hibiscus-0.0.1-py3.6.egg
├── hibiscus
│   ├── __init__.py
│   ├── decorator
│   │   ├── __init__.py
│   │   └── func_decorator.py
│   └── func_utils
│       ├── __init__.py
│       └── utils.py
├── setup.cfg
└── setup.py
```





执行命令

```text
python3 setup.py install
```

注意看dist目录里的Hibiscus-0.0.1-py3.6.egg，就是这个文件被安装到了site-packages目录下





# 补充|pip和python命令安装的区别

```text
python3 setup.py install
```

经过这种方式安装，会将Hibiscus-0.0.1-py3.6.egg文件安装到site-packages目录下

而如果使用pip命令安装，则会在site-packages目录下创建一个名为hibiscus的目录, 在这个目录下有程序的源码



简单总结：

- pip安装后可以在site-packages目录下找到源码
- 而使用python命令直接安装，则只能找到一个.egg文件











