[NumPy for Matlab users](https://numpy.org/doc/1.19/user/numpy-for-matlab-users.html#)

- [Introduction](https://numpy.org/doc/1.19/user/numpy-for-matlab-users.html#introduction)
- [Some Key Differences](https://numpy.org/doc/1.19/user/numpy-for-matlab-users.html#some-key-differences)
- ‘array’ or ‘matrix’? Which should I use?
  - [Short answer](https://numpy.org/doc/1.19/user/numpy-for-matlab-users.html#short-answer)
  - [Long answer](https://numpy.org/doc/1.19/user/numpy-for-matlab-users.html#long-answer)
- Table of Rough MATLAB-NumPy Equivalents
  - [General Purpose Equivalents](https://numpy.org/doc/1.19/user/numpy-for-matlab-users.html#general-purpose-equivalents)
  - [Linear Algebra Equivalents](https://numpy.org/doc/1.19/user/numpy-for-matlab-users.html#linear-algebra-equivalents)
- [Notes](https://numpy.org/doc/1.19/user/numpy-for-matlab-users.html#notes)
- [Customizing Your Environment](https://numpy.org/doc/1.19/user/numpy-for-matlab-users.html#customizing-your-environment)
- [Links](https://numpy.org/doc/1.19/user/numpy-for-matlab-users.html#links)

---

MATLAB®和NumPy / SciPy有很多共同点。 但是有很多差异。

创建NumPy和SciPy的目的是使用Python以最自然的方式进行数值和科学计算，而不是MATLAB®克隆

## 重点差异

| Matlab                                               | Numpy                               |
| :--------------------------------------------------- | :---------------------------------- |
| 基本数据类型：多维array(double浮点数)                | array                               |
| 索引：a(1)                                           | a[0]                                |
| 脚本语言；事后可添加GUI和创建功能完善的应用程序的API | 可处理矩阵堆叠stack                 |
| 按值传递；切片是复制原始数据                         | 按引用传递；切片是生成数组试图/view |

NumPy实用Arrays：

- 标准的vector/matrix/tensor类型
- 元素级运算和线代/矩阵级运算区别明显

Python 3.5：

- 之前：需要用dot代替*，进行张量(标量内积，矩阵向量乘法)
- 现在可以用矩阵乘法@运算符



## Numpy：包含array和matrix

- array class旨在成为用于多种数值计算的通用n维数组
- matrix class旨在专门帮助线性代数计算。 

实际上，两者之间只有几个主要区别。

|          | array                                              | matrix                           |
| -------- | -------------------------------------------------- | -------------------------------- |
| 运算符 * | 元素相乘                                           | 矩阵乘法                         |
| 运算符 @ | 矩阵乘法，multiply()，dot()                        | ==元素相乘==用multiply()         |
| 向量     | 1xN或Nx1：对一维数组转置仍是一维数组(默认为行向量) | 一维数组默认是行向量，或者列向量 |
|          |                                                    | A[:, 1]表示二维矩阵 Nx1          |
| 高维数组 | 维度>2                                             | 维度永远是2                      |
| 属性     | a.T                                                | .H 共轭转置                      |
|          |                                                    | .I 逆阵                          |
|          |                                                    | .A asarray()                     |
| 构造器   | 嵌套python序列：array([[1,2,3],[4,5,6]])           | 字符串：matrix("[1 2 3; 4 5 6]") |

针对array：

- +元素乘法很容易：A * B

- +所有操作（*，/，+，-等）都是按元素进行的
- -scipy.sparse中的稀疏矩阵不会与数组交互
- -必须记住，矩阵乘法具有自己的运算符@

针对matrix：

- +更像是MATLAB®矩阵
- +与scipy.sparse的交互更加干净
- -固定二维：要保存三维数据，您需要数组或矩阵的Python列表
- -固定二维：不能有向量。必须将它们强制转换为单列或单行矩阵
- -由于array是NumPy中的默认值，因此即使您将矩阵作为参数，某些函数也可能会返回一个数组
- -逐元素乘法需要调用一个函数multiple()



## ⭐matlab和numpy等价命令

```python
from numpy import *
import scipy.linalg
```

| #常用命令                                                    | Matlab             | numpy                                      |
| ------------------------------------------------------------ | ------------------ | ------------------------------------------ |
| func的解释                                                   | help func          | info(func) 或 help(func)                   |
|                                                              |                    | func? (在Ipython环境中)                    |
| 查找func定义的位置                                           | which func         |                                            |
| 打印func的Source                                             | type func          | source(func)                               |
|                                                              |                    | func?? (in Ipython)                        |
| 与逻辑(仅限标量)                                             | a && b             | a and b                                    |
| 或逻辑(仅限标量)                                             | a \|\| b           | a or b                                     |
| 复数                                                         | 1\*i, 1\*j, 1i, 1j | 1j                                         |
| 数据分辨最小精度：<br>最大小于1的浮点数到最小大于1的浮点数之间的距离 | eps                | np.spacing(1)                              |
| ODE(常微分方程)的积分：基于龙哥库塔4，5                      | ode45              | scipy.integrate.solve_ivp(f)               |
| ODE(常微分方程)的积分：基于BDF                               | ode15s             | scipy.integrate.solve_ivp(f, method='BDF') |



| #线性代数                                     | Matlab                          | numpy                                                        |
| --------------------------------------------- | ------------------------------- | ------------------------------------------------------------ |
| 数组维度                                      | ndims(a)                        | ndim(a) or a.ndim                                            |
| 数组元素的数量                                | numel(a)                        | size(a) or a.size                                            |
| 矩阵的维度                                    | size(a)                         | shape(a) or a.shape                                          |
| 第n个维度元素的数量：注意索引零点             | size(a,n)                       | a.shape[n-1]                                                 |
| 2x3矩阵                                       | [ 1 2 3; 4 5 6 ]                | array([[1.,2.,3.], [4.,5.,6.]])                              |
| 分块矩阵                                      | [ a b; c d ]                    | block([[a,b], [c,d]])                                        |
| 访问最后一个元素                              | a(end)                          | a[-1]                                                        |
| ==访问元素==                                  | a(2,5)                          | a[1,4]                                                       |
|                                               | a(2,:)                          | a[1] or a[1,:]                                               |
|                                               | a(1:5,:)                        | a[0:5] or a[:5] or a[0:5,:]                                  |
|                                               | a(end-4:end,:)                  | a[-5:]                                                       |
| 只读                                          | a(1:3,5:9)                      | a[0:3]\[:,4:9]                                               |
|                                               | a([2,4,5],[1,3])                | a[ix_([1,3,4],[0,2])]                                        |
| 跳行选择                                      | a(3:2:21,:)                     | a[ 2:21:2,:]                                                 |
|                                               | a(1:2:end,:)                    | a[ ::2,:]                                                    |
| ==逆序==                                      | a(end:\-1:1,:) or flipud(a)     | a[ ::-1,:]                                                   |
|                                               | a([1:end 1],:)                  | a[r_[:len(a),0]]                                             |
| 转置                                          | a.'                             | a.transpose() or a.T                                         |
| 共轭转置                                      | a'                              | a.conj().transpose() or a.conj().T                           |
| 矩阵乘法                                      | a * b                           | a @ b                                                        |
| 元素乘法                                      | a .* b                          | a * b                                                        |
| 元素除法                                      | a./b                            | a/b                                                          |
| 元素乘方                                      | a.^3                            | a**3                                                         |
| matlab比较后返回0和1 <br>numpy返回True和False | (a>0.5)                         | (a>0.5)                                                      |
| ==查找下标==                                  | find(a>0.5)                     | nonzero(a>0.5)                                               |
| 提取目标列                                    | a(:,find(v>0.5))                | a[:,nonzero(v>0.5)[0]]                                       |
| 提取目标列                                    | a(:,find(v>0.5))                | a[:,v.T>0.5]                                                 |
|                                               | a(a<0.5)=0                      | a[a<0.5]=0                                                   |
|                                               | a .* (a>0.5)                    | a * (a>0.5)                                                  |
| 设置为3                                       | a(:) = 3                        | a[:] = 3                                                     |
| ==引用 => 赋值==                              | y=x                             | y = x.copy()                                                 |
|                                               | y=x(2,:)                        | y = x[1,:].copy()                                            |
| 数组转化为向量(包含copy)                      | y=x(:)                          | y = x.flatten()                                              |
|                                               |                                 | 保留顺序：x.flatten('F')                                     |
| 生成向量                                      | 1:10                            | arange(1.,11.) or r_[1.:11.] or r_[1:10:10j]                 |
|                                               | 0:9                             | arange(10.) or r_[:10.] or r_[:9:10j]                        |
| 生成列向量                                    | [1:10]'                         | arange(1.,11.)[:, newaxis]                                   |
| 生成零矩阵                                    | zeros(3,4)                      | zeros((3,4))                                                 |
|                                               | zeros(3,4,5)                    | zeros((3,4,5))                                               |
| 生成单位矩阵                                  | ones(3,4)                       | ones((3,4))                                                  |
|                                               | eye(3)                          | eye(3)                                                       |
| 生成对角阵                                    | diag(a)                         | diag(a)                                                      |
|                                               | diag(a,0)                       | diag(a,0)                                                    |
| 生成随机矩阵                                  | rand(3,4)                       | random.rand(3,4) or random.random_sample((3, 4))             |
| N个等距点                                     | linspace(1,3,4)                 | linspace(1,3,4)                                              |
| 2D数组                                        | [x,y]=meshgrid(0:8,0:5)         | mgrid[0:9.,0:6.] or meshgrid(r_[0:9.],r_[0:6.]               |
|                                               |                                 | ogrid[0:9.,0:6.] or ix_(r_[0:9.],r_[0:6.]                    |
|                                               | [x,y]=meshgrid([1,2,4],[2,4,5]) | meshgrid([1,2,4],[2,4,5])                                    |
|                                               |                                 | ix_([1,2,4],[2,4,5])                                         |
| m*n个a值                                      | repmat(a, m, n)                 | tile(a, (m, n))                                              |
| concatenate 水平堆叠                          | [a b]                           | concatenate((a,b),1) or hstack((a,b)) or column_stack((a,b)) or c_[a,b] |
| 竖直堆叠                                      | [a; b]                          | concatenate((a,b)) or vstack((a,b)) or r_[a,b]               |
| 最大元素                                      | max(max(a))                     | a.max()                                                      |
| 每列最大的元素                                | max(a)                          | a.max(0)                                                     |
| 比较a,b 提取更大值                            | max(a,b)                        | maximum(a, b)                                                |
| L~2~范数                                      | norm(v)                         | sqrt(v @ v) or np.linalg.norm(v)                             |
| ==元素逻辑运算==                              | a & b                           | logical_and(a,b)                                             |
|                                               | a\|b                            | logical_or(a,b)                                              |
| ==位逻辑运算==                                | bitand(a,b)                     | a & b                                                        |
|                                               | bitor(a,b)                      | a\|b                                                         |
| 逆矩阵                                        | inv(a)                          | linalg.inv(a)                                                |
| 伪逆                                          | pinv(a)                         | linalg.pinv(a)                                               |
| 秩                                            | rank(a)                         | linalg.matrix_rank(a)                                        |
| ax=b                                          | a\b = a^-1^b                    | linalg.solve(a,b) if a is square; linalg.lstsq(a,b) otherwise |
| xa=b                                          | b/a = ba^-1^                    | Solve a.T x.T = b.T instead                                  |
| ==奇异值分解==                                | [U,S,V]=svd(a)                  | U, S, Vh = linalg.svd(a), V = Vh.T                           |
| cholesky分解：上三角                          | chol(a)                         | linalg.cholesky(a).T                                         |
|                                               |                                 | linalg.cholesky(a) 得到下三角                                |
| 特征值与特征向量                              | [V,D]=eig(a)                    | D,V = linalg.eig(a)                                          |
|                                               | [V,D]=eig(a,b)                  | D,V = scipy.linalg.eig(a,b)                                  |
| k个最大的特征值与特征向量                     | [V,D]=eigs(a,k)                 |                                                              |
| ==QR分解==                                    | [Q,R,P]=qr(a,0)                 | Q,R = scipy.linalg.qr(a)                                     |
| ==LU分解==                                    | [L,U,P]=lu(a)                   | L,U = scipy.linalg.lu(a) or LU,P=scipy.linalg.lu_factor(a)   |
| 共轭梯度求解                                  | conjgrad                        | scipy.sparse.linalg.cg                                       |
|                                               | fft(a)                          | fft(a)                                                       |
|                                               | ifft(a)                         | ifft(a)                                                      |
| 排序                                          | sort(a)                         | sort(a) or a.sort()                                          |
| 行的排序                                      | [b,I] = sortrows(a,i)           | I = argsort(a[:,i]), b=a[I,:]                                |
| 多元线性回归                                  | regress(y,X)                    | linalg.lstsq(X,y)                                            |
| TP的降采样                                    | decimate(x, q)                  | scipy.signal.resample(x, len(x)/q)                           |
|                                               | unique(a)                       | unique(a)                                                    |
|                                               | squeeze(a)                      | a.squeeze()                                                  |



## 细节点

```python
### 1.子矩阵：基于ix_进行索引
ind=[1,3]
a[np.ix_(ind,ind)]+=100

### 2.逻辑运算
'''
numpy：3&4 == 0 #3(011) 4(100)
numpy中 &优先级高于<和>  => z = (x > 1) & (x < 2)
matlab相反
'''

### 3.线性索引 => reshape之后
```



## 补充|环境配置

### Matlab

使用您喜欢的函数的位置来修改搜索路径。您可以将此类自定义放入启动脚本中，以便MATLAB在启动时运行。



### Numpy/Python

- PYTHONPATH环境变量

- 交互式Python解释器时执行特定的脚本文件，请定义PYTHONSTARTUP环境变量以包含启动脚本的名称

即便设置了环境变量，也需要import调用

```python
# Make all numpy available via shorter 'np' prefix
import numpy as np
# Make all matlib functions accessible at the top level via M.func()
import numpy.matlib as M
# Make some matlib functions accessible directly at the top level via, e.g. rand(3,3)
from numpy.matlib import rand,zeros,ones,empty,eye
# Define a Hermitian function
def hermitian(A, **kwargs):
    return np.transpose(A,**kwargs).conj()
# Make some shortcuts for transpose,hermitian:
#    np.transpose(A) --> T(A)
#    hermitian(A) --> H(A)
T = np.transpose
H = hermitian
```

