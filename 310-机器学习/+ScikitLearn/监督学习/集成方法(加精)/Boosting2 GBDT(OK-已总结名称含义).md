## 总结|思路脉络

@me|可视化

Gradient Boosting Decision Tree：多棵树，基于梯度下降的方式，迭代更新参数$F_{km}(x)$

- k表示类别
- m表示更新的轮次数
- 计算概率的公式仍然是逻辑回归、Softmax回归的公式：只不过参数从线性的$w^T\cdot x + b$变成了$F_{km}(x)$



==重点在于理解算法==：

- Gradient：$\hat{y}_{ik}=y_{ik} - p_k(x_i)$ 迭代得到残差，逼近残差
  - $p_{k,m}(x)=\frac{e^{F_{k,m}(x)}}{\sum_{l=1}^{K}F_{l,m}(x)}$
- Boosting：$F_km(x)=F_{k,m-1}(x)+\eta \cdot \sum \gamma_{jkm}\cdot 1(x \in R_{jkm})$   基于前面分类器进行迭代
- DT：GBDT构建的基础元素 => CART树(侧重回归特性)

---

 理论上，GBM 可以选择各种不同的学习算法作为基学习器，但倾向于选择决策树作为基学习器

- 决策树可以认为是 if-then 规则的集合，==易于理解，可解释性强，预测速度快==。
- 决策树算法相比于其他的算法需要更少的特征工程，
  - 比如可以不用做特征标准化，
  - 可以很好的**处理字段缺失**的数据，
  - 也可以不用关心特征间是否相互依赖等。
  - ==决策树能够自动组合多个特征。==

不过，单独使用决策树算法时，有容易过拟合缺点。

- ==梯度提升方法和决策树学习算法可以互相取长补短，是一对完美的搭档==



抑制单颗决策树的复杂度的方法：

- 限制树的最大深度
- 限制叶子节点的最少样本数量
- 限制节点分裂时的最少样本数量
- 吸收 bagging 的思想对训练样本采样（subsample）@随机森林
- 在学习单颗决策树时只使用一部分训练样本
- 借鉴随机森林的思路在学习单颗决策树时只采样一部分特征
- ==在目标函数中添加正则项==惩罚复杂的树结构等



分类问题|假设总体样本共有K类

- 二分类：逻辑回归
- 多分类：Softmax
  - 概率的参数$F_k(x)$是CART树(选择回归树)
  - 单样本损失函数(交叉熵) => 梯度：样本的真实标签与**预测概率**(概率公式$p_{k,m}(x)=\frac{e^{F_{k,m}(x)}}{\sum_{l=1}^{K}F_{l,m}(x)}$)之差



### 1 在训练的时候，是针对样本x的==每个可能的类==都训练一个分类回归树

- 注意：需提前知道类别的数量

  

### 1' 预测值转化为概率值|基于Softmax

- 得到标签与预测概率的差 => 也就是上述单样本损失函数的梯度

- 根据概率值：训练一个分类回归树，每棵树的训练过程其实就是CART树的生成过程 => 生成$F_1(x),...,F_k(x)$
  - 寻找回归树的最佳划分节点 => 直到生成树结束
  - 给这棵树的==每个叶子节点==分别赋一个参数$\gamma_{jkm}$（也就是我们下述`累加公式`中提到的c），来拟合残差
  - ![image-20210416111307696](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210416111307.png)
  - j表示树的分叉数量(也就是节点的度)
- 更新$F_{km}(x_i)$
  - ![image-20210413145902458](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210413145902.png)
  - k是样本的某个类别
  - m是迭代轮数
  - $x_i$是某个样本





### 2 第二轮训练：==target更新为残差值==@GB的核心 => 继续训练出三棵树



### 3 持续迭代M轮训练

- 每轮构建3棵树



### 4 最终：累加 => 直到接近目标的真实Label@GB的核心



### 5 应用|预测

训练完以后，新来一个样本$x_1$，我们要预测该样本类别的时候，便可以由这三个式子产生三个值$F_{1M}, F_{2M}, F_{3M}$



最终样本属于某个类别的概率为：

