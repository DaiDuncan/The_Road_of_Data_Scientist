ufuncs: Universal Functions

- 通用函数都已经向量化：比在元素上迭代要快很多
  - 速度更快，因为现代的CPU对这种操作进行了优化
- 还提供了广播和额外的方法，如 reduce, accumulate 等，对计算进行优化。

ufuncs也接受额外的参数：

- where 布尔数组或条件，定义**操作发生的位置**。
- dtype 定义了元素的**返回类型**。
- out 输出数组，返回值**应该被复制**。



```python
#==# 一般元素迭代的方法
x = [1, 2, 3, 4]
y = [4, 5, 6, 7]
z = []

for i, j in zip(x, y):
  z.append(i + j)
print(z)


#==# 向量化
import numpy as np

x = [1, 2, 3, 4]
y = [4, 5, 6, 7]
z = np.add(x, y)
print(z)
```



# 创建ufunc frompyfunc()

The `frompyfunc()` method takes the following arguments:

1. `function` - the name of the function.
2. `inputs` - the number of input arguments (arrays).
3. `outputs` - the number of output arrays.

```python
import numpy as np

def myadd(x, y):
  return x+y

myadd = np.frompyfunc(myadd, 2, 1)
print(myadd([1, 2, 3, 4], [5, 6, 7, 8]))	#[6 8 10 12]
```



## 检查: 是否是ufunc

```python
import numpy as np

print(type(np.add))	#<class 'numpy.ufunc'>
print(type(np.concatenate)) #<class 'builtin_function_or_method'>
```



```python
import numpy as np

if type(np.add) == np.ufunc:
  print('add is ufunc')
else:
  print('add is not ufunc')
```

---

# [官方API](https://numpy.org/doc/stable/reference/routines.math.html)

三角函数

- sin(), cos(), tan()
- 反三角函数
- 角度转换

双曲函数

小数取整

和、积、差

- 积分
- 梯度

指数、对数

特殊：

- bessel函数
- sinc采样函数

浮点数

有理数

- 最小公倍数
- 最大公约数

算术运算

处理复数



# 库|np.emath

