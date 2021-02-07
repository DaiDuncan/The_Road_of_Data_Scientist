# 效果

类似plotly的图形：可以互动





# 案例|简单示例

定义路径

所有对象下面的[`matplotlib.patches`](https://matplotlib.org/api/patches_api.html#module-matplotlib.patches)对象是[`Path`](https://matplotlib.org/api/path_api.html#matplotlib.path.Path)

- 支持标准的moveto，lineto，curveto命令集，以绘制由线段和样条线组成的简单且复合的轮廓
- 使用Path（x，y）个顶点的（N，2）数组和路径代码的N长度数组实例化



```python
### 例如，要将单位矩形从（0，0）绘制为（1，1）
import matplotlib.pyplot as plt
from matplotlib.path import Path
import matplotlib.patches as patches

verts = [
   (0., 0.),  # left, bottom
   (0., 1.),  # left, top
   (1., 1.),  # right, top
   (1., 0.),  # right, bottom
   (0., 0.),  # ignored
]

codes = [
    Path.MOVETO,
    Path.LINETO,
    Path.LINETO,
    Path.LINETO,
    Path.CLOSEPOLY,
]

### 传入命令集codes
path = Path(verts, codes)

fig, ax = plt.subplots()
patch = patches.PathPatch(path, facecolor='orange', lw=2)
ax.add_patch(patch)
ax.set_xlim(-2, 2)
ax.set_ylim(-2, 2)
plt.show()
```

- 支持一下命令集

| Code        | Vertices                         | Description                                                  |
| ----------- | -------------------------------- | ------------------------------------------------------------ |
| `STOP`      | 1 (ignored)                      | A marker for the end of the entire path (currently not required and ignored). |
| `MOVETO`    | 1                                | Pick up the pen and move to the given vertex.                |
| `LINETO`    | 1                                | Draw a line from the current position to the given vertex.   |
| `CURVE3`    | 2: 1 control point, 1 end point  | Draw a quadratic Bézier curve from the current position, with the given control point, to the given end point. |
| `CURVE4`    | 3: 2 control points, 1 end point | Draw a cubic Bézier curve from the current position, with the given control points, to the given end point. |
| `CLOSEPOLY` | 1 (the point is ignored)         | Draw a line segment to the start point of the current polyline. |



# 案例|贝塞尔曲线

一些路径组件需要多个顶点来指定它们

CURVE 3是具有一个控制点和一个端点的[贝塞尔](https://en.wikipedia.org/wiki/Bézier_curve)曲线，而CURVE4具有两个控制点和端点的三个顶点。下面的示例显示了CURVE4Bézier样条曲线-贝塞尔曲线将包含在起点，两个控制点和终点的凸包中

```python
verts = [
   (0., 0.),   # P0
   (0.2, 1.),  # P1
   (1., 0.8),  # P2
   (0.8, 0.),  # P3
]

codes = [
    Path.MOVETO,
    Path.CURVE4,
    Path.CURVE4,
    Path.CURVE4,
]

path = Path(verts, codes)

fig, ax = plt.subplots()
patch = patches.PathPatch(path, facecolor='none', lw=2)
ax.add_patch(patch)

xs, ys = zip(*verts)
ax.plot(xs, ys, 'x--', lw=2, color='black', ms=10)

ax.text(-0.05, -0.05, 'P0')
ax.text(0.15, 1.05, 'P1')
ax.text(1.05, 0.85, 'P2')
ax.text(0.85, -0.05, 'P3')

ax.set_xlim(-0.1, 1.1)
ax.set_ylim(-0.1, 1.1)
plt.show()
```