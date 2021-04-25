## 总结|思路脉络(经典+工程价值)

**2017年**由微软提出，是GBDT模型的另一个进化版本， 主要用于解决GBDT在海量数据中遇到的问题，以便更好更快的用于工业实践中。

LightGBM在xgboost的基础上进行了很多的优化， 可以看成是XGBoost的升级加强版，

- 它延续了xgboost的那一套集成学习的方式，
- 但是它更加关注模型的训练速度，相对于xgboost， 具有训练速度快和内存占用率低的特点。



回顾|xgboost是属于boosting家族，是GBDT算法的一个**工程实现**，

- **xgboost在进行最优分裂点的选择上是先进行预排序，然后对所有特征的所有分裂点计算按照这些分裂点位分裂后的全部样本的目标函数增益**，这样太费时间和空间
  - 预排序的话既需要保存数据的特征值， 还得保存特征排序后的索引



基于xgboost寻找最优分裂点的复杂度，LightGBM的改进：

- 样本的数量 => GOSS(单边梯度抽样算法)选择$a\%, b\%$样本：基于权重平衡数据分布
- 特征数量 => 互斥特征特征捆绑算法
- 分裂点的数量 => 直方图：做差加速



### 1 样本的数量|GOSS(单边梯度抽样算法)选择$a\%, b\%$样本：

背景|基于权重平衡数据分布：==梯度小的样本，训练误差也比较小== => ==可以关注梯度高的样本==

- 想降低残差的效果好，那么样本的梯度应该越大越好

- 但是要是**盲目的直接去掉这些梯度小的数据，这样就会改变数据的分布**



首先将要进行分裂的特征的所有取值按照绝对值大小降序排序(xgboost也进行了排序，但是LightGBM不用保存排序后的结果）

然后拿到前$a\%$的梯度大的样本，和剩下样本的$b\%$(随机选择)，在计算增益时，后面的这$b\%$通过乘上$\frac{1-a}{b}$来放大梯度小的样本的权重。

- 一方面算法将更多的注意力放在训练不足的样本上，
- 另一方面通过乘上权重来防止采样对原始数据分布造成太大的影响。

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210417110059.png)

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210417110204.webp)

梯度小的样本乘上相应的权重之后，我们在基于采样样本的估计直方图中可以==发现Ni的总个数依然是8个==， 虽然6个梯度小的样本中去掉了4个，留下了两个。

但是这2个样本在梯度上和个数上都进行了3倍的放大，所以可以防止采样对原数数据分布造成太大的影响， 这也就是论文里面说的将更多的注意力放在训练不足的样本上的原因。

> PS：小雨姑娘机器学习笔记中的那个例子挺有意思：GOSS的感觉就好像一个公寓里本来住了10个人，感觉太挤了，赶走了6个人，但是剩下的人要分摊他们6个人的房租



=> 在不降低太多精度的同时，**==减少了样本数量==，使得训练速度加快**。



### 2 特征数量|互斥特征特征捆绑算法

背景：==高维度的数据往往是稀疏的== => 启发我们设计一种无损的方法来减少特征的维度

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210417111027.webp)

上面这个过程的时间复杂度其实是$O(\#features^2)$的，

- 因为要遍历特征，
- 每个特征还要遍历所有的簇， 



所以==为了改善效率，可以不建立图，而是将特征按照非零值个数进行排序==，因为更多的非零值的特征会导致更多的冲突，所以跳过了上面的第一步，==直接排序然后第三步分簇==。



捆绑完了之后特征应该如何取值呢？

- 使得不同特征的值分到**簇中的不同bins**里面去，这可以通过在特征值中**加入一个偏置常量**来解决。

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210417112337.png)

利用这种思路，可以通过==对某些特征的取值重新编码==，将多个这样互斥的特征捆绑成为一个新的特征。

- ==@me|编码是一项抽象转换的艺术==

有趣的是，对于类别特征，如果转换成onehot编码，则这些onehot编码后的多个特征相互之间是互斥的，从而可以被捆绑成为一个特征。

因此，对于指定为类别型的特征，LightGBM可以直接将**每个类别取值和一个bin关联**，从而自动地处理它们，而无需预处理成onehot编码多此一举。



### 3 分裂点的数量|直方图：做差加速

- xgboost在每一层**都得动态构建直方图**， 因为它这个直方图算法不是针对某个特定的feature的，而是所有feature共享一个直方图（每个样本权重的二阶导）
  - 样本被划分到了不同的节点中，二阶导分布也就变了 => 每一层划分完了之后，**下一次依然需要构建这样的直方图**
- lightgbm对每个特征都有一个直方图，这样构建一次就OK
- ![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210417104302.webp)
- 特征被离散化后，找到的并不是很精确的分割点，所以会对结果产生影响。==但在实际的数据集上表明，离散化的分裂点对最终的精度影响并不大，甚至会好一些==。
  - 原因在于decision tree本身就是一个弱学习器，分割点是不是精确并不是太重要
  - 采用Histogram算法会起到正则化的效果，有效地防止模型的过拟合（bin数量决定了正则化的程度，bin越少惩罚越严重，欠拟合风险越高）



### 生长策略|Leaf-wise vs. Level-wise(Xgboost)

Leaf-wise 是一种更为高效的策略，每次**从当前所有叶子中，==找到分裂增益最大的一个叶子==，然后分裂**，如此循环

- Leaf-wise 的缺点是可能会**长出比较深的决策树，产生过拟合**

- 因此 LightGBM 在 Leaf-wise 之上增加了一个最大深度的限制，在保证高效率的同时防止过拟合





### 工程优化

1. 类别特征的支持（这个不算是工程）
   - 0-1编码：产生==样本切分不平衡问题，切分增益会非常小==
     - 如，国籍切分后，会产生是否中国，是否美国等一系列特征，这一系列特征上只有少量样本为 1，大量样本为 0。
     - 这种划分的增益$p_i \cdot log(\frac{1}{p_i})$非常小：
       - 较小的那个拆分样本集，它占总样本的比例太小。无论增益多大，乘以该比例之后几乎可以忽略；
       - 较大的那个拆分样本集，它几乎就是原始的样本集，增益几乎为零；
2. 高效并行
   - 特征并行：不同机器在==不同的特征集合==上分别寻找最优的分割点，然后在机器间同步最优的分割点
     - 减少通信过程的消耗
   - 数据并行：水平划分数据，让不同的机器**先在本地构造直方图**，然后进行全局的合并，最后在合并的直方图上面寻找最优分割点
   - 投票并行：在数据量很大的时候，使用投票并行的方式只合并部分特征的直方图从而达到降低通信量的目的，可以得到非常好的加速效果
3. Cache命中率优化

XGBoost对cache优化不友好：

- 预排序后，特征对梯度的访问是一种**随机访问**，并且不同的特征访问的顺序不一样，无法对cache进行优化
- 为了解决缓存命中率低的问题，XGBoost 提出了**缓存**访问算法进行改进



LightGBM 所使用直方图算法对 Cache 天生友好：

