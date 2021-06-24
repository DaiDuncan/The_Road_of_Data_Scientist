> [定义](https://www.w3schools.com/python/numpy/numpy_random.asp)|关于随机：Random number does NOT mean a different number every time. Random means something that **can not be predicted logically**.
>
> - 伪随机与真随机
>   - 我们不需要真正的随机数，除非它与安全有关（如加密密钥）或应用的基础是随机性（如数字轮盘）

# 基本操作

## 1 生成随机数

```python
from numpy import random
np.random.seed(0)  # seed for reproducibility

# 0-100的整数
x = random.randint(100)

# 0-1的浮点数
x = random.rand()

# 矩阵
x = random.randint(100, size=(5))
x = random.rand(5)

x = random.randint(100, size=(3, 5))
x = random.rand(3, 5)
```



## 2 从数组中抽样.choice()

```python
from numpy import random

x = random.choice([3, 5, 7, 9])
x = random.choice([3, 5, 7, 9], size=(3, 5))
print(x)
```





# 数据分布

## 基于概率分布的采样

```python
from numpy import random

# 按照某种概率分布：注意概率之和为1
x = random.choice([3, 5, 7, 9], p=[0.1, 0.3, 0.6, 0.0], size=(100))
print(x)
```



## 重新排列

- .shuffle(arr) 就地改变
- .permutation(arr) 不改变原始array

```python
from numpy import random
import numpy as np

arr = np.array([1, 2, 3, 4, 5])
random.shuffle(arr)


arr = np.array([1, 2, 3, 4, 5])
print(random.permutation(arr))
```



## 正态/高斯分布

背景：It fits the probability distribution of many events, eg. IQ Scores, Heartbeat etc.

```python
random.normal()
'''
loc - (Mean) where the peak of the bell exists.
scale - (Standard Deviation) how flat the graph distribution should be.
size - The shape of the returned array.
'''

from numpy import random

x = random.normal(size=(2, 3))
x = random.normal(loc=1, scale=2, size=(2, 3))
```



```python
from numpy import random
import matplotlib.pyplot as plt
import seaborn as sns

sns.distplot(random.normal(size=1000), hist=False)
plt.show()
```

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210513121645.png" alt="img" style="zoom:50%;" />

## 离散|二项分布

背景：It describes the outcome of binary scenarios, e.g. toss of a coin, it will either be head or tails.

```python
random.binomial()
'''
n - number of trials.
p - probability of occurence of each trial (e.g. for toss of a coin 0.5 each).
size - The shape of the returned array.
'''
from numpy import random

x = random.binomial(n=10, p=0.5, size=10)
```

```python
from numpy import random
import matplotlib.pyplot as plt
import seaborn as sns

sns.distplot(random.binomial(n=10, p=0.5, size=1000), hist=True, kde=False)

plt.show()
```

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210513121832.png" alt="img" style="zoom:50%;" />

### vs. 正态分布

正态分布是连续的，但当数据点足够多的时候：二项分布与正态分布相似





## 离散|泊松分布

背景：It estimates how many times an event can happen in a specified time. 

- e.g. If someone eats twice a day what is probability he will eat thrice?

```python
'''
lam - rate or known number of occurences e.g. 2 for above problem.
size - The shape of the returned array.
'''
from numpy import random

x = random.poisson(lam=2, size=10)
```



```python
from numpy import random
import matplotlib.pyplot as plt
import seaborn as sns

sns.distplot(random.poisson(lam=2, size=1000), kde=False)
plt.show()
```

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210513122108.png" alt="img" style="zoom:50%;" />

### vs. 正态分布

类似二项分布与正态分布的区别：数据点足够多的时候，两个分布相类似





## 均匀分布

背景：Used to describe probability where every event has equal chances of occuring.

- E.g. Generation of random numbers.

```python
'''
a - lower bound - default 0 .0.
b - upper bound - default 1.0.
size - The shape of the returned array.
'''
from numpy import random

x = random.uniform(size=(2, 3))
```



```python
from numpy import random
import matplotlib.pyplot as plt
import seaborn as sns

sns.distplot(random.uniform(size=1000), hist=False)
plt.show()
```

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210513122241.png" alt="img" style="zoom:50%;" />

 



## Logistic分布(人口增长)

在ML、DL中广泛应用

```python
'''
loc - mean, where the peak is. Default 0.
scale - standard deviation, the flatness of distribution. Default 1.
size - The shape of the returned array.
'''
from numpy import random

x = random.logistic(loc=1, scale=2, size=(2, 3))
```



```python
from numpy import random
import matplotlib.pyplot as plt
import seaborn as sns

sns.distplot(random.logistic(size=1000), hist=False)
plt.show()
```

![img](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210513150754.png)

### vs. 正态分布

```python
from numpy import random
import matplotlib.pyplot as plt
import seaborn as sns

sns.distplot(random.normal(scale=2, size=1000), hist=False, label='normal')
sns.distplot(random.logistic(size=1000), hist=False, label='logistic')
plt.show()
```

![img](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210513150901.png)

这两个分布几乎相同，但逻辑分布的尾部面积更大，也就是说，它表示**离平均值更远的事件发生的可能性更大**。

对于较高的尺度值（标准差），除了峰值之外，正态分布和Logistic分布几乎是相同的。



## 超几何分布Multinomial

```python
'''
n - number of possible outcomes (e.g. 6 for dice roll).
pvals - list of probabilties of outcomes (e.g. [1/6, 1/6, 1/6, 1/6, 1/6, 1/6] for dice roll).
size - The shape of the returned array.
'''
from numpy import random

x = random.multinomial(n=6, pvals=[1/6, 1/6, 1/6, 1/6, 1/6, 1/6])
```



## 指数分布

背景：describing time till next event e.g. failure/success etc. => 下一次事件发生所需要的时间

```python
'''
scale - inverse of rate ( see lam in poisson distribution ) defaults to 1.0.
size - The shape of the returned array.
'''
from numpy import random

x = random.exponential(scale=2, size=(2, 3))
```



```python
from numpy import random
import matplotlib.pyplot as plt
import seaborn as sns

sns.distplot(random.exponential(size=1000), hist=False)
plt.show()
```

![img](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210513151234.png)

### 泊松分布 & 指数分布

泊松分布处理的是一个时间段内发生的事件的数量，而指数分布处理的是这些事件发生的时间间隔。



## Chi Square分布

背景：is used as a basis to verify the hypothesis.(验证假设)

```python
'''
df - (degree of freedom).
size - The shape of the returned array.
'''
from numpy import random

x = random.chisquare(df=2, size=(2, 3))
```



```python
from numpy import random
import matplotlib.pyplot as plt
import seaborn as sns

sns.distplot(random.chisquare(df=1, size=1000), hist=False)
plt.show()
```

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210513151428.png" alt="img" style="zoom:50%;" />



## Rayleigh分布

背景：is used in signal processing.

```python
'''
scale - (standard deviation) decides how flat the distribution will be default 1.0).
size - The shape of the returned array.
'''
from numpy import random

x = random.rayleigh(scale=2, size=(2, 3))
```



```python
from numpy import random
import matplotlib.pyplot as plt
import seaborn as sns

sns.distplot(random.rayleigh(size=1000), hist=False)
plt.show()
```

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210513151538.png" alt="img" style="zoom:67%;" />



### vs. Chi Square

在单位stddev下，2个自由度的Rayleigh和chi square代表相同的分布。



## Pareto帕累托分布

背景：Pareto's law i.e. 80-20 distribution (20% factors cause 80% outcome).

```python
'''
a - shape parameter.
size - The shape of the returned array.
'''
from numpy import random

x = random.pareto(a=2, size=(2, 3))
```



```python
from numpy import random
import matplotlib.pyplot as plt
import seaborn as sns

sns.distplot(random.pareto(a=2, size=1000), kde=False)
plt.show()
```

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210513151729.png" alt="img" style="zoom:50%;" />



## Zipf分布

背景：根据Zipf定律对数据进行抽样

> 拉兹夫定律：在一个集合中，第n个常用词是最常用词的1/n倍。
>
> 例如，英语中第5个常用词的出现率几乎是最常用词的1/5。

```python
'''
a - distribution parameter.
size - The shape of the returned array.
'''
from numpy import random

x = random.zipf(a=2, size=(2, 3))
```



```python
from numpy import random
import matplotlib.pyplot as plt
import seaborn as sns

x = random.zipf(a=2, size=1000)
sns.distplot(x[x<10], kde=False)
plt.show()
```

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210513151854.png" alt="img" style="zoom:67%;" />