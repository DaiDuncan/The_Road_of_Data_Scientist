[Indexing](https://numpy.org/doc/1.19/user/basics.indexing.html#)

- [Assignment vs referencing](https://numpy.org/doc/1.19/user/basics.indexing.html#assignment-vs-referencing)
- [Single element indexing](https://numpy.org/doc/1.19/user/basics.indexing.html#single-element-indexing)
- [Other indexing options](https://numpy.org/doc/1.19/user/basics.indexing.html#other-indexing-options)
- [Index arrays](https://numpy.org/doc/1.19/user/basics.indexing.html#index-arrays)
- [Indexing Multi-dimensional arrays](https://numpy.org/doc/1.19/user/basics.indexing.html#indexing-multi-dimensional-arrays)
- [Boolean or “mask” index arrays](https://numpy.org/doc/1.19/user/basics.indexing.html#boolean-or-mask-index-arrays)
- [Combining index arrays with slices](https://numpy.org/doc/1.19/user/basics.indexing.html#combining-index-arrays-with-slices)
- [Structural indexing tools](https://numpy.org/doc/1.19/user/basics.indexing.html#structural-indexing-tools)
- [Assigning values to indexed arrays](https://numpy.org/doc/1.19/user/basics.indexing.html#assigning-values-to-indexed-arrays)
- [Dealing with variable numbers of indices within programs](https://numpy.org/doc/1.19/user/basics.indexing.html#dealing-with-variable-numbers-of-indices-within-programs)



Array indexing：基于方括号[]进行索引

## 索引的基本操作：[]

```python
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

### 切片
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

## 花式技巧|嵌套ndarray作为整数
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


### 逻辑表达式
>>> b = y>20
>>> y[b]
array([21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34])

m of y
array([False, False, False,  True,  True])
>>> y[b[:,5]]
array([[21, 22, 23, 24, 25, 26, 27],
       [28, 29, 30, 31, 32, 33, 34]])


### 高维数组
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





## 结合index arrays & 切片

```python
>>> y[np.array([0, 2, 4]), 1:3]
array([[ 1,  2],
       [15, 16],
       [29, 30]])

# 等价表示
>>> y[:, 1:3][np.array([0, 2, 4]), :]
array([[ 1,  2],
       [15, 16],
       [29, 30]])
```



## ⭐结构化的索引工具

```python
>>> y.shape
(5, 7)

### np.newaxis建立一个新的维度
>>> y[:,np.newaxis,:].shape  
(5, 1, 7)


>>> x = np.arange(5)
>>> x[:,np.newaxis] + x[np.newaxis,:]   #暗含广播机制
# x[:,np.newaxis] ==> [0, 1, 2, 3, 4]
# x[np.newaxis,:] ==> [0, 1, 2, 3, 4].T
array([[0, 1, 2, 3, 4],
       [1, 2, 3, 4, 5],
       [2, 3, 4, 5, 6],
       [3, 4, 5, 6, 7],
       [4, 5, 6, 7, 8]])


### 省略号
>>> z = np.arange(81).reshape(3,3,3,3)
# 分析过程：关注最小矩阵(最后两个维度)，作为一个单元 3*3
'''==>完整单元，比如第一个单元
[[0,1,2],
 [3,4,5],
 [6,7,8]]
'''

'''==>完整单元记为A B C
[[A1
  B1
  C1],
 [A2
  B2
  C2],
 [A3
  B3
  C3]]
'''

>>> z[1,...,2]   #等价于z[1,:,:,2]
# 第一个1表示A2 B2 C2的集合
# 最后一个2选择其中第2列 @如下图所示

'''综上[a, b, c, d]意义:
a：从最外的[]开始 ==> 第几个高维单元
b：同a
(如果a,b中仍有其它维度：同理)
c：最小单元的行
d：最小单元的列
'''

array([[29, 32, 35],
       [38, 41, 44],
       [47, 50, 53]])
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201209115343.png" alt="image-20201209115343713" style="zoom:67%;" />



## 赋值|给indexed arrays

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



## 索引：当数量范围是一个变量时

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

