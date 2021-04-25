## 总结|思路脉络⭐

### 1 处理类别数据 => TS 

- target leakage



### 2 TS：Greedy TS => +prior => Holdout TS => Leave-one-out TS => Ordered TS

- 具体原理不明 => 特性：满足P1、P2(相比于在线学习的移动窗)
- 测试结果：Ordered TS效果好
- @me|点评：工程经验



### 3 针对Gradient bias => 针对每一个样本$\bold{x}_k$，单独训练一个模型$M_k$，用来**估计**$\bold{x}_k$的梯度(不直接用样本$\bold{x}_k$)

- @me|点评：也只是工程经验……
- +多个随机排列：增强算法鲁棒性
  - -但是计算开销变大



### 4 针对Prediction shift => Ordered boosting

- 背景：Gradient bias + $\mathbb{D} \backslash \bold{x}_k$ => **基学习器**$h^t$与 $\hat{h}^t$产生偏差 => 最终影响模型$F^t$的泛化能力
- 示例|两个类别特征：两个独立数据集 vs. 相同数据集
  - 结论：数据规模越大，偏差越小 => 所以$M_k$预测梯度 + Ordered boosting更新$M_k$是可行的(==收敛的==)

- 特性：==为减少额外计算开支==
  - 所有的决策树共用同一种结构
  - 从j个样本中(j的范围是i的取值)维护模型$M_i$ (j<i)



### 5 具体实现

​	1）Ordered boosting mode vs. Plain boosting mode

- $s+1$个独立的**随机序列**
  - 序列$\sigma_1,...,\sigma_s$用来评估定义树结构的分裂 => phase1: choosing the tree structures
  - $\sigma_0$用来计算所得到的树的叶子节点的值 => choosing the values in leaves



上述的具体实现中完成了梯度的估计预测：再基于Gradient boosting进行优化

![image-20210419152311963](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210419152312.png)

---

## 背景

Yandex：俄罗斯硅谷

大多数产品使用ML，尤其是Gradient Boosting

- text search
- 推荐系统
- 自动驾驶
- 私人助理



社区：ODS 俄语为主



