## 导读|大纲

1. 理解问题；
2. 理解规则；
3. 尝试特定问题的一些方案和新策略；
4. **花大量的时间尝试**；
5. 使用正确的工具；
6. **合作**
7. 集成；



更加具体地：

1. 理解问题以及**需要优化的函数**；

2. 选择一个算法然后按照下面的流程进行迭代：

3. - 清洗数据；
   - 对数据进行Scale；
   - 特征transformation；
   - 特征derivations推导；
   - 使用交叉验证：
     - 特征选择；
     - 调节算法的超参数；
   - 使用大量的算法并进行实验，探索算法的优点
   - 找到最好的方案，或者对不同的方案进行集成(模型融合)

3. 理解不同的模型并且知道何时使用它们



# 竞赛流程

## 1 了解优化的指标

例如，如果我们的指标是AUC(Area Under the roc Curve)

- 这是一个排序指标；
- 好的案例排在坏的案例前面的一致性/概率；

- 并不是所有的算法都能够直接优化该指标



常见的指标又是多种多样的，例如：

- AUC；
- Classification Accuracy
- precision
- **NDCG**；
- MRMSE；
- MAE；
- **Deviance**;





## 2 选择靠谱的算法

- 在大多数情况下，许多算法都是在**实验之后**才能选到对应的哪一个；
- 谁尝试的多，参数调的细，那么就更有概率赢得比赛；
- 所有**合理的算法**都值得尝试；



## 3 清洗数据

- ==数据的清洗是依赖于算法的==，对于不同的算法，需要的清洗策略都不一样；

- 对于缺失值的处理是非常重要的，特定的算法比其它的更好；

- 在其它的情况，使用某个特定的值对其进行填充会更加有意义；

- 我们需要查找离异值，对于某些模型离异值没太多影响，但是对于其它的模型影响确是巨大的；

- 如何确定最佳的方案？

- - 所有的尝试一遍；
  - ==多读论文，选择论文中靠谱的==；



## 4 数据缩放

- 特定的算法不需要对数据进行缩放；

- 有一些算法需要对数据进行缩放：

- - Max Scaler：将特征处于最大对特征绝对值；
  - Normalization：减去均值处于方差；
  - Conditional scaling：在特定对情况下进行scale(比如医学数据中scale：让所有特征可比较)



## 5 特征转换

- 对于特定的算法对特征进行转化，可以使得模型收敛更快更好；

- 常见的转化方法包括：

- - Log, SQRT, 平滑变量;
  - 对类别特征进行Dummy;
  - 稀疏矩阵压缩数据；
  - 一阶导数：平滑数据
  - WoE(使用目标变量的信息信息，对变量进行转化)
  - 非监督方法降维：SVD, PCA, ISOMAP, KDTREE, 聚类clustering



## 6 特征生成

- 在许多问题中，该问题是最为重要的，举例来说：

- - 文本分类：生产词的字典，做TFIDF转化；
  - Sounds：将声音转化为频域（通过傅立叶变换）
  - 图像：做卷积等 => 提取不同成分
  - 相关性interactions：
  - 其它有意义的特征：例如降维等，某些模型的输出预测作为特征



## 7 模型验证

例子：交叉验证

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210324154401.webp" alt="图片" style="zoom: 67%;" />

## 8  特征选择

- 并非所有的特征对模型都是有帮助的，有些甚至为对模型带来破坏；

  - 没有提供任何信息@多重共线性
  - 只是噪声 => 不可预测

- 常见对特征选择策略有：

- - 特征重要程度
  - 前向选择（with or not进行模型的交叉验证）；
  - 后向选择（with or not进行模型的交叉验证）；
  - 噪音加入noise injection；
  - 混合所有的方案；

- 通常情况下前向的方式是通过cv进行选择的；



## 9 超参优化

- 很多模型的参数是非常重要的，例如树模型的深度等等；
- 寻找最佳参数的方案是==控制所有其它的参数只变动一个==@控制变量法，依据验证集的情况来调整；
- 暴力的方法可以使用Grid Search等；



## 10 训练大量的模型

没有一个模型是适用于所有问题的，所以需要进行大量模型的尝试 => 探索模型的优势

- 线性用LR(Linear regression), 非线性用RF(random forest)
- 使用模型，尝试捕获数据新的特性





## 11 模型集成

- 即使很一般的模型在集成的时候都有可能带来很大的帮助；

- 常见的策略有：

- - 简单平均；
  - Rank平均；
  - 人为设定参数(基于交叉验证)；
  - 几何均值平均；
  - Meta Modeling（stacking, stack generalization）





# #参考文献

[kaggle Top1 Kazanova谈数据竞赛获奖技巧](https://mp.weixin.qq.com/s?__biz=Mzk0NDE5Nzg1Ng==&mid=2247489908&idx=1&sn=19b3b3f0a2b85c70b6c61794b9bf5384&source=41#wechat_redirect)

- [youtube链接|Marios Michailidis](https://www.youtube.com/watch?v=ft7iVUpcsOM)



[link: 完整的介绍链接](https://www.hackerearth.com/blog/developers/winning-tips-machine-learning-competitions-kazanova-current-kaggle-3/)