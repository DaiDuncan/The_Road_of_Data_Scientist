数组的拷贝和视图之间的主要区别是，拷贝是一个新的数组，而视图只是原数组的一个视图。

- 副本是一个数据的完整的拷贝
  - 如果我们对副本进行修改，它不会影响到原始数据，物理内存不在同一位置。
- 视图是数据的一个别称或引用，通过该别称或引用亦便可访问、操作原有数据，但原有数据不会产生拷贝。
  - 如果我们对视图进行修改，它会影响到原始数据，物理内存在同一位置。



**View视图一般发生在：**

- 1、numpy 的切片操作返回原数据的视图。
- 2、调用 ndarray 的 view() 函数产生一个视图。



**Copy副本一般发生在：**

- ==Python 序列的切片操作==，调用deepCopy()函数。
- 调用 ndarray 的 copy() 函数产生一个副本。



## 无复制

简单的赋值**不会创建数组对象的副本**。 相反，它**使用原始数组的相同id()**来访问它。 

id()返回 Python 对象的通用标识符，类似于 C 中的指针。

此外，一个数组的任何变化都反映在另一个数组上。 例如，一个数组的形状改变也会改变另一个数组的形状。

```python
import numpy as np 
 
a = np.arange(6)  
print ('我们的数组是：')
print (a)
print ('调用 id() 函数：')
print (id(a))
print ('a 赋值给 b：')
b = a 
print (b)
print ('b 拥有相同 id()：')
print (id(b))
print ('修改 b 的形状：')
b.shape =  3,2  
print (b)
print ('a 的形状也修改了：')
print (a)

'''
我们的数组是：
[0 1 2 3 4 5]
调用 id() 函数：
4349302224
a 赋值给 b：
[0 1 2 3 4 5]
b 拥有相同 id()：
4349302224
修改 b 的形状：
[[0 1]
 [2 3]
 [4 5]]
a 的形状也修改了：
[[0 1]
 [2 3]
 [4 5]]
'''
```



## copy

```python
import numpy as np

arr = np.array([1, 2, 3, 4, 5])
x = arr.copy()
arr[0] = 42

print(arr)	#[42  2  3  4  5]
print(x)	#[1 2 3 4 5]
```



## view

```python
import numpy as np

arr = np.array([1, 2, 3, 4, 5])
x = arr.view()
arr[0] = 42

print(arr)	#[42  2  3  4  5]
print(x)	#[42  2  3  4  5]
```



```python
import numpy as np 
 
arr = np.arange(12)
print ('我们的数组：')
print (arr)
print ('创建切片：')
a=arr[3:]
b=arr[3:]
a[1]=123
b[2]=234
print(arr)
print(id(a),id(b),id(arr[3:]))
'''
我们的数组：
[ 0  1  2  3  4  5  6  7  8  9 10 11]
创建切片：
[  0   1   2   3 123 234   6   7   8   9  10  11]
4545878416 4545878496 4545878576
'''
```

变量 a,b 都是 arr 的一部分视图，对视图的修改会直接反映到原数据中。

但是我们观察 a,b 的 id，他们是不同的，也就是说，视图虽然指向原数据，但是他们和赋值引用还是有区别的。



## 检查|是否是原始数据⭐

```python
import numpy as np

arr = np.array([1, 2, 3, 4, 5])

x = arr.copy()
y = arr.view()

print(x.base)	#None
print(y.base)	#[1 2 3 4 5]
```



# 小结|Python的list拷贝 vs. numpy的array拷贝

![img](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210514145240.png)

