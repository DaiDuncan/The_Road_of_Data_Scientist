## 数据集加载和准备

```python
import numpy as np
import pandas as pd
from sklearn.datasets import load_breast_cancer
import matplotlib.pyplot as plt
from matplotlib import rcParams	#配置参数

rcParams['figure.figsize'] = 14, 7
rcParams['axes.spines.top'] = False
rcParams['axes.spines.right'] = False

# Load data
data = load_breast_cancer()

df = pd.concat([pd.DataFrame(data.data, columns=data.feature_names),pd.DataFrame(data.target, columns=['y'])], axis=1)
df.head()
```

![image-20210203203405181](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210203203405.png)

上述数据中有 30 个特征变量和一个目标变量。所有值都是数值，并且没有缺失的值。

在解决缩放问题之前，还需要执行训练、测试拆分。

```python
from sklearn.preprocessing import StandardScaler
from sklearn.model_selection import train_test_split

### 分离训练集、测试集
X = df.drop('y', axis=1)	
y = df['y']

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.25, random_state=42)

### 数据转换
ss = StandardScaler()
X_train_scaled = ss.fit_transform(X_train)
X_test_scaled = ss.transform(X_test)
```



# 方法1 从模型系数获取特征重要性

检查特征重要性的最简单方法是检查模型的系数。



例如，线性回归和逻辑回归都归结为一个方程，其中将系数(重要性)分配给每个输入值。

简单地说，如果分配的系数是一个大(负或正)数字，它会对预测产生一些影响。

相反，如果系数为零，则对预测没有任何影响。



@逻辑回归

拟合模型后，系数将存储在属性中coef_。

```python
from sklearn.linear_model import LogisticRegression

### 逻辑回归
model = LogisticRegression()
model.fit(X_train_scaled, y_train)
importances = pd.DataFrame(data={
    'Attribute': X_train.columns,
    'Importance': model.coef_[0]
})
importances = importances.sort_values(by='Importance', ascending=False)
# 可视化

plt.bar(x=importances['Attribute'], height=importances['Importance'], color='#087E8B')
plt.title('Feature importances obtained from coefficients', size=20)
plt.xticks(rotation='vertical')
plt.show()
```

![image-20210203203726392](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210203203726.png)

该方法最大特点：**「简单」**、**「高效」**。

系数越大(在正方向和负方向)，越影响预测效果。





# 方法2 从树模型获取重要性

训练==任何树模型==后，你都可以访问 feature_importances 属性。

这是获取功特征重要性的最快方法之一。

```python
from xgboost import XGBClassifier

model = XGBClassifier()
model.fit(X_train_scaled, y_train)

importances = pd.DataFrame(data={
    'Attribute': X_train.columns,
    'Importance': model.feature_importances_
})
importances = importances.sort_values(by='Importance', ascending=False)
# 可视化

plt.bar(x=importances['Attribute'], height=importances['Importance'], color='#087E8B')
plt.title('Feature importances obtained from coefficients', size=20)
plt.xticks(rotation='vertical')
plt.show()
```

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210320084039.webp)



# 方法3 从PCA分数获取特征重要性

主成分分析(PCA)是一种出色的降维技术，也可用于确定特征的重要性。



PCA 不会像前两种技术那样直接显示最重要的功能。

相反，它将返回 N 个主组件，其中 N 等于原始特征的数量。

```python
from sklearn.decomposition import PCA
pca = PCA().fit(X_train_scaled)

# 可视化
plt.plot(pca.explained_variance_ratio_.cumsum(), lw=3, color='#087E8B')
plt.title('Cumulative explained variance by number of principal components', size=20)
plt.show()
```

![image-20210203203921940](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210203203922.png)

这意味着你可以使用前五个主要组件解释源数据集中 90%的方差

```python
loadings = pd.DataFrame(
    data=pca.components_.T * np.sqrt(pca.explained_variance_), 
    columns=[f'PC{i}' for i in range(1, len(X_train.columns) + 1)],
    index=X_train.columns
)

loadings.head()
```

![image-20210203204012293](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210203204012.png)

第一个主要组成部分至关重要。

它只是一个要素，但它解释了数据集中超过 60% 的方差。

从上图中可以看到，它与平均半径特征之间的相关系数接近 0.8，这被认为是强正相关。





让我们可视化所有输入要素与第一个主组件之间的相关性。

```python
pc1_loadings = loadings.sort_values(by='PC1', ascending=False)[['PC1']]
pc1_loadings = pc1_loadings.reset_index()
pc1_loadings.columns = ['Attribute', 'CorrelationWithPC1']

plt.bar(x=pc1_loadings['Attribute'], height=pc1_loadings['CorrelationWithPC1'], color='#087E8B')
plt.title('PCA loading scores (first principal component)', size=20)
plt.xticks(rotation='vertical')
plt.show()
```

![image-20210203204117100](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210203204117.png)



# #参考文献

[Link: 知乎专栏](https://zhuanlan.zhihu.com/p/344944970)