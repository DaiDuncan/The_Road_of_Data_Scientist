[知乎专栏|白话CRF](https://zhuanlan.zhihu.com/p/34261803)



# **CRF 综述**

简单而又直白的讲，线性条件随机场，是只考虑 概率图中相邻变量是否满足特征函数 ![[公式]](https://www.zhihu.com/equation?tex=F%28y%2Cx%29) 的一个模型。

![img](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210608155439.jpeg)

例如在词性标注任务中，

- 两个动词相连我们可以给负分：转移特征函数![[公式]](https://www.zhihu.com/equation?tex=t%28y_%7B2%7D%3Dv.%2Cy_%7B3%7D%3Dv.%2Cx%2Ci%29+%3D+-1) ; 
- 把 a 标注成不定冠词可以给正分：状态特征 函数![[公式]](https://www.zhihu.com/equation?tex=s%28y_%7B3%7D%3Dart.%2Cx%2Ci%29+%3D+1) 。 



条件随机场的参数化定义为：

![[公式]](https://www.zhihu.com/equation?tex=P%28y%7Cx%29+%3D+%5Cfrac%7B1%7D%7BZ%28x%29%7Dexp%28+%5Csum_%7Bi%2Ck%7D%5E%7B%7D%7B%5Clambda_%7Bk%7D%7Dt_%7Bk%7D%28y_%7Bi-1%7D%2Cy_%7Bi%7D%2Cx%2Ci%29%2B%5Csum_%7Bi%2Cl%7D%5E%7B%7D%7B%5Cmu_%7Bl%7D%7Ds_%7Bl%7D%28y_%7Bi%7D%2Cx%2Ci%29+%29)

![[公式]](https://www.zhihu.com/equation?tex=Z%28x%29%3D+%5Csum_%7By%7D%5E%7B%7D%7Bexp%28+%5Csum_%7Bi%2Ck%7D%5E%7B%7D%7B%5Clambda_%7Bk%7D%7Dt_%7Bk%7D%28y_%7Bi-1%7D%2Cy_%7Bi%7D%2Cx%2Ci%29%2B%5Csum_%7Bi%2Cl%7D%5E%7B%7D%7B%5Cmu_%7Bl%7D%7Ds_%7Bl%7D%28y_%7Bi%7D%2Cx%2Ci%29+%29%7D)



当我们给每个特征函数不同的权重 ![[公式]](https://www.zhihu.com/equation?tex=%5Comega) ，把转移特征 ![[公式]](https://www.zhihu.com/equation?tex=t%28y_%7Bi-1%7D%2Cy_%7Bi%7D%2Cx%2Ci%29+) ，和状态特征 ![[公式]](https://www.zhihu.com/equation?tex=s%28y_%7Bi%7D%2Cx%2Ci%29+) 统一写成 ![[公式]](https://www.zhihu.com/equation?tex=F%28y%2Cx%29) 后，亦可以从条件随机场的简化形式看出条件随机场模型是如何工作的：

![[公式]](https://www.zhihu.com/equation?tex=P_%7Bw%7D%28y%7Cx%29+%3D+%5Cfrac%7B1%7D%7BZ_%7BW%7D%28x%29%7Dexp%28+w%5Ccdot+F%28y%2Cx%29+%29)

![[公式]](https://www.zhihu.com/equation?tex=Z_%7Bw%7D%28x%29+%3D+%5Csum_%7By%7D%5E%7B%7D%7Bexp%28+w%5Ccdot+F%28y%2Cx%29+%29%7D)

由此，我们可以窥一豹。

条件随机场模型在**统计语料库中相邻词是否满足特征函数的频数**，并依此给出 ![[公式]](https://www.zhihu.com/equation?tex=P_%7Bw%7D%28y%7Cx%29) ⭐

在给定的 ![[公式]](https://www.zhihu.com/equation?tex=%28x%2Cy%29) ，满足的特征函数越多，模型认为 ![[公式]](https://www.zhihu.com/equation?tex=P_%7Bw%7D%28y%7Cx%29) 越大。





特征函数便是图中的conditional。

![img](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210608155551.jpeg)



**以下是简单的说明，综合概述Naive Bayes，Logistic Regression, HMM, Linear-chain CRF之间的关系。**

## **Naive Bayes：**

统计训练资料里所有 ![[公式]](https://www.zhihu.com/equation?tex=x) 个数得到 ![[公式]](https://www.zhihu.com/equation?tex=V_%7Bx%7D) ，统计所有 ![[公式]](https://www.zhihu.com/equation?tex=X) 个数得到 ![[公式]](https://www.zhihu.com/equation?tex=V_%7BX%7D) 。

所以得到训练数据里 ![[公式]](https://www.zhihu.com/equation?tex=x) 的概率，也是先验概率![[公式]](https://www.zhihu.com/equation?tex=%5Ctilde%7BP%7D%28x%29+%3D+%5Cfrac%7BV_%7Bx%7D%7D%7BV_%7BX%7D%7D) 。照此方法我们也可以得到成对出现的 ![[公式]](https://www.zhihu.com/equation?tex=%28x%2Cy%29)的先验概率![[公式]](https://www.zhihu.com/equation?tex=%5Ctilde%7BP%7D%28x%2Cy%29+) 。

至此可以根据贝叶斯公式 ![[公式]](https://www.zhihu.com/equation?tex=%5Ctilde%7BP%7D%28x%29P%28y%7Cx%29+%3D+%5Ctilde%7BP%7D%28x%2Cy%29) 得到模型 ![[公式]](https://www.zhihu.com/equation?tex=P%28y%7Cx%29+%3D+%5Cfrac%7B%5Ctilde%7BP%7D%28x%2Cy%29%7D%7B%5Ctilde%7BP%7D%28x%29%7D) 。

就可以拿 ![[公式]](https://www.zhihu.com/equation?tex=P%28y%7Cx%29+) 开心做预测啦。





## **Logistic Regression,：**

狭义的多项逻辑回归参数化定义为： ![[公式]](https://www.zhihu.com/equation?tex=P%28Y%3Dk%7C+x%29+%3D+%5Cfrac%7B+exp%28w_%7Bk%7D%5Ccdot+x%29+%7D%7B+1%2B%5Csum_%7Bk%3D1%7D%5E%7BK-1%7D%7Bexp%28w_%7Bk%7D%5Ccdot+x%29%29%7D+%7D%2C+k+%3D+1%2C2%2C...%2CK-1)

是不是和CRF的定义式有一点像了？ ![[公式]](https://www.zhihu.com/equation?tex=P_%7Bw%7D%28y%7Cx%29+%3D+%5Cfrac%7Bexp%28+w%5Ccdot+F%28y%2Cx%29+%29%7D%7B%5Csum_%7By%7D%5E%7B%7D%7Bexp%28+w%5Ccdot+F%28y%2Cx%29+%29%7D%29%7D)？

我们是不是可以==把逻辑回归中的 ![[公式]](https://www.zhihu.com/equation?tex=w_%7Bk%7D%5Ccdot+x) 看作是特征函数==？



当我们把逻辑回归拓展成最大熵模型时，得到‘广义的逻辑回归模型’：

1.统计训练集中的先验 ![[公式]](https://www.zhihu.com/equation?tex=%5Ctilde%7BP%7D%28x%29+) ， ![[公式]](https://www.zhihu.com/equation?tex=%5Ctilde%7BP%7D%28x%2Cy%29+) 。

2.==设计特征函数== ![[公式]](https://www.zhihu.com/equation?tex=f%28x%2Cy%29) ， ![[公式]](https://www.zhihu.com/equation?tex=x) 与 ![[公式]](https://www.zhihu.com/equation?tex=y) 满足某种关系取1，不满足取0。(当然可以取其他值，比如之前举的例子，两个动词一起出现时就取-1)

3.训练数据集上符合特征的期望： ![[公式]](https://www.zhihu.com/equation?tex=E_%7B%5Ctilde%7BP%7D%7D+%3D+%5Csum_%7Bx%2Cy%7D%5E%7B%7D%7B%5Ctilde%7BP%7D%28x%2Cy%29f%28x%2Cy%29%7D) 。这个式子的意思是，假设我们只有一个特征函数 ![[公式]](https://www.zhihu.com/equation?tex=f) ，特定的 ![[公式]](https://www.zhihu.com/equation?tex=%28x%2Cy%29) 在总数为10的训练集上出现过1次，且刚好满足着唯一的特征函数，那么训练数据上的 ![[公式]](https://www.zhihu.com/equation?tex=E_%7B%5Ctilde%7BP%7D%7D+) 为0.1.

4.若模型学到了 ![[公式]](https://www.zhihu.com/equation?tex=P%28y%7Cx%29) ，我们便可以认为 ![[公式]](https://www.zhihu.com/equation?tex=%5Csum_%7Bx%2Cy%7D%5E%7B%7D%7B%5Ctilde%7BP%7D%28x%29P%28y%7Cx%29f%28x%2Cy%29%7D+%3D+%5Csum_%7Bx%2Cy%7D%5E%7B%7D%7B%5Ctilde%7BP%7D%28x%2Cy%29f%28x%2Cy%29%7D) 。至此，我们已经在Naive Bayes的基础上==融入了特征函数==，也就是conditional。

5.实际上这是满足条件的模型有很多，我们选一个可以让熵最大的模型。

经过一系列数学步骤（拉格朗日对偶，求偏导，使偏导数为0），我们得到最大熵模型 ![[公式]](https://www.zhihu.com/equation?tex=P_%7Bw%7D%28y%7Cx%29+%3D+%5Cfrac%7Bexp%28+w%5Ccdot+F%28y%2Cx%29+%29%7D%7B%5Csum_%7By%7D%5E%7B%7D%7Bexp%28+w%5Ccdot+F%28y%2Cx%29+%29%7D%29%7D) ，天啊，简直和CRF最后的模型一样一样的。





逻辑回归模型(最大熵模型)统计的是训练集中的**各种数据满足特征函数的频数**(conditional)，而贝叶斯模型统计的是**训练集中的各种数据的频数**。

而CRF统计的是**训练集中相关数据 (**比如说相邻的词，不相邻的词不统计) **满足特征函数**的频数。





## **HMM**：

![img](https://pic3.zhimg.com/80/v2-2092abe6f399ad504e65410b5a3f7e82_720w.jpg)

白箱里有2个红球，8个蓝球；黑箱里有7个红球，3个蓝球。**有放回取球**。

即白箱的观测概率 ![[公式]](https://www.zhihu.com/equation?tex=P_%7BO%E7%99%BD%7D+%3D+%E7%BA%A2%E7%90%830.2%EF%BC%8C%E8%93%9D%E7%90%830.8) ，黑箱的观测概率 ![[公式]](https://www.zhihu.com/equation?tex=P_%7BO%E9%BB%91%7D+%3D+%E7%BA%A2%E7%90%830.7%EF%BC%8C%E8%93%9D%E7%90%830.3) 。



设定规则，当我从白箱取球时，下一次继续从白箱取球的概率是0.4，从黑箱取球的概率是0.6。

当我从黑箱取球时，下一次继续从黑箱取球的概率是0.7，从白箱取球的概率是0.3。

即白箱的状态转移概率 ![[公式]](https://www.zhihu.com/equation?tex=P_%7BS%E7%99%BD%7D+%3D+%E7%99%BD%E7%AE%B10.4%EF%BC%8C%E9%BB%91%E7%AE%B10.6) ， 黑箱的状态转移概率![[公式]](https://www.zhihu.com/equation?tex=P_%7BS%E9%BB%91%7D+%3D+%E7%99%BD%E7%AE%B10.3%EF%BC%8C%E9%BB%91%E7%AE%B10.7)

从哪个箱子里取球不可观测，只能观测取出球的颜色。

HMM从可观测的球的序列中进行推测。

请自行推断HMM与CRF之间的conditional关系。





## **概率图模型**

简单的来说，不相连的变量之间毫无关系。

> 定义(概率无向图模型)：设有联合概率分布 ![[公式]](https://www.zhihu.com/equation?tex=P%28Y%29) ,由无向图 ![[公式]](https://www.zhihu.com/equation?tex=G+%3D+%28V%2CE%29) 表示。
>
> 图 ![[公式]](https://www.zhihu.com/equation?tex=G) 中，节点表示随机变量，边表示随机变量之间的以来关系。
>
> 如果联合概率分布满足成对，局部或全局马尔可夫性，就称此联合概率分布为概率无向图模型，或马尔科夫随机场。



**概率无向图的因子分解**

最大团

> 定义：无向图G中任何两个结点均有边连接的节点子集称为团（clique）.
>
> 若C是无向图G的一个团，并且不能再加进任何一个G的结点使其成为一个更大的团，则称此C为最大团（maximal clique）.



在无向概率图中，最大的任意两个结点都有相连的边的组团。

> Hammersley-Clifford 定理：概率==无向图模型的联合概率分布P(Y)==可以表示为如下形式：
> ![[公式]](https://www.zhihu.com/equation?tex=P%28Y%29+%3D+%5Cfrac%7B1%7D%7BZ%7D%5Csum_%7Bc%7D%5E%7B%7D%7B%5CPsi_%7Bc%7D%28Y_%7Bc%7D%29%7D)
> ![[公式]](https://www.zhihu.com/equation?tex=Z+%3D+%5Csum_%7BY%7D%5E%7B%7D%7B%5Cprod_%7Bc%7D%5E%7B%7D%5Cpsi_%7Bc%7D%28Y_%7Bc%7D%29%7D)
> 其中，C是无向图的最大团， ![[公式]](https://www.zhihu.com/equation?tex=Y_%7Bc%7D) 是 C 的结点对应的随机变量， ![[公式]](https://www.zhihu.com/equation?tex=%5CPsi_%7Bc%7D%28Y_%7Bc%7D%29) 是C上定义的严格正函数，乘积是在无向图所有最大团上进行的。

![img](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210608160101.jpeg)

就是可以==拆成最大团的概率连乘==。

![[公式]](https://www.zhihu.com/equation?tex=P%28Y%29+%3D+%5Cfrac%7B1%7D%7BZ%7D%5Csum_%7Bc%7D%5E%7B%7D%7B%5CPsi_%7Bc%7D%28Y_%7Bc%7D%29%7D)

![[公式]](https://www.zhihu.com/equation?tex=Z+%3D+%5Csum_%7BY%7D%5E%7B%7D%7B%5Cprod_%7Bc%7D%5E%7B%7D%5Cpsi_%7Bc%7D%28Y_%7Bc%7D%29%7D)

![[公式]](https://www.zhihu.com/equation?tex=%5CPsi_%7Bc%7D%28Y_%7Bc%7D%29+%3D+exp%7B-E%28Y_%7Bc%7D%29%7D)



## **线性条件随机场**

**模型的参数化形式**

![[公式]](https://www.zhihu.com/equation?tex=P%28y%7Cx%29+%3D+%5Cfrac%7B1%7D%7BZ%28x%29%7Dexp%28+%5Csum_%7Bi%2Ck%7D%5E%7B%7D%7B%5Clambda_%7Bk%7D%7Dt_%7Bk%7D%28y_%7Bi-1%7D%2Cy_%7Bi%7D%2Cx%2Ci%29%2B%5Csum_%7Bi%2Cl%7D%5E%7B%7D%7B%5Cmu_%7Bl%7D%7Ds_%7Bl%7D%28y_%7Bi%7D%2Cx%2Ci%29+%29)

![[公式]](https://www.zhihu.com/equation?tex=Z%28x%29%3D+%5Csum_%7By%7D%5E%7B%7D%7Bexp%28+%5Csum_%7Bi%2Ck%7D%5E%7B%7D%7B%5Clambda_%7Bk%7D%7Dt_%7Bk%7D%28y_%7Bi-1%7D%2Cy_%7Bi%7D%2Cx%2Ci%29%2B%5Csum_%7Bi%2Cl%7D%5E%7B%7D%7B%5Cmu_%7Bl%7D%7Ds_%7Bl%7D%28y_%7Bi%7D%2Cx%2Ci%29+%29%7D)



**模型的简化表示**

转移特征函数 ![[公式]](https://www.zhihu.com/equation?tex=t_%7Bk%7D%28y_%7Bi-1%7D%2Cy_%7Bi%7D%2Cx%2Ci%29) 和 状态特征函数 ![[公式]](https://www.zhihu.com/equation?tex=s_%7Bl%7D%28y_%7Bi%7D%2Cx%2Ci%29) 统一用 ![[公式]](https://www.zhihu.com/equation?tex=F%28y%2Cx%29) 表示。

同时将转移特征的权重 ![[公式]](https://www.zhihu.com/equation?tex=%5Clambda_%7Bk%7D) 与状态特征的权重 ![[公式]](https://www.zhihu.com/equation?tex=%5Cmu_%7Bl%7D) 统一用 ![[公式]](https://www.zhihu.com/equation?tex=%5Comega) 表示，于是模型可以简写为：

![[公式]](https://www.zhihu.com/equation?tex=P_%7Bw%7D%28y%7Cx%29+%3D+%5Cfrac%7B1%7D%7BZ_%7BW%7D%28x%29%7Dexp%28+w%5Ccdot+F%28y%2Cx%29+%29)

![[公式]](https://www.zhihu.com/equation?tex=Z_%7Bw%7D%28x%29+%3D+%5Csum_%7By%7D%5E%7B%7D%7Bexp%28+w%5Ccdot+F%28y%2Cx%29+%29%7D)



**线性条件随机场的学习算法**

可以通过**梯度下降**，拟牛顿法，改进的迭代尺度法等方法学习模型的参数。

- 梯度下降可行，但不容易收敛💡





# #参考文献

## 评论

1 想看懂你这些字需要首先去看非常详细的crf介绍文章才可以。

先掌握底层的每个细节，然后再来你这里宏观的清晰的概括一下。

事实上在词法分析这一块，即使是bert也没能干掉crf。



2 看这篇文章需要先把：朴素贝叶斯、逻辑斯蒂回归+最大熵模型、EM算法、隐马尔可夫模型HMM、条件随机场CRF 这些原理搞清楚才行



3 愿意关注传统的概率图模型的人不多了啊。。。 2018.03.17

- 已经被深度学习干翻了



