分类：二分类 & 多分类

回归：选择连续值中的某个值作为二叉树分裂的阈值



@me|可视化

1 概念|熵 $H=p_i \cdot log(\frac{1}{p_i})=-p_i \cdot log(p_i)$

- 概率小于1，log后自带负号



2 不同情境下的熵

- 样本点的熵
- 样本集：子集中的熵
- 特征的熵：不同特征的数量占比



> 在《数学之美》这本书里，吴军博士曾经告诫过:对于更好地判断一件事物是正确还是错误，唯一方法是引入更多的信息量。
>
> 那我们对于这个判决树，当然==倾向于选择自身不确定性更大的特征，因为自身不确定性越大，一旦被确定下来，引入的信息量就越多==。
>
> 这样的话，对于整个系统判决更加有效。



> 目标变量可以采用一组离散值的树模型称为分类树：在这些树结构中，叶子代表类标签，分支代表连词导致这些类标签的功能。
>
> 目标变量可以采用连续值（通常是实数）的决策树称为回归树。

---

关键点：混乱的数据==正确分类 => 信息熵 减少==

熵与信息增益



阈值分割的原理图：

- 分成两类：但是有不同的决策规则

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210413144440.png" alt="TEST  GRADES  GRADES  27  TEST  GRADES  22 " style="zoom:67%;" />



[link: 公众号|原文链接](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247498894&idx=3&sn=2dd2567aee2152447d1629ed725d89c7&chksm=ea8893fdddff1aebfe252cf0ace0c1243a639433d041d8a31554caefa91139bf59831dae63a2#rd)

决策树则无疑是最重要的经典算法之一

以此为基础，诞生了一大批集成算法，包括Random Forest、Adaboost、**GBDT**、**xgboost**，lightgbm

- 其中xgboost和lightgbm更是当先炙手可热的大赛算法



实际上，决策树算法也是个人最喜爱的算法之一（另一个是Naive Bayes），不仅出于其算法思想直观易懂（相较于SVM而言，简直好太多），更在于其较好的效果和巧妙的设计。



## 导论|现实背景

一个女孩的妈妈给他介绍男朋友：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210420210109.webp" alt="图片" style="zoom:80%;" />

决策树模型：

- 内部节点：表示特征或者属性
- 叶子节点：表示==类标签==

决策树分类过程：

- 用决策树分类，从根结点开始，对实例的某一特征进行测试，根据测试结果，将实例分配到其子结点；
- 这时，每一个子结点对应着该特征的一个取值。
- 如此递归地对实例进行测试并分配，直至达到叶结点。
- 最后将实例分到叶结点的类中。





# 基本思想|switch-case

决策树学习的过程是逐步决策的过程，而决策过程本身可以绘制成一棵树，这里的树是指数据结构与算法中的树。

执行决策树的训练过程其实无非就是以下几步：

- 1 特征选择|拿什么做决策——最优决策特征选取
  - 找一个分类效果或者说区分度较大的特征把数据集进行分开 => 分类效果或者区分度怎么去衡量？
- 2 决策树生成|什么时候停止——决策树分裂的终止条件；决策结果如何——最终叶子节点的取值
  - 构建的时候并不是选择所有的特征进行划分，因为那样的话，每 个叶子节点只对应了一个实例
  - 过拟合问题怎么解决？我们应该分到什么程度的时候就停止，不继续分下去了？
- ==3 决策树剪枝==
  - 训练生成决策树后：万一，我们拿过真的实例来测试，还是过拟合怎么办呢？



> - 我的择偶标准里面：帅不帅、有没有房子、工资高不高、有没有上进心等。这些特征里面，要先把哪个作为根节点，对男人进行分类？这就是特征选择问题
> - 如果我选男人的标准分的太细，那么就有可能分出的树，顺着一条标准找下去，那里只对应一个男的。这样的树就不太好了，因为每个男人肯定不是完全一样的啊，如果真有新的男的来了，我根本就对应不到我的标准里面去，那我还怎么选择？ 这就是构建树的时候，建的太细了，过拟合了。标准只适用于特定的人。
> - 那我要是真的建了这样一棵非常细的树之后怎么办？可以==从底下进行剪枝，太细的标准去掉，变成叶子节点就行了==啊。



## 基本概念

1 纯度

> 1. 集合1：6次都去打篮球
> 2. 集合2：4次去打篮球，2次不去打篮球
> 3. 集合3：3次去打篮球，3次不去打篮球
>
> 按照纯度指标：集合1 > 集合2 > 集合3。因为集合1的分歧最小，集合3的分歧最大。



2 信息熵

表示了信息的不确定度：理解起来就是衡量一组样本的**混乱程度**，==样本越混乱，越不容易做出决定==

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210420211017.png" alt="image-20210420211017264" style="zoom:80%;" />

- p(i|t) 代表了节点 t 为分类 i 的概率
- 当不确定性越大时，它所包含的信息量也就越大，信息熵也就越高

- 信息熵越大，纯度越低。当集合中的所有样本均匀混合时，信息熵最大，纯度最低。这时候也最难做出决定。



3 信息增益：信息熵之差

衡量纯度提高了多少

在得知特征X的信息后，而使得类Y的信息的不确定性减少的程度

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210420211133.webp)



