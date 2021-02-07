# Figure-level vs. axes-level functions

Axes-level: matplotlib.pyplot.Axes对象

Figure-level: [`FacetGrid`](https://seaborn.pydata.org/generated/seaborn.FacetGrid.html#seaborn.FacetGrid)

每一大类都有一个figure-level函数，提供各种axes-level函数的API

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231094347.png" alt="image-20201231094347494" style="zoom:67%;" />

## figure-level的优、缺点

| 优点                     | 缺点                               |
| :----------------------- | :--------------------------------- |
| 易于通过数据变量进行操作 | 许多参数不在函数参数列表中         |
| 默认情况下，图例在图形外 | 不能成为更大的matplotlib图的一部分 |
| 简单的figure-level调用   | 与matplotlib不同的API              |
| 不同图形尺寸参数化       | 不同图形尺寸参数化                 |

增加了一些额外的复杂性。

如果要制作一个独立、复杂的图形，其中包含不同的绘图类型 => 建议直接使用matplotlib设置图形，并使用axes-level函数填写各个组件



- sns.displot()

```python
### histplot()
sns.displot(data=penguins, x="flipper_length_mm", hue="species", multiple="stack")
# 分开为子图
sns.displot(data=penguins, x="flipper_length_mm", hue="species", col="species")


### kdeplot()
sns.displot(data=penguins, x="flipper_length_mm", hue="species", multiple="stack", kind="kde")
```



## Axes-level|[matplotlib.pyplot.gca()](https://matplotlib.org/api/_as_gen/matplotlib.pyplot.gca.html#matplotlib.pyplot.gca)

gca: Get the current axes，如果有必要，直接创建一个axes

- 参数ax

直接调用matplotlib函数

```python
### axs是(1, 2)
f, axs = plt.subplots(1, 2, figsize=(8, 4), gridspec_kw=dict(width_ratios=[4, 3]))
# axs[0]
sns.scatterplot(data=penguins, x="flipper_length_mm", y="bill_length_mm", hue="species", ax=axs[0])
# axs[1]
sns.histplot(data=penguins, x="species", hue="species", shrink=.8, alpha=.8, legend=False, ax=axs[1])
f.tight_layout()
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231100623.png" alt="image-20201231100622796" style="zoom:67%;" />





## Figure-level|API

只管理figure，不管理axes

- 图例legend置于图外

```python
tips = sns.load_dataset("tips")
g = sns.relplot(data=tips, x="total_bill", y="tip")
g.ax.axline(xy1=(10, 2), slope=.2, color="b", dashes=(5, 2))
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231101152.png" alt="image-20201231101152405" style="zoom:67%;" />



## Figure-level|[`FacetGrid`](https://seaborn.pydata.org/generated/seaborn.FacetGrid.html#seaborn.FacetGrid)

该方法不是matplotlib API的一部分，仅在使用figure-level功能时存在

```python
g = sns.relplot(data=penguins, x="flipper_length_mm", y="bill_length_mm", col="sex")
g.set_axis_labels("Flipper length (mm)", "Bill length (mm)")
```





# 设置属性

## 图像size

注意：图像尺寸取决于figure size以及axes在figure中的layout

[`matplotlib.pyplot.subplots()`](https://matplotlib.org/api/_as_gen/matplotlib.pyplot.subplots.html#matplotlib.pyplot.subplots)

- 参数figsize

**matplotlib.Figure.set_size_inches()**



针对figure-level函数：

1. 具有控制图像size的参数(针对subplot)
2. height, aspect与matplotlib(width, height有区别
   - seaborn中：width = height * aspect
3. 参数对应每个subplot，而不是整体的figure

```python
f, ax = plt.subplots()
f, ax = plt.subplots(1, 2, sharey=True)

g = sns.FacetGrid(penguins)
g = sns.FacetGrid(penguins, col="sex")
g = sns.FacetGrid(penguins, col="sex", height=3.5, aspect=.75)
```



# 多个视图

以下两个函数都是figure-level：默认情况下会创建具有多个子图的图形

但是对象不同：分别为[`JointGrid`](https://seaborn.pydata.org/generated/seaborn.JointGrid.html#seaborn.JointGrid)和[`PairGrid`](https://seaborn.pydata.org/generated/seaborn.PairGrid.html#seaborn.PairGrid)

## 1 [`jointplot()`](https://seaborn.pydata.org/generated/seaborn.jointplot.html#seaborn.jointplot)

绘制两个变量的关系或联合分布，同时添加边缘轴，分别显示每个变量的单变量分布

```python
sns.jointplot(data=penguins, x="flipper_length_mm", y="bill_length_mm", hue="species")
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231104649.png" alt="image-20201231104649343" style="zoom:67%;" />

- axes-level
  - [`scatterplot()`](https://seaborn.pydata.org/generated/seaborn.scatterplot.html#seaborn.scatterplot)
  - [`kdeplot()`](https://seaborn.pydata.org/generated/seaborn.kdeplot.html#seaborn.kdeplot)
- 参数kind

```python
sns.jointplot(data=penguins, x="flipper_length_mm", y="bill_length_mm", hue="species", kind="hist")
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231104856.png" alt="image-20201231104856327" style="zoom:67%;" />

## 2 [`pairplot()`](https://seaborn.pydata.org/generated/seaborn.pairplot.html#seaborn.pairplot)

集合了joint和marginal视图，但不是着眼于单个关系，而是同时可视化了变量的每个成对组合

```python
sns.pairplot(data=penguins, hue="species")
```

![image-20201231104756799](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231104757.png)