- 首先，所有的特征都采用相同的方式获得梯度（区别于XGBoost的不同特征通过不同的索引获得梯度），只需要**对梯度进行排序并可实现连续访问**，大大提高了缓存命中率；
- 其次，因为**不需要存储行索引**到叶子索引的数组，降低了存储消耗，而且也不存在 Cache Miss的问题







[link: 原文|公众号：Miracle8070](https://mp.weixin.qq.com/s/BIHr5GDunm2U-Szs0Dt32w)

# 1. 导论

今天又带来了一个在数据竞赛中刷分夺冠的必备神兵利器叫做LightGBM， **2017年**由微软提出，是GBDT模型的另一个进化版本， 主要用于解决GBDT在海量数据中遇到的问题，以便更好更快的用于工业实践中。

从 LightGBM 名字我们可以看出其是轻量级（Light）的梯度提升机器（GBM）， 所以面对大规模数据集，它依然非常淡定，跑起来更加轻盈。

谈到竞赛中的神器，我们难免又想到了xgboost， 同是神器， 既然有了一个xgboost， 为啥还要出个Lightgbm呢？所谓既生瑜何生亮， 难道Lightgbm相对于xgboost会有什么优势吗？

那是当然， LightGBM在xgboost的基础上进行了很多的优化， 可以看成是XGBoost的升级加强版，

- 它延续了xgboost的那一套集成学习的方式，
- 但是它更加关注模型的训练速度，相对于xgboost， 具有训练速度快和内存占用率低的特点。

对于Lightgbm， 重点就是两个字：要快，快，还是快！基于这些优势，lightGBM现在不管是在工业界和竞赛界，都混的越来越风生水起，名头大震， 

那么LightGBM到底是如何做到更快的训练速度和更低的内存使用的呢？在xgboost上做出了哪些优化策略呢？LightGBM和xgboost到底有何不同呢？LightGBM又是如何来解决实际问题的呢？



当然既然是基于xgboost进行的优化版本，所以这篇文章依然会看到xgboost的身影，以对比的方式进行学习，有利于加深对算法的理解。由于这个算法我也是刚接触，可能有些地方会理解不当或者有些细节描述不到，欢迎留言指出，这篇文章只是抛砖引玉，明白基本原理之后建议去读原文。



**大纲如下：**

- LightGBM？ 我们还得先从xgboost说起（看看xgboost存在的问题以及可以改进的地方）
- LightGBM的直方图算法（确实和xgboost的不一样）
- LightGBM的两大先进技术（单边梯度抽样GOSS和互斥特征捆绑EFB）
- LightGBM的生长策略（基于最大深度的Leaf-wise）
- LightGBM的工程优化（类别特征支持与并行化）
- LightGBM的实战应用（分为基础使用和调参）



# 2. LightGBM？ 我们还得先从xgboost说起

谈起Lightgbm， 我们已经知道了是xgboost的强化版本

我们在上一篇文章中提到过， xgboost是属于boosting家族，是GBDT算法的一个**工程实现**，

- 在模型的训练过程中是聚焦残差，
- 在目标函数中使用了二阶泰勒展开并加入了正则，
- 在决策树的生成过程中采用了精确贪心的思路，
- 寻找最佳分裂点的时候，使用了预排序算法， 
- 对所有特征都按照特征的数值进行预排序， 然后遍历**所有特征上的所有分裂点位**，计算按照这些候选分裂点位分裂后的**全部样本**的目标函数增益，找到最大的那个增益对应的特征和候选分裂点位，从而进行分裂。这样**一层一层**的完成建树过程， 
- xgboost训练的时候，是通过加法的方式进行训练，也就是每一次通过聚焦残差训练一棵树出来， 最后的预测结果是所有树的加和表示。

上面简单的把xgboost的一些知识给梳理了一下，我们主要是看看xgboost在树生成的过程中，是否存在某些策略上的问题啊！

机智的你可能会说：**xgboost在进行最优分裂点的选择上是先进行预排序，然后对所有特征的所有分裂点计算按照这些分裂点位分裂后的全部样本的目标函数增益**，这样会不会太费时间和空间了啊！ 

哈哈， 果真是一语中的， 还真会带来这样的问题， ==首先就是空间消耗很大==，因为预排序的话既需要保存数据的特征值， 还得保存特征排序后的索引，毕竟这样后续计算分割点的时候快一些，但是这样就需要消耗训练数据两倍的内存。

==其次， 时间上也有很大的开销，在遍历每一个分割点的时候，都需要进行分裂增益的计算，消耗的代价大。==

这时候你又可能说了xgboost不是有个近似分割的算法吗？这个不就对分裂点进行了分桶了，不就可以少遍历一些分裂点了？

嗯嗯， 这个其实就是下面要讲的lightgbm里面的直方图的思路， 所以直方图这个思路在xgboost里面也体现过，不算是lightgbm的亮点了， 这个是会有一些效果，可以减少点计算，但是比较微妙，lightgbm直方图算法进行了更好的优化(具体的下面说)， 比xgboost的这个还要快很多，并且XGB虽然每次只需要遍历几个可能的分裂节点，然后比较每个分裂节点的信息增益，选择最大的那个进行分割，但比较时需要考虑所有样本带来的信息增益，这样还是比较费劲。



所以基于xgboost寻找最优分裂点的复杂度，我总结了下面三点：

- 特征数量 => 特征捆绑
- 分裂点的数量 => 直方图：做差加速
- 样本的数量 => GOSS选择$a\%, b\%$样本：基于权重平衡数据分布



所以如果想在xgboost上面做出一些优化的话，我们是不是就可以从上面的三个角度下手，比如想个办法减少点特征数量啊， 分裂点的数量啊， 样本的数量啊等等。 

哈哈， 微软里面提出lightgbm的那些大佬还真就是这样做的， 

- **Lightgbm里面的直方图算法就是为了减少分裂点的数量， **
- **Lightgbm里面的单边梯度抽样算法就是为了减少样本的数量， **
- **而Lightgbm里面的互斥特征捆绑算法就是为了减少特征的数量。**

**并且后面两个是Lightgbm的亮点所在**。



# 3. LightGBM的直方图算法（Histogram）

==LightGBM的直方图算法是代替Xgboost的预排序算法的==， 

之前我们提到过，Lightgbm是在xgboost的基础上进行的优化，为什么要基于xgboost进行优化呢？

这是因为在GBDT的众多演化算法里面，Xgboost性能应该算是最好的一个，而Lightgbm也算是演变家族中的一员，所以为了凸显其优越性，都是一般和xgboost进行对比。

虽然直方图的算法思路不算是Lightgbm的亮点，毕竟xgboost里面的近似算法也是用的这种思想，但是这种思路对于xgboost的预排序本身也是一种优化，所以Lightgbm本着快的原则，也采用了这种直方图的思想。那么直方图究竟在做什么事情呢？



直方图算法说白了就是

- 把==连续的浮点特征离散化为k个整数==（也就是分桶bins的思想）， 比如[0, 0.1) ->0, [0.1, 0.3)->1。
- 并根据特征所在的bin对其进行**梯度累加和个数统计**，在遍历数据的时候，根据==离散化后的值作为索引==在直方图中累积统计量，当遍历一次数据后，直方图累积了需要的统计量，然后根据直方图的离散值，遍历寻找最优的分割点。

这么说起来，可能还是一脸懵逼， 那么就再来形象的画个图吧（有图就有真相了，哈哈，我们就拿出某一个连续特征来看看如何分桶的）：

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210417103737.png)这样在遍历到该特征的时候，只需要**根据直方图的离散值**，遍历寻找最优的分割点即可，由于bins的数量是远小于样本不同取值的数量的，所以分桶之后要遍历的分裂点的个数会少了很多，这样就可以减少计算量。

