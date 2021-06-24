> 2008年创造pandas
>
> **Data Science:** is a branch of computer science where we study how to **store**, **use** and **analyze** data for ==deriving information from it==.

**Pandas 是 Python 语言的一个扩展程序库，用于数据分析。**

Pandas 是一个开放源码、BSD 许可的库，提供高性能、易于使用的数据结构和数据分析工具。

Pandas 名字衍生自术语 "panel data"（面板数据）和 "Python data analysis"（Python 数据分析）。

Pandas 一个强大的分析**结构化数据**的工具集，基础是 [Numpy](https://www.runoob.com/numpy/numpy-tutorial.html)（提供高性能的矩阵运算）。

Pandas 可以从各种文件格式比如 CSV、JSON、SQL、Microsoft Excel 导入数据。

Pandas 可以对各种数据进行运算操作，比如归并、再成形、选择，还有数据清洗和数据加工特征。

Pandas 广泛应用在**学术、金融、统计学**等各个数据分析领域。



Pandas 的主要数据结构是 Series （一维数据）与 DataFrame（二维数据），这两种数据结构足以处理金融、统计、社会科学、工程等领域里的大多数典型用例。

- 类似于一维数组的对象，由一组数据（各种Numpy数据类型）以及一组与之相关的数据标签（即索引）组成
- 一个表格型的数据结构，它含有一组有序的列，每列可以是不同的值类型（数值、字符串、布尔型值）
  - DataFrame 既有行索引也有列索引，它可以被看做由 Series 组成的字典（共同用一个索引）

```python
pip install pandas
>>> import pandas
>>> pandas.__version__  # 查看版本

mydataset = {
  'sites': ["Google", "Runoob", "Wiki"],
  'number': [1, 2, 3]
}

myvar = pd.DataFrame(mydataset)
print(myvar)
```



# 数据结构|Series

> 概念
>
> label => Ind

Pandas Series 类似表格中等一个列（column），类似于一维数组，可以保存任何数据类型 

Series 由索引（index）和列组成，函数如下：

```python
pandas.Series( data, index, dtype, name, copy)
'''
data：一组数据(ndarray 类型)。
index：数据索引标签，如果不指定，默认从 0 开始。
dtype：数据类型，默认会自己判断。

name：设置名称。
copy：拷贝数据，默认为 False。
'''
import pandas as pd

a = [1, 2, 3]
myvar = pd.Series(a)
print(myvar)


#==# 指定索引
a = ["Google", "Runoob", "Wiki"]
myvar = pd.Series(a, index = ["x", "y", "z"])
print(myvar)


#==# key-value创建，含参数name
sites = {1: "Google", 2: "Runoob", 3: "Wiki"}
myvar = pd.Series(sites, index = [1, 2], name="RUNOOB-Series-TEST")
print(myvar)
```

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210518091429.jpeg" alt="img" style="zoom:50%;" />





# 数据结构|DataFrame

DataFrame 是一个表格型的数据结构，它含有一组有序的列，每列可以是不同的值类型（数值、字符串、布尔型值）

- DataFrame 既有行索引也有列索引，它可以被看做由 Series 组成的字典（共同用一个索引）
  - 行索引 record/idx
  - 列索引 colname => Series

![img](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210518091550.png)

![img](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210518091635.png)

## 0 创建

```python
pandas.DataFrame( data, index, columns, dtype, copy)
'''
data：一组数据(ndarray、series, map, lists, dict 等类型)。
index：索引值，或者可以称为行标签。
columns：列标签，默认为 RangeIndex (0, 1, 2, …, n) 。

dtype：数据类型。
copy：拷贝数据，默认为 False。
'''
#==# 创建：使用列表
import pandas as pd

data = [['Google',10],['Runoob',12],['Wiki',13]]
df = pd.DataFrame(data,columns=['Site','Age'],dtype=float)
print(df)


#==# 创建：使用ndarrays
'''
ndarray的长度必须相同，如果传递了index，则索引的长度应等于数组的长度。
如果没有传递索引，则默认情况下，索引将是range(n)，其中n是数组长度。
'''
data = {'Site':['Google', 'Runoob', 'Wiki'], 'Age':[10, 12, 13]}
df = pd.DataFrame(data)
print (df)


#==# 创建: 字典（key/value），其中字典的 key 为列名:
data = [{'a': 1, 'b': 2},{'a': 5, 'b': 10, 'c': 20}]
df = pd.DataFrame(data)
print (df)
'''输出结果：没有对应的部分数据为 NaN
   a   b     c
0  1   2   NaN
1  5  10  20.0
'''
```



## 1 索引

Pandas 可以使用 **loc** 属性返回指定行的数据，如果没有设置索引，第一行索引为 **0**，第二行索引为 **1**，以此类推：

- **注意：**返回结果(某一行)其实就是一个 Pandas Series 数据。

```python
import pandas as pd

data = {
  "calories": [420, 380, 390],
  "duration": [50, 40, 45]
}

# 数据载入到 DataFrame 对象
df = pd.DataFrame(data)

# 返回第一行：返回结果(某一行)其实就是一个 Pandas Series 数据
print(df.loc[0])
# 返回第二行
print(df.loc[1])
'''输出结果：
calories    420
duration     50
Name: 0, dtype: int64
calories    380
duration     40
Name: 1, dtype: int64
'''

# 返回多行：返回结果其实就是一个 Pandas DataFrame 数据
print(df.loc[[0, 1]])
```

