集成；**集思广益，博采众长**

AdaBoost：Adaptive Boosting自适应提升算法

- 由 Freund 等人于 1995 年提出，是对 Boosting 算法的一种实现

[link: 公众号|原文链接](https://mp.weixin.qq.com/s?__biz=MzIwODI2NDkxNQ==&mid=2247487921&idx=2&sn=2551bd0be849a2988963bdd791836e5d&chksm=97049a0da073131bfa919a0cab3176382af442b4e62a6b4118b873aaab8900a780ec8f8f86c2&scene=21#wechat_redirect)



## 背景

为了详细的理解这些原理，曾经看过西瓜书，统计学习方法，机器学习实战等书，也听过一些机器学习的课程，但总感觉话语里比较深奥，读起来没有耐心，并且==**理论到处有，而实战最重要**==， 所以在这里想用最浅显易懂的语言写一个**白话机器学习算法理论+实战系列**。



个人认为，==理解算法背后的idea==和使用，要比看懂它的数学推导更加重要。

- idea会让你有一个直观的感受，从而明白算法的合理性，数学推导只是将这种合理性用更加严谨的语言表达出来而已，打个比方，一个梨很甜，用数学的语言可以表述为糖分含量90%，但只有亲自咬一口，你才能真正感觉到这个梨有多甜，也才能真正理解数学上的90%的糖分究竟是怎么样的。
- 如果算法是个梨，本文的首要目的就是先带领大家咬一口。



**学习算法的过程，获得的不应该只有算法理论，还应该有乐趣和解决实际问题的能力！**

现在一般都是用集成算法了，这个是很强大的，就比如现在很火的xgboost，lightgbm等等。

学完了这个，你就会发现什么决策树，KNN的都是弟弟了。



==AdaBoost现在用的也不多了==，但是为啥还要学？

因为这些都是基础，现在传统的机器学习算法也不是那么常用了，我们还是得学，学习不要抱有功利之心，有了这些基础，我们才能更快的去掌握更加高级的算法，像xgboost， lightgbm，catboost等。

如果连决策树都不知道是啥，那还怎么学这些知识。 

**所谓万变不离其宗，就是这个道理了，学，重在学思维方式，重在以不变应万变**。



## 工作原理|三个臭皮匠，顶个诸葛亮

> 小学语文课本一篇名为《三个臭皮匠顶个诸葛亮》的文章。
>
> 文章中写到诸葛亮带兵过江，江水湍急，而且里面多是突出水面的礁石。
>
> 普通竹筏和船只很难过去,打头阵的船只都被水冲走触礁沉没，诸葛亮一筹莫展，也想不出好办法，入夜来了3个做牛皮活的皮匠献策。告诉诸葛亮买牛，然后把牛从肚皮下整张剥下来，封好切口后让士兵往里吹气，做成牛皮筏子，这样的筏子不怕撞，诸葛亮按此方法尝试并顺利过江.



> 现在这个社会，单靠一个人的力量是很难成事的， 现在干什么不都是讲究团队化了吗？同样是写一个软件，如果一个团队来做，即使人员都是学生，现学，但如果每个人有不同的分工，负责自己的那块，相信很快也能搞定这个任务，但如果是一个人来写，即使是个大牛，也累的要死，所以团队合作化给我们带来的不仅是效率，还有时间，而时间，就是金钱呐。



集成算法通常用两种：投票选举（bagging）和再学习（boosting）。

- 投票选举的场景类似把专家召集到一个会议桌前，当做一个决定的时候，让 K 个专家（K 个模型）分别进行分类（做出决定），然后选择**出现次数最多的那个类**（决定）作为最终的分类结果。（听说过伟大的随机森林吧，就是==训练很多棵树，少数服从多数==）
  - 而 bagging 在做投票选举的时候可以并行计算，也就是 K 个“专家”在做判断的时候是==相互独立的，不存在依赖性==。
- 再学习相当于把 K 个专家（K 个分类器）进行==加权融合==，形成一个新的超级专家（强分类器），让这个超级专家做判断。（而伟大的AdaBoost就是这种方式）
  - Boosting 的含义是提升，它的作用是每一次训练的时候都对上一次的训练进行改进提升，在训练的过程中这 K 个“专家”之间是有依赖性的，当引入第 K 个“专家”（第 K 个分类器）的时候，实际上是对前 K-1 个专家的优化。



## Boosting

Boosting 算法是集成算法中的一种，同时也是一类算法的总称。

这类算法通过训练多个弱分类器，将它们组合成一个强分类器。

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210413155650.png" alt="image-20210413155650230" style="zoom:80%;" />

### **1 分类器组合|调整权重**

假设弱分类器为 Gi(x)，它在强分类器中的权重 αi，那么就可以得出强分类器 f(x)：

- $f(x)=\sum_{i=1}^{n}\alpha_i G_i(x)$



### **2 分类器的权重更新|基于分类错误率**

那么这里就有两个问题：

1. 如何得到这些弱分类器（士兵），也就是在每次迭代训练的过程中，如何得到最优的弱分类器（士兵）？
2. 每个弱分类器（士兵）的权重是如何计算的？
   - 基于这个弱分类器对样本的分类错误率来决定它的权重
   - $\alpha_i = \frac{1}{2} log \frac{1-e_i}{e_i}$：$e_i$代表第 i 个分类器的分类错误率
     - 这个公式可以保证，分类器的分类错误率越高，相应的权重就越大。具体的公式推导在后文



回到第一个问题：如何在每次训练迭代的过程中选择最优的弱分类器？

> Adaboost是通过**改变样本的数据分布**来实现的，AdaBoost 会判断每次训练的样本是否正确分类，对于正确分类的样本，降低它的权重，==对于被错误分类的样本，增加它的权重==。
>
> 再基于上一次得到的分类准确率，来确定这次训练样本中每个样本的权重。
>
> 然后将修改过权重的新数据集传递给下一层的分类器进行训练。
>
> 这样做的好处就是，通过每一轮训练样本的动态权重，可以==让训练的焦点集中到难分类的样本上==，最终得到的弱分类器的组合更容易得到更高的分类准确率。

- 训练样本在开始的时候啊，都会有一个概率分布，也就是权重。比如n个样本，我假设每个样本的权重都是1/n，意味着同等重要
- 但是我们训练出一个分类器A之后，如果这个分类器A能把之前的样本正确的分类，就说明这些正确分类的样本由A来搞定就可以了。我们下一轮训练分类器B的时候就不需要太多的关注了，让B更多的去关注A分类错误的样本
  - **把A分类正确的样本的权重减小，分类错误的样本的权重增大**。这样，B在训练的时候，就能更加的关注这些错误样本了，因为一旦把这些样本分类错误，损失就会腾腾的涨（权重大呀），为了使损失降低，B就尽可能的分类出这些A没有分出的样本，问题解决
- 那如果训练出来的B已经很好了，误差很小了，仍然有分不出来的怎么办？**那同样的道理，把这些的权重增大，交给下一轮的C**。==每一轮的分类器各有专长的。==



### **3 权重的更新|迭代公式**

用 $D_{k+1}$ 代表第 k+1 轮训练中，样本的权重集合

- 其中$W_{k+1, 1}$代表第 k+1 轮中第一个样本的权重，
- 以此类推$W_{k+1, N}$代表第 k+1 轮中第 N 个样本的权重，

因此用公式表示为：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210413160905.png" alt="image-20210413160904823" style="zoom:80%;" />

第 k+1 轮中的样本权重，是根据该样本在第 k 轮的权重以及第 k 个分类器的准确率而定，具体的公式为：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210413160927.png" alt="image-20210413160926989" style="zoom:80%;" />

看到这个公式估计又懵逼，还是那句话，先不用管公式，只需要知道：

- 这个公式保证的就是，如果当前分类器把样本分类错误了，那么样本的w就会变大
- ==如果分类正确了，w就会减小。==
- 这里的$Z_k$是归一化系数。就是$∑ (w_{k,i} \cdot e^{-α_k \cdot y_i \cdot G_k(xi)})$



# 实例

假设有10个训练样本：

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210413161132.png)

