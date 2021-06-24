- [什么是线性判别分析？](https://easyai.tech/ai-definition/linear-discriminant-analysis/#waht)
- [百度百科版本](https://easyai.tech/ai-definition/linear-discriminant-analysis/#baidu)
- [维基百科版本](https://easyai.tech/ai-definition/linear-discriminant-analysis/#wiki)

## 原理本质

> 估计easyAI编辑的人没法简单解释原理，加上不重要，干脆就略过了

LDA 是一个很强大的工具，但它是一个==有监督的分类算法==，PCA 是一个无监督的算法，这是和 PCA 的一个很重要的区别。

- [link: 参考计算示例](https://blog.csdn.net/jnulzl/article/details/49894041)



数据投影：同类数据尽可能集中，不同类的数据尽可能分开

![img](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210429213633.png)

正交投影：要求向量x在向量w上的投影？

- 向量d与向量p正交

$$
d^{T} \cdot p = 0 \rightarrow (x-p)^T \cdot p=0 \rightarrow (x-cw)^T(cw)=x^{T}cw-c^2w^Tw=0 
$$

$$
\rightarrow x^Tw=cw^Tw \rightarrow c = \frac{w^Tx}{w^Tw}
$$



<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210429213647.png" alt="img" style="zoom:50%;" />

假设 w 是一个单位向量$w^Tw=1$，则对于任意向量 x，其在 w 上的投影可表示为：$\hat{x}=p=cw=(w^Tx)w$

- 实际上x是n维向量：$\vec{\hat{x}} = (w^T\vec{x})w = aw$
- 由于向量内积为常数：从而将n维下降为1维

1 投影数据的均值

以类别$D_1$为例：$m_1$即投影后的均值
$$
m_1 = \frac{1}{n_1} \sum_{x_i \in D_1} a_i = \frac{1}{n_1} \sum_{x_i \in D_1} w^Tx_i = w^T (\frac{1}{n_1} \sum_{x_i \in D_1} x_i) = w^T \mu_1
$$




2 LDA中的方差定义

以类别$D_1$为例
$$
s_1^2 = \sum_{x_j \in D_1} (a_j - m_i)^2
$$


3 综合

- 类与类之间的均值相差大
- 类内的方差小

结合上述两点：得到优化公式
$$
max_w J(w) = \frac{(m_1-m_2)^2}{s_1 + s_2}
$$
这个公式也叫做 Fisher LDA

![image-20210429215224126](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210429215233.png)

最终结果为：
$$
max_w J(w) = \frac{(w^T Bw)}{(w^TSw)}, S=\sum S_i
$$


4 [求解最优化问题](https://www.sohu.com/a/445501436_500659#:~:text=%E7%BA%BF%E6%80%A7%E5%88%A4%E5%88%AB%E5%88%86%E6%9E%90%E6%98%AF%E4%B8%80,%E4%BD%99%E7%9A%84%E4%B8%80%E7%A7%8D%E7%AE%97%E6%B3%95%E3%80%82)

![image-20210429215635412](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210429215635.png)

注意：$J(w)$作为最大值，是一个常量，用$\lambda$表示

最后得到(如果S是可逆矩阵)：$(S^{-1} B)w=\lambda w$ => 转化为特征值求解的问题



5 针对多分类

- C表示类别的个数

![image-20210429215842180](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210429215909.png)





## 什么是线性判别分析？

逻辑回归是一种传统上仅限于二分类问题的分类算法。

如果您有两个以上的类，则线性判别分析算法是首选的线性分类技术。



LDA与[方差分析](https://zh.wikipedia.org/wiki/方差分析)（ANOVA）和[回归分析](https://zh.wikipedia.org/wiki/回归分析)紧密相关，这两种分析方法也试图通过一些特征或测量值的线性组合来表示一个因变量。

@`Messtechnik 系统检测-匹配滤波器`：最大化信噪比，找到

![image-20210429202935229](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210429202935.png)

它**包含数据的统计属性**，为每个类计算。

对于单个输入变量，这包括：

1. 每个分类的平均值。
2. 在所有类别中计算的方差。

![线性判别分析-LDA](https://easy-ai.oss-cn-shanghai.aliyuncs.com/2019-04-29-132820.jpg)

通过计算每个类的判别值并**对具有最大值的类进行预测**来进行预测。

该技术==假设数据具有高斯分布（钟形曲线），因此最好事先从数据中删除异常值==。

它是分类预测建模问题的一种简单而强大的方法。

 

## 百度百科版本

线性判别分析(linear discriminant analysis，LDA)是对**费舍尔的线性鉴别方法的归纳**，这种方法使用统计学，模式识别和机器学习方法，试图找到两类物体或事件的特征的一个线性组合，以能够特征化或区分它们。

所得的组合可用来作为一个线性分类器，或者，更常见的是，为后续的分类**做降维处理**。

[查看详情](https://baike.baidu.com/item/线性判别分析/22657333)

 

## 维基百科版本

线性判别分析（LDA），正态判别分析（NDA）或判别函数分析是Fisher线性判别式的推广，

这是一种用于统计，模式识别和机器学习的方法，用于找出表征或分离两个或两个特征的线性特征组合。更多类的对象或事件。得到的组合可以用作线性分类器，或者更常见地，用于在稍后分类之前降低维数。

[查看详情](https://en.wikipedia.org/wiki/Linear_discriminant_analysis)