![image-20210413144827224](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210413144827.png)

---

[link: 原文链接](https://mp.weixin.qq.com/s/KpilI3s4rUwLSuUz3XcTQg) 2020.02.12

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210412203617.png)



## GBM + DT => GBDT

基于梯度提升算法的学习器叫做 GBM(Gradient Boosting Machine)。

理论上，GBM 可以选择各种不同的学习算法作为基学习器。

GBDT 实际上是 GBM 的一种情况。



为什么梯度提升方法倾向于选择决策树作为基学习器呢？(也就是 GB 为什么要和 DT 结合，形成 GBDT) 

- 决策树可以认为是 if-then 规则的集合，==易于理解，可解释性强，预测速度快==。
- 决策树算法相比于其他的算法需要更少的特征工程，
  - 比如可以不用做特征标准化，
  - 可以很好的**处理字段缺失**的数据，
  - 也可以不用关心特征间是否相互依赖等。
  - ==决策树能够自动组合多个特征。==



不过，单独使用决策树算法时，有容易过拟合缺点。

所幸的是，通过各种方法，抑制决策树的复杂性，降低单颗决策树的拟合能力，再通过梯度提升的方法集成多个决策树，最终能够很好的解决过拟合的问题。



=> 由此可见，==梯度提升方法和决策树学习算法可以互相取长补短，是一对完美的搭档==



抑制单颗决策树的复杂度的方法：

- 限制树的最大深度
- 限制叶子节点的最少样本数量
- 限制节点分裂时的最少样本数量
- 吸收 bagging 的思想对训练样本采样（subsample）
- 在学习单颗决策树时只使用一部分训练样本
- 借鉴随机森林的思路在学习单颗决策树时只采样一部分特征
- ==在目标函数中添加正则项==惩罚复杂的树结构等





## 示例|二分类问题: 基于回归决策树

二分类问题，1 表示可以考虑的相亲对象，0 表示不考虑的相亲对象。

特征维度有 3 个维度，分别对象 身高，金钱，颜值

![img](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210415214730.jpeg)

对应这个例子，训练结果是 perfect 的，全部正确， 特征权重可以看出，对应这个例子训练结果颜值的重要度最大，看一下训练得到的树。

Tree 0：

![img](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210415214747.jpeg)

Tree 1：

![img](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210415214835.jpeg)





# 1. GBDT多分类算法

## 1.1 Softmax回归的对数损失函数

当使用逻辑回归处理多标签的分类问题时，如果一个样本只对应于一个标签，我们可以==假设每个样本属于不同标签的概率服从于几何分布==，使用多项**逻辑回归**（Softmax Regression）来进行分类：

- 几何分布：第一次击中目标

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210416084151.gif" alt="img" style="zoom:67%;" />

根据交叉熵最大：选择分类对应的预测

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210416084250.jpeg" alt="img" style="zoom: 67%;" />





在二分类的逻辑回归中，对输入样本x==分类结果为类别1和0==的概率可以写成下列形式：

- x表示样本输入，$\theta$表示参数(比如样本分布参数等)
- $h_{\theta}(x)=\frac{1}{1+e^{-\theta^Tx}}$是模型预测的概率值
- y是样本对应的类标签

![image-20210412211159258](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210412211159.png)

将问题==泛化==为更一般的多分类情况：

![image-20210412211330329](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210412211330.png)

由于**连乘可能导致最终结果接近**的问题，一般对似然函数取对数的负数，变成**最小化对数似然函数**。

![image-20210412211442813](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210412211443.png)



> **补充：交叉熵**
> 假设p和q是关于样本集的两个分布，其中p是样本集的真实分布，q是样本集的估计分布，那么按照真实分布p来衡量识别一个样本所需要编码长度的期望（即平均编码长度）：
>
> ![image-20210412211521012](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210412211521.png)
>
> 如果用==估计分布q==来表示真实分布p的平均编码长度，应为：
>
> ![image-20210412211544039](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210412211544.png)
>
> 这是因为==用q来编码 来自于真实分布p的样本==，所以期望值H(p, q)中的概率是$p_i$。
>
> 而H(p, q)就是交叉熵。
> 可以看出，在多分类问题中，通过**最大似然估计**得到的对数似然损失函数(上述推导的公式)与通过**交叉熵**得到的交叉熵损失函数在形式上相同。



