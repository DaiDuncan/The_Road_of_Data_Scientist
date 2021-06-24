## 总结|思路脉络⭐

### 1 Bootstrapping：随机选择一组行/记录

- 超参数：max_samples
- 注意：一行可能被多次选中



### 2 为子树选择特征

- 超参数：max_features



### 3 选择根节点

- 对m个记录（从步骤1开始）进行决策树的拆分，并快速计算度量值
- 指标：选取基尼/熵值最小的随机特征作为根节点
  - 熵增是更加无序、混乱 => 熵最小倾向于分类更加正确



### 4 选择子节点

- 算法执行与步骤2过程相同，选择另一组3个随机特征



### 5 进一步拆分并创建子节点

- 继续选择特征（列）以选择其他子节点，直到出现以下任一情况
  - a）==已用完要拆分的行数==
  - b）拆分后的基尼/熵没有减少

最终结果|使用随机选择的行（记录）和列（特征）创建的第一个迷你决策树：



### 6 迭代|创建更多迷你决策树



### 7 生成森林

- 超参数：n_estimators



### 8 应用|推理

算法将记录传递到每个迷你树中：

- 记录中的值根据每个节点表示的变量遍历迷你树，最终到达一个叶节点
- 基于该记录结束的叶节点的值（在训练期间决定的），该迷你树被分配一个预测输出





---

> 随机森林或随机决策森林是用于分类，回归和其他任务的集成学习方法，其通过在训练时构建多个决策树并输出作为类的模式（分类）或平均预测（回归）的类来操作。
>
> 随机决策森林纠正决策树过度拟合其训练集的习惯。



N个样本，M个特征

1 随机

- 随机n个样本，随机m个特征(m<<M)构成一颗决策树

2 森林

- 多颗决策树构成森林

## 导论|图示

三层决策树：容易过拟合

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210416210446.png" alt="Large Tables  Gender  Age  14  Location  France  Australia  Platform  Android  Android  Android  Job  School  Temp  School  School  Hobby  Tennis  Tennis  Tennis  Videogames  App  LOCATION  PLATFORM  PLA&quot;ORM  If client is male, between 15 and 25,  in the US, on Android, in school,  likes tennis, pizza, but does not like  long walks on the beach, then they are  likely to download Pokemon Go. " style="zoom:80%;" />



二层决策树：减少过拟合的可能性

- 3棵决策树：投票表决

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210416210500.png" alt="Random Forests  PLATFORM  Gender  Age  14  Location  Ftar«e  Platform  Androd  Job  School  Temp  Retired  School  School  Hobby  Videogames  Tennis  Tennis  Videogames  App  e  e  GENDER  e es  Australia Android  ass " style="zoom:80%;" />

---

> 本文将介绍随机森林的基本概念、4 个构造步骤、4 种方式的对比评测、10 个优缺点和 4 个应用方向。

## 什么是随机森林？

随机森林属于 集成学习 中的 Bagging（Bootstrap AGgregation 的简称） 方法。



**决策树 – Decision Tree**

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210417090125.png" alt="图解决策树" style="zoom: 50%;" />

在解释随机森林前，需要先提一下决策树。

决策树是一种很简单的算法，他的解释性强，也符合人类的直观思维。

这是一种基于if-then-else规则的有监督学习算法，上面的图片可以直观的表达决策树的逻辑。





**随机森林 – Random Forest | RF**

![图解随机森林](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210417090514.png)

随机森林是由很多决策树构成的，**不同决策树之间没有关联**。

当我们进行分类任务时，新的输入样本进入，就让森林中的每一棵决策树分别进行判断和分类，每个决策树会得到一个自己的分类结果，**决策树的分类结果中哪一个分类最多**，那么随机森林就会把这个结果当做最终的结果。

 



## 随机森林的优缺点

**优点**

1. 它可以出来很高维度（特征很多）的数据，**并且不用降维，无需做特征选择**
2. 它可以判断特征的重要程度
3. 可以判断出不同特征之间的相互影响
4. 不容易过拟合
5. ==训练速度比较快，容易做成并行方法==
6. 实现起来比较简单
7. 对于不平衡的数据集来说，它可以平衡误差。
8. 如果有很大一部分的特征遗失，仍可以维持准确度。



**缺点**

