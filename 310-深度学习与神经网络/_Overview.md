[深度学习基本框架|知乎 小龙](https://zhuanlan.zhihu.com/p/339888125)

# 一、整体流程

（1）数据处理

（2）模型设计

（3）训练配置

（4）训练过程

（5）模型保持

![image-20210317142537212](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210317142537.png)



# 二、从纵向角度看整体流程

（1）数据处理：主要包括读取文件，预处理等，封装成函数然后方便后续使用

（2）模型设计：主要包括网络结构和损失函数。网络结构中需要了解和比较的是网络层级激活函数

（3）训练配置：主要包括优化函数和资源配置

（4）训练过程：主要关注评价指标，校验方式，作图

（5）模型保存：模型保存后主要用于预测



<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210317142653.png" alt="image-20210317142653251" style="zoom:150%;" />

备注：主要关注

- +++网络模型设计（多看论文）
- 激活函数👌
- 损失函数👌
- 优化器👌
- 学习率👌
- 评价指标👌
- 生成预测图
- +++多分析各种方式的优缺点和适用场景
- +正则化





# 关注要素

## 0 自适应调整参数

​	1）学习速率@AdaGrad

关键点在于梯度加速变量$r=\sum_{i=1}^{t}g_i^2$，使得参数更新时$\Delta \theta=\delta \cdot \frac{1}{\sqrt{\sum_{i=1}^tg_i^2+\delta}}\cdot g$公式中的$\frac{1}{\sqrt{\sum_{i=1}^tg_i^2+\delta}}$作为一个自适应regularizer

​	

​	2）惩罚项

各种损失函数中，偏离正确值越多，惩罚越严重





## 1 学习速率

分类问题中：损失函数均方差 vs. 交叉熵@ML-损失函数



