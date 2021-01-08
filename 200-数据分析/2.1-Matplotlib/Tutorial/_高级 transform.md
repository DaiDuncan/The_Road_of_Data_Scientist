| Coordinates      | Transformation object                                  | Description                                                  |
| ---------------- | ------------------------------------------------------ | ------------------------------------------------------------ |
| "data"           | `ax.transData`                                         | 数据的坐标系，由xlim和ylim控制                               |
| "axes"           | `ax.transAxes`                                         | [`Axes`](https://matplotlib.org/api/axes_api.html#matplotlib.axes.Axes)坐标系统:  (0, 0)在axes的左下角，(1, 1)在右上方 |
| "figure"         | `fig.transFigure                                       | [`Figure`](https://matplotlib.org/api/_as_gen/matplotlib.figure.Figure.html#matplotlib.figure.Figure)坐标系统:  (0, 0)在figure的左下角，(1, 1)在右上方 |
| "figure-inches"  | `fig.dpi_scale_trans`                                  | [`Figure`](https://matplotlib.org/api/_as_gen/matplotlib.figure.Figure.html#matplotlib.figure.Figure)坐标系统(英寸):  (0, 0)在figure的左下角，(width, height) 右上角(英寸) |
| "display"        | `None`, or `IdentityTransform()`                       | Pixel坐标系：(0, 0)在windows的左下角，(width, height) is top right of the display window in pixels. |
| "xaxis", "yaxis" | `ax.get_xaxis_transform()`, `ax.get_yaxis_transform()` | 混合坐标系：一个轴axis上使用data coordinates, 另一个轴axis上使用axes coordinates |



# data coordinates|数据本身

每当将数据添加到轴时，Matplotlib都会更新数据限制：

- [`set_xlim()`](https://matplotlib.org/api/_as_gen/matplotlib.axes.Axes.set_xlim.html#matplotlib.axes.Axes.set_xlim)
- [`set_ylim()`](https://matplotlib.org/api/_as_gen/matplotlib.axes.Axes.set_ylim.html#matplotlib.axes.Axes.set_ylim)

在下图中，数据限制在x轴上从0扩展到10，在y轴上从-1扩展到1

```python
import numpy as np
import matplotlib.pyplot as plt
import matplotlib.patches as mpatches

x = np.arange(0, 10, 0.005)
y = np.exp(-x/2.) * np.sin(2*np.pi*x)

fig, ax = plt.subplots()
ax.plot(x, y)
ax.set_xlim(0, 10)
ax.set_ylim(-1, 1)

plt.show()
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210105111856.png" alt="image-20210105111847582" style="zoom:67%;" />



可以使用ax.transData实例从您的转换 数据到您的显示坐标系，无论是单点或点的顺序

```python
In [14]: type(ax.transData)
Out[14]: <class 'matplotlib.transforms.CompositeGenericTransform'>

In [15]: ax.transData.transform((5, 0))
Out[15]: array([ 335.175,  247.   ])

In [16]: ax.transData.transform([(5, 0), (1, 2)])
Out[16]:
array([[ 335.175,  247.   ],
       [ 132.435,  642.2  ]])


### 逆transform
# 使用该inverted() 方法创建一个转换，从显示 坐标到数据坐标
In [41]: inv = ax.transData.inverted()

In [42]: type(inv)
Out[42]: <class 'matplotlib.transforms.CompositeGenericTransform'>

In [43]: inv.transform((335.175,  247.))
Out[43]: array([ 5.,  0.])
```





```python
x = np.arange(0, 10, 0.005)
y = np.exp(-x/2.) * np.sin(2*np.pi*x)

fig, ax = plt.subplots()
ax.plot(x, y)
ax.set_xlim(0, 10)
ax.set_ylim(-1, 1)

xdata, ydata = 5, 0
xdisplay, ydisplay = ax.transData.transform((xdata, ydata)) #不应该是[ 335.175,  247.   ]吗？ => 如果您使用不同的窗口大小或dpi设置，则显示坐标的确切值 可能会有所不同（显示的标记点可能与ipython会话中的显示点不同，因为文档图的默认大小不同）

bbox = dict(boxstyle="round", fc="0.8")
arrowprops = dict(
    arrowstyle="->",
    connectionstyle="angle,angleA=0,angleB=90,rad=10")

offset = 72
ax.annotate('data = (%.1f, %.1f)' % (xdata, ydata),
            (xdata, ydata), xytext=(-2*offset, offset), textcoords='offset points',
            bbox=bbox, arrowprops=arrowprops)

disp = ax.annotate('display = (%.1f, %.1f)' % (xdisplay, ydisplay),
                   (xdisplay, ydisplay), xytext=(0.5*offset, -offset),
                   xycoords='figure pixels',
                   textcoords='offset points',
                   bbox=bbox, arrowprops=arrowprops)

plt.show()
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210105112041.png" alt="image-20210105112041235" style="zoom:67%;" />



更改轴的x或y限制时，数据限制会更新，因此转换会产生一个新的显示点

当我们仅更改ylim时，只有y-display坐标会更改，而当我们更改xlim时，两者也会都更改

```python
In [54]: ax.transData.transform((5, 0))
Out[54]: array([ 335.175,  247.   ])

In [55]: ax.set_ylim(-1, 2)
Out[55]: (-1, 2)
In [56]: ax.transData.transform((5, 0))
Out[56]: array([ 335.175     ,  181.13333333])

    
In [57]: ax.set_xlim(10, 20)
Out[57]: (10, 20)
In [58]: ax.transData.transform((5, 0))
Out[58]: array([-171.675     ,  181.13333333])
```





# Axes coordinates|Axes对象

当在轴上放置文本时，此坐标系统非常有用，因为您通常希望将文本气泡放置在固定的位置（例如，轴窗格的左上角），并在平移或缩放时使该位置保持固定

```python
fig = plt.figure()
for i, label in enumerate(('A', 'B', 'C', 'D')):
    ax = fig.add_subplot(2, 2, i+1)
    ax.text(0.05, 0.95, label, transform=ax.transAxes,
            fontsize=16, fontweight='bold', va='top')

plt.show()
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210105112632.png" alt="image-20210105112631750" style="zoom:67%;" />



ax.transAxes用于放置文本的用处小

使用平移/缩放工具来移动，或手动更改数据xlim和ylim，您将看到数据移动，但是该圆将保持固定，因为它不在数据 坐标中，并且始终保持在轴的中心。

```python
fig, ax = plt.subplots()
x, y = 10*np.random.rand(2, 1000)
ax.plot(x, y, 'go', alpha=0.2)  # plot some data in data coordinates

circ = mpatches.Circle((0.5, 0.5), 0.25, transform=ax.transAxes,
                       facecolor='blue', alpha=0.75)
ax.add_patch(circ)
plt.show()
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210105112717.png" alt="image-20210105112717697" style="zoom:67%;" />



# 混合变换

例如，创建一个水平跨度，该跨度突出显示y数据的某些区域，但跨过x轴，而与数据限制，平移或缩放级别等无关其实这些混合线和跨度是如此有用

- [`axhline()`](https://matplotlib.org/api/_as_gen/matplotlib.axes.Axes.axhline.html#matplotlib.axes.Axes.axhline)
- [`axvline()`](https://matplotlib.org/api/_as_gen/matplotlib.axes.Axes.axvline.html#matplotlib.axes.Axes.axvline) 
- [`axhspan()`](https://matplotlib.org/api/_as_gen/matplotlib.axes.Axes.axhspan.html#matplotlib.axes.Axes.axhspan)
- [`axvspan()`](https://matplotlib.org/api/_as_gen/matplotlib.axes.Axes.axvspan.html#matplotlib.axes.Axes.axvspan)

```python
import matplotlib.transforms as transforms

fig, ax = plt.subplots()
x = np.random.randn(1000)

ax.hist(x, 30)
ax.set_title(r'$\sigma=1 \/ \dots \/ \sigma=2$', fontsize=16)

# the x coords of this transformation are data, and the y coord are axes
trans = transforms.blended_transform_factory(
    ax.transData, ax.transAxes)
# highlight the 1..2 stddev region with a span.
# We want x to be in data coordinates and y to span from 0..1 in axes coords.
rect = mpatches.Rectangle((1, 0), width=1, height=1, transform=trans,
                          color='yellow', alpha=0.5)
ax.add_patch(rect)

plt.show()
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210105112909.png" alt="image-20210105112909844" style="zoom:67%;" />





## x在*数据*坐标中而y在*坐标轴中*的混合变换 非常有用

- [`matplotlib.axes.Axes.get_xaxis_transform()`](https://matplotlib.org/api/_as_gen/matplotlib.axes.Axes.get_xaxis_transform.html#matplotlib.axes.Axes.get_xaxis_transform)
- [`matplotlib.axes.Axes.get_yaxis_transform()`](https://matplotlib.org/api/_as_gen/matplotlib.axes.Axes.get_yaxis_transform.html#matplotlib.axes.Axes.get_yaxis_transform)

上面的示例中，对的调用 blended_transform_factory()可以替换为get_xaxis_transform





## 绘制物理坐标

希望对象在图上具有一定的物理尺寸

如果以交互方式完成，则可以看到更改图形的大小不会更改圆与左下角的偏移，也不会更改其大小，并且无论轴的纵横比如何，该圆都将保持为圆形。

```python
fig, ax = plt.subplots(figsize=(5, 4))
x, y = 10*np.random.rand(2, 1000)
ax.plot(x, y*10., 'go', alpha=0.2)  # plot some data in data coordinates
# add a circle in fixed-coordinates
circ = mpatches.Circle((2.5, 2), 1.0, transform=fig.dpi_scale_trans,
                       facecolor='blue', alpha=0.75)
ax.add_patch(circ)
plt.show()
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210105113142.png" alt="image-20210105113142211" style="zoom:67%;" />

如果更改图形尺寸，则圆不会更改其绝对位置，而是会被裁剪

```python
fig, ax = plt.subplots(figsize=(7, 2)) #更改图形尺寸：数据不变，但是物理图形被裁剪
x, y = 10*np.random.rand(2, 1000)
ax.plot(x, y*10., 'go', alpha=0.2)  # plot some data in data coordinates
# add a circle in fixed-coordinates
circ = mpatches.Circle((2.5, 2), 1.0, transform=fig.dpi_scale_trans,
                       facecolor='blue', alpha=0.75)
ax.add_patch(circ)
plt.show()
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210105113209.png" alt="image-20210105113209204" style="zoom:67%;" />





第一个设置椭圆应该多大的缩放比例，第二个设置其位置

```python
trans = ScaledTranslation(xt, yt, scale_trans)
```

在交互式使用中，即使通过缩放更改了轴限制，椭圆也保持不变

```python
fig, ax = plt.subplots()
xdata, ydata = (0.2, 0.7), (0.5, 0.5)
ax.plot(xdata, ydata, "o")
ax.set_xlim((0, 1))

trans = (fig.dpi_scale_trans +
         transforms.ScaledTranslation(xdata[0], ydata[0], ax.transData))

# plot an ellipse around the point that is 150 x 130 points in diameter...
circle = mpatches.Ellipse((0, 0), 150/72, 130/72, angle=40,
                          fill=None, transform=trans)
ax.add_patch(circle)
plt.show()
```







## 使用偏移转换创造阴影效果

[`ScaledTranslation`](https://matplotlib.org/api/transformations.html#matplotlib.transforms.ScaledTranslation)

先在数据坐标（`ax.transData`）中绘制，然后使用偏移 `dx`和`dy`点`fig.dpi_scale_trans`。（在排版中，一个[点](https://en.wikipedia.org/wiki/Point_(typography))是1/72英寸，通过指定以[点](https://en.wikipedia.org/wiki/Point_(typography))为单位的偏移量，无论保存的dpi分辨率如何，您的图形都将看起来相同。）

```python
fig, ax = plt.subplots()

# make a simple sine wave
x = np.arange(0., 2., 0.01)
y = np.sin(2*np.pi*x)
line, = ax.plot(x, y, lw=3, color='blue')

# shift the object over 2 points, and down 2 points
dx, dy = 2/72., -2/72.
offset = transforms.ScaledTranslation(dx, dy, fig.dpi_scale_trans)
shadow_transform = ax.transData + offset

# now plot the same data with our offset transform;
# use the zorder to make sure we are below the line
ax.plot(x, y, lw=3, color='gray',
        transform=shadow_transform,
        zorder=0.5*line.get_zorder())

ax.set_title('creating a shadow effect with an offset transform')
plt.show()
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210105113352.png" alt="image-20210105113351802" style="zoom:67%;" />



# 改造管道

下述ax.transData：使用的转换是三个不同转换的组合

- self.transLimits是将您从数据转换为轴坐标的转换：将您的视图xlim和ylim映射到轴的单位空间（`transAxes`然后使用该单位空间显示空间）

```python
self.transData = self.transScale + (self.transLimits + self.transAxes)



In [80]: ax = subplot(111)

In [81]: ax.set_xlim(0, 10)
Out[81]: (0, 10)

In [82]: ax.set_ylim(-1, 1)
Out[82]: (-1, 1)

In [84]: ax.transLimits.transform((0, -1))
Out[84]: array([ 0.,  0.])

In [85]: ax.transLimits.transform((10, -1))
Out[85]: array([ 1.,  0.])

In [86]: ax.transLimits.transform((10, 1))
Out[86]: array([ 1.,  1.])

In [87]: ax.transLimits.transform((5, 0))
Out[87]: array([ 0.5,  0.5])
```

- 将单位 *轴*坐标转换回*数据*坐标

```python
In [90]: inv.transform((0.25, 0.25))
Out[90]: array([ 2.5, -0.5])
```

- self.transScale：负责数据的可选非线性缩放
  - 基本Matplotlib轴具有线性比例，因此仅将其设置为identity变换
  - 对数比例缩放函数（例如） semilogx()或使用显式将其设置为对数比例时set_xscale()，则将 ax.transScale属性设置为handle非线性投影



`transProjection`处理从空间（例如地图数据的纬度和经度，或极地数据的半径和theta）到可分离的笛卡尔坐标系的投影