想通过AdaBoost构建一个强分类器（诸葛亮出来），怎么做呢？模拟一下：

1. **首先**，我得先给这10个样本划分重要程度，也就是权重，由于是一开始，那就平等，都是1/10。

即初始权重D1=(0.1, 0.1, 0.1, 0.1, 0.1, 0.1, 0.1, 0.1, 0.1, 0.1)。假设我训练的3个基础分类器如下：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210413161218.png" alt="image-20210413161217800" style="zoom: 50%;" />

这个是一次迭代训练一个，这里为了解释这个过程，先有这三个。



2. 然后，我们**进行第一轮的训练**

分类器 f1 的错误率为 0.3，也就是 x 取值 6、7、8 时分类错误；
分类器 f2 的错误率为 0.4，即 x 取值 0、1、2、9 时分类错误；
分类器 f3 的错误率为 0.3，即 x 取值为 3、4、5 时分类错误。



根据误差率最小，我训练出一个分类器来如下(选择f1)：

- 这个分类器的错误率是0.3（x取值6,  7，8的时候分类错误），是误差率最低的了（怎么训练的？可以用一个决策树训练就可以啊）， 即e1 = 0.3

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210413161305.png" alt="image-20210413161305746" style="zoom:50%;" />



根据权重公式得到第一个弱分类器的权重：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210413161334.png" alt="image-20210413161334316" style="zoom:50%;" />![image-20210413161347334](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210413161347.png)