# 1.2 GBDT多分类原理⭐

将GBDT应用于二分类问题需要考虑**逻辑回归模型**，同理，对于GBDT多分类问题则需要考虑以下Softmax模型：

![image-20210412211645144](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210412211645.png)



其中$F_1...F_k$是==k个不同的CART(分类&回归树)回归树集成==。

每一轮的训练实际上是==训练了k棵树去拟合softmax的每一个分支模型的**负梯度**==。



softmax模型的**==单样本==损失函数**为：

- 这里的$y_i(i=1...k)$是样本label在k个类别上作one-hot编码之后的取值，==只有一维为1，其余都是0==。

![image-20210412211755923](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210412211756.png)

@me|由以上表达式不难推导：

- $F_i$表示在该维度为1的样本(其余均为0)
- 按照偏导数：展开得到下述结果
  - ln(...)
  - 分式求偏导

![image-20210412211835071](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210412211835.png)

从最后的结果可见，这k棵树同样是拟合了样本的真实标签与**预测概率**之差，与GBDT二分类的过程非常类似。

下图是Friedman在论文中对GBDT多分类给出的伪代码：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210413134452.webp" alt="图片" style="zoom:80%;" />

根据上面的伪代码具体到多分类这个任务上面来，我们假设总体样本共有K类。

来了一个样本x，我们需要使用GBDT来判断x属于样本的哪一类。

---

**1）第一步我们在训练的时候，是针对样本x的==每个可能的类==都训练一个分类回归树。** 

举例说明，目前样本有三类，也就是K=3，样本x属于第二类。

- 那么针对该样本的分类标签，其实可以用一个三维向量[0, 1, 0]来表示。0表示样本不属于该类，1表示样本属于该类。
- 由于样本已经属于第二类了，所以第二类对应的向量维度为1，其它位置为0。



**针对样本有三类的情况，我们实质上在每轮训练的时候是同时训练三棵树。** 

- 第一棵树针对样本x的第一类，输入(input, label)为(x, 0)。
- 第二棵树输入针对样本x的第二类，输入(input, label)为(x, 1)。
- 第三棵树针对样本x的第三类，输入(input, label)为(x, 0)。

这里每棵树的训练过程其实就是CART树的生成过程。

在此我们参照CART生成树的步骤即可解出三棵树，以及三棵树对x类别的预测值$F_1(x), F_2(x), F_3(x)$



**1'）预测值转化为概率值|基于Softmax**

那么在此类训练中，我们仿照多分类的逻辑回归 ，==使用Softmax 来产生概率==，则属于类别1的概率为：

![image-20210413135104863](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210413135105.png)

并且我们可以：

- 针对类别1求出残差$\hat{y}_1=0-p_1(x)$；
- 类别2求出残差$\hat{y}_2=1-p_2(x)$；
- 类别3求出残差$\hat{y}_3=0-p_3(x)$。



**2）第二轮训练：target更新为残差值**

然后开始第二轮训练，针对第一类输入为$(x, \hat{y}_1)$, 针对第二类输入为$(x, \hat{y}_2)$，针对第三类输入为$(x, \hat{y}_3)$。

继续训练出三棵树。



**3）持续迭代M次**

==一直迭代M轮==。

每轮构建3棵树。



当类别总数K=3时，我们其实应该有三个式子：

- 持续累加：直到接近目标的真实Label(在迭代的过程中：残差绝对值不断减小)

![image-20210413144739529](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210413144739.png)

当训练完以后，新来一个样本$x_1$，我们要预测该样本类别的时候，便可以由这三个式子产生三个值$F_{1M}, F_{2M}, F_{3M}$。

样本属于某个类别的概率为：

![image-20210413144827224](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210413144827.png)



# 2. 实例

## （1）数据集

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)![image-20210413145038396](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210413145043.png)



## （2）模型训练阶段