基于上面的这个方式，如果是把所有特征放到一块的话，应该是下面的这种感觉：

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210417103845.png)

这里注意一下，XGBoost 在进行预排序时**只考虑非零值进行加速**，

而 LightGBM 也采用类似策略：只用非零特征构建直方图。

这种离散化分桶思路其实有很多优点的， 

- 首先最明显就是内存消耗的降低，xgboost需要用32位的浮点数去存储特征值， 并用32位的整型去存储索引，而Lightgbm的直方图算法不仅不需要额外存储预排序的结果，而且可以**只保存特征离散化后的值**，而这个值一般用8位整型存储就足够了，内存消耗可以降低为原来的1/8。

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210417103939.webp)

然后在计算上的代价也大幅降低，预排序算法**每遍历一个特征值**就需要计算一次分裂的增益，而Lightgbm直方图算法**只需要计算k次（k可以认为是常数）**，时间复杂度从$O(\#featureValue_nums-1*\#features_num)$优化到$O(\#bin_nums-1*\#features_num)$。而我们知道featureValue_nums >> bin_nums



但是你知道吗？Histogram算法还可以进一步加速。

一个叶子节点的Histogram可以直接由父节点的Histogram和兄弟节点的Histogram做差得到。

一般情况下，构造Histogram需要遍历该叶子上的所有数据，通过该方法，只需要遍历Histogram的k个捅。

速度提升了一倍。

再说一下这个细节， 到底这是啥意思呢？



## 直方图作差加速

当节点分裂成两个时，右边的子节点的直方图其实等于其父节点的直方图减去左边子节点的直方图：

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210417104235.png)

这是为啥啊？看完之后，又一脸懵逼呢？ 

其实在说这么个意思， 举个例子就明白了，

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210417104302.webp)

@me|针对特征x1中的5个样本：特征x2分布如最后一层的第二个图(`1,1,2,1直方图`)

通过这种直方图加速的方式，又可以使得Lightgbm的速度快进一步啦。=> 直方图做差：计算便利



关于直方图算法基本上就是这些了，当然还有很多细节简单的描述一下， Histogram算法并不是完美的。

由于特征被离散化后，找到的并不是很精确的分割点，所以会对结果产生影响。==但在实际的数据集上表明，离散化的分裂点对最终的精度影响并不大，甚至会好一些==。

- 原因在于decision tree本身就是一个弱学习器，分割点是不是精确并不是太重要，
- 采用Histogram算法会起到正则化的效果，有效地防止模型的过拟合（bin数量决定了正则化的程度，bin越少惩罚越严重，欠拟合风险越高）。 

直方图算法可以起到的作用就是可以减小分割点的数量， 加快计算。

如果你要说xgboost不是后面的近似分割算法也进行了分桶吗？为啥会比lightgbm的直方图算法慢这么多呢？emmm, 你还记得xgboost那里的分桶是基于什么吗？ 那个算法叫做Weight Quantile Sketch算法，考虑的是对loss的影响权值，用的每个样本的hi来表示的，相当于基于hi的分布去找候选分割点，这样带来的一个问题就是每一层划分完了之后，**下一次依然需要构建这样的直方图**，毕竟样本被划分到了不同的节点中，二阶导分布也就变了。 

所以xgboost在每一层**都得动态构建直方图**， 因为它这个直方图算法不是针对某个特定的feature的，而是所有feature共享一个直方图（每个样本权重的二阶导）。

==而lightgbm对每个特征都有一个直方图，这样构建一次就OK==， 并且分裂的时候还能通过直方图作差进行加速。

故xgboost的直方图算法是不如lightgbm的直方图算法快的。



# 4. LightGBM的两大先进技术（GOSS & EFB）

到了这里，才是Lightgbm的亮点所在， 下面的这两大技术是Lightgbm相对于xgboost独有的， 分别是单边梯度抽样算法(GOSS)和互斥特征捆绑算法(EFB), 

我们上面说到，GOSS可以减少样本的数量，而EFB可以减少特征的数量，这样就能降低模型分裂过程中的复杂度。

减少样本和减少特征究竟是怎么做到的？



## **4.1 单边梯度抽样算法(GOSS)**

单边梯度抽样算法(Gradient-based One-Side Sampling)是从减少样本的角度出发， **排除大部分权重小的样本**，仅用剩下的样本计算信息增益，它是一种在减少数据和保证精度上平衡的算法。

看到这里你可能一下子跳出来进行反驳了，众所周知，GBDT中没有原始样本的权重，既然Lightgbm是GBDT的变种，应该也没有原始样本的权重，你这里怎么排除大部分权重小的样本？

哈哈，你还别说， 你这样想还真是有点道理的， 我们知道在AdaBoost中，会给每个样本一个权重，然后每一轮之后调大错误样本的权重，让后面的模型更加关注前面错误区分的样本，这时候样本权重是数据重要性的标志（你还记得AdaBoost的这个过程吗？），到了GBDT中， 确实没有一个像Adaboost里面这样的样本权重，理论上说是不能应用权重进行采样的， But, 我们发现啊， **GBDT中每个数据都会有不同的梯度值**， 这个对采样是十分有用的， 即==梯度小的样本，训练误差也比较小==，

说明数据已经被模型学习的很好了，因为GBDT不是聚焦残差吗？在训练新模型的过程中，梯度比较小的样本对于降低残差的作用效果不是太大，所以我们==可以关注梯度高的样本==，这样不就减少计算量了吗？ 

当然这里你可能没有明白为啥梯度小的样本对降低残差效果不大， 那咱可以看看GBDT的这个残差到底是个什么东西。 

我把我xgboost里面的一个图截过来：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210417105127.webp" alt="图片" style="zoom:80%;" />

当然GBDT没有用到二阶导，这个不用管，我们就看上面的一阶导部分，是不是可以发现这个参数其实就是每个样本梯度的一个相反数啊？ 也就是$gradient=(y_i-\hat{y}^{t-1})=-C \cdot g_i$

这个常数不用管， 这样也就是说如果我新的模型==想降低残差的效果好，那么样本的梯度应该越大越好==，所以这就是为啥梯度小的样本对于降低残差的效果不大。也是为啥样本的梯度大小可以反映样本权重的原因，这样说清楚了吧。



但是要是**盲目的直接去掉这些梯度小的数据，这样就会改变数据的分布了啊**，所Lightgbm才提出了单边梯度抽样算法，根据样本的权重信息对样本进行抽样，减少了大量梯度小的样本，但是还能不会过多的改变数据集的分布，这就比较牛了。怎么做的呢？



