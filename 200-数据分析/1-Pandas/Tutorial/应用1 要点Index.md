- [Different choices for indexing](https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#different-choices-for-indexing)
- [Basics](https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#basics)
- [Attribute access](https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#attribute-access)
- [Slicing ranges](https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#slicing-ranges)
- [Selection by label](https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#selection-by-label)
- [Selection by position](https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#selection-by-position)
- [Selection by callable](https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#selection-by-callable)
- [IX indexer is deprecated](https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#ix-indexer-is-deprecated)
- [Indexing with list with missing labels is deprecated](https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#indexing-with-list-with-missing-labels-is-deprecated)
- [Selecting random samples](https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#selecting-random-samples)
- [Setting with enlargement](https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#setting-with-enlargement)
- [Fast scalar value getting and setting](https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#fast-scalar-value-getting-and-setting)
- [Boolean indexing](https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#boolean-indexing)
- [Indexing with isin](https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#indexing-with-isin)
- [The where() Method and Masking](https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#the-where-method-and-masking)
- [The query() Method](https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#the-query-method)
- [Duplicate data](https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#duplicate-data)
- [Dictionary-like get() method](https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#dictionary-like-get-method)
- [The lookup() method](https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#the-lookup-method)
- [Index objects](https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#index-objects)
- [Set / reset index](https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#set-reset-index)
- [Returning a view versus a copy](https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy)

---

# Indexing and selecting data

axis labeling

Python和NumPy：

- 索引运算符[]
- 属性运算符.



# Basics⭐

| Object Type | Indexers                             |
| :---------- | :----------------------------------- |
| Series      | `s.loc[indexer]`                     |
| DataFrame   | `df.loc[row_indexer,column_indexer]` |

| Object Type | Selection        | Return Value Type                 |
| :---------- | :--------------- | :-------------------------------- |
| Series      | `series[label]`  | scalar value                      |
| DataFrame   | `frame[colname]` | `Series` corresponding to colname |



# 访问属性Attribute access



# 切片Slice



## 基于labels的切片

```python
In [52]: s = pd.Series(list('abcde'), index=[0, 3, 2, 5, 4])

In [53]: s.loc[3:5]
Out[53]: 
3    b
2    c
5    d
dtype: object
```



# 基于位置的选择|0-base



# 基于调用(函数)的选择

```python
In [84]: df1 = pd.DataFrame(np.random.randn(6, 4),
   ....:                    index=list('abcdef'),
   ....:                    columns=list('ABCD'))
   ....: 

In [85]: df1
Out[85]: 
          A         B         C         D
a -0.023688  2.410179  1.450520  0.206053
b -0.251905 -2.213588  1.063327  1.266143
c  0.299368 -0.863838  0.408204 -1.048089
d -0.025747 -0.988387  0.094055  1.262731
e  1.289997  0.082423 -0.055758  0.536580
f -0.489682  0.369374 -0.034571 -2.484478

In [86]: df1.loc[lambda df: df['A'] > 0, :]
Out[86]: 
          A         B         C         D
c  0.299368 -0.863838  0.408204 -1.048089
e  1.289997  0.082423 -0.055758  0.536580
```



# .ix已经被弃用 => .iloc .loc

- .loc：基于label
- .iloc：基于position

- 多个索引 .get_indexer

  - ```python
    In [97]: dfd.iloc[[0, 2], dfd.columns.get_indexer(['A', 'B'])]
    Out[97]: 
       A  B
    a  1  4
    c  3  6
    ```

- 



# 不建议缺少标签的索引 => .reindex

## reindexing





# 选择随机样本 sample()

- 参数weights
- 参数axis

```python
In [114]: s = pd.Series([0, 1, 2, 3, 4, 5])

In [115]: example_weights = [0, 0, 0.2, 0.2, 0.2, 0.4]

In [116]: s.sample(n=3, weights=example_weights)
Out[116]: 
5    5
4    4
3    3
dtype: int64

# Weights will be re-normalized automatically
In [117]: example_weights2 = [0.5, 0, 0, 0, 0, 0]

In [118]: s.sample(n=1, weights=example_weights2)
Out[118]: 
0    0
dtype: int64
```



# 直接添加 => 等价于append⭐

```python
In [126]: se = pd.Series([1, 2, 3])

In [127]: se
Out[127]: 
0    1
1    2
2    3
dtype: int64

In [128]: se[5] = 5.

In [129]: se
Out[129]: 
0    1.0
1    2.0
2    3.0
5    5.0
dtype: float64
    
    
### 针对DataFrame
In [130]: dfi = pd.DataFrame(np.arange(6).reshape(3, 2),
   .....:                    columns=['A', 'B'])
   .....: 

In [131]: dfi
Out[131]: 
   A  B
0  0  1
1  2  3
2  4  5

In [132]: dfi.loc[:, 'C'] = dfi.loc[:, 'A']

In [133]: dfi
Out[133]: 
   A  B  C
0  0  1  0
1  2  3  2
2  4  5  4
```



# 设置标量值|at .iat



# 布尔索引⭐

- |
- &
- ~

可结合map(lambda: )

# .isin索引⭐

```python
In [155]: s = pd.Series(np.arange(5), index=np.arange(5)[::-1], dtype='int64')

In [156]: s
Out[156]: 
4    0
3    1
2    2
1    3
0    4
dtype: int64

In [157]: s.isin([2, 4, 6])
Out[157]: 
4    False
3    False
2     True
1    False
0     True
dtype: bool

In [158]: s[s.isin([2, 4, 6])]
Out[158]: 
2    2
0    4
dtype: int64

    
### 针对df
In [165]: df = pd.DataFrame({'vals': [1, 2, 3, 4], 'ids': ['a', 'b', 'f', 'n'],
   .....:                    'ids2': ['a', 'n', 'c', 'n']})
   .....: 

In [166]: values = ['a', 'b', 1, 3]

In [167]: df.isin(values)
Out[167]: 
    vals    ids   ids2
0   True   True   True
1  False   True  False
2   True  False  False
3  False  False  False
```





# where()方法⭐

- 参数inplace
- 参数axis, level

```python
In [194]: df3 = pd.DataFrame({'A': [1, 2, 3],
   .....:                     'B': [4, 5, 6],
   .....:                     'C': [7, 8, 9]})
   .....: 

### 功能：大于4的元素都+10
In [195]: df3.where(lambda x: x > 4, lambda x: x + 10)
Out[195]: 
    A   B  C
0  11  14  7
1  12   5  8
2  13   6  9
```

df与numpy的区别

- df1.where(m, df2)
- np.where(m, df1, df2)



## mask()方法

逆where()的操作

```python
In [196]: s.mask(s >= 0) #大于0的元素 都要屏蔽掉
Out[196]: 
4   NaN
3   NaN
2   NaN
1   NaN
0   NaN
dtype: float64

In [197]: df.mask(df >= 0)
Out[197]: 
                   A         B         C         D
2000-01-01 -2.104139 -1.309525       NaN       NaN
2000-01-02 -0.352480       NaN -1.192319       NaN
2000-01-03 -0.864883       NaN -0.227870       NaN
2000-01-04       NaN -1.222082       NaN -1.233203
2000-01-05       NaN -0.605656 -1.169184       NaN
2000-01-06       NaN -0.948458       NaN -0.684718
2000-01-07 -2.670153 -0.114722       NaN -0.048048
2000-01-08       NaN       NaN -0.048788 -0.808838
```



# query()方法

```python
In [198]: n = 10

In [199]: df = pd.DataFrame(np.random.rand(n, 3), columns=list('abc'))

In [200]: df
Out[200]: 
          a         b         c
0  0.438921  0.118680  0.863670
1  0.138138  0.577363  0.686602
2  0.595307  0.564592  0.520630
3  0.913052  0.926075  0.616184
4  0.078718  0.854477  0.898725
5  0.076404  0.523211  0.591538
6  0.792342  0.216974  0.564056
7  0.397890  0.454131  0.915716
8  0.074315  0.437913  0.019794
9  0.559209  0.502065  0.026437

# pure python
In [201]: df[(df['a'] < df['b']) & (df['b'] < df['c'])]
Out[201]: 
          a         b         c
1  0.138138  0.577363  0.686602
4  0.078718  0.854477  0.898725
5  0.076404  0.523211  0.591538
7  0.397890  0.454131  0.915716

# query
In [202]: df.query('(a < b) & (b < c)')
Out[202]: 
          a         b         c
1  0.138138  0.577363  0.686602
4  0.078718  0.854477  0.898725
5  0.076404  0.523211  0.591538
7  0.397890  0.454131  0.915716
```

注意：如果索引名与列名重叠，则列名优先



## 针对MultiIndex

```python
In [214]: n = 10

In [215]: colors = np.random.choice(['red', 'green'], size=n)

In [216]: foods = np.random.choice(['eggs', 'ham'], size=n)

In [217]: colors
Out[217]: 
array(['red', 'red', 'red', 'green', 'green', 'green', 'green', 'green',
       'green', 'green'], dtype='<U5')

In [218]: foods
Out[218]: 
array(['ham', 'ham', 'eggs', 'eggs', 'eggs', 'ham', 'ham', 'eggs', 'eggs',
       'eggs'], dtype='<U4')

In [219]: index = pd.MultiIndex.from_arrays([colors, foods], names=['color', 'food'])

In [220]: df = pd.DataFrame(np.random.randn(n, 2), index=index)

In [221]: df
Out[221]: 
                   0         1
color food                    
red   ham   0.194889 -0.381994
      ham   0.318587  2.089075
      eggs -0.728293 -0.090255
green eggs -0.748199  1.318931
      eggs -2.029766  0.792652
      ham   0.461007 -0.542749
      ham  -0.305384 -0.479195
      eggs  0.095031 -0.270099
      eggs -0.707140 -0.773882
      eggs  0.229453  0.304418

In [222]: df.query('color == "red"')
Out[222]: 
                   0         1
color food                    
red   ham   0.194889 -0.381994
      ham   0.318587  2.089075
      eggs -0.728293 -0.090255
      
### ilevel_0表示级别0的index
In [223]: df.index.names = [None, None]

In [224]: df
Out[224]: 
                   0         1
red   ham   0.194889 -0.381994
      ham   0.318587  2.089075
      eggs -0.728293 -0.090255
green eggs -0.748199  1.318931
      eggs -2.029766  0.792652
      ham   0.461007 -0.542749
      ham  -0.305384 -0.479195
      eggs  0.095031 -0.270099
      eggs -0.707140 -0.773882
      eggs  0.229453  0.304418

In [225]: df.query('ilevel_0 == "red"')
Out[225]: 
                 0         1
red ham   0.194889 -0.381994
    ham   0.318587  2.089075
    eggs -0.728293 -0.090255
```



## 示例





## query()支持in与not in



## ==运算符与list对象

- ==类似list对象的in
- !=类似list对象的not in



## 布尔运算符 not ~



## query()性能

大于200,000行时，才可以看到使用numexpr引擎的性能优势(耗费时间少)

![image-20201219094359702](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201219094400.png)

# 重复数据⭐

- duplicated
- drop_duplicates
- keep
  - `keep='first'` （默认）：标记/删除重复项，但第一次出现除外。
  - `keep='last'`：标记/删除重复项（最后一次除外）。
  - `keep=False`：标记/删除所有重复项。

```python
In [264]: df2 = pd.DataFrame({'a': ['one', 'one', 'two', 'two', 'two', 'three', 'four'],
   .....:                     'b': ['x', 'y', 'x', 'y', 'x', 'x', 'x'],
   .....:                     'c': np.random.randn(7)})
   .....: 

In [265]: df2
Out[265]: 
       a  b         c
0    one  x -1.067137
1    one  y  0.309500
2    two  x -0.211056
3    two  y -1.842023
4    two  x -0.390820
5  three  x -1.964475
6   four  x  1.298329

### 指定column
df2.duplicated('a')
df2.duplicated('a', keep='last')


### 删除重复项
df2.drop_duplicates('a')
df2.drop_duplicates('a', keep='last')

### 针对多列
df2.duplicated(['a', 'b'])
df2.drop_duplicates(['a', 'b'])
```





# 类似字典的get()方法

```python
In [280]: s = pd.Series([1, 2, 3], index=['a', 'b', 'c'])

In [281]: s.get('a')  # equivalent to s['a']
Out[281]: 1

In [282]: s.get('x', default=-1)  #如果没有，就返回-1
Out[282]: -1
```



# lookup()方法

```python
In [283]: dflookup = pd.DataFrame(np.random.rand(20, 4), columns = ['A', 'B', 'C', 'D'])

In [284]: dflookup.lookup(list(range(0, 10, 2)), ['B', 'C', 'A', 'B', 'D'])
Out[284]: array([0.3506, 0.4779, 0.4825, 0.9197, 0.5019])
```



# Index 对象⭐

```python
In [285]: index = pd.Index(['e', 'd', 'a', 'b'])

In [286]: index
Out[286]: Index(['e', 'd', 'a', 'b'], dtype='object')

In [287]: 'd' in index
Out[287]: True
    
    
In [288]: index = pd.Index(['e', 'd', 'a', 'b'], name='something')

In [289]: index.name
Out[289]: 'something'
```



## 设置元数据

- 参数level

```python
### 直接设置属性
# 均默认返回一个copy：可设置inplace=True
rename
set_names
set_levels
set_codes

In [295]: ind = pd.Index([1, 2, 3])

In [296]: ind.rename("apple")
Out[296]: Int64Index([1, 2, 3], dtype='int64', name='apple')

In [297]: ind
Out[297]: Int64Index([1, 2, 3], dtype='int64')

In [298]: ind.set_names(["apple"], inplace=True)

In [299]: ind.name = "bob"

In [300]: ind
Out[300]: Int64Index([1, 2, 3], dtype='int64', name='bob')
    
    
    
### 参数level
In [301]: index = pd.MultiIndex.from_product([range(3), ['one', 'two']], names=['first', 'second'])

In [302]: index
Out[302]: 
MultiIndex([(0, 'one'),
            (0, 'two'),
            (1, 'one'),
            (1, 'two'),
            (2, 'one'),
            (2, 'two')],
           names=['first', 'second'])

In [303]: index.levels[1]
Out[303]: Index(['one', 'two'], dtype='object', name='second')

In [304]: index.set_levels(["a", "b"], level=1)
Out[304]: 
MultiIndex([(0, 'a'),
            (0, 'b'),
            (1, 'a'),
            (1, 'b'),
            (2, 'a'),
            (2, 'b')],
           names=['first', 'second'])
```



## index对象的运算符

```python
In [305]: a = pd.Index(['c', 'b', 'a'])

In [306]: b = pd.Index(['c', 'e', 'd'])

In [307]: a | b
Out[307]: Index(['a', 'b', 'c', 'd', 'e'], dtype='object')

In [308]: a & b
Out[308]: Index(['c'], dtype='object')

In [309]: a.difference(b)
Out[309]: Index(['a', 'b'], dtype='object')
    
### 异或：symmetric_difference (^)  => idx1.difference(idx2).union(idx2.difference(idx1))
```



## 缺失值







# 设置/重置 index

## 设置index

- 参数drop
- 参数inplace

```python
In [323]: data
Out[323]: 
     a    b  c    d
0  bar  one  z  1.0
1  bar  two  y  2.0
2  foo  one  x  3.0
3  foo  two  w  4.0

In [324]: indexed1 = data.set_index('c')

In [325]: indexed1
Out[325]: 
     a    b    d
c               
z  bar  one  1.0
y  bar  two  2.0
x  foo  one  3.0
w  foo  two  4.0

In [326]: indexed2 = data.set_index(['a', 'b'])

In [327]: indexed2
Out[327]: 
         c    d
a   b          
bar one  z  1.0
    two  y  2.0
foo one  x  3.0
    two  w  4.0
    
### 参数：append => 在原有index的基础上添加新的index
In [328]: frame = data.set_index('c', drop=False)

In [329]: frame = frame.set_index(['a', 'b'], append=True)

In [330]: frame
Out[330]: 
           c    d
c a   b          
z bar one  z  1.0
y bar two  y  2.0
x foo one  x  3.0
w foo two  w  4.0
```





## 重置index

```python
In [334]: data
Out[334]: 
         c    d
a   b          
bar one  z  1.0
    two  y  2.0
foo one  x  3.0
    two  w  4.0

In [335]: data.reset_index()
Out[335]: 
     a    b  c    d
0  bar  one  z  1.0
1  bar  two  y  2.0
2  foo  one  x  3.0
3  foo  two  w  4.0
```



## 添加临时index

```python
data.index = index
```



# view VS copy

(切片)索引返回的是view







# index的方法

- append() 合并索引
- diff() 计算差集
- intersection() 计算交集
- union() 计算并集
- isin() 计算包含
- delete() 删除已有值
- drop() 删除传入值
- unique/is_unique 计算重复值

需要注意，这样处理之后，返回的是一个新的 Index 对象，比如下面的 index drop。

```python
frame = DataFrame(np.arange(5),index=[1,2,3,4,5],columns=["数字"]);
print(frame)

newindex=frame.index.drop(2)
print(newindex)

   数字
1   0
2   1
3   2
4   3
5   4
Int64Index([1, 3, 4, 5], dtype='int64')
```