1. 随机森林已经被证明在某些**噪音较大**的分类或回归问题上会过拟合。
2. 对于有不同取值的属性的数据，==取值划分较多的属性==会对随机森林产生更大的影响，所以随机森林在这种数据上产出的属性权值是不可信的

 



## 随机森林 4 种实现方法对比测试

随机森林是常用的机器学习算法，既可以用于分类问题，也可用于回归问题。

本文对 scikit-learn、Spark MLlib、DolphinDB、XGBoost 四个平台的随机森林算法实现进行对比测试。

评价指标包括==内存占用、运行速度和分类准确性==。

测试结果如下：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210417090740.png" alt="随机森林 4 种实现方法对比测试" style="zoom: 67%;" />



 

## 随机森林的 4 个应用方向

随机森林可以在很多地方使用：

1. 对离散值的分类
2. 对连续值的回归
3. 无监督学习聚类
4. 异常点检测



# 原理|构造随机森林

> **注意**：我们不涉及建模中涉及的预处理或特征工程步骤，只查看当我们使用sklearn的RandomForestClassifier包调用.fit()和.transform()方法时，算法中会发生什么。

```python
sklearn.ensemble.RandomForestClassifier
```

## 数据

![img](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210417091031.png)

注：年龄、血糖水平、体重、性别、吸烟，... f98、f99都是自变量或特征。

- 糖尿病(Diabetic)是我们必须预测的y变量/因变量。



## step1-Bootstrapping

![img](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210417091118.png)

一旦我们将训练数据提供给RandomForestClassifier模型，它（该算法）会随机选择一组行。

这个过程称为Bootstrapping。

对于我们的示例，假设它选择m个记录。

- 注意：要选择的行数可由用户在超参数- max_samples
- 注意：一行可能被多次选中

![img](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210417091135.png)



## step2-为子树选择特征

现在，RF随机选择一个子集的特征/列。

为了简单起见，我们选择了3个随机特征。

- 超参数max_features中你可以控制这个数字

```python
import sklearn.ensemble.RandomForestClassifier

my_rf = RandomForestClassifier(max_features=8)
```



## step3-选择根节点

一旦选择了3个随机特征，算法将对m个记录（从步骤1开始）进行决策树的拆分，并快速计算度量值。

这个度量可以是gini，也可以是熵。

```python
criterion = 'gini' #( or 'entropy' . default= 'gini’ )
```

选取基尼/熵值最小的随机特征作为根节点。

记录在此节点的最佳拆分点进行拆分。

![img](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210417091632.png)

## step4-选择子节点

![img](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210417091555.png)

该算法执行与步骤2过程相同，选择另一组3个随机特征。

![img](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210417091639.png)

它根据条件（gini/熵），选择哪个特征将进入下一个节点/子节点，然后在这里进一步分割。



## step5-进一步拆分并创建子节点

![img](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210417091710.png)

继续选择特征（列）以选择其他子节点，直到出现以下任一情况

- a）==已用完要拆分的行数==
- b）拆分后的基尼/熵没有减少

![img](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210417091735.png)



最终结果|使用随机选择的行（记录）和列（特征）创建的第一个迷你决策树：

![img](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210417091755.png)

## step6-迭代|创建更多迷你决策树

![img](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210417091820.png)

## step7-生成森林

一旦达到默认值100棵树（现在有100棵迷你决策树），模型就完成了fit()过程。

- 超参数：n_estimators

```python
import sklearn.ensemble.RandomForestClassifier

my_rf = RandomForestClassifier(n_estimators=300)
```





## step8-应用|推理

预测一个看不见的数据集（测试数据集）中的值

为了推断（通常称为预测/评分）测试数据，该算法将记录传递到每个迷你树中。

![img](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210417091943.png)

记录中的值根据每个节点表示的变量遍历迷你树，最终到达一个叶节点。

基于该记录结束的叶节点的值（在训练期间决定的），该迷你树被分配一个预测输出。

![img](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210417091952.png)

![img](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210417092008.png)

迭代获得测试集每一行的预测的过程，以达到最终的精度。





## 参考(复习)|构造随机森林的 4 个步骤

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210417090530.png" alt="构造随机森林的4个步骤" style="zoom: 50%;" />

