# 操作|合并merge, join, concatenate

pandas中提供了大量的方法能够轻松对Series，DataFrame和Panel对象进行不同满足逻辑关系的合并操作
`df1.join(df2, axis=, join='outer')`
`pd.merge(df1, df2, on=[keys], suffixes=[], how='inner')`  suffixes表示合并后列名的后缀
`pd.concat([df1, df2, df3], axis = )`
`.append()`  只有纵向合并(添加新的数据)
注意：两种方式均可
1）`pd.func(data1, data2)`
2）`data1.func(data2)`

- 堆叠stack
  `df.stack()`
  `df.unstack(0)`  0表示第一行索引展开成列



## concatenate

pd.concat(objs, axis=0, join='outer', ignore_index=False, keys=None,
          levels=None, names=None, verify_integrity=False, copy=True)

- objs：Series或DataFrame
  - 如果是dict：那么使用keys作为index
- ==axis=0 默认跨行 => 合并列==
- ==join==
  - 'inner'表示交集
  - 'outer'表示全集
- ignore_index：当index没有意义时，index重新排列(如果ignore_index=True)
- ==keys==：构造层次结构，如果是多个级别，需要包含元组
- ==levels==：用于构造MultiIndex的特定级别（唯一值）
- ==names==：
- verify_integrity：检查新的轴是否包含重复项(运算消耗大)
- copy：默认拷贝

```python
frames = [df1, df2, df3]
result = pd.concat(frames)
```

示意图：

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201215195814.png" alt="image-20201215195814703" style="zoom: 67%;" />

- 参数 keys

`result = pd.concat(frames, keys=['x', 'y', 'z'])`

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201215195836.png" alt="image-20201215195836715" style="zoom:67%;" />

```python
In [7]: result.loc['y']
Out[7]: 
    A   B   C   D
4  A4  B4  C4  D4
5  A5  B5  C5  D5
6  A6  B6  C6  D6
7  A7  B7  C7  D7
```



- 参数 join

`join='inner'` 交集

![image-20201215200108725](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201215200108.png)

`df4.reindex(df1.index)` 改变df4的index(同df1)





### 自动将Series转换为DataFrame

```python
s2 = pd.Series(['_0', '_1', '_2', '_3'])
result = pd.concat([df1, s2, s2, s2], axis=1)
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201215201700.png" alt="image-20201215201659930" style="zoom:67%;" />



### objs是字典

```python
pieces = {'x': df1, 'y': df2, 'z': df3}
result = pd.concat(pieces)
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201215201910.png" alt="image-20201215201910639" style="zoom:67%;" />





## append

```python
result = df1.append(df2) #不改变df1，返回append之后的copy
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201215200328.png" alt="image-20201215200328085" style="zoom:67%;" />

```python
result = pd.concat([df1, df4], ignore_index=True, sort=False)
result = df1.append(df4, ignore_index=True, sort=False)
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201215200510.png" alt="image-20201215200510326" style="zoom:67%;" />



### 添加行

```python
# Series
s2 = pd.Series(['X0', 'X1', 'X2', 'X3'], index=['A', 'B', 'C', 'D'])
result = df1.append(s2, ignore_index=True)

# 字典
dicts = [{'A': 1, 'B': 2, 'C': 3, 'X': 4},
         {'A': 5, 'B': 6, 'C': 7, 'Y': 8}]
result = df1.append(dicts, ignore_index=True, sort=False)
```

Series：

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201215202358.png" alt="image-20201215202358781" style="zoom:67%;" />

字典：

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201215202614.png" alt="image-20201215202614654" style="zoom:67%;" />



## merge|参考关系数据库

```python
pd.merge(left, right, how='inner', on=None, left_on=None, right_on=None,
         left_index=False, right_index=False, sort=True,
         suffixes=('_x', '_y'), copy=True, indicator=False,
         validate=None)
```

- left：Series或DataFrame对象
- right：同left
- on：left和right共有的level names
  - column
  - index
