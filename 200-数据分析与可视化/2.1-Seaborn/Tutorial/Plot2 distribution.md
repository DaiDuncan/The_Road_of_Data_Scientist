step1：任何分析或建模数据的第一步都应该是了解变量的分布方式

- 测量值范围？
- 主要趋势是什么？
- 是否严重偏向一个方向？
- 是否有双峰证据？
- 有明显的离群值吗？



step2：该分布的特征，在数据集中的其他变量中分布是否有所不同？@第三变量：参数hue

- 是什么导致双峰分布？



## Axes-level

[`histplot()`](https://seaborn.pydata.org/generated/seaborn.histplot.html#seaborn.histplot)

[`kdeplot()`](https://seaborn.pydata.org/generated/seaborn.kdeplot.html#seaborn.kdeplot)

[`ecdfplot()`](https://seaborn.pydata.org/generated/seaborn.ecdfplot.html#seaborn.ecdfplot)

[`rugplot()`](https://seaborn.pydata.org/generated/seaborn.rugplot.html#seaborn.rugplot)



## Figure-level => 组合

[`displot()`](https://seaborn.pydata.org/generated/seaborn.displot.html#seaborn.displot)

[`jointplot()`](https://seaborn.pydata.org/generated/seaborn.jointplot.html#seaborn.jointplot) 

[`pairplot()`](https://seaborn.pydata.org/generated/seaborn.pairplot.html#seaborn.pairplot)



# histplot() 直方图

```python
### 单变量
penguins = sns.load_dataset("penguins")
sns.displot(penguins, x="flipper_length_mm")
```

## 参数|binwidth/bins

```python
sns.displot(penguins, x="flipper_length_mm", binwidth=3)
# 等价
sns.displot(penguins, x="flipper_length_mm", bins=20)
sns.displot(tips, x="size", bins=[1, 2, 3, 4, 5, 6, 7])


### 如果数据是离散的
sns.displot(tips, x="size", discrete=True)

### 如果是分类数据
# 参数shrink：稍微“缩小”以强调轴的分类特性
sns.displot(tips, x="day", shrink=.8)
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231114226.png" alt="image-20201231114226275" style="zoom:80%;" />



## 多变量

```python
sns.displot(penguins, x="flipper_length_mm", hue="species")
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231114825.png" alt="image-20201231114825563" style="zoom:67%;" />

默认情况下，不同的直方图彼此“分层”，难以区分 => 从条形图更改为“阶梯”图

```python
# 参数element
sns.displot(penguins, x="flipper_length_mm", hue="species", element="step")
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231114909.png" alt="image-20201231114909295" style="zoom:67%;" />

将它们“堆叠”

```python
# 参数multiple
sns.displot(penguins, x="flipper_length_mm", hue="species", multiple="stack")
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231115044.png" alt="image-20201231115044070" style="zoom: 67%;" />

堆叠的直方图强调了变量之间的整体关系，但是它会模糊其他特征

- 难以确定Adelie分布的模式

另一种选择是“避开”条形图，将其水平移动并减小它们的宽度。这样可以确保没有重叠，并且条形图在高度上保持可比性，但是只有当类别变量的级别数较少时，它才能很好地起作用

```python
# 参数multiple
sns.displot(penguins, x="flipper_length_mm", hue="sex", multiple="dodge")
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231115720.png" alt="image-20201231115720356" style="zoom:67%;" />

## .distplot() figure-level

```python
sns.displot(penguins, x="flipper_length_mm", col="sex", multiple="dodge")
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231115808.png" alt="image-20201231115807874" style="zoom:67%;" />



## 特殊|子集样本数量不相等

```python
### 参数stat
sns.displot(penguins, x="flipper_length_mm", hue="species", stat="density")
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231120049.png" alt="image-20201231120049421" style="zoom:67%;" />

默认情况下，规范化stat应用于整个分布，这仅会重新调整条形的高度 => 设置common_norm=False，每个子集将独立规范化：

```python
sns.displot(penguins, x="flipper_length_mm", hue="species", stat="density", common_norm=False)
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231120157.png" alt="image-20201231120156876" style="zoom:67%;" />

密度归一化，使得面积总和为1。但是，密度轴无法直接解释。

另一种选择是将高度标准化为总和为1 => 对于==离散变量==最有意义

```python
sns.displot(penguins, x="flipper_length_mm", hue="species", stat="probability")
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231131010.png" alt="image-20201231131010502" style="zoom:67%;" />



# kdeplot()|核密度估计

不适用离散的bins，而是使用高斯核对观测值进行平滑处理，从而产生连续的密度估计值

- 对比histplot()

```python
### 直方图histplot
penguins = sns.load_dataset("penguins")
sns.histplot(data=penguins, x="flipper_length_mm", hue="species", multiple="stack")


### 分布密度图kdeplot
sns.kdeplot(data=penguins, x="flipper_length_mm", hue="species", multiple="stack")
```

直方图histplot：

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231094059.png" alt="image-20201231094059442" style="zoom:67%;" />

分布密度图：

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231094119.png" alt="image-20201231094119258" style="zoom:67%;" />

```python
sns.displot(penguins, x="flipper_length_mm", kind="kde")
```

类似直方图中的bin大小，KDE准确表示数据的能力取决于平滑带宽的选择

窄带宽如何使双峰变得更加明显，但曲线变得不那么平滑。

相反，较大的带宽几乎完全掩盖了双峰性

```python
### 窄宽带
sns.displot(penguins, x="flipper_length_mm", kind="kde", bw_adjust=.25)

### 大的宽带
sns.displot(penguins, x="flipper_length_mm", kind="kde", bw_adjust=2)
```

窄宽带：

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231143722.png" alt="image-20201231143722095" style="zoom:67%;" />

大宽带：

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231143735.png" alt="image-20201231143735260" style="zoom:67%;" />



## 多变量

```python
### 为该变量的每个级别分别计算一个密度估计
# 参数hue
sns.displot(penguins, x="flipper_length_mm", hue="species", kind="kde")

# 参数multiple 
sns.displot(penguins, x="flipper_length_mm", hue="species", kind="kde", multiple="stack")
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231143951.png" alt="image-20201231143951410" style="zoom:67%;" />

```python
# Alpha值：透明度
sns.displot(penguins, x="flipper_length_mm", hue="species", kind="kde", fill=True)
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231144033.png" alt="image-20201231144033739" style="zoom:67%;" />



## #注意问题

优点：数据的重要特征很容易辨别（集中趋势，双峰态，偏斜），并且可以轻松比较子集

- KDE的逻辑假定基础分布是平滑无界的。

  这种假设可能会失败，当一个变量反映一个自然界的数量时。如果存在接近边界的观察值（例如，变量的小值不能为负），则KDE曲线可能会扩展为不切实际的值：

```python
sns.displot(tips, x="total_bill", kind="kde")


### 使用参数cut：指定曲线应延伸到极限数据点以外的距离
sns.displot(tips, x="total_bill", kind="kde", cut=0)
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231144234.png" alt="image-20201231144234475" style="zoom:67%;" />

- 对于离散数据或当数据自然连续但特定值被过度表示时，KDE方法也会失败

  即使数据本身不平滑，KDE也会始终显示平滑曲线

```python
### 钻石重量的分布
# kde更连续
diamonds = sns.load_dataset("diamonds")
sns.displot(diamonds, x="carat", kind="kde")

# hist更多的锯齿状分布
sns.displot(diamonds, x="carat")


### 两种方法结合起来：
sns.displot(diamonds, x="carat", kde=True)
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231144428.png" alt="image-20201231144427868" style="zoom:67%;" />



# ecdf|经验分布函数

- 直接表示每个数据点。这意味着没有要考虑的箱尺寸或平滑参数
- 曲线是单调递增的，因此非常适合比较多个分布

```python
sns.displot(penguins, x="flipper_length_mm", kind="ecdf")
sns.displot(penguins, x="flipper_length_mm", hue="species", kind="ecdf")
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231144634.png" alt="image-20201231144634060" style="zoom:67%;" />

主要缺点是，与直方图或密度曲线相比，它不直观地表示分布的形状

考虑一下双峰如何在直方图中立即显现，但是要在ECDF图中看到它，必须寻找变化的斜率。但是，==通过实践，可以通过检查ECDF来学习回答有关分布的所有重要问题，并且这样做可以是一种有效的方法==





# 可视化二元分布(双变量)

```python
### 类似heatmap()
sns.displot(penguins, x="bill_length_mm", y="bill_depth_mm")
```



```python
### 二元KDE图使用2D高斯平滑（x，y）
sns.displot(penguins, x="bill_length_mm", y="bill_depth_mm", kind="kde")
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231144839.png" alt="image-20201231144839657" style="zoom:67%;" />![image-20201231144947229](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231144947.png)

```python
### 参数hue：颜色区分
sns.displot(penguins, x="bill_length_mm", y="bill_depth_mm", hue="species")


### 二元KDE图的轮廓方法更适合评估重叠部分
sns.displot(penguins, x="bill_length_mm", y="bill_depth_mm", hue="species", kind="kde")
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231144839.png" alt="image-20201231144839657" style="zoom:67%;" />![image-20201231144947229](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231144947.png)



## 参数选择|bins大小 平滑带宽

```python
sns.displot(penguins, x="bill_length_mm", y="bill_depth_mm", binwidth=(2, .5))
# 参数cbar：添加颜色条
sns.displot(penguins, x="bill_length_mm", y="bill_depth_mm", binwidth=(2, .5), cbar=True)
```



双变量密度等值线的含义不太直接。因为不能直接解释密度，所以等高线是按密度的等比例绘制的，这意味着每条曲线都显示一个水平集，使得密度的某些比例p位于其下方

```python
# thresh密度的最小阈值
# levels表示显示密度level的数量
sns.displot(penguins, x="bill_length_mm", y="bill_depth_mm", kind="kde", thresh=.2, levels=4)

# levels接受列表值
sns.displot(penguins, x="bill_length_mm", y="bill_depth_mm", kind="kde", levels=[.01, .05, .1, .8])
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231145310.png" alt="image-20201231145310745" style="zoom:67%;" />





- 双变量直方图允许一个或两个变量是离散的

绘制一个离散变量和一个连续变量：提供了另一种比较条件单变量分布的方法：

```python
sns.displot(diamonds, x="price", y="clarity", log_scale=(True, False))
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231145412.png" alt="image-20201231145412071" style="zoom:67%;" />

- 绘制两个离散变量

显示观察结果交叉表的一种简便方法

```python
sns.displot(diamonds, x="color", y="clarity")
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231145446.png" alt="image-20201231145446447" style="zoom:67%;" />



# 结合多分布

## [`jointplot()`](https://seaborn.pydata.org/generated/seaborn.jointplot.html#seaborn.jointplot)|[`JointGrid`](https://seaborn.pydata.org/generated/seaborn.JointGrid.html#seaborn.JointGrid)类的API

使用两个变量的边际分布来扩充双变量关系图或分布图

```python
# 1分布是散点图
# 2边缘分布是直方图
sns.jointplot(data=penguins, x="bill_length_mm", y="bill_depth_mm")
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231145707.png" alt="image-20201231145706974" style="zoom:67%;" />



- 参数kind='kde'

```python
sns.jointplot(
    data=penguins,
    x="bill_length_mm", y="bill_depth_mm", hue="species",
    kind="kde"
)
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231145808.png" alt="image-20201231145808603" style="zoom:67%;" />

```python
g = sns.JointGrid(data=penguins, x="bill_length_mm", y="bill_depth_mm")
# 主要是histplot
g.plot_joint(sns.histplot)
# 边缘分布是boxplot
g.plot_marginals(sns.boxplot)
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231145858.png" alt="image-20201231145857896" style="zoom:67%;" />

```python
### 参数rug=True：边缘的轴须
sns.displot(
    penguins, x="bill_length_mm", y="bill_depth_mm",
    kind="kde", rug=True
)


sns.relplot(data=penguins, x="bill_length_mm", y="bill_depth_mm")
sns.rugplot(data=penguins, x="bill_length_mm", y="bill_depth_mm")
```





## [`pairplot()`](https://seaborn.pydata.org/generated/seaborn.pairplot.html#seaborn.pairplot)|[`PairGrid`](https://seaborn.pydata.org/generated/seaborn.PairGrid.html#seaborn.PairGrid)类API

```python
sns.pairplot(penguins)


g = sns.PairGrid(penguins)
g.map_upper(sns.histplot)  #上三角histplot
g.map_lower(sns.kdeplot, fill=True)  #下三角kdeplot
g.map_diag(sns.histplot, kde=True) #对角线histplot
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231183908.png" alt="image-20201231183907839" style="zoom:67%;" />