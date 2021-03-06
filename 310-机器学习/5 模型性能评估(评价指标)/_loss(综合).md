> 总的来说，损失函数的形式千变万化，但追究溯源还是万变不离其宗。
>
> 其本质便是给出一个能较全面合理的==描述两个特征或集合之间的相似性度量或距离度量==，
>
> - 针对某些特定的情况，如类别不平衡等，给予适当的惩罚因子进行权重的加减。
>
> 
>
> 大多数的损失都是基于**最原始的损失一步步改进的**，
>
> - 或提出更一般的形式，
> - 或提出更加具体实例化的形式。



损失函数用来==评价模型的预测值和真实值不一样的程度==，损失函数越好，通常模型的性能越好。

不同的模型用的损失函数一般也不一样。



损失函数分为经验风险损失函数和结构风险损失函数：

- 经验风险损失函数指：预测结果和实际结果的差别
- 结构风险损失函数是指：经验风险损失函数加上正则项。



## 损失函数|离散型变量

损失函数 (loss function) 是用来估量在一个样本点上模型的预测值 h(x) 与真实值 y 的不一致程度 => $L(y, h(x))$

- 0-1 损失函数 (misclassification)
- 指数损失函数 (adaBoost)
- 对数损失函数 (logitBoost)
- 支持向量损失函数 (support vector)

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210424121247.jpeg" alt="图片" style="zoom:67%;" />

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210424121503.jpeg)



## 损失函数|连续型变量

`评价`

- L2 损失函数 (regression boost)
- L1 损失函数
- Huber 损失函数

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210424121439.jpeg" alt="图片" style="zoom:67%;" />





# 深度学习Loss函数

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210318173010.png" alt="image-20210318173009882" style="zoom:80%;" />

## 区分损失函数(单个样本)、代价函数和目标函数

![image-20210318173111397](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210318173111.png)

![image-20210318173151249](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210318173151.png)

目标函数（Objective Function）通常是一个**更通用的术语**，表示任意希望被优化的函数，用于机器学习领域和非机器学习领域（比如运筹优化），比如说，最大似然估计（MLE）中的似然函数就是目标函数



一句话总结：A **loss function** is a ==part of a cost function== which is a type of an objective function



## 回归

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210608213900.webp" alt="图片" style="zoom:80%;" />

### 1 MSE

均方差损失Mean Squared Error Loss

机器学习、深度学习回归任务中最常用的一种损失函数，也称为 L2 Loss。其基本形式如下：

![image-20210318174411114](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210318174411.png)

优点： 收敛速度快，能够**对梯度给予合适的惩罚权重**，而不是“一视同仁”，使梯度更新的方向可以更加精确。

缺点： 对异常值十分敏感，梯度更新的方向很容易受离群点所主导，不具备鲁棒性。



#### 背后的假设：

实际上==在一定的假设下==，我们可以使用最大化似然得到均方差损失的形式。

假设**模型预测与真实值之间的误差服从标准高斯分布**($\mu=0, \sigma=1$)，给定一个xi，模型输出真实值yi的概率：

![image-20210318174527438](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210318174527.png)

进一步我们**假设数据集中N个样本点之间相互独立**，则给定所有x输出所有真实值y的概率，即似然（Likelihood）为所有$p(y_i|x_i)$的累乘：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210318174611.png" alt="image-20210318174611357"  />

通常为了计算方便，我们通常最大化对数似然（Log-Likelihood）：

![image-20210318174644812](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210318174657.png)

去掉与$\hat{y}_i$无关的第一项，然后转化为最小化负对数似然（Negative Log-Likelihood）：

![image-20210318174725869](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210318174726.png)

就是说在模型输出与真实值的误差服从高斯分布的假设下，最小化均方差损失函数与极大似然估计本质上是一致的

因此在这个假设能被满足的场景中（比如回归），均方差损失是一个很好的损失函数选择；

当==这个假设没能被满足的场景中（比如分类），均方差损失不是一个好的选择==





#### 补充|平方损失函数

标准形式如下：

![image-20210318170931364](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210318170931.png)

(1)经常应用于回归问题



### 2 MAE

平均绝对误差损失Mean Absolute Error Loss

也称为L1 Loss。其基本形式如下：

![image-20210318204535259](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210318204535.png)

优点： 对离群点（Outliers）或者异常值**更具有鲁棒性**。