1. 一个样本容量为N的样本，**有放回的抽取**N次，每次抽取1个，最终形成了N个样本。这选择好了的N个样本用来训练一个决策树，作为决策树根节点处的样本。
   - “随机”：对于每棵树都有放回的随机抽取一定量的训练样本
   - 再有放回的随机选取m个特征作为这棵树的分枝的依据：注意`m<<M`(假设有N个样本，M个特征)
   - ![img](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210417092243.png)
2. 当每个样本有M个属性时，在决策树的每个节点需要分裂时，随机从这M个属性中选取出m个属性，满足条件m << M。然后从这m个属性中采用某种策略（比如说信息增益）来**选择1个属性作为该节点的分裂属性**。
3. 决策树形成过程中每个节点都要按照步骤2来分裂（很容易理解，如果下一次该节点选出来的那一个属性是刚刚其父节点分裂时用过的属性，则该节点已经达到了叶子节点，无须继续分裂了）。一直到不能够再分裂为止。注意整个决策树形成过程中没有进行剪枝。
4. 按照步骤1~3建立大量的决策树，这样就构成了随机森林了。



# 原理|拓展：[特征重要程度](https://blog.csdn.net/cg896406166/article/details/83796557)

随机森林真正厉害的地方不在于它通过多棵树进行综合得出最终结果，而是在于通过迭代使得森林中的树不断变得优秀(森林中的树选用更好的特征进行分枝)



## 1 如何选出优秀的特征？

对于每一棵树都有m个特征，要知道某个特征在这个树中是否起到了作用，可以随机改变这个特征的值，使得“这棵树中有没有这个特征都无所谓”，之后比较改变前后的==测试集误差率==，误差率的差距作为该特征在该树中的重要程度(由袋外样本做测试集造成的误差称为**袋外误差**)

在一棵树中对于![m](https://private.codecogs.com/gif.latex?m)个特征都计算一次，就可以算法![m](https://private.codecogs.com/gif.latex?m)个特征在该树中的重要程度。

我们可以计算出所有树中的特征在各自树中的重要程度。



但这只能代表这些特征在树中的重要程度不能代表特征在整个森林中的重要程度。

怎么计算各特征在森林中的重要程度呢？

- 每个特征在多棵数中出现，取这个特征值在多棵树中的重要程度的均值即为该特征在森林中的重要程度。

![MDA(A_{i})=\frac{1}{ntree}\sum_{t=1}^{ntree}(errOOB_{t1}-errOOB_{t2})](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210417092447.gif)

- ntree表示特征$A_i$在森林中出现的次数
- $errOOB_{t1}$表示第t棵树中$A_i$属性值改变之后的袋外误差
- $errOOB_{t2}$表示第t棵树中正常$A_i$值的袋外误差

![img](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210417092641.png)

这样就得到了所有特征在森林中的重要程度。

==将所有的特征按照重要程度排序，去除森林中重要程度低的部分特征==，得到新的特征集。



=> 应用：降维，特征提取



## 2 如何选出最优秀的森林？

按照上面的步骤迭代多次，逐步去除相对较差的特征，==每次都会生成新的森林，直到剩余的特征数为![m](https://private.codecogs.com/gif.latex?m)为止==。(me-思考|去掉M-m个特征？?)

用这剩余的m个特征生成最好的森林。







# 总结

1 随机森林 vs. 神经网络

大家有可能有过这样的经历，辛辛苦苦搭好神经网络，最后预测的准确率还不如随机森林





# #参考文献

[link: 公众号|easyAI](https://easyai.tech/ai-definition/random-forest/)

[link: 博客|图解随机森林 2020.08.30](https://segmentfault.com/a/1190000023825756?utm_source=sf-similar-article)



# [补充|通俗理解](https://www.bilibili.com/video/BV11i4y1F7n4)

![image-20210617090259792](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210617090301.png)

随机选择：

- 部分样本
- 部分特征

原因：

- 不清楚异常值
- 不清楚哪些特征最重要



树与树之间是独立的 => 民主投票：得票多就是预测的分类@KNN方法

优点：

- 树之间是独立的 => 同时训练，高效快速
- 不容易过拟合(部分特征，深度小)
- 能处理特征多的高维数据，而且不需要做特征选择
- 合理训练后准确性很高 => 可作为分类方法的Baseline



集成学习：可以是不同的模型(神经网络可以和决策树共存在同一个集成系统中)

