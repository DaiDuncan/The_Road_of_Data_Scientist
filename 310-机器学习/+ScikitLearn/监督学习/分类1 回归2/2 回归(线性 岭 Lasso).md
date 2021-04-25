本文介绍线性回归模型：

- 从梯度下降和最小二乘的角度来求解线性回归问题，
- 以概率的方式解释了线性回归为什么采用平方损失，
- 然后介绍了线性回归中常用的两种范数来解决**过拟合**和**矩阵不可逆**的情况，
  - 分别对应岭回归和Lasso回归，
- 最后考虑到线性回归的局限性，介绍了一种局部加权线性回归，增加其**非线性表示能力**



## 补充

两种线性回归的缩减(shrinkage)方法

岭回归(Ridge Regression)

LASSO(Least Absolute Shrinkage and Selection Operator)

向前逐步回归计算回归系数



# 一、**线性回归**

## 线性回归

假设有数据有：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210421194840.webp" alt="图片" style="zoom:80%;" />

其中：m为训练集样本数，n为样本维度/向量维度

- $x^{(i)}=\{x_1^{(i)},..., x_j^{(i)}, ..., x_n^{(i)}  \}$是向量
- $y^i \in \mathbb{R}$，样本真实标签

线性回归采用一个**高维的线性函数**来尽可能的拟合所有的数据点，最简单的想法就是最小化函数值与真实值误差的平方

- 概率解释：高斯分布加最大似然估计

即有如下目标函数：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210421200257.webp" alt="图片" style="zoom:67%;" />

其中线性函数如下：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210421200305.webp" alt="图片" style="zoom:67%;" />

构建好线性回归模型的目标函数之后，接下来就是求解目标函数的最优解，即一个优化问题。

常用的梯度优化方法都可以拿来用，这里以梯度下降法来求解目标函数。

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210421200323.png" alt="图片" style="zoom:67%;" />



另外，线性回归也可以从最小二乘法的角度来看

下面先将样本表示向量化，$x \in \mathbb{R}^{n \times m}, y \in \mathbb{R}^m$，成如下数据矩阵。

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210421200733.webp" alt="图片" style="zoom:67%;" />

那么**目标函数**向量化形式如下：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210421200614.webp" alt="图片" style="zoom:67%;" />

可以看出目标函数是一个**凸二次规划问题**，其最优解在导数为0处取到。

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210421200621.png" alt="图片" style="zoom:67%;" />

==值得注意的上式中存在计算矩阵的逆== => 避免数据特征中存在共线性(即相关性比较大)

- 一般来讲当**样本数m大于数据维度n时**，矩阵可逆，可以采用最小二乘法求得目标函数的闭式解。

- 当数据维度大于样本数时，矩阵线性相关，不可逆。

此时最小化目标函数解不唯一，且非常多，出于这样一种情况，我们可以考虑**奥卡姆剃刀准则来简化模型复杂度**，使其**不必要的特征对应的w为0**。

所以引入正则项使得模型中w的**非0个数**最少。

当然，岭回归，lasso回归的**最根本的目的不是解决不可逆问题，而是防止过拟合**。



## **概率解释**

损失函数与最小二乘法采用最小化平方和的概率解释。

假设模型预测值与真实值的误差为$\epsilon^{(i)}$，那么预测值$h_{\theta}(x^{(i)})$与真实值$y^{(i)}$之间有如下关系：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210421201013.webp" alt="图片" style="zoom:80%;" />

根据中心极限定理，==当一个事件与很多**独立随机变量**有关，该事件服从正态分布==

一般来说，**连续值我们都倾向于假设服从正态分布**。

假设每个样本的误差$\epsilon^{(i)}$，独立同分布 => 服从均值为0，方差为σ的高斯分布 $\epsilon^{(i)} \sim N(0, \sigma^2)$，所以有：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210421201237.png" alt="图片" style="zoom:80%;" />

即==表示$y^{(i)}$满足以均值为$h_{\theta}(x^{(i)})$、方差为$\epsilon^{(i)}$的高斯分布：==

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210421201435.png" alt="图片" style="zoom:80%;" />

由最大似然估计有：(直接代入样本$x_i, y_i$的值)

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210421201540.png" alt="图片" style="zoom: 50%;" />