- left_on：以left对象的levels为主(包括columns和index)
- right_on
- left_index
- right_index
- ==how==
  - left
  - right
  - outer
  - inner
- sort
- suffixes：字符串元组 => 针对命名重叠的columns
- copy
- indicator：在输出数据帧中添加一列称为_merge，其中包含有关每一行的源信息
  - left_only：只在left对象中
  - right_only 
  - both 
- ==validate：检查keys值是否唯一==
  - “one_to_one” or “1:1”: checks if merge keys are unique in both left and right datasets.
  - “one_to_many” or “1:m”: checks if merge keys are unique in left dataset.
  - “many_to_one” or “m:1”: checks if merge keys are unique in right dataset.
  - “many_to_many” or “m:m”: allowed, but does not result in checks.



备注：join方法只考虑index，比merge更有效(参数选项更简洁)

```python
result = pd.merge(left, right, on='key')
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201215203524.png" alt="image-20201215203523978" style="zoom:80%;" />

```python
result = pd.merge(left, right, on=['key1', 'key2'])
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201215203557.png" alt="image-20201215203557429" style="zoom:80%;" />



| Merge method | SQL Join Name      | Description                                   |
| :----------- | :----------------- | :-------------------------------------------- |
| `left`       | `LEFT OUTER JOIN`  | Use keys from left frame only                 |
| `right`      | `RIGHT OUTER JOIN` | Use keys from right frame only                |
| `outer`      | `FULL OUTER JOIN`  | Use ==union== of keys from both frames        |
| `inner`      | `INNER JOIN`       | Use ==intersection== of keys from both frames |

```python
result = pd.merge(left, right, how='left', on=['key1', 'key2'])
result = pd.merge(left, right, how='right', on=['key1', 'key2'])
result = pd.merge(left, right, how='outer', on=['key1', 'key2'])
result = pd.merge(left, right, how='inner', on=['key1', 'key2'])
```

left：

![image-20201215203711947](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201215203712.png)

right：

![image-20201215203717686](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201215203717.png)

outer：

![image-20201215203724159](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201215203724.png)

inner：

![image-20201215203730232](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201215203730.png)

```python
In [49]: df = pd.DataFrame({"Let": ["A", "B", "C"], "Num": [1, 2, 3]})

In [50]: df
Out[50]: 
  Let  Num
0   A    1
1   B    2
2   C    3

In [51]: ser = pd.Series(
   ....:     ["a", "b", "c", "d", "e", "f"],
   ....:     index=pd.MultiIndex.from_arrays(
   ....:         [["A", "B", "C"] * 2, [1, 2, 3, 4, 5, 6]], names=["Let", "Num"]
   ....:     ),
   ....: )
   ....: 

In [52]: ser  #多个index(2个列表)
Out[52]: 
Let  Num
A    1      a
B    2      b
C    3      c
A    4      d
B    5      e
C    6      f
dtype: object

In [53]: pd.merge(df, ser.reset_index(), on=['Let', 'Num'])
Out[53]: 
  Let  Num  0
0   A    1  a
1   B    2  b
2   C    3  c
```



### keys重复

```python
# 列名重复
left = pd.DataFrame({'A': [1, 2], 'B': [2, 2]})
right = pd.DataFrame({'A': [4, 5, 6], 'B': [2, 2, 2]})
result = pd.merge(left, right, on='B', how='outer')
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201215205131.png" alt="image-20201215205131581" style="zoom:67%;" />

注意：join/merge在重复的keys上，导致返回的维度是rows*columns，容易造成内存溢出

- 参数validate => 检查keys值得唯一性
- 参数_merge

```python
In [60]: df1 = pd.DataFrame({'col1': [0, 1], 'col_left': ['a', 'b']})

In [61]: df2 = pd.DataFrame({'col1': [1, 2, 2], 'col_right': [2, 2, 2]})

In [62]: pd.merge(df1, df2, on='col1', how='outer', indicator=True)
Out[62]: 
   col1 col_left  col_right      _merge