GOSS 算法**保留了梯度大的样本**，**并对梯度小的样本进行==随机抽样==**，为了不改变样本的数据分布，**在==计算增益时为梯度小的样本引入一个常数进行平衡==**。

首先将要进行分裂的特征的所有取值按照绝对值大小降序排序(xgboost也进行了排序，但是LightGBM不用保存排序后的结果），

然后拿到前$a\%$的梯度大的样本，和剩下样本的$b\%$，在计算增益时，后面的这$b\%$通过乘上$\frac{1-a}{b}$来放大梯度小的样本的权重。

- 一方面算法将更多的注意力放在训练不足的样本上，
- 另一方面通过乘上权重来防止采样对原始数据分布造成太大的影响。

这个地方要注意一下，我看到很多资料描述的并不是那么清晰，如果去看论文的话，这个前$a\%$的样本和剩下样本的$b\%$其实是这样算的， 

- 前$a\%$就是选出$a\% \cdot \#samples$个样本作为梯度大的, 
- $b\%$就是在剩下的样本中==随机选出==$b\%  \cdot \#samples$个样本作为梯度小但是保留下来的样本，

这就是原文：

> ![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210417105719.png)len(I)  -> topN, len(I) -> randN。这里的I代表输入数据

一般讲写这种文章是不太愿意直接截图原文的， 但是这个地方确实好多文章讲的不是那么清晰， 我估计我即使是这样说可能依然一脸懵逼，不知道具体怎么操作，哈哈，好吧， 又得请出我这个灵魂画手看看具体应该怎么操作了，之所以为白话，就是尽量的讲清楚每个细节，看了下面图估计就明白怎么操作了（如果还不明白，那就多看两眼，无它，唯眼熟尔 ;)）：

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210417110059.png)

通过上面，我们就通过采样的方式，选出了我们的样本，两个梯度大的6号和7号，

然后又从剩下的样本里面随机选了2个梯度小的，4号和2号，

这时候我们重点看看基于采样样本的估计直方图长什么样子，毕竟我们从8个里面选出了四个，如果直接把另外四个给删掉的话，这时候会改变数据的分布，但应该怎么做呢？也就是乘上$\frac{1-a}{b}$来放大梯度小的样本的权重到底是怎么算的？看下图：

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210417110204.webp)

梯度小的样本乘上相应的权重之后，我们在基于采样样本的估计直方图中可以==发现Ni的总个数依然是8个==， 虽然6个梯度小的样本中去掉了4个，留下了两个。

但是这2个样本在梯度上和个数上都进行了3倍的放大，所以可以防止采样对原数数据分布造成太大的影响， 这也就是论文里面说的将更多的注意力放在训练不足的样本上的原因。

> PS：小雨姑娘机器学习笔记中的那个例子挺有意思：GOSS的感觉就好像一个公寓里本来住了10个人，感觉太挤了，赶走了6个人，但是剩下的人要分摊他们6个人的房租。（恰不恰当不知道，但是符合我的大白话系列，哈哈，具体的可以看后面的链接）

好了， 单边梯度抽样算法基本上就理清楚了，Lightgbm正是通过这样的方式，在不降低太多精度的同时，**==减少了样本数量==，使得训练速度加快**。



## **4.2 互斥特征捆绑算法(EFB)**

==高维度的数据往往是稀疏的==，这种稀疏性启发我们设计一种无损的方法来减少特征的维度。

通常被捆绑的特征都是互斥的（即特征不会同时为非零值，像one-hot），这样两个特征捆绑起来才不会丢失信息。

如果两个特征并不是完全互斥（部分情况下两个特征都是非零值），==可以用一个指标对特征不互斥程度进行衡量==，称之为冲突比率，当这个值较小时，我们可以选择把不完全互斥的两个特征捆绑，而不影响最后的精度。



到这又一脸懵逼，这又是说的什么鬼？

什么稀疏，互斥，冲突的？如果上面的听不懂，我可以举个比较极端的例子来看一下特征捆绑到底是在干嘛：

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210417110510.webp)

看到上面的这些特征够稀疏了吧（大部分都是0），而每一个特征都只有一个训练样本是非0且都不是同一个训练样本，这样的话特征之间也没有冲突了。

这样的情况就可以把这四个特征捆绑成一个，这样是不是维度就减少了啊。（有没有感觉这种矩阵很眼熟，从右往左的话有没有种`逆one-hot`的味道）



所以互斥特征捆绑算法（Exclusive Feature Bundling）是从减少特征的角度去帮助Lightgbm更快， 它指出如果将一些特征进行融合绑定，则可以降低特征数量。

这样在构建直方图的时候时间复杂度从$O(\#data \cdot \#features)$变成$O(\#data \cdot \#bundles)$, 这里的$\#bundles$指的特征融合后特征包的个数，且$\#bundles << \#features$。这样又可以使得速度加快了，哈哈。

但是针对这个特征捆绑融合，有两个问题需要解决， 毕竟像我上面举得那种极端的例子除了OneHot之后的编码，其实很少见。

- 怎么判定哪些特征应该绑在一起？
- 特征绑在一起之后，特征值应该如何确定呢？

对于问题一：EFB 算法利用特征和特征间的关系构造一个==加权无向图，并将其转换为图着色的问题来求解==，求解过程中采用的贪心策略。

感觉这里如果说成图着色问题的话反而有点难理解了，毕竟这里是加权无向图，而图着色问题可以去百度一下到底是怎么回事，反正觉得还不如直接说过程好理解，所以直接看过程反而简单一些。



其实说白了，捆绑特征就是在干这样的一件事：

- 首先将所有的特征看成图的各个顶点，将**不相互独立**的特征用一条边连起来，边的权重就是两个相连接的特征的总冲突值（也就是这两个特征上不同时为0的样本个数）。
- 然后按照节点的度对特征降序排序， **度越大，说明与其他特征的冲突越大**
- 对于每一个特征， 遍历已有的特征簇，如果发现该特征加入到特征簇中的矛盾数==不超过某一个阈值==，则将该特征加入到该簇中。
  - 如果该特征不能加入任何一个已有的特征簇，则新建一个簇，将该特征加入到新建的簇中。



什么？没明白？ 比如上面画的那个图的例子，会发现这些特征都是相互独立的点，度为0，这样排序之后也发现与其他特征的冲突为0，这样的直接放到一个簇里面就没问题，所以这四个特征直接可以合并。

当然一般没有这么巧的事， 所以我把上面的随便改几个数看看这个过程是什么样子的：

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210417111027.webp)上面这个过程的时间复杂度其实是$O(\#features^2)$的，

- 因为要遍历特征，
- 每个特征还要遍历所有的簇， 

在特征不多的情况下还行，但是如果特征维度很大，就不好使了。

所以==为了改善效率，可以不建立图，而是将特征按照非零值个数进行排序==，因为更多的非零值的特征会导致更多的冲突，所以跳过了上面的第一步，==直接排序然后第三步分簇==。



这样哪些特征捆绑的问题就解决了，下面就是第二个， 捆绑完了之后特征应该如何取值呢？

这里面的一个关键就是**原始特征能从合并的特征中分离出来**， 这是什么意思？

绑定几个特征在同一个bundle里需要保证绑定前的原始特征的值可以在bundle里面进行识别，考虑到直方图算法将连续的值保存为离散的bins，我们可以使得不同特征的值分到**簇中的不同bins**里面去，这可以通过在特征值中**加入一个偏置常量**来解决。



比如，我们把特征A和B绑定到了同一个bundle里面， A特征的原始取值区间[0,10), B特征原始取值区间[0,20), 这样如果直接绑定，那么会发现我从bundle里面取出一个值5， 就分不出这个5到底是来自特征A还是特征B了。

所以我们可以在B特征的取值上加一个常量10转换为[10, 30)，这样绑定好的特征取值就是[0,30),  我如果再从bundle里面取出5， 就一下子知道这个是来自特征A的。这样就可以放心的融合特征A和特征B了。

看下图：

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210417112337.png)特征捆绑算法到这里也就基本上差不多了， 通过EFB，许多排他的特征就被捆绑成了更少的密集特征，**这个大大减少的特征的数量，对训练速度又带来很大的提高**。