缺点： 由图可知其在0点处的导数不连续，使得求解效率低下，导致收敛速度慢；而对于较小的损失值，其梯度也同其他区间损失值的梯度一样大，所以不利于网络的学习。



#### 背后的假设：

同样可以在一定的假设下通过最大化似然得到 MAE 损失的形式

假设模型预测与真实值之间的误差服从拉普拉斯分布 Laplace distribution($\mu=0, b=1$)，给定一个xi，模型输出真实值yi的概率：

![image-20210318205640648](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210318205640.png)

与上面推导 MSE 时类似，我们可以得到的负对数似然（Negative Log-Likelihood）实际上就是MAE 损失的形式：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210318205700.png" alt="image-20210318205700702" style="zoom:80%;" />

#### 补充|绝对值损失函数

计算预测值与目标值的差的绝对值：

![image-20210318170816979](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210318170817.png)



### 小结|MAE vs. MSE

1 MSE比MAE能够更快收敛

当使用梯度下降算法时，MSE损失的梯度为$-\hat{y}_i$，而MAE损失的梯度为$\pm 1$。

所以，MSE的梯度会随着误差大小发生变化，而MAE的梯度一直保持为1，这不利于模型的训练



2 MAE对异常点更加鲁棒

从损失函数上看，MSE对误差平方化，使得异常点的误差过大；

从两个损失函数的假设上看，MSE假设了误差服从高斯分布，MAE假设了误差服从拉普拉斯分布，==拉普拉斯分布本身对于异常点更加鲁棒(右图)==

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210318210215.png" alt="image-20210318210215015" style="zoom:80%;" />

> 对于L1范数和L2范数，如果异常值对于实际业务非常重要，我们可以使用MSE作为我们的损失函数；另一方面，如果异常值**仅仅表示损坏的数据**，那我们应该选择MAE作为损失函数。
>
> 此外，考虑到收敛速度，在大多数的卷积神经网络中（CNN）中，我们通常会选择L2损失。
>
> 但是，还存在这样一种情形，当你的业务数据中，存在95%的数据其真实值为1000，而剩下5%的数据其真实值为10时，如果你使用MAE去训练模型，则训练出来的模型会偏向于将所有输入数据预测成1000，因为MAE对离群点不敏感，趋向于取中值。
>
> 而采用MSE去训练模型时，训练出来的模型会偏向于将大多数的输入数据预测成10，因为它对离群点异常敏感。
>
> 因此，大多数情况这两种回归损失函数并不适用，能否有什么办法可以同时利用这两者的优点呢？ 
>
> - **Smooth L1 Loss**



### 1+2 smooth L1损失函数

SLL：来自Fast RCNN

综合L1和L2损失的优点

- 在0点处附近采用了L2损失中的平方函数，使用圆滑部分代替，使其更加平滑易于收敛
- 在|x|>1的区间上，它又采用了L1损失中的线性函数，使得梯度能够快速下降



smooth函数：

![image-20210319144005908](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319144006.png)

损失函数：

![image-20210319144016654](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319144016.png)

> 通过对这三个损失函数进行求导可以发现，
>
> L1损失的导数为常数，如果不及时调整学习率，那么当值过小时，会导致模型很难收敛到一个较高的精度，而是趋向于一个固定值附近波动。
>
> 反过来，对于L2损失来说，由于在训练初期值较大时，其导数值也会相应较大，导致训练不稳定。
>
> 最后，可以发现Smooth L1在训练初期输入数值较大时能够较为稳定在某一个数值，而在后期趋向于收敛时也能够加速梯度的回传，很好的解决了前面两者所存在的问题。

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210608214259.webp)





### 3 Huber Loss⭐

Huber Loss是一种将MSE与MAE结合起来，取两者优点的损失函数，也被称作Smooth Mean Absolute Error Loss

原理很简单，就是在误差接近0时使用MSE，误差较大时使用MAE，公式为：

![image-20210318211253005](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210318211253.png)

上式中，$\delta$是Huber Loss的一个超参数，$\delta$的值是MSE与MAE两个损失连接的位置。

下图为$\delta=1.0$时的Huber Loss：

![image-20210318211342223](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210318211342.png)

可以看到在$[-\delta, \delta]$内实际上就是MSE的损失，使损失函数可导并且梯度更加稳定；

在$(-\infty, -\delta)$和$(\delta, \infty)$区间内为MAE损失，降低了异常点的影响，使训练更加鲁棒



