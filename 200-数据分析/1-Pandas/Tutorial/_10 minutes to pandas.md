[Link: 更加详细的Cookbook](https://pandas.pydata.org/pandas-docs/stable/user_guide/cookbook.html#cookbook)

```python
import numpy as np
import pandas as pd
```

# 创建

```python
### 创建Series
s = pd.Series([1, 3, 5, np.nan, 6, 8])

### 创建DataFrame
dates = pd.date_range('20130101', periods=6)
df = pd.DataFrame(np.random.randn(6, 4), index=dates, columns=list('ABCD'))

df2 = pd.DataFrame({'A': 1.,
                    'B': pd.Timestamp('20130102'),
                    'C': pd.Series(1, index=list(range(4)), dtype='float32'),
                    'D': np.array([3] * 4, dtype='int32'),
                    'E': pd.Categorical(["test", "train", "test", "train"]),
                    'F': 'foo'})
df2.dtypes

### 复制
df3 = df2.copy()
```

补充|IPython自动补全：df2.<TAB>

```
df2.A                  df2.bool
df2.abs                df2.boxplot
df2.add                df2.C
df2.add_prefix         df2.clip
df2.add_suffix         df2.columns
df2.align              df2.copy
df2.all                df2.count
df2.any                df2.combine
df2.append             df2.D
df2.apply              df2.describe
df2.applymap           df2.diff
df2.B                  df2.duplicated
```



# 查(选择)/改/增/删

get/set/add/delete

```python
### getting 查找
# .at .iat 
# .loc .iloc

df['A'] #选择列：返回Series
df[0:3] #选择行
df['20130102':'20130104']

### 基于label的选择
df.loc[dates[0]]
df.loc[:, ['A', 'B']]
df.loc['20130102':'20130104', ['A', 'B']] #基于label，闭区间
df.loc['20130102', ['A', 'B']]
df.loc[dates[0], 'A'] #具体的某个value
df.at[dates[0], 'A'] #具体的某个value

### 基于position的选择(左闭右开)
df.iloc[3]
df.iloc[3:5, 0:2]
df.iloc[[1, 2, 4], [0, 2]]
df.iloc[1:3, :]
df.iloc[:, 1:3]
df.iloc[1, 1] #具体的某个value
df.iat[1, 1] #具体的某个value

### 布尔逻辑表达式
df[df['A'] > 0]
df[df > 0]
# .isin
df2[df2['E'].isin(['two', 'four'])]
```



```python
### setting
s1 = pd.Series([1, 2, 3, 4, 5, 6], index=pd.date_range('20130102', periods=6))

#直接设置
df['F'] = s1   

# 基于label
df.at[dates[0], 'A'] = 0

# 基于position
df.iat[0, 1] = 0

# numpy
df.loc[:, 'D'] = np.array([5] * len(df))

### 布尔逻辑表达式
df2 = df.copy()
df2[df2 > 0] = -df2
```





# 遍历/排序/统计

```python
### 遍历
df.head()
df.tail(3)
df.index
df.columns
df.describe() #统计数据
df.T


# DataFrame.to_numpy()：不包含index或者column的label
In [17]: df.to_numpy() #针对浮点型数据，效率高
Out[17]: 
array([[ 0.4691, -0.2829, -1.5091, -1.1356],
       [ 1.2121, -0.1732,  0.1192, -1.0442],
       [-0.8618, -2.1046, -0.4949,  1.0718],
       [ 0.7216, -0.7068, -1.0396,  0.2719],
       [-0.425 ,  0.567 ,  0.2762, -1.0874],
       [-0.6737,  0.1136, -1.4784,  0.525 ]])

In [18]: df2.to_numpy() #多种类型数据，效率很低
Out[18]: 
array([[1.0, Timestamp('2013-01-02 00:00:00'), 1.0, 3, 'test', 'foo'],
       [1.0, Timestamp('2013-01-02 00:00:00'), 1.0, 3, 'train', 'foo'],
       [1.0, Timestamp('2013-01-02 00:00:00'), 1.0, 3, 'test', 'foo'],
       [1.0, Timestamp('2013-01-02 00:00:00'), 1.0, 3, 'train', 'foo']],
      dtype=object)
```



```python
### 排序
df.sort_index(axis=1, ascending=False)
df.sort_values(by='B')
```



```python
df
                   A         B         C  D    F
2013-01-01  0.000000  0.000000 -1.509059  5  NaN
2013-01-02  1.212112 -0.173215  0.119209  5  1.0
2013-01-03 -0.861849 -2.104569 -0.494929  5  2.0
2013-01-04  0.721555 -0.706771 -1.039575  5  3.0
2013-01-05 -0.424972  0.567020  0.276232  5  4.0
2013-01-06 -0.673690  0.113648 -1.478427  5  5.0

s = pd.Series([1, 3, 5, np.nan, 6, 8], index=dates).shift(2)

### 基本运算Operations
df.sub(s, axis='index')   #减法运算：df-相应的Series => 自动广播
Out[65]: 
                   A         B         C    D    F
2013-01-01       NaN       NaN       NaN  NaN  NaN
2013-01-02       NaN       NaN       NaN  NaN  NaN
2013-01-03 -1.861849 -3.104569 -1.494929  4.0  1.0
2013-01-04 -2.278445 -3.706771 -4.039575  2.0  0.0
2013-01-05 -5.424972 -4.432980 -4.723768  0.0 -1.0
2013-01-06       NaN       NaN       NaN  NaN  NaN
```



```python
### 统计
df.mean()
df.mean(1)   #axis=1
s.value_counts()
```



# ⭐函数方法

四个层级：

- 表格级 .pipe()
- 行列级 .==apply()==
- 聚合 .aggregate()   .agg()   transform()
- 元素级 ==.applymap()==

```python
df.apply(np.cumsum)
df.apply(lambda x: x.max() - x.min())
```



## Python lambda表达式

[Python|基础](../../410-Python基础/Python-进阶 常用函数.md)

本质：是一个函数，但形式很简便@函数式编程风格

- ==apply==
- filter

- [Link: 菜鸟教程|==map==](https://www.runoob.com/python/python-func-map.html)
- reduce：自身迭代计算@冒泡排序法

map(func,list)，对list的**每个元素**分别执行func函数操作，显然func函数的参数就是单个元素。

reduce(func,list)，对list的每个元素都执行func函数操作，最后汇总成一个结果。

```python
### python方法
# 1.apply：这个函数将来会逐步淘汰,在未来版本中最终会消失
# 取消了appy方法的使用，例如，用function(* args, **kwargs) 代替了方法appy
# 对比：取消前
def function(a, b):
    print a, b
apply(function, ("whither", "canada?"))
apply(function, (1, 2 + 3))

# 对比：取消后
>>> def add(a, b):
	print(a,b)
>>> add('I am', ' albertshao')
I am  albertshao


# 2.map：大多数的for循环可以用map来代替
# map(func,seq)：对seq中的每个元素进行操作
array = [1, 2, 3]
square_array = []
for i in array:
    square_array.append(i ** 2)
    
# 用map更改为：
array = [1, 2, 3]
square_array = map(lambda i: i ** 2, array)
# 列表推导式
array = [1, 2, 3]
square_array = [i ** 2 for i in array]


# 3.filter：与map类似 
# filter(func,seq)，对seq中的元素进行过滤，返回符合条件的那些元素
# 比如返回array = [1, 2, 3, 4]中的所有奇数：
filter(lambda i: i % 2, array)   #对2取余，返回结果为True的元素：偶数为零，奇数不为零(返回)

# 列表推导式
print [i for i in array if i % 2 != 0]


# 4.reduce
# reduce(func,seq)，对seq中的每个元素进行func操作，最后汇总返回一个值
# 求array = [1, 2, 3]所有元素的和：
print reduce(lambda x, y: x + y, array)   #reduce会先将array里面的头两个数分别作为x和y，求它们的和，然后把它的结果和第三个相加，再把结果和第四个相加，直到最后一个元素。

# 求array = [1, 2, 3]中的最大值：
print reduce(lambda x, y: x if x > y else y, array)

# 求strings = ["abc", "abcd", "def"]中”abc”出现的总次数：
print reduce(lambda count, str: count + str.count("abc"), strings, 0) #第三个参数0是count的初始值
```





# 专题|缺失值 np.nan

```python
### 数据集
df1 = df.reindex(index=dates[0:4], columns=list(df.columns) + ['E'])
df1.loc[dates[0]:dates[1], 'E'] = 1

In [57]: df1
Out[57]: 
                   A         B         C  D    F    E
2013-01-01  0.000000  0.000000 -1.509059  5  NaN  1.0
2013-01-02  1.212112 -0.173215  0.119209  5  1.0  1.0
2013-01-03 -0.861849 -2.104569 -0.494929  5  2.0  NaN
2013-01-04  0.721555 -0.706771 -1.039575  5  3.0  NaN

# 查看缺失值
pd.isna(df1)
Out[60]: 
                A      B      C      D      F      E
2013-01-01  False  False  False  False   True  False
2013-01-02  False  False  False  False  False  False
2013-01-03  False  False  False  False  False   True
2013-01-04  False  False  False  False  False   True

# 应对方法：直接删除
df1.dropna(how='any')

# 应对方法：填充缺失值
df1.fillna(value=5)
```



# 专题|Merge

## concat

```python
df = pd.DataFrame(np.random.randn(10, 4))
In [74]: df
Out[74]: 
          0         1         2         3
0 -0.548702  1.467327 -1.015962 -0.483075
1  1.637550 -1.217659 -0.291519 -1.745505
2 -0.263952  0.991460 -0.919069  0.266046
3 -0.709661  1.669052  1.037882 -1.705775
4 -0.919854 -0.042379  1.247642 -0.009920
5  0.290213  0.495767  0.362949  1.548106
6 -1.131345 -0.089329  0.337863 -0.945867
7 -0.932132  1.956030  0.017587 -0.016692
8 -0.575247  0.254161 -1.143704  0.215897
9  1.193555 -0.077118 -0.408530 -0.862495

# break it into pieces
In [75]: pieces = [df[:3], df[3:7], df[7:]]
    
### concat()
pd.concat(pieces)
```

备注：DataFrame中

- 添加列效率高
- 添加行：首先需要copy

=> 推荐传递预定义(内置)的list，构造DataFrame constructer；而不是迭代添加records

[Link: 官方文档|Merge, join, concatenate and compare](https://pandas.pydata.org/pandas-docs/stable/user_guide/merging.html#merging-concatenation)



## join

```python
### join：SQL style merge
left = pd.DataFrame({'key': ['foo', 'foo'], 'lval': [1, 2]})
right = pd.DataFrame({'key': ['foo', 'foo'], 'rval': [4, 5]})

# 例1
In [79]: left
Out[79]: 
   key  lval
0  foo     1
1  foo     2

In [80]: right
Out[80]: 
   key  rval
0  foo     4
1  foo     5

In [81]: pd.merge(left, right, on='key')
Out[81]: 
   key  lval  rval
0  foo     1     4
1  foo     1     5
2  foo     2     4
3  foo     2     5


# 例2
In [82]: left = pd.DataFrame({'key': ['foo', 'bar'], 'lval': [1, 2]})

In [83]: right = pd.DataFrame({'key': ['foo', 'bar'], 'rval': [4, 5]})

In [84]: left
Out[84]: 
   key  lval
0  foo     1
1  bar     2

In [85]: right
Out[85]: 
   key  rval
0  foo     4
1  bar     5

In [86]: pd.merge(left, right, on='key')
Out[86]: 
   key  lval  rval
0  foo     1     4
1  bar     2     5
```



## grouping

推荐以下一个或多个步骤

- Splitting：讲数据分为独立的列@Udacity Proj4 分析星巴克消费数据
- Applying：函数应用(4个层级)
- Combining：组合结果

```python
### 数据集
In [87]: df = pd.DataFrame({'A': ['foo', 'bar', 'foo', 'bar',
   ....:                          'foo', 'bar', 'foo', 'foo'],
   ....:                    'B': ['one', 'one', 'two', 'three',
   ....:                          'two', 'two', 'one', 'three'],
   ....:                    'C': np.random.randn(8),
   ....:                    'D': np.random.randn(8)})
   ....: 

In [88]: df
Out[88]: 
     A      B         C         D
0  foo    one  1.346061 -1.577585
1  bar    one  1.511763  0.396823
2  foo    two  1.627081 -0.105381
3  bar  three -0.990582 -0.532532
4  foo    two -0.441652  1.453749
5  bar    two  1.211526  1.208843
6  foo    one  0.268520 -0.080952
7  foo  three  0.024580 -0.264610


df.groupby('A').sum()
df.groupby(['A', 'B']).sum()
```





# 专题|reshaping

## stack

 “compresses” a level in the DataFrame’s columns



## unstack

by default unstacks the last level:

```python
### 数据集
In [91]: tuples = list(zip(*[['bar', 'bar', 'baz', 'baz',
   ....:                      'foo', 'foo', 'qux', 'qux'],
   ....:                     ['one', 'two', 'one', 'two',
   ....:                      'one', 'two', 'one', 'two']]))
   ....: 

In [92]: index = pd.MultiIndex.from_tuples(tuples, names=['first', 'second'])

In [93]: df = pd.DataFrame(np.random.randn(8, 2), index=index, columns=['A', 'B'])

In [94]: df2 = df[:4]

In [95]: df2
Out[95]: 
                     A         B
first second                    
bar   one    -0.727965 -0.589346
      two     0.339969 -0.693205
baz   one    -0.339355  0.593616
      two     0.884345  1.591431
    
    
### stack
stacked = df2.stack()

In [97]: stacked
Out[97]: 
first  second   
bar    one     A   -0.727965
               B   -0.589346
       two     A    0.339969
               B   -0.693205
baz    one     A   -0.339355
               B    0.593616
       two     A    0.884345
               B    1.591431
dtype: float64

    
### unstack
In [98]: stacked.unstack()
Out[98]: 
                     A         B
first second                    
bar   one    -0.727965 -0.589346
      two     0.339969 -0.693205
baz   one    -0.339355  0.593616
      two     0.884345  1.591431

In [99]: stacked.unstack(1)
Out[99]: 
second        one       two
first                      
bar   A -0.727965  0.339969
      B -0.589346 -0.693205
baz   A -0.339355  0.884345
      B  0.593616  1.591431

In [100]: stacked.unstack(0)
Out[100]: 
first          bar       baz
second                      
one    A -0.727965 -0.339355
       B -0.589346  0.593616
two    A  0.339969  0.884345
       B -0.693205  1.591431
```



## pivot

```python
### 数据集
In [101]: df = pd.DataFrame({'A': ['one', 'one', 'two', 'three'] * 3,
   .....:                    'B': ['A', 'B', 'C'] * 4,
   .....:                    'C': ['foo', 'foo', 'foo', 'bar', 'bar', 'bar'] * 2,
   .....:                    'D': np.random.randn(12),
   .....:                    'E': np.random.randn(12)})
   .....: 

In [102]: df
Out[102]: 
        A  B    C         D         E
0     one  A  foo -1.202872  0.047609
1     one  B  foo -1.814470 -0.136473
2     two  C  foo  1.018601 -0.561757
3   three  A  bar -0.595447 -1.623033
4     one  B  bar  1.395433  0.029399
5     one  C  bar -0.392670 -0.542108
6     two  A  foo  0.007207  0.282696
7   three  B  foo  1.928123 -0.087302
8     one  C  foo -0.055224 -1.575170
9     one  A  bar  2.395985  1.771208
10    two  B  bar  1.552825  0.816482
11  three  C  bar  0.166599  1.100230


### .pivot_table()
In [103]: pd.pivot_table(df, values='D', index=['A', 'B'], columns=['C'])
Out[103]: 
C             bar       foo
A     B                    
one   A  2.395985 -1.202872
      B  1.395433 -1.814470
      C -0.392670 -0.055224
three A -0.595447       NaN
      B       NaN  1.928123
      C  0.166599       NaN
two   A       NaN  0.007207
      B  1.552825       NaN
      C       NaN  1.018601
```





# 专题|time series

特别适用于金融领域的应用

```python
### 数据集
rng = pd.date_range('1/1/2012', periods=100, freq='S')
ts = pd.Series(np.random.randint(0, 500, len(rng)), index=rng)

# resample
ts.resample('5Min').sum()

Out[106]: 
2012-01-01    24182
Freq: 5T, dtype: int64
        
        
ts_utc = ts.tz_localize('UTC')   #时区
ts_utc.tz_convert('US/Eastern')   #更改时区

### timedelta：方便数学计算
ps = ts.to_period()   #时间跨度
ps.to_timestamp()

prng = pd.period_range('1990Q1', '2000Q4', freq='Q-NOV')
ts = pd.Series(np.random.randn(len(prng)), prng)

ts.index = (prng.asfreq('M', 'e') + 1).asfreq('H', 's') + 9
ts.head()
```





# ⭐专题|文本数据

Series.str 自带字符串处理方法

pattern-matching基于正则表达式regular expressions

```python
s = pd.Series(['A', 'B', 'C', 'Aaba', 'Baca', np.nan, 'CABA', 'dog', 'cat'])
s.str.lower()
```



# 专题|分类数据

```python
df = pd.DataFrame({"id": [1, 2, 3, 4, 5, 6],
                   "raw_grade": ['a', 'b', 'b', 'a', 'a', 'e']})

df["grade"] = df["raw_grade"].astype("category")

In [125]: df["grade"]
Out[125]: 
0    a
1    b
2    b
3    a
4    a
5    e
Name: grade, dtype: category
Categories (3, object): ['a', 'b', 'e']
   

# rename
 df["grade"].cat.categories = ["very good", "good", "very bad"]
    
    
df["grade"] = df["grade"].cat.set_categories(["very bad", "bad", "medium",
                                              "good", "very good"])
In [128]: df["grade"]
Out[128]: 
0    very good
1         good
2         good
3    very good
4    very good
5     very bad
Name: grade, dtype: category
Categories (5, object): ['very bad', 'bad', 'medium', 'good', 'very good']
    
    
### 排序：基于分类中的顺序，而不是字幕顺序
In [129]: df.sort_values(by="grade")
Out[129]: 
   id raw_grade      grade
5   6         e   very bad
1   2         b       good
2   3         b       good
0   1         a  very good
3   4         a  very good
4   5         a  very good


### grouping会显示空的类别
In [130]: df.groupby("grade").size()
Out[130]: 
grade
very bad     1
bad          0
medium       0
good         2
very good    3
dtype: int64
```



# 实用|可视化

```python
import matplotlib.pyplot as plt
plt.close('all')

ts = pd.Series(np.random.randn(1000),
               index=pd.date_range('1/1/2000', periods=1000))
ts = ts.cumsum()
ts.plot()
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201208210344.png" alt="image-20201208210344381" style="zoom:67%;" />



# 实用|IO

## CSV

```python
df.to_csv('foo.csv')
pd.read_csv('foo.csv')
```



## HDF5

```python
df.to_hdf('foo.h5', 'df')
pd.read_hdf('foo.h5', 'df')
```



## Excel

```python
df.to_excel('foo.xlsx', sheet_name='Sheet1')
pd.read_excel('foo.xlsx', 'Sheet1', index_col=None, na_values=['NA'])
```





# 问题|陷阱

```python
### 涉及逻辑比较
if pd.Series([False, True, False]):
    print("I was true")
Traceback
    ...
ValueError: The truth value of an array is ambiguous. Use a.empty, a.any() or a.all().
```