利用这种思路，可以通过==对某些特征的取值重新编码==，将多个这样互斥的特征捆绑成为一个新的特征。

- ==@me|编码是一项抽象转换的艺术==

有趣的是，对于类别特征，如果转换成onehot编码，则这些onehot编码后的多个特征相互之间是互斥的，从而可以被捆绑成为一个特征。

因此，对于指定为类别型的特征，LightGBM可以直接将**每个类别取值和一个bin关联**，从而自动地处理它们，而无需预处理成onehot编码多此一举。



# 5. LightGBM的生长策略（Leaf-wise）

上面我们已经整理完了LightGBM是如何在寻找最优分裂点的过程中降低时间复杂度的， 可以简单的回忆一下，我们说xgboost在寻找最优分裂点的时间复杂度其实可以归到三个角度：

- 特征的数量，
- 分裂点的数量
- 样本的数量。

而LightGBM也提出了三种策略分别从这三个角度进行优化，直方图算法就是为了减少分裂点的数量， GOSS算法为了减少样本的数量，而EFB算法是为了减少特征的数量。



那么lightgbm除了在寻找最优分裂点过程中进行了优化，其实在树的生成过程中也进行了优化， 它抛弃了xgboost里面的按层生长(level-wise)， 而是==使用了带有**深度限制**的按叶子生长(leaf-wise)==。



这个有什么好处吗？

好不好处先不谈，首先看看这两种生长方式是怎么回事， XGBoost 在树的生成过程中采用 Level-wise 的增长策略，该策略**遍历一次数据可以同时分裂同一层的叶子，容易进行多线程优化**，也好控制模型复杂度，不容易过拟合。

但实际上Level-wise是一种低效的算法，因为**它不加区分的对待同一层的叶子**，==实际上很多叶子的分裂增益较低，没必要进行搜索和分裂==，因此带来了很多没必要的计算开销(一层一层的走，不管它效果到底好不好)![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210417112631.webp)



Leaf-wise 则是一种更为高效的策略，每次**从当前所有叶子中，==找到分裂增益最大的一个叶子==，然后分裂**，如此循环。

因此同 Level-wise 相比，在分裂次数相同的情况下，**Leaf-wise 可以降低更多的误差，得到更好的精度**。

Leaf-wise 的缺点是可能会**长出比较深的决策树，产生过拟合**。==@me|trick: 深度太大容易过拟合==

因此 LightGBM 在 Leaf-wise 之上增加了一个最大深度的限制，在保证高效率的同时防止过拟合。（最大信息增益的优先， 我才不管层不层呢）

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210417112707.webp)所以看到这里应该知道Leaf-wise的优势了吧， Level-wise的做法会产生一些低信息增益的节点，浪费运算资源，但是这个对于防止过拟合挺有用。

而Leaf-wise能够追求更好的精度，降低误差，但是会带来过拟合问题。

那你可能问，那为啥还要用Leaf-wise呢？过拟合这个问题挺严重鸭！ 

但是人家能提高精度啊，哈哈，哪有那么十全十美的东西， 并且作者也使用了max_depth来控制树的高度。

其实敢用Leaf-wise还有一个原因就是Lightgbm在做数据合并，直方图和GOSS等各个操作的时候，==其实都有天然正则化的作用，所以作者感觉在这里使用Leaf-wise追求高精度是一个不错的选择。==



# 6.  LightGBM的工程优化

这部分其实涉及到工程上的一些问题了， 不算是本篇文章的重点内容，毕竟我只是想白话原理部分。

但是也做一个了解吧，毕竟Lightgbm提出的初衷就是解决工程上的问题，不过后面这些我主要是参考的一些其他资料，因为**着实没在工程上进行使用过**，参考资料都会在后面给出，具体的可以去那里面看。

工程优化这部分主要涉及到了三个点：

1. 类别特征的支持（这个不算是工程）
2. 高效并行
3. Cache命中率优化

## **6.1 支持类别特征**

首先从第一个点开始，**LightGBM是第一个==直接支持类别特征==的GBDT工具**。

我们知道大多数机器学习工具都无法直接支持类别特征，一般需要把类别特征，通过one-hot 编码，转化到多维的0/1特征，降低了空间和时间的效率。

但对于决策树来说，其实并不推荐使用独热编码，尤其是特征中类别很多，会存在以下问题：

- 会产生==样本切分不平衡问题，切分增益会非常小==。
  - 如，国籍切分后，会产生是否中国，是否美国等一系列特征，这一系列特征上只有少量样本为 1，大量样本为 0。
  - 这种划分的增益$p_i \cdot log(\frac{1}{p_i})$非常小：
    - 较小的那个拆分样本集，它占总样本的比例太小。无论增益多大，乘以该比例之后几乎可以忽略；
    - 较大的那个拆分样本集，它几乎就是原始的样本集，增益几乎为零；
- 影响决策树学习：决策树依赖的是**数据的统计信息**，而独热码编码会把数据切分到零散的小空间上。
  - 在这些零散的小空间上统计信息不准确的，学习效果变差。
  - 本质是因为独热码编码之后的特征的表达能力较差的，特征的预测能力被人为的拆分成多份，每一份与其他特征竞争最优划分点都失败，最终该特征得到的重要性会比实际值低。

==LightGBM 原生支持类别特征==，采用 many-vs-many 的切分方式将类别特征分为两个子集，实现类别特征的最优切分。

假设有某维特征有 k 个类别，则有$2^{(k-1)}-1$种可能，时间复杂度为$O(2^k)$，LightGBM 基于 Fisher 大佬的 《On Grouping For Maximum Homogeneity》实现了$O(k \cdot log_2(k))$的时间复杂度。

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210417120723.webp)

上图左边为基于 one-hot 编码进行分裂，后图为 LightGBM 基于 many-vs-many 进行分裂，右边叶子节点的含义是$X=A$或者$X=C$**放到左孩子**，其余放到右孩子, 

- 右边的切分方法，数据会被切分到两个比较大的空间，进一步的学习也会更好。



其基本思想在于每次分组时都会**根据训练目标**对类别特征进行分类，在枚举分割点之前，

- 先把直方图按照每个类别对应的**label均值**进行排序；
- 然后按照排序的结果依次枚举最优分割点。