#### 示例

![image-20210319144121453](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319144121.png)

蓝线表示平方误差MSE

红线表示绝对误差MAE：在$\delta$增大的情况下，逐渐接近平方误差(异常值影响越来越大)

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319144137.gif" alt="img" style="zoom:80%;" />





### 4 分位数损失Quantile Loss

分位数回归Quantile Regression是一类在实际应用中非常有用的回归算法，通常的回归算法是拟合目标值的**期望（MSE）或者中位数（MAE）**，而分位数回归可以通过==给定不同的分位点，拟合目标值的不同分位数==。

例如我们可以分别拟合出多个分位点，得到一个置信区间，如下图所示：

![image-20210319074808360](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319074808.png)

分位数回归是通过使用分位数损失Quantile Loss来实现这一点的，分位数损失形式如下：

![image-20210319074827450](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319074827.png)

式中的r为分位数，这个损失函数是一个分段的函数，将$\hat{y}_i \ge y_i$（高估）和 $\hat{y}_i \le y_i$（低估）时，低估的损失要比高估的损失更大；反之，当$r<0.5$时，高估的损失要比低估的损失更大，==分位数损失实现了分别用不同的系数控制高估和低估的损失==，进而实现分位数回归。

特别地，当$r=0.5$时，分位数损失退化为MAE损失，从这里可以看出 MAE 损失实际上是分位数损失的一个特例: 中位数回归

![image-20210319075015453](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319075015.png)



## 分类

### 5 交叉熵损失Cross Entropy Loss⭐

![image-20210429180601482](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210429180602.png)

对于分类问题，最常用的损失函数是交叉熵损失函数Cross Entropy Loss

#### 5.1 二分类

二分类中我们通常使用Sigmoid函数将模型的输出压缩到(0, 1)区间。

$$\hat{y}_i \in (0, 1)$$，用来代表**给定输入**$x_i$，模型判断为正类的概率。

![image-20210319075539261](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319075539.png)



##### 背景|基于似然函数推导过程

交叉熵函数与极大似然估计的联系：最小化交叉熵函数的本质就是在伯努利分布的条件下，对数似然函数的最大化

随机变量X满足伯努利分布，如果$P(X=1)=p, P(X=0)=1-P$，则X的概率密度函数是：$P(X)=p^X (1-p)^{1-X}$



由于只有正负两类，因此同时也得到了负类的概率：

![image-20210319075340217](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319075340.png)

结果|对于单个数据点(两种分类的情况)：

![image-20210319075416819](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319075416.png)

==假设数据点之间独立同分布==，则似然可以表示为：

![image-20210319075507660](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319075507.png)

对似然取对数，然后加负号变成最小化负对数似然，即为==交叉熵损失函数的形式==：

![image-20210319075539261](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319075539.png)

下图是对二分类的交叉熵损失函数的可视化：

- 基于标签为1，或者标签为0与预测概率  => 得到loss

![image-20210319075601380](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319075619.png)

蓝线是目标值为0时输出不同输出的损失，黄线是目标值为1时的损失。

可以看到约接近目标值损失越小，随着误差变差，损失==呈指数增长==。

#### 5.2 多分类

在多分类的任务中，交叉熵损失函数的推导思路和二分类是一样的，变化的地方是真实值$y_i$是一个One-hot 向量，同时模型输出的压缩由原来的Sigmoid函数换成Softmax函数。



Softmax 函数将每个维度的输出范围都限定在$(0, 1)$之间，同时所有维度的输出和为1，用于表示一个概率分布(每一个k对应一种分类的情况)。其中，$k \in K$表示K个类别中的一类

![image-20210319075722136](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319075722.png)



同样的假设数据点之间独立同分布，可得到负对数似然为：

![image-20210319075959820](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319080000.png)

由于$y_i$是一个One-hot向量，==除了目标类为1之外其他类别上的输出都为 0==，因此上式也可以写为：

![image-20210319080025659](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319080026.png)

其中，$c_i$是$x_i$的目标类，通常这个应用于多分类的交叉熵损失函数也被称为Softmax Loss或者Categorical Cross Entropy Loss





#### 补充|交叉熵损失函数 Cross-entropy loss function

标准形式如下:

![image-20210318171346542](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210318171346.png)

注意公式中x表示样本，y表示实际的标签，a表示预测的输出，n表示样本总数量。

