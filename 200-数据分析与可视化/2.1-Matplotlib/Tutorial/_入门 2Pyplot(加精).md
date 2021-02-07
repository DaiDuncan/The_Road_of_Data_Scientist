# 简介

matplotlib.pyplot是使matplotlib像MATLAB一样工作的函数的集合

pyplot API通常不如面向对象的API灵活

```python
import matplotlib.pyplot as plt
plt.plot([1, 2, 3, 4])
plt.ylabel('some numbers')
plt.show()
```





# 绘图样式

- 图的颜色的格式字符串
- 线型的格式字符串

格式字符串的字母和符号来自MATLAB

```python
plt.plot([1, 2, 3, 4], [1, 4, 9, 16], 'ro')
plt.axis([0, 6, 0, 20])
plt.show()


# evenly sampled time at 200ms intervals
t = np.arange(0., 5., 0.2)

# red dashes, blue squares and green triangles
plt.plot(t, t, 'r--', t, t**2, 'bs', t, t**3, 'g^')
plt.show()
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210102104304.png" alt="image-20210102104304469" style="zoom:67%;" />



## 线的特性 [`matplotlib.lines.Line2D`](https://matplotlib.org/api/_as_gen/matplotlib.lines.Line2D.html#matplotlib.lines.Line2D)

线宽，破折号样式，抗锯齿等

```python
plt.plot(x, y, linewidth=2.0)


### Line2D实例的setter方法
line, = plt.plot(x, y, '-')
line.set_antialiased(False) # turn off antialiasing

### step
lines = plt.plot(x1, y1, x2, y2)
# use keyword args
plt.setp(lines, color='r', linewidth=2.0)
# or MATLAB style string value pairs
plt.setp(lines, 'color', 'r', 'linewidth', 2.0)



### 获取可设置的线属性的列表
In [69]: lines = plt.plot([1, 2, 3])

In [70]: plt.setp(lines)
  alpha: float
  animated: [True | False]
  antialiased or aa: [True | False]
  ...snip
```

- [`Line2D`](https://matplotlib.org/api/_as_gen/matplotlib.lines.Line2D.html#matplotlib.lines.Line2D)属性

| Property                        | Value Type                                                |
| ------------------------------- | --------------------------------------------------------- |
| alpha                           | float                                                     |
| animated 动画                   | [True \| False]                                           |
| antialiased or aa               | [True \| False]                                           |
| clip_box                        | a matplotlib.transform.Bbox instance                      |
| clip_on                         | [True \| False]                                           |
| clip_path                       | a Path instance and a Transform instance, a Patch         |
| color or c 颜色                 | any matplotlib color                                      |
| contains                        | the hit testing function                                  |
| dash_capstyle                   | [`'butt'` | `'round'` | `'projecting'`]                   |
| dash_joinstyle                  | [`'miter'` | `'round'` | `'bevel'`]                       |
| dashes                          | sequence of on/off ink in points                          |
| ==data==                        | (np.array xdata, np.array ydata)                          |
| ==figure==                      | a matplotlib.figure.Figure instance                       |
| ==label==                       | any string                                                |
| ==linestyle or ls 线型==        | [ `'-'` | `'--'` | `'-.'` | `':'` | `'steps'` \| ...]     |
| ==linewidth or lw 线宽==        | float value in points                                     |
| ==marker== 标记                 | [ `'+'` | `','` | `'.'` | `'1'` | `'2'` | `'3'` | `'4'` ] |
| markeredgecolor or mec          | any matplotlib color                                      |
| markeredgewidth or mew          | float value in points                                     |
| markerfacecolor or mfc          | any matplotlib color                                      |
| ==markersize or ms 标记的大小== | float                                                     |
| markevery                       | [ None \| integer \| (startind, stride) ]                 |
| picker                          | used in interactive line selection                        |
| pickradius                      | the line pick selection radius                            |
| solid_capstyle                  | [`'butt'` | `'round'` | `'projecting'`]                   |
| solid_joinstyle                 | [`'miter'` | `'round'` | `'bevel'`]                       |
| transform                       | a matplotlib.transforms.Transform instance                |
| visible                         | [True \| False]                                           |
| xdata                           | np.array                                                  |
| ydata                           | np.array                                                  |
| zorder                          | any number                                                |







# 参数/关键字

- 参数data：
  - numpy.recarray
  - pandas.DataFrame

```python
data = {'a': np.arange(50),
        'c': np.random.randint(0, 50, 50),
        'd': np.random.randn(50)}