## 示例|打篮球的选择

影响因素：

- 天气
- 温度
- 湿度
- 刮风

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210420211253.png)

- Gain(D，天气) = 0.02
- Gain(D , 温度)=0.128（这个最大）
- Gain(D , 湿度)=0.020
- Gain(D , 刮风)=0.020

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210420211335.webp" alt="图片" style="zoom: 50%;" />



最终：得到完整的决策树

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210420211404.jpeg" alt="图片" style="zoom:50%;" />



衡量不纯度的指标有三种，而每一种不纯度对应一种决策树的生成算法：

- 信息增益（ID3算法，其实上面的构造决策树的步骤就是著名的ID3算法）
- 信息增益比（C4.5算法）
- 基尼指数（CART算法）



## 剪枝

想学习详细步骤的可以参考下面给出的笔记参考《统计学习方法之决策树》 《西瓜书之决策树》

之所以剪枝，是为了防止“过拟合”（Overfitting）现象的发生



一般来说，剪枝可以分为“预剪枝”（Pre-Pruning）和“后剪枝”（Post-Pruning）

- 预剪枝是在决策树构造时就进行剪枝。
  - 方法是在构造的过程中对节点进行评估，如果==对某个节点进行划分，在**验证集**中不能带来准确性的提升==，那么对这个节点进行划分就没有意义，这时就会把当前节点作为叶节点，不对其进行划分。
- 后剪枝就是在生成决策树之后再进行剪枝，
  - 通常会从决策树的叶节点开始，逐层向上对每个节点进行评估。
    - 如果==剪掉这个节点子树==，与保留该节点子树在分类准确性上差别不大，或者剪掉该节点子树，能在验证集中带来准确性的提升，那么就可以把该节点子树进行剪枝。
  - 方法是：用这个节点子树的==叶子节点汇总来替代该节点==，类标记为这个节点子树中最频繁的那个类。







# 从ID3到C4.5，如何作出最优的决策？

首先进入大众视野的决策树是ID3（Iterative Dichotomiser 3，即第三代迭代二叉树），这里称之为第三代，但似乎不存在所谓的第一代或第二代的前置版本；

另外，虽然叫二叉树，但ID3算法生成的决策树确是一个十足的多叉树。那么，ID3算法是基本原理是怎样的呢？



## 1. 经典ID3算法|信息熵增益

**1）如何描述分类准确性|信息熵减少(更有序)**

==从众多潜在的特征中选择一个最优的特征进行分裂。==

所以，这里的问题进一步等价于如何定义“最优”的特征。

在ID3算法中，引入了信息熵原理来反映特征重要性。

这里，信息熵既是一个通信学上的概念，而其中的熵则要追溯到物理学。来源于何处并不重要，关键的是信息熵可以用于表征一个事件的不确定程度。

这里不确定程度用通信学的角度讲，就是蕴含的信息量。

当然，为了使得机器学习的结果尽可能准确，我们当然是希望能==尽可能的通过一个个的特征消除这种不确定性，或者说不希望它有太多(杂乱)的信息量==。



如何理解这里的信息量呢？更具体说，如何通过信息熵来反应信息量？

举个例子：比如看一场足球赛事，

- 如果对阵双方实力悬殊，那么我们一般称之为没什么看点——毫无信息量；

- 而如果双方实力在伯仲之间，则比赛结果则会很有看点——信息量丰富！



这一事件可抽象为如下数学描述：球队A和球队B，各自胜率分别为pA和pB，显然pA+pB=1（假设该赛事不接受平局结果，必须分出胜负），球队实力悬殊意味着pA>>pB或pA<<pB；实力伯仲之间意味着pA≈pB，进而该事件的信息熵可用如下公式计算：

![image-20210413111400039](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210413111400.png)

将上述公式取值随pA在(0, 1)之间变化的取值结果绘制可视化曲线如下：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210413111411.png" alt="图片" style="zoom:80%;" />

上述结论说明，用信息熵可以刻画一个事件的信息量大小。

进一步地，对应到一个机器学习问题，我们的目标就是通过一个个特征将全量样本逐步分裂为多簇小量样本（原全量样本的子集），同时希望这一个个的样本子集内部尽可能的具有小信息量，以期预测结果更具确定性。

