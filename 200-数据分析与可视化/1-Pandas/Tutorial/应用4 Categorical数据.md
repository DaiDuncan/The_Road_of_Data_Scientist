- [Object creation](https://pandas.pydata.org/pandas-docs/stable/user_guide/categorical.html#object-creation)
- [CategoricalDtype](https://pandas.pydata.org/pandas-docs/stable/user_guide/categorical.html#categoricaldtype)
- [Description](https://pandas.pydata.org/pandas-docs/stable/user_guide/categorical.html#description)
- [Working with categories](https://pandas.pydata.org/pandas-docs/stable/user_guide/categorical.html#working-with-categories)
- [Sorting and order](https://pandas.pydata.org/pandas-docs/stable/user_guide/categorical.html#sorting-and-order)
- [Comparisons](https://pandas.pydata.org/pandas-docs/stable/user_guide/categorical.html#comparisons)
- [Operations](https://pandas.pydata.org/pandas-docs/stable/user_guide/categorical.html#operations)
- [Data munging](https://pandas.pydata.org/pandas-docs/stable/user_guide/categorical.html#data-munging)
- [Getting data in/out](https://pandas.pydata.org/pandas-docs/stable/user_guide/categorical.html#getting-data-in-out)
- [Missing data](https://pandas.pydata.org/pandas-docs/stable/user_guide/categorical.html#missing-data)
- [Differences to R’s factor](https://pandas.pydata.org/pandas-docs/stable/user_guide/categorical.html#differences-to-r-s-factor)
- [Gotchas](https://pandas.pydata.org/pandas-docs/stable/user_guide/categorical.html#gotchas)

---

分类变量具有有限的且通常是固定的数量的可能值（类别；R中的级别）。例子包括

- 性别，社会阶层，血型，国家隶属关系，观察时间
- 通过李克特量表的评分



分类数据的所有值都在类别或np.nan中



# 创建对象

```python
In [1]: s = pd.Series(["a", "b", "c", "a"], dtype="category")

In [2]: s
Out[2]: 
0    a
1    b
2    c
3    a
dtype: category
Categories (3, object): ['a', 'b', 'c']
    
    
### .astype
In [3]: df = pd.DataFrame({"A": ["a", "b", "c", "a"]})
In [4]: df["B"] = df["A"].astype('category')
```



## .cut()分组

```python
In [6]: df = pd.DataFrame({'value': np.random.randint(0, 100, 20)})

In [7]: labels = ["{0} - {1}".format(i, i + 9) for i in range(0, 100, 10)]

In [8]: df['group'] = pd.cut(df.value, range(0, 105, 10), right=False, labels=labels)

In [9]: df.head(10)
Out[9]: 
   value    group
0     65  60 - 69
1     49  40 - 49
2     56  50 - 59
3     43  40 - 49
4     43  40 - 49
5     91  90 - 99
6     32  30 - 39
7     87  80 - 89
8     36  30 - 39
9      8    0 - 9
```



## 控制默认行为|**CategoricalDtype**

默认行为：

1. 从数据推断类别。
2. 类别是无序的。

```python
In [26]: from pandas.api.types import CategoricalDtype

In [27]: s = pd.Series(["a", "b", "c", "a"])

In [28]: cat_type = CategoricalDtype(categories=["b", "c", "d"],
   ....:                             ordered=True)
   ....: 

In [29]: s_cat = s.astype(cat_type)

In [30]: s_cat
Out[30]: 
0    NaN
1      b
2      c
3    NaN
dtype: category
Categories (3, object): ['b' < 'c' < 'd']
```



```python
# 设置类别分类categories
categories = pd.unique(df.to_numpy().ravel())

### 编码codes + 分类categories => 对应
In [37]: splitter = np.random.choice([0, 1], 5, p=[0.5, 0.5]) #array([0, 0, 1, 1, 0])

In [38]: s = pd.Series(pd.Categorical.from_codes(splitter,
   ....:                                         categories=["train", "test"]))

#结果
0    train
1    train
2     test
3     test
4    train
dtype: category
Categories (2, object): ['train', 'test']
```



```python
### 恢复原始数据：数组与类别数组的转换
In [39]: s = pd.Series(["a", "b", "c", "a"])

In [40]: s
Out[40]: 
0    a
1    b
2    c
3    a
dtype: object

In [41]: s2 = s.astype('category')
In [42]: s2
Out[42]: 
0    a
1    b
2    c
3    a
dtype: category
Categories (3, object): ['a', 'b', 'c']

In [43]: s2.astype(str)
Out[43]: 
0    a
1    b
2    c
3    a
dtype: object

In [44]: np.asarray(s2)
Out[44]: array(['a', 'b', 'c', 'a'], dtype=object)
```

注意：

- 与R的因子函数相反，分类数据不会将输入值转换为字符串；类别最终将具有与原始值相同的数据类型。
- 与R的因子函数相反，当前无法在创建时分配/更改标签。创建类别后，请使用类别更改类别。





# 数据类型

类型：

1. `categories`：唯一值序列，无缺失值
2. `ordered`：一个布尔值

```python
In [45]: from pandas.api.types import CategoricalDtype

In [46]: CategoricalDtype(['a', 'b', 'c'])
Out[46]: CategoricalDtype(categories=['a', 'b', 'c'], ordered=False)

In [47]: CategoricalDtype(['a', 'b', 'c'], ordered=True)
Out[47]: CategoricalDtype(categories=['a', 'b', 'c'], ordered=True)

In [48]: CategoricalDtype()
Out[48]: CategoricalDtype(categories=None, ordered=False)
    
    
### 类别无序等价：类似集合
In [49]: c1 = CategoricalDtype(['a', 'b', 'c'], ordered=False)

# Equal, since order is not considered when ordered=False
In [50]: c1 == CategoricalDtype(['b', 'c', 'a'], ordered=False)
Out[50]: True

# Unequal, since the second CategoricalDtype is ordered
In [51]: c1 == CategoricalDtype(['a', 'b', 'c'], ordered=True)
Out[51]: False
    
    
### 所有CategoricalDtypecompare的实例都等于string 'category'
In [52]: c1 == 'category'
Out[52]: True
```





# 处理分类数据

## 属性：

- .cat.ordered
- .cat.categories

```python
In [57]: s = pd.Series(["a", "b", "c", "a"], dtype="category")

In [58]: s.cat.categories
Out[58]: Index(['a', 'b', 'c'], dtype='object')

In [59]: s.cat.ordered #新的分类数据不会自动排序 必须明确传递ordered=True
Out[59]: False
```

方法：

## .describe()

## .unique()

## .rename_categories()

```python
In [53]: cat = pd.Categorical(["a", "c", "c", np.nan], categories=["b", "a", "c"])

In [54]: df = pd.DataFrame({"cat": cat, "s": ["a", "c", "c", np.nan]})

### .describe()
In [55]: df.describe()
Out[55]: 
       cat  s
count    3  3
unique   2  2
top      c  c
freq     2  2

In [56]: df["cat"].describe()
Out[56]: 
count     3
unique    2
top       c
freq      2
Name: cat, dtype: object
        
### .unique()
In [66]: s.unique()

    
### .rename_categories()
# 列表格式
In [71]: s = s.cat.rename_categories([1, 2, 3])
In [72]: s
Out[72]: 
0    1
1    2
2    3
3    1
dtype: category
Categories (3, int64): [1, 2, 3]
    
    
# 字典格式
In [73]: s = s.cat.rename_categories({1: 'x', 2: 'y', 3: 'z'})
In [74]: s
Out[74]: 
0    x
1    y
2    z
3    x
dtype: category
Categories (3, object): ['x', 'y', 'z']
```

分类数据可以具有==除字符串以外==的其他类型的分类

注意，分配新类别是一个就地操作，而`Series.cat`默认情况下，大多数其他操作将返回一个新`Series`的dtype category。

注意：

```python
### 类别必须是唯一的，否则会引发ValueError：
In [75]: try:
   ....:     s.cat.categories = [1, 1, 1]
   ....: except ValueError as e:
   ....:     print("ValueError:", str(e))
   ....: 
ValueError: Categorical categories must be unique
    
### 类别也不能是空的/np.nan
In [76]: try:
   ....:     s.cat.categories = [1, 2, np.nan]
   ....: except ValueError as e:
   ....:     print("ValueError:", str(e))
   ....: 
ValueError: Categorical categories cannot be null
```



## .add_categories() 添加新的类别

## .remove_categories() 删除类别：删除的值将替换为np.nan

## .remove_unused_categories() 删除未使用的类别

## .set_categories() 设置类别

```python
### .add_categories()
In [77]: s = s.cat.add_categories([4])
In [78]: s.cat.categories
Out[78]: Index(['x', 'y', 'z', 4], dtype='object')

In [79]: s
Out[79]: 
0    x
1    y
2    z
3    x
dtype: category
Categories (4, object): ['x', 'y', 'z', 4]
   

### .remove_categories
In [80]: s = s.cat.remove_categories([4])

In [81]: s
Out[81]: 
0    x
1    y
2    z
3    x
dtype: category
Categories (3, object): ['x', 'y', 'z']
    
    
### .remove_unused_categories()
In [82]: s = pd.Series(pd.Categorical(["a", "b", "a"],
   ....:               categories=["a", "b", "c", "d"]))
   ....: 

In [83]: s
Out[83]: 
0    a
1    b
2    a
dtype: category
Categories (4, object): ['a', 'b', 'c', 'd']

In [84]: s.cat.remove_unused_categories()
Out[84]: 
0    a
1    b
2    a
dtype: category
Categories (2, object): ['a', 'b']
    
    
### .set_categories() 设置类别
In [85]: s = pd.Series(["one", "two", "four", "-"], dtype="category")

In [86]: s
Out[86]: 
0     one
1     two
2    four
3       -
dtype: category
Categories (4, object): ['-', 'four', 'one', 'two']

In [87]: s = s.cat.set_categories(["one", "two", "three", "four"])

In [88]: s
Out[88]: 
0     one
1     two
2    four
3     NaN
dtype: category
Categories (4, object): ['one', 'two', 'three', 'four']
```



# 排序

注意：如果分类是无序的，调用.min()/.max()将引发一个TypeError

- 分类是有序的：s.cat.ordered == True

```python
In [89]: s = pd.Series(pd.Categorical(["a", "b", "c", "a"], ordered=False))

In [90]: s.sort_values(inplace=True)

In [91]: s = pd.Series(["a", "b", "c", "a"]).astype(
   ....:     CategoricalDtype(ordered=True)
   ....: )
   ....: 

In [92]: s.sort_values(inplace=True)

In [93]: s
Out[93]: 
0    a
3    a
1    b
2    c
dtype: category
Categories (3, object): ['a' < 'b' < 'c']

In [94]: s.min(), s.max()
Out[94]: ('a', 'c')
```



## 参数as_ordered()/as_unordered()

```python
In [95]: s.cat.as_ordered()
Out[95]: 
0    a
3    a
1    b
2    c
dtype: category
Categories (3, object): ['a' < 'b' < 'c']

In [96]: s.cat.as_unordered()
Out[96]: 
0    a
3    a
1    b
2    c
dtype: category
Categories (3, object): ['a', 'b', 'c']
```





## 重新排序.reorder_categories()/.set_categories()

- .set_categories()

```python
In [97]: s = pd.Series([1, 2, 3, 1], dtype="category")
In [98]: s = s.cat.set_categories([2, 3, 1], ordered=True)

In [99]: s
Out[99]: 
0    1
1    2
2    3
3    1
dtype: category
Categories (3, int64): [2 < 3 < 1]

In [100]: s.sort_values(inplace=True)

In [101]: s
Out[101]: 
1    2
2    3
0    1
3    1
dtype: category
Categories (3, int64): [2 < 3 < 1]

In [102]: s.min(), s.max()  #返回类别2和类别1
Out[102]: (2, 1)
```

- .reorder_categories()

对于Categorical.reorder_categories()，所有旧类别都必须包含在新类别中，并且不允许新类别。这将必然使排序顺序与类别顺序相同。

```python
In [103]: s = pd.Series([1, 2, 3, 1], dtype="category")
In [104]: s = s.cat.reorder_categories([2, 3, 1], ordered=True)

In [105]: s
Out[105]: 
0    1
1    2
2    3
3    1
dtype: category
Categories (3, int64): [2 < 3 < 1]

In [106]: s.sort_values(inplace=True)
In [107]: s
Out[107]: 
1    2
2    3
0    1
3    1
dtype: category
Categories (3, int64): [2 < 3 < 1]

In [108]: s.min(), s.max()
Out[108]: (2, 1)
```



## 多个columns排序

```python
In [109]: dfs = pd.DataFrame({'A': pd.Categorical(list('bbeebbaa'),
   .....:                                         categories=['e', 'a', 'b'],
   .....:                                         ordered=True),
   .....:                     'B': [1, 2, 1, 2, 2, 1, 2, 1]})
   .....: 

In [110]: dfs.sort_values(by=['A', 'B'])
Out[110]: 
   A  B
2  e  1
3  e  2
7  a  1
6  a  2
0  b  1
5  b  1
1  b  2
4  b  2
```





# 比较

可以比较的三种情况：

| 长度一致的类似列表的对象(list, Series, array, …) | ==<br>!=             |
| ------------------------------------------------ | -------------------- |
| ordered==True前提下：两个相同分类对象的比较      | ==，!=，>，>=，<，<= |
| 分类数据与标量的所有比较                         |                      |

```python
In [113]: cat = pd.Series([1, 2, 3]).astype(
   .....:     CategoricalDtype([3, 2, 1], ordered=True)
   .....: )
   .....: 
In [114]: cat_base = pd.Series([2, 2, 2]).astype(
   .....:     CategoricalDtype([3, 2, 1], ordered=True)
   .....: )
   .....: 
In [115]: cat_base2 = pd.Series([2, 2, 2]).astype(
   .....:     CategoricalDtype(ordered=True)
   .....: )
   .....: 

    
In [116]: cat
Out[116]: 
0    1
1    2
2    3
dtype: category
Categories (3, int64): [3 < 2 < 1]

In [117]: cat_base
Out[117]: 
0    2
1    2
2    2
dtype: category
Categories (3, int64): [3 < 2 < 1]

In [118]: cat_base2 
Out[118]: 
0    2
1    2
2    2
dtype: category
Categories (1, int64): [2] #与前两者类别不相同
    
    
### 比较
# 具有相同类别和顺序的分类比较
In [119]: cat > cat_base
Out[119]: 
0     True
1    False
2    False
dtype: bool

# 与标量比较
In [120]: cat > 2
Out[120]: 
0     True
1    False
2    False
dtype: bool
    
# 具有相同长度和标量的任何类似列表的对象
In [121]: cat == cat_base
Out[121]: 
0    False
1     True
2    False
dtype: bool

In [122]: cat == np.array([1, 2, 3])
Out[122]: 
0    True
1    True
2    True
dtype: bool

In [123]: cat == 2
Out[123]: 
0    False
1     True
2    False
dtype: bool
    
    
### 注意：两个类别对象不相同
In [124]: try:
   .....:     cat > cat_base2
   .....: except TypeError as e:
   .....:     print("TypeError:", str(e))
   .....: 
TypeError: Categoricals can only be compared if 'categories' are the same. Categories are different lengths
    
    
### 如需比较：将类型明确转化为原始值
In [125]: base = np.array([1, 2, 3])

In [126]: try:
   .....:     cat > base
   .....: except TypeError as e:
   .....:     print("TypeError:", str(e))
   .....: 
TypeError: Cannot compare a Categorical for op __gt__ with type <class 'numpy.ndarray'>.
If you want to compare values, use 'np.asarray(cat) <op> other'.

In [127]: np.asarray(cat) > base
Out[127]: array([False, False, False])
    
    
### 针对无序分类
In [128]: c1 = pd.Categorical(['a', 'b'], categories=['a', 'b'], ordered=False)
In [129]: c2 = pd.Categorical(['a', 'b'], categories=['b', 'a'], ordered=False)

In [130]: c1 == c2
Out[130]: array([ True,  True])
```





# 运算

- ser.min()
- ser.max()
- ser.mode()
- ser.value_counts()

```python
In [131]: s = pd.Series(pd.Categorical(["a", "b", "c", "c"],
   .....:               categories=["c", "a", "b", "d"]))
   .....: 

In [132]: s.value_counts()
Out[132]: 
c    2
b    1
a    1
d    0
dtype: int64
```



- df.groupby()：将显示“未使用”类别

```python
In [133]: cats = pd.Categorical(["a", "b", "b", "b", "c", "c", "c"],
   .....:                       categories=["a", "b", "c", "d"])
   .....: 

In [134]: df = pd.DataFrame({"cats": cats, "values": [1, 2, 2, 2, 3, 4, 5]})

In [135]: df.groupby("cats").mean()
Out[135]: 
      values
cats        
a        1.0
b        2.0
c        4.0
d        NaN

In [136]: cats2 = pd.Categorical(["a", "a", "b", "b"], categories=["a", "b", "c"])

In [137]: df2 = pd.DataFrame({"cats": cats2,
   .....:                     "B": ["c", "d", "c", "d"],
   .....:                     "values": [1, 2, 3, 4]})
   .....: 

In [138]: df2.groupby(["cats", "B"]).mean()
Out[138]: 
        values
cats B        
a    c     1.0
     d     2.0
b    c     3.0
     d     4.0
c    c     NaN
     d     NaN
```

- pd.pivot_table()

```python
In [139]: raw_cat = pd.Categorical(["a", "a", "b", "b"], categories=["a", "b", "c"])

In [140]: df = pd.DataFrame({"A": raw_cat,
   .....:                    "B": ["c", "d", "c", "d"],
   .....:                    "values": [1, 2, 3, 4]})
   .....: 

In [141]: pd.pivot_table(df, values='values', index=['A', 'B'])
Out[141]: 
     values
A B        
a c       1
  d       2
b c       3
  d       4
```





# 数据处理

## 查：索引访问

- .loc
- .iloc
- .at
- .iat

```python
In [142]: idx = pd.Index(["h", "i", "j", "k", "l", "m", "n"])

In [143]: cats = pd.Series(["a", "b", "b", "b", "c", "c", "c"],
   .....:                  dtype="category", index=idx)
   .....: 

In [144]: values = [1, 2, 2, 2, 3, 4, 5]

In [145]: df = pd.DataFrame({"cats": cats, "values": values}, index=idx)

In [146]: df.iloc[2:4, :]
Out[146]: 
  cats  values
j    b       2
k    b       2

In [147]: df.iloc[2:4, :].dtypes
Out[147]: 
cats      category
values       int64
dtype: object

In [148]: df.loc["h":"j", "cats"]
Out[148]: 
h    a
i    b
j    b
Name: cats, dtype: category
Categories (3, object): ['a', 'b', 'c']

In [149]: df[df["cats"] == "b"]
Out[149]: 
  cats  values
i    b       2
j    b       2
k    b       2


In [150]: df.loc["h", :]
In [151]: df.iat[0, 0]
In [153]: df.at["h", "cats"]  # returns a string 'x'
```

相对于R的factor函数，后者`factor(c(1,2,3))[1]` 返回单个值factor



## 查：字符串 日期时间访问

- .dt
- .str

```python
In [155]: str_s = pd.Series(list('aabb'))

In [156]: str_cat = str_s.astype('category')

In [157]: str_cat
Out[157]: 
0    a
1    a
2    b
3    b
dtype: category
Categories (2, object): ['a', 'b']

In [158]: str_cat.str.contains("a")
Out[158]: 
0     True
1     True
2    False
3    False
dtype: bool

In [159]: date_s = pd.Series(pd.date_range('1/1/2015', periods=5))

In [160]: date_cat = date_s.astype('category')

In [161]: date_cat
Out[161]: 
0   2015-01-01
1   2015-01-02
2   2015-01-03
3   2015-01-04
4   2015-01-05
dtype: category
Categories (5, datetime64[ns]): [2015-01-01, 2015-01-02, 2015-01-03, 2015-01-04, 2015-01-05]

In [162]: date_cat.dt.day
Out[162]: 
0    1
1    2
2    3
3    4
4    5
dtype: int64
```





## 改/设置

```python
In [167]: idx = pd.Index(["h", "i", "j", "k", "l", "m", "n"])

# 创建Categorical对象
In [168]: cats = pd.Categorical(["a", "a", "a", "a", "a", "a", "a"],
   .....:                       categories=["a", "b"])
   .....: 

In [169]: values = [1, 1, 1, 1, 1, 1, 1]

In [170]: df = pd.DataFrame({"cats": cats, "values": values}, index=idx)

In [171]: df.iloc[2:4, :] = [["b", 2], ["b", 2]] #Series类别中含有b

In [172]: df
Out[172]: 
  cats  values
h    a       1
i    a       1
j    b       2
k    b       2
l    a       1
m    a       1
n    a       1

In [173]: try:
   .....:     df.iloc[2:4, :] = [["c", 3], ["c", 3]] #Series类别中不含c
   .....: except ValueError as e:
   .....:     print("ValueError:", str(e))
   .....: 
ValueError: Cannot setitem on a Categorical with a new category, set the categories first
```





## Merging / concatenation

Ser或DF均为相同类别 =>  组合生成category数据类型

非categorical数据类型的合并会导致更高的内存消耗 => 使用.astype转换数据类型

```python
In [182]: from pandas.api.types import union_categoricals

# same categories
In [183]: s1 = pd.Series(['a', 'b'], dtype='category')

In [184]: s2 = pd.Series(['a', 'b', 'a'], dtype='category')

In [185]: pd.concat([s1, s2])
Out[185]: 
0    a
1    b
0    a
1    b
2    a
dtype: category
Categories (2, object): ['a', 'b']

# different categories：内存消耗大
In [186]: s3 = pd.Series(['b', 'c'], dtype='category')

In [187]: pd.concat([s1, s3])
Out[187]: 
0    a
1    b
0    b
1    c
dtype: object

# Output dtype is inferred based on categories values
In [188]: int_cats = pd.Series([1, 2], dtype="category")

In [189]: float_cats = pd.Series([3.0, 4.0], dtype="category")

In [190]: pd.concat([int_cats, float_cats])
Out[190]: 
0    1.0
1    2.0
0    3.0
1    4.0
dtype: float64

In [191]: pd.concat([s1, s3]).astype('category') #改变数据类型
Out[191]: 
0    a
1    b
0    b
1    c
dtype: category
Categories (3, object): ['a', 'b', 'c']

In [192]: union_categoricals([s1.array, s3.array])
Out[192]: 
['a', 'b', 'b', 'c']
Categories (3, object): ['a', 'b', 'c']
```

| arg1              | arg2              | identical | result                     |
| :---------------- | :---------------- | :-------- | :------------------------- |
| category          | category          | True      | category                   |
| category (object) | category (object) | False     | object (dtype is inferred) |
| category (int)    | category (float)  | False     | float (dtype is inferred)  |



## Unioning

合并不是相同类别的分类对象

```python
### union_categoricals => 注意合并有序类别对象时：顺序要一致，否则报错
In [193]: from pandas.api.types import union_categoricals

In [194]: a = pd.Categorical(["b", "c"])
In [195]: b = pd.Categorical(["a", "b"])

In [196]: union_categoricals([a, b])
Out[196]: 
['b', 'c', 'a', 'b']
Categories (3, object): ['b', 'c', 'a']
    
    
### 参数：sort_categories=True
In [197]: union_categoricals([a, b], sort_categories=True)
Out[197]: 
['b', 'c', 'a', 'b']
Categories (3, object): ['a', 'b', 'c']
    
    
### 参数：ignore_ordered=True参数组合具有不同类别或顺序的有序分类
In [201]: a = pd.Categorical(["a", "b", "c"], ordered=True)

In [202]: b = pd.Categorical(["c", "b", "a"], ordered=True)

In [203]: union_categoricals([a, b], ignore_order=True)
Out[203]: 
['a', 'b', 'c', 'c', 'b', 'a']
Categories (3, object): ['a', 'b', 'c']
```



注意：可能会改变类别的codes

```python
In [207]: c1 = pd.Categorical(["b", "c"])

In [208]: c2 = pd.Categorical(["a", "b"])

In [209]: c1
Out[209]: 
['b', 'c']
Categories (2, object): ['b', 'c']

# "b" is coded to 0
In [210]: c1.codes
Out[210]: array([0, 1], dtype=int8)

In [211]: c2
Out[211]: 
['a', 'b']
Categories (2, object): ['a', 'b']

# "b" is coded to 1
In [212]: c2.codes
Out[212]: array([0, 1], dtype=int8)

In [213]: c = union_categoricals([c1, c2])

In [214]: c
Out[214]: 
['b', 'c', 'a', 'b']
Categories (3, object): ['b', 'c', 'a']

# "b" is coded to 0 throughout, same as c1, different from c2
In [215]: c.codes
Out[215]: array([0, 1, 2, 0], dtype=int8)
```



# 数据I/O

- HDFStore
- Stata格式文件
- CSV
- SQL：to_sql

```python
In [216]: import io

In [217]: s = pd.Series(pd.Categorical(['a', 'b', 'b', 'a', 'a', 'd']))

# rename the categories
In [218]: s.cat.categories = ["very good", "good", "bad"]

# reorder the categories and add missing categories
In [219]: s = s.cat.set_categories(["very bad", "bad", "medium", "good", "very good"])

In [220]: df = pd.DataFrame({"cats": s, "vals": [1, 2, 3, 4, 5, 6]})

### csv文件
In [221]: csv = io.StringIO()

In [222]: df.to_csv(csv)

In [223]: df2 = pd.read_csv(io.StringIO(csv.getvalue()))

In [224]: df2.dtypes
Out[224]: 
Unnamed: 0     int64
cats          object
vals           int64
dtype: object

In [225]: df2["cats"]
Out[225]: 
0    very good
1         good
2         good
3    very good
4    very good
5          bad
Name: cats, dtype: object

# Redo the category
In [226]: df2["cats"] = df2["cats"].astype("category")

In [227]: df2["cats"].cat.set_categories(["very bad", "bad", "medium",
   .....:                                 "good", "very good"],
   .....:                                inplace=True)
   .....: 

In [228]: df2.dtypes
Out[228]: 
Unnamed: 0       int64
cats          category
vals             int64
dtype: object

In [229]: df2["cats"]
Out[229]: 
0    very good
1         good
2         good
3    very good
4    very good
5          bad
Name: cats, dtype: category
Categories (5, object): ['very bad', 'bad', 'medium', 'good', 'very good']
```







# 缺失值

缺失值不应仅包含在分类中，而应包含在类别categories中values

当使用分类法时codes，缺失值将始终具有的代码-1

```python
In [230]: s = pd.Series(["a", "b", np.nan, "a"], dtype="category")

# only two categories
In [231]: s
Out[231]: 
0      a
1      b
2    NaN
3      a
dtype: category
Categories (2, object): ['a', 'b']

In [232]: s.cat.codes
Out[232]: 
0    0
1    1
2   -1
3    0
dtype: int8
```

- .isna()
- .fillna()
- .dropna()



# 与R语言中factor的区别

- R中的levels => categories
  - 都是string类型
  - 而Pandas中的categories可以是任意类型
- 开始创建time对象时，不能指定特定labels，必须用s.cat.rename_categories(new_labels)
- R允许NA值在类别中，Pandas不允许NA类别，但允许NA值





# 陷阱/注意问题

## 内存使用

Categorical内存与(categories的数量+数据的长度)成正比

object类型只是数据的长度的固定常数倍

```python
In [237]: s = pd.Series(['foo', 'bar'] * 1000)

# object dtype
In [238]: s.nbytes
Out[238]: 16000

# category dtype
In [239]: s.astype('category').nbytes
Out[239]: 2016
```

如果类别的数量接近数据的长度，那么Categorical使用的内存可能比object类型还要多

```python
In [240]: s = pd.Series(['foo%04d' % i for i in range(2000)])

# object dtype
In [241]: s.nbytes
Out[241]: 16000

# category dtype
In [242]: s.astype('category').nbytes
Out[242]: 20000
```



## Categorical不是Numpy array

Categorical的实现是Python对象，而不是底层NumPy数组dtype

```python
In [243]: try:
   .....:     np.dtype("category")
   .....: except TypeError as e:
   .....:     print("TypeError:", str(e))
   .....: 
TypeError: data type 'category' not understood

In [244]: dtype = pd.Categorical(["a"]).dtype

In [245]: try:
   .....:     np.dtype(dtype)
   .....: except TypeError as e:
   .....:     print("TypeError:", str(e))
   .....: 
TypeError: Cannot interpret 'CategoricalDtype(categories=['a'], ordered=False)' as a data type
```





### 检查ser是否包含分类数据

```python
In [248]: hasattr(pd.Series(['a'], dtype='category'), 'cat')
Out[248]: True

In [249]: hasattr(pd.Series(['a']), 'cat')
Out[249]: False
```



```python
In [250]: s = pd.Series(pd.Categorical([1, 2, 3, 4]))

In [251]: try:
   .....:     np.sum(s)
   .....: except TypeError as e:
   .....:     print("TypeError:", str(e))
   .....: 
TypeError: Categorical cannot perform the operation sum
```



## .apply()

```python
In [252]: df = pd.DataFrame({"a": [1, 2, 3, 4],
   .....:                    "b": ["a", "b", "c", "d"],
   .....:                    "cats": pd.Categorical([1, 2, 3, 2])})
   .....: 

In [253]: df.apply(lambda row: type(row["cats"]), axis=1)
Out[253]: 
0    <class 'int'>
1    <class 'int'>
2    <class 'int'>
3    <class 'int'>
dtype: object

In [254]: df.apply(lambda col: col.dtype, axis=0)
Out[254]: 
a          int64
b         object
cats    category
dtype: object
```



## CategoricalIndex

```python
In [255]: cats = pd.Categorical([1, 2, 3, 4], categories=[4, 2, 3, 1])

In [256]: strings = ["a", "b", "c", "d"]

In [257]: values = [4, 2, 3, 1]

In [258]: df = pd.DataFrame({"strings": strings, "values": values}, index=cats)

In [259]: df.index
Out[259]: CategoricalIndex([1, 2, 3, 4], categories=[4, 2, 3, 1], ordered=False, dtype='category')

# This now sorts by the categories order
In [260]: df.sort_index()
Out[260]: 
  strings  values
4       d       1
2       b       2
3       c       3
1       a       4
```