data['b'] = data['a'] + 10 * np.random.randn(50)
data['d'] = np.abs(data['d']) * 100

plt.scatter('a', 'b', c='c', s='d', data=data)
plt.xlabel('entry a')
plt.ylabel('entry b')
plt.show()
```





# 多个图形

```python
import matplotlib.pyplot as plt
plt.figure(1)                # the first figure
plt.subplot(211)             # the first subplot in the first figure
plt.plot([1, 2, 3])
plt.subplot(212)             # the second subplot in the first figure
plt.plot([4, 5, 6])


plt.figure(2)                # a second figure
plt.plot([4, 5, 6])          # creates a subplot(111) by default

plt.figure(1)                # figure 1 current; subplot(212) still current
plt.subplot(211)             # make subplot(211) in figure1 current
plt.title('Easy as 1, 2, 3') # subplot 211 title
```



- 清除当前图形
  - figure：clf
  - axes：cla





# 使用文本注释⭐

- xlabel
- ylabel
- title

所有[`text`](https://matplotlib.org/api/_as_gen/matplotlib.pyplot.text.html#matplotlib.pyplot.text)功能都返回一个[`matplotlib.text.Text`](https://matplotlib.org/api/text_api.html#matplotlib.text.Text) 实例

- 参数fontsize
- 参数color

```python
mu, sigma = 100, 15
x = mu + sigma * np.random.randn(10000)

# the histogram of the data
n, bins, patches = plt.hist(x, 50, density=1, facecolor='g', alpha=0.75)


plt.xlabel('Smarts')
plt.ylabel('Probability')
plt.title('Histogram of IQ')
# 文本的位置+文本内容
plt.text(60, .025, r'$\mu=100,\ \sigma=15$')
plt.axis([40, 160, 0, 0.03])
plt.grid(True)
plt.show()
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210102105031.png" alt="image-20210102105031469" style="zoom:67%;" />



```python
ax = plt.subplot(111)

t = np.arange(0.0, 5.0, 0.01)
s = np.cos(2*np.pi*t)
line, = plt.plot(t, s, lw=2)

# 文本用来注释绘图
plt.annotate('local max', xy=(2, 1), xytext=(3, 1.5),
             arrowprops=dict(facecolor='black', shrink=0.05),
             )

plt.ylim(-2, 2)
plt.show()
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210102105310.png" alt="image-20210102105310284" style="zoom:67%;" />



# 轴的尺度变化

plt.xscale（'log'）

```python
# Fixing random state for reproducibility
np.random.seed(19680801)

# make up some data in the open interval (0, 1)
y = np.random.normal(loc=0.5, scale=0.4, size=1000)
y = y[(y > 0) & (y < 1)]
y.sort()
x = np.arange(len(y))

# plot with various axes scales
plt.figure()

# linear
plt.subplot(221)
plt.plot(x, y)
plt.yscale('linear')
plt.title('linear')
plt.grid(True)

# log
plt.subplot(222)
plt.plot(x, y)
plt.yscale('log')
plt.title('log')
plt.grid(True)

# symmetric log
plt.subplot(223)
plt.plot(x, y - y.mean())
plt.yscale('symlog', linthresh=0.01)
plt.title('symlog')
plt.grid(True)

# logit
plt.subplot(224)
plt.plot(x, y)
plt.yscale('logit')
plt.title('logit')
plt.grid(True)
# Adjust the subplot layout, because the logit one may take more space
# than usual, due to y-tick labels like "1 - 10^{-3}"
plt.subplots_adjust(top=0.92, bottom=0.08, left=0.10, right=0.95, hspace=0.25,
                    wspace=0.35)

plt.show()
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210102105418.png" alt="image-20210102105418401" style="zoom:67%;" />