看下面这个图：

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210417120853.png)从上面可以看到，$avg(y)$为类别的均值。

当然，这个方法很容易过拟合，所以LightGBM里面还增加了很多对于这个方法的约束和正则化。

实验结果证明，**这个方法可以使训练速度加速8倍**。



## **6.2 支持高效并行**

我们知道，并行计算可以使得速度更快， lightgbm支持三个角度的并行：

- 特征并行，
- 数据并行
- 投票并行。



下面我们一一来看看：

1. 特征并行 

特征并行的主要思想是**不同机器在==不同的特征集合==上分别寻找最优的分割点，然后在机器间同步最优的分割点**。

XGBoost使用的就是这种特征并行方法。

这种特征并行方法有个很大的缺点：就是对数据进行垂直划分，每台机器所含数据不同，然后使用不同机器找到不同特征的最优分裂点，**划分结果需要通过通信告知每台机器**，增加了额外的复杂度。

LightGBM 则不进行数据垂直划分，而是**在每台机器上==保存全部训练数据==，在得到最佳划分方案后可在本地执行划分而减少了不必要的通信**。

具体过程如下图所示。

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210417121104.png)

2. 数据并行 

传统的数据并行策略主要为水平划分数据，让不同的机器**先在本地构造直方图**，然后进行全局的合并，最后在合并的直方图上面寻找最优分割点。

这种数据划分有一个很大的缺点：通讯开销过大。

