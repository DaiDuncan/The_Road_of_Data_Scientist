操作|分组grouping

`data.groupby(['Year'])['Salary'].sum()`
`data.groupby(['Name'])['Salary'].sum()`
`data.groupby(['Year', 'Department'])['Salary'].sum()`
`data.groupby(['Year'])['Salary'].mean()`
`pandas.melt(df, id_vars=None, value_vars=None, var_name=None, value_name='value', col_level=None)`  保留指定列，其他列打散：类似汇总统计的逆操作



- 数据整理：进入特定分组
  `pd.cut()`
  1). 等距划分分组  pd.cut(ndarray, num) num表示划分的组数
  2). 设定分组，使得每个数据点归入到相应分组
  ![image](https://cdn.nlark.com/yuque/0/2020/png/1136179/1591174539156-9df863aa-2854-41e7-8755-b7d8b483bc75.png)

---

# 本质|split-apply-combine

起源：SQL Group by

```sql
SELECT Column1, Column2, mean(Column3), sum(Column4)
FROM SomeTable
GROUP BY Column1, Column2
```



## split

- Splitting the data into groups 最直接简单

- Applying a function to each group independently.

- Combining the results into a data structure.



## apply

### Aggregation

汇总一个或多个统计：

- sums 
- means
- sizes
- counts



### Transformation

- 正规化：比如zscore
- 填充缺失值



### Filtration

按照(逻辑)规则过滤



## combine

### 函数|官方文档

```python
DataFrame.groupby(by=None, axis=0, level=None, as_index=True, sort=True, group_keys=True, squeeze=<object object>, observed=False, dropna=True)

'''
axis和level配合：指明哪一个或几个column，或index
	默认axis=0:跨行 => 针对列
'''
```





# 分组 => Groupby对象

```python
In [1]: df = pd.DataFrame([('bird', 'Falconiformes', 389.0),
   ...:                    ('bird', 'Psittaciformes', 24.0),
   ...:                    ('mammal', 'Carnivora', 80.2),
   ...:                    ('mammal', 'Primates', np.nan),
   ...:                    ('mammal', 'Carnivora', 58)],
   ...:                   index=['falcon', 'parrot', 'lion', 'monkey', 'leopard'],
   ...:                   columns=('class', 'order', 'max_speed'))
   ...: 

In [2]: df
Out[2]: 
          class           order  max_speed
falcon     bird   Falconiformes      389.0
parrot     bird  Psittaciformes       24.0
lion     mammal       Carnivora       80.2
monkey   mammal        Primates        NaN
leopard  mammal       Carnivora       58.0
```

映射方法：

- Python函数
- list或者NumPy数组
- dict或Series
- DataFrame
  - df.groupby('A')默认是df['A']，但即可指column，也可指index level => 同时具备时，报错 

```python
### .difference：补集
In [10]: df2 = df.set_index(['A', 'B'])

In [11]: grouped = df2.groupby(level=df2.index.names.difference(['B']))

In [12]: grouped.sum()
Out[12]: 
            C         D
A                      
bar -1.591710 -1.739537
foo -0.752861 -1.402938
```



```python
### groupby对象
# 注意：pandas Index objects支持重复值 => 重复的index看作一组
In [15]: lst = [1, 2, 3, 1, 2, 3]

In [16]: s = pd.Series([1, 2, 3, 10, 20, 30], lst)
# groupby对象
In [17]: grouped = s.groupby(level=0) #默认axis=0，而level=0：Series只有一列，特殊对待 => 按照index分组(在DataFrame中针对column分组)
    
In [18]: grouped.first()
Out[18]: 
1    1
2    2
3    3
dtype: int64

In [19]: grouped.last()
Out[19]: 
1    10
2    20
3    30
dtype: int64

In [20]: grouped.sum()
Out[20]: 
1    11
2    22
3    33
dtype: int64
    

```



## groupby对象|排序

默认sort=True：对keys进行排序

```python
In [21]: df2 = pd.DataFrame({'X': ['B', 'B', 'A', 'A'], 'Y': [1, 2, 3, 4]})

In [22]: df2.groupby(['X']).sum()
Out[22]: 
   Y
X   
A  7
B  3

In [23]: df2.groupby(['X'], sort=False).sum()
Out[23]: 
   Y
X   
B  3
A  7

# 组内的数据顺序仍保持原样
In [24]: df3 = pd.DataFrame({'X': ['A', 'B', 'A', 'B'], 'Y': [1, 4, 3, 2]})

In [25]: df3.groupby(['X']).get_group('A')
Out[25]: 
   X  Y
0  A  1
2  A  3

In [26]: df3.groupby(['X']).get_group('B')
Out[26]: 
   X  Y
1  B  4
3  B  2
```



## groupby对象|dropna

```python
In [27]: df_list = [[1, 2, 3], [1, None, 4], [2, 1, 3], [1, 2, 2]]

In [28]: df_dropna = pd.DataFrame(df_list, columns=["a", "b", "c"])

In [29]: df_dropna
Out[29]: 
   a    b  c
0  1  2.0  3
1  1  NaN  4
2  2  1.0  3
3  1  2.0  2

# Default `dropna` is set to True, which will exclude NaNs in keys
In [30]: df_dropna.groupby(by=["b"], dropna=True).sum()
Out[30]: 
     a  c
b        
1.0  2  3
2.0  2  5

# In order to allow NaN in keys, set `dropna` to False
In [31]: df_dropna.groupby(by=["b"], dropna=False).sum()
Out[31]: 
     a  c
b        
1.0  2  3
2.0  2  5
NaN  1  4
```



## groupby对象|属性⭐

```python
### 属性.groups
In [32]: df.groupby('A').groups
Out[32]: {'bar': [1, 3, 5], 'foo': [0, 2, 4, 6, 7]}

In [33]: df.groupby(get_letter_type, axis=1).groups
Out[33]: {'consonant': ['B', 'C', 'D'], 'vowel': ['A']}
    
In [34]: grouped = df.groupby(['A', 'B'])

In [35]: grouped.groups
Out[35]: {('bar', 'one'): [1], ('bar', 'three'): [3], ('bar', 'two'): [5], ('foo', 'one'): [0, 6], ('foo', 'three'): [7], ('foo', 'two'): [2, 4]}

In [36]: len(grouped)
Out[36]: 6
    
### <TAB>展示所有属性：gb.<TAB>
In [37]: df
Out[37]: 
               height      weight  gender
2000-01-01  42.849980  157.500553    male
2000-01-02  49.607315  177.340407    male
2000-01-03  56.293531  171.524640    male
2000-01-04  48.421077  144.251986  female
2000-01-05  46.556882  152.526206    male
2000-01-06  68.448851  168.272968  female
2000-01-07  70.757698  136.431469    male
2000-01-08  58.909500  176.499753  female
2000-01-09  76.435631  174.094104  female
2000-01-10  45.306120  177.540920    male

In [38]: gb = df.groupby('gender')
    
In [39]: gb.<TAB>  # noqa: E225, E999
gb.agg        gb.boxplot    gb.cummin     gb.describe   gb.filter     gb.get_group  gb.height     gb.last       gb.median     gb.ngroups    gb.plot       gb.rank       gb.std        gb.transform
gb.aggregate  gb.count      gb.cumprod    gb.dtype      gb.first      gb.groups     gb.hist       gb.max        gb.min        gb.nth        gb.prod       gb.resample   gb.sum        gb.var
gb.apply      gb.cummax     gb.cumsum     gb.fillna     gb.gender     gb.head       gb.indices    gb.mean       gb.name       gb.ohlc       gb.quantile   gb.size       gb.tail       gb.weight
```

- gb.get_group
- gb.groups 
- gb.indices 



## groupby对象|MultiIndex

```python
### 示例：two-level MultiIndex
In [40]: arrays = [['bar', 'bar', 'baz', 'baz', 'foo', 'foo', 'qux', 'qux'],
   ....:           ['one', 'two', 'one', 'two', 'one', 'two', 'one', 'two']]
   ....: 

In [41]: index = pd.MultiIndex.from_arrays(arrays, names=['first', 'second'])

In [42]: s = pd.Series(np.random.randn(8), index=index)

In [43]: s
Out[43]: 
first  second
bar    one      -0.919854
       two      -0.042379
baz    one       1.247642
       two      -0.009920
foo    one       0.290213
       two       0.495767
qux    one       0.362949
       two       1.548106
dtype: float64
    
    
In [44]: grouped = s.groupby(level=0) #等价于s.groupby(level='second')
In [45]: grouped.sum()
Out[45]: 
first
bar   -0.962232
baz    1.237723
foo    0.785980
qux    1.911055
dtype: float64
    
    
In [47]: s.sum(level='second')
Out[47]: 
second
one    0.980950
two    1.991575
dtype: float64
    
### 针对多个Index
In [49]: s.groupby(level=['first', 'second']).sum()
Out[49]: 
first  second
bar    doo      -1.220674
baz    bee      -0.608004
foo    bop       1.023898
qux    bop       0.000895
dtype: float64
    
In [50]: s.groupby(['first', 'second']).sum()
```



# 分组|基于index levels或columns

```python
In [51]: arrays = [['bar', 'bar', 'baz', 'baz', 'foo', 'foo', 'qux', 'qux'],
   ....:           ['one', 'two', 'one', 'two', 'one', 'two', 'one', 'two']]
   ....: 

In [52]: index = pd.MultiIndex.from_arrays(arrays, names=['first', 'second'])

In [53]: df = pd.DataFrame({'A': [1, 1, 1, 1, 2, 2, 3, 3],
   ....:                    'B': np.arange(8)},
   ....:                   index=index)
   ....: 

In [54]: df
Out[54]: 
              A  B
first second      
bar   one     1  0
      two     1  1
baz   one     1  2
      two     1  3
foo   one     2  4
      two     2  5
qux   one     3  6
      two     3  7
# second index + A列
# 等价于df.groupby(['second', 'A']).sum()
In [55]: df.groupby([pd.Grouper(level=1), 'A']).sum()   # 等价level='second'
Out[55]: 
          B
second A   
one    1  2
       2  4
       3  6
two    1  4
       2  5
       3  7
```



## 分组后|选择列

```python
In [58]: grouped = df.groupby(['A'])
In [59]: grouped_C = grouped['C']   #等价于df['C'].groupby(df['A'])
In [60]: grouped_D = grouped['D']
    
In [61]: df['C'].groupby(df['A'])
Out[61]: <pandas.core.groupby.generic.SeriesGroupBy object at 0x7fbfaffe13a0>
```





## 分组后|遍历：基于groupby对象

- name
- group

```python
In [62]: grouped = df.groupby('A')

In [63]: for name, group in grouped:
   ....:     print(name)
   ....:     print(group)
   ....: 
bar
     A      B         C         D
1  bar    one  0.254161  1.511763
3  bar  three  0.215897 -0.990582
5  bar    two -0.077118  1.211526
foo
     A      B         C         D
0  foo    one -0.575247  1.346061
2  foo    two -1.143704  1.627081
4  foo    two  1.193555 -0.441652
6  foo    one -0.408530  0.268520
7  foo  three -0.862495  0.024580

### 如果是multiple keys：group name是元组
In [64]: for name, group in df.groupby(['A', 'B']):
   ....:     print(name)
   ....:     print(group)
   ....: 
('bar', 'one')
     A    B         C         D
1  bar  one  0.254161  1.511763
('bar', 'three')
     A      B         C         D
3  bar  three  0.215897 -0.990582
('bar', 'two')
     A    B         C         D
5  bar  two -0.077118  1.211526
('foo', 'one')
     A    B         C         D
0  foo  one -0.575247  1.346061
6  foo  one -0.408530  0.268520
('foo', 'three')
     A      B         C        D
7  foo  three -0.862495  0.02458
('foo', 'two')
     A    B         C         D
2  foo  two -1.143704  1.627081
4  foo  two  1.193555 -0.441652

```





## 分组后|选择组：get_group()

```python
In [65]: grouped.get_group('bar')
Out[65]: 
     A      B         C         D
1  bar    one  0.254161  1.511763
3  bar  three  0.215897 -0.990582
5  bar    two -0.077118  1.211526

### 如果是多列：传递参数 => 元组
In [66]: df.groupby(['A', 'B']).get_group(('bar', 'one'))
Out[66]: 
     A    B         C         D
1  bar  one  0.254161  1.511763
```





## 分组后|Aggregation

```python
In [67]: grouped = df.groupby('A')

In [68]: grouped.aggregate(np.sum)
Out[68]: 
            C         D
A                      
bar  0.392940  1.732707
foo -1.796421  2.824590


In [69]: grouped = df.groupby(['A', 'B'])

In [70]: grouped.aggregate(np.sum)
Out[70]: 
                  C         D
A   B                        
bar one    0.254161  1.511763
    three  0.215897 -0.990582
    two   -0.077118  1.211526
foo one   -0.983776  1.614581
    three -0.862495  0.024580
    two    0.049851  1.185429
```

聚合的结果将以分组的名称作为基于分组的轴的新索引。

对于多个keys，默认情况下，结果为 MultiIndex：但可以使用以下as_index选项更改

```python
In [71]: grouped = df.groupby(['A', 'B'], as_index=False)  #结果生成新的index：而分组的levels变为column

In [72]: grouped.aggregate(np.sum)
Out[72]: 
     A      B         C         D
0  bar    one  0.254161  1.511763
1  bar  three  0.215897 -0.990582
2  bar    two -0.077118  1.211526
3  foo    one -0.983776  1.614581
4  foo  three -0.862495  0.024580
5  foo    two  0.049851  1.185429

In [73]: df.groupby('A', as_index=False).sum()
Out[73]: 
     A         C         D
0  bar  0.392940  1.732707
1  foo -1.796421  2.824590


### as_index=False等价于.reset_index()
In [74]: df.groupby(['A', 'B']).sum().reset_index()
Out[74]: 
     A      B         C         D
0  bar    one  0.254161  1.511763
1  bar  three  0.215897 -0.990582
2  bar    two -0.077118  1.211526
3  foo    one -0.983776  1.614581
4  foo  three -0.862495  0.024580
5  foo    two  0.049851  1.185429
```

分组的聚合函数：所有的函数均排除NaN

| Function           | Description                                |
| :----------------- | :----------------------------------------- |
| `mean()`           | Compute mean of groups                     |
| `sum()`            | Compute sum of group values                |
| `size()`           | Compute group sizes                        |
| `count()`          | Compute count of group                     |
| `std()`            | Standard deviation of groups               |
| `var()`            | Compute variance of groups                 |
| `sem()`            | Standard error of the mean of groups       |
| `describe()`       | Generates descriptive statistics           |
| `first()`          | Compute first of group values              |
| `last()`           | Compute last of group values               |
| `nth()` 类似filter | Take nth value, or a subset if n is a list |
| `min()`            | Compute min of group values                |
| `max()`            | Compute max of group values                |

示例：df.groupby('A').agg(lambda ser: 1)



### 聚合多个函数

```python
### 针对Series：list或者dict映射
In [77]: grouped = df.groupby('A')

In [78]: grouped['C'].agg([np.sum, np.mean, np.std])
Out[78]: 
          sum      mean       std
A                                
bar  0.392940  0.130980  0.181231
foo -1.796421 -0.359284  0.912265


### 针对DataFrame：函数list => 每一列一个函数均可
In [79]: grouped.agg([np.sum, np.mean, np.std])
Out[79]: 
            C                             D                    
          sum      mean       std       sum      mean       std
A                                                              
bar  0.392940  0.130980  0.181231  1.732707  0.577569  1.366330
foo -1.796421 -0.359284  0.912265  2.824590  0.564918  0.884785


# 可重新命名函数运算之后的结果
# DataFrame同样操作
In [80]: (grouped['C'].agg([np.sum, np.mean, np.std])
   ....:              .rename(columns={'sum': 'foo',
   ....:                               'mean': 'bar',
   ....:                               'std': 'baz'}))
   ....: 
Out[80]: 
          foo       bar       baz
A                                
bar  0.392940  0.130980  0.181231
foo -1.796421 -0.359284  0.912265


### Pandas允许使用多个lambda函数
In [83]: grouped['C'].agg([lambda x: x.max() - x.min(),
   ....:                   lambda x: x.median() - x.mean()])
   ....: 
Out[83]: 
     <lambda_0>  <lambda_1>
A                          
bar    0.331279    0.084917
foo    2.337259   -0.215962
```

注意：

- 函数运算后，输出列名唯一
- 允许使用多个lambda函数



### 聚合函数结果的命名

- 值是元组，其第一个元素是要选择的列，第二个元素是要应用于该列的聚合

```python
In [84]: animals = pd.DataFrame({'kind': ['cat', 'dog', 'cat', 'dog'],
   ....:                         'height': [9.1, 6.0, 9.5, 34.0],
   ....:                         'weight': [7.9, 7.5, 9.9, 198.0]})
   ....: 

In [85]: animals
Out[85]: 
  kind  height  weight
0  cat     9.1     7.9
1  dog     6.0     7.5
2  cat     9.5     9.9
3  dog    34.0   198.0

### .agg()是一个元组 
In [86]: animals.groupby("kind").agg(
   ....:     min_height=pd.NamedAgg(column='height', aggfunc='min'),
   ....:     max_height=pd.NamedAgg(column='height', aggfunc='max'),
   ....:     average_weight=pd.NamedAgg(column='weight', aggfunc=np.mean),
   ....: )
   ....: 
Out[86]: 
      min_height  max_height  average_weight
kind                                        
cat          9.1         9.5            8.90
dog          6.0        34.0          102.75


# 上述.agg()可简写为：
In [87]: animals.groupby("kind").agg(
   ....:     min_height=('height', 'min'),
   ....:     max_height=('height', 'max'),
   ....:     average_weight=('weight', np.mean),
   ....: )
   ....: 
Out[87]: 
      min_height  max_height  average_weight
kind                                        
cat          9.1         9.5            8.90
dog          6.0        34.0          102.75


# 如果所需的输出列名称不是有效的python关键字，请构建字典并解压缩关键字参数
# **表示解压缩
In [88]: animals.groupby("kind").agg(**{
   ....:     'total weight': pd.NamedAgg(column='weight', aggfunc=sum),
   ....: })
   ....: 
Out[88]: 
      total weight
kind              
cat           17.8
dog          205.5


### .agg()也适用于Series groupby => 没有列选择，因此最终结果是一个值
In [89]: animals.groupby("kind").height.agg(
   ....:     min_height='min',
   ....:     max_height='max',
   ....: )
   ....: 
Out[89]: 
      min_height  max_height
kind                        
cat          9.1         9.5
dog          6.0        34.0
```



### 不同的函数 => 应用到不同的列

```python
In [90]: grouped.agg({'C': np.sum,
   ....:              'D': lambda x: np.std(x, ddof=1)})
   ....: 
Out[90]: 
            C         D
A                      
bar  0.392940  1.366330
foo -1.796421  0.884785


# 函数名称可以是字符串
In [91]: grouped.agg({'C': 'sum', 'D': 'std'})
Out[91]: 
            C         D
A                      
bar  0.392940  1.366330
foo -1.796421  0.884785
```





### Cython优化的聚合函数

目前仅对sum mean std sem进行了优化





## Transformation

```python
In [94]: index = pd.date_range('10/1/1999', periods=1100)

In [95]: ts = pd.Series(np.random.normal(0.5, 2, 1100), index)

In [96]: ts = ts.rolling(window=100, min_periods=100).mean().dropna()

In [97]: ts.head()
Out[97]: 
2000-01-08    0.779333
2000-01-09    0.778852
2000-01-10    0.786476
2000-01-11    0.782797
2000-01-12    0.798110
Freq: D, dtype: float64

In [98]: ts.tail()
Out[98]: 
2002-09-30    0.660294
2002-10-01    0.631095
2002-10-02    0.673601
2002-10-03    0.709213
2002-10-04    0.719369
Freq: D, dtype: float64

In [99]: transformed = (ts.groupby(lambda x: x.year)
   ....:                  .transform(lambda x: (x - x.mean()) / x.std()))
   ....: 
    
    
### 现在希望每个组中的平均值为0，标准差为1
# Original Data
In [100]: grouped = ts.groupby(lambda x: x.year)

In [101]: grouped.mean()
Out[101]: 
2000    0.442441
2001    0.526246
2002    0.459365
dtype: float64

In [102]: grouped.std()
Out[102]: 
2000    0.131752
2001    0.210945
2002    0.128753
dtype: float64

# Transformed Data
In [103]: grouped_trans = transformed.groupby(lambda x: x.year)

In [104]: grouped_trans.mean()
Out[104]: 
2000    1.168208e-15
2001    1.454544e-15
2002    1.726657e-15
dtype: float64

In [105]: grouped_trans.std()
Out[105]: 
2000    1.0
2001    1.0
2002    1.0
dtype: float64
    
    
# 视觉上比较差异
In [106]: compare = pd.DataFrame({'Original': ts, 'Transformed': transformed})

In [107]: compare.plot()
Out[107]: <AxesSubplot:>
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201217114841.png" alt="image-20201217114841513" style="zoom:67%;" />



```python
In [108]: ts.groupby(lambda x: x.year).transform(lambda x: x.max() - x.min())
Out[108]: 
2000-01-08    0.623893
2000-01-09    0.623893
2000-01-10    0.623893
2000-01-11    0.623893
2000-01-12    0.623893
                ...   
2002-09-30    0.558275
2002-10-01    0.558275
2002-10-02    0.558275
2002-10-03    0.558275
2002-10-04    0.558275
Freq: D, Length: 1001, dtype: float64
            

### 内置方法来产生相同的输出
In [109]: max = ts.groupby(lambda x: x.year).transform('max')
In [110]: min = ts.groupby(lambda x: x.year).transform('min')

In [111]: max - min
Out[111]: 
2000-01-08    0.623893
2000-01-09    0.623893
2000-01-10    0.623893
2000-01-11    0.623893
2000-01-12    0.623893
                ...   
2002-09-30    0.558275
2002-10-01    0.558275
2002-10-02    0.558275
2002-10-03    0.558275
2002-10-04    0.558275
Freq: D, Length: 1001, dtype: float64
            
            
### 常见应用 .transform(lambda x: x.fillna(x.mean()))
In [112]: data_df
Out[112]: 
            A         B         C
0    1.539708 -1.166480  0.533026
1    1.302092 -0.505754       NaN
2   -0.371983  1.104803 -0.651520
3   -1.309622  1.118697 -1.161657
4   -1.924296  0.396437  0.812436
..        ...       ...       ...
995 -0.093110  0.683847 -0.774753
996 -0.185043  1.438572       NaN
997 -0.394469 -0.642343  0.011374
998 -1.174126  1.857148       NaN
999  0.234564  0.517098  0.393534

[1000 rows x 3 columns]

In [113]: countries = np.array(['US', 'UK', 'GR', 'JP'])

In [114]: key = countries[np.random.randint(0, 4, 1000)]

In [115]: grouped = data_df.groupby(key)

# Non-NA count in each group
In [116]: grouped.count()
Out[116]: 
      A    B    C
GR  209  217  189
JP  240  255  217
UK  216  231  193
US  239  250  217

In [117]: transformed = grouped.transform(lambda x: x.fillna(x.mean()))
```





# 窗口和重采样操作

groupby方法：

- resample()
- expanding()
- rolling()

```python
In [125]: df_re = pd.DataFrame({'A': [1] * 10 + [5] * 10,
   .....:                       'B': np.arange(20)})
   .....: 

In [126]: df_re
Out[126]: 
    A   B
0   1   0
1   1   1
2   1   2
3   1   3
4   1   4
.. ..  ..
15  5  15
16  5  16
17  5  17
18  5  18
19  5  19

[20 rows x 2 columns]

### rolling：4个4个一组 => @类似窗函数卷积 
In [127]: df_re.groupby('A').rolling(4).B.mean()
Out[127]: 
A    
1  0      NaN
   1      NaN
   2      NaN
   3      1.5
   4      2.5
         ... 
5  15    13.5
   16    14.5
   17    15.5
   18    16.5
   19    17.5
Name: B, Length: 20, dtype: float64
            
            
### expanding():类似cumsum => 累加 
In [128]: df_re.groupby('A').expanding().sum()
Out[128]: 
         A      B
A                
1 0    1.0    0.0
  1    2.0    1.0
  2    3.0    3.0
  3    4.0    6.0
  4    5.0   10.0
...    ...    ...
5 15  30.0   75.0
  16  35.0   91.0
  17  40.0  108.0
  18  45.0  126.0
  19  50.0  145.0

[20 rows x 2 columns]


### resample()：获取数据帧每组中的每日频率
# .set_index('date')
In [129]: df_re = pd.DataFrame({'date': pd.date_range(start='2016-01-01', periods=4,
   .....:                                             freq='W'),
   .....:                       'group': [1, 1, 2, 2],
   .....:                       'val': [5, 6, 7, 8]}).set_index('date')
   .....: 

In [130]: df_re
Out[130]: 
            group  val
date                  
2016-01-03      1    5
2016-01-10      1    6
2016-01-17      2    7
2016-01-24      2    8

### 一维扩展
# group 1 => 03-10
# group 2 => 17-24
In [131]: df_re.groupby('group').resample('1D').ffill()
Out[131]: 
                  group  val
group date                  
1     2016-01-03      1    5
      2016-01-04      1    5
      2016-01-05      1    5
      2016-01-06      1    5
      2016-01-07      1    5
...                 ...  ...
2     2016-01-20      2    7
      2016-01-21      2    7
      2016-01-22      2    7
      2016-01-23      2    7
      2016-01-24      2    8

[16 rows x 2 columns]
```



# 过滤Filtration

```python
In [132]: sf = pd.Series([1, 1, 2, 3, 3, 3])

In [133]: sf.groupby(sf).filter(lambda x: x.sum() > 2)
Out[133]: 
3    3
4    3
5    3
dtype: int64
    
    
### 应用：组别数量>2
In [134]: dff = pd.DataFrame({'A': np.arange(8), 'B': list('aabbbbcc')})
# 可设置dropna=False
In [135]: dff.groupby('B').filter(lambda x: len(x) > 2)
Out[135]: 
   A  B
2  2  b
3  3  b
4  4  b
5  5  b


### 针对多列，必须指定哪一列作为过滤对象
In [137]: dff['C'] = np.arange(8)

In [138]: dff.groupby('B').filter(lambda x: len(x['C']) > 2)
Out[138]: 
   A  B  C
2  2  b  2
3  3  b  3
4  4  b  4
5  5  b  5
```



# 调用方法

```python
### 每个数据组上调用一个方法 => 很冗长，如果需要传递其他参数，可能会很不整洁
In [140]: grouped = df.groupby('A')

In [141]: grouped.agg(lambda x: x.std())
Out[141]: 
            C         D
A                      
bar  0.181231  1.366330
foo  0.912265  0.884785


### metaprogramming|元编程：dispatch
# function wrapper
In [142]: grouped.std()
Out[142]: 
            C         D
A                      
bar  0.181231  1.366330
foo  0.912265  0.884785


# .fillna
In [143]: tsdf = pd.DataFrame(np.random.randn(1000, 3),
   .....:                     index=pd.date_range('1/1/2000', periods=1000),
   .....:                     columns=['A', 'B', 'C'])
   .....: 

In [144]: tsdf.iloc[::2] = np.nan

In [145]: grouped = tsdf.groupby(lambda x: x.year)

In [146]: grouped.fillna(method='pad')  #原本奇数行都是np.nan，基于pad 前项填充
Out[146]: 
                   A         B         C
2000-01-01       NaN       NaN       NaN
2000-01-02 -0.353501 -0.080957 -0.876864
2000-01-03 -0.353501 -0.080957 -0.876864
2000-01-04  0.050976  0.044273 -0.559849
2000-01-05  0.050976  0.044273 -0.559849
...              ...       ...       ...
2002-09-22  0.005011  0.053897 -1.026922
2002-09-23  0.005011  0.053897 -1.026922
2002-09-24 -0.456542 -1.849051  1.559856
2002-09-25 -0.456542 -1.849051  1.559856
2002-09-26  1.123162  0.354660  1.128135

[1000 rows x 3 columns]

### 适用于Series
# nlargest()
# nsmallest()
In [147]: s = pd.Series([9, 8, 7, 5, 19, 1, 4.2, 3.3])
In [148]: g = pd.Series(list('abababab'))

In [149]: gb = s.groupby(g)

In [150]: gb.nlargest(3)
Out[150]: 
a  4    19.0
   0     9.0
   2     7.0
b  1     8.0
   3     5.0
   7     3.3
dtype: float64

In [151]: gb.nsmallest(3)
Out[151]: 
a  6    4.2
   2    7.0
   0    9.0
b  5    1.0
   7    3.3
   3    5.0
dtype: float64
```



# 灵活的apply

```python
# grouped['C'].apply(lambda x: x.describe())


### 自定义函数：用于DataFrame
In [155]: grouped = df.groupby('A')['C']
# 可以改变返回对象的维度
In [156]: def f(group):
   .....:     return pd.DataFrame({'original': group,
   .....:                          'demeaned': group - group.mean()})
   .....: 

In [157]: grouped.apply(f)
Out[157]: 
   original  demeaned
0 -0.575247 -0.215962
1  0.254161  0.123181
2 -1.143704 -0.784420
3  0.215897  0.084917
4  1.193555  1.552839
5 -0.077118 -0.208098
6 -0.408530 -0.049245
7 -0.862495 -0.503211


### 自定义函数：用于Series
In [158]: def f(x):
   .....:     return pd.Series([x, x ** 2], index=['x', 'x^2'])
   .....: 

In [159]: s = pd.Series(np.random.rand(5))

In [160]: s
Out[160]: 
0    0.321438
1    0.493496
2    0.139505
3    0.910103
4    0.194158
dtype: float64

In [161]: s.apply(f)
Out[161]: 
          x       x^2
0  0.321438  0.103323
1  0.493496  0.243538
2  0.139505  0.019462
3  0.910103  0.828287
4  0.194158  0.037697
```



# 拓展|Numba加速示例

如果将Numba作为可选依赖项安装，则transform和 aggregate方法支持engine='numba'和engine_kwargs参数。

engine_kwargs参数是关键字参数的字典，该参数将传递到 numba.jit装饰器中。

这些关键字参数将应用于传递的函数。目前只nogil，nopython和parallel支持，以及它们的默认值分别设置为False，True，False

在性能方面， 由于Numba会产生一些函数编译开销，因此首次使用Numba引擎运行函数会很慢。但是，已编译的函数将被缓存，并且随后的调用将很快。通常，Numba引擎具有大量数据点（例如1百万+）的性能。

```python
In [1]: N = 10 ** 3

In [2]: data = {0: [str(i) for i in range(100)] * N, 1: list(range(100)) * N}

In [3]: df = pd.DataFrame(data, columns=[0, 1])

In [4]: def f_numba(values, index):
   ...:     total = 0
   ...:     for i, value in enumerate(values):
   ...:         if i % 2:
   ...:             total += value + 5
   ...:         else:
   ...:             total += value * 2
   ...:     return total
   ...:

In [5]: def f_cython(values):
   ...:     total = 0
   ...:     for i, value in enumerate(values):
   ...:         if i % 2:
   ...:             total += value + 5
   ...:         else:
   ...:             total += value * 2
   ...:     return total
   ...:

In [6]: groupby = df.groupby(0)
# Run the first time, compilation time will affect performance
In [7]: %timeit -r 1 -n 1 groupby.aggregate(f_numba, engine='numba')  # noqa: E225
2.14 s ± 0 ns per loop (mean ± std. dev. of 1 run, 1 loop each)
# Function is cached and performance will improve
In [8]: %timeit groupby.aggregate(f_numba, engine='numba')
4.93 ms ± 32.3 µs per loop (mean ± std. dev. of 7 runs, 100 loops each)

In [9]: %timeit groupby.aggregate(f_cython, engine='cython')
18.6 ms ± 84.8 µs per loop (mean ± std. dev. of 7 runs, 100 loops each)
```





# 其他特征

```python
In [162]: df
Out[162]: 
     A      B         C         D
0  foo    one -0.575247  1.346061
1  bar    one  0.254161  1.511763
2  foo    two -1.143704  1.627081
3  bar  three  0.215897 -0.990582
4  foo    two  1.193555 -0.441652
5  bar    two -0.077118  1.211526
6  foo    one -0.408530  0.268520
7  foo  three -0.862495  0.024580

# 注意：B列看作nuisance(麻烦列)，被排除在外
In [163]: df.groupby('A').std()
Out[163]: 
            C         D
A                      
bar  0.181231  1.366330
foo  0.912265  0.884785
```

df.groupby('A').colname.std()效率比df.groupby('A').std().colname更高 => 提前选取目标列

备注：任何object列，都被看作麻烦列(在groupby中自动从聚合函数中排除)

```python
In [164]: from decimal import Decimal

In [165]: df_dec = pd.DataFrame(
   .....:     {'id': [1, 2, 1, 2],
   .....:      'int_column': [1, 2, 3, 4],
   .....:      'dec_column': [Decimal('0.50'), Decimal('0.15'),
   .....:                     Decimal('0.25'), Decimal('0.40')]
   .....:      }
   .....: )
   .....: 

# Decimal columns can be sum'd explicitly by themselves...
### 只有特别针对dec_column列时，才不会被排除
In [166]: df_dec.groupby(['id'])[['dec_column']].sum()
Out[166]: 
   dec_column
id           
1        0.75
2        0.55

# ...but cannot be combined with standard data types or they will be excluded
In [167]: df_dec.groupby(['id'])[['int_column', 'dec_column']].sum()
Out[167]: 
    int_column
id            
1            4
2            6

# Use .agg function to aggregate over standard and "nuisance" data types
# at the same time
### 特别指明dec_column列
In [168]: df_dec.groupby(['id']).agg({'int_column': 'sum', 'dec_column': 'sum'})
Out[168]: 
    int_column dec_column
id                       
1            4       0.75
2            6       0.55
```



# 处理(未)观察到的分类项

```python
In [169]: pd.Series([1, 1, 1]).groupby(pd.Categorical(['a', 'a', 'a'],
   .....:                                             categories=['a', 'b']),
   .....:                              observed=False).count()
   .....: 
Out[169]: 
a    3
b    0
dtype: int64

# observed=True 只考虑存在的项
In [170]: pd.Series([1, 1, 1]).groupby(pd.Categorical(['a', 'a', 'a'],
   .....:                                             categories=['a', 'b']),
   .....:                              observed=True).count()
   .....: 
Out[170]: 
a    3
dtype: int64 
    
    
In [171]: s = pd.Series([1, 1, 1]).groupby(pd.Categorical(['a', 'a', 'a'],
   .....:                                                 categories=['a', 'b']),
   .....:                                  observed=False).count()
   .....: 

In [172]: s.index.dtype
Out[172]: CategoricalDtype(categories=['a', 'b'], ordered=False)
```



# 处理缺失值|NA NaT

分组key中有NaN或NaT值，这些值将被自动排除





# 分组|按特定顺序

```python
In [173]: data = pd.Series(np.random.randn(100))
### 按四分位分组：前提 => 已排序
In [174]: factor = pd.qcut(data, [0, .25, .5, .75, 1.])

In [175]: data.groupby(factor).mean()
Out[175]: 
(-2.645, -0.523]   -1.362896
(-0.523, 0.0296]   -0.260266
(0.0296, 0.654]     0.361802
(0.654, 2.21]       1.073801
dtype: float64
```



# 分组|按特定描述：比如时间

```python
In [176]: import datetime

In [177]: df = pd.DataFrame({'Branch': 'A A A A A A A B'.split(),
   .....:                    'Buyer': 'Carl Mark Carl Carl Joe Joe Joe Carl'.split(),
   .....:                    'Quantity': [1, 3, 5, 1, 8, 1, 9, 3],
   .....:                    'Date': [
   .....:                        datetime.datetime(2013, 1, 1, 13, 0),
   .....:                        datetime.datetime(2013, 1, 1, 13, 5),
   .....:                        datetime.datetime(2013, 10, 1, 20, 0),
   .....:                        datetime.datetime(2013, 10, 2, 10, 0),
   .....:                        datetime.datetime(2013, 10, 1, 20, 0),
   .....:                        datetime.datetime(2013, 10, 2, 10, 0),
   .....:                        datetime.datetime(2013, 12, 2, 12, 0),
   .....:                        datetime.datetime(2013, 12, 2, 14, 0)]
   .....:                    })
   .....: 

In [178]: df
Out[178]: 
  Branch Buyer  Quantity                Date
0      A  Carl         1 2013-01-01 13:00:00
1      A  Mark         3 2013-01-01 13:05:00
2      A  Carl         5 2013-10-01 20:00:00
3      A  Carl         1 2013-10-02 10:00:00
4      A   Joe         8 2013-10-01 20:00:00
5      A   Joe         1 2013-10-02 10:00:00
6      A   Joe         9 2013-12-02 12:00:00
7      B  Carl         3 2013-12-02 14:00:00
       
### pd.Grouper(freq='1M', key='Date')是对时间序列的特定描述
In [179]: df.groupby([pd.Grouper(freq='1M', key='Date'), 'Buyer']).sum()
Out[179]: 
                  Quantity
Date       Buyer          
2013-01-31 Carl          1
           Mark          3
2013-10-31 Carl          6
           Joe           9
2013-12-31 Carl          3
           Joe           9
    
    
### key与level ???
In [180]: df = df.set_index('Date')

In [181]: df['Date'] = df.index + pd.offsets.MonthEnd(2)

In [182]: df.groupby([pd.Grouper(freq='6M', key='Date'), 'Buyer']).sum()
Out[182]: 
                  Quantity
Date       Buyer          
2013-02-28 Carl          1
           Mark          3
2014-02-28 Carl          9
           Joe          18

In [183]: df.groupby([pd.Grouper(freq='6M', level='Date'), 'Buyer']).sum()
Out[183]: 
                  Quantity
Date       Buyer          
2013-01-31 Carl          1
           Mark          3
2014-01-31 Carl          9
           Joe          18
```





# 选取group的行、列

```python
In [184]: df = pd.DataFrame([[1, 2], [1, 4], [5, 6]], columns=['A', 'B'])

In [185]: df
Out[185]: 
   A  B
0  1  2
1  1  4
2  5  6

In [186]: g = df.groupby('A')

In [187]: g.head(1)
Out[187]: 
   A  B
0  1  2
2  5  6

In [188]: g.tail(1)
Out[188]: 
   A  B
1  1  4
2  5  6

### nth行、列 
In [189]: df = pd.DataFrame([[1, np.nan], [1, 4], [5, 6]], columns=['A', 'B'])
In [190]: g = df.groupby('A')
    
In [191]: g.nth(0)  #每个分组的第0行
In [192]: g.nth(-1) #每个分组的最后一行
In [193]: g.nth(1)
    
    
    
### 选择非空项
# nth(0) is the same as g.first()
In [194]: g.nth(0, dropna='any')
Out[194]: 
     B
A     
1  4.0
5  6.0

In [195]: g.first()
Out[195]: 
     B
A     
1  4.0
5  6.0

# nth(-1) is the same as g.last()
In [196]: g.nth(-1, dropna='any')  # NaNs denote group exhausted when using dropna
Out[196]: 
     B
A     
1  4.0
5  6.0

In [197]: g.last()
Out[197]: 
     B
A     
1  4.0
5  6.0

In [198]: g.B.nth(0, dropna='all')
Out[198]: 
A
1    4.0
5    6.0
Name: B, dtype: float64
        
        
### as_index：去掉索引
In [199]: df = pd.DataFrame([[1, np.nan], [1, 4], [5, 6]], columns=['A', 'B'])

In [200]: g = df.groupby('A', as_index=False)

In [201]: g.nth(0)
Out[201]: 
   A    B
0  1  NaN
2  5  6.0

In [202]: g.nth(-1)
Out[202]: 
   A    B
1  1  4.0
2  5  6.0


### 应用：筛选每个月的第一天、第四天和最后一天
In [203]: business_dates = pd.date_range(start='4/1/2014', end='6/30/2014', freq='B')

In [204]: df = pd.DataFrame(1, index=business_dates, columns=['a', 'b'])

# get the first, 4th, and last date index for each month
In [205]: df.groupby([df.index.year, df.index.month]).nth([0, 3, -1])
Out[205]: 
        a  b
2014 4  1  1
     4  1  1
     4  1  1
     5  1  1
     5  1  1
     5  1  1
     6  1  1
     6  1  1
     6  1  1
```





# 遍历|group items

cumcount：查看每一行在其组中出现的顺序

```python
In [206]: dfg = pd.DataFrame(list('aaabba'), columns=['A'])

In [207]: dfg
Out[207]: 
   A
0  a
1  a
2  a
3  b
4  b
5  a

In [208]: dfg.groupby('A').cumcount()
Out[208]: 
0    0
1    1
2    2
3    0
4    1
5    3
dtype: int64

In [209]: dfg.groupby('A').cumcount(ascending=False)
Out[209]: 
0    3
1    2
2    1
3    1
4    0
5    0
dtype: int64
```



# 遍历|groups

ngroup：分配给group的数字是在遍历groupby对象时候的次序，而不是第一次出现的次序

```python
In [210]: dfg = pd.DataFrame(list('aaabba'), columns=['A'])

In [211]: dfg
Out[211]: 
   A
0  a
1  a
2  a
3  b
4  b
5  a

In [212]: dfg.groupby('A').ngroup()
Out[212]: 
0    0
1    0
2    0
3    1
4    1
5    0
dtype: int64

In [213]: dfg.groupby('A').ngroup(ascending=False)
Out[213]: 
0    1
1    1
2    1
3    0
4    0
5    1
dtype: int64
```



# 可视化

```python
In [214]: np.random.seed(1234)
In [215]: df = pd.DataFrame(np.random.randn(50, 2))
In [216]: df['g'] = np.random.choice(['A', 'B'], size=50)
In [217]: df.loc[df['g'] == 'B', 1] += 3
    
In [218]: df.groupby('g').boxplot()
Out[218]: 
A         AxesSubplot(0.1,0.15;0.363636x0.75)
B    AxesSubplot(0.536364,0.15;0.363636x0.75)
dtype: object
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201217173300.png" alt="image-20201217173259732" style="zoom:50%;" />



# Pipe

示例：

假设有一个带有商店，产品，收入和销售数量的列的DataFrame。

我们想对 每个商店和每个产品的价格（即收入/数量）进行分组计算。

我们可以通过多步骤操作来做到这一点，但是用管道表达方式可以使代码更具可读性

```python
### 原始数据
In [219]: n = 1000
In [220]: df = pd.DataFrame({'Store': np.random.choice(['Store_1', 'Store_2'], n),
   .....:                    'Product': np.random.choice(['Product_1',
   .....:                                                 'Product_2'], n),
   .....:                    'Revenue': (np.random.random(n) * 50 + 10).round(2),
   .....:                    'Quantity': np.random.randint(1, 10, size=n)})
   .....: 

In [221]: df.head(2)
Out[221]: 
     Store    Product  Revenue  Quantity
0  Store_2  Product_1    26.12         1
1  Store_2  Product_1    28.86         1


# 查找每个商店/产品的价格
In [222]: (df.groupby(['Store', 'Product'])
   .....:    .pipe(lambda grp: grp.Revenue.sum() / grp.Quantity.sum())
   .....:    .unstack().round(2))
   .....: 
Out[222]: 
Product  Product_1  Product_2
Store                        
Store_1       6.82       7.05
Store_2       6.30       6.64



In [223]: def mean(groupby):
   .....:     return groupby.mean()
   .....: 

In [224]: df.groupby(['Store', 'Product']).pipe(mean)
Out[224]: 
                     Revenue  Quantity
Store   Product                       
Store_1 Product_1  34.622727  5.075758
        Product_2  35.482815  5.029630
Store_2 Product_1  32.972837  5.237589
        Product_2  34.684360  5.224000
```





# #Examples

## 基于df.sum()

```python
In [225]: df = pd.DataFrame({'a': [1, 0, 0], 'b': [0, 1, 0],
   .....:                    'c': [1, 0, 0], 'd': [2, 3, 4]})
   .....: 

In [226]: df
Out[226]: 
   a  b  c  d
0  1  0  1  2
1  0  1  0  3
2  0  0  0  4

In [227]: df.groupby(df.sum(), axis=1).sum()
Out[227]: 
   1  9
0  2  2
1  1  3
2  0  4

# 备注：df.sum() => 只有1和0两个groups
a    1
b    1
c    1
d    9
dtype: int64
```



## 基于.ngroup()

```python
In [228]: dfg = pd.DataFrame({"A": [1, 1, 2, 3, 2], "B": list("aaaba")})

In [229]: dfg
Out[229]: 
   A  B
0  1  a
1  1  a
2  2  a
3  3  b
4  2  a

In [230]: dfg.groupby(["A", "B"]).ngroup()
Out[230]: 
0    0
1    0
2    1
3    2
4    1
dtype: int64

In [231]: dfg.groupby(["A", [0, 0, 0, 1, 1]]).ngroup()
Out[231]: 
0    0
1    0
2    1
3    3
4    2
dtype: int64
```



## 基于index重新分组

```python
In [232]: df = pd.DataFrame(np.random.randn(10, 2))

In [233]: df
Out[233]: 
          0         1
0 -0.793893  0.321153
1  0.342250  1.618906
2 -0.975807  1.918201
3 -0.810847 -1.405919
4 -1.977759  0.461659
5  0.730057 -1.316938
6 -0.751328  0.528290
7 -0.257759 -1.081009
8  0.505895 -1.701948
9 -1.006349  0.020208

In [234]: df.index // 5
Out[234]: Int64Index([0, 0, 0, 0, 0, 1, 1, 1, 1, 1], dtype='int64')

In [235]: df.groupby(df.index // 5).std()
Out[235]: 
          0         1
0  0.823647  1.312912
1  0.760109  0.942941
```



## 对列分组，使用不同函数

```python
In [236]: df = pd.DataFrame({'a': [0, 0, 0, 0, 1, 1, 1, 1, 2, 2, 2, 2],
   .....:                    'b': [0, 0, 1, 1, 0, 0, 1, 1, 0, 0, 1, 1],
   .....:                    'c': [1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0],
   .....:                    'd': [0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1]})
   .....: 

# 对b列求和
# 对c列求均值
In [237]: def compute_metrics(x):
   .....:     result = {'b_sum': x['b'].sum(), 'c_mean': x['c'].mean()}
   .....:     return pd.Series(result, name='metrics')
   .....: 

In [238]: result = df.groupby('a').apply(compute_metrics)

In [239]: result
Out[239]: 
metrics  b_sum  c_mean
a                     
0          2.0     0.5
1          2.0     0.5
2          2.0     0.5

In [240]: result.stack()
Out[240]: 
a  metrics
0  b_sum      2.0
   c_mean     0.5
1  b_sum      2.0
   c_mean     0.5
2  b_sum      2.0
   c_mean     0.5
dtype: float64
```