(1)本质上也是一种**对数似然函数**，可用于二分类和多分类任务中。

二分类问题中的loss函数（输入数据是softmax或者sigmoid函数的输出）：

![image-20210318171422049](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210318171422.png)

多分类问题中的loss函数（输入数据是softmax或者sigmoid函数的输出）：

![image-20210318171434008](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210318171434.png)



(2)当使用sigmoid作为激活函数的时候，常用交叉熵损失函数而不用均方误差损失函数，因为它可以完美解决==平方损失函数权重更新过慢==的问题，具有以下良好性质：

- 误差大的时候，权重更新快；
- 误差小的时候，权重更新慢



#### 小结|Logistics loss和Cross Entropy Loss

对于Logistics loss，我们说的是二分类问题，$\hat{y}$是一个数；

对于Cross Entropy Loss，我们说的是多分类问题，$\hat{y}$是一个k维的向量。

当k=2时，Logistics loss与Cross Entropy Loss一致



#### 思考|为什么用交叉熵损失

分类中为什么不用均方差损失？

上文在介绍均方差损失的时候讲到实际上均方差损失假设了误差服从高斯分布，在分类任务下这个假设没办法被满足，因此效果会很差



为什么是交叉熵损失呢？

（1）一个角度是用最大似然来解释：也就是我们上面的推导

（2）另一个角度是用信息论来解释交叉熵损失：假设对于样本$x_i$存在一个最优分布$y_i^*$真实地表明了这个样本属于各个类别的概率，那么我们希望模型的输出$\hat{y}_i$尽可能地逼近这个最优分布。

在==信息论中，我们可以使用KL散度（Kullback–Leibler Divergence）来衡量两个分布的相似性==。

给定分布p和分布q， 两者的 KL 散度公式如下：

![image-20210319081113804](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319081114.png)

其中第一项为分布p的信息熵，第二项为分布p和分布q的交叉熵。

将最优分布$y_i^*$和输出分布$\hat{y}_i$代入分布p和分布q得到：

![image-20210319081224245](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319081224.png)

希望两个分布尽量相近，因此我们最小化KL散度。

同时由于上式第一项信息熵仅与最优分布本身相关，因此我们在最小化的过程中可以忽略掉

但我们并不知道最优分布$y_i^*$，但训练数据里面的目标值$y_i$，可以看做是$y_i^*$的一个近似分布：

![image-20210319081422290](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319081422.png)



这个是针对单个训练样本的损失函数，如果考虑整个数据集，则：

![image-20210319081552151](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319081552.png)

可以看到通过==最小化交叉熵==的角度推导出来的结果和使用==最大化似然==得到的结果是一致的



（3）最后一个角度为BP过程：



当使用平方误差损失函数时：

视角1）

$C=\frac{1}{2n}\sum_x(a-y)^2$

- y是我们期望的输出
- a为神经元的实际输出：$a=\sigma(z), z=wx+b$

![image-20210319143119281](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319143119.png)

因为sigmoid的性质，导致$\sigma'(x)$在z取大部分值时会很小，最终导致导致参数w和b更新非常慢





视角2）

最后一层的误差$\delta^{(l)}=-(y-a^{(l)})f'(z^{(l)})$，最后一项$f'(z^{(l)})$是激活函数的导数

- 当激活函数为Sigmoid函数时，如果$z^{(l)}$的值非常大，函数的梯度趋于饱和，即$f'(z^{(l)})$的绝对值非常小，导致$\delta^{(l)}$的取值也非常小，使得基于梯度的学习速度非常缓慢；

---

当使用交叉熵损失函数时，

视角1）

$C=-\frac{1}{n}\sum_x[y\cdot lna + (1-y)\cdot ln(1-a)]$

- y是我们期望的输出
- a为神经元的实际输出：$a=\sigma(z), z=wx+b$

![image-20210319143523299](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319143523.png)

![image-20210319143427262](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319143427.png)

参数更新公式中没有$\sigma'(x)$这一项，权重的更新受(a-y)影响，受到误差的影响，所以

- 当误差大的时候，权重更新快；
- 当误差小的时候，权重更新慢。

这是一个很好的性质。





视角2）

最后一层的误差为$\delta^{(l)}=f(z_k^{(l)})-1=a_k^{(l)}-1$，此时导数是线性的，因此不存在学习速度过慢的问题。



