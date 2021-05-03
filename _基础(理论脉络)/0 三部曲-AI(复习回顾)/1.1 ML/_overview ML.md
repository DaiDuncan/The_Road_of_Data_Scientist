[link](https://easyai.tech/ai-definition/machine-learning/)

- [机器学习、人工智能、深度学习是什么关系？](https://easyai.tech/ai-definition/machine-learning/#ai-ml-dl)
- [什么是机器学习？](https://easyai.tech/ai-definition/machine-learning/#what)
- [监督学习、非监督学习、强化学习](https://easyai.tech/ai-definition/machine-learning/#3kinds)
- [机器学习实操的7个步骤](https://easyai.tech/ai-definition/machine-learning/#7steps)
- [15种经典机器学习算法](https://easyai.tech/ai-definition/machine-learning/#15ways)
- [百度百科+维基百科](https://easyai.tech/ai-definition/machine-learning/#wiki)
- [补充资料2：优质扩展阅读](https://easyai.tech/ai-definition/machine-learning/#other)



## 机器学习、人工智能、深度学习是什么关系？

1956 年提出 AI 概念，短短3年后（1959） [Arthur Samuel](https://en.wikipedia.org/wiki/Arthur_Samuel) 就提出了机器学习的概念：

> Field of study that gives computers the ability to learn without being explicitly programmed.
>
> 机器学习研究和构建的是一种特殊算法（**而非某一个特定的算法**），能够让计算机自己在数据中学习从而进行预测。



所以，**机器学习不是某种具体的算法，而是很多算法的统称。**

机器学习包含了很多种不同的算法，深度学习就是其中之一，其他方法包括决策树，聚类，贝叶斯等。

深度学习的灵感来自大脑的结构和功能，即许多神经元的互连。人工神经网络（ANN）是模拟大脑生物结构的算法。

不管是机器学习还是深度学习，都属于人工智能（AI）的范畴。所以人工智能、机器学习、深度学习可以用下面的图来表示：

![人工智能、机器学习、深度学习的关系](https://easyai.tech/wp-content/uploads/2018/12/ai-ml-dl.png)



## 什么是机器学习？

在解释机器学习的原理之前，先把最精髓的基本思路介绍给大家，理解了机器学习最本质的东西，就能更好的利用机器学习，同时这个解决问题的思维还可以用到工作和生活中。

**机器学习的基本思路**

1. 把现实生活中的**问题抽象成数学模型**，并且很清楚模型中不同参数的作用
2. 利用数学方法对这个数学模型进行求解，从而解决现实生活中的问题
3. 评估这个数学模型，是否真正的解决了现实生活中的问题，解决的如何？

**无论使用什么算法，使用什么样的数据，最根本的思路都逃不出上面的3步！**

![机器学习的基本思路](https://easyai.tech/wp-content/uploads/2018/12/ml-silu.png)机器学习的基本思路

当我们理解了这个基本思路，我们就能发现：

不是所有问题都可以转换成数学问题的。那些没有办法转换的现实问题 AI 就没有办法解决。

- 同时最难的部分也就是把现实问题转换为数学问题这一步。

 

**机器学习的原理**

下面以监督学习为例，给大家讲解一下机器学习的实现原理。

假如我们正在教小朋友识字（一、二、三）。

我们首先会拿出3张卡片，然后便让小朋友看卡片，一边说“一条横线的是一、两条横线的是二、三条横线的是三”。



不断重复上面的过程，小朋友的大脑就在不停的学习。



当重复的次数足够多时，小朋友就学会了一个新技能——认识汉字：一、二、三。



我们用上面人类的学习过程来类比机器学习。机器学习跟上面提到的人类学习过程很相似。

- 上面提到的认字的卡片在机器学习中叫——训练集
- 上面提到的“一条横线，两条横线”这种区分不同汉字的属性叫——特征
- 小朋友不断学习的过程叫——建模
- 学会了识字后总结出来的规律叫——模型

**通过训练集，不断识别特征，不断建模，最后形成有效的模型，这个过程就叫“机器学习”！**

![机器学习原理说明4](https://easyai.tech/wp-content/uploads/2018/12/ml-step-4.png)

 

## 监督学习、非监督学习、强化学习

机器学习根据训练方法大致可以分为3大类：

1. 监督学习
2. 非监督学习
3. 强化学习

除此之外，大家可能还听过“半监督学习”之类的说法，但是那些都是基于上面3类的变种，本质没有改变。

 

**监督学习**

监督学习是指我们给算法一个数据集，并且给定正确答案。机器通过数据来学习正确答案的计算方法。

举个栗子：

我们准备了一大堆猫和狗的照片，我们想让机器学会如何识别猫和狗。当我们使用监督学习的时候，我们需要给这些照片打上标签。

![将打好标签的照片用来训练](https://easyai.tech/wp-content/uploads/2018/12/tag-cat-dog.png)将打好标签的照片用来训练

我们给照片打的标签就是“正确答案”，机器通过大量学习，就可以学会在新照片中认出猫和狗。

![当机器遇到新的小狗照片时就能认出他](https://easyai.tech/wp-content/uploads/2018/12/dog.png)当机器遇到新的小狗照片时就能认出他

这种通过大量人工打标签来帮助机器学习的方式就是监督学习。这种学习方式效果非常好，但是成本也非常高。

[ 了解更多关于 监督学习](https://easyai.tech/ai-definition/supervised-learning/)

 

**非监督学习**

非监督学习中，给定的数据集没有“正确答案”，**所有的数据都是一样的**。

无监督学习的任务是从给定的数据集中，挖掘出潜在的结构。

举个栗子：

我们把一堆猫和狗的照片给机器，不给这些照片打任何标签，但是我们希望机器能够将这些照片分分类。

![将不打标签的照片给机器](https://easyai.tech/wp-content/uploads/2018/12/hun.png)将不打标签的照片给机器

通过学习，机器会把这些照片分为2类，一类都是猫的照片，一类都是狗的照片。



虽然跟上面的监督学习看上去结果差不多，但是有着本质的差别：

**非监督学习中，虽然照片分为了猫和狗，但是机器并不知道哪个是猫，哪个是狗。**

**对于机器来说，相当于分成了 A、B 两类。**

![机器可以将猫和狗分开，但是并不知道哪个是猫，哪个是狗](https://easyai.tech/wp-content/uploads/2018/12/fenlei.png)机器可以将猫和狗分开，但是并不知道哪个是猫，哪个是狗

[ 了解更多关于 非监督学习](https://easyai.tech/ai-definition/unsupervised-learning/)

 

**强化学习**

==强化学习更接近生物学习的本质，因此有望获得更高的智能==。

它关注的是智能体如何在环境中采取一系列行为，从而获得**最大的累积回报**。

通过强化学习，一个智能体应该知道在什么状态下应该采取什么行为。

==最典型的场景就是打游戏。==

2019年1月25日，AlphaStar（Google 研发的人工智能程序，采用了强化学习的训练方式） 完虐星际争霸的职业选手职业选手“TLO”和“MANA”。[新闻链接](http://sports.sina.com.cn/go/2019-01-25/doc-ihqfskcp0200300.shtml)

[ 了解更多关于 强化学习](https://easyai.tech/ai-definition/reinforcement-learning/)

 

## 机器学习实操的7个步骤

通过上面的内容，我们对机器学习已经有一些模糊的概念了，这个时候肯定会特别好奇：到底怎么使用机器学习？

机器学习在实际操作层面一共分为7步：

1. 收集数据
2. 数据准备
3. 选择一个模型
4. 训练
5. 评估
6. 参数调整
7. 预测（开始使用）

![机器学习的7个步骤](https://easyai.tech/wp-content/uploads/2018/12/7steps-ml.png)

机器学习的7个步骤

假设我们的任务是通过酒精度和颜色来区分红酒和啤酒，下面详细介绍一下机器学习中每一个步骤是如何工作的。

![案例目标：区分红酒和啤酒](https://easyai.tech/wp-content/uploads/2018/12/wine-beer.png)案例目标：区分红酒和啤酒

 

**步骤1：收集数据**

我们在超市买来一堆不同种类的啤酒和红酒，然后再买来测量颜色的光谱仪和用于测量酒精度的设备。

这个时候，我们把买来的所有酒都标记出他的颜色和酒精度，会形成下面这张表格。

| 颜色 | 酒精度 | 种类 |
| :--- | :----- | :--- |
| 610  | 5      | 啤酒 |
| 599  | 13     | 红酒 |
| 693  | 14     | 红酒 |
| …    | …      | …    |

**这一步非常重要，因为数据的数量和质量直接决定了预测模型的好坏。**

 

**步骤2：数据准备**

在这个例子中，我们的数据是很工整的，但是在实际情况中，我们收集到的数据会有很多问题，所以会涉及到数据清洗等工作。

当数据本身没有什么问题后，我们将数据分成3个部分：训练集（60%）、验证集（20%）、测试集（20%），用于后面的验证和评估工作。

![数据要分为3个部分：训练集、验证集、测试集](https://easyai.tech/wp-content/uploads/2018/12/3dataset.png)

 

**步骤3：选择一个模型**

研究人员和数据科学家多年来创造了许多模型。有些非常适合图像数据，有些非常适合于序列（如文本或音乐），有些用于数字数据，有些用于基于文本的数据。

在我们的例子中，由于我们只有2个特征，**颜色和酒精度**，我们可以使用一个小的线性模型，这是一个相当简单的模型。

 

**步骤4：训练**

大部分人都认为这个是最重要的部分，其实并非如此~ 数据数量和质量、还有模型的选择比训练本身重要更多（训练知识台上的3分钟，更重要的是台下的10年功）。

这个过程就不需要人来参与的，机器独立就可以完成，整个过程就好像是在做算术题。

因为机器学习的本质就是**将问题转化为数学问题，然后解答数学题的过程**。

 

**步骤5：评估**

一旦训练完成，就可以评估模型是否有用。

这是我们之前预留的验证集和测试集发挥作用的地方。**评估的指标**主要有 准确率、召回率、F值。

这个过程可以让我们看到模型如何对尚未看到的数是如何做预测的。这意味着代表模型在现实世界中的表现。

 

**步骤6：参数调整**

完成评估后，您可能希望了解是否可以以任何方式进一步改进训练。

我们可以通过调整参数来做到这一点。

当我们进行训练时，我们隐含地假设了一些参数，我们可以通过认为的调整这些参数让模型表现的更出色。

 

**步骤7：预测**

我们上面的6个步骤都是为了这一步来服务的。

这也是机器学习的价值。这个时候，当我们买来一瓶新的酒，只要告诉机器他的颜色和酒精度，他就会告诉你，这时啤酒还是红酒了。



## 15种经典机器学习算法

| 算法                                                         | 训练方式   |
| :----------------------------------------------------------- | :--------- |
| [线性回归](https://easyai.tech/ai-definition/linear-regression/) | 监督学习   |
| [逻辑回归](https://easyai.tech/ai-definition/logistic-regression/) | 监督学习   |
| [线性判别分析](https://easyai.tech/ai-definition/linear-discriminant-analysis/) | 监督学习   |
| [决策树](https://easyai.tech/ai-definition/decision-tree/)   | 监督学习   |
| [朴素贝叶斯](https://easyai.tech/ai-definition/naive-bayes-classifier/) | 监督学习   |
| [K邻近](https://easyai.tech/ai-definition/k-nearest-neighbors/) | 监督学习   |
| [学习向量量化](https://easyai.tech/ai-definition/learning-vector-quantization/) | 监督学习   |
| [支持向量机](https://easyai.tech/ai-definition/svm/)         | 监督学习   |
| [随机森林](https://easyai.tech/ai-definition/random-forest/) | 监督学习   |
| [AdaBoost](https://easyai.tech/ai-definition/adaboost/)      | 监督学习   |
| 高斯混合模型                                                 | 非监督学习 |
| [限制波尔兹曼机](https://easyai.tech/ai-definition/restricted-boltzmann-machine/) | 非监督学习 |
| [K-means 聚类](https://easyai.tech/ai-definition/k-means-clustering/) | 非监督学习 |
| 最大期望算法                                                 | 非监督学习 |



 

## 百度百科+维基百科

百度百科版本

机器学习(Machine Learning, ML)是一门多领域交叉学科，涉及概率论、统计学、逼近论、**凸分析**、算法复杂度理论等多门学科。

专门研究计算机怎样模拟或实现人类的学习行为，以获取新的知识或技能，重新组织已有的知识结构使之不断改善自身的性能。 

它是人工智能的核心，是使计算机具有智能的根本途径，其应用遍及人工智能的各个领域，它主要使用归纳、综合而不是演绎。

[查看详情](https://baike.baidu.com/item/机器学习/217599?fr=aladdin)

 

维基百科版本

机器学习是利用计算机算法和统计模型是计算机系统使用，逐步提高完成特定任务的能力。

机器学习建立样本数据的数学模型，称为“ 训练数据 ”，以便在不明确编程以执行任务的情况下进行预测或决策。

机器学习算法用于电子邮件过滤，网络入侵者检测和计算机视觉的应用，开发用于执行任务的特定指令的算法是不可行的。

机器学习与计算统计密切相关，计算统计侧重于使用计算机进行预测。

数学优化的研究为机器学习领域提供了方法，理论和应用领域。

数据挖掘是机器学习中的一个研究领域，侧重于通过无监督学习进行探索性数据分析。在跨业务问题的应用中，机器学习也被称为预测分析。

[查看详情](https://en.wikipedia.org/wiki/Machine_learning)

 



# #参考文献

- [强化学习-Reinforcement learning | RL](https://easyai.tech/ai-definition/reinforcement-learning/) 16
- [K均值聚类（k-means clustering）](https://easyai.tech/ai-definition/k-means-clustering/) 1
- [逻辑回归 - Logistic regression](https://easyai.tech/ai-definition/logistic-regression/) 3
- [受限玻尔兹曼机（Restricted Boltzmann machine | RBM）](https://easyai.tech/ai-definition/restricted-boltzmann-machine/)
- [学习向量量化 - Learning vector quantization | LVQ](https://easyai.tech/ai-definition/learning-vector-quantization/)



拓展视野类文章（11）

[统计学和机器学习到底有什么区别？](https://mp.weixin.qq.com/s/xCJBowXS89UlHA07R8WNuw)（2019-4）

[可解释的机器学习](https://easyai.tech/blog/interpretable-machine-learning/)（2019-4）

[机器学习不是统计学！这篇文章终于把真正区别讲清楚了](https://mp.weixin.qq.com/s/XCkXdo81_0Qn5AR-LWAWUw)（2019-4）

[小样本学习（Few-shot Learning）综述](https://mp.weixin.qq.com/s/-73CC3JqnM7wxEqIWCejWQ)（2019-4）

[Quora上的大牛们最喜欢哪种机器学习算法?](https://mp.weixin.qq.com/s/1GEZmAdwHyrhqurpxWaYDw)（2019-3）

[独家解读！阿里重磅发布机器学习平台PAI 3.0](https://mp.weixin.qq.com/s/rX8L63-jDGJT6lCAj04I3Q)（2019-3）

[计算性能提升100倍！Uber推出机器学习可视化调试工具Manifold](https://cloud.tencent.com/developer/news/391214)

[机器学习领域，史上引用次数最多的论文 Top 10](https://easyai.tech/blog/machine-learning-top-10-paper/)

[行业前沿：AutoML会成为机器学习世界的主流吗？](http://www.sohu.com/a/296935307_100118081)

[入坑机器学习，你首先得知道这十个知识点…](https://36kr.com/p/5092929.html)



实践类文章（4）

[写给机器学习从业者的12条宝贵建议](https://mp.weixin.qq.com/s/AANo0iliShDavXgZZIsQ7g)（2019-4）

[整理一些机器学习入门方法和资料](https://mp.weixin.qq.com/s/Pl6kwt05lArrsBVwk4bAkg)（2019-3-22）

[干货|模型评估方法基础总结](https://mp.weixin.qq.com/s/OmSxnVL6pYYzB9_jDV4Lqg)

[深度解析机器学习系统的八大坑](https://mp.weixin.qq.com/s/L2082oEubUDKg19y5N1Bgg)



相关资源（15）

[吐血整理！这可能是最全的机器学习工具手册](https://mp.weixin.qq.com/s/y5efL4mxOb678LQ0b6_dUg)（2019-5）

[谷歌开源大规模神经网络模型高效训练库 GPipe](https://mp.weixin.qq.com/s/J4JtDFS_tw9sWx-my5x7GQ)（2019-3）

「公开课视频」[一次性掌握机器学习基础知识脉络](https://mp.weixin.qq.com/s/yGAzz5Layw5fAVZkQw7qdg)

「书籍」《[python机器学习](https://book.douban.com/subject/27000110/)》

「书籍」《[Scikit-Learn与TensorFlow机器学习实用指南](https://book.douban.com/subject/27154347/)》

「教程」[机器学习 100 天](https://github.com/MLEveryday/100-Days-Of-ML-Code)（入门推荐）

「教程」[吴恩达的在线视频课程——机器学习](https://www.coursera.org/learn/machine-learning)

[「谷歌研究总监推荐」100页看完机器学习](https://mp.weixin.qq.com/s/hvnvil2LFDudD0Giz7_rDA)

「教程」[李宏毅主页（大量视频教程+资料）](http://speech.ee.ntu.edu.tw/~tlkagk/index.html)

「教程」[Google的机器学习速成课](https://developers.google.com/machine-learning/crash-course/)（需要科学上网）

「教程」[亚马逊的机器学习课程](https://aws.amazon.com/cn/training/learning-paths/machine-learning/)

「教程」[微软的机器学习课程](https://aischool.microsoft.com/en-us/machine-learning/learning-paths)

「付费课程」[机器学习40讲——极客时间](https://time.geekbang.org/column/intro/97)

「资源」[2018年10大机器学习开源项目](https://easyai.tech/blog/ml-10-open-source/)

[总结机器学习优质学习文章Top50！](https://mp.weixin.qq.com/s/DuNCR_UfPnLq4WhGsj7Cyw)

