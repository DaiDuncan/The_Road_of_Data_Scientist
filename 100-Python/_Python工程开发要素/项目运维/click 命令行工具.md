## 意义

我们编写一个具有特定功能的脚本时，通常脚本的输入都是硬编码在代码里的，但如果你实现的是一个比较通用的命令行工具，那么它运行时所需要的参数就不能写死在代码里，不然每次运行时都要修改代码。



比如你写了一个获取城市天气信息的脚本，程序的主函数需要城市的名称，你总不能每次运行时都修改脚本里的代码吧

命令行工具在使用时传入运行所需参数

```python
pip install Click
pip uninstall Click
```



pip就是一个命令工具，它要根据不同的命令参数执行不同的任务

python标准库提供了Argparse这个命令行解析工具，但着实不好用，此外，你也可以使用sys.argv来==临时充当命令行解析工具，但不建议这么做==





# click

## click.command()

脚本里的函数被click.command()装饰以后，函数就变成了一个命令行工具

```python
import click

@click.command()
def say_hello():
    click.echo('hello')

if __name__ == '__main__':
    say_hello()
```



运行程序：

```text
python3 demo.py
hello
```





## click.option()

可以为工具设置可选参数

```python
import click

@click.command()
@click.option("-name", default='world', help="姓名", type=str)
def say_hello(name):
    click.echo('hello {name}'.format(name=name))

if __name__ == '__main__':
    say_hello()
```

运行程序：

```text
python3 demo.py -name=lili
hello lili
```

只用click， 很轻松的就传递了参数



不仅如此，还提供了查看文档的功能，执行命令

```shell
python3 demo.py --help	#脚本的后面使用--help参数

Usage: demo.py [OPTIONS]

Options:
  -name TEXT  姓名
  --help      Show this message and exit.
```





## click.argument

为工具设置必传参数

```python
import click

@click.command()
@click.option('--count', default=1, help='number of greetings')
@click.argument('name')
def hello(count, name):
    for x in range(count):
        click.echo('Hello %s!' % name)

if __name__ == '__main__':
    hello()
```



```python
### --help命令查看工具文档
Usage: demo.py [OPTIONS] NAME	#NAME参数是必传的

Options:
  --count INTEGER  number of greetings
  --help           Show this message and exit.

# 使用方法
python3 demo.py --count=2 world

Hello world!
Hello world!
```





## click.group()

前面展示的命令工具，都只有一个命令，实际生产中显然是不够用的，click.group()可以让我们把许多命令加到一个组中

```python
import click

@click.group()
def cli():
    pass

@click.command()
def initdb():
    '''
    初始化数据库
    :return:
    '''
    click.echo('Initialized the database')

@click.command()
def dropdb():
    '''
    删除数据库
    :return:
    '''
    click.echo('Dropped the database')

cli.add_command(initdb)
cli.add_command(dropdb)

if __name__ == '__main__':
    cli()
```



还可以使用更简单的方法来实现相同的功能

```python
import click

@click.group()
def cli():
    pass

@cli.command()
def initdb():
    '''
    初始化数据库
    :return:
    '''
    click.echo('Initialized the database')

@cli.command()
def dropdb():
    '''
    删除数据库
    :return:
    '''
    click.echo('Dropped the database')

if __name__ == '__main__':
    cli()
```



在函数里增加了注释，目的是使用--help命令时可以查看注释

```text
python3 demo.py --help

sage: demo.py [OPTIONS] COMMAND [ARGS]...

Options:
  --help  Show this message and exit.

Commands:
  dropdb  删除数据库 :return:
  initdb  初始化数据库 :return:
```



实际使用

```text
python3 demo.py dropdb
```





# 打包 setup

说是命令行工具，但是仍然是用python命令在执行python脚本，想要实现那种直接使用命令的效果，需要将代码打包，以2.4中的代码为例，在demo.py同目录下新建一个setup.py文件，内容为

```python
from setuptools import setup

setup(
    name='click-example-db',
    version='0.1',
    py_modules=['cooldb'],
    include_package_data=True,
    install_requires=[
        'click',
    ],
    entry_points='''
        [console_scripts]
        cooldb=demo:cli
    ''',
)
```

1. name 就是包的名字
2. version 指定了版本
3. py_modules 指定了模块名称
4. entry_points 设置成[console_scripts]， 表明要安装成可以在终端调用的命令模块，
5. cooldb=demo:cli，cooldb是命令，执行该命令时，等同于执行demo.py的cli



最关键的一步了，使用pip进行打包安装

```shell
pip install --editable .	#注意editable后面的那个点，千万别落下
```

安装过程：

```text
Obtaining file:///Users/kwsy/kwsy/coolpython
Requirement already satisfied: click in /Library/Frameworks/Python.framework/Versions/3.6/lib/python3.6/site-packages (from click-example-db==0.1) (7.0)
Installing collected packages: click-example-db
  Running setup.py develop for click-example-db
Successfully installed click-example-db
```





安装成功后，就可以像使用pip命令一样使用cooldb命令了

```text
cooldb dropdb
Dropped the database
```



```shell
cooldb --help

Usage: cooldb [OPTIONS] COMMAND [ARGS]...

Options:
  --help  Show this message and exit.

Commands:
  dropdb  删除数据库 :return:
  initdb  初始化数据库 :return:
```