- 如果使用点对点通信，一台机器的通讯开销大约为$O(\#machine \cdot \#feature \cdot \#bin)$；

- 如果使用集成的通信，则通讯开销为$O(2 \cdot \#feature \cdot \#bin)$。

LightGBM在数据并行中使用**分散规约 (Reduce scatter)** 把直方图合并的任务分摊到不同的机器，降低通信和计算，并利用直方图做差，进一步减少了一半的通信量。

具体过程如下图所示。

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210417121247.png)

3. 投票并行 

基于投票的数据并行则进一步优化数据并行中的通信代价，使通信代价变成常数级别。

在数据量很大的时候，使用投票并行的方式只合并部分特征的直方图从而达到降低通信量的目的，可以得到非常好的加速效果。

具体过程如下图所示。大致步骤为两步：

1. - 本地找出 Top K 特征，并基于**投票筛选**出可能是最优分割点的特征；
   - 合并时只合并每个机器选出来的特征。

2. ![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210417121318.png)



## **6.3 Cache命中率优化**

XGBoost对cache优化不友好，如下图所示。

在预排序后，特征对梯度的访问是一种**随机访问**，并且不同的特征访问的顺序不一样，无法对cache进行优化。

同时，在每一层长树的时候，需要随机访问一个行索引到叶子索引的数组，并且不同特征访问的顺序也不一样，也会造成较大的cache miss。

为了解决缓存命中率低的问题，XGBoost 提出了缓存访问算法进行改进。![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210417121337.png)

而 LightGBM 所使用直方图算法对 Cache 天生友好：

- 首先，所有的特征都采用相同的方式获得梯度（区别于XGBoost的不同特征通过不同的索引获得梯度），只需要**对梯度进行排序并可实现连续访问**，大大提高了缓存命中率；
- 其次，因为**不需要存储行索引**到叶子索引的数组，降低了存储消耗，而且也不存在 Cache Miss的问题。![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210417121354.webp)

# 7. LightGBM的实战应用⭐⭐

Lightgbm实战部分，我们先用Lightgbm做一个波士顿房价预测的任务， 这个任务比较简单，用lightgbm有点大材小用的感觉，但是在这里就是想看看Lightgbm到底应该如何使用，如何训练预测和调参等。

其实在复杂的数据上也是这样的使用方法，而波士顿房价数据集不用过多的数据预处理内容，在sklearn直接有，导入数据直接建立模型即可。

所以这里才考虑使用一个简单的数据集，既能说明问题，也能节省时间，还能节省篇幅。 

然后我们再用sklearn的乳腺癌数据看看lightgbm应该怎么调参。

这两部分称为基本使用和调参技术。



## **7.1 lightgbm的基本使用**

Lightgbm支持两种形式的调用接口：

- 原生形式
- sklearn接口的形式

所以接下来我们用波士顿房价的数据集先来看看这两种接口应该怎么使用：

### 1 原生形式使用lightgbm

```python
import lightgbm as lgb
from sklearn.metrics import mean_squared_error
from sklearn.datasets import load_boston
from sklearn.model_selection import train_test_split
 
boston = load_boston()
data = boston.data
target = boston.target
X_train, X_test, y_train, y_test = train_test_split(data, target, test_size=0.2)
 
# 创建成lgb特征的数据集格式
lgb_train = lgb.Dataset(X_train, y_train)
lgb_eval = lgb.Dataset(X_test, y_test, reference=lgb_train)
 
# 将参数写成字典下形式
params = {
    'task': 'train',
    'boosting_type': 'gbdt',  # 设置提升类型
    'objective': 'regression',  # 目标函数
    'metric': {'l2', 'auc'},  # 评估函数
    'num_leaves': 31,  # 叶子节点数
    'learning_rate': 0.05,  # 学习速率
    'feature_fraction': 0.9,  # 建树的特征选择比例
    'bagging_fraction': 0.8,  # 建树的样本采样比例
    'bagging_freq': 5,  # k 意味着每 k 次迭代执行bagging
    'verbose': 1  # <0 显示致命的, =0 显示错误 (警告), >0 显示信息
}
 
# 训练 cv and train
gbm = lgb.train(params, lgb_train, num_boost_round=20, valid_sets=lgb_eval, early_stopping_rounds=5)
 
# 保存模型到文件
#gbm.save_model('model.txt')
joblib.dump(lgb, './model/lgb.pkl')
 
# 预测数据集
y_pred = gbm.predict(X_test, num_iteration=gbm.best_iteration)
 
# 评估模型
print('The rmse of prediction is:', mean_squared_error(y_test, y_pred) ** 0.5)
```



### 2 sklearn接口形式的Lightgbm

```python
# 加载数据
boston = load_boston()
data = boston.data
target = boston.target
X_train, X_test, y_train, y_test = train_test_split(data, target, test_size=0.2)
 
# 创建模型，训练模型
gbm = lgb.LGBMRegressor(objective='regression', num_leaves=31, learning_rate=0.05, n_estimators=20)
gbm.fit(X_train, y_train, eval_set=[(X_test, y_test)], eval_metric='l1', early_stopping_rounds=5)
 
# 测试机预测
y_pred = gbm.predict(X_test, num_iteration=gbm.best_iteration_)
 
# 模型评估
print('The rmse of prediction is:', mean_squared_error(y_test, y_pred) ** 0.5)
 
# feature importances
print('Feature importances:', list(gbm.feature_importances_))
 
# 网格搜索，参数优化
estimator = lgb.LGBMRegressor(num_leaves=31)
param_grid = {
    'learning_rate': [0.01, 0.1, 1],
    'n_estimators': [20, 40]
}
gbm = GridSearchCV(estimator, param_grid)
gbm.fit(X_train, y_train)
print('Best parameters found by grid search are:', gbm.best_params_)
```



## **7.2 Lightgbm调参⭐**

Lightgbm的参数非常多，有核心参数，学习控制参数，IO参数，目标函数参数，度量参数等很多，

但是我们调参的时候不需要关注这么多，只需要记住常用的关键的一些参数即可，

下面从四个问题的维度整理一些调参的指导：

> - 针对leaf-wise树的参数优化
>
> - - num_leaves:控制了叶节点的数目。它是控制树模型复杂度的主要参数。
>     - 如果是level-wise，则该参数为$2^{depth}$，其中depth为树的深度。
>     - 但是当叶子数量相同时，leaf-wise的树要远远深过level-wise树，非常容易导致过拟合。
>     - 因此应该让num_leaves小于$2^{depth}$。
>     - 在leaf-wise树中，并不存在depth的概念。因为不存在一个从leaves到depth的合理映射。
>   - min_data_in_leaf: **每个叶节点的最少样本数量**。它是处理leaf-wise树的过拟合的重要参数。将它设为较大的值，可以避免生成一个过深的树。但是也可能导致欠拟合。
>   - max_depth：控制了树的最大深度。该参数可以显式的限制树的深度。
>
> - 针对更快的训练速度
>
> - - 通过设置 bagging_fraction 和 bagging_freq 参数来使用 bagging 方法
>   - 通过设置 feature_fraction 参数来**使用特征的子抽样**
>   - 使用较小的 max_bin
>   - 使用 save_binary 在未来的学习过程对数据加载进行加速
>
> - 获得更好的准确率
>
> - - 使用**较大的 max_bin** （学习速度可能变慢）
>   - 使用较小的 learning_rate 和较大的 num_iterations
>   - 使用较大的 num_leaves （可能导致过拟合）
>   - 使用更大的训练数据
>   - 尝试DART
>
> - 缓解过拟合
>
> - - 使用较小的 max_bin， **分桶粗一些**
>   - 使用较小的 num_leaves  **不要在单棵树分的太细**
>   - 使用 lambda_l1, lambda_l2 和 min_gain_to_split 来使用正则
>   - 尝试 max_depth 来避免生成过深的树
>   - 使用 min_data_in_leaf 和 min_sum_hessian_in_leaf， 确保叶子节点有足够多的数据

下面就以一个乳腺癌数据的例子，看看我们应该怎么具体去调参：



LightGBM的调参过程和RF、GBDT等类似，其基本流程如下：

- 首先选择较高的学习率，大概0.1附近，这样是**为了加快收敛的速度**。这对于调参是很有必要的。
- 对决策树基本参数调参
- 正则化参数调参
- 最后降低学习率，这里是为了最后提高准确率



下面具体看看：

### **第一步：学习率和迭代次数**

我们先把学习率先定一个较高的值，这里取 learning_rate = 0.1，其次确定估计器boosting/boost/boosting_type的类型，不过默认都会选gbdt。

迭代的次数，也可以说是残差树的数目，参数名为n_estimators/num_iterations/num_round/num_boost_round。

我们可以先将该参数设成一个较大的数，然后在cv结果中查看最优的迭代次数，具体如代码。



在这之前，我们必须给其他重要的参数一个初始值。

初始值的意义不大，只是为了方便确定其他参数。

下面先给定一下初始值：



以下参数根据具体项目要求定：

```python
'boosting_type'/'boosting': 'gbdt'
'objective': 'binary'
'metric': 'auc'
 
# 以下是选择的初始值
'max_depth': 5     # 由于数据集不是很大，所以选择了一个适中的值，其实4-10都无所谓。
'num_leaves': 30   # 由于lightGBM是leaves_wise生长，官方说法是要小于2^max_depth
'subsample'/'bagging_fraction':0.8           # 数据采样
'colsample_bytree'/'feature_fraction': 0.8   # 特征采样
```



下面用Lightgbm的cv函数确定

```python
import pandas as pd
import lightgbm as lgb
from sklearn.datasets import load_breast_cancer
from sklearn.cross_validation import train_test_split
 
canceData=load_breast_cancer()
X=canceData.data
y=canceData.target
X_train,X_test,y_train,y_test=train_test_split(X,y,random_state=0,test_size=0.2)
params = {    
          'boosting_type': 'gbdt',
          'objective': 'binary',
          'metric': 'auc',
          'nthread':4,
          'learning_rate':0.1,
          'num_leaves':30, 
          'max_depth': 5,   
          'subsample': 0.8, 
          'colsample_bytree': 0.8, 
    }
    
data_train = lgb.Dataset(X_train, y_train)
cv_results = lgb.cv(params, data_train, num_boost_round=1000, nfold=5, stratified=False, shuffle=True, metrics='auc',early_stopping_rounds=50,seed=0)
print('best n_estimators:', len(cv_results['auc-mean']))
print('best cv score:', pd.Series(cv_results['auc-mean']).max())

# 结果：
('best n_estimators:', 188)
('best cv score:', 0.99134716298085424)
```

我们根据以上结果，就可以取n_estimators=188



### **第二步：确定max_depth和num_leaves**

这是提高精确度的最重要的参数。

这里我们引入sklearn里的GridSearchCV()函数进行搜索

```python
from sklearn.grid_search import GridSearchCV

params_test1={'max_depth': range(3,8,1), 'num_leaves':range(5, 100, 5)}
              
gsearch1 = GridSearchCV(estimator = lgb.LGBMClassifier(boosting_type='gbdt',objective='binary',metrics='auc',learning_rate=0.1, n_estimators=188, max_depth=6, bagging_fraction = 0.8,feature_fraction = 0.8), 
                       param_grid = params_test1, scoring='roc_auc',cv=5,n_jobs=-1)
gsearch1.fit(X_train,y_train)
gsearch1.grid_scores_, gsearch1.best_params_, gsearch1.best_score_


# 结果：
([mean: 0.99248, std: 0.01033, params: {'num_leaves': 5, 'max_depth': 3},
  mean: 0.99227, std: 0.01013, params: {'num_leaves': 10, 'max_depth': 3},
  mean: 0.99227, std: 0.01013, params: {'num_leaves': 15, 'max_depth': 3},
 ······
  mean: 0.99331, std: 0.00775, params: {'num_leaves': 85, 'max_depth': 7},
  mean: 0.99331, std: 0.00775, params: {'num_leaves': 90, 'max_depth': 7},
  mean: 0.99331, std: 0.00775, params: {'num_leaves': 95, 'max_depth': 7}],
 {'max_depth': 4, 'num_leaves': 10},
 0.9943573667711598)
```

根据结果，我们取max_depth=4, num_leaves=10



### **第三步：确定min_data_in_leaf和max_bin**

```python
params_test2={'max_bin': range(5,256,10), 'min_data_in_leaf':range(1,102,10)}
              
gsearch2 = GridSearchCV(estimator = lgb.LGBMClassifier(boosting_type='gbdt',objective='binary',metrics='auc',learning_rate=0.1, n_estimators=188, max_depth=4, num_leaves=10,bagging_fraction = 0.8,feature_fraction = 0.8), 
                       param_grid = params_test2, scoring='roc_auc',cv=5,n_jobs=-1)
gsearch2.fit(X_train,y_train)
gsearch2.grid_scores_, gsearch2.best_params_, gsearch2.best_score_
```

这个结果就不显示了，根据结果，我们取min_data_in_leaf=51，max_bin in=15。



### **第四步：确定feature_fraction、bagging_fraction、bagging_freq**

```python
 params_test3={'feature_fraction': [0.6,0.7,0.8,0.9,1.0],
              'bagging_fraction': [0.6,0.7,0.8,0.9,1.0],
              'bagging_freq': range(0,81,10)
}
              
gsearch3 = GridSearchCV(estimator = lgb.LGBMClassifier(boosting_type='gbdt',objective='binary',metrics='auc',learning_rate=0.1, n_estimators=188, max_depth=4, num_leaves=10,max_bin=15,min_data_in_leaf=51), 
                       param_grid = params_test3, scoring='roc_auc',cv=5,n_jobs=-1)
gsearch3.fit(X_train,y_train)
gsearch3.grid_scores_, gsearch3.best_params_, gsearch3.best_score_
```



### **第五步：确定lambda_l1和lambda_l2**

```python
params_test4={'lambda_l1': [1e-5,1e-3,1e-1,0.0,0.1,0.3,0.5,0.7,0.9,1.0],
              'lambda_l2': [1e-5,1e-3,1e-1,0.0,0.1,0.3,0.5,0.7,0.9,1.0]
}
              
gsearch4 = GridSearchCV(estimator = lgb.LGBMClassifier(boosting_type='gbdt',objective='binary',metrics='auc',learning_rate=0.1, n_estimators=188, max_depth=4, num_leaves=10,max_bin=15,min_data_in_leaf=51,bagging_fraction=0.6,bagging_freq= 0, feature_fraction= 0.8), 
                       param_grid = params_test4, scoring='roc_auc',cv=5,n_jobs=-1)
gsearch4.fit(X_train,y_train)
gsearch4.grid_scores_, gsearch4.best_params_, gsearch4.best_score_
```

### **第六步：确定 min_split_gain**

```python
params_test5={'min_split_gain':[0.0,0.1,0.2,0.3,0.4,0.5,0.6,0.7,0.8,0.9,1.0]}
              
gsearch5 = GridSearchCV(estimator = lgb.LGBMClassifier(boosting_type='gbdt',objective='binary',metrics='auc',learning_rate=0.1, n_estimators=188, max_depth=4, num_leaves=10,max_bin=15,min_data_in_leaf=51,bagging_fraction=0.6,bagging_freq= 0, feature_fraction= 0.8,
lambda_l1=1e-05,lambda_l2=1e-05), 
                       param_grid = params_test5, scoring='roc_auc',cv=5,n_jobs=-1)
gsearch5.fit(X_train,y_train)
gsearch5.grid_scores_, gsearch5.best_params_, gsearch5.best_score_
```



### **第七步：降低学习率，增加迭代次数，验证模型**

```python
model=lgb.LGBMClassifier(boosting_type='gbdt',objective='binary',metrics='auc',learning_rate=0.01, n_estimators=1000, max_depth=4, num_leaves=10,max_bin=15,min_data_in_leaf=51,bagging_fraction=0.6,bagging_freq= 0, feature_fraction= 0.8,
lambda_l1=1e-05,lambda_l2=1e-05,min_split_gain=0)
model.fit(X_train,y_train)
y_pre=model.predict(X_test)
print("acc:",metrics.accuracy_score(y_test,y_pre))
print("auc:",metrics.roc_auc_score(y_test,y_pre))
```

这样会发现， 调完参之后， 比使用默认参数的acc，auc都会有所提高。

还有一种是LightGBM的cv函数调参， 这个比较省事，写好代码自动寻优，但是需要调参经验，如何设置一个好的参数范围， 这个不写了，篇幅太长，具体的看最后面那个链接吧。



关于lightgbm的实战部分，先说这么多吧，因为这个实战的东西，说再多也是个经验，遇到不同的问题，不同的数据，得需要不断的尝试，况且我用lightgbm实战的并不太多，经验不足，所以说再多就可能误人子弟了。

就此打住哈哈。



# 8. 总结

到这里终于把Lightgbm说的差不多了，不知不觉依然是整理了这么多，篇幅和xgboost差不多，因为这个算法也是超级的重要，面试的时候也会扣得很细，所以能多整理点还是尽量多整理一些。

这次我重点放到了算法的原理上面，尽量用白话的语言去描述，关于论文里面的算法流程图我可没敢放上来，但具体的细节还是建议去看原文，这次内容挺多，依然是快速回顾一遍。



讲Lightgbm，基本上就是围绕着快进行的，为了实现这个目的，lightgbm基于xgboost做了很多的优化，

- 首先，从寻找最优分裂点上，我们说了直方图算法算法原理，这个可以降低分裂点的数量，
- 然后我们又说了lightgbm的两大亮点技术GOSS和EFB的算法原理， 
  - 前者是为了降低样本的数量，
  - 后者是为了减少特征的数量，

这样从这三个角度lightgbm降低了xgboost在寻找最优分裂点上的复杂度，从而实现了快。



然后Lightgbm又从树的生长策略上对xgboost进行了优化，使用了Leaf-wise实现了高精度。

最后工程上Lightgbm首次支持类别特征，并且在并行方式上也做了很多的优化，然后就是提高了cache的命中率，这些方式都提高了lightgbm的训练速度，所以相比于xgboost，快，更快，越来越快 ;)  

