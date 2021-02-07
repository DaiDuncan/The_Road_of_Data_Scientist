rc的意义：配置文件后缀名 e.g. '.xinitrc', '.vimrc' and '.bashrc'

-  they are automatically **R**un at startup and they **C**onfigure your stuff.



# [`style`](https://matplotlib.org/api/style_api.html#module-matplotlib.style)

Matplotlib提供了许多预定义的样式。例如，有一个预定义的样式称为“ ggplot”，它模仿了ggplot（R的流行绘图软件包）的美感

```python
import numpy as np
import matplotlib.pyplot as plt
import matplotlib as mpl
from cycler import cycler
plt.style.use('ggplot')
data = np.random.randn(50)


### 列出所有可用样式
print(plt.style.available)

['Solarize_Light2', '_classic_test_patch', 'bmh', 'classic', 'dark_background', 'fast', 'fivethirtyeight', 'ggplot', 'grayscale', 'seaborn', 'seaborn-bright', 'seaborn-colorblind', 'seaborn-dark', 'seaborn-dark-palette', 'seaborn-darkgrid', 'seaborn-deep', 'seaborn-muted', 'seaborn-notebook', 'seaborn-paper', 'seaborn-pastel', 'seaborn-poster', 'seaborn-talk', 'seaborn-ticks', 'seaborn-white', 'seaborn-whitegrid', 'tableau-colorblind10']
```





## 自定义风格

使用[`style.use`](https://matplotlib.org/api/style_api.html#matplotlib.style.use)样式表的路径或URL

```python
### ./images/presentation.mplstyle
axes.titlesize : 24
axes.labelsize : 20
lines.linewidth : 3
lines.markersize : 10
xtick.labelsize : 16
ytick.labelsize : 16
    
    
>>> import matplotlib.pyplot as plt
>>> plt.style.use('./images/presentation.mplstyle')


### 将<style-name>.mplstyle文件放入库mpl_configdir/stylelib
>>> import matplotlib.pyplot as plt
>>> plt.style.use(<style-name>)
```





## 临时试用风格|with

```python
with plt.style.context('dark_background'):
    plt.plot(np.sin(np.linspace(0, 2 * np.pi)), 'r-o')
plt.show()
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210102114823.png" alt="image-20210102114823343" style="zoom:67%;" />







# rcParams

所有rc设置都存储在名为的类似于字典的变量中[`matplotlib.rcParams`](https://matplotlib.org/api/matplotlib_configuration_api.html#matplotlib.rcParams)

[`matplotlib.rcdefaults`](https://matplotlib.org/api/matplotlib_configuration_api.html#matplotlib.rcdefaults) 将恢复标准的Matplotlib默认设置

## 动态rc设置

- python脚本
- python shell交互

```python
mpl.rcParams['lines.linewidth'] = 2
mpl.rcParams['lines.linestyle'] = '--'
plt.plot(data)

### 更改轴的颜色 .prop_cycle
mpl.rcParams['axes.prop_cycle'] = cycler(color=['r', 'g', 'b', 'y'])
plt.plot(data)  # first color is red


### 参数/关键字：一次更改多个元素
mpl.rc('lines', linewidth=4, linestyle='-.')
plt.plot(data)
```



## 静态rc设置|`matplotlibrc`文件

图形大小和DPI，线宽，颜色和样式，轴，轴和网格属性，文本和字体属性等



如果未通过调用指定URL或路径 `style.use('<path>/<style-name>.mplstyle')`，则Matplotlib将按`matplotlibrc`以下顺序在四个位置中查找 ：

1. matplotlibrc 当前工作目录
2. \$MATPLOTLIBRC如果是文件，则为else $MATPLOTLIBRC/matplotlibrc
3. 特定于用户的位置显示，具体取决于您的平台
   1. 在Linux和FreeBSD上，如果您已自定义环境，它将显示 `.config/matplotlib/matplotlibrc`（或 `$XDG_CONFIG_HOME/matplotlib/matplotlibrc`）。
   2. 在其他平台上，它显示为`.matplotlib/matplotlibrc`
4. *INSTALL*/matplotlib/mpl-data/matplotlibrc



```python
### 显示当前活动matplotlibrc文件的加载位置
>>> import matplotlib
>>> matplotlib.matplotlib_fname()
'/home/foo/.config/matplotlib/matplotlibrc'
```







# #[matplotlibrc文件](https://matplotlib.org/tutorials/introductory/customizing.html#matplotlibrc-sample)

