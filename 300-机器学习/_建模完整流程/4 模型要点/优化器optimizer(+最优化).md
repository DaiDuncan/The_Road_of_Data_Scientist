本**综述**中，我们介绍梯度下降的不同变形形式，总结这些算法面临的挑战，介绍最常用的优化算法，回顾并行和分布式架构，以及调研用于优化梯度下降的其他的策略。

| 主题                                                         | 内容                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 介绍梯度下降的不同变形形式                                   | 其中，**小批量梯度下降**是最受欢迎的                         |
| 简要总结在训练的过程中所面临的挑战                           |                                                              |
| 介绍最常用的优化算法<br />包括这些算法在解决以上挑战时的动机以及如何得到更新规则的推导形式 | 动量法，Nesterov加速梯度<br />Adagrad，Adadelta，RMSprop，Adam |
| 简单讨论在并行和分布式环境中优化梯度下降的算法和框架         |                                                              |
| 思考对优化梯度下降有用的一些其他策略                         | 洗牌和课程学习，批量归一化和early stopping                   |





## ML optimizer定义

> finding the parameters $\theta$ of a neural network that significantly reduce a cost function $J(\theta)$ , which typically includes a performance measure evaluated on the **entire training set** as well as **additional regularization terms**



# 梯度下降法(Gradient Descent)

虽然梯度下降优化算法越来越受欢迎，但通常作为黑盒优化器使用，因此很难对其优点和缺点的进行实际的解释

- 学习率 => 步长



![image-20210319085952248](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319085952.png)

梯度下降法目前主要分为三种方法，区别在于每次参数更新时计算的样本数据量不同：

- 批量梯度下降法(BGD, Batch Gradient Descent)
- 随机梯度下降法(SGD, Stochastic Gradient Descent)
- 小批量梯度下降法(Mini-batch Gradient Descent)

根据数据量的不同，我们在**参数更新的精度**和**更新过程中所需要的时间**两个方面做出权衡。



## 批量梯度下降法BGD

![image-20210319090104068](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319090104.png)

批梯度下降法的速度会很慢

同时，批梯度下降法无法处理超出内存容量限制的数据集。

批梯度下降法同样也不能**在线更新模型，即在运行的过程中，不能增加新的样本**。



### 代码

```python
for i in range(nb_epochs):
    params_grad = evaluate_gradient(loss_function, data, params)
    params = params - learning_rate * params_grad
```

对于给定的迭代次数，首先，我们利用全部数据集计算损失函数关于参数向量`params`的梯度向量`params_grad`

​	注意，最新的深度学习库中提供了自动求导的功能，可以有效地计算关于参数梯度