所以，为达成这一目标，自然是希望优先选取那些能**尽可能多地降低这一信息量的特征**参与分裂，以期又快又好的达成训练目标。



**2）一层决策|遍历某个特征的所有取值 => 分裂后的信息熵**

将上述过程进行数学描述，即为：设选取**特征F具有K个取值**（特征F为离散型特征），

- 则选取特征F参与决策树训练意味着将原来的样本集分裂为K个子集，
- 在==每个子集内部==独立计算信息熵，

进而以特征F分裂后的条件信息熵为：

- 其中||Fi||为特征F第i个取值的**个数**，而||F||则为特征F的**总数（即样本集总数）**。

![image-20210413111512974](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210413111513.png)

@me|理解公式：注意上述`在每个子集内部独立计算信息熵`

- i=1...K 表示K个分裂的子集
  - $||F_i||$是该子集内第i个取值的个数，||F||是总数
    - 两者之比表示：该子集中，分类正确的比例
  - $H_{F_i}$也是熵(按照上述熵的定义$H=-\sum_{i=1}^K p_i \cdot log_2(p_i)$)：即在子集$F_i$中，也有K个分类的样本集，根据H的定义计算$H_{F_i}$





进而，选取特征F训练带来的信息熵增益（即，带来的不确定性降低）可计算为:

![image-20210413111540095](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210413111540.png)



进而，通过遍历各特征逐一计算各自的信息熵增益，==增益最大的特征就是当前情况下应当首选的特征==。

由于在每一轮对选定的特征都进行全分裂，所以**对于每个分裂后的样本子集中，该特征列即不再存在**。



**3）多层决策|轮询所有特征**

按照上述流程，即可递归完成一颗决策树的训练过程。

那么==既然是一个递归过程，就自然存在递归的终止条件，也就是决策树完成训练终止分裂的条件==。

按照ID3算法的设计，当一颗决策树满足下列**条件之一**时，即终止分裂：

- 所有特征全部用完；
- 分裂后的样本子集是纯的，即**相应分支上的信息熵为0**；
- 达到预定的决策树**分裂深度，即最大深度**；
- 分裂后的样本子集数量少于**预定叶子节点样本数**时；
- 分裂带来的**信息熵增益小于指定增益阈值**时

@me：上述也就是机器学习算法中超参数的含义



经过上述分裂过程，预期将得到形如下图的一棵多叉树，其中第一轮选取特征F参与训练，得到K个子树，最终分裂终止后得到N个叶子节点。

值得指出的是，各叶子节点经历的分裂次数可能是不同的，即**有的分支分裂次数多些**，有的则可能少一些。

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210413112519.png)

**4）最终决策|叶子节点的target占比**

==最后，对于分裂得到的每个叶子节点==，通过统计**该叶子节点中标签占比最多的取值**即为最终预测结果，若存在相同则随机选取其中一个。



以上就是经典的ID3算法基本原理，算法思想简单直观，训练效果也不错，但主要存在以下两大局限性：

- 仅支持离散特征，如果是==特征连续则首先需要离散化处理==；
- 采用信息熵增益选择最优特征参与训练，==在同等条件下，**取值种类多的特征**比取值种类少的特征更占优势==。

针对第二个缺陷，C4.5决策算法给出了解决方案。

- 这种缺陷不是每次都会发生，只是存在一定的概率。
- 在大部分情况下，ID3 都能生成不错的决策树分类。
  - 针对可能发生的缺陷，后人提出了新的算法进行改进。



> 解读|缺陷
>
> 假设我们把样本编号也作为一种属性的话，那么有多少样本，就会对应多少个分支，每一个分支只有一个实例，这时候每一个分支上Entropy(Di)=0，没有混乱度，显然这时候Gain(D,编号) = Entropy(D) - Entropy(Di) =  Entropy(D) - 0 
>
> 显然是最大的(因为已知被减项已经是最小值0了)，那么按照ID3算法的话，会选择这个编号当做第一个分裂点。
> 我们知道，编号这个属性显然是对我们做选择来说没有意义的，出现过拟合不说，编号这个属性对分类任务作用根本就不大。所以这就是ID3算法存在的一个不足之处。

## 2. C4.5决策树算法|信息熵增益比

### 1）采用信息增益比

ID3算法中，在寻找最优特征参与分裂时，会逐一遍历选取能带来最大信息熵增益的特征，而不考虑该特征的自身情况。

这里，特征自身情况是指该特征可能的取值数目和分布情况。

所以在C4.5版本决策树中，最大的改进则在于将分裂准则从信息熵增益变为**信息熵增益比**，即信息熵增益与**特征自身信息熵**的比值。

![image-20210413112637338](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210413112637.png)