然后，我们就得根据这个分类器，来更新我们的训练样本的权重了

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210413161347.png" alt="image-20210413161347334" style="zoom:50%;" />

根据这个公式，就可以计算权重矩阵为：

D2=(0.0715, 0.0715, 0.0715, 0.0715, 0.0715, 0.0715, 0.1666, 0.1666, 0.1666, 0.0715)。



你会发现，6, 7, 8样本的权重变大了，其他的权重变小（这就意味着，下一个分类器训练的时候，重点关注6, 7, 8这三个样本）





3. **进行第二轮的训练**，继续统计三个分类器的准确率，可以得到：

分类器 f1 的错误率为 0.1666 * 3，也就是 x 取值为 6、7、8 时分类错误。

分类器 f2 的错误率为 0.0715 * 4，即 x 取值为 0、1、2、9 时分类错误。

分类器 f3 的错误率为 0.0715 * 3，即 x 取值 3、4、5时分类错误。



在这 3 个分类器中，f3 分类器的错误率最低，因此我们选择 f3 作为第二轮训练的最优分类器，即：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210413161515.png" alt="image-20210413161515046" style="zoom:50%;" />

根据分类器权重公式得到：$\alpha_2 = 0.6496$

同样，我们对下一轮的样本更新求权重值：$w_{k+1, i}=...$

可以得到 D3=(0.0455,0.0455,0.0455,0.1667, 0.1667,0.01667,0.1060, 0.1060, 0.1060, 0.0455)。



你会发现， G2分类错误的3，4， 5这三个样本的权重变大了，说明下一轮的分类器重点在上三个样本上面。





4. 接下来**我们开始第三轮的训练**, 我们继续统计三个分类器的准确率，可以得到

分类器 f1 的错误率为 0.1060 * 3，也就是 x 取值 6、7、8 时分类错误。
分类器 f2 的错误率为 0.0455 * 4，即 x 取值为 0、1、2、9 时分类错误。
分类器 f3 的错误率为 0.1667 * 3，即 x 取值 3、4、5 时分类错误。
在这 3 个分类器中，f2 分类器的错误率最低，因此我们选择 f2 作为第三轮训练的最优分类器，即：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210413161620.png" alt="image-20210413161620049" style="zoom:50%;" />

根据分类器权重公式得到：$\alpha_3 = 0.7514$



5. 假设我们只进行 3 轮的训练，选择 3 个弱分类器，组合成一个强分类器，那么==最终的强分类器==

`G(x) = 0.4236*G1(x) + 0.6496*G2(x)+0.7514*G3(x)`



这个过程不难的，简单梳理就是：

1. 确定初始样本的权重，然后训练分类器，根据误差最小，选择分类器，得到误差率，计算该分类器的权重
2. 然后根据该分类器的误差去重新计算样本的权重
3. 进行下一轮训练，若不停止，就重复上述过程。

> 理解起来这其实就是一个利用敌人去使自己的士兵变强的问题，假设敌人有10个人，我这边5个人（训练5轮）。
>
> 首先，我让这5个人分别去打那10个，选出最厉害的那一个，作为第一轮分类器， 然后10个敌人里面他能打过的，可以重要性降低，重点研究他打不过的那些人的套路
>
> 然后再训练，这样选出的第2个人，就可以对付一些第一个人打不过的敌人。
> 同理，后面再重点研究第2个人打不过的那些人，让第3个人来打， 慢慢的下去，直到结束。
>
> 这样就会发现，这五个人，虽然单拿出一个来，我敌人的这10个单挑的时候，没有一个人能完胜10局，但是这5个人放在一块的组合，就可以完胜这10局。



> 诸葛亮再厉害，水平也就是单挑10局可以完胜这10局，而我用普通的五个小士兵，经过5轮训练，这个组合也可以完胜10局，而后者的培养成本远远比一个诸葛亮的培养成本低的多的多。



# 实战|预测房价

对波士顿房价进行预测，并对比弟弟算法