​	如果你自己求梯度，那么，梯度检查是一个不错的主意  [link: 正确检查梯度的一些技巧](http://cs231n.github.io/neural-networks-3/)

然后，我们利用梯度的方向和学习率更新参数，学习率决定我们将以多大的步长更新参数。

- 对于凸误差函数，批梯度下降法能够保证收敛到全局最小值
- 对于非凸函数，则收敛到一个局部最小值。



## 随机梯度下降法SGD

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319090156.png" alt="image-20210319090156667" style="zoom:80%;" />

对于大数据集，因为批梯度下降法在每一个参数更新之前，会对相似的样本计算梯度，所以在计算过程中会有冗余。

而SGD在每一次更新中只执行一次，从而消除了冗余。



因而，通常SGD的运行速度更快，同时，可以用于在线学习。

SGD以高方差频繁地更新，导致目标函数出现如图1所示的剧烈波动。

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319100502.png" alt="image-20210319100502523" style="zoom:80%;" />



与批梯度下降法的收敛会使得损失函数陷入局部最小相比，由于SGD的波动性，一方面，波动性使得SGD可以跳到新的和潜在更好的局部最优。

另一方面，这使得最终收敛到特定最小值的过程变得复杂，因为SGD会一直持续波动。



然而，已经证明当我们缓慢减小学习率，SGD与批梯度下降法具有相同的收敛行为，对于非凸优化和凸优化，可以分别收敛到局部最小值和全局最小值。

### 代码

与批梯度下降的代码相比，SGD的代码片段仅仅是在对训练样本的遍历和利用每一条样本计算梯度的过程中增加一层循环。

```python
for i in range(nb_epochs):
    np.random.shuffle(data)	#每一次循环中，打乱训练样本
    for example in data:
        params_grad = evaluate_gradient(loss_function, example, params)
        params = params - learning_rate * params_grad
```







## 小批量梯度下降法⭐

小批量梯度下降法就是结合BGD和SGD的折中

对于含有n个训练样本的数据集，每次参数更新，选择一个==大小为m(m<n)的mini-batch==数据样本计算其梯度，其参数更新公式如下：

![image-20210319090246247](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319090246.png)

小批量梯度下降法即保证了训练的速度，又能保证最后收敛的准确率，目前的SGD默认是小批量梯度下降算法。



### 代码

不是在所有样本上做迭代，我们现在只是在大小为50的小批量数据上做迭代：

```python
for i in range(nb_epochs):
    np.random.shuffle(data)
    for batch in get_batches(data, batch_size=50):
        params_grad = evaluate_gradient(loss_function, batch, params)
        params = params - learning_rate * params_grad
```



### 特性

通常，小批量数据的大小在50到256之间，也可以根据不同的应用有所变化。



优点：

a)减少参数更新的方差，这样可以得到更加稳定的收敛结果；

b)可以利用最新的深度学习库中高度优化的矩阵优化方法，高效地求解每个小批量数据的梯度。



## 小结|挑战