引入交叉熵损失函数目的是解决一些实例在刚开始训练时学习得非常慢的问题，其主要针对激活函数为Sigmod 函数，如果在输出神经元是S型神经元时，交叉熵一般都是更好的选择

交叉熵无法改善隐藏层中神经元发生的学习缓慢，交叉熵损失函数只对网络输出明显背离预期时发生的学习缓慢有改善效果， 交叉熵损失函数==并不能改善或避免神经元饱和==，而是当输出层神经元发生饱和时，能够避免其学习缓慢的问题。







### 6 合页损失Hinge Loss(SVM 最大化边界距离)

合页损失（Hinge Loss）是另外一种==二分类==损失函数，==适用于 maximum-margin 的分类==

支持向量机Support Vector Machine (SVM)模型的损失函数，本质上就是Hinge Loss + L2正则化。

合页损失的公式如下：

![image-20210319082851421](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319082851.png)

下图是y为正类，即sgn(y)时=1，不同输出的合页损失示意图：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319150553.jpeg" alt="img" style="zoom:80%;" />

y为正类，当模型输出负值时会有较大的惩罚，当模型输出为正值且在(0, 1)区间时还会有一个较小的惩罚。

即合页损失不仅惩罚预测错的，并且对于预测对了但是置信度不高的也会给一个惩罚，只有置信度高的才会有零损失。

使用合页损失直觉上理解是要==找到一个决策边界==，使得所有数据点被这个边界正确地、高置信地被分类



#### 特性

Hinge loss是一个凸函数，所以适合所有的机器学习凸优化方法。

虽然Hinge loss函数不可微，但我们可以求它的分段梯度。

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319150654.png" alt="image-20210319150654556" style="zoom:80%;" />





#### 补充|Hinge 损失函数

标准形式如下：

![image-20210318171153716](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210318171153.png)

(1)hinge损失函数表示如果被分类正确，损失为0，否则损失就为$1-y\cdot f(x)$

​	SVM就是使用这个损失函数。

(2)==一般的f(x)是预测值，在-1到1之间，y是目标值(-1或1)==

​	其含义是， f(x)的值在-1和+1之间就可以了，并不鼓励|f(x)|>1，即并不鼓励分类器过度自信，让某个正确分类的样本距离分割线超过1并不会有任何奖励，从而使分类器可以更专注于整体的误差。

(3) 健壮性相对较高，对异常点、噪声不敏感，但它没太好的概率解释。




#### 补充|感知损失(perceptron loss)函数

标准形式如下：

![image-20210318171312914](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210318171313.png)

(1)是Hinge损失函数的一个变种，Hinge loss对判定边界附近的点(正确端)惩罚力度很高

​	而perceptron loss只要样本的判定类别正确的话，它就满意，不管其判定边界的距离。

​	它比Hinge loss简单，因为==不是max-margin boundary，所以模型的泛化能力没 hinge loss强==。




### 7 0/1损失函数

指预测值和目标值不相等为1， 否则为0：

![image-20210318164642725](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210318164642.png)

特点：

(1)0-1损失函数直接对应分类判断错误的个数，但是它==是一个非凸函数==，不太适用

(2)感知机就是用的这种损失函数

​	但是相等这个条件太过严格，因此可以放宽条件，即满足|Y-f(x)|<T时认为相等

![image-20210318170759832](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210318170800.png)



### 8 指数损失

标准形式：$L(Y|f(X))=e^{-Y \cdot f(X)}$



Adaboost中使用了指数损失函数

李航书中证明了：Adaboost算法是前向分步算法的特例，模型是由基本分类器组成的加法模型，损失函数为指数函数

(1)对离群点、噪声非常敏感。经常用在AdaBoost算法中





### 9 对数损失/对数似然损失Log-likelihood Loss

标准形式如下：

![image-20210318170853248](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210318170853.png)

(1) log对数损失函数能非常好的**表征概率**分布

​	在很多场景尤其是**多分类**，如果需要知道结果属于每个类别的置信度，那它非常适合。

(2)健壮性不强，相比于hinge loss对噪声更敏感。

(3)逻辑回归的损失函数就是log对数损失函数。



### 小结|对数损失 vs 交叉熵损失

![img](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319142104.jpeg)



# #参考文献

[link: 知乎专栏|yyHaker 常见的损失函数(loss function)总结](https://zhuanlan.zhihu.com/p/58883095)