其中，分母部分即为选取特征F自身的信息熵。

> 示例|解读：特征自身的信息熵
>
> ![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210420220545.png)
>
> 分母 = -1/7log1/7 * 7 = -log1/7 也就是说类别越多，混乱程度越大，这时候信息增益比也会减小



虽然仅此一处细节改动，但却有效抑制了分裂过程中对取值数目较多特征的偏好，一定程度上保证了决策树训练过程的高效。

另外，==C4.5决策树还有其他比较重要的优化设计，例如**支持特征缺失值的处理以及增加了剪枝策略等**==，此处不做展开。



### 2）采用悲观剪枝

ID3 构造决策树的时候，容易产生过拟合的情况。

在 C4.5 中，会在**决策树构造之后**采用悲观剪枝（PEP），这样可以提升决策树的泛化能力。

悲观剪枝是后剪枝技术中的一种：

- 通过递归估算==每个内部节点的分类错误率==，
- 比较剪枝前后这个节点的分类错误率来决定是否对其进行剪枝。

这种剪枝方法不再需要一个单独的测试数据集。



### 3）离散化|处理连续属性⭐

C4.5 可以处理连续属性的情况，对连续的属性进行离散化的处理。

比如打篮球存在的“湿度”属性，不按照“高、中”划分，而是**按照湿度值**进行计算，那么湿度取什么值都有可能。

- 该怎么选择这个阈值呢，**C4.5 选择具有最高信息增益的划分所对应的阈值。**



### 4）**处理缺失值⭐**

针对数据集不完整的情况，C4.5 也可以进行处理。

假如我们得到的是如下的数据，你会发现这个数据中存在两点问题。

- 第一个问题是，数据集中存在数值缺失的情况，如何进行属性选择？
- 第二个问题是，假设已经做了属性划分，但是样本在这个属性上有缺失值，该如何对样本进行划分？

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210420221119.webp)

我们不考虑缺失的数值，可以得到温度 D={2-,3+,4+,5-,6+,7-}。

- 温度 = 高：D1={2-,3+,4+} ；
- 温度 = 中：D2={6+,7-}；
- 温度 = 低：D3={5-} 。

这里 + 号代表打篮球，- 号代表不打篮球。

- 比如 ID=2 时，决策是不打篮球，我们可以记录为 2-。



针对将属性选择为温度的信息增益为：

- Gain(D′, 温度)=Ent(D′)-0.792=1.0-0.792=0.208 
  - 属性熵 =1.459, 
- 信息增益率 Gain_ratio(D′, 温度)=0.208/1.459=0.1426。
- D′的样本个数为 6，而 D 的样本个数为 7，所以==所占权重比例==为 6/7，所以 Gain(D′，温度) 所占权重比例为 6/7，
- 所以：Gain_ratio(D, 温度)=6/7*0.1426=0.122。

这样即使在温度属性的数值有缺失的情况下，我们依然可以计算信息增益，并对属性进行选择。



对于上面的第二个问题(样本在划分好的属性上有缺失值)，需要考虑权重。

具体的参考《西瓜书之决策树》 这里只给出答案：

- 针对问题一， 就是假如有些样本在某个属性上存在值缺失，那么我计算信息增益的时候，我不考虑这些样本就可以了。但用了多少样本，要在不考虑带缺失值样本的前提下计算的信息增益的基础上，==乘以一个权重==。
- 针对问题二，如果出现样本在该属性上的值缺失， 则把该样本划分到所有的分支里面，但是权重不一样（这个权重就是**每个分支里的节点个数占的总数比例**），这样，如果再往下划分属性，对于该样本来说，**算条件熵的时候，得考虑上他本身的一个权重了**。





通过以上，我们介绍了决策树的两个经典版本：ID3和C4.5

- 其中前者作为决策树首个进入大众视野的版本，引入了信息熵原理参与特征的选择和分裂，明确了决策树终止分裂的条件；
- 而C4.5则在ID3的基础上，优化了信息熵增益分裂准则偏好选择取值较多特征的弊端，改用信息熵增益比的准则进行分裂。





但同时，C4.5版本决策树仍然存在以下弊端：

- 仍然仅支持离散特征的训练
- 生成的决策树是一个多叉树，==从计算机的视角来看不如二叉树简单==；
- 无论是信息熵增益还是增益比，都涉及大量的**对数计算**，计算效率低下；
- 仍然仅可用于分类任务，对于回归预测无能为力。



对此，一个更高级版本的决策树算法应运而生，CART树，也是当前工业界一直沿用的决策树算法。





# CART树|完善：分类 + 回归