## 1 Sklearn工具

- Classifier 这个类，一般都会对应着 Regressor 类

```python
from sklearn.ensemble import AdaBoostClassifier	#分类
from sklearn.ensemble import AdaBoostRegressor	#回归


# 创建AdaBoost分类器
AdaBoostClassifier(base_estimator=None, n_estimators=50, learning_rate=1.0, algorithm=’SAMME.R’, random_state=None)
'''
- base_estimator：代表的是弱分类器。
	在 AdaBoost 的分类器和回归器中都有这个参数，在 AdaBoost 中默认使用的是决策树，一般我们不需要修改这个参数，当然你也可以指定具体的分类器。

- n_estimators：算法的最大迭代次数，也是分类器的个数，每一次迭代都会引入一个新的弱分类器来增加原有的分类器的组合能力。默认是 50。

- learning_rate：代表学习率，取值在 0-1 之间，默认是 1.0。如果学习率较小，就需要比较多的迭代次数才能收敛，也就是说学习率和迭代次数是有相关性的。
	当你调整 learning_rate 的时候，往往也需要调整 n_estimators 这个参数。

- algorithm：代表我们要采用哪种 boosting 算法，一共有两种选择：SAMME 和 SAMME.R。
	默认是 SAMME.R。这两者之间的区别在于对弱分类权重的计算方式不同。

- random_state：代表随机数种子的设置，默认是 None。随机种子是用来控制随机模式的，当随机种子取了一个值，也就确定了一种随机规则，其他人取这个值可以得到同样的结果。
	如果不设置随机种子，每次得到的随机数也就不同。
'''



# 创建AdaBoost回归
AdaBoostRegressor(base_estimator=None, n_estimators=50, learning_rate=1.0, loss=‘linear’, random_state=None)
'''
回归和分类的参数基本是一致的，不同点在于回归算法里没有 algorithm 这个参数，但多了一个 loss 参数。

- loss 代表损失函数的设置，一共有 3 种选择，分别为 linear、square 和 exponential，它们的含义分别是线性、平方和指数。
	默认是线性。一般采用线性就可以得到不错的效果。
'''
```



创建好 AdaBoost 分类器或回归器之后，我们就可以输入训练集对它进行训练。

- 我们使用 fit 函数，传入训练集中的样本特征值 train_X 和结果 train_y，模型会自动拟合。
- 使用 predict 函数进行预测，传入测试集中的样本特征值 test_X，然后就可以得到预测结果。



## 2 对AdaBoost对房价进行预测

使用sklearn自带的波士顿房价数据集，用AdaBoost对房价进行预测:

### 数据集

一共包括了 506 条房屋信息数据，每一条数据都包括了 13 个指标(下表)，以及一个房屋价位。

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210413163154.webp)



处理思路（还是之前的处理套路）：

> 首先加载数据，将数据分割成训练集和测试集，
>
> 然后创建 AdaBoost 回归模型，传入训练集数据进行拟合，
>
> 再传入测试集数据进行预测，就可以得到预测结果。
>
> 最后将预测的结果与实际结果进行对比，得到两者之间的误差。

```python
from sklearn.model_selection import train_test_split
from sklearn.metrics import mean_squared_error
from sklearn.datasets import load_boston
from sklearn.ensemble import AdaBoostRegressor

# 加载数据
data=load_boston()

# 分割数据
train_x, test_x, train_y, test_y = train_test_split(data.data, data.target, test_size=0.25, random_state=33)

# 使用AdaBoost回归模型
regressor=AdaBoostRegressor()
regressor.fit(train_x,train_y)
pred_y = regressor.predict(test_x)
mse = mean_squared_error(test_y, pred_y)

print("房价预测结果 ", pred_y)
print("均方误差 = ",round(mse,2))
```



结果：均方误差 =  18.05





## 3.1 对比|决策树和KNN

```python
# 使用决策树回归模型
dec_regressor=DecisionTreeRegressor()
dec_regressor.fit(train_x,train_y)
pred_y = dec_regressor.predict(test_x)
mse = mean_squared_error(test_y, pred_y)
print("决策树均方误差 = ",round(mse,2))


# 使用KNN回归模型
knn_regressor=KNeighborsRegressor()
knn_regressor.fit(train_x,train_y)
pred_y = knn_regressor.predict(test_x)
mse = mean_squared_error(test_y, pred_y)
print("KNN均方误差 = ",round(mse,2))
```

决策树均方误差 =  23.84
KNN均方误差 =  27.87



这里就会发现，AdaBoost 的均方误差更小，也就是结果更优。

