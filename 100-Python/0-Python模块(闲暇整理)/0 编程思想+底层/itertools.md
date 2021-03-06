`itertools`提供了非常有用的用于==操作迭代对象==的函数



# 无限序列

## .count()

```python
>>> import itertools
>>> natuals = itertools.count(1)
>>> for n in natuals:
...     print(n)
...
1
2
3
...
```

`count()`会创建一个无限的迭代器，所以上述代码会打印出自然数序列，根本停不下来，只能按`Ctrl+C`退出





## .cycle()

把传入的一个序列无限重复下去：同样停不下来

```python
>>> import itertools
>>> cs = itertools.cycle('ABC') # 注意字符串也是序列的一种
>>> for c in cs:
...     print(c)
...
'A'
'B'
'C'
'A'
'B'
'C'
...
```





## .repeat()

把一个元素无限重复下去，不过如果提供第二个参数就可以限定重复次数：

```python
>>> ns = itertools.repeat('A', 3)
>>> for n in ns:
...     print(n)
...
A
A
A
```



无限序列只有在`for`迭代时才会无限地迭代下去，如果只是创建了一个迭代对象，它不会事先把无限个元素生成出来，事实上也不可能在内存中创建无限多个元素。





无限序列虽然可以无限迭代下去，但是通常我们会通过`takewhile()`等函数根据条件判断来截取出一个有限的序列：

```python
>>> natuals = itertools.count(1)
>>> ns = itertools.takewhile(lambda x: x <= 10, natuals)
>>> list(ns)
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```



注意：遇到第一个不满足的条件就会终止

- 当x=2时，就会终止，而不会筛选所有2N-1以内的奇数

```python
num_odd = itertools.takewhile(lambda x: (x%2==1 and x <= 2*N -1), natuals)
```





# 迭代器操作函数

## chain()

把一组迭代对象串联起来，形成一个更大的迭代器：

```python
>>> for c in itertools.chain('ABC', 'XYZ'):
...     print(c)
# 迭代效果：'A' 'B' 'C' 'X' 'Y' 'Z'
```





## groupby()

把迭代器中相邻的重复元素挑出来放在一起：

```python
>>> for key, group in itertools.groupby('AAABBBCCAAA'):
...     print(key, list(group))
...
A ['A', 'A', 'A']
B ['B', 'B', 'B']
C ['C', 'C']
A ['A', 'A', 'A']
```

实际上挑选规则是通过函数完成的，只要作用于函数的两个元素返回的值相等，这两个元素就被认为是在一组的，而函数返回值作为组的key。



如果我们要忽略大小写分组，就可以让元素`'A'`和`'a'`都返回相同的key：

```python
>>> for key, group in itertools.groupby('AaaBBbcCAAa', lambda c: c.upper()):
...     print(key, list(group))
...
A ['A', 'a', 'a']
B ['B', 'B', 'b']
C ['c', 'C']
A ['A', 'A', 'a']
```



# #示例

## 计算这个序列的前N项和

```python
# -*- coding: utf-8 -*-
import itertools

def pi(N):
    ' 计算pi的值 '
    # step 1: 创建一个奇数序列: 1, 3, 5, 7, 9, ...
    natuals = itertools.count(1, 2)
    
    # step 2: 取该序列的前N项: 1, 3, 5, 7, 9, ..., 2*N-1.
    num_odd = itertools.takewhile(lambda x: x<=2*N -1, natuals)

    
    # step 3: 添加正负符号并用4除: 4/1, -4/3, 4/5, -4/7, 4/9, ...
    # step 4: 求和:
    ## c=map(lambda x:4/x if (x+1)/2%2==1 else -4/x,b)   
    ## 在上述添加+-号的基础上，可以用reduce来计算累和
    sum = 0
    for num, sign in zip(num_odd, itertools.cycle('+-')):
        value = int(sign+str(num))
        sum += 4/value

    return sum



# 测试:
print(pi(10))
print(pi(100))
print(pi(1000))
print(pi(10000))
assert 3.04 < pi(10) < 3.05
assert 3.13 < pi(100) < 3.14
assert 3.140 < pi(1000) < 3.141
assert 3.1414 < pi(10000) < 3.1415
print('ok')
```





---

`itertools`模块提供的全部是处理迭代功能的函数，它们的返回值不是list，而是`Iterator`

只有用`for`循环迭代的时候才真正计算。

