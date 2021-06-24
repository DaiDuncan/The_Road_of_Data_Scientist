> Udacity适合形成第一印象
>
> 李宏毅适合形成整体框架 + 把握重点

# 零、[李宏毅 21.03.12](https://www.youtube.com/watch?v=OP5HcXJg2Aw)

## 1 情境引导

假设：输入图像大小一样

- 前提要求：图片大小归一化

整体框架：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210611102123.png" alt="image-20210611102123119" style="zoom:67%;" />

彩色图像的数学描述：

(width, height, RGB) => flatten

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210611102216.png" alt="image-20210611102215836" style="zoom:67%;" />



维度诅咒：假设用fully connected network

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210611102325.png" alt="image-20210611102325646" style="zoom:67%;" />



## 2 基于特性建模

### 特性1）局部特征

不需要看整张图片：只需要局部特征 => 避免全连接的维度诅咒

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210611102406.png" alt="image-20210611102406021" style="zoom: 67%;" />



### 特性2）同样特征在不同位置 => 共享权重

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210611103020.png" alt="image-20210611103020007" style="zoom:67%;" />

@例子：大型课程(基础课程：比如数学，计算机编程)

共享权重的前提：receptive field位置不一致(同样的位置：有一组neurous/filters)

- 位置一致：表示输入一致
- 共享权重针对的是：待检测的局部特征(也就是卷积核filter一致)

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210611103256.png" alt="image-20210611103256082" style="zoom:67%;" />

me|直观理解：同样的卷积核(参数是一样的)，遍历所有位置



💡最经典的设置：

1）64个神经元

2）这64个神经元也就是64个filter，单个filter固定参数

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210611103502.png" alt="image-20210611103501490" style="zoom:67%;" />



小结|卷积层 = receptive field + parameter sharing

- Bias比较大 

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210611103715.png" alt="image-20210611103715236" style="zoom:67%;" />

全连接网络通用 VS. 卷积层专用于图像处理



### 视角1 神经元

隐含层的每一个神经元只关注一个receptive field(`3*3*3`, 最后的3表示RGB 3-channels)

- 神经元对receptive field的划分
  - 范围可以重叠，甚至完全相同
- receptive field的大小不一致？(因为模式/局部特征的大小不一样)
- receptive field只考虑RGB的一部分? 比如只有红色的特征
- receptive field是长方形？

=> network corparation

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210611102631.png" alt="image-20210611102630923" style="zoom:67%;" />

💡最经典的设置：

0）常见的receptive field 3\*3\*3D

1）一个神经元关注该receptive field的所有channels

2）通常有一组神经元(比如64)来关注该receptive field

3）stride, overlap, padding

- stride 1或者2：希望彼此有重叠

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210611102827.png" alt="image-20210611102827276" style="zoom:67%;" />





### 视角2 filter

> me|整体理解：=> 理解完全正确
>
> 视角1：固定receptive field，有一组参数 =>  64个神经元
>
> 视角2：64个神经元对应64个卷积核，遍历不同位置
>
> - 每一个神经元 => filter
>
> <img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210611104813.png" alt="image-20210611104813297" style="zoom:67%;" />
>
> 汇总：
>
> - 用卷积核 => **理解**
> - 用神经元 => **运用**

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210611103902.png" alt="image-20210611103901941" style="zoom:67%;" />



filter中的数值就是未知的模型参数，需要通过梯度下降来求解

filter对应的特征：数值最大的地方所在的位置对应相应filter的特征

- 不同的filters的结果 => feature map

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210611104012.png" alt="image-20210611104012051" style="zoom:67%;" />

- 64个filter => feature map有64个channels(上图一个filter对应一层channel)



多个卷积层的维度关系：

- 第二层filter的高度是第一层feature map的层数：都是64
  - 原本通道是RGB
  - 现在通道是Filter的个数

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210611104205.png" alt="image-20210611104204941" style="zoom:67%;" />



filter `3*3`如何表示`5*5`的图像数据

- filter中的一个数值(卷积计算得到的数值)，对应的是`3*3`的局部特征

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210611104518.png" alt="image-20210611104518157" style="zoom:67%;" />

两种视角|neurous 与 filter

- neurous中的权重就是filter中的参数
- 注意：还有bias(所以说filter用来理解，真正用的时候还是回归到神经网络的神经元)

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210611104647.png" alt="image-20210611104646969" style="zoom:67%;" />



不同的位置，可以共享参数

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210611104748.png" alt="image-20210611104747677" style="zoom:67%;" />





### 特性3）池化

pooling /subsampling不影响物体的形状

- 没有参数 => 作用类似激活函数

- max pooling, 算术平均，几何平均

- 范围大小可自定义



结果：通道没变，数据量变小

注意：如果要检测非常精细的东西 => pooling会影响性能

- ==近年来：运算能力强，全部用卷积，不用pooling==

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210322223726.png" alt="image-20210322223725841" style="zoom:67%;" />

整个CNN网络结构：

![image-20210322223657670](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210322223658.png)



## 3 应用：Alpha Go

问题重述：19*19的分类问题 => 选择哪个分类：下一步怎么走

![image-20210611105228358](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210611105228.png)

棋盘的定位：全连接 VS. CNN

- 48个通道：48个不同的状态(本质是局部特征) 

  - ![image-20210611105316776](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210611105317.png)
  - 这个位置是不是要被吃
  - 这个位置边上的状态：连接起来
  - 同样的特征可以在任何位置

  