后面作为收尾，依然是给出了一个实战示例，并整理了一些调参技术。



下面就与xgboost对比一下，==总结一下lightgbm的优点==作为收尾， 从内存和速度两方面总结：

1. 内存更小

2. - XGBoost 使用预排序后需要**记录特征值**及**其对应样本的统计值的索引**，而 LightGBM 使用了直方图算法将**特征值转变为 bin 值**，且**不需要记录特征到样本的索引**，将空间复杂度从 O(2*#data) 降低为 O(#bin) ，极大的减少了内存消耗；
   - LightGBM 采用了直方图算法将存储特征值转变为存储 bin 值，降低了内存消耗；
   - LightGBM 在训练过程中采用互斥特征捆绑算法减少了特征数量，降低了内存消耗。

3. 速度更快

4. - LightGBM 采用了直方图算法将遍历样本转变为**遍历直方图**，极大的降低了时间复杂度；
   - LightGBM 在训练过程中采用单边梯度算法**过滤掉梯度小的样本**，减少了大量的计算；
   - LightGBM 采用了基于 **Leaf-wise 算法的增长策略构建树**，减少了很多不必要的计算量；
   - LightGBM 采用优化后的特征并行、数据并行方法加速计算，**当数据量非常大的时候还可以采用投票并行的策略**；
   - LightGBM 对缓存也进行了优化，增加了 Cache hit 的命中率。

好了，lightgbm的故事就先到这里了， 希望能对你有所帮助，本文依然是抛砖引玉， 还是建议去看看原文，毕竟这个算法还是超级重要的，面试的时候也会抠得很细， 不看原文的话有些精华get不到。