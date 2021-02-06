需求总是在变化，代码总是在修改，项目总是在重构

如果你不认真维护自己的项目，让糟糕的代码积累的越来越多，迟早有一天会积重难返



修改代码或者重构过程中，经常会出现这样的情况，一些函数已经不再使用了，但是出于某种原因，这些代码没有被删除，而是被保留了下来

```text
pip install dead
```



工具需要配合git来使用，它首先使用git ls-files命令来获取所有的文件，然后逐一进行检查，将每一个文件编译成字节码，寻找定义和引用，==如果最终找不到引用==，就会给出提示

```text
kwsy@zhangdongshengdeMacBook-Air:~/kwsy/coolpython$     dead
chapter is never read, defined in app.py:31
html is never read, defined in app.py:31
chapter is never read, defined in app.py:42
html is never read, defined in app.py:42
index is never read, defined in app.py:17
python_primary_tutorial is never read, defined in app.py:23
python_primary is never read, defined in app.py:29
python_senior_index is never read, defined in app.py:35
python_senior is never read, defined in app.py:40
filter is never read, defined in common/log.py:39
category is never read, defined in demo.py:2
fly is never read, defined in demo.py:7
PRIMARY is never read, defined in utils/generate_html.py:8
```

那些只有定义却没有被调用执行的函数都会被检查出来，虽然大部分都是正常的代码。



据作者自己所言，他是在飞机上花了15分钟完成的这个项目。

它还不是一个完美的项目，个别时候还会出现误报，但是那些真正的只有定义而没有调用的函数都会被检查出来，这样可以帮助你寻找项目里的死代码。