首先，由于我们需要转化个二分类的问题，所以需要先做一步one-hot：

- $y_i$经过one-hot变成$y_{i, k}, (k=0...2)$

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210413145055.png)

> 参数设置：
>
> - 学习率：learning_rate = 1
> - 树的深度：max_depth = 2
> - 迭代次数：n_trees = 5



首先对所有的样本，进行初始化$F_{k0}(x_i)=\frac{count(k)}{count(n)}$，就是各类别在总样本集中的占比，结果如下表。

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210413145143.png)

**注意：** 在Friedman论文里全部初始化为0，但在sklearn里是初始化==**先验概率（就是各类别的占比）**==，这里我们用sklearn中的方法进行初始化。



### 1）对第一个类别($y_i=0$)拟合第一棵树(m=1)⭐

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210413145217.webp)

**首先**，利用公式$p_{k,m}(x)=\frac{e^{F_{k,m}(x)}}{\sum_{l=1}^{K}F_{l,m}(x)}$计算概率。

- $p_{0, 0}=0.3412$

**其次**，==计算负梯度值==，以$x_1$为例$(k=0, i=1)$：

![image-20210413145403197](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210413145403.png)

同样地，计算其它样本可以有下表：

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210413145508.png)



**接着**，==寻找回归树的最佳划分节点==。

在GBDT的建树中，可以采用如MSE、MAE等作为分裂准则来确定分裂点。

本文采用的分裂准则是`MSE`，具体计算过程如下。

- ==遍历所有特征的取值(表中$x_i$的值)，将每个特征值依次作为分裂点==，然后计算左子结点与右子结点上的MSE，寻找两者加和最小的一个。

比如，选择$x_8=1$作为分裂点时(x<1为左结点)

- 左子结点上的集合的MSE为：$MSE_{left}=0$

- 右子节点上的集合的MSE为：

![image-20210413145603745](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210413145603.png)

比如选择$x_9=2$作为分裂点时(x<2)。

![image-20210413145649510](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210413145649.png)



对所有特征计算完后可以发现，当选择$x_6=31$做为分裂点时，可以得到最小的MSE=1.42857。

下图展示==以$x_6=31$为分裂点的$\hat{y}_{i0}$拟合一颗回归树==的示意图：

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210413145809.webp)

**然后**，我们的树满足了设置，还需要做一件事情，给这棵树的==每个叶子节点==分别赋一个参数$\gamma_{jkm}$（也就是我们上面公式中提到的c），来拟合残差。

@me|如何选择参数？⭐@上述算法

![image-20210416111307696](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210416111307.png)

| $x_i$            | 6      | 12     | 14     | 18     | 20     | 65      | ==31==  | 40      | 1       | 2       | 100     | 101     | 65      | 54      |
| ---------------- | ------ | ------ | ------ | ------ | ------ | ------- | ------- | ------- | ------- | ------- | ------- | ------- | ------- | ------- |
| $\hat{y}_{i, 0}$ | 0.6588 | 0.6588 | 0.6588 | 0.6588 | 0.6588 | -0.3412 | -0.3412 | -0.3412 | -0.3412 | -0.3412 | -0.3412 | -0.3412 | -0.3412 | -0.3412 |
| 子树{$R_{jkm}$}  | Left   | Left   | Left   | Left   | Left   |         |         |         | Left    | Left    |         |         |         |         |

根据算法中$\gamma_{jkm}$的计算公式：
$$
\gamma_{101}=\frac{2}{3}\cdot \frac{0.6588*5+(-0.3412)*2}{0.6588*(1-0.6588)*5 + 0.3412*(1-0.3412)*2}=1.1066
$$


![image-20210413145840149](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210413145840.png)



**最后**，==更新$F_{km}(x_i)$==可得下表：

- I()表示数量统计

![image-20210413145902458](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210413145902.png)

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210413145914.webp)

至此第一个类别（类别）的第一棵树拟合完毕，下面开始拟合第二个类别（类别）的第一棵树。



### 2）对第二个类别($y_i=1$)拟合第一棵树(m=1)

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210413145947.webp)

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210413150025.png" alt="image-20210413150024892" style="zoom:80%;" />