[Yandex Research](https://resarch.yandex.com)

Areas：

- ML
- CV
- NLP
- Web Mining and Search
- Conputational Economics

会议：NIPS, ICML, CVPR, ACL, KDD, SIGIR,...



### 应用

医疗

工业

金融

**音乐、视频推荐**

销售预测



在很多数据集上，效果比XGBoost, LightGBM, MatrixNet, H2O要好

- Matrix Nets：用于目标检测的深层架构
  - 矩阵网（xNets）使用分层矩阵对具有不同大小和宽高比的目标进行建模
- [H2O](https://zhuanlan.zhihu.com/p/87107981)：AutoML框架 => H2O是一个完全开源的、分布式的、具有线性可扩展性的内存的机器学习平台
  - 支持最广泛使用的统计和机器学习的算法，包括DRF，GBM，XGBoost，DL等
  - 具有模型解释能力
  - H2O.ai是初创公司Oxdata于2014年推出的一个独立开源机器学习平台，它的主要服务对象是数据科学家和数据工程师，主要功能就是为app提供快速的机器学习引擎。



# 基本概念和符号

## 符号标记

==前提假设==|数据集：$\mathcal{D}=\{(x_k, y_k \}_{k=1...n}, x_k \in \mathbb{R}^m, y_k\in \mathbb{R}$

- 样本：$(x_k, y_k)$是来自总体的采样样本，独立同分布
  - 独立：互相不影响
  - 同分布：来自同一个总体



损失函数：$L(y, \hat{y})$

寻找模型：$F=argmin_f \mathcal{L}(f), \mathcal{L}(f):= \mathbb{E}(L(y, f(\bold{x})))$

Gradient boosting：$F^t=F^{t-1}+\alpha \cdot h^t, h^t=argmin_{h \in H} \mathcal{L}(F^{t-1}+h)$

- $\alpha$：学习速率

- H是一组深度有限的决策树



oblivous tree：

反例：

- Age的判断条件：常数不一致

![image-20210419092019959](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210419092020.png)





# 导论

## CatBoost

CatBoost是俄罗斯的搜索巨头Y andex在2017年开源的机器学习库，也是Boosting族算法的一种，同前面介绍过的XGBoost和LightGBM类似，依然是**在GBDT算法框架下的一种改进实现**，是一种基于对称决策树（oblivious trees）算法的参数少、支持类别型变量和高准确性的GBDT框架，

主要解决的痛点是高效合理地处理类别型特征，这个从它的名字就可以看得出来，CatBoost是由catgorical和boost组成，

另外是==处理梯度偏差（Gradient bias）以及预测偏移（Prediction shift）问题==，提高算法的准确性和泛化能力。



![img](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210417170550.jpeg)



CatBoost主要有以下五个特性：

- **无需调参**即可获得较高的模型质量，==采用默认参数就可以获得非常好的结果==，减少在调参上面花的时间
- 支持类别型变量，无需对非数值型特征进行预处理
- **快速、可扩展的GPU版本**，可以用基于GPU的梯度提升算法实现来训练你的模型，支持多卡并行
- 提高准确性，提出一种全新的梯度提升机制来构建模型以减少过拟合
- 快速预测，即便应对延时非常苛刻的任务也能够快速高效部署模型



CatBoost的主要算法原理可以参照以下两篇论文：

- [Anna Veronika Dorogush, Andrey Gulin, Gleb Gusev, Nikita Kazeev, Liudmila Ostroumova Prokhorenkova, Aleksandr Vorobev "Fighting biases with dynamic boosting". arXiv:1706.09516, 2017](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/1706.09516.pdf)
- [Anna Veronika Dorogush, Vasily Ershov, Andrey Gulin "CatBoost: gradient boosting with categorical features support". Workshop on ML Systems at NIPS 2017](https://link.zhihu.com/?target=http%3A//learningsys.org/nips17/assets/papers/paper_11.pdf)



## Categorical features

所谓类别型变量（Categorical features）是指其值是离散的集合且相互比较并无意义的变量，比如用户的ID、产品ID、颜色等。

因此，==这些变量无法在二叉决策树当中直接使用==。

常规的做法是将这些类别变量通过预处理的方式**转化成数值型变量**再喂给模型，比如用一个或者若干个数值来代表一个类别型特征。



目前广泛用于**低势**（一个有限集的元素个数是一个自然数）类别特征的处理方法是`One-hot encoding`：将原来的特征删除，然后对于每一个类别加一个0/1的用来指示是否含有该类别的数值型特征。

`One-hot encoding`可以在数据预处理时完成，也**可以在模型训练的时候完成**，从训练时间的角度，后一种方法的实现更为高效，CatBoost对于低势类别特征也是采用后一种实现。



显然，在**高势**特征当中，比如 `user ID`，这种编码方式会产生大量新的特征，造成维度灾难。 => 分组

一种折中的办法是可以将类别==分组==成有限个的群体再进行 `One-hot encoding`。

一种常被使用的方法是==根据目标变量的统计==（Target Statistics，以下简称TS）进行分组

- ==目标变量统计用于估算每个类别的目标变量期望值。==

- 甚至有人**直接用TS作为一个新的数值型变量**来代替原来的类别型变量。

重要的是，可以通过对TS数值型特征的阈值设置，基于对数损失($-logP(Y|X)$)、基尼系数或者均方差，得到一个对于训练集而言**将类别一分为二**的所有可能划分当中最优的那个。



在LightGBM当中，**类别型特征**用每一步梯度提升时的梯度统计（Gradient Statistics，以下简称GS）来表示。 => 回顾|梯度与$g_i$

虽然为建树提供了重要的信息，但是这种方法有以下两个缺点：

- 增加计算时间，因为需要对每一个类别型特征，在迭代的每一步，都需要对GS进行计算；
- 增加存储需求，对于一个类别型变量，需要存储每一次**分离每个节点的类别**。

为了克服这些缺点，LightGBM以**损失部分信息为代价==将所有的长尾类别归位一类==**，作者声称这样处理高势特征时比起 `One-hot encoding`还是好不少。



不过如果采用TS特征，那么==对于每个类别只需要计算和存储一个数字==。

- 如此看到，采用TS作为一个新的数值型特征是最有效、信息损失最小的处理类别型特征的方法。

TS也被广泛采用，在点击预测任务当中，这个场景当中的类别特征有用户、地区、广告、广告发布者等。

接下来我们着重讨论TS，暂时将`One-hot encoding`和GS放一边。



# 原理|关键概念

## Target statistics⭐

一个有效和高效的处理类别型特征$i$的方式是用一个与某些TS相等的数值型变量$\hat{x}_k^i$来代替第$k$个训练样本的类别${x}_k^i$。

| 符号说明      |                              |
| ------------- | ---------------------------- |
| i             | 表示某个类别特征             |
| k             | 表示第k个训练样本            |
| $\hat{x}_k^i$ | 替代的数值型值(TS：公式如下) |

通常用基于类别的目标变量$y$的期望来进行估算：$\hat{x}_k^i\approx\mathbb{E}(y|x^i={x}_k^i)$。



### Greedy TS

估算$\mathbb{E}(y|x^i={x}_k^i)$**最直接的方式**就是用训练样本当中相同类别$x_k^i$的目标变量$y$的平均值。 

- 注意指示函数I：当满足下标表示的条件时，值是1，否则是0

$$
\hat{x}_k^i=\frac{\sum_{j=1}^n\mathbb{I}_{{x_j^i={x}_k^i}}\cdot y_i}{\sum_{j=1}^n\mathbb{I}_{{x_j^i={x}_k^i}}}
$$

显然，这样的处理方式很容易引起过拟合。

举个例子，假如在整个训练集当中，所有样本的类别${x}_k^i$**都互不相同**(特异)，即$k$个样本有$k$个类别，那么新产生的数值型特征的值将与目标变量的值(也就是y)相同。

- 这是一种目标穿越（target leakage），非常容易引起过拟合。
- target leakage导致conditional shift => $\hat{x}^i|y$在训练集、测试集分布不一致



### Greedy TS + prior

比较好的一种做法是采用一个先验概率$p$进行平滑处理： 

- 其中$a>0$是先验概率$p$的权重
- 对于先验概率p，通常的做法是设置为数据集当中**目标变量的平均值**。

$$
\hat{x}_k^i=\frac{\sum_{j=1}^n\mathbb{I}_{{x_j^i={x}_k^i}}\cdot y_i+a\,p}{\sum_{j=1}^n\mathbb{I}_{{x_j^i={x}_k^i}}+a}
$$



不过这样的平滑处理依然**无法完全避免目标穿越**：特征$\hat{x}_k^i$是通过自变量$X_k$的目标$y_k$计算所得。

这将会==导致条件偏移==：对于训练集和测试集，$\hat{x}_k^i|y$的分布会有所不同。



再举个例子，假设第$i$个特征为类别型特征，并且特征所有取值为无重复的集合，

- 然后对于该特征的某个类别$A$，对于一个分类任务，我们有$P(y=1|x^i=A)=0.5$。
- 然后在训练集当中，$\hat{x}_k^i=\frac{y_k+a\,p}{1+a}$(取值无重复)，于是用阈值$t=\frac{0.5+a\,p}{1+a}$就可以仅用一次分裂就训练集**完美分开**。
- 但是，对于测试集，因为还无法判断此时目标变量的类别，所以这一项$\sum_{j=1}^n\mathbb{I}_{{x_j^i={x}_k^i}}=0$，最后得到的TS值为$p$，
  - 并且得到的模型在$p<t$时，预测目标为0，反之则为1，两种情况下的准确率都为0.5。

![image-20210419093451612](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210419093451.png)





总结一下，我们希望TS有以下的性质： 

$${\rm{P1}\quad}\mathbb{E}(\hat{x}^i|y=v)=\mathbb{E}(\hat{x}_k^i|y_k=v)$$    其中，$(X_k, y_k)$是第$k$个训练样本。

- 等式左边是测试集
- 等式右边是训练集

在我们的例子当中，测试集$\mathbb{E}(\hat{x}_k^i|y_k)=\frac{y_k+a\,p}{1+a}$， 训练集$\mathbb{E}(\hat{x}_k^i|y)=p$，显然无法满足上述条件。



### Holdout TS

留出TS，就是将训练集一分为二：$\mathcal{D}=\hat{\mathcal{D}}_0 \sqcup\hat{\mathcal{D}}_1$，然后根据下式用$\mathcal{D}_k=\hat{\mathcal{D}}_0$来计算TS，并将$\hat{\mathcal{D}}_1$作为训练样本。 

$$
\hat{x}_k^i=\frac{\sum_{X_j \in {\mathcal{D}_k}}\mathbb{I}_{{x_j^i={x}_k^i}}\cdot y_i+a\,p}{\sum_{X_j \in {\mathcal{D}_k}}\mathbb{I}_{{x_j^i={x}_k^i}}+a}
$$
这样处理能够满足同分布的问题，但是却大大减少了训练样本的数量。

- 问题：只用了一半数据 => 方差大



${\rm{P2}\quad} \hat{x}_k^i$低方差



### Leave-one-out TS

初看起来，留一TS（Leave-one-out TS）能够非常好地工作：

- 对于训练样本：$\mathcal{D}_k=\mathcal{D}\backslash \bold{x}_k$
- 对于测试样本：$\mathcal{D}_k=\mathcal{D}$

但事实上，这并没有给预防target leakage带来多少益处。



举个例子，考虑一个**常数**类别型特征：对于所有的样本，$x_k^i=A$。(当类别的target的值都一样时)

在二分类的条件下：

![image-20210419100241779](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210419100241.png)

此时，同样可以用阈值$\hat{x}_k^i=\frac{n^{+}-0.5+a\,p}{n-1+a}$将训练集完美的分类。

1）$\hat{x}_k^i$变小

- 假如$y_k$为0，相比真实值，不变
- 假如$y_k$为1，相比真实值，变小 => $\frac{4}{5}>\frac{3}{4}>\frac{1}{2}$(只要分母大于分子：也就是真分式，那么分子、分母减去同样的值，整体会变小)

2）与P1产生冲突：@me：影响比较小

![image-20210419100402096](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210419100402.png)





### [Ordered TS](https://youtu.be/jLU6kNRiZ5o)⭐

@me|为什么换顺序之后就有效？

- @me|理解为对原始分类特征数据的数值编码：包含了不同类别属性的信息



@me|公式理解

1 样本中：对k进行遍历 => 一个一个样本遍历

2 对某个特定的样本(固定k值)：查找该样本前面(已排序)的样本，加入到TS公式的计算当中



==从在线学习==**按照时间序列**获得样本得到的启发，CatBoost依靠排序原则，采用了一种更为有效的策略。

![image-20210419102352860](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210419102353.png)

主要有以下几个步骤：

- 产生一个随机排列顺序$\sigma$并==对数据集进行编号==
- 对于训练样本：$\mathcal{D}_k={X_j: \sigma(j)<\sigma(k)}$
- 对于测试样本：$\mathcal{D}_k=\mathcal{D}$
- 根据带先验概率的Greedy TS计算$\hat{x}_k^i$

这样计算得到的 Ordered TS能够满足P1，同时也能够使用所有的训练样本。

且比在线学习的划窗（sliding window）处理能够进一步减小$\hat{x}_k^i$的方差。

==需要注意的是，CatBoost在不同的迭代上会采用不同的排列顺序。==

- 在一个迭代中使用多个排序效果不好，会导致bias => 在不同方面，独立地使用



对比ordered TS(base approach)的效果：

- target leakage => 导致prediction shift

![image-20210419102605175](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210419102605.png)





## 特征组合

- 预处理中不需要合并特征

CatBoost的另外一项重要实现是**将不同类别型特征的组合作为新的特征**，以获得高阶依赖（high-order dependencies），

- 比如在广告点击预测当中用户ID与广告话题之间的联合信息，
- 又或者在音乐推荐引用当中，用户ID和音乐流派，
  - 如果有些用户更喜欢摇滚乐，那么将用户ID和音乐流派分别转换为数字特征时，这种用户内在的喜好信息就会丢失。

然而，组合的数量会随着数据集中类别型特征的数量成指数增长，因此在算法中考虑所有组合是不现实的。

为当前树构造新的分割点时，CatBoost会采用**贪婪的策略考虑组合**。

- 对于树的第一次分割，不考虑任何组合。
- 对于下一个分割，CatBoost将当前树的所有组合、类别型特征与**数据集中的所有类别型特征**相结合，并将新的组合类别型特征**动态地转换为数值型特征**。



CatBoost还通过以下方式生成数值型特征和类别型特征的组合：

- 树中选定的所有分割点都被视为**具有两个值的类别型特征**，并像类别型特征一样地被进行组合考虑。



<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210419112144.png" alt="image-20210419112143634" style="zoom:80%;" />





# 原理|关键过程

## Gradient bias => $M_k$专门预测$g^i(\bold{x}_k, y_k)$

CatBoost，和所有标准梯度提升算法一样，都是通过构建新树来拟合当前模型的梯度。

然而，所有经典的提升算法都存在由**有偏的点态梯度估计**引起的过拟合问题。

在每个步骤中使用的梯度都使用**当前模型中的相同的数据点来估计**，这==导致估计梯度在特征空间的任何域中的分布与该域中梯度的真实分布相比发生了偏移==，从而导致过拟合。



为了解决这个问题，CatBoost对经典的梯度提升算法进行了一些改进，简要介绍如下：

在许多利用GBDT框架的算法（例如，XGBoost、LightGBM）中，构建下一棵树分为两个阶段：

- 选择树结构
- 在树结构固定后计算叶子节点的值

为了选择最佳的树结构，算法==通过枚举不同的分割，用这些分割构建树==，对得到的叶子节点中计算值，然后对得到的树计算评分，最后选择最佳的分割。

两个阶段叶子节点的值都是被当做**梯度或牛顿步长的近似值**来计算。



在CatBoost中，第二阶段使用传统的GBDT框架执行，第一阶段使用修改后的版本。



既然原来的梯度估计是有偏的，那么能不能改成无偏估计呢？

设$F^i$为构建$i$棵树后的模型，$g^i(X_k,y_k)$为构建$i$棵树后第$k$个训练样本上面的**梯度值**。

为了使得 $g^i(X_k,y_k)$无偏于模型 $F^i$，我们需要==在没有$X_k$参与的情况下对模型$F^i$进行训练==。

由于我们需要对所有训练样本计算无偏的梯度估计，乍看起来对于$F^i$的训练不能使用任何样本，貌似无法实现的样子。

我们运用下面这个技巧来处理这个问题：

- 对于每一个样本$X_k$，我们==训练一个单独的模型==$M_k$，且该模型从**不使用基于该样本的梯度**估计进行更新。
- 我们使用$M_k$来估计$X_k$上的梯度，并使用这个估计对结果树进行评分。



用伪码描述如下，其中$Loss(y,a)$是需要优化的损失函数，$y$是标签值， $a$是公式计算值。

- @me|用样本i之前的i-1个样本来训练针对样本i的模型$M_i$
  - 这也是一种工程经验：而非严格理论支持的方法…………

![img](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210418214514.jpeg)



值得注意的是${M_{i}}$模型的建立并没有样本${X_{i}}$ 的参与，并且CatBoost中==所有的树${M_{i}}$的共享同样的结构==。

在CatBoost中，我们==生成训练数据集的$s$个随机排列==。

**采用多个随机排列是为了增强算法的鲁棒性**，这在前面的Odered TS当中对于类别型特征的处理有介绍到：

- 针对每一个随机排列，==计算得到其梯度==，为了与Ordered TS保持一致，这里的排列与用于计算Ordered TS时的排列相同。
- 我们==使用不同的排列来训练不同的模型，因此不会导致过拟合==。
- 对于==每个==排列$\sigma$，我们训练$n$个不同的模型${M_{i}}$，如上所示。
  - 这意味着为了构建一棵树，需要对每个排列存储并重新计算，其时间复杂度近似于$O(n^2)$：对于每个模型${M_{i}}$，我们必须更新${M_{1}}({X_{1}}),...,{M_{i}}({X_{i}})$
  - 因此，时间复杂度变成$O(sn^2)$。($s$个随机排列)
  - 当然，在具体实现当中，CatBoost使用了其它的技巧，可以将构建一个树的时间复杂度降低到$O(sn)$。



## [Prediction shift](https://youtu.be/db-iLhQvcH8?t=1014)

预测偏移（Prediction shift）是由上一节所讨论的梯度偏差造成的。

本节希望用数学语言严格地对预测偏差进行描述和分析。



首先来看下梯度提升的整体迭代过程：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210419161736.png" alt="image-20210419161735934" style="zoom: 67%;" />

- 对于梯度提升：$F^t = F^{t-1}+\alpha^t\,h^t,\,h^t=\underset{h \in H}{\mathrm{argmin}}\mathcal{L}(F^{t-1}+h)$
  - $F^{\cdot}$表示模型
  - h：下文中有解释 => 表示基学习器
- $g^t(X,y):=\frac{\partial L(y,s)}{\partial s}|_{s=F^{t-1}(x)}$
- $\hat{h}^t=\underset{h \in H}{\mathrm{argmin}}\mathbb{E}{\left(-g^t(X,y)-h(X)\right)}^2$
- $h^t=\underset{h \in H}{\mathrm{argmin}}\frac{1}{n}\sum_{k=1}^n{\left(-g^t(X_k,y_k)-h(X_k)\right)}^2$
  - 由于无法计算期望(可能是不了解对象的分布)，所以用平均的方式替代



关键过程：在预测梯度$g^t(\bold{x}_k, y_k)$的过程中，基于$\mathbb{D} \backslash {X_k}$，仅仅用$M_k$来预测$\bold{x}_k$的梯度，而非实际梯度

也就是说：训练集与测试集的梯度的期望不一致，从而产生如下的偏移过程。

- 根据$\mathbb{D} \backslash {X_k}$进行随机计算的条件分布$g^t(X_k,y_k)|X_k$与测试集的分布$g^t(X,y)|X$发生偏移
- 基于公式$h^t$ => 这样导致**基学习器**$h^t$与 $\hat{h}^t$产生偏差
- 最后影响模型$F^t$的泛化能力







下面以一个回归任务为例，从理论上分析计算偏差的值。⭐

假设以下边界条件：

- 损失函数：$L(y,\hat{y})=(y-\hat{y})^2$
- 两个相互独立的特征$x^1,x^2$，随机变量，**符合伯努利分布**，先验概率$p=1/2$
- 目标函数：$y=f^*(x)=c_1\,x^1+c_2\,x^2$
- 梯度提升迭代次数为2
- 树深度为1
- 学习率：$\alpha=1$

最后得到的模型为：$F=F^2=h^1+h^2$，其中$h^1,h^2$**分别基于**$x^1$和 $x^2$。



区分数据集是否独立，我们有以下两个推论：

- 如果使用了规模为$n$的**两个独立数据集**$\mathcal{D}_1$和 $\mathcal{D}_2$来分别估算$h^1$ 和$h^2$，则对于任意$x \in \{0,1\}^2$(二维向量)，有：$\mathbb{E}_{\mathcal{D}_1,\mathcal{D}_2}\,F^2(X)=f^*(X)+O(\frac{1}{2^n})$
- 如果使用了**相同的数据集**$\mathcal{D}$来估算$h^1$ 和$h^2$，则有：$\mathbb{E}_{\mathcal{D}_1,\mathcal{D}_2}\,F^2(X)=f^*(X)+O(\frac{1}{2^n})-\frac{1}{n-1}c_2(x^2-\frac{1}{2})$

显然，偏差部分$\frac{1}{n-1}c_2(x^2-\frac{1}{2})$与数据集的规模$n$成反比，==与映射关系$f^*$也有关系==，在我们的例子当中，与$c_2$成正比。

- 数据集越小：偏差越大

- 注意：也跟函数模型有关 => $c_2$





## Ordered boosting|针对Prediction shift

为了克服上一节所描述的预测偏移问题，我们提出了一种新的叫做Ordered boosting的算法。

==假设用$I$棵树来学习一个模型，为了确保$r^{I-1}(X_k,y_k)$无偏，需要确保模型$F^{I-1}$的训练没有用到样本$X_k$。==@me|核心所在



由于我们需要对所有训练样本计算无偏的梯度估计，乍看起来对于$F^{I-1}$的训练不能使用任何样本，貌似无法实现的样子，但是事实上可以通过一些技巧来进行克服，具体的算法在前面已经有所描述，而且是作者较新的论文当中的描述，这里不再赘述。



本节主要讲讲Ordered boosting的具体实现。

==Ordered boosting算法好是好，但是**在大部分的实际任务当中都不具备使用价值**，因为需要训练$n$个不同的模型，大大增加的内存消耗和时间复杂度。==

在CatBoost当中，我们实现了一个基于GBDT框架的修改版本。

前面提到过，在传统的GBDT框架当中，构建下一棵树分为两个阶段：

- ==选择树结构==，
- 在树结构固定后计算叶子节点的值。

CatBoost主要在第一阶段进行优化。



图示|ordered boosting：

- 最后的模型$M_n^T$基于所有的样本

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210419161952.png" alt="image-20210419161951629" style="zoom:67%;" />

![image-20210419105315354](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210419105315.png)

![image-20210419105522261](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210419105522.png)

- n个样本
- I次迭代
- 从j个样本中(j的范围是i的取值)维护模型$M_i$



tricks：

1 所有的决策树共用同一种结构：避免额外对树的维护开支

2 从j个样本中(j的范围是i的取值)维护模型$M_i$ => 减少计算量



## 实现|phase1: choosing the tree structures

在建树的阶段，CatBoost有两种提升模式，

- Ordered：对Ordered boosting算法的优化
- Plain：采用内建的ordered TS对类别型特征**进行转化后的标准GBDT算法**



### 1）Ordered boosting mode

一开始，CatBoost对训练集产生$s+1$个独立的**随机序列**。

序列$\sigma_1,...,\sigma_s$用来评估定义树结构的分裂，$\sigma_0$用来计算所得到的树的叶子节点的值。

因为，在一个给定的序列当中，对于较短的序列，无论是TS的计算还是基于Ordered boosting的预测都会有较大的方差，所以仅仅用一个序列可能引起最终模型的方差，这里我们**会有多个序列**进行学习。

![image-20210419110947814](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210419110947.png)



==CatBoost采用对称树作为基学习器，对称意味着在树的同一层，其分裂标准都是相同的。==

对称树具有平衡、不易过拟合并能够大大减少测试时间的特点。



![image-20210419111516128](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210419111516.png)

建树的具体算法如下伪码描述。

![img](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210418220816.jpeg)

在Ordered boosting模式的学习过程当中，

- 我们维持一个模型$M_{r,j}$，其中$M_{r,j}(i)$表示基于在序列$\sigma_r$当中的前$j$个样本学习得到的模型对于第$i$个样本的预测。

- 在算法的每一次迭代$t$，我们从${\sigma_1,...,\sigma_s}$当中**抽样一个随机序列**$\sigma_r$，并基于此构建第$t$步的学习树$T_t$。

- 然后，基于$M_{r,j}(i)$，计算相应的梯度$grad_{r,j}(i)=\frac{\partial L(y_i,s)}{\partial s}|_{s=M_{r,j}(i)}$。
- 接下来，我们会==用余弦相似度==来近似梯度$G$，其中对于每一个样本$i$，我们取梯度$grad_{r,\sigma(i)-1}(i)$。
  - 在候选分裂评估过程当中，第$i$个样本的叶子节点的值$\delta(i)$由与$i$同属一个叶子的$leaf_r(i)$的所有样本的前$p$个样本的梯度值$grad_{r,\sigma(i)-1}$求平均得到。
  - 需要注意的是，$leaf_r(i)$取决于选定的序列$\sigma_r$，因为$\sigma_r$会影响第$i$个样本的Ordered TS。
  - 当树$T_t$的结构确定以后，我们用它来提升所有的模型$M_{r^{'},j}$

我们需要强调下，一个相同的树结构$T_t$会被用于所有的模型，但是会根据$r^{'}$和 $j$的不同设置不同的叶子节点的值以后应用于不同的模型。



### 2）Plain boosting mode

Plain boosting模式的算法与标准GBDT流程类似，但是**如果出现了类别型特征**，它会基于$\sigma_1,...,\sigma_s$得到的TS维持$s$个支持模型$M_r$。



## 实现|phase2：choosing the values in leaves？？？

当所有的树结构确定以后，最终模型的**叶子节点值的计算**与标准梯度提升过程类似。

第$i$个样本与叶子$leaf_0(i)$进行匹配，我们用$\sigma_0$来计算这里的TS。

![image-20210419110959136](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210419110959.png)



当最终模型$F$在测试期间应用于新的样本，我们采用整个训练集来计算TS。



# 总结|优缺点

特性：基于ordered boosting(permutation-driven)

- 有效针对分类数据
  - 一般用0-1编码
  - Catboost使用TS：$x_k^i$ => $\hat{x}_k^i=\mathbb{E}(y|x^i=x_k^i)$



## 优点

- 能够处理类别特征
- 能够有效防止过拟合
- 模型训练精度高
- 调参时间相对较多



## 缺点

- 对于类别特征的处理==需要大量的内存和时间==
- **不同随机数的设定**对于模型预测结果有一定的影响





## 课程讨论

1 时间开销大？

如果没有类别数据，和lightGBM差不多，

如果有类别数据：

- 1）在算法中自动处理了类别特征
- 2）如果允许特征组合：需要更多时间



2 对稀疏变量

- 基于TS更加快速
- 最差的情况：每个类别的值都是unique特异的



3 工业应用

之前都适用MatrixNet(也是基于GB)

目前Catboost效果越来越好：应用场景也越来越多



4 对连续变量：如何处理缺失值

没什么特殊技巧：一般的标准方法



# #参考文献

- [《机器学习》，周志华](https://link.zhihu.com/?target=https%3A//book.douban.com/subject/26708119/)
- [《统计学习方法》（第二版），李航](https://link.zhihu.com/?target=https%3A//book.douban.com/subject/33437381/)
- [CatBoost Docs](https://link.zhihu.com/?target=https%3A//catboost.ai/)
- [CatBoost](https://link.zhihu.com/?target=https%3A//github.com/catboost/catboost)
- [CatBoost: unbiased boosting with categorical features](https://link.zhihu.com/?target=https%3A//papers.nips.cc/paper/7898-catboost-unbiased-boosting-with-categorical-features.pdf)
- [CatBoost: gradient boosting with categorical features support](https://link.zhihu.com/?target=http%3A//learningsys.org/nips17/assets/papers/paper_11.pdf)
- [CatBoost: Python package training parameters](https://link.zhihu.com/?target=https%3A//catboost.ai/docs/concepts/python-reference_parameters-list.html)



**如何从0开始理解？**

- [link: 视频|原理：课程讲解 40min 2018.05.15](https://youtu.be/db-iLhQvcH8)
- [link: 视频-bilibili|应用：基于python演示 2020.04.21](https://www.bilibili.com/video/av327844968/)
- [link: 视频-youtube|教程 2019.07.18](https://www.youtube.com/watch?v=usdEWSDisS0)
- 官网
  - [link: 官网 tutorials|数据集-IT管理权限](https://catboost.ai/docs/concepts/tutorials.html)
  - [link: 官网 github](https://github.com/catboost/tutorials/blob/master/python_tutorial.ipynb)
- 应用
  - [link: 5分钟讲解|数据集-泰坦尼克号](https://www.youtube.com/watch?v=dvZLk7LxGzc)



## [link: 视频讲解40min](https://www.youtube.com/watch?v=db-iLhQvcH8)

- 9'00 公式 贪心TS
- 9'30'' 基于先验的贪心TS
  - 期望的代替：求和取平均
- 19‘25‘’ proposition
- 21‘30’‘ Ordered Boosting
- 24‘47'' 算法关键点：随机排序 => 求解TS
- 30' 损失函数
  - logloss
  - 0-1loss
- [讲解后的讨论](https://youtu.be/db-iLhQvcH8?t=2120)



[link: 论文](https://arxiv.org/abs/1706.09516)

[link: 简短总结](https://youtu.be/jLU6kNRiZ5o)



# @me|问题梳理

0 @me|直观理解：用TS替代类别特征的潜力？



1 ordered效果有这么好吗？







# @me|总结

## 1 知识点

0 梯度：是上升

- 负梯度：下降



1 特殊情况

- 要么类别的值特异
- 要么类别的target的值都一样



2 不同的数据类型

homogeneous data => 使用神经网络

- image
- sound
- text
- video



heterogeneous data => GBM提供最好的解答

- table data



GPU更适合GB算法



目标函数：

- target是二分类标签：对数损失
- target是概率 => 交叉熵



过拟合：提前终止



==经典例子==：

- 医生：关注FN更少一些(有病一定要检测出来)

- 法院：关注FP少一些



## 2 普遍问题

1 样本中有噪声？

- 0-1编码：适合对称的样本



2 数据量到多少：可以用神经网络？

- 否则用ML
- 更少则用统计推断