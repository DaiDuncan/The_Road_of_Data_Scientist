# Python-技巧 | max/min函数

**声明**：以下内容主要基于[cnblogs | max/min函数的用法](https://www.cnblogs.com/whatisfantasy/p/6273913.html)

```python
## 源码

def max(*args, key=None): # known special case of max
    """
    max(iterable, *[, default=obj, key=func]) -> value
    max(arg1, arg2, *args, *[, key=func]) -> value

    With a single iterable argument, return its biggest item. The
    default keyword-only argument specifies an object to return if
    the provided iterable is empty.
    With two or more arguments, return the largest argument.
    """
    pass
```



## 一 初级技巧

```python
tmp = max(1,2,4)
print(tmp)


# 可迭代对象
a = [1, 2, 3, 4, 5, 6]
tmp = max(a)
print(tmp)
```





## 二 进阶技巧：添加属性key

当key参数不为空时，就以**key的函数对象**为判断的标准。

如果我们想找出一组数中绝对值最大的数，就可以配合lamda先进行处理，再找出最大值

```python
a = [-9, -8, 1, 3, -4, 6]
tmp = max(a, key=lambda x: abs(x))
print(tmp)
```





## 三 高级技巧：找出字典中值最大的那组数据

如果有一组商品，其名称和价格都存在一个字典中，可以用下面的方法快速找到价格最贵的那组商品：

```python
prices = {
    'A':123,
    'B':450.1,
    'C':12,
    'E':444,
}
# 在对字典进行数据操作的时候，默认只会处理key，而不是value
# 先使用zip把字典的keys和values翻转过来，再用max取出值最大的那组数据
max_prices = max(zip(prices.values(), prices.keys()))
print(max_prices) # (450.1, 'B')


## 当字典中的value相同的时候，才会比较key：
prices = {
    'A': 123,
    'B': 123,
}

max_prices = max(zip(prices.values(), prices.keys()))
print(max_prices) # (123, 'B')

min_prices = min(zip(prices.values(), prices.keys()))
print(min_prices) # (123, 'A')
```







## 参考文献

[cnblogs | max/min函数的用法](https://www.cnblogs.com/whatisfantasy/p/6273913.html)