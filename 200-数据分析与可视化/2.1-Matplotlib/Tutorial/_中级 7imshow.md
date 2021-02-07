[`imshow()`](https://matplotlib.org/api/_as_gen/matplotlib.axes.Axes.imshow.html#matplotlib.axes.Axes.imshow)允许您将图像（将进行色彩映射的2D数组（基于*norm*和*cmap*）或将按原样使用的3D RGB（A）数组）渲染到数据空间中的矩形区域





# 默认extent=None

```python
generate_imshow_demo_grid(extents=[None])
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210104163331.png" alt="image-20210104163331658" style="zoom:80%;" />

对于形状为（M，N）的数组，第一个索引沿垂直方向运行，第二个索引沿水平方向运行

| origin | [0, 0] position | extent                                   |
| ------ | --------------- | ---------------------------------------- |
| upper  | top left        | `(-0.5, numcols-0.5, numrows-0.5, -0.5)` |
| lower  | bottom left     | `(-0.5, numcols-0.5, -0.5, numrows-0.5)` |

`rcParams["image.origin"]` (default: `'upper'`)



```python
extents = [(-0.5, 6.5, -0.5, 5.5),
           (-0.5, 6.5, 5.5, -0.5),
           (6.5, -0.5, -0.5, 5.5),
           (6.5, -0.5, 5.5, -0.5)]

columns = generate_imshow_demo_grid(extents)
set_extent_None_text(columns['upper'][1])
set_extent_None_text(columns['lower'][0])
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210104163526.png" alt="image-20210104163526426" style="zoom:80%;" />



## 轴的限制

通过显式设置[`set_xlim`](https://matplotlib.org/api/_as_gen/matplotlib.axes.Axes.set_xlim.html#matplotlib.axes.Axes.set_xlim)/来 固定轴限制，则将[`set_ylim`](https://matplotlib.org/api/_as_gen/matplotlib.axes.Axes.set_ylim.html#matplotlib.axes.Axes.set_ylim)强制一定大小和方向的轴

- 坐标将锚定图像，然后将图像填充到朝向数据空间中该点的框。`(left, bottom)``(right, top)`
- 第一列始终最靠近“左侧”。
- *origin*控制第一行是最接近“ top”还是“ bottom”。
- 图像可以沿任一方向反转。
- 图像的“左右”和“上下”感可能与屏幕方向分离。

```python
generate_imshow_demo_grid(extents=[None] + extents,
                          xlim=(-2, 8), ylim=(-1, 6))

plt.show()
```

![range =，原点：上，原点：上，原点：上，原点：上，原点：上，原点：下，原点：下，原点：下，原点：下，原点：下](https://matplotlib.org/_images/sphx_glr_imshow_extent_003.png)