是[`numpy.lib.scimath`](https://numpy.org/doc/stable/reference/routines.emath.html#module-numpy.lib.scimath)的同名引用

```python
>>> import math
>>> from numpy.lib import scimath
>>> scimath.log(-math.exp(1)) == (1+1j*math.pi)
True
```

| [`sqrt`](https://numpy.org/doc/stable/reference/generated/numpy.lib.scimath.sqrt.html#numpy.lib.scimath.sqrt)(x) | Compute the square root of x.                  |
| ------------------------------------------------------------ | ---------------------------------------------- |
| [`log`](https://numpy.org/doc/stable/reference/generated/numpy.lib.scimath.log.html#numpy.lib.scimath.log)(x) | Compute the natural logarithm of *x*.          |
| [`log2`](https://numpy.org/doc/stable/reference/generated/numpy.lib.scimath.log2.html#numpy.lib.scimath.log2)(x) | Compute the logarithm base 2 of *x*.           |
| [`logn`](https://numpy.org/doc/stable/reference/generated/numpy.lib.scimath.logn.html#numpy.lib.scimath.logn)(n, x) | Take log base n of x.                          |
| [`log10`](https://numpy.org/doc/stable/reference/generated/numpy.lib.scimath.log10.html#numpy.lib.scimath.log10)(x) | Compute the logarithm base 10 of *x*.          |
| [`power`](https://numpy.org/doc/stable/reference/generated/numpy.lib.scimath.power.html#numpy.lib.scimath.power)(x, p) | Return x to the power p, (x**p).               |
| [`arccos`](https://numpy.org/doc/stable/reference/generated/numpy.lib.scimath.arccos.html#numpy.lib.scimath.arccos)(x) | Compute the inverse cosine of x.               |
| [`arcsin`](https://numpy.org/doc/stable/reference/generated/numpy.lib.scimath.arcsin.html#numpy.lib.scimath.arcsin)(x) | Compute the inverse sine of x.                 |
| [`arctanh`](https://numpy.org/doc/stable/reference/generated/numpy.lib.scimath.arctanh.html#numpy.lib.scimath.arctanh)(x) | Compute the inverse hyperbolic tangent of *x*. |



# 基本算术

所有的算术函数都有参数`where`

## 1 Addition

```python
import numpy as np

arr1 = np.array([10, 11, 12, 13, 14, 15])
arr2 = np.array([20, 21, 22, 23, 24, 25])
newarr = np.add(arr1, arr2)
print(newarr)	#[30 32 34 36 38 40] 
```



### 1.1 Summation

Addition只涉及两个元素，Summation可以包含多个元素

```python
import numpy as np

arr1 = np.array([1, 2, 3])
arr2 = np.array([1, 2, 3])
newarr = np.sum([arr1, arr2])
print(newarr)	#12


#==# 基于某个轴的summation
import numpy as np

arr1 = np.array([1, 2, 3])
arr2 = np.array([1, 2, 3])
newarr = np.sum([arr1, arr2], axis=1)	#跨列：针对行的运算
print(newarr)	#[6 6]
```



### 1.2 cumsun()累计求和

[1, 2, 3, 4] would be [1, 1+2, 1+2+3, 1+2+3+4] = [1, 3, 6, 10].

```python
import numpy as np

arr = np.array([1, 2, 3])
newarr = np.cumsum(arr)
print(newarr)	#[1 3 6]
```





## 2 Subtraction

```python
import numpy as np

arr1 = np.array([10, 20, 30, 40, 50, 60])
arr2 = np.array([20, 21, 22, 23, 24, 25])
newarr = np.subtract(arr1, arr2)
print(newarr)	#[-10 -1 8 17 26 35]
```



## 2.1 np.diff(arr)

```python
import numpy as np

arr = np.array([10, 15, 25, 5])
newarr = np.diff(arr)
print(newarr)	#[5 10 -20] because 15-10=5, 25-15=10, and 5-25=-20
```



```python
#==# 多次操作：参数n
import numpy as np

arr = np.array([10, 15, 25, 5])
newarr = np.diff(arr, n=2)
print(newarr)	#[5 -30] because: 15-10=5, 25-15=10, and 5-25=-20 AND 10-5=5 and -20-10=-30
```

E.g. for [1, 2, 3, 4], 

- the discrete difference with n = 2 would be [2-1, 3-2, 4-3] = [1, 1, 1] , 
- then, since n=2, we will do it once more, with the new result: [1-1, 1-1] = [0, 0]



## 3 Multiplication

```python
import numpy as np

arr1 = np.array([10, 20, 30, 40, 50, 60])
arr2 = np.array([20, 21, 22, 23, 24, 25])
newarr = np.multiply(arr1, arr2)
print(newarr)	#[200 420 660 920 1200 1500]
```

### 3.1 np.prod(arr)

```python
import numpy as np

arr = np.array([1, 2, 3, 4])
x = np.prod(arr)
print(x)	#1*2*3*4 = 24


arr1 = np.array([1, 2, 3, 4])
arr2 = np.array([5, 6, 7, 8])
x = np.prod([arr1, arr2])
print(x)	#1*2*3*4*5*6*7*8 = 40320
```



```python
#==# 针对某个轴
import numpy as np

arr1 = np.array([1, 2, 3, 4])
arr2 = np.array([5, 6, 7, 8])
newarr = np.prod([arr1, arr2], axis=1)
print(newarr)	#[24 1680]
```



### 3.2 np.cumprod(arr)

```python
import numpy as np

arr = np.array([5, 6, 7, 8])
newarr = np.cumprod(arr)
print(newarr)	#[5 30 210 1680]
```







## 4 Division

```python
import numpy as np

arr1 = np.array([10, 20, 30, 40, 50, 60])
arr2 = np.array([3, 5, 10, 8, 2, 33])
newarr = np.divide(arr1, arr2)
print(newarr)	#[3.33333333 4. 3. 5. 25. 1.81818182]
```



## 5 Power

```python
import numpy as np

arr1 = np.array([10, 20, 30, 40, 50, 60])
arr2 = np.array([3, 5, 6, 8, 2, 33])
newarr = np.power(arr1, arr2)
print(newarr)	#[1000 3200000 729000000 6553600000000 2500 0] 最后的0应该是溢出了
```



## 6 Remainder余数

```python
import numpy as np

arr1 = np.array([10, 20, 30, 40, 50, 60])
arr2 = np.array([3, 7, 9, 8, 2, 33])
newarr = np.mod(arr1, arr2)	#等价于newarr = np.remainder(arr1, arr2)
print(newarr)	#[1 6 3 0 0 27]
```



## 7 Quotient and Mod商和余数

```python
import numpy as np

arr1 = np.array([10, 20, 30, 40, 50, 60])
arr2 = np.array([3, 7, 9, 8, 2, 33])
newarr = np.divmod(arr1, arr2)
print(newarr)	#(array([3, 2, 3, 5, 25, 1]), array([1, 6, 3, 0, 0, 27]))
```



## 8 Absolute Values绝对值

```python
import numpy as np

arr = np.array([-1, -2, 1, 2, 3, -4])
newarr = np.absolute(arr)
print(newarr)	#[1 2 1 2 3 4]
```



## 9 倒数

numpy.reciprocal() 函数返回参数逐元素的倒数。如 **1/4** 倒数为 **4/1**。

```python
import numpy as np 
 
a = np.array([0.25,  1.33,  1,  100])  
print ('我们的数组是：')
print (a)
print ('\n')
print ('调用 reciprocal 函数：')
print (np.reciprocal(a))

'''
我们的数组是：
[  0.25   1.33   1.   100.  ]


调用 reciprocal 函数：
[4.        0.7518797 1.        0.01     ]
'''
```





# 小数取整

- truncation 截断去尾
- fix 固定小数点 => 等价于截断
- rounding
- floor
- ceil

```python
import numpy as np

#==# 截断去尾
arr = np.trunc([-3.1666, 3.6667])	#等价于arr = np.fix([-3.1666, 3.6667])
print(arr)	#[-3.  3.] 去掉小数点 => 更接近0


#==# 四舍五入
arr = np.around(3.1666, 2)	#保留2位小数
print(arr)


#==# 最近更小的整数
arr = np.floor([-3.1666, 3.6667]) 
print(arr)	#[-4.  3.]


#==# 最近更大的整数
arr = np.ceil([-3.1666, 3.6667])
print(arr)	#[-3.  4.]
```



# log对数

不同的基底：

- 2
- e
- 10

```python
import numpy as np

arr = np.arange(1, 10)
print(np.log2(arr))


arr = np.arange(1, 10)
print(np.log10(arr))


arr = np.arange(1, 10)
print(np.log(arr))


#==# 任意底数
from math import log
import numpy as np

nplog = np.frompyfunc(log, 2, 1)
print(nplog(100, 15))
```

备注：np.frompyfunc(log, 2, 1)

- numpy中没有提供任意底数的log函数 => 基于frompyfunc()，使用内置函数math.log()
- 参数含义：2个输入，1个输出



python中的对数函数：

```python
import math
# 不写底数时默认以e为底
math.log(100)

# 以2 e 10为底
math.log2(100)
math.log10(100)
```



# 最小公倍数Lowest Common Multiple

```python
import numpy as np

num1 = 4
num2 = 6
x = np.lcm(num1, num2)
print(x)	#12
```



### 数组 => reduce()

```python
import numpy as np

arr = np.array([3, 6, 9])
x = np.lcm.reduce(arr)	#基于lcm的连乘reduce
print(x)	#18
```



# 最大公约数Greatest Common Denominator

```python
import numpy as np

num1 = 6
num2 = 9
x = np.gcd(num1, num2)
print(x)	#3
```



### 数组 => reduce()

```python
import numpy as np

arr = np.array([20, 8, 32, 36, 16])
x = np.gcd.reduce(arr)
print(x)	#4
```

 



# 三角函数

| 【基本三角函数】              |                                                              |
| ----------------------------- | ------------------------------------------------------------ |
| np.sin(np.pi/2)               |                                                              |
| .cos()                        |                                                              |
| .tan()                        |                                                              |
|                               |                                                              |
| 【反三角函数】                |                                                              |
| .arcsin()                     |                                                              |
| .arccos()                     |                                                              |
| .arctan()                     |                                                              |
|                               |                                                              |
| 【直角三角形】Hypotenues 斜边 | NumPy提供了hypot()函数，该函数接收基数和垂直值并根据勾股定理生成斜边。 |
|                               |                                                              |

```python
import numpy as np

arr = np.array([np.pi/2, np.pi/3, np.pi/4, np.pi/5])
x = np.sin(arr)
print(x)



#==# 角度转变为弧度
arr = np.array([90, 180, 270, 360])
x = np.deg2rad(arr)
print(x)


#==# 弧度转变为角度
# 等价函数：np.degrees(弧度)
arr = np.array([np.pi/2, np.pi, 1.5*np.pi, 2*np.pi])
x = np.rad2deg(arr)
print(x)
```



```python
#==# 直角三角形
import numpy as np

base = 3
perp = 4
x = np.hypot(base, perp)
print(x)	#结果是5
```



# 双曲三角函数Hyperbolic

- sinh(), cosh() and tanh()
- arcsinh(), arccosh() and arctanh()