虽然 AdaBoost 使用了弱分类器，但是通过 50 个甚至更多的弱分类器组合起来而形成的强分类器，在很多情况下结果都优于其他算法。

因此 AdaBoost 也是常用的分类和回归算法之一。





## 3.2 对比|AdaBoost与决策树模型的比较

在 sklearn 中 AdaBoost 默认采用的是决策树模型，我们可以随机生成一些数据，然后对比下 

- AdaBoost 中的弱分类器（也就是决策树弱分类器）
- 决策树分类器
- AdaBoost 模型在分类准确率上的表现。



@数据集

> 如果想要随机生成数据，我们可以使用 sklearn 中的 make_hastie_10_2 函数生成二分类数据。
>
> 假设我们生成 12000 个数据，取前 2000 个作为测试集，其余作为训练集。



```python
import numpy as np
import matplotlib.pyplot as plt
from sklearn import datasets
from sklearn.metrics import zero_one_loss
from sklearn.tree import DecisionTreeClassifier
from sklearn.ensemble import  AdaBoostClassifier

# 设置AdaBoost迭代次数
n_estimators=200
# 使用
X,y=datasets.make_hastie_10_2(n_samples=12000,random_state=1)
# 从12000个数据中取前2000行作为测试集，其余作为训练集
train_x, train_y = X[2000:],y[2000:]
test_x, test_y = X[:2000],y[:2000]


# 弱分类器
dt_stump = DecisionTreeClassifier(max_depth=1,min_samples_leaf=1)
dt_stump.fit(train_x, train_y)
dt_stump_err = 1.0-dt_stump.score(test_x, test_y)


# 决策树分类器
dt = DecisionTreeClassifier()
dt.fit(train_x,  train_y)
dt_err = 1.0-dt.score(test_x, test_y)


# AdaBoost分类器
ada = AdaBoostClassifier(base_estimator=dt_stump,n_estimators=n_estimators)
ada.fit(train_x,  train_y)


# 三个分类器的错误率可视化
fig = plt.figure()


# 设置plt正确显示中文
plt.rcParams['font.sans-serif'] = ['SimHei']
ax = fig.add_subplot(111)
ax.plot([1,n_estimators],[dt_stump_err]*2, 'k-', label=u'决策树弱分类器 错误率')
ax.plot([1,n_estimators],[dt_err]*2,'k--', label=u'决策树模型 错误率')
ada_err = np.zeros((n_estimators,))


# 遍历每次迭代的结果 i为迭代次数, pred_y为预测结果
for i,pred_y in enumerate(ada.staged_predict(test_x)):
     # 统计错误率
    ada_err[i]=zero_one_loss(pred_y, test_y)
    
# 绘制每次迭代的AdaBoost错误率
ax.plot(np.arange(n_estimators)+1, ada_err, label='AdaBoost Test 错误率', color='orange')
ax.set_xlabel('迭代次数')
ax.set_ylabel('错误率')
leg=ax.legend(loc='upper right',fancybox=True)
plt.show()
```

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210413163531.jpeg" alt="图片" style="zoom:50%;" />

从图中你能看出来，弱分类器的错误率最高，只比随机分类结果略好，准确率稍微大于 50%。决策树模型的错误率明显要低很多。而 AdaBoost 模型在迭代次数超过 25 次之后，错误率有了明显下降，经过 125 次迭代之后错误率的变化形势趋于平缓。



能看出，虽然单独的一个决策树弱分类器效果不好，但是==多个决策树弱分类器组合起来形成的 AdaBoost 分类器，分类效果要好于决策树模型==。





# 总结

今天，学习了AdaBoost算法，从集成到AdaBoost的原理到最后的小实战，全都过了一遍，通过今天的学习，我们会发现，集成算法的强大和成本小。

现在很多应用都使用的集成技术，AdaBoost现在用的不多了，==无论是打比赛还是日常应用，都喜欢用xgboost，lightgbm，catboost这些算法了==。

当然，虽然学习的深入，这些算法肯定也会大白话出来。

但是出来之前，还是先搞懂AdaBoost的原理吧，这样也好对比，而对比，印象也就越深刻。





# # 参考文献

- http://note.youdao.com/noteshare?id=66342c3e1397080d344c1e097e78a58b&sub=8612E6460FD24B08929CF91F4B2E2C00
- http://note.youdao.com/noteshare?id=bbcaf64e112fe562e908e78964d85da1&sub=98DF566CEB644BC49CDF2E089DD1EA26
- https://time.geekbang.org