# 二、**岭回归和Lasso回归**

## 岭回归|二范数 => 矩阵可逆化

岭回归的目标函数在一般的线性回归的基础上==加入了正则项==，在保证最佳拟合误差的同时，**使得参数尽可能的“简单”**，**使得模型的泛化能力强**（即不过分相信从训练数据中学到的知识）

正则项：针对参数$\theta$ => 一般采用一，二范数

- 使得模型更具有泛化性，
- 同时可以解决线性回归中不可逆情况

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210421201708.png" alt="图片" style="zoom:80%;" />

其**迭代优化函数**如下：@梯度下降

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210421201826.webp" alt="图片" style="zoom:67%;" />

另外从最小二乘的角度来看，通过引入二范正则项，使其主对角线元素来强制矩阵可逆。

- @参考上述线性回归的目标函数 => 加上正则项$J(\theta)=\frac{1}{2}[(\theta^T \cdot x - y^T)(\theta^T \cdot x - y^T)^T + \lambda \cdot ||\theta||^2]$
- 目标函数对参数$\theta$求导后：得到下式

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210421202128.png" alt="图片" style="zoom:80%;" />

@me|理解：如果$\lambda$可逆调节能力太强 => 也意味着正则化惩罚很大

### 岭回归的几何意义

以两个变量为例, 残差平方和可以表示为$w_1, w_2$的一个二次函数，是一个在三维空间中的抛物面，可以用等值线来表示。

而限制条件$w_1^2 + w_2^2 < t$， 相当于在二维平面的一个圆。

这个时候等值线与圆相切的点便是在约束条件下的最优点，如下图所示，

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210421212802.png" alt="img" style="zoom:80%;" />

### 岭回归的性质

- 当岭参数$\lambda=0$时，得到的解是最小二乘解
- 当岭参数$\lambda$**趋向更大**时，岭回归系数$w_i$趋向于0，约束项t很小



### 岭回归|Python实现

```python
def ridge_regression(X, y, lambd=0.2):	#不同的lambda得到不同的w值
    ''' 获取岭回归系数
    '''
    XTX = X.T*X
    m, _ = XTX.shape
    I = np.matrix(np.eye(m))
    w = (XTX + lambd*I).I*X.T*y
    return w
```



### 岭迹图

