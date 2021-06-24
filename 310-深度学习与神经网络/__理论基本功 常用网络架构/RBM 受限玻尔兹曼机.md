# [Udacity讲师](https://www.youtube.com/watch?v=Fkw0_aAtwIw&list=RDCMUCgBncpylJ1kiVaPyP-PZauQ&index=10)

## 1 Introduction

观察不同人(彼此间“独立”)出现的情况：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210620223645.png" alt="image-20210620223645123" style="zoom:67%;" />

## 2 Scores

量化|得分：相关的对象 & 彼此的联系相加

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210620223844.png" alt="image-20210620223844199" style="zoom:67%;" />

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210620223906.png" alt="image-20210620223906447" style="zoom: 67%;" />

负分数：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210620223952.png" alt="image-20210620223952069" style="zoom:67%;" />

分数汇总：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210620224038.png" alt="image-20210620224038571" style="zoom:67%;" />





## 3 Probabilities(通用技巧)

> $v_i$表示可见层
>
> $h_i$表示隐藏层
>
> $W_{ij}$表示联结的权重
>
> 能量E = - Score

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210620224220.png" alt="image-20210620224220000" style="zoom:80%;" />

0 最初的想法|求解Score的占比

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210620224430.png" alt="image-20210620224430307" style="zoom: 67%;" />

1 由于Score相加可能为0(分母为0) => 映射为$e^{x}$ => softmax

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210620224531.png" alt="image-20210620224530986" style="zoom:80%;" />

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210620224612.png" alt="image-20210620224612617" style="zoom:80%;" />

备注：显示结果0可能是数值太小，计算之后近似为0 => 意义：筛选出最突出的特征情况

## 4 Training 

根据样本 => 改变情境出现的概率

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210620224757.png" alt="image-20210620224757023" style="zoom:80%;" />



### 4.1 Contrastive Divergence：

- 增加出现的情境的概率
- 减少所有其他的概率

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210620225142.png" alt="image-20210620225142017" style="zoom:67%;" />

优化|最大似然估计：

- 第一个式子：所有情境出现的概率相乘
- 第二个式子|针对样本总体：连乘转化为log相加 => 相加的结果：以期望的方式呈现(期望的随机变量是`情境中出现的对象的编码`)
- 第三个式子|针对某个情境$v_n$：用来更新相应的权重(E是能量，-E是分数Score)
  - 蓝色：情境$v_n$ 增加的部分
  - 红色：所有情境减少的部分

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210620225255.png" alt="image-20210620225255575" style="zoom: 80%;" />



### 4.2 Small Problem

情境涉及对象越多 => 情境数量指数增加$2^n$(n表示对象数量) => 上述公式中归一化的分母Z，很难求解(维度太大)



### 4.3 Gibbs Sampling

移掉红球/蓝球，用蓝球替换

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210620230331.png" alt="image-20210620230331217" style="zoom:80%;" />

随机增加(目标情境中的**一种**情况)

随机减少(所有情境中的任意**一个**)

可以**多次遍历**数据集

 

**应用结果**：⭐

- A和C同时出现的概率相加(遍历所有隐藏状态：类似对联合概率的某个变量遍历求和 => “消除”该变量)

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210620231100.png" alt="image-20210620231100154" style="zoom:80%;" />





### 4.4 Updating Weights

假设增加/减少(学习率)是0.1

1）增加目标情境之一的概率：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210620231340.png" alt="image-20210620231340210" style="zoom: 80%;" />



2）随机减少一种情境的概率(在上述增加的基础上)：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210620231407.png" alt="image-20210620231407127" style="zoom:80%;" />

3）遍历所有样本数据

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210620231454.png" alt="image-20210620231453980" style="zoom:80%;" />





## 5 Sampling Problems

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210620231754.png" alt="image-20210620231754312" style="zoom:80%;" />

Q1 如何选择“增加”的概率？

基于可见层：选择符合条件的一种情境 => 那么以下四种可能情境$2^2=4$(隐藏层有2个节点)选择哪一种呢？

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210621104342.png" alt="image-20210621104341863" style="zoom:67%;" />



Q2 如何选择“减少”的概率？





### 5.1 Independent Sampling

1 单独可见层对象，或者单独的隐藏层节点 

- 考虑该对象的score(图中紫色数值)
- 考虑与其他对象连接的权重(图中绿色数值)

2 sigmoid函数：将score转化为概率 => 但要注意：不一定指向某个具体的概率事件/随机变量

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210621104951.png" alt="image-20210621104951171" style="zoom:80%;" />

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210621105044.png" alt="image-20210621105044278" style="zoom:80%;" />



<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210621105109.png" alt="image-20210621105109256" style="zoom:80%;" />

备注：上图中$P()=\sigma(-4)=0.018$



### 5.2 Picking Random Samples with Conditions|针对增加概率

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210621105235.png" alt="image-20210621105235572" style="zoom:80%;" />

1 基于数据/可见层，决定了隐藏层的数据分布

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210621105329.png" alt="image-20210621105329025" style="zoom:67%;" />



2 回到Gibbs Sampling

- 选择隐藏层最有可能出现的情况

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210621105447.png" alt="image-20210621105447164" style="zoom:67%;" />



### 5.3 Picking Completely Random Samples|针对减少概率

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210621105613.png" alt="image-20210621105613645" style="zoom:80%;" />

- 从“邻近情境”开始考虑
  - 可见层、隐藏层交替出现
- “随机游走”的次数越多，越偏向于随机 => 该次数是模型的超参数

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210621110223.png" alt="image-20210621110223426" style="zoom:80%;" />

备注：上述蓝色是增加的概率，红色是减少的概率





## Summary

基于数据集Data，通过构建RBM模型，==得到数据分布的概率==

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210621110410.png" alt="image-20210621110410350" style="zoom:80%;" />

应用：基于上述概率分布，生成新的数据集

- me|按照概率随机生成“可见层”的情境

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210621110659.png" alt="image-20210621110659508" style="zoom:80%;" />

应用|领域：生成图像(手写字体)

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210621110833.png" alt="image-20210621110832980" style="zoom: 67%;" />



