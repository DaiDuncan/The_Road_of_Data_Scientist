[link: 深度学习的四个学习阶段](https://mp.weixin.qq.com/s/HML9hwoaFUWuoy22nbk29w)

![img](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210608203935.png)





## 阶段1：入门级

入门级能够掌握以下技能：

- 能够处理小型数据集

- 理解经典机器学习技术的关键概念

- 理解经典网络DNN(dense)、CNN和RNN

  - > Keras里的dense layer，就是全连结(fully connected)层。
    >
    > Keras里的dropout layper，就是**随机删去了一些连接**，也就相当于是sparse layer

### 数据处理

在入门级使用的数据集很小，可以放入主内存中。

只需几行代码即可应用此类操作。在此阶段数据包括Audio、Image、Time-series和Text等类型。



### 经典机器学习

在深入研究深度学习之前，学习基本机器学习技术是一个不错的选择，其包括回归、聚类、SVM和树模型。



### 网络

掌握常见的网络层，以及相应的神经网络；GAN、AE、VAE、DNN、CNN、RNN 等等。

**在入门阶段，可以优先掌握DNN、CNN和RNN。**



### 理论

没有神经网络就没有深度学习，没有（数学）理论就没有神经网络。

可以通过了解数学符号来开始学习，**可以从矩阵、线性代数和概率论开始你的学习**。





## 阶段2：进阶水平

进阶和入门级之间**没有真正的分界**，进阶水平能够处理更大的数据集，能够使用高级网络处理自定义项模型：

- 处理更大的数据集
- 能够自定义模型完成任务
- 网络模型精度变得更好

### 数据处理

能够处理几GB的数据集，需要自定义数据扩增方法和数据处理函数。

### 自己完成任务

能够根据具体任务完成代码的开发，而不是参考MNIST的教程完成编码。

### 自定义网络

处理自定义项目时，如何**处理数据**？

如何定义自己的网络层？

### 模型训练

掌握**迁移学习的思路**，学会使用预训练权重完成新任务。

并掌握**冻结部分网络层**的方法。

### 深度学习理论

掌握深度学习模型的正向传播和反向传播，特别是链式求导法则。

掌握激活函数和目标函数的作用，能够选择合适的激活函数和目标函数。

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/uoTGEibAZUEiazgGHicVx4KUN7Jz40WOAgL9U24PIagTQhvuUpnU8gKwSmIWfj3vRENIECOzSGb6fQT9bs79CTCEw/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



## 阶段3：熟练水平

与进阶相比你需要掌握更多的数据集处理方法，并掌握==加速模型训练==的方法：

- 大规模数据的处理和存储
- 网络模型的调参
- 无监督学习和强化学习

### 数据处理

需要掌握几百GB数据集的处理，学会**Linux的操作**。

此阶段可能接触到**多模态任务**。



### 无监督项目

开始尝试无监督网络模型的搭建，如自编码器和GAN模型，能够掌握模型原理。

### 模型训练

掌握模型调参的方法和==常见的日志和可视化工具，如TensorBoard的使用==。

掌握学习率的调节方法，如余弦退火。

掌握**多机和混合精度训练**。

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/uoTGEibAZUEiazgGHicVx4KUN7Jz40WOAgLrGZA7Phfh3C7ukbtGdwMlFUR3JdaWAIibKnkQI33A3v2RgsWJNb14icg/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

## 阶段4：专家级

掌握前沿的学术模型的发展，知道自己的兴趣是什么，并**能提出新的模型**：

- 学会使用JAX或DALI处理数据
- 熟悉图神经网络和Transformer模型

