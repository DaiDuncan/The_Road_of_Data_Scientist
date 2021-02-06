# (ing...)Python-积累 | 细节

## 一 待分类

- Windows默认GBK：适合命令行
- 链式比较：如 42 < res < 50
- 内置方法：前后各两个下划线(不允许外部访问)
- Lambda：快速创建在代码中，以后不会用到的函数

- - map(func, iter)：将该函数应用到，可迭代对象的每个元素的迭代器
  - filter(func, iter)：返回一个由可迭代对象中的特定元素对应的True

- 生成器yield()——>迭代器

当内存不够存储完整实现的列表时，或者计算每个列表元素的代价很高，你希望尽量推迟计算时，就可以使用生成器。但是这些元素**只能遍历一次**

- chunker()：接受一个可迭代对象并**每次生成指定大小**的部分数据@内存控制
- iter(object[, sentinel])：生成迭代器

- - object:支持迭代的集合对象

![image](https://cdn.nlark.com/yuque/0/2020/png/1136179/1591177149676-3376096c-db74-49c0-be57-079dca09e00b.png)

![image](https://cdn.nlark.com/yuque/0/2020/png/1136179/1591177149819-de880d13-083e-4cf5-81ee-80ddac2f37e0.png)



## 【标准库】

### 1 常用模块

> `math`  .factorial(x) 阶乘
>
> `random`  .randint(a,b)
>
> `datetime`  当前时间和日期
>
> ```
> os
> ```
>
> `sys`  直接使用 Python 解释器
>
> ```
> csv
> ```
>
> `zipfile`  从 zip 文件中提取所有文件
>
> `time` `timeit`  显示代码的运行时间



### 2 推荐模块

> `collections`  常见数据类型的实用扩展，包括 OrderedDict、defaultdict 和 namedtuple
>
> `string`  关于字符串的更多函数
>
> `re`  正则表达式
>
> `json` 读写 json 文件（面向网络开发）



### 3 第三方库(软件包)

> `IPython`  更好的交互式 Python 解释器
>
> `requests`  适用于访问网络 API
>
> `Flask`  小型框架，用于构建网络应用和 API
>
> `Django`  功能更丰富的网络应用构建框架
>
> `Beautiful Soup`  解析 HTML 并从中提取信息。适合网页数据抽取
>
> `pytest`  扩展了 Python 的内置断言，并且是最具单元性的模块
>
> `PyYAML`  读写 YAML 文件
>
> ```
> NumPy
> pandas
> ggplot
> ```
>
> `Pillow`  Python 图片库可以向你的 Python 解释器添加图片处理功能
>
> `pyglet`  面向游戏开发的跨平台应用框架
>
> `Pygame`  用于编写游戏的一系列 Python 模块
>
> `pytz`  Python 的世界时区定义