CART（Classification And Regression Tree，顾名思义，分类和回归树）决策树的提出正是为了解决ID3和C4.5版本中存在的局限性，包括**不支持连续型特征**、对数计算效率低、多叉树更为复杂以及不能用于回归预测任务。

CART树给出了全部的解决方案：

- 特征选取准则由信息熵改用==gini指数==，
- 并由==多叉改为二叉==(ID3 和 C4.5 算法可以生成二叉树或多叉树)，同时==避免了对数运算==；
- 增加MSE准则，用于回归训练任务



> 例子|解读：分类树+回归树
>
> ![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210420221640.webp)
>
> - 如果我构造了一棵决策树，想要基于数据判断这个人的职业身份，这个就属于分类树，因为是从几个分类中来做选择。
>   - 分类树可以处理离散数据，也就是**数据种类有限的数据**，它输出的是样本的类别
> - 如果是给定了数据，想要预测这个人的年龄，那就属于回归树。
>   - 回归树可以对连续型的数值进行预测，也就是**数据在某个区间内都有取值的可能**，它输出的是一个数值。





## 1）从信息熵到gini指数|解决log计算难

通信学原理，ID3和C4.5均采用信息熵来评判特征的优劣。

再看单个事件信息熵的计算公式：

![image-20210413112927752](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210413112927.png)



容易理解，该信息熵由事件的各种可能取值**对应熵的加和**得到，其中每种取值的熵为其概率与**概率对数值**的乘积得到（由于概率<=1，其对数值<=0），同时为了保证信息熵大于0更易于理解，且信息熵计算结果越大表征更多的信息量，对**最终结果取负数作为最终的信息熵增益**。



那么==为了避免对数运算==而又不影响计算结果的**单调性**，**将log(p)直接用p替代显然是可行的方案**。

此时计算过程即为该事件各种取值概率平方的加和，且加和计算结果仍然是取值越大、信息量越大。

但此时存在的一个问题是最终结果是负数，不符合常规的信息量的理解，所以**进一步将此结果+1**，即得到gini指数计算公式如下：

![image-20210413113049919](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210413113050.png)

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210420222019.webp" alt="图片" style="zoom: 50%;" />

看看怎么算：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210420222127.webp" alt="图片" style="zoom: 67%;" />

这样，我们在选择特征的时候，就可以根据基尼指数最小来选择特征了。

- 不用考虑$H_A(D)$,也就是不用考虑D在A的划分之下本身样本的一个混乱程度了。
- 因为每次都分两个叉，不用担心叉太多影响结果了。



可见，从信息熵演变为gini指数是一个自然的过渡思路，但计算效率将会大大提升。

这里，补充信息熵与gini指数取值的可视化对比曲线，仍以前述的球赛结果概率为例：⭐

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210413113122.png)



当基尼系数越小的时候，说明样本之间的差异性小，不确定程度低。这一点和熵的定义类似

> 补充信息：
>
> 你可能在经济学中听过说基尼系数，它是用来衡量一个国家收入差距的常用指标。当基尼系数大于 0.4 的时候，说明财富差异悬殊。基尼系数在 0.2-0.4 之间说明分配合理，财富差距不大。

分类的过程本身是一个**不确定度降低的过程**，即纯度的提升过程。

所以 CART 算法在构造分类树的时候，会选择基尼系数最小的属性作为属性的划分。



由于CART算法中，只把类别分为两类，所以K=2，二分类问题概率表示：p1(1-p1) + p2(1-p2)





## 2）从多叉树到二叉树|适应计算机数据

在改用gini指数替换信息熵解决了对数运算的问题后，

CART决策树进一步地调整对每个特征的分裂策略：实现从多叉到二叉，并兼容离散型特征和连续型特征：对每轮选定的特征**不再完全执行分裂**，而仅依据特征取值==是否大于某给定阈值==将原数据集分为两类，小于该阈值的为左子树，大于该阈值的则为右子树。



显然，这里阈值的选取将直接决定左右子树分裂的结果好坏，所以**最优阈值需要寻找而不能随机指定**。



直观来看，寻找最优阈值的策略，最简单也是最有效的，就是**遍历**该特征的所有可能阈值取值，以实现所有分裂方案的遍历。

- 具体来说，==首先将该特征的所有取值从小到大排列，==
- 而后逐一选取**==相邻==两个取值**的均值作为阈值（例如，特征有N种取值，则可能的阈值即为N-1个），
- 遍历计算分裂结果的好坏。



正因为这个通过选取阈值将原树节点分为二叉树子树节点的设计，既实现了兼容离散特征和连续特征，也完成了从多叉树到二叉树的过渡，可谓是==CART树中的一个核心设计==。

值得指出，基于CART树中这样的设计，==每轮训练后选定的**特征实际上并未完全使用**，所以在后续训练中仍然可以继续参与分裂，这也是CART树与ID3和C4.5中的一个显著不同。==