0     0        a        NaN   left_only
1     1        b        2.0        both
2     2      NaN        2.0  right_only
3     2      NaN        2.0  right_only
```



备注：category 类别

- merge针对category类别与object类比，操作一致





## join

```python
left.join(right, on=key_or_keys)
pd.merge(left, right, left_on=key_or_keys, right_index=True,
      how='left', sort=False)
```



```python
result = left.join(right)  #不论index是否相同，简单合并的一种方式
```

![image-20201215205905802](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201215205905.png)



### 基于key columns连接index

```python
result = pd.merge(left, right, left_on='key', right_index=True,
                  how='left', sort=False);
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201215210100.png" alt="image-20201215210100152" style="zoom:80%;" />

### 单个index连接到MultiIndex

```python
### 针对MultiIndex
In [95]: left = pd.DataFrame({'A': ['A0', 'A1', 'A2', 'A3'],
   ....:                      'B': ['B0', 'B1', 'B2', 'B3'],
   ....:                      'key1': ['K0', 'K0', 'K1', 'K2'],
   ....:                      'key2': ['K0', 'K1', 'K0', 'K1']})
   ....: 

In [96]: index = pd.MultiIndex.from_tuples([('K0', 'K0'), ('K1', 'K0'),
   ....:                                   ('K2', 'K0'), ('K2', 'K1')])
   ....: 

In [97]: right = pd.DataFrame({'C': ['C0', 'C1', 'C2', 'C3'],
   ....:                       'D': ['D0', 'D1', 'D2', 'D3']},
   ....:                      index=index)  #生成MultiIndex
   ....: 
In [98]: result = left.join(right, on=['key1', 'key2'])
```

![image-20201215210209876](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201215210210.png)



### 连接两个MultiIndexes

```python
In [112]: leftindex = pd.MultiIndex.from_tuples([('K0', 'X0'), ('K0', 'X1'),
   .....:                                        ('K1', 'X2')],
   .....:                                       names=['key', 'X'])
   .....: 

In [113]: left = pd.DataFrame({'A': ['A0', 'A1', 'A2'],
   .....:                      'B': ['B0', 'B1', 'B2']},
   .....:                     index=leftindex)
   .....: 

In [114]: rightindex = pd.MultiIndex.from_tuples([('K0', 'Y0'), ('K1', 'Y1'),
   .....:                                         ('K2', 'Y2'), ('K2', 'Y3')],
   .....:                                        names=['key', 'Y'])
   .....: 

In [115]: right = pd.DataFrame({'C': ['C0', 'C1', 'C2', 'C3'],
   .....:                       'D': ['D0', 'D1', 'D2', 'D3']},
   .....:                      index=rightindex)
   .....: 

In [116]: result = pd.merge(left.reset_index(), right.reset_index(),
   .....:                   on=['key'], how='inner').set_index(['key', 'X', 'Y'])
   .....: 
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201215210602.png" alt="image-20201215210602624"  />

- left的multiindex：K和X
- right的multiindex：K和Y



### 混合columns和index level的连接

```python
In [117]: left_index = pd.Index(['K0', 'K0', 'K1', 'K2'], name='key1')

In [118]: left = pd.DataFrame({'A': ['A0', 'A1', 'A2', 'A3'],
   .....:                      'B': ['B0', 'B1', 'B2', 'B3'],
   .....:                      'key2': ['K0', 'K1', 'K0', 'K1']},
   .....:                     index=left_index)
   .....: 

In [119]: right_index = pd.Index(['K0', 'K1', 'K2', 'K2'], name='key1')

In [120]: right = pd.DataFrame({'C': ['C0', 'C1', 'C2', 'C3'],
   .....:                       'D': ['D0', 'D1', 'D2', 'D3'],
   .....:                       'key2': ['K0', 'K0', 'K0', 'K1']},
   .....:                      index=right_index)
   .....: 

