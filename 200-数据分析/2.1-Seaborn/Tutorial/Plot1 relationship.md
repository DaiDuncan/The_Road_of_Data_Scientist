```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
sns.set_theme(style="darkgrid")
```

[`relplot()`](https://seaborn.pydata.org/generated/seaborn.relplot.html#seaborn.relplot)：

- [`scatterplot()`](https://seaborn.pydata.org/generated/seaborn.scatterplot.html#seaborn.scatterplot)(kind='scatter')
- [`lineplot()`](https://seaborn.pydata.org/generated/seaborn.lineplot.html#seaborn.lineplot)(kind='line')



# scatterplot() 强调关系

```python
tips = sns.load_dataset("tips")
sns.relplot(x="total_bill", y="tip", data=tips);

### 第三个变量：参数 颜色hue
sns.relplot(x="total_bill", y="tip", hue="smoker", data=tips);

### 第三个变量：参数 标记形状style
sns.relplot(x="total_bill", y="tip", hue="smoker", style="smoker",
            data=tips);

### 第四个变量：参数 颜色+形状
# 注意：眼睛对形状的敏感程度远不及对颜色的敏感程度
sns.relplot(x="total_bill", y="tip", hue="smoker", style="time", data=tips);


sns.relplot(x="total_bill", y="tip", hue="size", data=tips);
# 自定义调色板
sns.relplot(x="total_bill", y="tip", hue="size", palette="ch:r=-.5,l=.75", data=tips);

### 更改每个点的大小：参数 size
sns.relplot(x="total_bill", y="tip", size="size", data=tips);
# 将数据单位的值范围(15, 200)，归一化为面积单位的范围
sns.relplot(x="total_bill", y="tip", size="size", sizes=(15, 200), data=tips);
```



# lineplot() 强调连续性

```python
df = pd.DataFrame(dict(time=np.arange(500),
                       value=np.random.randn(500).cumsum()))
g = sns.relplot(x="time", y="value", kind="line", data=df)
g.fig.autofmt_xdate()

### 注意：默认绘图之前，已经对x值进行了排序
# 禁用默认设置：sort=False
df = pd.DataFrame(np.random.randn(500, 2).cumsum(axis=0), columns=["x", "y"])
sns.relplot(x="x", y="y", sort=False, kind="line", data=df);
```

当x未排序时：

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231110528.png" alt="image-20201231110528660" style="zoom:67%;" />



# Aggregation 表现不确定性

```python
fmri = sns.load_dataset("fmri")
### 参数ci：默认95%置信区间
sns.relplot(x="timepoint", y="signal", kind="line", data=fmri);
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231110633.png" alt="image-20201231110633315" style="zoom:67%;" />

```python
### 对于较大的数据集，这可能会占用大量时间
# 禁用ci=None
sns.relplot(x="timepoint", y="signal", ci=None, kind="line", data=fmri);

# ci='sd' 标准偏差
sns.relplot(x="timepoint", y="signal", kind="line", ci="sd", data=fmri);
```



```python
### 关闭预测估计
sns.relplot(x="timepoint", y="signal", estimator=None, kind="line", data=fmri);
```





# 设置属性

```python
### 三个变量：默认颜色区分
sns.relplot(x="timepoint", y="signal", hue="event", kind="line", data=fmri);

### 四个变量：颜色区分 + markers默认破折线区分
sns.relplot(x="timepoint", y="signal", hue="region", style="event",
            kind="line", data=fmri);

### 四个变量：颜色区分 + markers标记区分
sns.relplot(x="timepoint", y="signal", hue="region", style="event",
            dashes=False, markers=True, kind="line", data=fmri);

### 多次采样
sns.relplot(x="timepoint", y="signal", hue="region",
            units="subject", estimator=None,
            kind="line", data=fmri.query("event == 'stim'"));
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231111123.png" alt="image-20201231111123413" style="zoom:67%;" />

## 改变颜色

```python
from matplotlib.colors import LogNorm
palette = sns.cubehelix_palette(light=.7, n_colors=6)
### 参数hue_norm
sns.relplot(x="time", y="firing_rate",
            hue="coherence", style="choice",
            hue_norm=LogNorm(),
            kind="line",
            data=dots.query("coherence > 0"));
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231111505.png" alt="image-20201231111505209" style="zoom:67%;" />![image-20201231111516876](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231111517.png)



## 改变线宽

```python
### 参数size
sns.relplot(x="time", y="firing_rate",
            size="coherence", style="choice",
            kind="line", data=dots);
```

![image-20201231111516876](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231111517.png)



# 日期数据

利用matplotlib的功能来对刻度标签中的日期进行格式化，但是所有这些格式化都必须在matplotlib层进行

```python
df = pd.DataFrame(dict(time=pd.date_range("2017-1-1", periods=500),
                       value=np.random.randn(500).cumsum()))
g = sns.relplot(x="time", y="value", kind="line", data=df)
g.fig.autofmt_xdate()
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231111641.png" alt="image-20201231111641020" style="zoom:67%;" />



# 多个变量之间的关系

注意：[`relplot()`](https://seaborn.pydata.org/generated/seaborn.relplot.html#seaborn.relplot)基于[`FacetGrid`](https://seaborn.pydata.org/generated/seaborn.FacetGrid.html#seaborn.FacetGrid)

```python
sns.relplot(x="total_bill", y="tip", hue="smoker",
            col="time", data=tips);
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231111736.png" alt="image-20201231111736731" style="zoom:67%;" />![image-20201231111909088](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231111909.png)

```python
sns.relplot(x="timepoint", y="signal", hue="subject",
            col="region", row="event", height=3,
            kind="line", estimator=None, data=fmri);
'''
每个图的尺寸：height, aspect(长宽比)
'''
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231111933.png" alt="image-20201231111932922" style="zoom: 80%;" />![image-20201231112050790](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231112051.png)

```python
### 级别很多：分布到col上
sns.relplot(x="timepoint", y="signal", hue="event", style="event",
            col="subject", col_wrap=5,
            height=3, aspect=.75, linewidth=2.5,
            kind="line", data=fmri.query("region == 'frontal'"));
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201231112051.png" alt="image-20201231111932922" style="zoom: 80%;" />