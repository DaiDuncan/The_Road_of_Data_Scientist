```python
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt

sns.set_theme(color_codes=True)
tips = sns.load_dataset("tips")
```

seaborn本身并不是统计分析的工具包。要获得与回归模型的拟合相关的定量度量，应使用statsmodels。

但是，seaborn的目标是使通过可视化快速而轻松地浏览数据集变得容易，因为这样做与通过统计表浏览数据集一样重要（如果不重要的话）

- [`regplot()`](https://seaborn.pydata.org/generated/seaborn.regplot.html#seaborn.regplot)

- [`lmplot()`](https://seaborn.pydata.org/generated/seaborn.lmplot.html#seaborn.lmplot)

```python
### 默认95%置信区间
sns.regplot(x="total_bill", y="tip", data=tips);
sns.lmplot(x="total_bill", y="tip", data=tips);
```



[`regplot()`](https://seaborn.pydata.org/generated/seaborn.regplot.html#seaborn.regplot)接受各种格式的`x`和`y`变量

- 简单的numpy数组
- pandasSeries对象
- 作为DataFrame传递给的pandas对象中变量的引用data

[`lmplot()`](https://seaborn.pydata.org/generated/seaborn.lmplot.html#seaborn.lmplot)  => 针对Long型数据/整洁数据

- `data`作为必需参数
- xandy变量必须指定为字符串



## 变量之一为离散值(效果差)

```python
sns.lmplot(x="size", y="tip", data=tips);
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231194623.png" alt="image-20201231194622889" style="zoom:67%;" />

- 选择1：向离散值添加一些随机噪声（“抖动”） => 仅应用于散点图数据，不会影响回归线拟合本身

  - `sns.lmplot(x="size", y="tip", data=tips, x_jitter=.05);`

  <img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231194734.png" alt="image-20201231194734159" style="zoom:67%;" />

- 选择2：将每个离散分组中的观测值折叠起来，以绘制中心趋势的估计以及置信区间

  - `sns.lmplot(x="size", y="tip", data=tips, x_estimator=np.mean);`

  <img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231194758.png" alt="image-20201231194758204" style="zoom:67%;" />



# 拟合不同的模型

简单线性回归模型非常容易拟合，但是不适用于某些数据集：比如[安斯库姆四重奏](https://en.wikipedia.org/wiki/Anscombe's_quartet)数据集

```python
anscombe = sns.load_dataset("anscombe")
### I中数据线性
sns.lmplot(x="x", y="y", data=anscombe.query("dataset == 'I'"),
           ci=None, scatter_kws={"s": 80});

### II中数据抛物线：二次
sns.lmplot(x="x", y="y", data=anscombe.query("dataset == 'II'"),
           ci=None, scatter_kws={"s": 80});
# 参数order=2
sns.lmplot(x="x", y="y", data=anscombe.query("dataset == 'II'"),
           order=2, ci=None, scatter_kws={"s": 80});


### III中数据：某个点异常
sns.lmplot(x="x", y="y", data=anscombe.query("dataset == 'III'"),
           ci=None, scatter_kws={"s": 80});
# 拟合稳健的回归：使用不同的损失函数来权衡相对较大的残差
# 参数robust
sns.lmplot(x="x", y="y", data=anscombe.query("dataset == 'III'"),
           robust=True, ci=None, scatter_kws={"s": 80});
```





- 当y变量为二进制时

```python
tips["big_tip"] = (tips.tip / tips.total_bill) > .15
sns.lmplot(x="total_bill", y="big_tip", data=tips,
           y_jitter=.03);
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231195108.png" alt="image-20201231195108166" style="zoom:67%;" />

效果不好 => 选择逻辑回归

```python
### 参数logistic=True
sns.lmplot(x="total_bill", y="big_tip", data=tips,
           logistic=True, y_jitter=.03);
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231195149.png" alt="image-20201231195148881" style="zoom:67%;" />

注意，逻辑回归估计值比简单回归计算量大得多（鲁棒回归也是如此）





## 非参数回归：[lowess smoother](https://en.wikipedia.org/wiki/Local_regression)

- 假设少：但计算量大 => 所以不设置置信区间

```python
sns.lmplot(x="total_bill", y="tip", data=tips,
           lowess=True);
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231195343.png" alt="image-20201231195343350" style="zoom:67%;" />



## [`residplot()`](https://seaborn.pydata.org/generated/seaborn.residplot.html#seaborn.residplot)|检测简单回归模型是否适合数据集

绘制每个观察值的残差值。理想情况下，这些值应随机分布在：y = 0两侧

```python
sns.residplot(x="x", y="y", data=anscombe.query("dataset == 'I'"),
              scatter_kws={"s": 80});
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231195508.png" alt="image-20201231195507817" style="zoom:67%;" />

如果残差中存在结构，则表明简单的线性回归是不合适的

```python
sns.residplot(x="x", y="y", data=anscombe.query("dataset == 'II'"),
              scatter_kws={"s": 80});
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231195529.png" alt="image-20201231195529638" style="zoom:67%;" />



# 多变量

- regplot()只显示单个变量的关系

-  [`lmplot()`](https://seaborn.pydata.org/generated/seaborn.lmplot.html#seaborn.lmplot)结合[`regplot()`](https://seaborn.pydata.org/generated/seaborn.regplot.html#seaborn.regplot)和[`FacetGrid`](https://seaborn.pydata.org/generated/seaborn.FacetGrid.html#seaborn.FacetGrid)：可提供一个简单的界面，以在“多面”图上显示线性回归

```python
sns.lmplot(x="total_bill", y="tip", hue="smoker", data=tips);
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231195727.png" alt="image-20201231195726881" style="zoom:67%;" />

```python
# 补充：添加形状的标记
sns.lmplot(x="total_bill", y="tip", hue="smoker", data=tips,
           markers=["o", "x"], palette="Set1");

sns.lmplot(x="total_bill", y="tip", hue="smoker", col="time", data=tips);
sns.lmplot(x="total_bill", y="tip", hue="smoker",
           col="time", row="sex", data=tips);
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231195819.png" alt="image-20201231195818793" style="zoom:67%;" />





# 设置图形的大小、形状

- regplot()提供了axes-level的功能
- 当前Axes：默认和figure具有相同的尺寸

```python
### 创建新的figure对象：设置尺寸
f, ax = plt.subplots(figsize=(5, 6))
sns.regplot(x="total_bill", y="tip", data=tips, ax=ax); #新的axes对象：ax=ax

### 直接设置参数：height、aspect
sns.lmplot(x="total_bill", y="tip", col="day", data=tips,
           col_wrap=2, height=3);
sns.lmplot(x="total_bill", y="tip", col="day", data=tips,
           aspect=.5);
```





# 配合[`jointplot()`](https://seaborn.pydata.org/generated/seaborn.jointplot.html#seaborn.jointplot)

```python
### 参数：kind='reg'
sns.jointplot(x="total_bill", y="tip", data=tips, kind="reg");
```



# 配合[`pairplot()`](https://seaborn.pydata.org/generated/seaborn.pairplot.html#seaborn.pairplot)

```python
sns.pairplot(tips, x_vars=["total_bill", "size"], y_vars=["tip"],
             height=5, aspect=.8, kind="reg");
sns.pairplot(tips, x_vars=["total_bill", "size"], y_vars=["tip"],
             hue="smoker", height=5, aspect=.8, kind="reg");
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231200236.png" alt="image-20201231200236277" style="zoom:67%;" />

