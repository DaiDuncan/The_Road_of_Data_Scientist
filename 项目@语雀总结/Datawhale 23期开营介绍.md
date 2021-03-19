学习1.0：信息输出者 & 知识接收者

学习2.0：project/problem？

学习3.0：连接，创造，建构



Datawhale：专注于AI领域的开源组织



# 评审标准

原创性：内容的思考深度

扩展性：思考广度

条理性：排版



# 运营

1 缴纳

2 开营仪式

- 组队

3 学习打卡

- 协作
- 开源社区

4 结营仪式



# 项目介绍

[link: B站项目介绍](https://www.bilibili.com/video/BV1cA411T74x)

- 14：01 区块链编程实践
- 21：09 集成学习（上）
- 28：09 深度推荐模型
- 34：52 心跳信号分类

## 1区块链编程实践

环境：ubuntu, node.js, go

概念：官方文档

工具：solidity语言(不要求太深入)

+多实践，多查多问



## 集成学习（上）⭐

数据科学竞赛的高分选手模型：除了深度学习以外，无一例外地集成学习+模型融合

不懂得模型的底层知识 => 不知道参数的含义

课程内容：

- 推导基础模型
- sklearn应用
- 集成学习



![image-20210314183514358](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210314183514.png)



### 要求

1 算法工程师

公式手推 => sklearn的API查看参数意义



2 跨学科使用

只需要了解建模思路，理清公式脉络后 => 查看sklearn使用API



### 后续|集成学习2

![image-20210314183902330](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210314183903.png)





## 深度推荐模型

经典的深度学习模型

王喆：《深度学习推荐系统》

![image-20210314184056833](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210314184057.png)

DeepCTR：阿里算法工程师的开源包

FunRec：开源项目



@模型原理：

- 模型原论文
- 大量的优秀博客



@代码实现：参考DeepCTR编程风格



+Tensorflow 2.x



## 心跳信号分类

[link: github项目地址](https://github.com/datawhalechina/team-learning-data-mining/tree/master/HeartbeatClassification)

比较熟悉

问题定位：多分类

​	+时间序列

​	+医学大数据

学习周期：2-5h * 15天



建议：

1 理解赛题背景

2 baseline

3 心跳分析的相关算法

4 模型融合



勤学如春起之苗，不见其增，日有所长。 辍学如磨刀之石，不见其损，日有所亏。 ——陶渊明 《劝学》



# 推荐系统⭐

[FunRec：开源项目](https://github.com/datawhalechina/team-learning-rs)

[第一章 推荐系统基础](https://github.com/datawhalechina/team-learning-rs/tree/master/RecommendationSystemFundamentals)

- 1.1 基础推荐算法
  -  [1.1.1 推荐系统概述](https://github.com/datawhalechina/team-learning-rs/blob/master/RecommendationSystemFundamentals/01 概述.md)
  -  [1.1.2 协同过滤](https://github.com/datawhalechina/team-learning-rs/blob/master/RecommendationSystemFundamentals/02 协同过滤.md)
  -  [1.1.3 矩阵分解](https://github.com/datawhalechina/team-learning-rs/blob/master/RecommendationSystemFundamentals/03 矩阵分解.md)
  -  [1.1.4 FM](https://github.com/datawhalechina/team-learning-rs/blob/master/RecommendationSystemFundamentals/04 FM.md)
  -  [1.1.5 GBDT+LR](https://github.com/datawhalechina/team-learning-rs/blob/master/RecommendationSystemFundamentals/06 GBDT%2BLR.md)
- 1.2 基于深度组合的深度推荐算法
  -  [深度学习模型搭建基础](https://github.com/datawhalechina/team-learning-rs/blob/master/DeepRecommendationModel/深度学习推荐系统模型搭建基础.md)
  -  [1.2.1 NeuralCF](https://github.com/datawhalechina/team-learning-rs/blob/master/DeepRecommendationModel/NeuralCF.md)
  -  [1.2.2 Deep Crossing](https://github.com/datawhalechina/team-learning-rs/blob/master/DeepRecommendationModel/DeepCrossing.md)
  -  [1.2.3 PNN](https://github.com/datawhalechina/team-learning-rs/blob/master/DeepRecommendationModel/PNN.md)
  -  [1.2.3 Wide&Deep](https://github.com/datawhalechina/team-learning-rs/blob/master/RecommendationSystemFundamentals/05 Wide%26Deep.md)
  -  [1.2.4 DeepFM](https://github.com/datawhalechina/team-learning-rs/blob/master/DeepRecommendationModel/DeepFM.md)
  -  [1.2.5 Deep&Cross](https://github.com/datawhalechina/team-learning-rs/blob/master/DeepRecommendationModel/DeepCrossing.md)
  -  [1.2.6 NFM](https://github.com/datawhalechina/team-learning-rs/blob/master/DeepRecommendationModel/NFM.md)
- 1.3 深度推荐算法前沿
  -  [1.3.1 AFM](https://github.com/datawhalechina/team-learning-rs/blob/master/DeepRecommendationModel/AFM.md)
  -  [1.3.2 DIN](https://github.com/datawhalechina/team-learning-rs/blob/master/DeepRecommendationModel/DIN.md)
  -  [1.3.3 DIEN](https://github.com/datawhalechina/team-learning-rs/blob/master/DeepRecommendationModel/DIEN.md)
  -  ...





**第二章 推荐系统进阶**

- 2.1 [竞赛实践(天池入门赛-新闻推荐)](https://github.com/datawhalechina/team-learning-rs/tree/master/RecommandNews)
  -  [2.1.1 赛题理解](https://github.com/datawhalechina/team-learning-rs/blob/master/RecommandNews/赛题理解%2BBaseline.ipynb)
  -  [2.1.2 Baseline](https://github.com/datawhalechina/team-learning-rs/blob/master/RecommandNews/赛题理解%2BBaseline.ipynb)
  -  [2.1.3 数据分析](https://github.com/datawhalechina/team-learning-rs/blob/master/RecommandNews/数据分析.ipynb)
  -  [2.1.4 多路召回](https://github.com/datawhalechina/team-learning-rs/blob/master/RecommandNews/多路召回.ipynb)
  -  [2.1.5 特征工程](https://github.com/datawhalechina/team-learning-rs/blob/master/RecommandNews/特征工程.ipynb)
  -  [2.1.6 排序模型](https://github.com/datawhalechina/team-learning-rs/blob/master/RecommandNews/排序模型%2B模型融合.ipynb)
  -  [2.1.7 模型集成](https://github.com/datawhalechina/team-learning-rs/blob/master/RecommandNews/排序模型%2B模型融合.ipynb)
- 2.2推荐系统架构
  -  2.2.1 基础架构
  -  2.2.2 数据处理
  -  2.2.3 特征工程
  -  2.2.4 多路召回
  -  2.2.5 排序模型
  -  2.2.6 模型评估
  -  2.2.7 线上服务
- 2.3 新闻推荐架构实践
  -  计划中...



**第三章 推荐系统应用**

-  信息流推荐
-  视频推荐
-  音乐推荐
-  广告推荐