可以知道求得的岭系数 ![[公式]](https://www.zhihu.com/equation?tex=w_i) 是岭参数 ![[公式]](https://www.zhihu.com/equation?tex=%5Clambda) 的函数，不同的 ![[公式]](https://www.zhihu.com/equation?tex=%5Clambda) 得到不同的岭参数 ![[公式]](https://www.zhihu.com/equation?tex=w_i) , 因此我们可以增大 ![[公式]](https://www.zhihu.com/equation?tex=%5Clambda) 的值来得到岭回归系数的变化，以及岭参数的变化轨迹图(岭迹图), 

- 不存在奇异性时，岭迹图应稳定的逐渐趋向于0

通过岭迹图我们可以:

1. 观察较佳的 ![[公式]](https://www.zhihu.com/equation?tex=%5Clambda) 取值
2. 观察变量是否有多重共线性



上面我们通过函数ridge_regression实现了计算岭回归系数的计算，我们使用《机器学习实战》中的鲍鱼年龄的数据来进行计算并绘制不同$\lambda$的岭参数变化的轨迹图。

[代码 2017.10.28](https://github.com/PytLab/MLBox/tree/master/linear_regression)

```python
#== 选取30组不同的λλ来获取岭系数矩阵包含30个不同的岭系数
def ridge_traj(X, y, ntest=30):
    ''' 获取岭轨迹矩阵
    '''
    _, n = X.shape
    ws = np.zeros((ntest, n))
    for i in range(ntest):
        w = ridge_regression(X, y, lambd=exp(i-10))
        ws[i, :] = w.T
    return ws

#== 绘制岭轨迹图
if '__main__' == __name__:
    ntest = 30
    # 加载数据
    X, y = load_data('abalone.txt')
    # 中心化 & 标准化
    X, y = standarize(X), standarize(y)
    # 绘制岭轨迹
    ws = ridge_traj(X, y, ntest)
    fig = plt.figure()
    ax = fig.add_subplot(111)
    lambdas = [i-10 for i in range(ntest)]
    ax.plot(lambdas, ws)	#横轴是lambda
    plt.show()
```

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210421213208.png" alt="img" style="zoom: 67%;" />

上图绘制了回归系数 ![[公式]](https://www.zhihu.com/equation?tex=w_i) 与 ![[公式]](https://www.zhihu.com/equation?tex=log%28%5Clambda%29) 的关系：

- 在最左边 ![[公式]](https://www.zhihu.com/equation?tex=%5Clambda) 系数最小时(0以及负数)，可以得到**所有系数的原始值**(与标准线性回归相同); 
- 而在右边，系数全部缩减为0, 从不稳定趋于稳定；

==为了定量的找到最佳参数值，还需要进行交叉验证==。

要判断**哪些变量对结果的预测最具影响力，可以观察他们的系数大小**即可。





## Lasso回归|一范数

LASSO的惩罚项为: $\sum_{i=1}^{n} |w_i| \le t$

虽然只是形式稍有不同，但是得到的结果却又很大差别。

- 在LASSO中，当λ很小的时候，一些系数会随着变为0
- 而岭回归却很难使得某个系数**恰好**缩减为0. 

![img](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210421213749.png)

相比圆，方形的顶点更容易与抛物面相交，顶点就意味着对应的很多系数为0，而岭回归中的圆上的任意一点都很容易与抛物面相交很难得到正好等于0的系数。

这也就意味着，==lasso起到了很好的筛选变量的作用。==





Lasso回归采用一范数来约束，使参数**非零个数最少**。

而Lasso和岭回归的区别很好理解，在优化过程中，最优解为**函数等值线**与**约束空间的交集**，正则项可以看作是约束空间。

可以看出二范的约束空间是一个球形，而一范的约束空间是一个方形，这也就是二范会得到很多参数接近0的值，而一范则尽可能非零参数最少。

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210421202850.webp" alt="图片" style="zoom:80%;" />



### LASSO回归系数的计算

虽然惩罚函数只是做了细微的变化，但是相比岭回归可以直接通过矩阵运算得到回归系数相比，LASSO的计算变得相对复杂。

由于惩罚项中含有绝对值，此函数的导数是连续不光滑的，所以无法进行求导并使用梯度下降优化。

本部分使用**坐标下降法**对LASSO回归系数进行计算。



坐标下降法是==每次选择一个维度的参数==进行**一维优化**，然后不断的迭代对多个维度进行更新直到函数收敛。

SVM对偶问题的[优化算法SMO](http://pytlab.github.io/2017/09/01/%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0%E7%AE%97%E6%B3%95%E5%AE%9E%E8%B7%B5-SVM%E4%B8%AD%E7%9A%84SMO%E7%AE%97%E6%B3%95/)也是类似的原理

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210421214427.png" alt="image-20210421214427690" style="zoom:67%;" />

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210421214515.png" alt="image-20210421214515297" style="zoom: 33%;" />

下面我们分别对LASSO的cost function的两部分求解:

1) RSS部分

![image-20210422083442263](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210422083442.png)

求导：

![image-20210422083502357](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210422083502.png)

令：

![image-20210422083736875](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210422083737.png)

- $z_k = \sum_{i=1}^m x_{ik}^2$ 

得到:

![image-20210422083649169](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210422083649.png)



2）正则项

关于惩罚项的求导我们需要使用subgradient，可以参考[LASSO（least absolute shrinkage and selection operator） 回归中 如何用梯度下降法求解？](https://www.zhihu.com/question/22332436/answer/21068494)

![image-20210422083830086](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210422083830.png)

这样整体的偏导数:

![image-20210422083932431](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210422083932.png)

令 $\frac{\partial f(w)}{\partial w_k} = 0$ 得到

![image-20210422084013809](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210422084013.png)

通过上面的公式我们便可以==每次选取一维进行优化==，并不断迭代得到最优回归系数。

- k表示不同的维度 => $p_k, z_k$





### Lasso回归|Python实现

```python
def lasso_regression(X, y, lambd=0.2, threshold=0.1):
    ''' 通过坐标下降(coordinate descent)法获取LASSO回归系数
    '''
    # 计算残差平方和
    rss = lambda X, y, w: (y - X*w).T*(y - X*w)
    # 初始化回归系数w.
    m, n = X.shape
    w = np.matrix(np.zeros((n, 1)))
    r = rss(X, y, w)
    # 使用坐标下降法优化回归系数w
    niter = itertools.count(1)
    for it in niter:
        for k in range(n):
            # 计算常量值z_k和p_k
            z_k = (X[:, k].T*X[:, k])[0, 0]
            p_k = 0
            for i in range(m):
                p_k += X[i, k]*(y[i, 0] - sum([X[i, j]*w[j, 0] for j in range(n) if j != k]))
            if p_k < -lambd/2:
                w_k = (p_k + lambd/2)/z_k
            elif p_k > lambd/2:
                w_k = (p_k - lambd/2)/z_k
            else:
                w_k = 0
            w[k, 0] = w_k
        r_prime = rss(X, y, w)
        delta = abs(r_prime - r)[0, 0]
        r = r_prime
        print('Iteration: {}, delta = {}'.format(it, delta))
        if delta < threshold:
            break
    return w
```

我们选取$\lambda=10$, 收敛阈值为0.1来获取回归系数

```python
if '__main__' == __name__:
    X, y = load_data('abalone.txt')
    X, y = standarize(X), standarize(y)
    w = lasso_regression(X, y, lambd=10)
    y_prime = X*w
    # 计算相关系数
    corrcoef = get_corrcoef(np.array(y.reshape(1, -1)),
                            np.array(y_prime.reshape(1, -1)))
    print('Correlation coefficient: {}'.format(corrcoef))
```

结果：

> 迭代了150步收敛到0.1，计算相对比较耗时:
>
> ```python3
> Iteration: 146, delta = 0.1081124857935265
> Iteration: 147, delta = 0.10565615985365184
> Iteration: 148, delta = 0.10326058648411163
> Iteration: 149, delta = 0.10092418256476776
> Iteration: 150, delta = 0.09864540659987142
> Correlation coefficient: 0.7255254877587117
> ```



### LASSO回归系数轨迹

类似岭轨迹，我们也可以改变λ的值得到不同的回归系数

```python
ntest = 30
# 绘制轨迹
ws = lasso_traj(X, y, ntest)
fig = plt.figure()
ax = fig.add_subplot(111)
lambdas = [i-10 for i in range(ntest)]
ax.plot(lambdas, ws)
plt.show()
```

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210422084417.png" alt="img" style="zoom: 67%;" />

通过与岭轨迹图进行对比发现，随着λ的增大，系数逐渐趋近于0

- 但是岭回归没有系数真正为0，
- 而lasso中不断有系数变为0. 

迭代过程中输出如下图:

![img](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210422084450.jpeg)



### 逐步向前回归

LASSO计算复杂度相对较高，本部分稍微介绍一下逐步向前回归，他属于一种贪心算法，

- 给定初始系数向量，
- 然后不断迭代遍历每个系数，增加或减小一个很小的数，看看代价函数是否变小，
  - 如果变小就保留，
  - 如果变大就舍弃，
  - 然后不断迭代直到回归系数达到稳定。

```python
def stagewise_regression(X, y, eps=0.01, niter=100):
    ''' 通过向前逐步回归获取回归系数
    '''
    m, n = X.shape
    w = np.matrix(np.zeros((n, 1)))
    min_error = float('inf')
    all_ws = np.matrix(np.zeros((niter, n)))
    # 计算残差平方和
    rss = lambda X, y, w: (y - X*w).T*(y - X*w)
    for i in range(niter):
        print('{}: w = {}'.format(i, w.T[0, :]))
        for j in range(n):
            for sign in [-1, 1]:
                w_test = w.copy()
                w_test[j, 0] += eps*sign
                test_error = rss(X, y, w_test)
                if test_error < min_error:
                    min_error = test_error
                    w = w_test
        all_ws[i, :] = w.T
    return all_ws
```

我们取变化量为0.005，迭代步数为1000次，得到回归系数随着迭代次数的变化曲线:

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210422084647.png" alt="img" style="zoom:67%;" />

逐步回归算法的主要有点在于他可以帮助人们理解现有的模型并作出改进。

当构建了一个模型后，可以运行**逐步回归算法找出重要的特征**，及时==停止那些不重要特征的收集==。





# 三、**局部加权线性回归**

## 表示非线性的关系？

值得注意的是线性模型的表示能力有限，但是并不一定表示线性模型只能处理线性分布的数据。

这里有两种常用的线性模型**非线性化**。



对于上面的线性函数的构造，我们可以看出模型在以$x_0, x_1, ..., x_n$的坐标上是线性的，但是并不表示线性的模型就一定只能用于线性分布问题上。

假如我们只有一个特征$x_0$，而实际上回归值是$y=x_0^2$等，我们同样可以采用线性模型，因为我们完全可以把输入空间映射到高维空间$(x_1^3, x_1^2, x_1^1)$，其实这也是==核方法以及PCA空间变换的一种思想==

凡是对输入空间进行线性，非线性的变换，都是把输入空间映射到特征空间的思想，所以只需要把非线性问题转化为线性问题即可。



## 局部线性

另外一种是局部线性思想，即对每一个样本构建一个加权的线性模型。



考虑到线性回归的表示能力有限，可能出现**欠拟合**现象。

局部加权线性回归为**每一个待预测的点**构建一个加权的线性模型。

其加权的方式是根据**预测点**与**数据集中的点的距离**来为数据集中的点**赋权重**，

- 当某点距离预测点较远时，其权重较小，反之较大。



由于这种权重的机制引入使得局部加权线性回归产生了一种==局部分段拟合==的效果。

由于该方法==对于**每一个预测点**==构建一个加权线性模型，都要**重新计算与数据集中所有点的距离来确定权重值**，进而确定针对该预测点的线性模型，计算成本高，同时为了实现**无参估计来计算权重**，需要存储整个数据集。



局部加权线性回归，在线性回归基础上引入权重，其目标函数（下面的目标函数是==针对一个预测样本==的）如下：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210421203203.webp" alt="图片" style="zoom:67%;" />

一般选择下面的权重函数，权重函数选择并非因为其类似于高斯函数，而是**根据数据分布的特性**，但权重函数的选取并不一定依赖于数据特性。

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210421203217.png" alt="图片" style="zoom: 50%;" />

对于上面的目标函数，我们的目标同样是求解，使得损失函数最小化

同样局部加权线性回归可以采用梯度的方法，也可以从最小二乘法的角度给出闭式解。

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210421203302.png" alt="图片" style="zoom:80%;" />

其中W是**对角**矩阵，$W_{ii}=w^{(i)}$



# **代码实战|伪代码**

## 1 **线性回归**

```c++
/**
线性回归函数的实现，考虑一般的线性回归，最小平方和作为损失函数，则目标函数是一个无约束的凸二次规划问题，
由凸二次规划问题的极小值在导数为0处取到，且极小值为全局最小值，且有闭式解。
根据数学表达式实现矩阵之间的运算求得参数w。
**/
int regression(Matrix x,Matrix y)
{
    Matrix xT=x.transposeMatrix();
    Matrix xTx=xTx.multsMatrix(xT,x);
    Matrix xTx_1=xTx.niMatrix();
    Matrix xTx_1xT=xTx_1xT.multsMatrix(xTx_1,xT);
    Matrix ws;
    ws=ws.multsMatrix(xTx_1xT,y);
    cout<<"ws"<<endl;
    ws.print();
    return 0;
}
```





## 2 岭回归和Lasso回归

```c++
/**
下面的岭回归函数只是在一般的线性回归函数的基础上在对角线上引入了岭的概念，不仅有解决矩阵不可逆的线性，同样也有正则项的目的，
采用常用的二范数就得到了直接引入lam的形式。
**/

int ridgeRegres(Matrix x,Matrix y,double lam)
{
    Matrix xT=x.transposeMatrix();
    Matrix xTx=xTx.multsMatrix(xT,x);
    Matrix denom(xTx.row,xTx.col,lam,"diag");
    xTx=xTx.addMatrix(xTx,denom);
    Matrix xTx_1=xTx.niMatrix();
    Matrix xTx_1xT=xTx_1xT.multsMatrix(xTx_1,xT);
    Matrix ws=ws.multsMatrix(xTx_1xT,y);
    cout<<"ws"<<endl;
    ws.print();
    return 0;
}
```





## 3 局部加权线性回归





```c++
/**
局部加权线性回归是在线性回归的基础上对每一个测试样本（训练的时候就是每一个训练样本）在其已有的样本进行一个加权拟合，
权重的确定可以通过一个核来计算，常用的有高斯核（离测试样本越近，权重越大，反之越小），这样对每一个测试样本就得到了不一样的
权重向量，所以最后得出的拟合曲线不再是线性的了，这样就增加的模型的复杂度来更好的拟合非线性数据。
**/
//需要注意的是局部加权线性回归是对每一个样本进行权重计算，所以对于每一个样本都有一个权重w，所以下面的函数只是局部线性回归的一个主要辅助函数
Matrix locWeightLineReg(Matrix test,Matrix x,Matrix y,const double &k)
{
    Matrix w(x.row,x.row,0,"T");
    double temp=0;
    int i,j;

    /**
    根据测试样本点与整个样本的距离已经选择的核确定局部加权矩阵，采用对角线上为局部加权值
    **/
    for(i=0;i<x.row;i++)
    {
        temp=0;
        for(j=0;j<x.col;j++)
        {
            temp+=(test.data[0][j]-x.data[i][j])*(test.data[0][j]-x.data[i][j]);
        }
        w.data[i][i]=exp(temp/-2.0*k*k);
    }
    Matrix xT=x.transposeMatrix();
    Matrix wx=wx.multsMatrix(w,x);
    Matrix xTwx;
    xTwx=xTwx.multsMatrix(xT,wx);
    Matrix xTwx_1;
    xTwx_1=xTwx.niMatrix();
    Matrix xTwx_1xT;
    xTwx_1xT=xTwx_1xT.multsMatrix(xTwx_1,xT);
    Matrix xTwx_1xTw;
    xTwx_1xTw=xTwx_1xTw.multsMatrix(xTwx_1xT,w);
    Matrix ws = xTwx_1xTw * y;
    return ws;
}
```





# 总结

线性回归核心思想最小化平方误差，可以从最小化损失函数和最小二乘角度来看，优化过程可以采用梯度方法和闭式解。在闭式解问题中需要注意矩阵可逆问题。

考虑到过拟合和欠拟合问题，有岭回归和lasso回归来防止过拟合，局部加权线性回归通过加权实现非线性表示。



岭回归和LASSO均是在标准线性回归的基础上加上正则项来减小模型的方差。

这里其实便涉及到了权衡偏差(Bias)和方差(Variance)的问题。

- 方差针对的是模型之间的差异，即不同的训练数据得到模型的区别越大说明模型的方差越大。
- 而偏差指的是**模型预测值与样本数据**之间的差异。

所以为了在过拟合和欠拟合之前进行权衡，我们需要确定适当的模型复杂度来使得**总误差**最小。

![img](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210422084826.jpeg)





# #参考文献

[link: 原文|公众号-文杰：机器学习2019.10.16](https://mp.weixin.qq.com/s/1rEWRjeQQfX_EVvtFMRG6w)

**详细代码:** https://github.com/myazi/myLearn/blob/master/LineReg.cpp



[link: 岭回归和LASSO 2017.10.28](https://zhuanlan.zhihu.com/p/30535220#:~:text=LASSO(The%20Least%20Absolute%20Shrinkage,%E4%B8%BA0%E8%BF%9B%E8%A1%8C%E7%89%B9%E5%BE%81%E7%AD%9B%E9%80%89%E3%80%82 )

- 《Machine Learning in Action》
- [机器学习中的Bias(偏差)，Error(误差)，和Variance(方差)有什么区别和联系？](https://www.zhihu.com/question/27068705)
- [Lasso回归的坐标下降法推导](https://link.zhihu.com/?target=http%3A//blog.csdn.net/u012151283/article/details/77487729)
- [数据什么时候需要做中心化和标准化处理？](https://www.zhihu.com/question/37069477)