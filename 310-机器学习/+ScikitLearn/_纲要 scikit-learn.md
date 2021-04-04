scikit-learn只是最流行的机器学习库==之一==

- 含交叉验证

# [预处理](https://scikit-learn.org/stable/modules/preprocessing.html#preprocessing)

| Preprocessing类 |                  |
| --------------- | ---------------- |
| 方法            | .fit(data)       |
|                 | .transform(data) |
| 属性            | .mean_           |



```python
from sklearn import preprocessing
import numpy as np

from sklearn.datasets import make_classification
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import train_test_split
from sklearn.pipeline import make_pipeline
from sklearn.preprocessing import StandardScaler
```

## 1 标准化Standardization|均值 方差

正常白化处理又叫标准化处理，减去均值除于方差，也就是将数据变为均值为0方差为1的正态分布（注意，这里指的是数据的分布，数值不一定在0-1区间）

![image-20210117201836623](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210117201836.png)



## 2 转换|非线性





## 3 归一化Normalization

把数据大小限定在特定的范围，比如一般都是[0,1]或[-1,1]

![image-20210117201820062](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210117201820.png)



## 4 分类数据的编码





## 5 离散化/分组bins





## 6 处理缺失值





## 7 特征工程|所向是特征





## 8 转换|常用





## 补充|中心化Zero-center

减去均值，方差不做要求



## 补充|正则化Regularization

对模型参数进行正则化处理，如L1, L2正则化





## #小结

使数值都落入到统一的数值范围，把有量纲表达式变成无量纲表达式，使得在建模过程中，各个特征维度无差别对待，消除一些数值尺度差异带来的重要偏见。

经过上述处理的数据，能加快模型训练速度，促进收敛



归一化/标准化有一个很重要的性质：线性变换不会改变原始数据的数值排序

- 本质上是一种线性变换



### 区别：

Zero-center，Standardization和Normalization是**对输入数据**进行处理

Regularization一般是**对模型的参数**进行处理，譬如在Cost Function里面的惩罚项。从而把捕捉到的趋势从局部细微趋势，调整到整体大概趋势。虽然一定程度上的放宽了建模要求，但是能有效防止over-fitting的问题，增加模型准确性。因此 Regularization是针对模型而言









# [监督|分类与回归](https://scikit-learn.org/stable/supervised_learning.html#supervised-learning)

## 1 线性模型





## 2 线性与二次判别分析





## 3 Kernelridge regression = Ridge 回归 + 分类





## 4 SVC支持向量机





## 5 随机梯度下降





## 6 最邻近





## 7 高斯过程





## 8 Cross decomposition





## 9 朴素贝叶斯





## 10 决策树





## 11 集成Ensemble算法

### 1）bagging “民主”平均



### 2）boosting “专家”集中





## 12 算法|多类别 多输出





## 13 特征选择





## 14 Semi-监督学习





## 15 Isotonic回归



## 16 概率校准？？





## 17 神经网络模型(监督)













# [无监督|降维](https://scikit-learn.org/stable/modules/decomposition.html#decompositions)|矩阵分解

## 1 PCA主成分分析





## 2 SVD奇异值分解





## 3 字典学习？？？





## 4 因子分析





## 5 ICA独立成分分析





## 6 非负矩阵分解|NMF NNMF





## 7 Latent Dirichlet Allocation？？？









# [无监督|聚类](https://scikit-learn.org/stable/modules/clustering.html#clustering)

![../_images/sphx_glr_plot_cluster_comparison_0011.png](https://scikit-learn.org/stable/_images/sphx_glr_plot_cluster_comparison_0011.png)



## 1 K-means





## 2 Affinity Propagation



## 3 Mean Shift



## 4 Spectral聚类



## 5 层次Hierarchical聚类



## 6 DBSCAN



## 7 OPTICS



## 8 Birch



## #聚类性能的评价







# [模型选择/评价](https://scikit-learn.org/stable/model_selection.html)

## 1交叉验证



## 2 调整超参数

- Grid Search



## 3 评分



## 4 验证曲线

- 验证曲线
- 学习曲线





# #参考文献

[Link: 官网Tutorial](https://scikit-learn.org/stable/)

[Link: 知乎专栏|Standardization，Normalization 和Regularization的区别](https://zhuanlan.zhihu.com/p/95726841)