[link: cs231n](https://cs231n.github.io/neural-networks-3/)

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319104950.png" alt="image-20210319104950608" style="zoom:80%;" />

- 选择合适的learning rate比较困难，学习率太低会收敛缓慢，学习率过高会使收敛时的波动过大，导致损失函数在最小值附近波动甚至偏离最小值
  - 学习率调整[<u>*17-指定参考文献的页码*</u>]试图在训练的过程中通过例如退火的方法调整学习率，即根据预定义的策略或者当相邻两代之间的**下降值小于某个阈值时**减小学习率。
  - 然而，策略和阈值需要预先设定好，因此无法适应数据集的特点[4]
- 所有参数都是用同样的learning rate：
  - 如果数据是稀疏的，同时，特征的频率差异很大时，我们也许不想以同样的学习率更新所有的参数
  - 对于出现次数较少的特征，我们对其执行更大的学习率。
- ==高度非凸==误差函数普遍出现在神经网络中，在优化这类函数时，另一个关键的挑战是使函数避免陷入无数次优的局部最小值。
  - Dauphin等人[5]指出出现这种困难实际上==并不是来自局部最小值，而是来自鞍点==，即那些在一个维度上是递增的，而在另一个维度上是递减的。这些鞍点通常被具有相同误差的点包围，因为在任意维度上的梯度都近似为0，所以SGD很难从这些鞍点中逃开。
  - ==SGD容易收敛到局部最优，并且在某些情况下可能被困在鞍点==





# 优化|动量优化法

引入物理学中的动量思想，加速梯度下降，有Momentum和Nesterov两种算法。

当我们将一个小球从山上滚下来，没有阻力时，它的动量会越来越大，但是如果遇到了阻力，速度就会变小，动量优化法就是借鉴此思想，使得梯度方向在不变的维度上，参数更新变快，梯度有所改变时，更新参数变慢，这样就能够加快收敛并且减少动荡。



SGD很难通过陡谷，即在一个维度上的表面弯曲程度远大于其他维度的区域[19]，这种情况通常出现在局部最优点附近。在这种情况下，SGD摇摆地通过陡谷的斜坡，同时，沿着底部到局部最优点的路径上只是缓慢地前进，这个过程如图2a所示。

如图2b所示，动量法[16]是一种帮助SGD在相关方向上加速并抑制摇摆的一种方法。



<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319102859.png" alt="image-20210319102859249" style="zoom:80%;" />







## Momentum

参数更新时在一定程度上保留之前更新的方向，同时又利用当前batch的梯度微调最终的更新方向，简言之就是通过**积累之前的动量**来加速当前的梯度。

假设$m_t$表示t时刻的动量， $\mu$表示动量因子，通常取值0.9或者近似值，在SGD的基础上增加动量，则参数更新公式如下：

![image-20210319090539661](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319090539.png)

在梯度方向改变时，momentum能够降低参数更新速度，从而减少震荡；

在梯度方向相同时，momentum可以加速参数更新， 从而加速收敛。

总而言之，momentum能够==加速SGD收敛，抑制震荡==。



### 特性

- 下降初期时，使用上一次参数更新，下降方向一致，乘上较大的$\mu$能够进行很好的加速
- 下降中后期时，在局部最小值来回震荡的时候，梯度趋于0，$\mu$使得更新幅度增大，跳出陷阱
- 在梯度改变方向的时候，$\mu$能够减少更新 

总而言之，momentum项能够在相关方向加速SGD，抑制振荡，从而加快收敛



## NAG（Nesterov accelerated gradient）

球从山上滚下的时候，盲目地沿着斜率方向，往往并不能令人满意。

我们希望有一个智能的球，这个球能够知道它将要去哪，以至于在重新遇到斜率上升时能够知道减速。

![image-20210319090703311](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319090703.png)

momentum首先计算一个梯度(短的蓝色向量)，然后在更新累积梯度的方向进行一个大的跳跃(长的蓝色向量)

nesterov项首先在之前累积的梯度方向进行一个大的跳跃(棕色向量)，计算梯度然后进行校正(绿色梯向量)。这个具有预见性的更新防止我们前进得太快，同时增强了算法的响应能力，这一点在很多的任务中对于RNN的性能提升有着重要的意义[2]。



对于NAG的直观理解的另一种解释可以参见http://cs231n.github.io/neural-networks-3/，同时Ilya Sutskever在其博士论文[18]中给出更详细的综述。



既然我们能够使得我们的更新适应误差函数的斜率以相应地加速SGD，我们同样也想要使得我们的更新能够适应每一个单独参数，以根据每个参数的重要性决定大的或者小的更新。



## 小结

momentum项和nesterov项都是为了==使梯度更新更加灵活==，对不同情况有针对性

但是，人工设置一些学习率总还是有些生硬

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319105038.png" alt="image-20210319105038616" style="zoom:80%;" />



# 优化|自适应学习率优化

在机器学习中，学习率是一个非常重要的超参数，但是学习率是非常难确定的，虽然可以通过多次训练来确定合适的学习率，但是一般也不太确定多少次训练，能够得到最优的学习率 

=> 确定学习率是玄学事件，对人为的经验要求比较高

- 对于出现次数较少的特征，我们对其采用更大的学习率 => 因此，Adagrad非常适合处理稀疏数据
- 对于出现次数较多的特征，我们对其采用较小的学习率

Dean等人[6]发现Adagrad能够极大提高了SGD的鲁棒性并将其应用于Google的大规模神经网络的训练，其中包含了[YouTube视频](http://www.wired.com/2012/06/google-x-neural-network/)中的猫的识别。此外，Pennington等人[15]利用Adagrad训练Glove词向量，因为低频词比高频词需要更大的步长。



存在一些策略自适应地调节学习率的大小，从而提高训练速度。 

目前的自适应学习率优化算法主要有：AdaGrad算法，RMSProp算法，Adam算法以及AdaDelta算法。

## AdaGrad(梯度平方和累积)

定义参数：

- 全局学习率$\delta$，一般会选择$\delta=0.01$
- 一个极小的常量$\epsilon$，通常取值10e-8，目的是为了分母不为0
- 梯度加速变量(gradient accumulation variable) r

![image-20210319090956185](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319090956.png)

![image-20210319091129852](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319091130.png)



更准确的定义：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319103508.png" alt="image-20210319103508506" style="zoom:80%;" />



Adagrad算法的一个主要优点是无需手动调整学习率。

在大多数的应用场景中，通常采用常数0.01



Adagrad的一个主要缺点是它在分母中累加梯度的平方：由于没增加一个正项，在整个训练过程中，累加的和会持续增长。

这会导致学习率变小以至于最终变得无限小，在学习率无限小时，Adagrad算法将无法取得额外的信息。接下来的Adadelta算法旨在解决这个不足。





### 特性

- -仍需要手工设置一个全局学习率$\delta$, 如果$\delta$设置过大的话，会使regularizer过于敏感，对梯度的调节太大
- +前期能够放大梯度
- -中后期，分母上梯度累加的平方和会越来越大，使得参数更新量趋近于0，使得训练提前结束，无法学习
- +适合处理稀疏梯度



## Adadelta(指数衰减$\rho$: 窗口 平方均值)

Adagrad会累加之前==所有的==梯度平方

而Adadelta只累加固定大小w(计算历史梯度的窗口)的项，无需存储先前的w个平方梯度，而是将梯度的平方 递归地表示成所有历史梯度平方的均值

![Figure1](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319092222.gif)

从上式中可以看出，Adadelta其实还是依赖于全局学习率 $\delta$ ，但是作者做了一定处理，经过近似牛顿迭代法之后

==在t时刻的均值$E[g^2]_t$只取决于先前的均值和当前的梯度==（分量$\rho$类似于动量项）：

![image-20210319092314791](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319092314.png)

可以看出Adadelta已经不依赖全局learning rate了



### 准确描述

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319104307.png" alt="image-20210319104306636" style="zoom:80%;" />

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319104323.png" alt="image-20210319104323521" style="zoom:80%;" />





### 特性

- +训练初中期，加速效果不错，很快。
- -训练后期，反复在局部最小值附近抖动。



## RMSprop(指数衰减$\rho$: 移动平均)

RMSprop是一个未被发表的自适应学习率的算法，该算法由Geoff Hinton在其[Coursera课堂的课程6e](http://www.cs.toronto.edu/~tijmen/csc321/slides/lecture_slides_lec6.pdf)中提出。

- RMSprop和Adadelta在相同的时间里被独立的提出，都起源于对Adagrad的极速递减的学习率问题的求解



RMSProp算法修改了AdaGrad的梯度平方和累加为==指数加权的移动平均==，使得其在非凸设定下效果更好。

- RMSprop将学习率分解成一个平方梯度的指数衰减的平均



设定参数：

- 全局初始学习率$\delta$, 默认设为0.001; 
- decay rate $\rho$,默认设置为0.9
- 一个极小的常量$\epsilon$，通常为10e-6

![image-20210319092514601](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319092514.png)

### 特点

- 其实RMSprop依然依赖于全局学习率$\delta$
- RMSprop算是Adagrad的一种发展，和Adadelta的变体，效果趋于二者之间
- 适合处理==非平稳目标(包括季节性和周期性)==——对于RNN效果很好





## Adam⭐(指数衰减$\rho$: 平方梯度平均 + 梯度均值)

 Adaptive Moment Estimation

Adam对每一个参数都计算自适应的学习率

- 除了像Adadelta和RMSprop一样存储一个指数衰减的历史平方梯度的平均$v_t$
- Adam同时还保存一个历史梯度的指数衰减均值$m_t$，类似于动量



Adam中动量直接并入了梯度一阶矩（指数加权）的估计。

其次，相比于缺少修正因子，导致二阶矩估计可能在训练初期具有很高偏置的RMSProp，Adam包括偏置修正，修正从原点初始化的一阶矩（动量项）和（非中心的）二阶矩估计。

- $m_t$和$v_t$分别是对梯度的一阶矩（均值）和二阶矩（非确定的方差）的估计
- 当$m_t$和$v_t$初始化为0向量时，Adam的作者发现它们都偏向于0，尤其是在初始化的步骤和当衰减率很小的时候（例如β1和β2趋向于1）



默认参数值设定为：

![image-20210319092644932](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319092645.png)

迭代过程：

![Figure2](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319092714.gif)

其中：

- $m_t, n_t$分别是对梯度的一阶矩估计和二阶矩估计
- $\hat{m}_t, \hat{n}_t是对$$m_t, n_t$的偏差校正，这样可以近似为对期望的无偏估计



### 特性

- 作者从经验上表明Adam在实际中表现很好，同时，与其他的自适应学习算法相比，其更有优势。

- Adam梯度经过偏置校正后，每一次迭代学习率都有一个固定范围，使得参数比较平稳。
- 结合了Adagrad善于处理稀疏梯度和RMSprop==善于处理非平稳目标==的优点
- +为不同的参数计算不同的自适应学习率
- +也==适用于大多非凸优化问题==——适用于大数据集和高维空间。
- +对内存需求较小



## YamAdam 

[link: Yamada modified Adam](https://www.biorxiv.org/content/10.1101/348557v4.full)

![Figure3](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319112421.gif)



## Adamax

Adamax是Adam的一种变体，此方法对学习率的上限提供了一个更简单的范围

公式上的变化如下：

可以看出，Adamax学习率的边界范围更简单

![image-20210319112458321](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319112458.png)



## Nadam

类似于带有Nesterov动量项的Adam

公式如下：

![image-20210319112544275](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319112544.png)

Nadam对学习率有了更强的约束，同时对梯度的更新也有更直接的影响。

一般而言，在想使用带动量的RMSprop，或者Adam的地方，大多可以使用Nadam取得更好的效果。





# #总结|算法的表现

下图是各个算法在等高线的表现，它们都从相同的点出发，走不同的路线达到最小值点。

可以看到：

- Adagrad，Adadelta和RMSprop在正确的方向上很快地转移方向，并且快速地收敛
- Momentum和NAG先被领到一个偏远的地方，然后才确定正确的方向
  - NAG比momentum率先更正方向：通过对最优点的预见增强其响应能力
- SGD则是缓缓地朝着最小值点前进

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319094913.gif" alt="优化算法Optimizer比较和总结" style="zoom:80%;" />



鞍点对SGD的训练造成很大困难

- 注意，SGD，动量法和NAG在鞍点处很难打破对称性，尽管后面两个算法最终设法逃离了鞍点
- 而Adagrad，RMSprop和Adadelta能够快速想着梯度为负的方向移动，其中Adadelta走在最前面

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319104855.gif" alt="img" style="zoom:80%;" />



## 应用|选择使用哪种优化算法

自适应学习速率的方法，即 Adagrad、 Adadelta、 RMSprop 和Adam，最适合这些场景下最合适，并在这些场景下得到最好的收敛性

如果输入数据是稀疏的，选择任一自适应学习率算法可能会得到最好的结果。

- 选用这类算法的另一个好处是无需调整学习率，选用默认值就可能达到最好的结果。



总的来说，RMSprop是Adagrad的扩展形式，用于处理在Adagrad中急速递减的学习率

RMSprop与Adadelta相同，所不同的是Adadelta在更新规则中使用参数的均方根进行更新

最后，Adam是将偏差校正和动量加入到RMSprop中

在这样的情况下，**RMSprop、Adadelta和Adam是很相似的算法**并且在相似的环境中性能都不错。

- 在想使用带动量的RMSprop，或者Adam的地方，**大多可以使用Nadam**取得更好的效果



Kingma等人[9]指出在优化后期由于梯度变得越来越稀疏，偏差校正能够帮助Adam微弱地胜过RMSprop。综合看来，Adam可能是最佳的选择。





有趣的是，最近许多论文中采用不带动量的SGD和一种简单的学习率的退火策略。

已表明，通常SGD能够找到最小值点，但是比其他优化的SGD**花费更多的时间**，与其他算法相比，SGD更加依赖鲁棒的初始化和退火策略，同时，SGD**可能会陷入鞍点，而不是局部极小值点**。

- SGD通常训练时间更长，但是在好的初始化和学习率调度方案的情况下，结果更可靠



因此，如果你关心的是快速收敛和训练一个深层的或者复杂的神经网络，你应该选择一个自适应学习率的方法。



# SGD进阶|并行和分布式

当存在大量的大规模数据和廉价的集群时，利用分布式SGD来加速是一个显然的选择。

SGD本身有固有的顺序：一步一步，我们进一步进展到最小。

SGD提供了良好的收敛性，但SGD的运行缓慢，特别是对于大型数据集。

相反，SGD异步运行速度更快，但客户端之间非最理想的通信会导致差的收敛。

此外，我们也可以在一台机器上并行SGD，这样就无需大的计算集群。



以下是已经提出的优化的并行和分布式的SGD的算法和框架。



## 1 Hogwild!

Niu等人[14]提出称为Hogwild!的更新机制，Hogwild!允许在多个CPU上并行执行SGD更新。

在无需对参数加锁的情况下，处理器可以访问共享的内存。

这种方法只适用于稀疏的输入数据，因为每一次更新只会修改一部分参数。

在这种情况下，该更新策略几乎可以达到一个最优的收敛速率，因为CPU之间不可能重写有用的信息。



## 2 Downpour SGD

Downpour SGD是SGD的一种异步的变形形式，在Google，Dean等人[6]在他们的DistBelief框架（TensorFlow的前身）中使用了该方法。

Downpour SGD在训练集的子集上并行运行多个模型的副本。

这些模型将各自的更新发送给一个参数服务器，参数服务器跨越了多台机器。每一台机器负责存储和更新模型的一部分参数。

然而，因为副本之间是彼此不互相通信的，即通过共享权重或者更新，因此可能会导致参数发散而不利于收敛。



## 3 延迟容忍SGD⭐

通过容忍延迟算法的开发，McMahan和Streeter[11]将AdaGraad扩展成并行的模式，该方法不仅适应于历史梯度，同时适应于更新延迟。

该方法已经在实践中被证实是有效的。



## 4 TensorFlow

[TensorFlow](https://www.tensorflow.org/)[1]是Google近期开源的框架，该框架用于实现和部署==大规模机器学习模型==。

TensorFlow是基于DistBelief开发，同时TensorFlow已经在内部用来在大量移动设备和大规模分布式系统的执行计算。

在[2016年4月](http://googleresearch.blogspot.ie/2016/04/announcing-tensorflow-08-now-with.html)发布的分布式版本依赖于图计算，图计算即是对每一个设备将图划分成多个子图，同时，通过发送、接收节点对完成节点之间的通信。



## 5 弹性平均SGD

Zhang等人[22]提出的弹性平均SGD（Elastic Averaging SGD，EASGD）连接了异步SGD的参数客户端和一个弹性力，即参数服务器存储的一个中心变量。

EASGD使得局部变量能够从中心变量震荡得更远，这在理论上使得在参数空间中能够得到更多的探索。

经验表明这种增强的探索能力通过发现新的局部最优点，能够提高整体的性能。





# SGD优化的其他策略

介绍可以与前面提及到的任一算法配合使用的其他的一些策略，以进一步提高SGD的性能。

对于其他的一些常用技巧的概述可以参见[10]。

## 1 数据集的洗牌和课程学习

总的来说，我们希望避免向我们的模型中以一定意义的顺序提供训练数据，因为这样会使得优化算法产生偏差。

因此，在每一轮迭代后对训练数据洗牌是一个不错的主意。



另一方面，在很多情况下，我们是逐步解决问题的，而将训练集按照某个有意义的顺序排列会提高模型的性能和SGD的收敛性，如何将训练集建立一个有意义的排列被称为**课程学习**[3]。

Zaremba and Sutskever[20]只能使用课程学习训练LSTM来评估简单程序，并表明组合或混合策略比单一的策略更好，通过增加难度来排列示例。





## 2 批量归一化

为了便于学习，我们通常用0均值和单位方差初始化我们的参数的初始值来归一化。

随着不断训练，参数得到不同的程度的更新，我们失去了这种归一化，随着网络变得越来越深，这种现象会降低训练速度，且放大参数变化。



批量归一化[8]在每次小批量数据反向传播之后重新对参数进行0均值单位方差标准化。

通过将模型架构的一部分归一化，我们能够使用更高的学习率，更少关注初始化参数。

批量归一化还充当正则化的作用，减少（有时甚至消除）Dropout的必要性。



## 3 Early stopping⭐

如Geoff Hinton所说：“Early Stopping是美丽好免费午餐”（[NIPS 2015 Tutorial slides](http://www.iro.umontreal.ca/~bengioy/talks/DL-Tutorial-NIPS2015.pdf)）。

你因此必须在训练的过程中时常在验证集上监测误差，在验证集上如果损失函数不再显著地降低，那么应该提前结束训练。



## 4 梯度噪音

Neelakantan等人[12]在每个梯度更新中增加满足高斯分布$N(0,σ^2_t)$的噪音：

![image-20210319111828420](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319111828.png)

高斯分布的方差，需要根据如下的策略退火：

![image-20210319111842596](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319111842.png)

他们指出增加了噪音，使得网络对不好的初始化更加鲁棒，同时对深层的和复杂的网络的训练特别有益。

他们猜测增加的噪音使得模型更有机会逃离当前的局部最优点，==以发现新的局部最优点==，这在更深层的模型中更加常见。







# #拓展

不讨论在实际中不适合在高维数据集中计算的算法，例如诸如[牛顿法](https://en.wikipedia.org/wiki/Newton's_method_in_optimization)的二阶方法





# #参考文献

[link: 优化算法Optimizer比较和总结](https://zhuanlan.zhihu.com/p/55150256)

花书：深度学习

[link: Hyperparameter-free optimizer of stochastic gradient descent that incorporates unit correction and moment estimation](https://www.biorxiv.org/content/10.1101/348557v4.full)

[link: 原文|An overview of gradient descent optimization algorithms](https://ruder.io/optimizing-gradient-descent/)⭐

- [link: 针对原文的翻译版](https://blog.csdn.net/google19890102/article/details/69942970)

- [link: CS231n 学习材料](https://cs231n.github.io/optimization-1/)

[link: 深度学习最全优化方法总结比较（SGD，Adagrad，Adadelta，Adam，Adamax，Nadam）](https://zhuanlan.zhihu.com/p/22252270)

- 各种优化方法的详细内容及公式只好去认真啃论文

---

- [1] Abadi, M., Agarwal, A., Barham, P., Brevdo, E., Chen, Z., Citro, C., … Zheng, X. (2015). TensorFlow : Large-Scale Machine Learning on Heterogeneous Distributed Systems.
- [2] Bengio, Y., Boulanger-Lewandowski, N., & Pascanu, R. (2012). Advances in Optimizing Recurrent Networks. Retrieved from http://arxiv.org/abs/1212.0901
- [3] Bengio, Y., Louradour, J., Collobert, R., & Weston, J. (2009). Curriculum learning. Proceedings of the 26th Annual International Conference on Machine Learning, 41–48. http://doi.org/10.1145/1553374.1553380
- [4] Darken, C., Chang, J., & Moody, J. (1992). Learning rate schedules for faster stochastic gradient search. Neural Networks for Signal Processing II Proceedings of the 1992 IEEE Workshop, (September), 1–11. http://doi.org/10.1109/NNSP.1992.253713
- [5] Dauphin, Y., Pascanu, R., Gulcehre, C., Cho, K., Ganguli, S., & Bengio, Y. (2014). Identifying and attacking the saddle point problem in high-dimensional non-convex optimization. arXiv, 1–14. Retrieved from http://arxiv.org/abs/1406.2572
- [6] Dean, J., Corrado, G. S., Monga, R., Chen, K., Devin, M., Le, Q. V, … Ng, A. Y. (2012). Large Scale Distributed Deep Networks. NIPS 2012: Neural Information Processing Systems, 1–11. http://doi.org/10.1109/ICDAR.2011.95
- [7] Duchi, J., Hazan, E., & Singer, Y. (2011). Adaptive Subgradient Methods for Online Learning and Stochastic Optimization. Journal of Machine Learning Research, 12, 2121–2159. Retrieved from http://jmlr.org/papers/v12/duchi11a.html
- [8] Ioffe, S., & Szegedy, C. (2015). Batch Normalization : Accelerating Deep Network Training by Reducing Internal Covariate Shift. arXiv Preprint arXiv:1502.03167v3
- [9] Kingma, D. P., & Ba, J. L. (2015). Adam: a Method for Stochastic Optimization. International Conference on Learning Representations, 1–13.
- [10] LeCun, Y., Bottou, L., Orr, G. B., & Müller, K. R. (1998). Efficient BackProp. Neural Networks: Tricks of the Trade, 1524, 9–50. http://doi.org/10.1007/3-540-49430-8_2
- [11] Mcmahan, H. B., & Streeter, M. (2014). Delay-Tolerant Algorithms for Asynchronous Distributed Online Learning. Advances in Neural Information Processing Systems (Proceedings of NIPS), 1–9. Retrieved from http://papers.nips.cc/paper/5242-delay-tolerant-algorithms-for-asynchronous-distributed-online-learning.pdf
- [12] Neelakantan, A., Vilnis, L., Le, Q. V., Sutskever, I., Kaiser, L., Kurach, K., & Martens, J. (2015). Adding Gradient Noise Improves Learning for Very Deep Networks, 1–11. Retrieved from http://arxiv.org/abs/1511.06807
- [13] Nesterov, Y. (1983). A method for unconstrained convex minimization problem with the rate of convergence o(1/k2). Doklady ANSSSR (translated as Soviet.Math.Docl.), vol. 269, pp. 543– 547.
- [14] Niu, F., Recht, B., Christopher, R., & Wright, S. J. (2011). Hogwild! : A Lock-Free Approach to Parallelizing Stochastic Gradient Descent, 1–22.
- [15] Pennington, J., Socher, R., & Manning, C. D. (2014). Glove: Global Vectors for Word Representation. Proceedings of the 2014 Conference on Empirical Methods in Natural Language Processing, 1532–1543. http://doi.org/10.3115/v1/D14-1162
- [16] Qian, N. (1999). On the momentum term in gradient descent learning algorithms. Neural Networks : The Official Journal of the International Neural Network Society, 12(1), 145–151. http://doi.org/10.1016/S0893-6080(98)00116-6
- [17] H. Robinds and S. Monro, “A stochastic approximation method,” Annals of Mathematical Statistics, vol. 22, pp. 400–407, 1951.
- [18] Sutskever, I. (2013). Training Recurrent neural Networks. PhD Thesis.
- [19] Sutton, R. S. (1986). Two problems with backpropagation and other steepest-descent learning procedures for networks. Proc. 8th Annual Conf. Cognitive Science Society.
- [20] Zaremba, W., & Sutskever, I. (2014). Learning to Execute, 1–25. Retrieved from http://arxiv.org/abs/1410.4615
- [21] Zeiler, M. D. (2012). ADADELTA: An Adaptive Learning Rate Method. Retrieved from http://arxiv.org/abs/1212.5701
- [22] Zhang, S., Choromanska, A., & LeCun, Y. (2015). Deep learning with Elastic Averaging SGD. Neural Information Processing Systems Conference (NIPS 2015), 1–24. Retrieved from http://arxiv.org/abs/1412.6651



