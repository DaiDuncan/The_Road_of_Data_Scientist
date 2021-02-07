```python
import seaborn as sns
import matplotlib.pyplot as plt
sns.set_theme(style="ticks", color_codes=True)
```

注意：分类图当前不支持`size`或`style` => 添加变量只能用参数hue

# [`catplot()`](https://seaborn.pydata.org/generated/seaborn.catplot.html#seaborn.catplot)|API

统一的API可以轻松地在不同种类之间进行切换，并从多个角度查看数据

分类散点图：

- [`stripplot()`](https://seaborn.pydata.org/generated/seaborn.stripplot.html#seaborn.stripplot)（带有`kind="strip"`;默认值）
- [`swarmplot()`](https://seaborn.pydata.org/generated/seaborn.swarmplot.html#seaborn.swarmplot)（带有`kind="swarm"`）

分类分布图：

- [`boxplot()`](https://seaborn.pydata.org/generated/seaborn.boxplot.html#seaborn.boxplot)（带有`kind="box"`）
- [`violinplot()`](https://seaborn.pydata.org/generated/seaborn.violinplot.html#seaborn.violinplot)（带有`kind="violin"`）
- [`boxenplot()`](https://seaborn.pydata.org/generated/seaborn.boxenplot.html#seaborn.boxenplot)（带有`kind="boxen"`）

分类估计图：

- [`pointplot()`](https://seaborn.pydata.org/generated/seaborn.pointplot.html#seaborn.pointplot)（带有`kind="point"`）
- [`barplot()`](https://seaborn.pydata.org/generated/seaborn.barplot.html#seaborn.barplot)（带有`kind="bar"`）
- [`countplot()`](https://seaborn.pydata.org/generated/seaborn.countplot.html#seaborn.countplot)（带有`kind="count"`）



# 分类的散点图

## 默认[`stripplot()`](https://seaborn.pydata.org/generated/seaborn.stripplot.html#seaborn.stripplot)

```python
tips = sns.load_dataset("tips")
sns.catplot(x="day", y="total_bill", data=tips)

# 禁用jitter
sns.catplot(x="day", y="total_bill", jitter=False, data=tips)
```

含有少量的jitter：

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231192140.png" alt="image-20201231192140662" style="zoom:67%;" />



## [`swarmplot()`](https://seaborn.pydata.org/generated/seaborn.swarmplot.html#seaborn.swarmplot)

仅适用于相对较小的数据集，但可以更好地表示观测值的分布

```python
sns.catplot(x="day", y="total_bill", kind="swarm", data=tips)

### 增加变量：参数hue
sns.catplot(x="day", y="total_bill", hue="sex", kind="swarm", data=tips)
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231192319.png" alt="image-20201231192319296" style="zoom:67%;" />



## 参数order|分类顺序

```python
sns.catplot(x="smoker", y="tip", order=["No", "Yes"], data=tips)

### 转置分类轴：改变x y的位置
sns.catplot(x="total_bill", y="day", hue="time", kind="swarm", data=tips)
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231192505.png" alt="image-20201231192505066" style="zoom:67%;" />





# 分类的分布

## 箱线图 [`boxplot()`](https://seaborn.pydata.org/generated/seaborn.boxplot.html#seaborn.boxplot)

四分位数

```python
sns.catplot(x="day", y="total_bill", kind="box", data=tips)

# 参数hue: 分类沿着分类轴移动，不会重叠 => dodging躲避
sns.catplot(x="day", y="total_bill", hue="smoker", kind="box", data=tips)
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231192654.png" alt="image-20201231192654727" style="zoom:67%;" />

```python
### 参数dodge：可禁用
tips["weekend"] = tips["day"].isin(["Sat", "Sun"])
sns.catplot(x="day", y="total_bill", hue="weekend",
            kind="box", dodge=False, data=tips)
```



## 增强箱线图 [`boxenplot()`](https://seaborn.pydata.org/generated/seaborn.boxenplot.html#seaborn.boxenplot)|适合大型数据集

```python
diamonds = sns.load_dataset("diamonds")
sns.catplot(x="color", y="price", kind="boxen",
            data=diamonds.sort_values("color"))
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231192844.png" alt="image-20201231192843732" style="zoom:67%;" />



## 小提琴图 [`violinplot()`](https://seaborn.pydata.org/generated/seaborn.violinplot.html#seaborn.violinplot)

显示了：

- 箱形图的四分位
- 晶须值

```python
sns.catplot(x="total_bill", y="day", hue="sex",
            kind="violin", data=tips)

### 由于小提琴图使用KDE，因此可能需要调整一些其他参数
sns.catplot(x="total_bill", y="day", hue="sex",
            kind="violin", bw=.15, cut=0,
            data=tips)


### 参数hue只有两个级别时，也可以“拆分”小提琴 => 参数split
sns.catplot(x="day", y="total_bill", hue="sex",
            kind="violin", split=True, data=tips)
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231193033.png" alt="image-20201231193033736" style="zoom:67%;" />

```python
### 显示每个观察值而不是汇总箱图值
sns.catplot(x="day", y="total_bill", hue="sex",
            kind="violin", inner="stick", split=True,
            palette="pastel", data=tips)
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231193136.png" alt="image-20201231193136444" style="zoom:67%;" />



```python
### 组合swarmplot()或striplot()
g = sns.catplot(x="day", y="total_bill", kind="violin", inner=None, data=tips)
sns.swarmplot(x="day", y="total_bill", color="k", size=3, data=tips, ax=g.ax)
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231193228.png" alt="image-20201231193227919" style="zoom:67%;" />





# 分类的统计

## 条形图 [`barplot()`](https://seaborn.pydata.org/generated/seaborn.barplot.html#seaborn.barplot)|显示统计量

当每个类别中都有多个观察值时，它还会计算估计值周围的置信区间，并使用误差线进行绘制

```python
### 默认统计值为均值
titanic = sns.load_dataset("titanic")
sns.catplot(x="sex", y="survived", hue="class", kind="bar", data=titanic)
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231193428.png" alt="image-20201231193427915" style="zoom:67%;" />



## [`countplot()`](https://seaborn.pydata.org/generated/seaborn.countplot.html#seaborn.countplot)|显示数量

```python
sns.catplot(x="deck", kind="count", palette="ch:.25", data=titanic)
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231193515.png" alt="image-20201231193515341" style="zoom:67%;" />



## [`pointplot()`](https://seaborn.pydata.org/generated/seaborn.pointplot.html#seaborn.pointplot) 点图|

使用另一轴上的高度对估计值进行编码，但是它不会显示完整的条形，而是绘制点估计和置信区间

```python
sns.catplot(x="sex", y="survived", hue="class", kind="point", data=titanic)
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231193639.png" alt="image-20201231193639425" style="zoom:67%;" />![image-20201231193744725](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231193744.png)

```python
### 分类图缺少参数style => 改用markers, linestyles和hue来增加区分度
sns.catplot(x="class", y="survived", hue="sex",
            palette={"male": "g", "female": "m"},
            markers=["^", "o"], linestyles=["-", "--"],
            kind="point", data=titanic)
```

![image-20201231193744725](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231193744.png)



# 针对wide型数据

```python
iris = sns.load_dataset("iris")
sns.catplot(data=iris, orient="h", kind="box")
```

axes-level函数接受Pandas或numpy对象的向量，而不是DataFrame

```python
sns.violinplot(x=iris.species, y=iris.sepal_length)
```

要控制由上述功能绘制的图形的大小和形状，必须使用matplotlib命令自己设置图形

```python
f, ax = plt.subplots(figsize=(7, 3)) #图形的大小
sns.countplot(y="deck", data=titanic, color="c")
```





# 多变量汇总

背景：[`catplot()`](https://seaborn.pydata.org/generated/seaborn.catplot.html#seaborn.catplot)基于[`FacetGrid`](https://seaborn.pydata.org/generated/seaborn.FacetGrid.html#seaborn.FacetGrid)

```python
sns.catplot(x="day", y="total_bill", hue="smoker",
            col="time", aspect=.7,
            kind="swarm", data=tips)
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231194006.png" alt="image-20201231194005610" style="zoom:67%;" />

```python
g = sns.catplot(x="fare", y="survived", row="class",
                kind="box", orient="h", height=1.5, aspect=4,
                data=titanic.query("fare > 0")) #data：调用.query()
g.set(xscale="log")
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231194032.png" alt="image-20201231194032020" style="zoom:67%;" />

