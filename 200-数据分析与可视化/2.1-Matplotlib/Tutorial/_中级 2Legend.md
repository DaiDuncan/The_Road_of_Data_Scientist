[`legend()`](https://matplotlib.org/api/_as_gen/matplotlib.pyplot.legend.html#matplotlib.pyplot.legend)  ：plt.legend([handles], [texts])

entry

- key：colored/patterned marker

- label：text 

handle：生成legend entry

```python
### 不带参数：自动获取图例句柄handles及其关联的标签labels
handles, labels = ax.get_legend_handles_labels()
ax.legend(handles, labels)


### 将适当的句柄直接传递给legend()
line_up, = plt.plot([1, 2, 3], label='Line 2')
line_down, = plt.plot([3, 2, 1], label='Line 1')
plt.legend(handles=[line_up, line_down])
# 等价于
plt.legend([line_up, line_down], ['Line Up', 'Line Down'])
```



- 并非所有句柄都可以自动转换为图例条目

```python
import matplotlib.patches as mpatches
import matplotlib.pyplot as plt

### 需要创建一个可以使用的Artist
#用红色表示的数据条目
red_patch = mpatches.Patch(color='red', label='The red data')  
plt.legend(handles=[red_patch])

plt.show()


#带有标记的线
import matplotlib.lines as mlines

blue_line = mlines.Line2D([], [], color='blue', marker='*',
                          markersize=15, label='Blue stars')
plt.legend(handles=[blue_line])
plt.show()
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210104141700.png" alt="image-20210104141700866" style="zoom:67%;" />

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210104141648.png" alt="image-20210104141648296" style="zoom:67%;" />





# legend loc

```python
plt.legend(bbox_to_anchor=(1, 1),
           bbox_transform=plt.gcf().transFigure)

plt.subplot(211) #第一行
plt.plot([1, 2, 3], label="test1")
plt.plot([3, 2, 1], label="test2")


# Place a legend above this subplot, expanding itself to
# fully use the given bounding box.
plt.legend(bbox_to_anchor=(0., 1.02, 1., .102), loc='lower left',
           ncol=2, mode="expand", borderaxespad=0.)

plt.subplot(223) #左下角
plt.plot([1, 2, 3], label="test1")
plt.plot([3, 2, 1], label="test2")
# Place a legend to the right of this smaller subplot.
plt.legend(bbox_to_anchor=(1.05, 1), loc='upper left', borderaxespad=0.)

plt.show()
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210104141800.png" alt="image-20210104141800358" style="zoom:67%;" />



## 同一个Axes上的多个legend

```python
line1, = plt.plot([1, 2, 3], label="Line 1", linestyle='--')
line2, = plt.plot([3, 2, 1], label="Line 2", linewidth=4)

# Create a legend for the first line.
first_legend = plt.legend(handles=[line1], loc='upper right')

### 添加第一个legend
# Add the legend manually to the current Axes.
ax = plt.gca().add_artist(first_legend)

### 添加第二个legend
# Create another legend for the second line.
plt.legend(handles=[line2], loc='lower right')

plt.show()
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210104142217.png" alt="image-20210104142217210" style="zoom:67%;" />



# legend Handlers

将句柄作为参数提供给适当的[`HandlerBase`](https://matplotlib.org/api/legend_handler_api.html#matplotlib.legend_handler.HandlerBase)子类

1. [`get_legend_handler_map()`](https://matplotlib.org/api/legend_api.html#matplotlib.legend.Legend.get_legend_handler_map) ：`handler_map`关键字
2. 检查handle`是否在新创建的`handler_map`中
3. 检查的`handle`类型是否在新创建的 `handler_map`中
4. 检查`handle`的mro中的任何类型是否在新创建的`handler_map`的类型中

```python
from matplotlib.legend_handler import HandlerLine2D

line1, = plt.plot([3, 2, 1], marker='o', label='Line 1')
line2, = plt.plot([1, 2, 3], marker='o', label='Line 2')

plt.legend(handler_map={line1: HandlerLine2D(numpoints=4)}) #设计line1的图例样式
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210104142510.png" alt="image-20210104142510559" style="zoom:67%;" />

- “第1行”现在有4个标记点
- “第2行”有2个标记点（默认）





除了用于复杂图类型的处理程序（如误差线，茎图和直方图）外，默认值`handler_map`还具有一个特殊的`tuple`处理程序（[`legend_handler.HandlerTuple`](https://matplotlib.org/api/legend_handler_api.html#matplotlib.legend_handler.HandlerTuple)），它简单地将给定元组中每个项目的句柄绘制在另一个之上。下面的示例演示了如何将两个图例键彼此叠加在一起 

```python
from numpy.random import randn

z = randn(10)

red_dot, = plt.plot(z, "ro", markersize=15)
# Put a white cross over some of the data.
white_cross, = plt.plot(z[:5], "w+", markeredgewidth=3, markersize=15)

plt.legend([red_dot, (red_dot, white_cross)], ["Attr A", "Attr A+B"])
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210104152236.png" alt="image-20210104152235967" style="zoom:67%;" />



```python
from matplotlib.legend_handler import HandlerLine2D, HandlerTuple

p1, = plt.plot([1, 2.5, 3], 'r-d')
p2, = plt.plot([3, 2, 1], 'k-o')

l = plt.legend([(p1, p2)], ['Two keys'], numpoints=1,
               handler_map={tuple: HandlerTuple(ndivide=None)})
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210104152757.png" alt="image-20210104152757227" style="zoom:67%;" />



## 自定义legend handler

`legend_artist`方法

```python
import matplotlib.patches as mpatches


class AnyObject:
    pass


class AnyObjectHandler:
    def legend_artist(self, legend, orig_handle, fontsize, handlebox):
        x0, y0 = handlebox.xdescent, handlebox.ydescent
        width, height = handlebox.width, handlebox.height
        patch = mpatches.Rectangle([x0, y0], width, height, facecolor='red',
                                   edgecolor='black', hatch='xx', lw=3,
                                   transform=handlebox.get_transform())
        handlebox.add_artist(patch)
        return patch


plt.legend([AnyObject()], ['My first handler'],
           handler_map={AnyObject: AnyObjectHandler()})
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210104152920.png" alt="image-20210104152920478" style="zoom:67%;" />



- 补充|例如，要产生椭圆的图例键，而不是矩形的图例键

```python
from matplotlib.legend_handler import HandlerPatch


class HandlerEllipse(HandlerPatch):
    def create_artists(self, legend, orig_handle,
                       xdescent, ydescent, width, height, fontsize, trans):
        center = 0.5 * width - 0.5 * xdescent, 0.5 * height - 0.5 * ydescent
        p = mpatches.Ellipse(xy=center, width=width + xdescent,
                             height=height + ydescent)
        self.update_prop(p, orig_handle, legend)
        p.set_transform(trans)
        return [p]


c = mpatches.Circle((0.5, 0.5), 0.25, facecolor="green",
                    edgecolor="red", linewidth=3)
plt.gca().add_patch(c)

plt.legend([c], ["An ellipse, not a rectangle"],
           handler_map={mpatches.Circle: HandlerEllipse()})
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210104153111.png" alt="image-20210104153111142" style="zoom:67%;" />