💡Pooling：5-6页的paper，没有讲网络架构 => 附录提到架构

没有用pooling => 运用之道，存乎一心

- 图像数据：`19*19*48 channels`
  - zero pad: `23*23`
  - k个filter `5*5`,  stride是1
  - ReLU
- 隐藏层2-12：
  - zero pad: `21*21`
  - k个filter `3*3`,  stride是1
  - ReLU
- 最后的隐藏层：
  - filter `1*1`,  stride是1
    - 每个位置有不同的bias
- 输出：softmax

重点参数：滤波器的数量k

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210611105407.png" alt="image-20210611105406605" style="zoom:67%;" />



其他应用：语音，文字处理的文献了解

- 细节：考虑语音、文字本身的特性

[语音](https://dl.acm.org/doi/10.1109/TASLP.2014.2339736)

![image-20210611110015875](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210611110016.png)

[自然语言处理](https://www.aclweb.org/anthology/S15-2079/)

![image-20210611110046820](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210611110047.png)





💡CNN不适合处理图像放大scaling、旋转rotation => data augmentation

- [spatial transformer layer空间转换层](https://youtu.be/SoCywZ1hZak) 



补充：数学讲解OVERFITTING和模型弹性



后续：self attention

- 改变：输入不是一个向量，而是一组向量
  - 每个向量长度都不一样：句子的长度不一样
- 0-1编码：单词间关系独立
- self attention：重点强调某些单词 => 比如主、谓、宾



后续：transformer, bert

transformer: Seq2seq

- input、ouput不一致@语音识别
- @机器翻译
- @语音翻译：语音识别 + 机器翻译
  - 世界上语言太多：有半数没有文字，无法做语音识别



# 一、[Udacity讲师 17.03.21](https://www.youtube.com/watch?v=2-Ol7ZB0MmU&list=RDCMUCgBncpylJ1kiVaPyP-PZauQ&index=4)

## 1 直觉|斜线

0 两种斜线(不同方向)占四个像素：PC视角是一维的flatten

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210610101343.png" alt="image-20210610101343350" style="zoom:67%;" />

1 区分方法：flatten像素，每一个都有不同的权重

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210610101459.png" alt="image-20210610101459118" style="zoom:67%;" />

2 一共16种情况：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210610101624.png" alt="image-20210610101623794" style="zoom:67%;" />



3 权重如何更新优化？

- 有标签的数据：定义标签在PC中的呈现方式
- 卷积核线性变换等运算后的结果与呈现方式的差异 => loss
- 基于loss，利用BP+梯度下降 => 调整卷积核参数/权重

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210610101751.png" alt="image-20210610101750853" style="zoom:67%;" />



## 2 提炼规律|简单符号

0 CNN网络结构：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210610102050.png" alt="image-20210610102049917" style="zoom:80%;" />

1 卷积核：线性变换 => 相关运算

2 卷积 + 池化：

- 池化：提取特征(也起到了降维的结果)
  - 比如数值是4：那么就有与卷积核相应的特征
    - 第一个卷积核特征是下斜线 => 那么数值为4的位置是下斜线

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210610102011.png" alt="image-20210610102011054" style="zoom:67%;" />

结果合并：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210610102430.png" alt="image-20210610102430446" style="zoom:67%;" />





3 全连接层

基向量的不同组合

- 基向量：上斜线，下斜线



卷积+池化的结果：

![image-20210610102930496](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210610102930.png)



Filter：每一个分类有对应的特征 => softmax分类

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210610103049.png" alt="image-20210610103049443" style="zoom:80%;" />

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210610103117.png" alt="image-20210610103117411" style="zoom:80%;" />



4 整体回顾

1）两个不同的卷积核 => 对应两种基本特征：上斜线，下斜线

2）两种卷积核的遍历运算：下图`convolution layer`

![image-20210610103456608](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210610103457.png)



特例展示：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210610104045.png" alt="image-20210610104044929" style="zoom:80%;" />



5 Gradient descent

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210610104211.png" alt="image-20210610104211029" style="zoom:80%;" />

## 3 实例

![image-20210610104243764](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210610104244.png)

1 也许用到一到多个全连接层 => ==与初始对象/分类对象维度相同==

- 关键：大量的图片用来训练这些参数(主要是卷积核/局部特征的参数)



2 人脸识别|不同层的特征是什么？

- 第一层：简单的点，线，圆
- 第二层：一系列点、线、圆组成的局部特征
- 第三层：人脸特征

![image-20210610104510942](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210610104511.png)



# me|总结

0 如何训练？参数是什么？

- 视角1）卷积核的数值？
- 视角2）卷积核固定，权重不同 => 本质上就是卷积核的数值
  - 卷积核：线性变换
  - 激活函数：一般是ReLU => 非线性变换
    - 作用1：保留正数(只需要filter之后为正数的特征：说明特征匹配上了)
    - 作用2：求导简便

me|细节视角：卷积核线性变换，本质上就是AX相乘 => 对A梯度下降：优化参数A，也就是卷积核的数值



1 思路溯源|CNN卷积层

一维数据，就是$\vec{W} \cdot \vec{x} + \vec{b}$

二维数据，对应矩阵的卷积核，本质上与上述的$\vec{W}$一致