## 3）从分类树到回归树|保留连续值

从机器学习视角的理解，分类任务和回归任务的唯一区别即在于预测结果是离散标签还是连续取值。

而二者对决策树训练过程产生影响的是：

- 如何评价特征选取的优劣，
- 以及分裂结束后如何根据叶子节点中样本的表现得到预测结果。



对于第一个影响，即如何评价特征选取优劣的问题，在分类树中通过信息熵或者gini指数来表征分裂后节点中样本表现的纯度（分类结果越接近，信息量越小），

- 那么在回归树中自然可以联想到类似的表征方式：分裂后节点中样本**取值越集中则纯度越高**。

因为在回归任务中样本取值为连续值，所以表达取值的集中程度自然**可联想到用方差来表述**，用机器学习语言描述就是用MSE（均方误差）衡量。



即对于==每轮==训练，

- 1）**对各个特征进行逐一遍历**，
- 2）对**每个特征的==所有可能阈值==进行第二层遍历**，=> ==二叉分裂==

分别计算分裂后的MSE之和，MSE越小意味着该特征越优秀。



对于第二个影响，即根据训练结果如何确定最终回归预测结果的问题，其思路也较为自然：在完成分裂过程后，将==每个叶子节点取值的平均值==作为后续预测过程的回归值。



至此，CART树便完成了对ID3和C4.5的几个核心问题的改进：既可用于分类也可用于回归任务，同时具有较高的训练效率。

==此外，CART树也对剪枝过程做了相应的改进，即引入**代价复杂度剪枝策略**（Cost-Complexity Pruning，CCP）==，此处也不再展开。

### 3.1 创建分类树

> Python 的 sklearn 中，如果我们想要创建 CART 分类树，可以直接使用 DecisionTreeClassifier 这个类。创建这个类的时候，
>
> 默认情况下 criterion 这个参数等于 gini，也就是按照基尼系数来选择属性划分，即默认采用 CART 分类树



代码：给 iris 数据集构造一棵分类决策树

```python
# encoding=utf-8
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score
from sklearn.tree import DecisionTreeClassifier
from sklearn.datasets import load_iris
# 准备数据集
iris=load_iris()
# 获取特征集和分类标识
features = iris.data
labels = iris.target
# 随机抽取33%的数据作为测试集，其余为训练集
train_features, test_features, train_labels, test_labels = train_test_split(features, labels, test_size=0.33, random_state=0)
# 创建CART分类树
clf = DecisionTreeClassifier(criterion='gini')
# 拟合构造CART分类树
clf = clf.fit(train_features, train_labels)
# 用CART分类树做预测
test_predict = clf.predict(test_features)
# 预测结果与测试集结果作比对
score = accuracy_score(test_labels, test_predict)
print("CART分类树准确率 %.4lf" % score)
```

运行结果：CART分类树准确率 0.9600

图示|结果的决策树：

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210420222619.webp)

### 3.2 CART回归树

CART 回归树划分数据集的过程和分类树的过程是一样的，只是回归树得到的预测结果是连续值，而且评判“不纯度”的指标不同。

在 CART 分类树中采用的是基尼系数作为标准，那么在 CART 回归树中，如何评价“不纯度”呢？

- 实际上我们要根据样本的混乱程度，也就是样本的离散程度来评价“不纯度”。

样本的离散程度具体的计算方式是，==先计算所有样本的均值==，然后计算每个样本值到均值的差值。

- 我们假设 x 为样本的个体，均值为$\mu$。
- 为了统计样本的离散程度，我们可以取差值的绝对值，或者方差。
  - 绝对值：$|x-\mu|$
  - 方差：$s=\frac{1}{n}\sum(x-\mu)^2$

这两种方式都可以让我们找到节点划分的方法，通常使用最小二乘偏差的情况更常见一些。



代码示例：波士顿房价数据集

- 该数据集给出了影响房价的一些指标，比如犯罪率，房产税等，最后给出了房价

```python
# encoding=utf-8
from sklearn.metrics import mean_squared_error
from sklearn.model_selection import train_test_split
from sklearn.datasets import load_boston
from sklearn.metrics import r2_score,mean_absolute_error,mean_squared_error
from sklearn.tree import DecisionTreeRegressor
# 准备数据集
boston=load_boston()
# 探索数据
print(boston.feature_names)
# 获取特征集和房价
features = boston.data
prices = boston.target
# 随机抽取33%的数据作为测试集，其余为训练集
train_features, test_features, train_price, test_price = train_test_split(features, prices, test_size=0.33)
# 创建CART回归树
dtr=DecisionTreeRegressor()
# 拟合构造CART回归树
dtr.fit(train_features, train_price)
# 预测测试集中的房价
predict_price = dtr.predict(test_features)
# 测试集的结果评价
print('回归树二乘偏差均值:', mean_squared_error(test_price, predict_price))
print('回归树绝对值偏差均值:', mean_absolute_error(test_price, predict_price))
```

