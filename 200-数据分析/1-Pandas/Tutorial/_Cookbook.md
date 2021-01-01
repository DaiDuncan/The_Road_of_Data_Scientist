[Link: Cookbook](https://pandas.pydata.org/pandas-docs/stable/user_guide/cookbook.html)

# 赋值本质：if-then结构

```python
### 数据集dataset
In [1]: df = pd.DataFrame({'AAA': [4, 5, 6, 7],
   ...:                    'BBB': [10, 20, 30, 40],
   ...:                    'CCC': [100, 50, -30, -50]})
   ...: 

In [2]: df
Out[2]: 
   AAA  BBB  CCC
0    4   10  100
1    5   20   50
2    6   30  -30
3    7   40  -50


df.loc[df.AAA >= 5, 'BBB'] = -1
df.loc[df.AAA >= 5, ['BBB', 'CCC']] = 555


# mask:True/False表
In [9]: df_mask = pd.DataFrame({'AAA': [True] * 4,
   ...:                         'BBB': [False] * 4,
   ...:                         'CCC': [True, False] * 2})
   ...: 

In [10]: df.where(df_mask, -1000)
    
    
In [13]: df['logic'] = np.where(df['AAA'] > 5, 'high', 'low')
```





# 选择部分内容Splitting

```python
In [17]: df[df.AAA <= 5]
In [18]: df[df.AAA > 5]
    
    
In [21]: df.loc[(df['BBB'] < 25) & (df['CCC'] >= -40), 'AAA']
    
In [28]: df.loc[(df.CCC - aValue).abs().argsort()]
    
### functools
In [30]: df
Out[30]: 
   AAA  BBB  CCC
0    4   10  100
1    5   20   50
2    6   30  -30
3    7   40  -50

In [31]: Crit1 = df.AAA <= 5.5
In [32]: Crit2 = df.BBB == 10.0
In [33]: Crit3 = df.CCC > -40.0

# 
In [35]: import functools

In [36]: CritList = [Crit1, Crit2, Crit3]

In [37]: AllCrit = functools.reduce(lambda x, y: x & y, CritList)

In [38]: df[AllCrit]
Out[38]: 
   AAA  BBB  CCC
0    4   10  100
```





# 选择：基于索引

切片方式：

1. iloc 基于位置(类似python切片：左闭右开)
2. loc 基于标签(闭区间)
3. 一般情况：df选择部分内容

```python
In [41]: df[(df.AAA <= 6) & (df.index.isin([0, 2, 4]))]
    
### 切片方式
In [43]: df.loc['bar':'kar']  # Label

    In [44]: df[0:3] 
In [45]: df['bar':'kar']
    
# 特殊情况：index时自然数
In [46]: data = {'AAA': [4, 5, 6, 7],
   ....:         'BBB': [10, 20, 30, 40],
   ....:         'CCC': [100, 50, -30, -50]}
   ....: 

In [47]: df2 = pd.DataFrame(data=data, index=[1, 2, 3, 4])  # Note index starts at 1.

In [48]: df2.iloc[1:3]  # Position-oriented
Out[48]: 
   AAA  BBB  CCC
2    5   20   50
3    6   30  -30

In [49]: df2.loc[1:3]  # Label-oriented
Out[49]: 
   AAA  BBB  CCC
1    4   10  100
2    5   20   50
3    6   30  -30

### 补充：互补情况
In [52]: df[~((df.AAA <= 6) & (df.index.isin([0, 2, 4])))]
```





# 新的列

```python
In [54]: df
Out[54]: 
   AAA  BBB  CCC
0    1    1    2
1    2    1    1
2    1    2    3
3    3    2    1

In [55]: source_cols = df.columns   # Or some subset would work too

In [56]: new_cols = [str(x) + "_cat" for x in source_cols]

In [57]: categories = {1: 'Alpha', 2: 'Beta', 3: 'Charlie'}

# 添加新的列：基于值的映射 => 应用：分类数据标签化
In [58]: df[new_cols] = df[source_cols].applymap(categories.get)

In [59]: df
Out[59]: 
   AAA  BBB  CCC  AAA_cat BBB_cat  CCC_cat
0    1    1    2    Alpha   Alpha     Beta
1    2    1    1     Beta   Alpha    Alpha
2    1    2    3    Alpha    Beta  Charlie
3    3    2    1  Charlie    Beta    Alpha


### 基于groupby的方法
In [61]: df
Out[61]: 
   AAA  BBB
0    1    2
1    1    1
2    1    3
3    2    4
4    2    5
5    2    1
6    3    2
7    3    3


# idxmin：获取最小值的index
In [62]: df.loc[df.groupby("AAA")["BBB"].idxmin()]
Out[62]: 
   AAA  BBB
1    1    1
5    2    1
6    3    2


# first：排序后获取第一个值
In [63]: df.sort_values(by="BBB").groupby("AAA", as_index=False).first()
Out[63]: 
   AAA  BBB
0    1    1
1    2    1
2    3    2
```





# MultiIndex

```python
In [65]: df
Out[65]: 
   row  One_X  One_Y  Two_X  Two_Y
0    0    1.1    1.2   1.11   1.22
1    1    1.1    1.2   1.11   1.22
2    2    1.1    1.2   1.11   1.22

# As Labelled Index
In [66]: df = df.set_index('row')

In [67]: df
Out[67]: 
     One_X  One_Y  Two_X  Two_Y
row                            
0      1.1    1.2   1.11   1.22
1      1.1    1.2   1.11   1.22
2      1.1    1.2   1.11   1.22

# With Hierarchical Columns
In [68]: df.columns = pd.MultiIndex.from_tuples([tuple(c.split('_'))
   ....:                                         for c in df.columns])
   ....: 

In [69]: df
Out[69]: 
     One        Two      
       X    Y     X     Y
row                      
0    1.1  1.2  1.11  1.22
1    1.1  1.2  1.11  1.22
2    1.1  1.2  1.11  1.22

# Now stack & Reset
In [70]: df = df.stack(0).reset_index(1)

In [71]: df
Out[71]: 
    level_1     X     Y
row                    
0       One  1.10  1.20
0       Two  1.11  1.22
1       One  1.10  1.20
1       Two  1.11  1.22
2       One  1.10  1.20
2       Two  1.11  1.22

# And fix the labels (Notice the label 'level_1' got added automatically)
In [72]: df.columns = ['Sample', 'All_X', 'All_Y']

In [73]: df
Out[73]: 
    Sample  All_X  All_Y
row                     
0      One   1.10   1.20
0      Two   1.11   1.22
1      One   1.10   1.20
1      Two   1.11   1.22
2      One   1.10   1.20
2      Two   1.11   1.22
```



```python
### 应用：MultiIndex对应的四则运算
In [74]: cols = pd.MultiIndex.from_tuples([(x, y) for x in ['A', 'B', 'C']
   ....:                                   for y in ['O', 'I']])
   ....: 

In [75]: df = pd.DataFrame(np.random.randn(2, 6), index=['n', 'm'], columns=cols)

In [76]: df
Out[76]: 
          A                   B                   C          
          O         I         O         I         O         I
n  0.469112 -0.282863 -1.509059 -1.135632  1.212112 -0.173215
m  0.119209 -1.044236 -0.861849 -2.104569 -0.494929  1.071804

# Column Index C的两个标签O和I分别作为对应的除数
In [77]: df = df.div(df['C'], level=1)

In [78]: df
Out[78]: 
          A                   B              C     
          O         I         O         I    O    I
n  0.387021  1.633022 -1.244983  6.556214  1.0  1.0
m -0.240860 -0.974279  1.741358 -1.963577  1.0  1.0


### 基于xs进行MultiIndex的切片
In [79]: coords = [('AA', 'one'), ('AA', 'six'), ('BB', 'one'), ('BB', 'two'),
   ....:           ('BB', 'six')]
   ....: 

In [80]: index = pd.MultiIndex.from_tuples(coords)

In [81]: df = pd.DataFrame([11, 22, 33, 44, 55], index, ['MyData'])

In [82]: df
Out[82]: 
        MyData
AA one      11
   six      22
BB one      33
   two      44
   six      55

# Note : level and axis are optional, and default to zero
# To take the cross section of the 1st level and 1st axis the index
In [83]: df.xs('BB', level=0, axis=0)
Out[83]: 
     MyData
one      33
two      44
six      55

# 2nd level of the 1st axis
In [84]: df.xs('six', level=1, axis=0)
Out[84]: 
    MyData
AA      22
BB      55
```



# 排序

`df.sort_values(by=, ascending=False)`



# 缺失值

```python
In [100]: df = pd.DataFrame(np.random.randn(6, 1),
   .....:                   index=pd.date_range('2013-08-01', periods=6, freq='B'),
   .....:                   columns=list('A'))
   .....: 

In [101]: df.loc[df.index[3], 'A'] = np.nan

In [102]: df
Out[102]: 
                   A
2013-08-01  0.721555
2013-08-02 -0.706771
2013-08-05 -1.039575
2013-08-06       NaN
2013-08-07 -0.424972
2013-08-08  0.567020

### 填补缺失值
In [103]: df.reindex(df.index[::-1]).ffill()
Out[103]: 
                   A
2013-08-08  0.567020
2013-08-07 -0.424972
2013-08-06 -0.424972
2013-08-05 -1.039575
2013-08-02 -0.706771
2013-08-01  0.721555
```



# 替代Replace

```python
In [116]: df = pd.DataFrame({'A': [1, 1, 2, 2], 'B': [1, -1, 1, 2]})

In [117]: gb = df.groupby('A')

In [118]: def replace(g):
   .....:     mask = g < 0
   .....:     return g.where(mask, g[~mask].mean())
   .....: 

In [119]: gb.transform(replace)
Out[119]: 
     B
0  1.0
1 -1.0
2  1.5
3  1.5
```





# ⭐Grouping 含丰富的应用示例

[Basic grouping with apply](https://stackoverflow.com/questions/15322632/python-pandas-df-groupy-agg-column-reference-in-agg)

Unlike agg, apply’s callable is passed a sub-DataFrame which gives you access to all the columns

```python
In [104]: df = pd.DataFrame({'animal': 'cat dog cat fish dog cat cat'.split(),
   .....:                    'size': list('SSMMMLL'),
   .....:                    'weight': [8, 10, 11, 1, 20, 12, 12],
   .....:                    'adult': [False] * 5 + [True] * 2})
   .....: 

In [105]: df
Out[105]: 
  animal size  weight  adult
0    cat    S       8  False
1    dog    S      10  False
2    cat    M      11  False
3   fish    M       1  False
4    dog    M      20  False
5    cat    L      12   True
6    cat    L      12   True

# List the size of the animals with the highest weight.
In [106]: df.groupby('animal').apply(lambda subf: subf['size'][subf['weight'].idxmax()])
Out[106]: 
animal
cat     L
dog     M
fish    M
dtype: object
```

[Using get_group](https://stackoverflow.com/questions/14734533/how-to-access-pandas-groupby-dataframe-by-key)

```
In [107]: gb = df.groupby(['animal'])

In [108]: gb.get_group('cat')
Out[108]: 
  animal size  weight  adult
0    cat    S       8  False
2    cat    M      11  False
5    cat    L      12   True
6    cat    L      12   True
```

[Apply to different items in a group](https://stackoverflow.com/questions/15262134/apply-different-functions-to-different-items-in-group-object-python-pandas)

```
In [109]: def GrowUp(x):
   .....:     avg_weight = sum(x[x['size'] == 'S'].weight * 1.5)
   .....:     avg_weight += sum(x[x['size'] == 'M'].weight * 1.25)
   .....:     avg_weight += sum(x[x['size'] == 'L'].weight)
   .....:     avg_weight /= len(x)
   .....:     return pd.Series(['L', avg_weight, True],
   .....:                      index=['size', 'weight', 'adult'])
   .....: 

In [110]: expected_df = gb.apply(GrowUp)

In [111]: expected_df
Out[111]: 
       size   weight  adult
animal                     
cat       L  12.4375   True
dog       L  20.0000   True
fish      L   1.2500   True
```

[Expanding apply](https://stackoverflow.com/questions/14542145/reductions-down-a-column-in-pandas)

```
In [112]: S = pd.Series([i / 100.0 for i in range(1, 11)])

In [113]: def cum_ret(x, y):
   .....:     return x * (1 + y)
   .....: 

In [114]: def red(x):
   .....:     return functools.reduce(cum_ret, x, 1.0)
   .....: 

In [115]: S.expanding().apply(red, raw=True)
Out[115]: 
0    1.010000
1    1.030200
2    1.061106
3    1.103550
4    1.158728
5    1.228251
6    1.314229
7    1.419367
8    1.547110
9    1.701821
dtype: float64
```

[Replacing some values with mean of the rest of a group](https://stackoverflow.com/questions/14760757/replacing-values-with-groupby-means)

```
In [116]: df = pd.DataFrame({'A': [1, 1, 2, 2], 'B': [1, -1, 1, 2]})

In [117]: gb = df.groupby('A')

In [118]: def replace(g):
   .....:     mask = g < 0
   .....:     return g.where(mask, g[~mask].mean())
   .....: 

In [119]: gb.transform(replace)
Out[119]: 
     B
0  1.0
1 -1.0
2  1.5
3  1.5
```

[Sort groups by aggregated data](https://stackoverflow.com/questions/14941366/pandas-sort-by-group-aggregate-and-column)

```
In [120]: df = pd.DataFrame({'code': ['foo', 'bar', 'baz'] * 2,
   .....:                    'data': [0.16, -0.21, 0.33, 0.45, -0.59, 0.62],
   .....:                    'flag': [False, True] * 3})
   .....: 

In [121]: code_groups = df.groupby('code')

In [122]: agg_n_sort_order = code_groups[['data']].transform(sum).sort_values(by='data')

In [123]: sorted_df = df.loc[agg_n_sort_order.index]

In [124]: sorted_df
Out[124]: 
  code  data   flag
1  bar -0.21   True
4  bar -0.59  False
0  foo  0.16  False
3  foo  0.45   True
2  baz  0.33  False
5  baz  0.62   True
```

[Create multiple aggregated columns](https://stackoverflow.com/questions/14897100/create-multiple-columns-in-pandas-aggregation-function)

```
In [125]: rng = pd.date_range(start="2014-10-07", periods=10, freq='2min')

In [126]: ts = pd.Series(data=list(range(10)), index=rng)

In [127]: def MyCust(x):
   .....:     if len(x) > 2:
   .....:         return x[1] * 1.234
   .....:     return pd.NaT
   .....: 

In [128]: mhc = {'Mean': np.mean, 'Max': np.max, 'Custom': MyCust}

In [129]: ts.resample("5min").apply(mhc)
Out[129]: 
Mean    2014-10-07 00:00:00        1
        2014-10-07 00:05:00      3.5
        2014-10-07 00:10:00        6
        2014-10-07 00:15:00      8.5
Max     2014-10-07 00:00:00        2
        2014-10-07 00:05:00        4
        2014-10-07 00:10:00        7
        2014-10-07 00:15:00        9
Custom  2014-10-07 00:00:00    1.234
        2014-10-07 00:05:00      NaT
        2014-10-07 00:10:00    7.404
        2014-10-07 00:15:00      NaT
dtype: object

In [130]: ts
Out[130]: 
2014-10-07 00:00:00    0
2014-10-07 00:02:00    1
2014-10-07 00:04:00    2
2014-10-07 00:06:00    3
2014-10-07 00:08:00    4
2014-10-07 00:10:00    5
2014-10-07 00:12:00    6
2014-10-07 00:14:00    7
2014-10-07 00:16:00    8
2014-10-07 00:18:00    9
Freq: 2T, dtype: int64
```

[Create a value counts column and reassign back to the DataFrame](https://stackoverflow.com/questions/17709270/i-want-to-create-a-column-of-value-counts-in-my-pandas-dataframe)

```
In [131]: df = pd.DataFrame({'Color': 'Red Red Red Blue'.split(),
   .....:                    'Value': [100, 150, 50, 50]})
   .....: 

In [132]: df
Out[132]: 
  Color  Value
0   Red    100
1   Red    150
2   Red     50
3  Blue     50

In [133]: df['Counts'] = df.groupby(['Color']).transform(len)

In [134]: df
Out[134]: 
  Color  Value  Counts
0   Red    100       3
1   Red    150       3
2   Red     50       3
3  Blue     50       1
```

[Shift groups of the values in a column based on the index](https://stackoverflow.com/q/23198053/190597)

```
In [135]: df = pd.DataFrame({'line_race': [10, 10, 8, 10, 10, 8],
   .....:                    'beyer': [99, 102, 103, 103, 88, 100]},
   .....:                   index=['Last Gunfighter', 'Last Gunfighter',
   .....:                          'Last Gunfighter', 'Paynter', 'Paynter',
   .....:                          'Paynter'])
   .....: 

In [136]: df
Out[136]: 
                 line_race  beyer
Last Gunfighter         10     99
Last Gunfighter         10    102
Last Gunfighter          8    103
Paynter                 10    103
Paynter                 10     88
Paynter                  8    100

In [137]: df['beyer_shifted'] = df.groupby(level=0)['beyer'].shift(1)

In [138]: df
Out[138]: 
                 line_race  beyer  beyer_shifted
Last Gunfighter         10     99            NaN
Last Gunfighter         10    102           99.0
Last Gunfighter          8    103          102.0
Paynter                 10    103            NaN
Paynter                 10     88          103.0
Paynter                  8    100           88.0
```

[Select row with maximum value from each group](https://stackoverflow.com/q/26701849/190597)

```
In [139]: df = pd.DataFrame({'host': ['other', 'other', 'that', 'this', 'this'],
   .....:                    'service': ['mail', 'web', 'mail', 'mail', 'web'],
   .....:                    'no': [1, 2, 1, 2, 1]}).set_index(['host', 'service'])
   .....: 

In [140]: mask = df.groupby(level=0).agg('idxmax')

In [141]: df_count = df.loc[mask['no']].reset_index()

In [142]: df_count
Out[142]: 
    host service  no
0  other     web   2
1   that    mail   1
2   this    mail   2
```

[Grouping like Python’s itertools.groupby](https://stackoverflow.com/q/29142487/846892)

```
In [143]: df = pd.DataFrame([0, 1, 0, 1, 1, 1, 0, 1, 1], columns=['A'])

In [144]: df['A'].groupby((df['A'] != df['A'].shift()).cumsum()).groups
Out[144]: {1: [0], 2: [1], 3: [2], 4: [3, 4, 5], 5: [6], 6: [7, 8]}

In [145]: df['A'].groupby((df['A'] != df['A'].shift()).cumsum()).cumsum()
Out[145]: 
0    0
1    1
2    0
3    1
4    2
5    3
6    0
7    1
8    2
Name: A, dtype: int64
```



# ⭐Pivot

[Partial sums and subtotals](https://stackoverflow.com/questions/15570099/pandas-pivot-tables-row-subtotals/15574875#15574875)

```
In [151]: df = pd.DataFrame(data={'Province': ['ON', 'QC', 'BC', 'AL', 'AL', 'MN', 'ON'],
   .....:                         'City': ['Toronto', 'Montreal', 'Vancouver',
   .....:                                  'Calgary', 'Edmonton', 'Winnipeg',
   .....:                                  'Windsor'],
   .....:                         'Sales': [13, 6, 16, 8, 4, 3, 1]})
   .....: 

In [152]: table = pd.pivot_table(df, values=['Sales'], index=['Province'],
   .....:                        columns=['City'], aggfunc=np.sum, margins=True)
   .....: 

In [153]: table.stack('City')
Out[153]: 
                    Sales
Province City            
AL       All         12.0
         Calgary      8.0
         Edmonton     4.0
BC       All         16.0
         Vancouver   16.0
...                   ...
All      Montreal     6.0
         Toronto     13.0
         Vancouver   16.0
         Windsor      1.0
         Winnipeg     3.0

[20 rows x 1 columns]
```

[Frequency table like plyr in R](https://stackoverflow.com/questions/15589354/frequency-tables-in-pandas-like-plyr-in-r)

```
In [154]: grades = [48, 99, 75, 80, 42, 80, 72, 68, 36, 78]

In [155]: df = pd.DataFrame({'ID': ["x%d" % r for r in range(10)],
   .....:                    'Gender': ['F', 'M', 'F', 'M', 'F',
   .....:                               'M', 'F', 'M', 'M', 'M'],
   .....:                    'ExamYear': ['2007', '2007', '2007', '2008', '2008',
   .....:                                 '2008', '2008', '2009', '2009', '2009'],
   .....:                    'Class': ['algebra', 'stats', 'bio', 'algebra',
   .....:                              'algebra', 'stats', 'stats', 'algebra',
   .....:                              'bio', 'bio'],
   .....:                    'Participated': ['yes', 'yes', 'yes', 'yes', 'no',
   .....:                                     'yes', 'yes', 'yes', 'yes', 'yes'],
   .....:                    'Passed': ['yes' if x > 50 else 'no' for x in grades],
   .....:                    'Employed': [True, True, True, False,
   .....:                                 False, False, False, True, True, False],
   .....:                    'Grade': grades})
   .....: 

In [156]: df.groupby('ExamYear').agg({'Participated': lambda x: x.value_counts()['yes'],
   .....:                             'Passed': lambda x: sum(x == 'yes'),
   .....:                             'Employed': lambda x: sum(x),
   .....:                             'Grade': lambda x: sum(x) / len(x)})
   .....: 
Out[156]: 
          Participated  Passed  Employed      Grade
ExamYear                                           
2007                 3       2         3  74.000000
2008                 3       3         0  68.500000
2009                 3       2         2  60.666667
```

[Plot pandas DataFrame with year over year data](https://stackoverflow.com/questions/30379789/plot-pandas-data-frame-with-year-over-year-data)

To create year and month cross tabulation:

```
In [157]: df = pd.DataFrame({'value': np.random.randn(36)},
   .....:                   index=pd.date_range('2011-01-01', freq='M', periods=36))
   .....: 

In [158]: pd.pivot_table(df, index=df.index.month, columns=df.index.year,
   .....:                values='value', aggfunc='sum')
   .....: 
Out[158]: 
        2011      2012      2013
1  -1.039268 -0.968914  2.565646
2  -0.370647 -1.294524  1.431256
3  -1.157892  0.413738  1.340309
4  -1.344312  0.276662 -1.170299
5   0.844885 -0.472035 -0.226169
6   1.075770 -0.013960  0.410835
7  -0.109050 -0.362543  0.813850
8   1.643563 -0.006154  0.132003
9  -1.469388 -0.923061 -0.827317
10  0.357021  0.895717 -0.076467
11 -0.674600  0.805244 -1.187678
12 -1.776904 -1.206412  1.130127
```



# Timeseries

# Timedeltas

# Resampling



# ⭐Merge

[Append two dataframes with overlapping index (emulate R rbind)](https://stackoverflow.com/questions/14988480/pandas-version-of-rbind)

```
In [177]: rng = pd.date_range('2000-01-01', periods=6)

In [178]: df1 = pd.DataFrame(np.random.randn(6, 3), index=rng, columns=['A', 'B', 'C'])

In [179]: df2 = df1.copy()
```

Depending on df construction, `ignore_index` may be needed

```
In [180]: df = df1.append(df2, ignore_index=True)

In [181]: df
Out[181]: 
           A         B         C
0  -0.870117 -0.479265 -0.790855
1   0.144817  1.726395 -0.464535
2  -0.821906  1.597605  0.187307
3  -0.128342 -1.511638 -0.289858
4   0.399194 -1.430030 -0.639760
5   1.115116 -2.012600  1.810662
6  -0.870117 -0.479265 -0.790855
7   0.144817  1.726395 -0.464535
8  -0.821906  1.597605  0.187307
9  -0.128342 -1.511638 -0.289858
10  0.399194 -1.430030 -0.639760
11  1.115116 -2.012600  1.810662
```

[Self Join of a DataFrame](https://github.com/pandas-dev/pandas/issues/2996)

```
In [182]: df = pd.DataFrame(data={'Area': ['A'] * 5 + ['C'] * 2,
   .....:                         'Bins': [110] * 2 + [160] * 3 + [40] * 2,
   .....:                         'Test_0': [0, 1, 0, 1, 2, 0, 1],
   .....:                         'Data': np.random.randn(7)})
   .....: 

In [183]: df
Out[183]: 
  Area  Bins  Test_0      Data
0    A   110       0 -0.433937
1    A   110       1 -0.160552
2    A   160       0  0.744434
3    A   160       1  1.754213
4    A   160       2  0.000850
5    C    40       0  0.342243
6    C    40       1  1.070599

In [184]: df['Test_1'] = df['Test_0'] - 1

In [185]: pd.merge(df, df, left_on=['Bins', 'Area', 'Test_0'],
   .....:          right_on=['Bins', 'Area', 'Test_1'],
   .....:          suffixes=('_L', '_R'))
   .....: 
Out[185]: 
  Area  Bins  Test_0_L    Data_L  Test_1_L  Test_0_R    Data_R  Test_1_R
0    A   110         0 -0.433937        -1         1 -0.160552         0
1    A   160         0  0.744434        -1         1  1.754213         0
2    A   160         1  1.754213         0         2  0.000850         1
3    C    40         0  0.342243        -1         1  1.070599         0
```



# 可视化plotting



# ⭐数据I/O

[Performance comparison of SQL vs HDF5](https://stackoverflow.com/questions/16628329/hdf5-and-sqlite-concurrency-compression-i-o-performance)

### CSV

The [CSV](https://pandas.pydata.org/pandas-docs/stable/user_guide/io.html#io-read-csv-table) docs

[read_csv in action](https://wesmckinney.com/blog/update-on-upcoming-pandas-v0-10-new-file-parser-other-performance-wins/)

[appending to a csv](https://stackoverflow.com/questions/17134942/pandas-dataframe-output-end-of-csv)

[Reading a csv chunk-by-chunk](https://stackoverflow.com/questions/11622652/large-persistent-dataframe-in-pandas/12193309#12193309)

[Reading only certain rows of a csv chunk-by-chunk](https://stackoverflow.com/questions/19674212/pandas-data-frame-select-rows-and-clear-memory)

[Reading the first few lines of a frame](https://stackoverflow.com/questions/15008970/way-to-read-first-few-lines-for-pandas-dataframe)

Reading a file that is compressed but not by `gzip/bz2` (the native compressed formats which `read_csv` understands). This example shows a `WinZipped` file, but is a general application of opening the file within a context manager and using that handle to read. [See here](https://stackoverflow.com/questions/17789907/pandas-convert-winzipped-csv-file-to-data-frame)

[Inferring dtypes from a file](https://stackoverflow.com/questions/15555005/get-inferred-dataframe-types-iteratively-using-chunksize)

[Dealing with bad lines](https://github.com/pandas-dev/pandas/issues/2886)

[Dealing with bad lines II](http://nipunbatra.github.io/2013/06/reading-unclean-data-csv-using-pandas/)

[Reading CSV with Unix timestamps and converting to local timezone](http://nipunbatra.github.io/2013/06/pandas-reading-csv-with-unix-timestamps-and-converting-to-local-timezone/)

[Write a multi-row index CSV without writing duplicates](https://stackoverflow.com/questions/17349574/pandas-write-multiindex-rows-with-to-csv)



#### Reading multiple files to create a single DataFrame

The best way to combine multiple files into a single DataFrame is to read the individual frames one by one, put all of the individual frames into a list, and then combine the frames in the list using `pd.concat()`:

```python
In [189]: for i in range(3):
   .....:     data = pd.DataFrame(np.random.randn(10, 4))
   .....:     data.to_csv('file_{}.csv'.format(i))
   .....: 

In [190]: files = ['file_0.csv', 'file_1.csv', 'file_2.csv']

In [191]: result = pd.concat([pd.read_csv(f) for f in files], ignore_index=True)
```

You can use the same approach to read all files matching a pattern. Here is an example using `glob`:

```python
In [192]: import glob

In [193]: import os

In [194]: files = glob.glob('file_*.csv')

In [195]: result = pd.concat([pd.read_csv(f) for f in files], ignore_index=True)
```

Finally, this strategy will work with the other `pd.read_*(...)` functions described in the [io docs](https://pandas.pydata.org/pandas-docs/stable/user_guide/io.html#io).

#### Parsing date components in multi-columns

Parsing date components in multi-columns is faster with a format

```python
In [196]: i = pd.date_range('20000101', periods=10000)

In [197]: df = pd.DataFrame({'year': i.year, 'month': i.month, 'day': i.day})

In [198]: df.head()
Out[198]: 
   year  month  day
0  2000      1    1
1  2000      1    2
2  2000      1    3
3  2000      1    4
4  2000      1    5

In [199]: %timeit pd.to_datetime(df.year * 10000 + df.month * 100 + df.day, format='%Y%m%d')
   .....: ds = df.apply(lambda x: "%04d%02d%02d" % (x['year'],
   .....:                                           x['month'], x['day']), axis=1)
   .....: ds.head()
   .....: %timeit pd.to_datetime(ds)
   .....: 
5.25 ms +- 68.8 us per loop (mean +- std. dev. of 7 runs, 100 loops each)
1.35 ms +- 3 us per loop (mean +- std. dev. of 7 runs, 1000 loops each)
```

#### Skip row between header and data

```python
In [200]: data = """;;;;
   .....:  ;;;;
   .....:  ;;;;
   .....:  ;;;;
   .....:  ;;;;
   .....:  ;;;;
   .....: ;;;;
   .....:  ;;;;
   .....:  ;;;;
   .....: ;;;;
   .....: date;Param1;Param2;Param4;Param5
   .....:     ;m²;°C;m²;m
   .....: ;;;;
   .....: 01.01.1990 00:00;1;1;2;3
   .....: 01.01.1990 01:00;5;3;4;5
   .....: 01.01.1990 02:00;9;5;6;7
   .....: 01.01.1990 03:00;13;7;8;9
   .....: 01.01.1990 04:00;17;9;10;11
   .....: 01.01.1990 05:00;21;11;12;13
   .....: """
   .....: 
```

##### Option 1: pass rows explicitly to skip rows

```python
In [201]: from io import StringIO

In [202]: pd.read_csv(StringIO(data), sep=';', skiprows=[11, 12],
   .....:             index_col=0, parse_dates=True, header=10)
   .....: 
Out[202]: 
                     Param1  Param2  Param4  Param5
date                                               
1990-01-01 00:00:00       1       1       2       3
1990-01-01 01:00:00       5       3       4       5
1990-01-01 02:00:00       9       5       6       7
1990-01-01 03:00:00      13       7       8       9
1990-01-01 04:00:00      17       9      10      11
1990-01-01 05:00:00      21      11      12      13
```

##### Option 2: read column names and then data

```
In [203]: pd.read_csv(StringIO(data), sep=';', header=10, nrows=10).columns
Out[203]: Index(['date', 'Param1', 'Param2', 'Param4', 'Param5'], dtype='object')

In [204]: columns = pd.read_csv(StringIO(data), sep=';', header=10, nrows=10).columns

In [205]: pd.read_csv(StringIO(data), sep=';', index_col=0,
   .....:             header=12, parse_dates=True, names=columns)
   .....: 
Out[205]: 
                     Param1  Param2  Param4  Param5
date                                               
1990-01-01 00:00:00       1       1       2       3
1990-01-01 01:00:00       5       3       4       5
1990-01-01 02:00:00       9       5       6       7
1990-01-01 03:00:00      13       7       8       9
1990-01-01 04:00:00      17       9      10      11
1990-01-01 05:00:00      21      11      12      13
```



### SQL

The [SQL](https://pandas.pydata.org/pandas-docs/stable/user_guide/io.html#io-sql) docs

[Reading from databases with SQL](https://stackoverflow.com/questions/10065051/python-pandas-and-databases-like-mysql)



### Excel

The [Excel](https://pandas.pydata.org/pandas-docs/stable/user_guide/io.html#io-excel) docs

[Reading from a filelike handle](https://stackoverflow.com/questions/15588713/sheets-of-excel-workbook-from-a-url-into-a-pandas-dataframe)

[Modifying formatting in XlsxWriter output](https://pbpython.com/improve-pandas-excel-output.html)



### HTML

[Reading HTML tables from a server that cannot handle the default request header](https://stackoverflow.com/a/18939272/564538)



### HDFStore

本质：PyTables => dict-like object 

- reads and writes pandas using the high performance HDF5 format
- using the excellent [PyTables](https://www.pytables.org/) library

层级数据格式（Hierarchical Data Format：HDF）是设计用来存储和组织大量数据的一组文件格式（HDF4，HDF5）。 它最初开发于美国国家超级计算应用中心，现在由非营利社团HDF Group支持，其任务是确保HDF5技术的持续开发和存储在HDF中数据的持续可访问性。

The [HDFStores](https://pandas.pydata.org/pandas-docs/stable/user_guide/io.html#io-hdf5) docs

[Simple queries with a Timestamp Index](https://stackoverflow.com/questions/13926089/selecting-columns-from-pandas-hdfstore-table)

[Managing heterogeneous data using a linked multiple table hierarchy](https://github.com/pandas-dev/pandas/issues/3032)

[Merging on-disk tables with millions of rows](https://stackoverflow.com/questions/14614512/merging-two-tables-with-millions-of-rows-in-python/14617925#14617925)

[Avoiding inconsistencies when writing to a store from multiple processes/threads](https://stackoverflow.com/a/29014295/2858145)

De-duplicating a large store by chunks, essentially a recursive reduction operation. Shows a function for taking in data from csv file and creating a store by chunks, with date parsing as well. [See here](https://stackoverflow.com/questions/16110252/need-to-compare-very-large-files-around-1-5gb-in-python/16110391#16110391)

[Creating a store chunk-by-chunk from a csv file](https://stackoverflow.com/questions/20428355/appending-column-to-frame-of-hdf-file-in-pandas/20428786#20428786)

[Appending to a store, while creating a unique index](https://stackoverflow.com/questions/16997048/how-does-one-append-large-amounts-of-data-to-a-pandas-hdfstore-and-get-a-natural/16999397#16999397)

[Large Data work flows](https://stackoverflow.com/questions/14262433/large-data-work-flows-using-pandas)

[Reading in a sequence of files, then providing a global unique index to a store while appending](https://stackoverflow.com/questions/16997048/how-does-one-append-large-amounts-of-data-to-a-pandas-hdfstore-and-get-a-natural)

[Groupby on a HDFStore with low group density](https://stackoverflow.com/questions/15798209/pandas-group-by-query-on-large-data-in-hdfstore)

[Groupby on a HDFStore with high group density](https://stackoverflow.com/questions/25459982/trouble-with-grouby-on-millions-of-keys-on-a-chunked-file-in-python-pandas/25471765#25471765)

[Hierarchical queries on a HDFStore](https://stackoverflow.com/questions/22777284/improve-query-performance-from-a-large-hdfstore-table-with-pandas/22820780#22820780)

[Counting with a HDFStore](https://stackoverflow.com/questions/20497897/converting-dict-of-dicts-into-pandas-dataframe-memory-issues)

[Troubleshoot HDFStore exceptions](https://stackoverflow.com/questions/15488809/how-to-trouble-shoot-hdfstore-exception-cannot-find-the-correct-atom-type)

[Setting min_itemsize with strings](https://stackoverflow.com/questions/15988871/hdfstore-appendstring-dataframe-fails-when-string-column-contents-are-longer)

[Using ptrepack to create a completely-sorted-index on a store](https://stackoverflow.com/questions/17893370/ptrepack-sortby-needs-full-index)

Storing Attributes to a group node

```
In [206]: df = pd.DataFrame(np.random.randn(8, 3))

In [207]: store = pd.HDFStore('test.h5')

In [208]: store.put('df', df)

# you can store an arbitrary Python object via pickle
In [209]: store.get_storer('df').attrs.my_attribute = {'A': 10}

In [210]: store.get_storer('df').attrs.my_attribute
Out[210]: {'A': 10}
```

You can create or load a HDFStore in-memory by passing the `driver` parameter to PyTables. Changes are only written to disk when the HDFStore is closed.

```
In [211]: store = pd.HDFStore('test.h5', 'w', diver='H5FD_CORE')

In [212]: df = pd.DataFrame(np.random.randn(8, 3))

In [213]: store['test'] = df

# only after closing the store, data is written to disk:
In [214]: store.close()
```



### Binary files

pandas readily accepts NumPy record arrays, if you need to read in a binary file consisting of an array of C structs. For example, given this C program in a file called `main.c` compiled with `gcc main.c -std=gnu99` on a 64-bit machine,

```
#include <stdio.h>
#include <stdint.h>

typedef struct _Data
{
    int32_t count;
    double avg;
    float scale;
} Data;

int main(int argc, const char *argv[])
{
    size_t n = 10;
    Data d[n];

    for (int i = 0; i < n; ++i)
    {
        d[i].count = i;
        d[i].avg = i + 1.0;
        d[i].scale = (float) i + 2.0f;
    }

    FILE *file = fopen("binary.dat", "wb");
    fwrite(&d, sizeof(Data), n, file);
    fclose(file);

    return 0;
}
```

the following Python code will read the binary file `'binary.dat'` into a pandas `DataFrame`, where each element of the struct corresponds to a column in the frame:

```
names = 'count', 'avg', 'scale'

# note that the offsets are larger than the size of the type because of
# struct padding
offsets = 0, 8, 16
formats = 'i4', 'f8', 'f4'
dt = np.dtype({'names': names, 'offsets': offsets, 'formats': formats},
              align=True)
df = pd.DataFrame(np.fromfile('binary.dat', dt))
```

注意：offset问题与机器架构有关，推荐HDF5 or parquet，都支持pandas



# ⭐计算

- 相关性

  `df.corr(method=distcorr)`



# ⭐万能应用：创建示例数据

```python
In [241]: def expand_grid(data_dict):
   .....:     rows = itertools.product(*data_dict.values())
   .....:     return pd.DataFrame.from_records(rows, columns=data_dict.keys())
   .....: 

In [242]: df = expand_grid({'height': [60, 70],
   .....:                   'weight': [100, 140, 180],
   .....:                   'sex': ['Male', 'Female']})
   .....: 

In [243]: df
Out[243]: 
    height  weight     sex
0       60     100    Male
1       60     100  Female
2       60     140    Male
3       60     140  Female
4       60     180    Male
5       60     180  Female
6       70     100    Male
7       70     100  Female
8       70     140    Male
9       70     140  Female
10      70     180    Male
11      70     180  Female
```