至此第二个类别（类别1）的第一棵树拟合完毕。

然后再拟合第三个类别（类别2）的第一棵树，过程也是重复上述步骤，所以这里就不再重复了。



### 3）迭代：多轮拟合M棵树

在拟合完==所有类别的第一棵树==后就开始拟合第二棵树，反复进行，直到训练了M轮。

- 第m-1棵树：用参数m来做标记
  - 第二棵树(m-1=1)是在第一棵树(m-1=0)的基础之上





# 3. 手撕代码

本篇文章所有数据集和代码均在我的GitHub中，地址：https://github.com/Microstrong0305/WeChat-zhihu-csdnblog-code/tree/master/Ensemble%20Learning/GBDT_Multi-class



## 3.1 用Python3实现GBDT多分类算法

**需要的Python库：**

```
pandas、PIL、pydotplus、matplotlib
```

其中`pydotplus`库会自动调用`Graphviz`，所以需要去`Graphviz`官网下载`graphviz-2.38.msi`安装，再将安装目录下的`bin`添加到系统环境变量，最后重启计算机。

由于用`Python3`实现`GBDT`多分类算法代码量比较多，我这里就不列出详细代码了，感兴趣的同学可以去我的`GitHub`中看一下，地址：https://github.com/Microstrong0305/WeChat-zhihu-csdnblog-code/tree/master/Ensemble%20Learning/GBDT_Multi-class/GBDT_GradientBoostingMultiClassifier

## 3.2 用sklearn实现GBDT多分类算法

```python
import numpy as np
from sklearn.ensemble import GradientBoostingClassifier

'''
调参：
- loss：损失函数。有deviance和exponential两种。
	deviance是采用对数似然，exponential是指数损失，后者相当于AdaBoost。

- n_estimators:最大弱学习器个数，默认是100
	调参时要注意过拟合或欠拟合，一般和learning_rate一起考虑。

- learning_rate:步长，即每个弱学习器的权重缩减系数，默认为0.1，取值范围0-1，当取值为1时，相当于权重不缩减。较小的learning_rate相当于更多的迭代次数。

- subsample:子采样，默认为1，取值范围(0,1]，当取值为1时，相当于没有采样。
	小于1时，即进行采样，按比例采样得到的样本去构建弱学习器。这样做可以防止过拟合，但是值不能太低，会造成高方差。

- init：初始化弱学习器。不使用的话就是第一轮迭代构建的弱学习器.如果没有先验的话就可以不用管
	由于GBDT使用CART回归决策树。以下参数用于调优弱学习器，主要都是为了防止过拟合

- max_feature：树分裂时考虑的最大特征数，默认为None，也就是考虑所有特征。
	可以取值有：log2,auto,sqrt

- max_depth：CART最大深度，默认为None

- min_sample_split：划分节点时需要保留的样本数。
	当某节点的样本数小于某个值时，就当做叶子节点，不允许再分裂。默认是2

- min_sample_leaf：叶子节点最少样本数。
	如果某个叶子节点数量少于某个值，会同它的兄弟节点一起被剪枝。默认是1

- min_weight_fraction_leaf：叶子节点最小的样本权重和。
	如果小于某个值，会同它的兄弟节点一起被剪枝。一般用于权重变化的样本。默认是0
	
- max_leaf_nodes：最大叶子节点数
'''

gbdt = GradientBoostingClassifier(loss='deviance', learning_rate=1, n_estimators=5, subsample=1
                              	  , min_samples_split=2, min_samples_leaf=1, max_depth=2
                                  , init=None, random_state=None, max_features=None
                                  , verbose=0, max_leaf_nodes=None, warm_start=False
                                  )

train_feat = np.array([[6],
                       [12],
                       [14],
                       [18],
                       [20],
                       [65],
                       [31],
                       [40],
                       [1],
                       [2],
                       [100],
                       [101],
                       [65],
                       [54],
                       ])
train_label = np.array([[0], [0], [0], [0], [0], [1], [1], [1], [1], [1], [2], [2], [2], [2]]).ravel()

test_feat = np.array([[25]])
test_label = np.array([[0]])
print(train_feat.shape, train_label.shape, test_feat.shape, test_label.shape)

gbdt.fit(train_feat, train_label)
pred = gbdt.predict(test_feat)
print(pred, test_label)
```

