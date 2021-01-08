使用[`mpl_toolkits.axes_grid1`](https://matplotlib.org/api/toolkits/axes_grid1.html#module-mpl_toolkits.axes_grid1)工具包控制图的布局



[ImageGrid](https://matplotlib.org/tutorials/toolkits/axes_grid.html#imagegrid)，[RGB Axes](https://matplotlib.org/tutorials/toolkits/axes_grid.html#rgb-axes)和 [AxesDivider](https://matplotlib.org/tutorials/toolkits/axes_grid.html#axesdivider)是帮助类，用于调整（多个）轴的位置

[ParasiteAxes](https://matplotlib.org/tutorials/toolkits/axes_grid.html#parasiteaxes) 提供了类似Twinx（或Twiny）的功能



---

[axes_grid1工具包概述](https://matplotlib.org/tutorials/toolkits/axes_grid.html#)

- [什么是axes_grid1工具包？](https://matplotlib.org/tutorials/toolkits/axes_grid.html#what-is-axes-grid1-toolkit)
- axes_grid1
  - [图像网格](https://matplotlib.org/tutorials/toolkits/axes_grid.html#imagegrid)
  - [AxesDivider类别](https://matplotlib.org/tutorials/toolkits/axes_grid.html#axesdivider-class)
  - 颜色条的高度（或宽度）与主轴同步
    - [使用AxesDivider的scatter_hist.py](https://matplotlib.org/tutorials/toolkits/axes_grid.html#scatter-hist-py-with-axesdivider)
  - ParasiteAxes 伴随轴 
  - - [例子1. twinx](https://matplotlib.org/tutorials/toolkits/axes_grid.html#example-1-twinx)
    - [例子2.双胞胎](https://matplotlib.org/tutorials/toolkits/axes_grid.html#example-2-twin)
  - [锚定艺术家](https://matplotlib.org/tutorials/toolkits/axes_grid.html#anchoredartists)
  - InsetLocator
    - [RGB轴](https://matplotlib.org/tutorials/toolkits/axes_grid.html#rgb-axes)
- [轴分频器](https://matplotlib.org/tutorials/toolkits/axes_grid.html#axesdivider)

---

# ImageGrid

```python
import matplotlib.pyplot as plt
from mpl_toolkits.axes_grid1 import ImageGrid
import numpy as np

im1 = np.arange(100).reshape((10, 10))
im2 = im1.T
im3 = np.flipud(im1)
im4 = np.fliplr(im2)

fig = plt.figure(figsize=(4., 4.))
grid = ImageGrid(fig, 111,  # similar to subplot(111)
                 nrows_ncols=(2, 2),  # creates 2x2 grid of axes
                 axes_pad=0.1,  # pad between axes in inch.
                 )

for ax, im in zip(grid, [im1, im2, im3, im4]):
    # Iterating over the grid returns the Axes.
    ax.imshow(im)

plt.show()
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210105204956.png" alt="image-20210105204956577" style="zoom:80%;" />





# AxesDivider类

在后台，ImageGrid类和RGBAxes类利用 [`AxesDivider`](https://matplotlib.org/api/_as_gen/mpl_toolkits.axes_grid1.axes_divider.AxesDivider.html#mpl_toolkits.axes_grid1.axes_divider.AxesDivider)该类，其作用是在绘制时计算轴的位置

axes_divider模块提供了一个辅助函数[`make_axes_locatable`](https://matplotlib.org/api/_as_gen/mpl_toolkits.axes_grid1.axes_divider.make_axes_locatable.html#mpl_toolkits.axes_grid1.axes_divider.make_axes_locatable)

```python
ax = subplot(1, 1, 1)
divider = make_axes_locatable(ax)
```





# 伴随轴

```python
from mpl_toolkits.axes_grid1 import host_subplot
import matplotlib.pyplot as plt

host = host_subplot(111)

par = host.twinx()

host.set_xlabel("Distance")
host.set_ylabel("Density")
par.set_ylabel("Temperature")

p1, = host.plot([0, 1, 2], [0, 1, 2], label="Density")
p2, = par.plot([0, 1, 2], [0, 3, 2], label="Temperature")

leg = plt.legend()

host.yaxis.get_label().set_color(p1.get_color())
leg.texts[0].set_color(p1.get_color())

par.yaxis.get_label().set_color(p2.get_color())
leg.texts[1].set_color(p2.get_color())

plt.show()
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210105205328.png" alt="image-20210105205328416" style="zoom:80%;" />



# RGB轴

```python
import numpy as np
from matplotlib import cbook
import matplotlib.pyplot as plt
from mpl_toolkits.axes_grid1.axes_rgb import make_rgb_axes, RGBAxes


def get_rgb():
    Z = cbook.get_sample_data("axes_grid/bivariate_normal.npy", np_load=True)
    Z[Z < 0] = 0.
    Z = Z / Z.max()

    R = Z[:13, :13]
    G = Z[2:, 2:]
    B = Z[:13, 2:]

    return R, G, B


def make_cube(r, g, b):
    ny, nx = r.shape
    R = np.zeros((ny, nx, 3))
    R[:, :, 0] = r
    G = np.zeros_like(R)
    G[:, :, 1] = g
    B = np.zeros_like(R)
    B[:, :, 2] = b

    RGB = R + G + B

    return R, G, B, RGB


def demo_rgb1():
    fig = plt.figure()
    ax = RGBAxes(fig, [0.1, 0.1, 0.8, 0.8], pad=0.0)
    r, g, b = get_rgb()
    ax.imshow_rgb(r, g, b)


def demo_rgb2():
    fig, ax = plt.subplots()
    ax_r, ax_g, ax_b = make_rgb_axes(ax, pad=0.02)

    r, g, b = get_rgb()
    im_r, im_g, im_b, im_rgb = make_cube(r, g, b)
    ax.imshow(im_rgb)
    ax_r.imshow(im_r)
    ax_g.imshow(im_g)
    ax_b.imshow(im_b)

    for ax in fig.axes:
        ax.tick_params(axis='both', direction='in')
        for sp1 in ax.spines.values():
            sp1.set_color("w")
        for tick in ax.xaxis.get_major_ticks() + ax.yaxis.get_major_ticks():
            tick.tick1line.set_markeredgecolor("w")
            tick.tick2line.set_markeredgecolor("w")


demo_rgb1()
demo_rgb2()

plt.show()
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210105205423.png" alt="image-20210105205423546" style="zoom:80%;" />![image-20210105205430198](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210105205430.png)

![image-20210105205430198](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210105205430.png)