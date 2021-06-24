# 导入|应用背景

> 拓展：[在注意力机制中使用einsum() 2020.08.12](https://www.youtube.com/watch?v=7wMQgveLiQ4)

```python
import numpy as np
def rnd(*args):
	return np.random.random(args)
# 测试自定义函数
np.matmul(rnd(4, 5), rnd(5, 6)).shape	#(4, 6)

#==# 一维向量与矩阵
E = 32
query_embedding = rnd(E)
keys_embeddings = rnd(10, E)

v1 = np.matmul(query_embedding, keys_embeddings.T)
v2 = np.matmul(keys_embeddings, query_embedding)

v1.shape, v2.shape, np.all(np.isclose(v1, v2))	#((10,), (10,), True)


#==# 矩阵
N = 100
E = 32

query_embeddings = rnd(N, E)
keys_embeddings = rnd(N, 10, E)

query_embeddings.reshape(N, 1, E).shape	#(100, 1, 32)
keys_embeddings.transpose(0, 2, 1).shape	#(100, 32, 10)
np.matmul(query_embeddings.reshape(N, 1, E),
          keys_embeddings.transpose(0, 2, 1)).shape	#(100, 1, 10): 从内部的矩阵(后面两维)开始计算矩阵乘法
np.squeeze(np.matmul(query_embeddings.reshape(N, 1, E),
          keys_embeddings.transpose(0, 2, 1))).shape	#(100, 10): 消除维度为1的axis
```

应用场景：

- 数据batch

```python
# np.einsum()
A, B = rnd(4, 5), rnd(5, 6)

m = np.matmul(A, B)
e = np.einsum('ij,jk->ik', A, B)

m.shape, e.shape, np.all(np.isclose(m, e))	#((4, 6), (4, 6), True)

# 例子2
A, B = rnd(5, 32), rnd(10, 32)

m = np.matmul(A, B.T)	#需要额外的转置
e = np.einsum('ij,kj->ik', A, B)

m.shape, e.shape, np.all(np.isclose(m, e))	#((5, 10), (5, 10), True)
```



实用场景：

```python
E = 32

query_embedding = rnd(E)
keys_embeddings = rnd(10, E)

m = np.matmul(query_embedding, keys_embeddings.T)	
e = np.einsum('e,ke->k', query_embedding, keys_embeddings)

m.shape, e.shape, np.all(np.isclose(m, e))	#((10,), (10,), True)
```

![image-20210517113122829](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210517113123.png)





# 导入|示例

## 1 矩阵相乘

![image-20210517093818710](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210517093818.png)

$M_{ij} = \sum_k A_{ik} B_{kj} \rightarrow A_{ik} B_{kj}$(最后一个表达式基于爱因斯坦求和标记)

```python
#==# 遍历
A = np.random.rand(3, 5)
B = np.random.rand(5, 2)
M = np.empty((3, 2))

for i in range(3):
	for j in range(2):
        total = 0
        for k in range(5):
            total += A[i, k] * B[k, j]
            
        M[i, j] = total
        

#==# 相应的einsum
M = np.einsum('ik,kj->ij', A, B)
```

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210517094314.png" alt="image-20210517094313423" style="zoom:67%;" />



## 2 向量外积

![image-20210517094431383](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210517094431.png)



# 规则

1 哑指标相加求和 => 消除

- “消除”的字母：表示该axis会相加求和

```python
#==# 矩阵相乘
M = np.einsum('ik,kj->ij', A, B)

#==# 一维向量自身sum
x = np.ones(3)
sum_x = np.einsum('i->', x)
```



2 自由指标的axes可以是任何顺序

```python
x = np.ones((5, 4, 3))
np.einsum('ijk->kji', x)	#变换轴 np.swapaxes()
```





# 代码演示

## 一维向量

![image-20210517105625441](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210517105625.png)

```python
#==# 第二个向量遍历求和
a = np.array([0, 1, 2, 3])
b = np.array([4, 5, 6, 7])
np.einsum('i, j->i', a, b) #[0, 22, 44, 66] => b中求和再乘以a中的每一个元素
'''伪代码
initialize output
for each i:
	for each j:
		output(i) += A(i) * B(j)
'''


#==# 两个向量遍历求和
a = np.array([0, 1, 2, 3])
b = np.array([4, 5, 6, 7])
np.einsum('i, j->', a, b) #132
'''伪代码
initialize output
for each i:
	for each j:
		output += A(i) * B(j)
'''


#==# 两个向量遍历求和
a = np.array([0, 1, 2, 3])
b = np.array([4, 5, 6, 7])
np.einsum('z,z->z', a, b) #[0, 5, 12, 21]
'''伪代码
initialize output
for each z:
	output(z) += A(z) * B(z)
'''


#==# 外积
a = np.array([0, 1, 2, 3])
b = np.array([4, 5, 6, 7])
np.einsum("s,t->st", a, b)
'''output
[[0 0 0 0]
 [4 5 6 7]
 [8 10 12 14]
 [12 15 18 21]]

伪代码
initial output
for each s:
	for each t:
		output(s, t) += a(s) * b(t)
'''
```



## 矩阵

注意：`C*D.T`计算过程中消耗内存

![image-20210517105849294](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210517105849.png)

```python
c = np.array([[0, 1], [2, 3]])
d = np.array([[4, 5], [6, 7]])
np.einsum('ij,ji->', c, d) #37
'''伪代码
initial output
for each i:
	for each j:
		output += c(i, j) * d(j, i)
'''
```



# 小结|[回顾Wiki](https://zh.wikipedia.org/wiki/%E7%88%B1%E5%9B%A0%E6%96%AF%E5%9D%A6%E6%B1%82%E5%92%8C%E7%BA%A6%E5%AE%9A)

> 1916 Einstein notation爱因斯坦标记法/Einstein summation convention爱因斯坦求和约定
>
> 在数学里，特别是将线性代数套用到物理时，爱因斯坦求和约定（Einstein summation convention）是一种标记的约定，又称为爱因斯坦标记法（Einstein notation），在处理**关于坐标的方程式**时非常有用。

由于只有总和不变，而总和所涉及的每一个项目都有可能会改变，所以，爱因斯坦提出了这标记法，重复标号表示总和，不需要用到求和符号：

采用爱因斯坦标记法，余向量都是以下标来标记，而向量都是以上标来标记。$y=\sum_{i=1}^{n} c_i x^i \rightarrow c_i x^i$

- me|看wiki的介绍：余向量$c_i$是系数



逆变、协变用来表示张量

## 应用|一般运算

> 矩阵A的第m行，第n列，标记$A_{mn}$改为$A_n^m$

### 1 内积

给予向量![{\mathbf  {a}}\,\!](https://wikimedia.org/api/rest_v1/media/math/render/svg/833955ef4a84e17636125df425fb40a2f19ba112)和余向量![{\boldsymbol  {\alpha }}\,\!](https://wikimedia.org/api/rest_v1/media/math/render/svg/ef9687123c051c50cce92c09b9d7f28676168cd6)，其向量和余向量的内积为标量：$a \cdot \alpha = a^{i} \alpha_i$



### 2 向量乘以矩阵

矩阵A和向量a，乘积是向量b：$b^{i} = A_{j}^{i} a^{j}$



### 3 矩阵乘法

$C_{k}^{i} = A_{j}^{i} B_{k}^{j}$



### 4 迹

$t = A_{i}^{i}$



### 5 外积

M维向量![{\mathbf  {a}}\,\!](https://wikimedia.org/api/rest_v1/media/math/render/svg/833955ef4a84e17636125df425fb40a2f19ba112)和N维余向量![{\boldsymbol  {\alpha }}\,\!](https://wikimedia.org/api/rest_v1/media/math/render/svg/ef9687123c051c50cce92c09b9d7f28676168cd6)的[外积](https://zh.wikipedia.org/wiki/外積)是一个M×N矩阵$A=a \alpha \rightarrow A_{j}^{i} = a^{i} \alpha_{j}$ 



## 应用|向量的运算

### 1 向量的内积

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210517131921.png" alt="image-20210517131921019" style="zoom:67%;" />

### 2 向量的叉积

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210517131905.png" alt="image-20210517131905259" style="zoom:67%;" />





# #参考文献

0 Youtube视频

- [Einsum Is All You Need: NumPy, PyTorch and TensorFlow 2020.07.18](https://www.youtube.com/watch?v=pkVwUVEHmfI)
- [示例：Python NumPy For Your Grandma 2021.01.20](https://www.youtube.com/watch?v=ULY6pncbRY8)