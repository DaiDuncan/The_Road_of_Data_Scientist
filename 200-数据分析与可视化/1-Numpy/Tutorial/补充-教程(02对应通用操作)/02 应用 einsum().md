# [官方文档](https://numpy.org/doc/stable/reference/generated/numpy.einsum.html#numpy.einsum)

> - Trace of an array, [`numpy.trace`](https://numpy.org/doc/stable/reference/generated/numpy.trace.html#numpy.trace).
> - Return a diagonal, [`numpy.diag`](https://numpy.org/doc/stable/reference/generated/numpy.diag.html#numpy.diag).
> - Array axis summations, [`numpy.sum`](https://numpy.org/doc/stable/reference/generated/numpy.sum.html#numpy.sum).
> - Transpositions and permutations, [`numpy.transpose`](https://numpy.org/doc/stable/reference/generated/numpy.transpose.html#numpy.transpose).
> - Matrix multiplication and dot product, [`numpy.matmul`](https://numpy.org/doc/stable/reference/generated/numpy.matmul.html#numpy.matmul) [`numpy.dot`](https://numpy.org/doc/stable/reference/generated/numpy.dot.html#numpy.dot).
> - Vector inner and outer products, [`numpy.inner`](https://numpy.org/doc/stable/reference/generated/numpy.inner.html#numpy.inner) [`numpy.outer`](https://numpy.org/doc/stable/reference/generated/numpy.outer.html#numpy.outer).
> - Broadcasting, element-wise and scalar multiplication, [`numpy.multiply`](https://numpy.org/doc/stable/reference/generated/numpy.multiply.html#numpy.multiply).
> - Tensor contractions, [`numpy.tensordot`](https://numpy.org/doc/stable/reference/generated/numpy.tensordot.html#numpy.tensordot).
> - Chained array operations, in efficient calculation order, [`numpy.einsum_path`](https://numpy.org/doc/stable/reference/generated/numpy.einsum_path.html#numpy.einsum_path).

```python
numpy.einsum(subscripts, *operands, out=None, dtype=None, order='K', casting='safe', optimize=False)
```



# 综述|[einsum is all you need 2018.04.30](https://rockt.github.io/2018/04/30/einsum)

> 推荐拓展：
>
> - [Einstein Summation in Numpy 2016.02.04](https://obilaniu6266h16.wordpress.com/2016/02/04/einstein-summation-in-numpy/)
> - [A basic introduction to NumPy's einsum 2015.05.02](https://ajcr.net/Basic-guide-to-einsum/)

## 背景|维度的现实含义

数据$\mathcal{T} = \mathbb{R}^{N \times T \times K}$

- 一个batch中有N个训练样本
- T个长序列
- 每个序列中有K维词向量

变换矩阵$W \in \mathbb{R}^{K \times Q}$

![image-20210517133907715](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210517133907.png)



变化：当数据$\mathcal{T}$是4阶张量时 => 涉及到运算时维度的对应

![image-20210517134020142](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210517134020.png)

## 代码示例|基于pytorch

```python
np.einsum()
torch.einsum()
tf.einsum()
```

### 向量、矩阵运算

1 矩阵转置
$$
B_{ji} = A_{ij}
$$

```python
import torch
a = torch.arange(6).reshape(2, 3)
torch.einsum('ij->ji', [a])	#(3, 2)
```



2 求和
$$
b = \sum_i \sum_j A_{ij} = A_{ij}
$$

```python
a = torch.arange(6).reshape(2, 3)
torch.einsum('ij->', [a])	#(1,)
```



3 列求和
$$
b_j = \sum_i A_{ij} = A_{ij}
$$

```python
a = torch.arange(6).reshape(2, 3)
torch.einsum('ij->j', [a])	#(1, 3)
```



4 行求和
$$
b_i = \sum_j A_{ij} = A_{ij}
$$

```python
a = torch.arange(6).reshape(2, 3)
torch.einsum('ij->i', [a])	#(1, 2)
```



5 矩阵-向量
$$
c_i = \sum_k A_{ik} b_k = A_{ik} b_k
$$

```python
a = torch.arange(6).reshape(2, 3)
b = torch.arange(3)
torch.einsum('ik,k->i', [a, b])	#(1,2)
```



6 矩阵-矩阵
$$
C_{ij} = \sum_k A_{ik} B_{kj} = A_{ik} B_{kj}
$$

```python
a = torch.arange(6).reshape(2, 3)
b = torch.arange(15).reshape(3, 5)
torch.einsum('ik,kj->ij', [a, b])	#(2, 5)
```



7 点积/内积

向量：
$$
c = \sum_i a_i b_i = a_i b_i
$$

```python
a = torch.arange(3)
b = torch.arange(3,6)  # -- a vector of length 3 containing [3, 4, 5]
torch.einsum('i,i->', [a, b])	#(1,)
```

矩阵：
$$
c = \sum_i \sum_j A_{ij} B_{ij} = A_{ij} B_{ij}
$$

```python
a = torch.arange(6).reshape(2, 3)
b = torch.arange(6,12).reshape(2, 3)
torch.einsum('ij,ij->', [a, b])	#(1,)
```



8 Hadamard乘积
$$
C_{ij} = A_{ij} B_{ij}
$$

```python
a = torch.arange(6).reshape(2, 3)
b = torch.arange(6,12).reshape(2, 3)
torch.einsum('ij,ij->ij', [a, b])	#(2, 3)
```



9 外积
$$
C_{ij} = a_i b_j
$$

```python
a = torch.arange(3)
b = torch.arange(3,7)  # -- a vector of length 4 containing [3, 4, 5, 6]
torch.einsum('i,j->ij', [a, b])	#(3, 4)
```



10 Batch矩阵乘积
$$
C_{ijl} = \sum_k A_{ijk} B_{ikl} = A_{ijk} B_{ikl}
$$

```python
a = torch.randn(3,2,5)
b = torch.randn(3,5,3)
torch.einsum('ijk,ikl->ijl', [a, b])	#(3, 2, 3)
```



11 Tensor contraction|维度(q, s) => (q-1, s-1)

Batch矩阵乘积是Tensor contraction的一种特殊情况

维度变化：

- $\mathcal{A} \in \mathbb{R}^{I_1 \times ...  \times I_n}$ => n阶张量
- $\mathcal{B} \in \mathbb{R}^{J_1 \times ...  \times J_m}$ => m阶张量
- 假设n=4, m=5。同时：$I_2 = J_3, I_3 = J_5$
- 结果：$\mathcal{C} \in \mathbb{R}^{I_1 \times I_4 \times J_1 \times J_2  \times J_4}$

![image-20210517144707553](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210517144707.png)

```python
a = torch.randn(2,3,5,7)
b = torch.randn(11,13,3,17,5)
torch.einsum('pqrs,tuqvr->pstuv', [a, b]).shape	#torch.Size([2, 7, 11, 13, 17])
```



12 双线性映射

einsum可以作用于超过两个以上的张量：

![image-20210517145159939](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210517145200.png)

```python
a = torch.randn(2,3)
b = torch.randn(5,3,7)
c = torch.randn(2,7)
torch.einsum('ik,jkl,il->ij', [a, b, c])	#(2, 5)
```





### 实际案例

1 神经网络|更新状态

实际中的参数维度：

- 数据batch $Z \in \mathbb{R}^{B \times K}$
- 变换 $W \in \mathbb{R}^{A \times K \times K}$

![image-20210517145314497](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210517145314.png)

```python
import torch.nn.functional as F

def random_tensors(shape, num=1, requires_grad=False):
  tensors = [torch.randn(shape, requires_grad=requires_grad) for i in range(0, num)]
  return tensors[0] if num == 1 else tensors

# Parameters
# -- [num_actions x hidden_dimension]
b = random_tensors([5, 3], requires_grad=True)
# -- [num_actions x hidden_dimension x hidden_dimension]
W = random_tensors([5, 3, 3], requires_grad=True)

def transition(zl):
  # -- [batch_size x num_actions x hidden_dimension]
  # zl.unsqueeze(1) => (2, 1, 3)
  # 1. einsum()结果是(2, 5, 3) 注意：下标调换了顺序
  # 2. +b(5, 3) 广播机制 => (2, 5, 3)
  # 3. +(2, 1, 3) 广播机制 => (2, 5, 3)
  return zl.unsqueeze(1) + F.tanh(torch.einsum("bk,aki->bai", [zl, W]) + b)

# Sampled dummy inputs
# -- [batch_size x hidden_dimension]
zl = random_tensors([2, 3])	#(2, 3)

transition(zl)	#结果是(2, 5, 3)
```



[torch.unsqueeze()](https://pytorch.org/docs/stable/generated/torch.unsqueeze.html)

```python
torch.unsqueeze(input, dim) → Tensor
'''
input (Tensor) – the input tensor.
dim (int) – the index at which to insert the singleton dimension
'''
>>> x = torch.tensor([1, 2, 3, 4])	#(4,)
>>> torch.unsqueeze(x, 0)			#(1, 4)
tensor([[ 1,  2,  3,  4]])	
>>> torch.unsqueeze(x, 1)			#(4, 1)
tensor([[ 1],
        [ 2],
        [ 3],
        [ 4]])
```



2 注意力机制：word-by-word attention mechanism

![image-20210517155812280](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210517155812.png)

```python
# Parameters
# -- [hidden_dimension]
bM, br, w = random_tensors([7], num=3, requires_grad=True)
# -- [hidden_dimension x hidden_dimension]
WY, Wh, Wr, Wt = random_tensors([7, 7], num=4, requires_grad=True)

# Single application of attention mechanism 
def attention(Y, ht, rt1):
  # -- [batch_size x hidden_dimension] 
  tmp = torch.einsum("ik,kl->il", [ht, Wh]) + torch.einsum("ik,kl->il", [rt1, Wr])
  Mt = F.tanh(torch.einsum("ijk,kl->ijl", [Y, WY]) + tmp.unsqueeze(1).expand_as(Y) + bM)
  # -- [batch_size x sequence_length]
  at = F.softmax(torch.einsum("ijk,k->ij", [Mt, w])) 
  # -- [batch_size x hidden_dimension]
  rt = torch.einsum("ijk,ij->ik", [Y, at]) + F.tanh(torch.einsum("ij,jk->ik", [rt1, Wt]) + br)
  # -- [batch_size x hidden_dimension], [batch_size x sequence_dimension]
  return rt, at

# Sampled dummy inputs
# -- [batch_size x sequence_length x hidden_dimension]
Y = random_tensors([3, 5, 7])
# -- [batch_size x hidden_dimension]
ht, rt1 = random_tensors([3, 7], num=2)

rt, at = attention(Y, ht, rt1)
at  # -- print attention weights
```



## 小结

张量运算的“瑞士军刀”，但仍需要自定义构造维度

- 比如`unsqueeze` => 广播机制

也需要使用split, concatenate, index等函数



注意：pytorch中的einsum目前不支持对角线元素(比如提取对角线元素，求矩阵的迹)，下列代码错误

```python
torch.einsum('ii->i', [torch.randn(3, 3)])
```







## 拓展|汇总：目前支持矩阵的迹

```python
import torch	#numpy, tensorflow均可

x = torch.rand((2, 3))

# Permutation of Tensors
torch.einsum("ij->ji", x)

# Summation
torch.einsum("ij->", x)

# Column Sum
torch.einsum("ij->j", x)

# Row Sum
torch.einsum("ij->i", x)

# Matrix-Vector Multiplication
v = torch.rand((1, 3))
torch.einsum("ij,kj->ik", x, v)	#(2,3)(1,3)->(2,1)不用考虑reshape => 直接了然

# Matrix-Matrix Multiplication
torch.einsum("ij,kj->ik", x, x)	#(2,3)(2,3)->(2,2)

# Dot product first row with first row of matrix
torch.einsum("i,i->", x[0], x[0])

# Dot product with matrix
torch.einsum("ij,ij->", x, x)	#(2,3)(2,3)->(1,)

# Hadamard product(element-wise multiplication)
torch.einsum("ij,ij->ij", x, x)

# Outer product
a = torch.rand((3))
b = torch.rand((5))
torch.einsum("i,j->ij", a, b)

### Batch Matrix Multiplication ### me: 基于numpy的数组表示 => 消除内数组
a = torch.rand((3, 2, 5))
b = torch.rand((3, 5, 3))
torch.einsum("ijk,ikl->ijl", a, b)

# Matrix Diagonal: 提取对角线元素
x = torch.rand((3, 3))
torch.einsum("ii->i", x)

# Matrix Trace
torch.einsum("ii->", x)
```



# [Einstein Summation in Numpy 2016.02.04](https://obilaniu6266h16.wordpress.com/2016/02/04/einstein-summation-in-numpy/)

> [源代码](https://pastebin.com/9GPvxwUn)

不必考虑：

- 张量的正确顺序：转置，维度的先后位置

只需要考虑：

- 哪些维度需要运算：inner/element-wise/outer
- 输出的维度



字符串表示与维度的关系：

![image-20210517163935430](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210517163935.png)





for-loop细节：

- 外部的for循环 => 自由指标 => 出现在等式左侧的结果中
- 内部的for循环 => 哑指标 => 求和

![Einsum-matrixMul](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210517164016.png)



自由指标：

![Einsum-freeIndices](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210517164211.png)



哑指标：

![Einsum-summationIndices](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210517164220.png)



## 代码示例|基于numpy

### 向量、矩阵运算

1 矩阵对角元素

这只有在矩阵是方形的情况下才有效

![Einsum-matrixDiag](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210517164352.png)



2 矩阵的迹

![einsum-matrixtr](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210517164437.png)



3 三个矩阵

![Einsum-quadForm](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210517164529.png)



4 Batched outer product

![Einsum-batchOuter](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210517164602.png)





### 实际案例

1 MLP⭐

> me: 想起我最开始实现神经网络的前馈、反馈时，被维度搞昏了头

![Einsum-mlpStochastic](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210517165140.png)



2 batched

![Einsum-mlpBatch](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210517165212.png)





## 警惕|错误示例

- 格式字符串没有按照规定
  - ASCII字符
  - 多出来的字符
- 输出：
  - 重复index
- 使用过程：错误匹配

![einsum-fmtstringrules](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210517164751.png)

- The number of argument specifications must match the number of arguments, or else you are indexing into air. 参数的数量必须与参数的数量相匹配，否则你就是在对空气进行索引
- The argument specification string must be as long as the argument’s dimensionality, or else the `total` being summed into won’t be a scalar [2]. 参数字符串必须和参数的维度一样长，否则被求和的总数就不是标量
- Using an output index that doesn’t exist amongst the input indices would be like using an undeclared variable. 使用一个不存在于输入索引中的输出索引，就像使用一个未声明的变量
- Reusing an index in the output specification would leave uninitialized elements outside a diagonal. 在输出规范中重复使用一个索引会在对角线外**留下未初始化的元素**
- Using the same index for axes of different lengths doesn’t make sense – which is the right length to use? 对不同长度的轴使用相同的索引是没有意义的--哪种长度才是正确的?



## 小结

注意：

1. 在einsum()中涉及广播机制@上述`神经网络|更新状态`的代码

### philosophy指导思想

- 给指标恰当的命名符号 Give each axis that notionally exists within the problem its own label. It is best if memorable ones can be chosen, like in the MLP problem above.
- 元素间的乘法 If you want element-wise multiplication between the axes of two arguments, use the same index for both (`"a,a->a"`).
- 某个轴的求和 If you want summation along a given axis, **don’t** put its index in the output specification (`"a->"`).
- 内积 If you want an inner product, which is element-wise multiplication between two axes followed by the summing-out of those axes, then *do both of the above* (`"a,a->"`).
- 字符串中可自定义转置 If a tensor is used as argument to `einsum()`, simply copy-paste its specification from the `einsum()` that created it. Input transpositions are automagically handled.
- For the output, simply state what is the form of the tensor that you want. The genie in `einsum()` will give it to you, and you have infinite wishes.



### 常用的字符串标记

| 字符串标记       | 作用                                                         |
| ---------------- | ------------------------------------------------------------ |
| `"a,a->" `       | 向量内积(假设两个向量维度一样)                               |
| `"a,a->a"`       | 向量元素间的乘积(假设两个向量维度一样)                       |
| `"a,b->ab"`      | 向量外积                                                     |
| `"ab->ba"`       | 矩阵转置                                                     |
| `"ii->i"`        | 矩阵对角线元素                                               |
| `"ii->"`         | 矩阵的迹                                                     |
| `"a->"`          | 1D求和                                                       |
| `"ab->"`         | 2D求和                                                       |
| `"abc->"`        | 3D求和                                                       |
| `"ab,ab->"`      | 矩阵的内积(If you pass twice the same argument, it becomes a matrix L2 norm) |
| `"ab,b->a"`      | 矩阵乘以向量(矩阵左乘)                                       |
| `"a,ab->b"`      | 向量乘以矩阵(矩阵右乘)                                       |
| `"ab,bc->ac"`    | 矩阵乘积                                                     |
| `"Yab,Ybc->Yac"` | Batch矩阵乘积                                                |
| `"a,ab,b->"`     | Quadratic form / Mahalanobis Distance                        |

马哈拉诺比斯距离：

![image-20210517170507598](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210517170507.png)





# [einsum()简要提炼 2015.05.02](https://ajcr.net/Basic-guide-to-einsum/)

> 在2011年，numpy 1.6.0中加入einsum()函数

定位：代替multiply, sum, transpose; trace、tensordot等

- 矩阵求迹：trace
- 求矩阵对角线：diag
- 张量（沿轴）求和：sum⭐
- 张量转置：transopose⭐
- 矩阵乘法：dot⭐
- 张量乘法：tensordot
- 向量内积：inner
- 外积：outer

```python
A = np.array([0, 1, 2])

B = np.array([[ 0,  1,  2,  3],
              [ 4,  5,  6,  7],
              [ 8,  9, 10, 11]])
#==# 逐个元素相乘，然后沿着axis=1跨列 进行行求和
# 1 使A成为列向量(3, 1) => 触发广播机制:A中的每一个元素与B中的每一行相乘 => 得到结果(3, 4)
# 2 沿着axis=1跨列 进行行求和 => (3,)
>>> (A[:, np.newaxis] * B).sum(axis=1)
array([ 0, 22, 76])

#==# np.einsum()
>>> np.einsum('i,ij->i', A, B)
array([ 0, 22, 76])
```

优化细节：

1. A不需要reshape
2. 不需要创建临时变量来保存`A[:, np.newaxis] * B`的结果



## 3条规则

1 哑指标：两个axes的维度的长度相等

2 自由指标：沿着这个axis来求和

3 对于未求和的axes，我们可以自由调整顺序(暗含transpose)



## 汇总|表格

### 一维数组

| **Call signature**  | **NumPy equivalent** |            **Description**             |
| :-----------------: | :------------------: | :------------------------------------: |
|     `('i', A)`      |         `A`          |          returns a view of A           |
|    `('i->', A)`     |       `sum(A)`       |          sums the values of A          |
| `('i,i->i', A, B)`  |       `A * B`        | element-wise multiplication of A and B |
|   `('i,i', A, B)`   |    `inner(A, B)`     |        inner product of A and B        |
| `('i,j->ij', A, B)` |    `outer(A, B)`     |        outer product of A and B        |



### 二维数组

| **Call signature**      | **NumPy equivalent**     | **Description**                          |
| ----------------------- | ------------------------ | ---------------------------------------- |
| `('ij', A)`             | `A`                      | returns a view of A                      |
| `('ji', A)`             | `A.T`                    | view transpose of A                      |
| `('ii->i', A)`          | `diag(A)`                | view main diagonal of A                  |
| `('ii', A)`             | `trace(A)`               | sums main diagonal of A                  |
| `('ij->', A)`           | `sum(A)`                 | sums the values of A                     |
| `('ij->j', A)`          | `sum(A, axis=0)`         | sum down the columns of A (across rows)  |
| `('ij->i', A)`          | `sum(A, axis=1)`         | sum horizontally along the rows of A     |
| `('ij,ij->ij', A, B)`   | `A * B`⭐                 | element-wise multiplication of A and B   |
| `('ij,ji->ij', A, B)`   | `A * B.T`⭐               | element-wise multiplication of A and B.T |
| `('ij,jk', A, B)`       | `dot(A, B)`              | matrix multiplication of A and B         |
| `('ij,kj->ik', A, B)`   | `inner(A, B)`            | inner product of A and B                 |
| `('ij,kj->ikj', A, B)`  | `A[:, None] * B`⭐        | each row of A multiplied by B            |
| `('ij,kl->ijkl', A, B)` | `A[:, :, None, None]*B`⭐ | each value of A multiplied by B          |



注意：当数组的维度很大时，允许使用`...`来表示省略

- `np.einsum('...ij,ji->...', a, b)` a最后的两个axes与2D数组b相乘



### 张量与矩阵相乘

又被称为模态积，modal product

若给定张量![[公式]](https://www.zhihu.com/equation?tex=%7B%5Cmathcal%7B+X%7D%7D)为![[公式]](https://www.zhihu.com/equation?tex=%7B%5Cmathcal%7B+X%7D%7D%5Cleft%28+%3A%2C%3A%2C1+%5Cright%29+%3D%5Cleft%5B+%5Cbegin%7Barray%7D%7Bcc%7D+1+%26+2+%5C%5C+3+%26+4+%5C%5C+%5Cend%7Barray%7D+%5Cright%5D)，![[公式]](https://www.zhihu.com/equation?tex=%7B%5Cmathcal%7B+X%7D%7D%5Cleft%28+%3A%2C%3A%2C2+%5Cright%29+%3D%5Cleft%5B+%5Cbegin%7Barray%7D%7Bcc%7D+5+%26+6+%5C%5C+7+%26+8+%5C%5C+%5Cend%7Barray%7D+%5Cright%5D)，其大小为![[公式]](https://www.zhihu.com/equation?tex=2%5Ctimes+2%5Ctimes+2)，另外给定矩阵![[公式]](https://www.zhihu.com/equation?tex=A%3D%5Cleft%5B+%5Cbegin%7Barray%7D%7Bcc%7D+1+%26+2+%5C%5C+3+%26+4+%5C%5C+5+%26+6+%5C%5C+%5Cend%7Barray%7D+%5Cright%5D)，试想一下：张量![[公式]](https://www.zhihu.com/equation?tex=%7B%5Cmathcal%7B+X%7D%7D)和矩阵![[公式]](https://www.zhihu.com/equation?tex=A)相乘会得到什么呢？

```python
import numpy as np
# X: (2, 2, 2)
X = np.array([[[1, 2], [3, 4]], 
              [[5, 6], [7, 8]]])
# A: (3, 2)
A = np.array([[1, 2], [3, 4], [5, 6]])
```

假设![[公式]](https://www.zhihu.com/equation?tex=%7B%5Cmathcal%7B+Y%7D%7D%3D%7B%5Cmathcal%7B+X%7D%7D%5Ctimes+_1A)，则对于张量![[公式]](https://www.zhihu.com/equation?tex=%7B%5Cmathcal%7B+Y%7D%7D)在任意索引![[公式]](https://www.zhihu.com/equation?tex=%5Cleft%28+i%2Cj%2Ck+%5Cright%29+)上的值为![[公式]](https://www.zhihu.com/equation?tex=y_%7Bijk%7D%3D%5Csum_%7Bm%3D1%7D%5E%7B2%7D%7B%5Cleft%28+x_%7Bmjk%7D%5Ccdot+a_%7Bim%7D+%5Cright%29+%7D+)，这一运算规则也不难发现，张量![[公式]](https://www.zhihu.com/equation?tex=%7B%5Cmathcal%7B+Y%7D%7D)的大小为![[公式]](https://www.zhihu.com/equation?tex=3%5Ctimes+2%5Ctimes+2)，以![[公式]](https://www.zhihu.com/equation?tex=%5Cleft%28+1%2C1%2C1+%5Cright%29+)位置为例，![[公式]](https://www.zhihu.com/equation?tex=y_%7B111%7D%3D%5Csum_%7Bm%3D1%7D%5E%7B2%7D%7B%5Cleft%28+x_%7Bm11%7D%5Ccdot+a_%7B1m%7D+%5Cright%29+%7D+%3Dx_%7B111%7D%5Ccdot+a_%7B11%7D%2Bx_%7B211%7D%5Ccdot+a_%7B12%7D%3D7)

再以![[公式]](https://www.zhihu.com/equation?tex=%5Cleft%28+1%2C1%2C2+%5Cright%29+)位置为例，![[公式]](https://www.zhihu.com/equation?tex=y_%7B112%7D%3D%5Csum_%7Bm%3D1%7D%5E%7B2%7D%7B%5Cleft%28+x_%7Bm12%7D%5Ccdot+a_%7B1m%7D+%5Cright%29+%7D)![[公式]](https://www.zhihu.com/equation?tex=+%3Dx_%7B112%7D%5Ccdot+a_%7B11%7D%2Bx_%7B212%7D%5Ccdot+a_%7B12%7D%3D19)，这样，可以得到张量![[公式]](https://www.zhihu.com/equation?tex=%7B%5Cmathcal%7B+Y%7D%7D)为

![[公式]](https://www.zhihu.com/equation?tex=%7B%5Cmathcal%7B+Y%7D%7D%5Cleft%28+%3A%2C%3A%2C1+%5Cright%29+%3D%5Cleft%5B+%5Cbegin%7Barray%7D%7Bcc%7D+x_%7B111%7D+a_%7B11%7D%2Bx_%7B211%7D+a_%7B12%7D+%26+x_%7B121%7D+a_%7B11%7D%2Bx_%7B221%7D+a_%7B12%7D+%5C%5C+x_%7B111%7D+a_%7B21%7D%2Bx_%7B211%7D+a_%7B22%7D+%26+x_%7B121%7D+a_%7B21%7D%2Bx_%7B221%7D+a_%7B22%7D+%5C%5C+x_%7B111%7D+a_%7B31%7D%2Bx_%7B211%7D+a_%7B32%7D+%26+x_%7B121%7D+a_%7B31%7D%2Bx_%7B221%7D+a_%7B32%7D+%5C%5C+%5Cend%7Barray%7D+%5Cright%5D)，

![[公式]](https://www.zhihu.com/equation?tex=%7B%5Cmathcal%7B+Y%7D%7D%5Cleft%28+%3A%2C%3A%2C2+%5Cright%29+%3D%5Cleft%5B+%5Cbegin%7Barray%7D%7Bcc%7D+x_%7B112%7D+a_%7B11%7D%2Bx_%7B212%7D+a_%7B12%7D+%26+x_%7B122%7D+a_%7B11%7D%2Bx_%7B222%7D+a_%7B12%7D+%5C%5C+x_%7B112%7D+a_%7B21%7D%2Bx_%7B212%7D+a_%7B22%7D+%26+x_%7B122%7D+a_%7B21%7D%2Bx_%7B222%7D+a_%7B22%7D+%5C%5C+x_%7B112%7D+a_%7B31%7D%2Bx_%7B212%7D+a_%7B32%7D+%26+x_%7B122%7D+a_%7B31%7D%2Bx_%7B222%7D+a_%7B32%7D+%5C%5C+%5Cend%7Barray%7D+%5Cright%5D)，

即![[公式]](https://www.zhihu.com/equation?tex=%7B%5Cmathcal%7B+Y%7D%7D%5Cleft%28+%3A%2C%3A%2C1+%5Cright%29+%3D%5Cleft%5B+%5Cbegin%7Barray%7D%7Bcc%7D+1%5Ctimes+1%2B3%5Ctimes+2+%26+2%5Ctimes+1%2B4%5Ctimes+2+%5C%5C+1%5Ctimes+3%2B3%5Ctimes+4+%26+2%5Ctimes+3%2B4%5Ctimes+4+%5C%5C+1%5Ctimes+5%2B3%5Ctimes+6+%26+2%5Ctimes+5%2B4%5Ctimes+6+%5C%5C+%5Cend%7Barray%7D+%5Cright%5D%3D%5Cleft%5B+%5Cbegin%7Barray%7D%7Bcc%7D+7+%26+10+%5C%5C+15+%26+22+%5C%5C+23+%26+34+%5C%5C+%5Cend%7Barray%7D+%5Cright%5D)，![[公式]](https://www.zhihu.com/equation?tex=%7B%5Cmathcal%7B+Y%7D%7D%5Cleft%28+%3A%2C%3A%2C2+%5Cright%29+%3D%5Cleft%5B+%5Cbegin%7Barray%7D%7Bcc%7D+5%5Ctimes+1%2B7%5Ctimes+2+%26+6%5Ctimes+1%2B8%5Ctimes+2+%5C%5C+5%5Ctimes+3%2B7%5Ctimes+4+%26+6%5Ctimes+3%2B8%5Ctimes+4+%5C%5C+5%5Ctimes+5%2B7%5Ctimes+6+%26+6%5Ctimes+5%2B8%5Ctimes+6+%5C%5C+%5Cend%7Barray%7D+%5Cright%5D%3D%5Cleft%5B+%5Cbegin%7Barray%7D%7Bcc%7D+19+%26+22+%5C%5C+43+%26+50+%5C%5C+67+%26+78+%5C%5C+%5Cend%7Barray%7D+%5Cright%5D).



- **np.einsum('kij, li->klj', X, A)**返回 ![[公式]](https://www.zhihu.com/equation?tex=%7B%5Cmathcal%7B+Y%7D%7D%3D%7B%5Cmathcal%7B+X%7D%7D%5Ctimes+_1A%5Cin%5Cmathbb%7BR%7D%5E%7B3%5Ctimes+2%5Ctimes+2%7D) ，即

```python
np.einsum('kij, li->klj', X, A) # mode-1 product
Out[17]: 
array([[[ 7, 10],
        [15, 22],
        [23, 34]],

       [[19, 22],
        [43, 50],
        [67, 78]]])
```

- **np.einsum('kij, lj->kil', X, A)**返回 ![[公式]](https://www.zhihu.com/equation?tex=%7B%5Cmathcal%7B+Y%7D%7D%3D%7B%5Cmathcal%7B+X%7D%7D%5Ctimes+_2A%5Cin%5Cmathbb%7BR%7D%5E%7B2%5Ctimes+3%5Ctimes+2%7D) ，即

```python
np.einsum('kij, lj->kil', X, A) # mode-2 product
Out[18]: 
array([[[ 5, 11, 17],
        [11, 25, 39]],

       [[17, 39, 61],
        [23, 53, 83]]])
```

- **np.einsum('kij, lk->lij', X, A)**返回 ![[公式]](https://www.zhihu.com/equation?tex=%7B%5Cmathcal%7B+Y%7D%7D%3D%7B%5Cmathcal%7B+X%7D%7D%5Ctimes+_3A%5Cin%5Cmathbb%7BR%7D%5E%7B2%5Ctimes+2%5Ctimes+3%7D) ，即

```python
np.einsum('kij, lk->lij', X, A) # mode-3 product
Out[19]: 
array([[[11, 14],
        [17, 20]],

       [[23, 30],
        [37, 44]],

       [[35, 46],
        [57, 68]]])
```



### 四阶张量与矩阵相乘

```python
import numpy as np
# X: (2, 2, 2, 2)
X = np.array([[[[1, 2], [3, 4]], [[5, 6], [7, 8]]], 
              [[[9, 10], [11, 12]], [[13, 14], [15, 16]]]])
# A: (3, 2)
A = np.array([[1, 2], [3, 4], [5, 6]])

Y1 = np.einsum('lkij, mi->lkmj', X, A) # mode-1 product

Y2 = np.einsum('lkij, mj->lkim', X, A) # mode-2 product

Y3 = np.einsum('lkij, mk->lmij', X, A) # mode-3 product

Y4 = np.einsum('lkij, ml->mkij', X, A) # mode-4 product
```

注意：由于Python中的数组形状（shape）会与张量定义的大小有所不同，为避免混淆，将索引标记的先后顺序定义为 ![[公式]](https://www.zhihu.com/equation?tex=i%5Crightarrow+j%5Crightarrow+k%5Crightarrow+l) .



## 警惕|注意情况

1 不会自动改变数据类型(可能会溢出)

```python
>>> a = np.ones(300, dtype=np.int8)
>>> np.sum(a) # correct result
300
>>> np.einsum('i->', a) # produces incorrect result
44
```



2 没有输出时：自动调整axes排列顺序

比如`np.einsum('kij', M)` => 按照字母顺序，自动变成ijk三个轴的顺序



3 效率|不一定最快

- np.dot()
- np.inner()
- np.tensordot()





# [效率测试2019.07.03](https://zhuanlan.zhihu.com/p/71639781)

以numpy做一下测试，对比einsum与各种函数的速度，这里使用python内建的timeit模块进行时间测试

1 先测试（四维）两张量相乘然后求所有元素之和，对应的公式为：

![[公式]](https://www.zhihu.com/equation?tex=%5Csum_i+%5Csum_j+%5Csum_k+%5Csum_l++a_%7Bijkl%7D+b_%7Bijkl%7D)

```python
from timeit import Timer
import numpy as np

# 定义两个全局变量
a = np.random.rand(64, 128, 128, 64)
b = np.random.rand(64, 128, 128, 64)

# 定义使用einsum与sum的函数
def einsum():
    temp = np.einsum('ijkl,ijkl->', a, b)
    
def npsum():
    temp = (a * b).sum()

# 打印运行时间
print("einsum cost:", Timer("einsum()", "from __main__ import einsum").timeit(20))
print("npsum cost:", Timer("npsum()", "from __main__ import npsum").timeit(20))

# 结果
einsum cost: 1.5560735
npsum cost: 8.0874927
```



```python
#==# 上面Timer是timeit模块内的一个类
Timer(stmt, setup).timeit(number)
    # stmt: 要测试的语句
    # setup: 传入stmt的运行环境，比如stmt中要导入的模块等。
    # 可以写一行语句，也可以写多行语句，写多行语句时要用分号；隔开语句
    # number: 执行次数
```



2 接下来测试单个张量求和：

![[公式]](https://www.zhihu.com/equation?tex=%5Csum_i+%5Csum_j+%5Csum_k+%5Csum_l+a_%7Bijkl%7D)

```python
def einsum():
    temp = np.einsum('ijkl->', a)
    
def npsum():
    temp = a.sum()
    
'''测试结果
einsum cost: 3.2716003
npsum cost: 6.7865246
'''
```

还是einsum更快，所以哪怕是单个张量求和，numpy上也可以用einsum替代

同样，求均值（mean）、方差（var）、标准差（std）也是一样



3 接下来测试einsum与dot函数，首先列一下矩阵乘法的公式以以及einsum表达式：

![[公式]](https://www.zhihu.com/equation?tex=c_%7Bik%7D+%3D+%5Csum_j+a_%7Bij%7D+b_%7Bjk%7D)

![[公式]](https://www.zhihu.com/equation?tex=c_%7Bik%7D+%3D+a_%7Bij%7D+b_%7Bjk%7D)

```python
a = np.random.rand(2024, 2024)
b = np.random.rand(2024, 2024)

# einsum与dot比较
def einsum():
    res = np.einsum('ik,kj->ij', a, b)

def dot():
    res = np.dot(a, b)

print("einsum cost:", Timer("einsum()", "from __main__ import einsum").timeit(20))
print("dot cost:", Timer("dot()", "from __main__ import dot").timeit(20))

# einsum cost: 80.2403851
# dot cost: 2.0842243
```

比dot慢了40倍（并且差距随着矩阵规模的平方增加），这还怎么打天下？不过在numpy的实现里，einsum是可以进行优化的，去掉不必要的中间结果，减少不必要的转置、变形等等，可以提升很大的性能，将einsum的实现改一下：

```python
def einsum():
    res = np.einsum('ik,kj->ij', a, b, optimize=True)
'''加了一个参数optimize=True，官方文档上该参数是可选参数，接受4个值：
optimize : {False, True, ‘greedy’, ‘optimal’}, optional
	如果设为True, 默认是'greedy
'''

einsum cost: 2.0330937
dot cost: 1.9866218
```

通过优化，虽然还是稍慢一些，但是einsum的速度与dot达到了一个量级；不过numpy官方手册上有个einsum_path，说是可以进一步提升速度

但是我在自己电脑上（i7-9750H）测试效果并不稳定，这里简单的介绍一下该函数的用法为：

```python
path = np.einsum_path('ik,kj->ij', a, b)[0]
np.einsum('ik,kj->ij', a, b, optimize=path)
```

einsum_path返回一个einsum可使用的优化路径列表，一般使用第一个优化路径；

另外，optimize及**einsum_path函数**只有numpy实现了，tensorflow和pytorch上至少现在没有



4 最后，再测试einsum与另一个常用的函数tensordot

首先定义两个四维张量的及tensordot函数：

```python
a = np.random.rand(128, 128, 64, 64)
b = np.random.rand(128, 128, 64, 64)

def tensordot():
    res = np.tensordot(a, b, ([0, 1], [0, 1]))
```

该实现对应的公式为：

![[公式]](https://www.zhihu.com/equation?tex=c_%7Bklmn%7D++%3D+%5Csum_i+%5Csum_j+a_%7Bijkl%7D+b_%7Bijmn%7D)

所以einsum函数的实现为：

```python
def einsum():
    res = np.einsum('ijkl,ijmn->klmn', a, b, optimize=True)
```

tensordot也是**链接到BLAS**实现的函数，所以不加optimize肯定比不了，最后结果为：

```python
print("einsum cost:", Timer("einsum()", "from __main__ import einsum").timeit(1))
print("tensordot cost:", Timer("tensordot()", "from __main__ import tensordot").timeit(1))

# einsum cost: 4.2361331
# tensordot cost: 4.2580409
```

测试了10多次，基本上速度一样，einsum表现好一点的；不过说是一个函数打天下，肯定是做不到的

还有一些数组的**分割、合并、指数、对数**等功能没法实现，需要使用别的函数，其他的基本都可以用einsum来实现，简单而又高效



## 补充

1 经过进一步测试发现，优化反而出现速度降低的情况，例如：

```python
def einsum():
    temp = einsum('...->', a, optimize=True)

def test():
    temp = a.sum()
```

上面两中对数组求和的方法，当a是一维向量时，或者a是多维但是规模很小是，**优化的**einsum反而更慢，但是去掉optimize参数后表现比内置的sum函数稍好，我认为**优化是有一个固定的成本**



2 还有一个坑需要注意的是，有些情况的省略号不加optimize会报错，就拿上面的栗子而言：

```python
np.einsum('...->', a, optimize=True)   # 正常运行
np.einsum('...->', a)   # 报错
```

很无奈，试了很多次，不加optimize就是会报错，但是并不是所有的省略号写法都需要加optimize，例如：

![[公式]](https://www.zhihu.com/equation?tex=c_%2A+%3D+%5Csum_i+a_%7Bi%2A%7D)

![[公式]](https://www.zhihu.com/equation?tex=c_%2A%3Da_%2A+b_%2A)

使用省略号实现上面两个公式并不需要加optimize，能够正常运行

```python
np.einsum('i...->...', a)   # 正常
np.einsum('...,...->...', a, b)   # 正常
```

但是如果碰到下面的公式：

![[公式]](https://www.zhihu.com/equation?tex=c_i+%3D+%5Csum_%2A+a_%7Bi%2A%7D)

上式表示将a除第一个维度之外，剩下的维度全部累加，这种实现就必须要加optimize

```python
np.einsum('i...->i', a, optimize=True)   # 必须加optimize，不然报错
```



补充例子：

```python
c = (a * b).sum()
# 如果不知道a, b的维数，使用einsum实现上面的功能也必须要加optimize
c = einsum('...,...->', a, b, optimize=True)
```

总结一下，在计算量很小时，优化因为有一定的成本，所以速度会慢一些；**但是**，既然计算量小，慢一点又怎样呢，而且**使用优化之后，可以更加肆意的使用省略号写表达式**，变量的维数也不用考虑了，所以建议无脑使用优化

