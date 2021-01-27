None不能理解为0，因为==0是有意义的==，而None是一个特殊的空值

None在python中是一个特殊的对象，它表示空值，其类型为NoneType

```python
>>> type(None)
<class 'NoneType'>
```



None只存在一个，python解释器启动时创建，解释器退出时销毁

```python
>>> a = None
>>> b = None
>>> a == b
True
>>> a is b
True
```



# None的运算

不支持任何运算，也没有内建方法，除了表示空以外，什么都做不了。

如果要判断一个对象是否为None,==使用is身份运算符==

```python
>>> a = None
>>> a is None
True
```



# None的使用

如果一个函数，没有显式return任何数据，则默认返回None

在判断语句中，None等价于False

```python
>>> a = None
>>> not a
True
```





# #参考文献

[Link: 酷Python](http://www.coolpython.net/python_primary/function/none.html)