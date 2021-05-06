# Python-技巧 | range与xrange

声明：以下内容主要基于[cnblogs | range和xrange](https://www.cnblogs.com/whatisfantasy/p/5954123.html)



## 一 Python2.7

### 1 range

用户获取指定范围内的数

```python
range([start,] stop[, step])

>>> range(1,5)    #代表从1到5(不包含5)
[1, 2, 3, 4]
>>> range(1,5,2)    #代表从1到5，间隔2(不包含5)
[1, 3]
>>> range(5)    #代表从0到5(不包含5)
[0, 1, 2, 3, 4]


a = range (0,5)
print(type(a))

OUTPUT:
<type 'list'>
```



### 2 xrange

用法和range只有**在使用for的时候**才会逐个创建元素，提高了性能，建议使用xrange。

```python
a = xrange (0,5)
print(type(a))

OUTPUT:
<type 'xrange'>


## 两者对比
a = range(0,5)
b = xrange(0,5)

print a        
print b        

for ai in a:
    print ai

for bi in b:
    print bi

OUTPUT:
[0, 1, 2, 3, 4]
xrange(5)
0
1
2
3
4
0
1
2
3
4
```

从上面的运行结果可以看到，range会直接生成整个列表。而xrange返回的是一个生成器，生成器是一个可迭代对象，只有在对生成器进行迭代时（for循环），元素才逐个被创建。xrange大大提升了代码的执行效率，因此在python2.7中xrange的使用非常广泛。





## 二 python3.5

删去了原来的range，把原来的xrange改成了range

```python
a = range (0,5)
print(type(a))

OUTPUT:
<class 'range'>
```



## 参考文献

[cnblogs | range和xrange](https://www.cnblogs.com/whatisfantasy/p/5954123.html)