In [121]: result = left.merge(right, on=['key1', 'key2'])
```

![image-20201215210954878](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201215210955.png)

- key1是index level
- key2是column



### columns的命名重复

- 参数 suffixes

```python
result = pd.merge(left, right, on='k', suffixes=('_l', '_r'))
```

![image-20201215211144601](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201215211144.png)

```python
left = left.set_index('k')
right = right.set_index('k')
result = left.join(right, lsuffix='_l', rsuffix='_r')
```

![image-20201215211217577](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201215211217.png)





## 合并多个DataFrames

```python
right2 = pd.DataFrame({'v': [7, 8, 9]}, index=['K1', 'K1', 'K2'])
result = left.join([right, right2])
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201215211408.png" alt="image-20201215211407914" style="zoom:80%;" />

```python
### 两个相似index的DataFrame：进行合并/修补
df1 = pd.DataFrame([[np.nan, 3., 5.], [-4.6, np.nan, np.nan],
                    [np.nan, 7., np.nan]])

df2 = pd.DataFrame([[-42.6, np.nan, -8.2], [-5., 1.6, 4]],index=[1, 2])

result = df1.combine_first(df2)  #重点在df1：左边值缺少，用右边的值修补

df1.update(df2) #重点在df2
```

combine_first：

![image-20201215211647746](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201215211647.png)

update：

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201215211940.png" alt="image-20201215211940035" style="zoom:80%;" />



## 针对Timeseries的合并

- Timeseries可泛指一切有序数据

```python
pd.merge_ordered(left, right, left_by='s')  #输出结果：s列有序
# 可选参数fill_method='ffill'
```

left：

![image-20201215212408708](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201215212408.png)

right：

![image-20201215212430944](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201215212431.png)

merge后：

![image-20201215212639235](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201215212639.png)

```python
pd.merge_asof(trades, quotes, on='time', by='ticker') #以trades为主
```

trades：交易

![image-20201215212821829](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201215212821.png)

quotes：报价

![image-20201215212846314](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201215212846.png)

merge_asof：

![image-20201215212919554](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201215212919.png)

```python
### 实用：仅在报价时间和交易时间之间的2毫秒之内进行结算 0.002s
pd.merge_asof(trades, quotes, on='time', by='ticker', tolerance=pd.Timedelta('2ms'))
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201215213312.png" alt="image-20201215213312775" style="zoom:80%;" />

0.038的MSFT没有匹配的报价

而AAPL：报价比交易晚了0.001 => 说明交易前后$\pm0.002$s以内均可



```python
### 实用1：仅在报价时间和交易时间之间的10毫秒之内进行结算 0.010s
### 实用2：时间点相同的排除在外
pd.merge_asof(trades, quotes, on='time', by='ticker', tolerance=pd.Timedelta('10ms'), allow_exact_matches=False)
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201215213836.png" alt="image-20201215213836772" style="zoom:80%;" />



## compare()

针对两个DataFrame 或 Series

```python
df2 = df.copy()
df2.loc[0, 'col1'] = 'c'
df2.loc[2, 'col3'] = 4.0

In [151]: df.compare(df2)
Out[151]: 
  col1       col3      
  self other self other
0    a     c  NaN   NaN
2  NaN   NaN  3.0   4.0

In [151]: df.compare(df2)
Out[151]: 
  col1       col3      
  self other self other
0    a     c  NaN   NaN
2  NaN   NaN  3.0   4.0
### 参数
# align_axis self和other改变到index的位置
# keep_shape：保留DataFrame的形状
In [153]: df.compare(df2, keep_shape=True)
Out[153]: 
  col1       col2       col3      
  self other self other self other
0    a     c  NaN   NaN  NaN   NaN
1  NaN   NaN  NaN   NaN  NaN   NaN
2  NaN   NaN  NaN   NaN  3.0   4.0
3  NaN   NaN  NaN   NaN  NaN   NaN
4  NaN   NaN  NaN   NaN  NaN   NaN
```

df：

![image-20201215214014570](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201215214014.png)

