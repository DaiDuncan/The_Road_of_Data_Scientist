- “lattice” or “trellis” plotting：栅栏格子图@FacetGrid



# [`FacetGrid`](https://seaborn.pydata.org/generated/seaborn.FacetGrid.html#seaborn.FacetGrid)

- row
- col
- hue

应用：

- [`relplot()`](https://seaborn.pydata.org/generated/seaborn.relplot.html#seaborn.relplot)
- [`displot()`](https://seaborn.pydata.org/generated/seaborn.displot.html#seaborn.displot)
- [`catplot()`](https://seaborn.pydata.org/generated/seaborn.catplot.html#seaborn.catplot)
- [`lmplot()`](https://seaborn.pydata.org/generated/seaborn.lmplot.html#seaborn.lmplot)

## FacetGrid.map()

```python
### 建立初始化的空白网格
tips = sns.load_dataset("tips")
g = sns.FacetGrid(tips, col="time")

### 可视化数据的主要方法是使用FacetGrid.map()
# histplot
g = sns.FacetGrid(tips, col="time")
g.map(sns.histplot, "tip")

# scatterplot
g = sns.FacetGrid(tips, col="sex", hue="smoker")
g.map(sns.scatterplot, "total_bill", "tip", alpha=.7)
g.add_legend()  #legend单独绘制
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210101103304.png" alt="image-20210101103304190" style="zoom:67%;" />



- 参数：margin_titles

```python
g = sns.FacetGrid(tips, row="smoker", col="time", margin_titles=True)
g.map(sns.regplot, "size", "total_bill", color=".3", fit_reg=False, x_jitter=.1)
```

注意，margin_titlesmatplotlib API并未正式支持该功能，因此可能无法在所有情况下均正常运行

- 参数：height, aspect

```python
g = sns.FacetGrid(tips, col="day", height=4, aspect=.5)
g.map(sns.barplot, "sex", "total_bill", order=["Male", "Female"])
```

- 分类的排序

```python
# 基于.value_counts()
ordered_days = tips.day.value_counts().index
g = sns.FacetGrid(tips, row="day", row_order=ordered_days,
                  height=1.7, aspect=4,)
g.map(sns.kdeplot, "total_bill")
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210101103526.png" alt="image-20210101103525900" style="zoom:67%;" />

- 颜色
  - [`color_palette()`](https://seaborn.pydata.org/generated/seaborn.color_palette.html#seaborn.color_palette)
  - 参数hue

```python
pal = dict(Lunch="seagreen", Dinner=".7")
g = sns.FacetGrid(tips, hue="time", palette=pal, height=5)
g.map(sns.scatterplot, "total_bill", "tip", s=100, alpha=.5)
g.add_legend()


### 一个变量有多个levels：不能使用row => 参数col_wrap
attend = sns.load_dataset("attention").query("subject <= 12")
g = sns.FacetGrid(attend, col="subject", col_wrap=4, height=2, ylim=(0, 10))
g.map(sns.pointplot, "solutions", "score", order=[1, 2, 3], color=".3", ci=None)
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210101103701.png" alt="image-20210101103701741" style="zoom:67%;" />



## FacetGrid.set()|标签ticks

## FacetGrid.set_axis_labels()|标签labels

```python
with sns.axes_style("white"):
    g = sns.FacetGrid(tips, row="sex", col="smoker", margin_titles=True, height=2.5)
g.map(sns.scatterplot, "total_bill", "tip", color="#334488")
g.set_axis_labels("Total bill (US Dollars)", "Tip")
g.set(xticks=[10, 30, 50], yticks=[2, 6, 10])
g.fig.subplots_adjust(wspace=.02, hspace=.02)
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210101103911.png" alt="image-20210101103911631" style="zoom:67%;" />



## figure与axes对象

分别作为成员属性存储在fig和处axes（一个二维数组）

制作没有行或列faceting的图形时，还可以使用该ax属性直接访问单轴

```python
g = sns.FacetGrid(tips, col="smoker", margin_titles=True, height=4)
g.map(plt.scatter, "total_bill", "tip", color="#338844", edgecolor="white", s=50, lw=1)
for ax in g.axes.flat:  #访问单个axes
    ax.axline((0, 0), slope=.2, c=".2", ls="--", zorder=0)
g.set(xlim=(0, 60), ylim=(0, 14))
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210101104050.png" alt="image-20210101104050628" style="zoom:67%;" />





# 自定义图像函数

规则：

1. 必须绘制在“当前活动”的matplotlib上Axes

   如果想直接使用其方法，可以调用matplotlib.pyplot.gca()以获取对当前Axes方法的引用

2. 接受在位置参数中绘制的数据 => FacetGrid.map()

3. 必须能够接受参数color和label关键字参数

   大多数情况下，最简单的方法是捕获的通用字典**kwargs并将其传递给基础绘图函数

```python
from scipy import stats
def quantile_plot(x, **kwargs):
    quantiles, xr = stats.probplot(x, fit=False) 
    plt.scatter(xr, quantiles, **kwargs) #通用字典**kwargs并将其传递给基础绘图函数

g = sns.FacetGrid(tips, col="sex", height=4)
g.map(quantile_plot, "total_bill") #"total_bill"对应quantile_plot中的参数x
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210101130827.png" alt="image-20210101130827169" style="zoom:67%;" />

- 绘制双变量图：先接受x轴变量，然后接受y轴变量

```python
def qqplot(x, y, **kwargs):
    _, xr = stats.probplot(x, fit=False)
    _, yr = stats.probplot(y, fit=False)
    plt.scatter(xr, yr, **kwargs)

g = sns.FacetGrid(tips, col="smoker", height=4)
g.map(qqplot, "total_bill", "tip")


### 含参数hue
g = sns.FacetGrid(tips, hue="time", col="sex", height=4)
g.map(qqplot, "total_bill", "tip")
g.add_legend()
```

qqplot|基本的scatter()：

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210101131007.png" alt="image-20210101131007166" style="zoom:67%;" />

qqplot|FacetGrid含参数hue：

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210101131159.png" alt="image-20210101131159850" style="zoom:67%;" />



- 映射功能：该功能无法正常使用colorandlabel关键字参数

  允许使用matplotlib.pyplot.hexbin()，否则将无法与FacetGridAPI配合使用

```python
def hexbin(x, y, color, **kwargs):
    cmap = sns.light_palette(color, as_cmap=True)  #设置颜色
    plt.hexbin(x, y, gridsize=15, cmap=cmap, **kwargs) #hexbin是六边形图

with sns.axes_style("dark"):
    g = sns.FacetGrid(tips, hue="time", col="time", height=4)
g.map(hexbin, "total_bill", "tip", extent=[0, 50, 0, 10]);
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210101131411.png" alt="image-20210101131410883" style="zoom:67%;" />



# 结合PairGrid

```python
iris = sns.load_dataset("iris")
g = sns.PairGrid(iris)
g.map(sns.scatterplot)


g = sns.PairGrid(iris)
g.map_diag(sns.histplot) #对角线
g.map_offdiag(sns.scatterplot) #非对角线
```

- 一种常见的使用：通过单独的分类变量为观察结果着色

```python
g = sns.PairGrid(iris, hue="species") #参数hue：针对分类变量species
g.map_diag(sns.histplot)
g.map_offdiag(sns.scatterplot)
g.add_legend()
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210101131544.png" alt="image-20210101131544483" style="zoom:80%;" />

```python
### 只关注特定品种
g = sns.PairGrid(iris, vars=["sepal_length", "sepal_width"], hue="species")
g.map(sns.scatterplot)
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210101131730.png" alt="image-20210101131730001" style="zoom:67%;" />



```python
### 基于对角线：选择不同的三角区域
g = sns.PairGrid(iris)
g.map_upper(sns.scatterplot)
g.map_lower(sns.kdeplot)
g.map_diag(sns.kdeplot, lw=3, legend=False)
```

- 改变行、列的目标对象

```python
### 参数y_vars, x_vars
g = sns.PairGrid(tips, y_vars=["tip"], x_vars=["total_bill", "size"], height=4)
g.map(sns.regplot, color=".3")
g.set(ylim=(-1, 11), yticks=[0, 5, 10])
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210101131833.png" alt="image-20210101131833802" style="zoom:67%;" />



## 设置属性

- 调色板

```python
g = sns.PairGrid(tips, hue="size", palette="GnBu_d")  #palette
g.map(plt.scatter, s=50, edgecolor="white")  #edgecolor
g.add_legend()
```



## pairplot快速简便

```python
sns.pairplot(iris, hue="species", height=2.5)
g = sns.pairplot(iris, hue="species", palette="Set2", diag_kind="kde", height=2.5)
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210101132113.png" alt="image-20210101132113623" style="zoom:67%;" />