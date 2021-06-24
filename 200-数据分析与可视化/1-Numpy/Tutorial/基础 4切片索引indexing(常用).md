[link: 官方API|routines](https://numpy.org/doc/stable/reference/routines.indexing.html)



Array indexing：基于方括号[]进行索引

注意：indexing和slicing是两回事

- indexing按照下标进行索引：从0开始
- slicing：`[start:end:step]` => ==[start, end)==
  - 切片对象可以通过内置的 slice 函数 => 本质就是`[start:end:step]`
  - 切片向量既可以为array,也可以为list类型 => 比如[1, 2, 4]  =>  np.array([1,2,4])
- `<slice> = <array>[start_row:end_row, start_col:end_col]`

## 基本操作

- 注意负数的含义

```python
a = np.arange(10)
s = slice(2,7,2)   # 从索引 2 开始到索引 7 停止，间隔为2
print (a[s])	#[2  4  6]


# 一维
>>> x = np.arange(10)
>>> x[2]
2
>>> x[-2]
8


# 二维
>>> x.shape = (2,5) # now x is 2-dimensional
>>> x[1,3]
8
>>> x[1,-1]
9

>>> x[0]
array([0, 1, 2, 3, 4])
>>> x[0][2]   #等价于x[0, 2]
2


>>> b = a[:,:]  #[all rows, all columns]

#==# 切片
>>> x = np.arange(10)
>>> x[2:5]
array([2, 3, 4])
>>> x[:-7]
array([0, 1, 2])
>>> x[1:7:2]
array([1, 3, 5])
>>> y = np.arange(35).reshape(5,7)
>>> y[1:5:2,::3]
array([[ 7, 10, 13],
       [21, 24, 27]])


#==# 花式技巧|嵌套ndarray作为整数
>>> x = np.arange(10,1,-1)
>>> x
array([10,  9,  8,  7,  6,  5,  4,  3,  2])
>>> x[np.array([3, 3, 1, 8])]
array([7, 7, 9, 2])
>>> x[np.array([3,3,-3,8])]
array([7, 7, 4, 2])

>>> x[np.array([3, 3, 20, 8])]   #问题：索引数值的范围越界
<type 'exceptions.IndexError'>: index 20 out of bounds 0<=index<9

>>> x[np.array([[1,1],[2,3]])]
array([[9, 9],
       [8, 7]])



#==# 逻辑表达式
>>> b = y>20
>>> y[b]
array([21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34])

m of y
array([False, False, False,  True,  True])
>>> y[b[:,5]]
array([[21, 22, 23, 24, 25, 26, 27],
       [28, 29, 30, 31, 32, 33, 34]])



#==# 高维数组
>>> x = np.arange(30).reshape(2,3,5)  #2个list，每个list是3行5列
>>> x
array([[[ 0,  1,  2,  3,  4],
        [ 5,  6,  7,  8,  9],
        [10, 11, 12, 13, 14]],
       [[15, 16, 17, 18, 19],
        [20, 21, 22, 23, 24],
        [25, 26, 27, 28, 29]]])
# 第一个list：选择第一、二行
# 第二个list：选择后两行
>>> b = np.array([[True, True, False], [False, True, True]])   
>>> x[b]
array([[ 0,  1,  2,  3,  4],
       [ 5,  6,  7,  8,  9],
       [20, 21, 22, 23, 24],
       [25, 26, 27, 28, 29]])
```





## 拓展: 基于ndarray的index

```python
>>> y[np.array([0, 2, 4]), 1:3]	#y = np.arange(35).reshape(5,7)
array([[ 1,  2],
       [15, 16],
       [29, 30]])

# 等价表示
>>> y[:, 1:3][np.array([0, 2, 4]), :]	#0,2,4行，1:3列
array([[ 1,  2],
       [15, 16],
       [29, 30]])
```





## 拓展: 基于变量的index

```python
>>> indices = (1,1,1,1)   #上述z = np.arange(81).reshape(3,3,3,3)
>>> z[indices]
40

# 切片
>>> indices = (1,1,1,slice(0,2)) # same as [1,1,1,0:2]
>>> z[indices]
array([39, 40])

# 省略号
>>> indices = (1, Ellipsis, 1) # same as [1,...,1]
>>> z[indices]
array([[28, 31, 34],
       [37, 40, 43],
       [46, 49, 52]])


>>> z[[1,1,1,1]] #不常用：produces a large array
'''
每一次取z[1]，然后形成一个列表
'''
array([[[[27, 28, 29],
         [30, 31, 32], ...
>>> z[(1,1,1,1)] #元组：returns a single value
40
```





## 切片 => 赋值

- 选择数组的子集
  - single index
  - slices
  - index and mask arrays

```python
>>> x = np.arange(10)
>>> x[2:7] = 1
# 注意数量的匹配
>>> x[2:7] = np.arange(5)

# 当x[1]最初是整数时:赋值从浮点数变为整型
>>> x[1] = 1.2
>>> x[1]
1

# 当x[1]最初是整数时：无法赋值-复数、浮点数
>>> x[1] = 1.2j
TypeError: can't convert complex to int
    
    
>>> x = np.arange(0, 50, 10)
>>> x
array([ 0, 10, 20, 30, 40])
>>> x[np.array([1, 1, 3, 1])] += 1
# 最初的想法：第一个元素10连续加3次，第3个元素加1次
### 注意底层机理：原始数据参与计算时会生成一个临时数据，每次针对临时数据进行操作
# 结果：x[1]+1进行3次，每一次的x[1]都是从原始数据中提取，也就是10
>>> x
array([ 0, 11, 20, 31, 40])
```



# 高级索引

## 整数数组索引

```python
import numpy as np 
 
x = np.array([[1,  2],  [3,  4],  [5,  6]]) 
y = x[[0,1,2],  [0,1,0]]	#获取数组中(0,0)，(1,1)和(2,0)位置处的元素
print (y)	#[1  4  5] => 按照x中的索引 => 形成1D数组




#==# 获取4X3 数组中的四个角的元素。 
# 行索引是 [0,0] 和 [3,3]，而列索引是 [0,2] 和 [0,2]
x = np.array([[  0,  1,  2],[  3,  4,  5],[  6,  7,  8],[  9,  10,  11]])  
print ('我们的数组是：' )
print (x)
print ('\n')
rows = np.array([[0,0],[3,3]]) 
cols = np.array([[0,2],[0,2]]) 
y = x[rows,cols]  
print  ('这个数组的四个角元素是：')
print (y)
'''按照rows, cols中的索引 => 形成2D数组
我们的数组是：
[[ 0  1  2]
 [ 3  4  5]
 [ 6  7  8]
 [ 9 10 11]]


这个数组的四个角元素是：
[[ 0  2]
 [ 9 11]]
'''
```



`:`, `...`等同于某个整数数组

```python
import numpy as np
 
a = np.array([[1,2,3], [4,5,6],[7,8,9]])
b = a[1:3, 1:3]
c = a[1:3,[1,2]]	#比如1:3 等价于[1, 2]
d = a[...,1:]
print(b)
print(c)
print(d)
'''
[[5 6]
 [8 9]]
[[5 6]
 [8 9]]
[[2 3]
 [5 6]
 [8 9]]
'''
```



## 布尔索引

通过一个布尔数组来索引目标数组

```python
import numpy as np 
 
x = np.array([[  0,  1,  2],[  3,  4,  5],[  6,  7,  8],[  9,  10,  11]])  
print ('我们的数组是：')
print (x)
print ('\n')
# 现在我们会打印出大于 5 的元素  
print  ('大于 5 的元素是：')
print (x[x >  5])

'''
我们的数组是：
[[ 0  1  2]
 [ 3  4  5]
 [ 6  7  8]
 [ 9 10 11]]


大于 5 的元素是：
[ 6  7  8  9 10 11]
'''
```



 **~**（取补运算符）来过滤 NaN：

```python
import numpy as np 
 
a = np.array([np.nan,  1,2,np.nan,3,4,5])  
print (a[~np.isnan(a)])
```



如何从数组中过滤掉非复数元素：

```python
import numpy as np 
 
a = np.array([1,  2+6j,  5,  3.5+5j])  
print (a[np.iscomplex(a)])	#[2.0+6.j  3.5+5.j]
```



## 花式索引

利用整数数组进行索引

花式索引根据索引数组的值作为目标数组的某个轴的下标来取值。对于使用一维整型数组作为索引，如果目标是一维数组，那么索引的结果就是对应位置的元素；如果目标是二维数组，那么就是对应下标的行。

花式索引跟切片不一样，它==总是将数据复制到新数组中==。



1 传入顺序索引数组

```python
import numpy as np 
 
x=np.arange(32).reshape((8,4))
print (x[[4,2,1,7]])	#原型是x[]，数组[4,2,1,7]表示对应下标的行序列/数组
'''
[[16 17 18 19]
 [ 8  9 10 11]
 [ 4  5  6  7]
 [28 29 30 31]]
'''
```



2 传入倒序索引数组

```python
import numpy as np 
 
x=np.arange(32).reshape((8,4))
print (x[[-4,-2,-1,-7]])
'''
[[16 17 18 19]
 [24 25 26 27]
 [28 29 30 31]
 [ 4  5  6  7]]
'''
```



3 传入多个索引数组（要使用np.ix_）

```python
import numpy as np 
 
x=np.arange(32).reshape((8,4))
print (x[np.ix_([1,5,7,2],[0,3,1,2])])	#行序列是[1,5,7,2]，列的顺序是[0,3,1,2]
'''
[[ 4  7  5  6]
 [20 23 21 22]
 [28 31 29 30]
 [ 8 11  9 10]]
'''
```



`help(numpy.ix_)`

```python
#x[np.ix_([1,5,7,2],[0,3,1,2])]这句话会输出一个4*4的矩阵
x[1,0] x[1,3] x[1,1] x[1,2]
x[5,0] x[5,3] x[5,1] x[5,2]
x[7,0] x[7,3] x[7,1] x[7,2]
x[2,0] x[2,3] x[2,1] x[2,2]
```