运行结果（每次运行结果可能会有不同）：

> ['CRIM''ZN''INDUS''CHAS''NOX''RM''AGE''DIS''RAD''TAX''PTRATIO''B''LSTAT']
>
> - 回归树二乘偏差均值: 23.80784431137724
> - 回归树绝对值偏差均值: 3.040119760479042

能看到 CART 回归树的使用和分类树类似，只是最后求得的预测值是个连续值



# 对比|分类树 vs. 回归树

**分类树**在每一次分支的时候，穷举每一个特征的每一个阈值，然后按照大于或者小于阈值的方式将其相互分开。这就是分类树的算法。

而**回归树**也十分类似，但是在**每个节点都会得到一个预测值**。

> 以年龄为例，该预测值等于属于这个节点的所有人年龄的平均值。
>
> 分枝时穷举每一个feature的每个阈值找最好的分割点，但衡量最好的标准不再是最大熵，而是最小化均方差--即`（每个人的年龄-预测年龄）^2 的总和 / N`，或者说是每个人的预测误差平方和 除以 N。
>
> 这很好理解，被预测出错的人数越多，错的越离谱，均方差就越大，通过最小化均方差能够找到最靠谱的分枝依据。
>
> 分枝直到每个叶子节点上人的年龄都唯一（这太难了）或者达到预设的终止条件（如叶子个数上限），
>
> 若最终叶子节点上人的年龄不唯一，则以该节点上所有人的平均年龄做为该叶子节点的预测年龄。



分类树用于分类标签值，如晴天/阴天/雾/雨、用户性别、网页是否是垃圾页面；

回归树用于预测实数值，如明天的温度、用户的年龄、网页的相关程度；



两者的区别：

- 分类树的结果不能进行加减运算，晴天+晴天没有实际意义；
- 回归树的结果是预测一个数值，可以进行加减运算，例如 20 岁+3 岁=23 岁。
- ==GBDT 中的决策树是回归树==，预测结果是一个数值
  - 在点击率预测方面常用 GBDT，例如用户点击某个内容的概率。



# 补充|决策树的代码底层实现

构造决策树时， 需要解决的第一个问题就是，当前数据集上的哪个特征在划分数据集时起到决定作用，

- 需要找到这样的特征，把原始数据集划分为几个数据子集， 
- 然后再在剩余的特征里面进 一步划分，依次进行下去， 

所以分下面几个步骤：

## 1）**计算给定数据的香农熵**

```python
def calcShannonEnt(dataset):
    numEntries = len(dataset)     # 样本数量
    labelCounts = {}
    for featVec in dataset:
        currentLabel = featVec[-1]         # 遍历每个样本，获取标签
        if currentLabel notin labelCounts.keys():
            labelCounts[currentLabel] = 0
        labelCounts[currentLabel] += 1
    
    shannonEnt = 0.0
    for key in labelCounts:
        prob = float(labelCounts[key]) / numEntries
        shannonEnt -= prob * math.log(prob, 2)
    
    return shannonEnt
```



## 2）**按照给定的特征划分数据**

```python
# 按照给定特征划分数据集
def splitDataSet(dataset, axis, value):
    retDataSet = []
    for featVec in dataset:
        if featVec[axis] == value:
            reducedFeatVec = featVec[:axis]
            reducedFeatVec.extend(featVec[axis+1:])
            retDataSet.append(reducedFeatVec)
    return retDataSet
```



## 3）**选择最好的数据集划分数据**

```python
def chooseBestFeatureToSplit(dataSet):
    numFeatures = len(dataSet[0]) - 1# 获取总的特征数
    baseEntropy = calcShannonEnt(dataSet)
    bestInfoGain = 0.0
    bestFeature = -1
    
    # 下面开始变量所有特征， 对于每个特征，要遍历所有样本， 根据遍历的样本划分开数据集，然后计算新的香农熵
    for i in range(numFeatures):
        featList = [example[i] for example in  dataSet]   #  获取遍历特征的这一列数据，接下来进行划分
        uniqueVals = set(featList)              # 从列表中创建集合是Python语言得到唯一元素值的最快方法
        newEntropy = 0.0
        for value in uniqueVals:
            subDataSet = splitDataSet(dataSet, i, value)
            prob = len(subDataSet) / float(len(dataSet))
            newEntropy += prob * calcShannonEnt(subDataSet)
        infoGain = baseEntropy - newEntropy
        if (infoGain > bestInfoGain):
            bastInfoGain = infoGain
            bestFeature = i
    
    return bestFeature
```



