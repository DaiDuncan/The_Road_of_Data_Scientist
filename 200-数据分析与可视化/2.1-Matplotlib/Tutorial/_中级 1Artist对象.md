# Artist介绍

Artist：在画布上进行渲染

matplotlib API分为三层

- matplotlib.backend_bases.FigureCanvas 画布：Figure所在
- matplotlib.backend_bases.Renderer 对象：画在FigureCanvas上
- matplotlib.artist.Artist 对象：如何在画布上渲染



FigureCanvas和 Renderer：用户界面工具包(比如wxPython)，绘画语言(比如PostScript®)

Artist：具体元素，比如figure, text, lines => 用户95%的时间精力都在此





## 两种类型的Artist

1. primitives 基本元素：标准图形对象 => 呈现在画布上

   - [`Line2D`](https://matplotlib.org/api/_as_gen/matplotlib.lines.Line2D.html#matplotlib.lines.Line2D) 
   - [`Rectangle`](https://matplotlib.org/api/_as_gen/matplotlib.patches.Rectangle.html#matplotlib.patches.Rectangle)
   - [`Text`](https://matplotlib.org/api/text_api.html#matplotlib.text.Text)
   - [`AxesImage`](https://matplotlib.org/api/image_api.html#matplotlib.image.AxesImage)

2. containers 容器：展示primitives的位置

   - Tick

   - [`Axis`](https://matplotlib.org/api/axis_api.html#matplotlib.axis.Axis) 
   - [`Axes`](https://matplotlib.org/api/axes_api.html#matplotlib.axes.Axes)
   - [`Figure`](https://matplotlib.org/api/_as_gen/matplotlib.figure.Figure.html#matplotlib.figure.Figure)







# 示例|具体过程

## 1 创建figure对象

```python
import matplotlib.pyplot as plt
fig = plt.figure()
ax = fig.add_subplot(2, 1, 1) # two rows, one column, first plot
```





## 2 创建Axes对象

Axes大概是matplotlib API中最重要的类，大部分的时间基于Axes工作

Axes有许多特殊的辅助方法（plot()， text()， hist()， imshow()），以打造最常见的基本图形（Line2D， Text， Rectangle， AxesImage）

这些辅助方法将获取您的数据（例如numpy数组和字符串）并Artist根据需要创建原始实例（例如Line2D），将它们添加到相关容器中，并在需要时绘制它们

```python
fig2 = plt.figure()
ax2 = fig2.add_axes([0.15, 0.1, 0.7, 0.3]) #[left, bottom, width, height]
```



```python
import numpy as np
t = np.arange(0.0, 1.0, 0.01)
s = np.sin(2*np.pi*t)

### ax.plot 创建了一个 Line2D实例并将其添加到Axes.lines列表中
line, = ax.plot(t, s, color='blue', lw=2)

In [101]: ax.lines[0]  #Axes.lines列表的长度为1
Out[101]: <matplotlib.lines.Line2D instance at 0x19a95710>
In [102]: line
Out[102]: <matplotlib.lines.Line2D instance at 0x19a95710>

### 删除line对象
del ax.lines[0]
ax.lines.remove(line)  # one or the other, not both!
```



### Axes的属性

每个`Axes` 实例均包含[`XAxis`](https://matplotlib.org/api/axis_api.html#matplotlib.axis.XAxis)和 [`YAxis`](https://matplotlib.org/api/axis_api.html#matplotlib.axis.YAxis)实例，用于处理刻度线，刻度线标签和轴标签的布局和绘制

- [`ax.set_xlabel`](https://matplotlib.org/api/_as_gen/matplotlib.axes.Axes.set_xlabel.html#matplotlib.axes.Axes.set_xlabel)：将传递有关[`Text`](https://matplotlib.org/api/text_api.html#matplotlib.text.Text) 实例的信息[`XAxis`](https://matplotlib.org/api/axis_api.html#matplotlib.axis.XAxis)

```python
import numpy as np
import matplotlib.pyplot as plt

fig = plt.figure()
fig.subplots_adjust(top=0.8)

### 第一个子图
ax1 = fig.add_subplot(211)
ax1.set_ylabel('volts')
ax1.set_title('a sine wave')

t = np.arange(0.0, 1.0, 0.01)
s = np.sin(2*np.pi*t)
line, = ax1.plot(t, s, color='blue', lw=2)

# Fixing random state for reproducibility
np.random.seed(19680801)

### 第二个子图
ax2 = fig.add_axes([0.15, 0.1, 0.7, 0.3])
n, bins, patches = ax2.hist(np.random.randn(1000), 50,
                            facecolor='yellow', edgecolor='yellow')
ax2.set_xlabel('time (s)')

plt.show()
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210103100401.png" alt="image-20210103100401789" style="zoom:67%;" />







# 自定义对象

每个 [`Artist`](https://matplotlib.org/api/artist_api.html#matplotlib.artist.Artist)元素都有用于配置其外观的大量属性列表

- 图形本身包含图形 [`Rectangle`](https://matplotlib.org/api/_as_gen/matplotlib.patches.Rectangle.html#matplotlib.patches.Rectangle)的大小
- 图形的背景颜色和透明度
- 每个Axes边界框

实例存储为成员变量Figure.patch，Axes.patch  => 例如矩形，圆形和多边形



==每个matplotlib Artist具有以下属性==：

| Property           | Description                                                  |
| ------------------ | ------------------------------------------------------------ |
| ==alpha 透明度==   | The transparency - a scalar from 0-1                         |
| animated           | A boolean that is used to facilitate animated drawing<br>简化动画的绘制 |
| ==axes==           | The axes that the Artist lives in, **possibly None**         |
| clip_box 边框      | The bounding box that clips the Artist                       |
| clip_on 是否剪切   | Whether clipping is enabled                                  |
| clip_path 剪切路径 | The path the artist is clipped to                            |
| contains           | A picking function to test whether the artist contains the **pick point** |
| ==figure==         | The figure instance the artist lives in, **possibly None**   |
| ==label==          | A text label (e.g., for auto-labeling)                       |
| picker             | A python object that controls object picking                 |
| transform          | The transformation                                           |
| visible 可见       | A boolean whether the artist should be drawn<br>是否绘制该Artist |
| zorder             | A number which determines the drawing order                  |
| rasterized 栅格化  | Boolean; Turns vectors into raster graphics (for compression & eps transparency) |

个属性都可以通过老式的setter或getter来访问(是的，我们知道这会激怒Pythonista，我们计划支持通过属性或特征直接访问，但尚未完成)

```python
a = o.get_alpha()
o.set_alpha(0.5*a)

### 一次设置多个属性 => 参数
o.set(alpha=0.5, zorder=2)
```





## 检查属性 [`matplotlib.artist.getp()`](https://matplotlib.org/api/_as_gen/matplotlib.artist.getp.html#matplotlib.artist.getp)

```python
In [149]: matplotlib.artist.getp(fig.patch)
    alpha = 1.0
    animated = False
    antialiased or aa = True
    axes = None
    clip_box = None
    clip_on = False
    clip_path = None
    contains = None
    edgecolor or ec = w
    facecolor or fc = 0.75
    figure = Figure(8.125x6.125)
    fill = 1
    hatch = None
    height = 1
    label =
    linewidth or lw = 1.0
    picker = None
    transform = <Affine object at 0x134cca84>
    verts = ((0, 0), (0, 1), (1, 1), (1, 0))
    visible = True
    width = 1
    window_extent = <Bbox object at 0x134acbcc>
    x = 0
    y = 0
    zorder = 1
```







# 对象容器

尽管容器还具有一些属性，但是通常也要配置基元（Text 实例的字体，Line2D的宽度）

[`Axes`](https://matplotlib.org/api/axes_api.html#matplotlib.axes.Axes) [`Artist`](https://matplotlib.org/api/artist_api.html#matplotlib.artist.Artist) 是一个容器：包含图像中的许多基本元素primitives

容器本身也具有属性：

- xscale：'linear' or 'log'





## Figure container

- 顶层容器： [`matplotlib.figure.Figure`](https://matplotlib.org/api/_as_gen/matplotlib.figure.Figure.html#matplotlib.figure.Figure)
  - Figure.patch：图的背景 [`Rectangle`](https://matplotlib.org/api/_as_gen/matplotlib.patches.Rectangle.html#matplotlib.patches.Rectangle)
  - [`Figure.axes`](https://matplotlib.org/api/_as_gen/matplotlib.figure.Figure.html#matplotlib.figure.Figure.axes)
    - 子图（[`add_subplot()`](https://matplotlib.org/api/_as_gen/matplotlib.figure.Figure.html#matplotlib.figure.Figure.add_subplot)）
    - 轴（[`add_axes()`](https://matplotlib.org/api/_as_gen/matplotlib.figure.Figure.html#matplotlib.figure.Figure.add_axes)）



由于图保留了“当前轴”（请参见[`Figure.gca`](https://matplotlib.org/api/_as_gen/matplotlib.figure.Figure.html#matplotlib.figure.Figure.gca)和 [`Figure.sca`](https://matplotlib.org/api/_as_gen/matplotlib.figure.Figure.html#matplotlib.figure.Figure.sca)）的概念，以支持pylab / pyplot状态机，因此不应直接从轴列表中插入或删除轴，而应使用

- [`add_subplot()`](https://matplotlib.org/api/_as_gen/matplotlib.figure.Figure.html#matplotlib.figure.Figure.add_subplot)
- [`add_axes()`](https://matplotlib.org/api/_as_gen/matplotlib.figure.Figure.html#matplotlib.figure.Figure.add_axes)方法插入
- [`delaxes()`](https://matplotlib.org/api/_as_gen/matplotlib.figure.Figure.html#matplotlib.figure.Figure.delaxes)方法删除

```python
In [156]: fig = plt.figure()

In [157]: ax1 = fig.add_subplot(211)

In [158]: ax2 = fig.add_axes([0.1, 0.1, 0.7, 0.3])

In [159]: ax1
Out[159]: <matplotlib.axes.Subplot instance at 0xd54b26c>

In [160]: print(fig.axes)
[<matplotlib.axes.Subplot instance at 0xd54b26c>,
<matplotlib.axes.Axes instance at 0xd3f0b2c>]



### 遍历Axes列表
for ax in fig.axes:
    ax.grid(True)
```

Figure还具有自己的text, line, patches和images：可以直接添加基本元素primitives



### 坐标系

可以通过设置Artist的transform属性来控制它

图形坐标

- （0，0）是图形的左下角
- （1，1）是图形的右上角

```python
import matplotlib.lines as lines

fig = plt.figure()

l1 = lines.Line2D([0, 1], [0, 1], transform=fig.transFigure, figure=fig)
l2 = lines.Line2D([0, 1], [1, 0], transform=fig.transFigure, figure=fig)
fig.lines.extend([l1, l2])

plt.show()
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210103115609.png" alt="image-20210103115609122" style="zoom: 33%;" />



### Figure属性

| Figure attribute | Description                                                  |
| ---------------- | ------------------------------------------------------------ |
| axes             | A list of Axes instances (includes Subplot)                  |
| ==patch==        | The Rectangle background                                     |
| images           | A list of FigureImage patches - useful for raw pixel display |
| legends          | A list of Figure Legend instances (different from Axes.legends) |
| lines            | A list of Figure Line2D instances (rarely used, see Axes.lines) |
| ==patches==      | A list of Figure patches (rarely used, see Axes.patches)     |
| texts            | A list Figure Text instances                                 |





## Axes container

[`matplotlib.axes.Axes`](https://matplotlib.org/api/axes_api.html#matplotlib.axes.Axes)

```python
ax = fig.add_subplot(111)
rect = ax.patch  # a Rectangle instance
rect.set_facecolor('green')


### 调用.plot()时，创建一个matplotlib.lines.Line2D() 实例
In [213]: x, y = np.random.rand(2, 100)
In [214]: line, = ax.plot(x, y, '-', color='blue', linewidth=2)
    
### 调用.hist()
In [233]: n, bins, rectangles = ax.hist(np.random.randn(1000), 50)

In [234]: rectangles
Out[234]: <a list of 50 Patch objects>
In [235]: print(len(ax.patches))
```

注意：不应直接将对象添加到`Axes.lines`或 `Axes.patches`列表中

```python
In [262]: fig, ax = plt.subplots()

# create a rectangle instance
In [263]: rect = matplotlib.patches.Rectangle((1, 1), width=5, height=12)

# by default the axes instance is None
In [264]: print(rect.axes)
None

# and the transformation instance is set to the "identity transform"
In [265]: print(rect.get_transform())
<Affine object at 0x13695544>

# now we add the Rectangle to the Axes
In [266]: ax.add_patch(rect)

# and notice that the ax.add_patch method has set the axes
# instance
In [267]: print(rect.axes)
Axes(0.125,0.1;0.775x0.8)

# and the transformation has been set too
In [268]: print(rect.get_transform())
<Affine object at 0x15009ca4>

# the default axes transformation is ax.transData
In [269]: print(ax.transData)
<Affine object at 0x15009ca4>

# notice that the xlimits of the Axes have not been changed
In [270]: print(ax.get_xlim())
(0.0, 1.0)

# but the data limits have been updated to encompass the rectangle
In [271]: print(ax.dataLim.bounds)
(1.0, 1.0, 5.0, 12.0)

# we can manually invoke the auto-scaling machinery
In [272]: ax.autoscale_view()

# and now the xlim are updated to encompass the rectangle
In [273]: print(ax.get_xlim())
(1.0, 6.0)

# we have to manually force a figure draw
In [274]: ax.figure.canvas.draw()
```



### Axes方法、Artist与容器

| Helper method                  | Artist               | Container               |
| ------------------------------ | -------------------- | ----------------------- |
| ax.annotate - text annotations | Annotate             | ax.texts                |
| ax.bar - bar charts            | Rectangle            | ax.patches              |
| ax.errorbar - error bar plots  | Line2D and Rectangle | ax.lines and ax.patches |
| ax.fill - shared area          | Polygon              | ax.patches              |
| ax.hist - histograms           | Rectangle            | ax.patches              |
| ax.imshow - image data         | AxesImage            | ax.images               |
| ax.legend - axes legends       | Legend               | ax.legends              |
| ax.plot - xy plots             | Line2D               | ax.lines                |
| ax.scatter - scatter charts    | PolygonCollection    | ax.collections          |
| ax.text - text                 | Text                 | ax.texts                |

除此之外，两个重要的Artist容器：用于处理刻度线和标签

- [`XAxis`](https://matplotlib.org/api/axis_api.html#matplotlib.axis.XAxis)
- [`YAxis`](https://matplotlib.org/api/axis_api.html#matplotlib.axis.YAxis)

```python
for label in ax.get_xticklabels():
    label.set_color('orange')
```



### Axes属性

| Axes attribute | Description                            |
| -------------- | -------------------------------------- |
| artists        | A list of Artist instances             |
| patch          | Rectangle instance for Axes background |
| collections    | A list of Collection instances         |
| images         | A list of AxesImage                    |
| legends        | A list of Legend instances             |
| lines          | A list of Line2D instances             |
| ==patches==    | A list of Patch instances              |
| texts          | A list of Text instances               |
| xaxis          | matplotlib.axis.XAxis instance         |
| yaxis          | matplotlib.axis.YAxis instance         |





## Axis container

[`matplotlib.axis.Axis`](https://matplotlib.org/api/axis_api.html#matplotlib.axis.Axis)

- tick lines
- grid lines
- tick labels
- axis label
- auto-scaling, panning and zooming
- [`Locator`](https://matplotlib.org/api/ticker_api.html#matplotlib.ticker.Locator)实例
- [`Formatter`](https://matplotlib.org/api/ticker_api.html#matplotlib.ticker.Formatter)实例

每个Axis对象都包含一个label属性，以及刻度：

- [`xlabel`](https://matplotlib.org/api/_as_gen/matplotlib.pyplot.xlabel.html#matplotlib.pyplot.xlabel)
- [`ylabel`](https://matplotlib.org/api/_as_gen/matplotlib.pyplot.ylabel.html#matplotlib.pyplot.ylabel)
- [`axis.XTick`](https://matplotlib.org/api/axis_api.html#matplotlib.axis.XTick)
- [`axis.YTick`](https://matplotlib.org/api/axis_api.html#matplotlib.axis.YTick)

主要标签：[`axis.Axis.get_major_ticks`](https://matplotlib.org/api/_as_gen/matplotlib.axis.Axis.get_major_ticks.html#matplotlib.axis.Axis.get_major_ticks)

次要标签：[`axis.Axis.get_minor_ticks`](https://matplotlib.org/api/_as_gen/matplotlib.axis.Axis.get_minor_ticks.html#matplotlib.axis.Axis.get_minor_ticks)

```python
fig, ax = plt.subplots()
axis = ax.xaxis
axis.get_ticklocs()

axis.get_ticklabels()
axis.get_ticklines()

### 参数minor：返回次要标签
axis.get_ticklabels(minor=True)
axis.get_ticklines(minor=True)
```



### 访问方法

| Accessor method     | Description                                                  |
| ------------------- | ------------------------------------------------------------ |
| get_scale           | The scale of the axis, e.g., 'log' or 'linear'               |
| get_view_interval   | The interval instance of the axis view limits                |
| get_data_interval   | The interval instance of the axis data limits                |
| get_gridlines       | A list of grid lines for the Axis                            |
| get_label           | The axis label - a Text instance                             |
| get_ticklabels      | A list of Text instances - keyword minor=True\|False         |
| get_ticklines       | A list of Line2D instances - keyword minor=True\|False       |
| get_ticklocs        | A list of Tick locations - keyword minor=True\|False         |
| get_major_locator   | The [`ticker.Locator`](https://matplotlib.org/api/ticker_api.html#matplotlib.ticker.Locator) instance for major ticks |
| get_major_formatter | The [`ticker.Formatter`](https://matplotlib.org/api/ticker_api.html#matplotlib.ticker.Formatter) instance for major ticks |
| get_minor_locator   | The [`ticker.Locator`](https://matplotlib.org/api/ticker_api.html#matplotlib.ticker.Locator) instance for minor ticks |
| get_minor_formatter | The [`ticker.Formatter`](https://matplotlib.org/api/ticker_api.html#matplotlib.ticker.Formatter) instance for minor ticks |
| get_major_ticks     | A list of Tick instances for major ticks                     |
| get_minor_ticks     | A list of Tick instances for minor ticks                     |
| grid                | Turn the grid on or off for the major or minor ticks         |



### 自定义轴和刻度属性

```python
# plt.figure creates a matplotlib.figure.Figure instance
fig = plt.figure()
rect = fig.patch  # a rectangle instance
rect.set_facecolor('lightgoldenrodyellow')

ax1 = fig.add_axes([0.1, 0.3, 0.4, 0.4])
rect = ax1.patch
rect.set_facecolor('lightslategray')


for label in ax1.xaxis.get_ticklabels():
    # label is a Text instance
    label.set_color('red')
    label.set_rotation(45)
    label.set_fontsize(16)

for line in ax1.yaxis.get_ticklines():
    # line is a Line2D instance
    line.set_color('green')
    line.set_markersize(25)
    line.set_markeredgewidth(3)

plt.show()
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210103164600.png" alt="image-20210103164600052" style="zoom:67%;" />





## tick contrainer

[`matplotlib.axis.Tick`](https://matplotlib.org/api/axis_api.html#matplotlib.axis.Tick)

- tick实例
- grid line实例
- label实例





### tick属性

| Tick attribute | Description     |
| -------------- | --------------- |
| tick1line      | Line2D instance |
| tick2line      | Line2D instance |
| gridline       | Line2D instance |
| label1         | Text instance   |
| label2         | Text instance   |

```python
import numpy as np
import matplotlib.pyplot as plt

# Fixing random state for reproducibility
np.random.seed(19680801)

fig, ax = plt.subplots()
ax.plot(100*np.random.rand(20))

# Use automatic StrMethodFormatter
ax.yaxis.set_major_formatter('${x:1.2f}')  #使用美元符号设置右侧刻度线

#在yaxis的右侧将其颜色变为绿色
ax.yaxis.set_tick_params(which='major', labelcolor='green',
                         labelleft=False, labelright=True) 

plt.show()
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210103164926.png" alt="image-20210103164926425" style="zoom:67%;" />