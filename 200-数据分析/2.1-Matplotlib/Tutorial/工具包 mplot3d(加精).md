[mplot3d工具包](https://matplotlib.org/tutorials/toolkits/mplot3d.html#)

- 入门
  - [线图](https://matplotlib.org/tutorials/toolkits/mplot3d.html#line-plots)
  - [散点图](https://matplotlib.org/tutorials/toolkits/mplot3d.html#scatter-plots)
  - [线框图](https://matplotlib.org/tutorials/toolkits/mplot3d.html#wireframe-plots)
  - [表面图](https://matplotlib.org/tutorials/toolkits/mplot3d.html#surface-plots)
  - [三面图](https://matplotlib.org/tutorials/toolkits/mplot3d.html#tri-surface-plots)
  - [等高线图](https://matplotlib.org/tutorials/toolkits/mplot3d.html#contour-plots)
  - [填充轮廓图](https://matplotlib.org/tutorials/toolkits/mplot3d.html#filled-contour-plots)
  - [多边形图](https://matplotlib.org/tutorials/toolkits/mplot3d.html#polygon-plots)
  - [条形图](https://matplotlib.org/tutorials/toolkits/mplot3d.html#bar-plots)
  - [颤动](https://matplotlib.org/tutorials/toolkits/mplot3d.html#quiver)
  - [3D中的2D图](https://matplotlib.org/tutorials/toolkits/mplot3d.html#d-plots-in-3d)
  - [文本](https://matplotlib.org/tutorials/toolkits/mplot3d.html#text)
  - [子绘图](https://matplotlib.org/tutorials/toolkits/mplot3d.html#subplotting)

[`Axes3D`](https://matplotlib.org/api/_as_gen/mpl_toolkits.mplot3d.axes3d.Axes3D.html#mpl_toolkits.mplot3d.axes3d.Axes3D)通过将`projection="3d"` 关键字参数传递给来创建（类的）3D轴[`Figure.add_subplot`](https://matplotlib.org/api/_as_gen/matplotlib.figure.Figure.html#matplotlib.figure.Figure.add_subplot)：

```python
import matplotlib.pyplot as plt
fig = plt.figure()
ax = fig.add_subplot(111, projection='3d')
```



## 线图

```python
Axes3D.plot（*self*，*xs*，*ys*，** args*，*zdir = 'z'*，*** kwargs*）
'''
- xs(一维数组): x坐标
- ys(一维数组): y坐标
- zs(一维数组): z坐标
- zdir{'x'，'y'，'z'} 默认为'z': 用作z的方向
- kwarhs: matplotlib.axes.Axes.plot的参数
'''


import numpy as np
import matplotlib.pyplot as plt


plt.rcParams['legend.fontsize'] = 10

fig = plt.figure()
ax = fig.gca(projection='3d')

# Prepare arrays x, y, z
theta = np.linspace(-4 * np.pi, 4 * np.pi, 100)
z = np.linspace(-2, 2, 100)
r = z**2 + 1
x = r * np.sin(theta)
y = r * np.cos(theta)

ax.plot(x, y, z, label='parametric curve')
ax.legend()

plt.show()
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210105205927.png" alt="image-20210105205927627"  />





## [散点图](https://matplotlib.org/tutorials/toolkits/mplot3d.html#scatter-plots)

```python
Axes3D.scatter（self，xs，ys，zs = 0，zdir = 'z'，s = 20，c = None，depthshade = True，* args，** kwargs）
'''
- kwargs: scatter的参数
'''

import matplotlib.pyplot as plt
import numpy as np

# Fixing random state for reproducibility
np.random.seed(19680801)


def randrange(n, vmin, vmax):
    """
    Helper function to make an array of random numbers having shape (n, )
    with each number distributed Uniform(vmin, vmax).
    """
    return (vmax - vmin)*np.random.rand(n) + vmin

fig = plt.figure()
ax = fig.add_subplot(111, projection='3d')

n = 100

# For each set of style and range settings, plot n random points in the box
# defined by x in [23, 32], y in [0, 100], z in [zlow, zhigh].
for m, zlow, zhigh in [('o', -50, -25), ('^', -30, -5)]:
    xs = randrange(n, 23, 32)
    ys = randrange(n, 0, 100)
    zs = randrange(n, zlow, zhigh)
    ax.scatter(xs, ys, zs, marker=m)

ax.set_xlabel('X Label')
ax.set_ylabel('Y Label')
ax.set_zlabel('Z Label')

plt.show()
```

![image-20210105210515857](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210105210516.png)





## [线框图](https://matplotlib.org/tutorials/toolkits/mplot3d.html#wireframe-plots)

```python
Axes3D.plot_wireframe（self，X，Y，Z，* args，** kwargs）
# Line3DCollection

```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210105210806.png" alt="image-20210105210806696" style="zoom:80%;" />



## [表面图⭐](https://matplotlib.org/tutorials/toolkits/mplot3d.html#surface-plots)

```python
Axes3D.plot_surface（self，X，Y，Z，* args，norm = None，vmin = None，vmax = None，lightsource = None，** kwargs）
# Poly3DCollection
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210105210822.png" alt="image-20210105210822577" style="zoom:80%;" />



## [三面图](https://matplotlib.org/tutorials/toolkits/mplot3d.html#tri-surface-plots)

```python
Axes3D.plot_trisurf（self，* args，color = None，norm = None，vmin = None，vmax = None，lightsource = None，** kwargs）
# Poly3DCollection
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210105210843.png" alt="image-20210105210842955" style="zoom:80%;" />

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210105210905.png" alt="image-20210105210905093" style="zoom:80%;" />



## [等高线图⭐](https://matplotlib.org/tutorials/toolkits/mplot3d.html#contour-plots)

```python
Axes3D.contour（self，X，Y，Z，* args，extend3d = False，步幅= 5，zdir = 'z'，offset = None，** kwargs）
# matplotlib.axes.Axes.contour
```

![image-20210105210919226](C:\Users\Stein_2\AppData\Roaming\Typora\typora-user-images\image-20210105210919226.png)



## [填充等高线图](https://matplotlib.org/tutorials/toolkits/mplot3d.html#filled-contour-plots)

```python
Axes3D.contourf（self，X，Y，Z，* args，zdir = 'z'，offset = None，** kwargs）
# matplotlib.axes.Axes.contourf
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210105211016.png" alt="image-20210105211016321" style="zoom:80%;" />



## [多边形图](https://matplotlib.org/tutorials/toolkits/mplot3d.html#polygon-plots)

```python
Axes3D.add_collection3d（self，col，zs = 0，zdir = 'z'）

```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210105211040.png" alt="image-20210105211040670" style="zoom:80%;" />



## [条形图](https://matplotlib.org/tutorials/toolkits/mplot3d.html#bar-plots)
```python
Axes3D.bar（self，left，height，zs = 0，zdir = 'z'，* args，** kwargs）
# matplotlib.axes.Axes.bar
```

![image-20210105211103879](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210105211104.png)

## [流动图](https://matplotlib.org/tutorials/toolkits/mplot3d.html#quiver)

```python
Axes3D.quiver（X，Y，Z，U，V，W，/，长度= 1，arrow_length_ratio = 0.3，枢轴= '尾巴'，归一化= False，** kwargs）
# LineCollection
```

![image-20210105211237072](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210105211237.png)

## [文本](https://matplotlib.org/tutorials/toolkits/mplot3d.html#text)

```python
Axes3D.text（self，x，y，z，s，zdir = None，** kwargs）
```

![image-20210105211310238](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210105211310.png)

## [3D中的2D图](https://matplotlib.org/tutorials/toolkits/mplot3d.html#d-plots-in-3d)

![image-20210105211254498](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210105211254.png)

## [3D中的subplots](https://matplotlib.org/tutorials/toolkits/mplot3d.html#subplotting)

![image-20210105211330800](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210105211330.png)