## 4）递归的创建决策树

```python
# 返回最多的那个标签
def majorityCnt(classList):
    classCount = {}
    for vote in classList:
        if vote notin classCount.keys():
            classCount[vote] = 0
            classCount[vote] += 1
    sortedClassCount = sorted(classCount.values(), reverse=True)
    
    return sortedClassCount[0]

# 递归构建决策树
def createTree(dataSet, labels):
    classList = [example[-1] for example in dataSet]
    if classList.count(classList[0]) == len(classList):   # 类别完全相同则停止划分 这种 (1,2,'yes') (3,4,'yes')
        return classList[0]
    if len(dataSet[0]) == 1:          # 遍历所有特征时，(1, 'yes') (2, 'no')这种形式 返回出现次数最多的类别
        return majorityCnt(classList)
    
    bestFeat = chooseBestFeatureToSplit(dataSet)    # 选择最好的数据集划分方式,返回的是最好特征的下标
    bestFeatLabel = labels[bestFeat]         # 获取到那个最好的特征
    myTree = {bestFeatLabel:{}}        # 创建一个myTree,保存创建的树的信息
    del(labels[bestFeat])          # 从标签中药删除这个选出的最好的特征，下一轮就不用这个特征了
    featValues = [example[bestFeat] for example in dataSet]    # 获取到选择的最好的特征的所有取值
    uniqueVals = set(featValues)
    for value in uniqueVals:
        subLabels = labels[:]
        myTree[bestFeatLabel][value] = createTree(splitDataSet(dataSet, bestFeat, value), subLabels)  # 这是个字典嵌套字典的形式
    
    return myTree
```

这就是ID3算法的底层代码。

可以参考隐形眼镜分类的这个项目：https://github.com/zhongqiangwu960812/MLProjects/tree/master/MachineLearningInAction/DecisionTree



# 实战|决策树Sklearn

到目前为止，sklearn 中只实现了 ID3 与 CART 决策树，所以我们暂时只能使用这两种决策树

- entropy: 基于信息熵，也就是 ID3 算法，实际结果与 C4.5 相差不大；
- gini：默认参数，基于基尼系数。
  - CART 算法是基于基尼系数做属性划分的，所以 criterion=gini 时，实际上执行的是 CART 算法。

```python
DecisionTreeClassifier(class_weight=None, criterion='entropy', max_depth=None,
            max_features=None, max_leaf_nodes=None,
            min_impurity_decrease=0.0, min_impurity_split=None,
            min_samples_leaf=1, min_samples_split=2,
            min_weight_fraction_leaf=0.0, presort=False, random_state=None,
            splitter='best')
```

除了设置 criterion 采用不同的决策树算法外，一般建议使用默认的参数，

- 默认参数不会限制决策树的最大深度，
- 不限制叶子节点数，
- 认为所有分类的权重都相等等

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210420223430.jpeg)





# 总结

决策树是机器学习中的一个经典算法，主要历经了ID3、C4.5和CART树三大版本，算法原理较为直观易懂，改进迭代的过程也容易理解，尤其是以决策树算法为基础更衍生了众多的集成学习算法。

除了本文讲述的基本原理外，决策树的另一大核心就是剪枝策略。



## [决策树的优缺点](https://easyai.tech/ai-definition/decision-tree/)

**优点**

- 决策树**易于理解和解释**，可以可视化分析，容易提取出规则；
- 可以同时处理标称型和数值型数据；
- 比较适合处理**有缺失属性的样本**；
- 能够处理不相关的特征；
- 测试数据集时，**运行速度比较快**；
- 在相对短的时间内能够对大型数据源做出可行且效果良好的结果。



**缺点**

- 容易发生过拟合（==随机森林可以很大程度上减少过拟合==）；
- 容易**忽略数据集中属性的相互关联**；
- 对于那些各类别样本数量不一致的数据，在决策树中，进行属性划分时，不同的判定准则会带来不同的属性选择倾向；
  - 信息增益准则对可取数目较多的属性有所偏好（典型代表ID3算法），
  - 而增益率准则（CART）则对可取数目较少的属性有所偏好，
  - 但CART进行属性划分时候不再简单地直接利用增益率尽心划分，而是采用一种启发式规则）（只要是使用了信息增益，都有这个缺点，如RF）。
- ID3算法计算信息增益时结果偏向数值比较多的特征。



# #参考文献

[link: 公众号|白话机器学习之决策树](https://mp.weixin.qq.com/s?__biz=MzA4ODUxNjUzMQ==&mid=2247485256&idx=1&sn=d686b2f359fb83d0152502f97005adcd&scene=19#wechat_redirect)