用`sklearn`实现`GBDT`多分类算法的`GitHub`地址：https://github.com/Microstrong0305/WeChat-zhihu-csdnblog-code/blob/master/Ensemble%20Learning/GBDT_Multi-class/GBDT_multiclass_sklearn.py



# 4. 总结

在本文中，

- 我们首先从Softmax回归引出GBDT的多分类算法原理；

- 其次用实例来讲解GBDT的多分类算法；
- 然后不仅用Python3实现GBDT多分类算法，还用sklearn实现GBDT多分类算法；
- 最后简单的对本文做了一个总结。



至此，GBDT用于解决回归任务、二分类任务和多分类任务就完整的深入理解了一遍。



## 小结

1 [link: 博客|CSDN](https://blog.csdn.net/songyunli1111/article/details/83688360)

GBDT是由多棵树组成的，而且每一颗树都依赖于之前树建立后的残差，因此它的建树过程不是并行的，而是串行的，所以速度较慢。

- 这种个体学习器之间存在强依赖关系、必须串行生成的序列化方法，就是boosting方法，

- 而个体学习器间不存在强依赖关系、可同时生成的并行化方法，就是bagging，如随机森林。



但对于GBDT，所有的树一旦建好，用它来预测时是并行的，最终的预测值就是所有树的预测值之和。

对于随机森林，它的预测也是并行的，但最终的预测值是所有树预测值的平均。(注：这是对于回归问题)




# 5. Reference

【1】Friedman J H. Greedy function approximation: a gradient boosting machine[J]. Annals of statistics, 2001: 1189-1232.
【2】《推荐系统算法实践》，黄美灵著。
【3】《百面机器学习》，诸葛越主编、葫芦娃著。
【4】Softmax函数与交叉熵，地址：https://blog.csdn.net/behamcheung/article/details/71911133#%E5%AF%B9%E6%95%B0%E4%BC%BC%E7%84%B6%E5%87%BD%E6%95%B0
【5】图示Softmax及交叉熵损失函数，地址：https://blog.csdn.net/Hearthougan/article/details/82706834
【6】GBDT详细讲解&常考面试题要点，地址：https://mp.weixin.qq.com/s/M2PwsrAnI1S9SxSB1guHdg
【7】机器学习算法GBDT的面试要点总结-上篇，地址：https://www.cnblogs.com/ModifyRong/p/7744987.html
【8】Gradient Boosting Decision Tree，地址：http://gitlinux.net/2019-06-11-gbdt-gradient-boosting-decision-tree/
【9】GBDT算法用于分类问题 - hunter7z的文章 - 知乎，地址：https://zhuanlan.zhihu.com/p/46445201
【10】GBDT原理与实践-多分类篇，地址：https://blog.csdn.net/qq_22238533/article/details/79199605
【11】代码实战之GBDT，地址：https://louisscorpio.github.io/2018/01/19/%E4%BB%A3%E7%A0%81%E5%AE%9E%E6%88%98%E4%B9%8BGBDT/
【12】Freemanzxp/GBDT_Simple_Tutorial，地址：https://github.com/Freemanzxp/GBDT_Simple_Tutorial



# [补充|通俗理解](https://www.bilibili.com/video/BV1U5411n7vH)

与随机森林的区别：

1）GBDT中的树都是回归树

- 分类树：单纯将苹果区分为好，坏
- 回归树：能给苹果的好坏程度打分

2）GBDT的每棵树都在前一棵树的基础上

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210617091203.png" alt="image-20210617091202685" style="zoom: 67%;" />



应用：网页，商品，电影的**评分**

- 预测关联程度，点击率，或者用户的喜好程度

=> 搜索，广告，推荐系统



优点：

- 处理标签，数值等各类数据
- 解释性强

缺点：

- 树与树之间是依赖的：需要较长的训练时间

