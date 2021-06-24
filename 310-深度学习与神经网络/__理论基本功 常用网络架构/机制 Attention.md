# 一、[通俗理解](https://www.bilibili.com/video/BV1G64y1S7bc/?spm_id_from=333.788.recommend_more_video.2)

## 背景

文本表达有重点：“我爱吃苹果”

- 侧重点可以是“我”
- “爱” => 我的主观情感
- “苹果” => 具体是什么



从复杂的输入信息中心，找出对当前输出最重要的部分。



## Attention结构

- Query: 输入信息

- Key-Value成对出现
  - **源语言、原始文本**等已有的信息@参考搜索：已经建立的索引数据库

1 计算Q-K之间的相关性，得到不同K**对输出**的影响程度

2 再与对应的V相乘 => 得到Q的输出

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210613092559.png" alt="image-20210613092558334" style="zoom: 67%;" />

示例：阅读理解

- Q是问题：“Where is Alice?”
- K-V是原始文本：《Wonder Land》

1 计算Q与K的相关性

找到文本中**最需要注意的部分**



2 利用V得到答案

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210613092809.png" alt="image-20210613092808413" style="zoom:67%;" />



## 变式

### 1 self-attention

只关注输入序列元素之间的关系

- 将**输入序列**直接转化为Q-K-V

- 在内部进行Attention计算

=> 注意文本的内在形式，利于内容的再展示

- 备注|下图细节：四个self-attention的计算都与$x_1$相关，输出对应$Y_1$

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210613093038.png" alt="image-20210613093037794" style="zoom:80%;" />



### 2 Multi-Head attention

在self-attention的基础上：

1 使用**多种变换**生成的Q K V进行运算

2 将他们对相关性的**结论综合起来** => 进一步增强self-attention的效果

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210613093248.png" alt="image-20210613093247598" style="zoom:80%;" />

## 应用

1 应用于Transformer, Bert, GPT

2 最早诞生于CV

- 图片分类
- 图片描述
- 视频描述

指出**注意到的区域**



# 二、[CV中的Attention](https://www.bilibili.com/video/BV1SA41147uA/?spm_id_from=333.788.recommend_more_video.-1)

## 背景

0 人类视觉：选择性地关注所有信息的一部分

- 视网膜的不同部分具有不一样的信息处理能力



1 没有严格的数学定义

传统的局部图像特征提取，滑动窗口方法等都可以看作是一种注意力机制

神经网络中：通常是一个额外的神经网络，**硬性选择输入 => 给输入的不同部分分配不同的权重** => 从而做到重要信息的筛选

- 在空间维度引入attention：比如**inception网络的多尺度**(让并联的卷积层有不同的权重)

  ![image-20210613094054756](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210613094055.png)

- 在通道维度引入attention => 论文`Squeeze-and-Excitation Networks 公司Momenta+牛津大学`



## 论文

- ImageNet 2017竞赛图像分类任务的冠军

- 2018 VCPR会议
- 2019 IEEE|Transactions on Pattern Analysis and Machine Intelligence期刊

### 0 overview

通过自动学习的方式(用另外一个新的神经网络实现)，获取每个特征通道的重要程度 => 给每一个特征通道赋予权重值 => 让神经网络重点关注某些特征通道(颜色表示权重)

![image-20210613094357398](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210613094358.png)



### 1 具体实现

1）Squeeze

- 将Height, Width平面映射成一个实数

![image-20210613094757516](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210613094758.png)
$$
z_c = \mathbf{F}_{sq}(\mathbf{u}_c) = \frac{1}{H \times W} \sum_{i=1}^{H} \sum_{j=1}^{W}u_c(i, j)
$$
2）Excitation|参数转化为权重值

关键点：

- 如何基于参数赋予权重？
- 如何构建通道间的相关性？

3）Scale

![image-20210613095228764](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210613095229.png)





### 2 应用|如何嵌入到主流网络中？

> 概念
>
> inception model: 初始模型
>
> SE block: Squeeze-Excitation模块

![image-20210613095520959](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210613095521.png)

![image-20210613095548873](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210613095549.png)



### 3 效果

本质：用全连接网络，来学习特征的权重

- top-1 err.表示只关注一个错误时
- top-5 err.表示关注5个错误时 => 容错率高

![image-20210613095937518](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210613095938.png)

![image-20210613100026032](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210613100026.png)

