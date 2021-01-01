- [Hierarchical indexing (MultiIndex)](https://pandas.pydata.org/pandas-docs/stable/user_guide/advanced.html#hierarchical-indexing-multiindex)
- [Advanced indexing with hierarchical index](https://pandas.pydata.org/pandas-docs/stable/user_guide/advanced.html#advanced-indexing-with-hierarchical-index)
- [Sorting a MultiIndex](https://pandas.pydata.org/pandas-docs/stable/user_guide/advanced.html#sorting-a-multiindex)
- [Take methods](https://pandas.pydata.org/pandas-docs/stable/user_guide/advanced.html#take-methods)
- [Index types](https://pandas.pydata.org/pandas-docs/stable/user_guide/advanced.html#index-types)
- [Miscellaneous indexing FAQ](https://pandas.pydata.org/pandas-docs/stable/user_guide/advanced.html#miscellaneous-indexing-faq)

---

# Series 的层次化索引

Series的层次化索引方便许多，只用在构造时传入一个嵌套list即可

- list的第一个嵌套子list为外层（level0）索引
- 第二个嵌套子list为内层（level1）索引

不同level之间的索引可以不对称，比如a有1，2，3三个子索引，而b只有1，3两个子索引

```python
data = Series(np.random.randn(9),index=[list("AAABBBCCC"),list("123123123")]);
print(data)
print(data["A"])
print(data["A":"C"])
print(data[["A","C"]]) #和单层索引一样，可以使用index name、切片、花式索引等

A  1    0.766414
   2    0.103040
   3   -0.081656
B  1    0.290946
   2   -0.289479
   3    1.186381
C  1   -0.472114
   2   -1.061467
   3   -2.309760
dtype: float64
    

1    0.766414
2    0.103040
3   -0.081656
dtype: float64
    

A  1    0.766414
   2    0.103040
   3   -0.081656
B  1    0.290946
   2   -0.289479
   3    1.186381
C  1   -0.472114
   2   -1.061467
   3   -2.309760
dtype: float64
    
    
A  1    0.766414
   2    0.103040
   3   -0.081656
C  1   -0.472114
   2   -1.061467
   3   -2.309760
dtype: float64
    
    
###
print(data[:,"2"])
print(data["A","2"]) 

A    0.103040
B   -0.289479
C   -1.061467
dtype: float64
    
0.10304022968206712
```



# DataFrame的层次化索引

本身就有两个维度的索引，因此就必须使用idx（IndexSlice）进行区分，要选择指定行

- `.loc[idx[:,”2”],:]`
- `.loc(axis=0)[idx[:,”2”]]`



```python
data = DataFrame(np.random.randn(9,2),
index=[list("aaabbbccc"),list("123123123")],columns=list("AB"))

data #层次索引的index 或者 column采用[[],[]]书写，分别代表其不同层次
        A				B
a	1	1.219370	0.167672
    2	0.232690	0.147475
    3	-1.886220	0.024700
b	1	1.051342	-0.810769
    2	0.770830	0.713649
    3	-1.841257	1.770243
c	1	-0.514284	0.610397
    2	-0.804532	1.362790
    3	-0.430893	1.799304
    
    
print(data.index)
print(data.A)
print(data["A"]) #这些和单维度索引都是一样的


MultiIndex(levels=[['a', 'b', 'c'], ['1', '2', '3']],
           labels=[[0, 0, 0, 1, 1, 1, 2, 2, 2], [0, 1, 2, 0, 1, 2, 0, 1, 2]])


a  1    1.219370
   2    0.232690
   3   -1.886220
b  1    1.051342
   2    0.770830
   3   -1.841257
c  1   -0.514284
   2   -0.804532
   3   -0.430893
Name: A, dtype: float64
        
        
a  1    1.219370
   2    0.232690
   3   -1.886220
b  1    1.051342
   2    0.770830
   3   -1.841257
c  1   -0.514284
   2   -0.804532
   3   -0.430893
Name: A, dtype: float64
        
        
### 一般而言，推荐使用xs选取索引:
# 需要指定轴axis和索引层次level，以及被索引的名称（对于单个name，传入字符串，对于多个name，传入元组）
# 最复杂的情况是跨层次选择索引，比如选择level0的a列和level1的1列需要指定level为（0，1），指定选取列为（”a”,”1”）

print(data.loc["a"])
#选取单个索引

print(data.xs("A",axis=1,level=None))
#print(data.xs("A",axis=1,level=0))#对于没有分层的column index，不能出现level值
#选取多层索引之第一层索引

print(data.xs("a",axis=0,level=0)) #选取第一层row索引，指定轴为0
#选取多层索引之第二层索引

print(data.xs("1",axis=0,level=1)) #选取第二层row索引，因为1对应有三个，因此返回了3行
#选取多层索引之多层索引

print(data.xs(("a","1"),axis=0,level=(0,1))) 
#如果一二层，则需要声明level为两者，row索引放在set元组里。

'''me|疑惑
对axis=0的疑惑：此处是行(而默认为跨行：选择列)
'''
         A         B
1  1.21937  0.167672
2  0.23269  0.147475
3 -1.88622  0.024700

a  1    1.219370
   2    0.232690
   3   -1.886220
b  1    1.051342
   2    0.770830
   3   -1.841257
c  1   -0.514284
   2   -0.804532
   3   -0.430893
Name: A, dtype: float64
        
         A         B
1  1.21937  0.167672
2  0.23269  0.147475
3 -1.88622  0.024700

          A         B
a  1.219370  0.167672
b  1.051342 -0.810769
c -0.514284  0.610397

           A         B
a 1  1.21937  0.167672



### 创建一个idx对象，或者使用slice()来辅助选取
# 比如要选取row => .loc[row, :]
# 比如选择a行的1，2行 => syx = IndexSlice["a",["1","2"]] => loc[syx,:]
# 如果你不想调用IndexSlice的话，那么使用slice(None)来代替“：”
# 使用(slice(),slice())进行第一层和第二层的切片,比如syy = (slice("a"),slice(["1","2"]))，然后传入 loc[syy,:] 进行调用


#简单的方式是采用切片slice(None):
print(data.loc["a"])
print(data.loc[(slice("a")),:]) #提倡采用.loc[(slice()),:] 而不是.loc[slice()]
print(data.loc[(slice(None),slice("1")),:]) #如果完全保留，则需要声明slice(None)

#其实，slice等同于IndexSlice：
idx = pd.IndexSlice
print(data.loc[idx[:,"1"],idx[:]])
#这个也可以写成：
print(data.loc(axis=0)[idx[:,"1"]]) #以省去写idx[:]的麻烦
```



# 调整level顺序|swaplevel和sortlevel

比如将level0的一个index换到level1，或者对于level进行排序，可以使用swaplevel和sortlevel进行操作

```python
DataFrame.swaplevel(i=-2, j=-1, axis=0)

#data.swaplevel(0,1,axis=0) #其中level可以为int或者是index.name设置的名称
data.index.names = ["abc","num"]; #names而不是name需要注意
print(data)

data = data.swaplevel("abc","num")
print(data)
                A         B
abc num                    
1   a    1.219370  0.167672
2   a    0.232690  0.147475
3   a   -1.886220  0.024700
1   b    1.051342 -0.810769
2   b    0.770830  0.713649
3   b   -1.841257  1.770243
1   c   -0.514284  0.610397
2   c   -0.804532  1.362790
3   c   -0.430893  1.799304

                A         B
num abc                    
a   1    1.219370  0.167672
    2    0.232690  0.147475
    3   -1.886220  0.024700
b   1    1.051342 -0.810769
    2    0.770830  0.713649
    3   -1.841257  1.770243
c   1   -0.514284  0.610397
    2   -0.804532  1.362790
    3   -0.430893  1.799304
    
    
    
### DataFrame.sortlevel(level=0, axis=0, ascending=True, inplace=False, sort_remaining=True)
# 一般在进行swaplevel的时候也要进行sortlevel，
# pd现在的版本已经抛弃了这个方法，改成了sort_index by level
print(data.sort_index(axis=0,level=1)) #注意看level不同的区别
print(data.sort_index(axis=0,level=0)) #data.sortlevel()
                A         B
num abc                    
1   a    1.219370  0.167672
2   a    0.232690  0.147475
3   a   -1.886220  0.024700
1   b    1.051342 -0.810769
2   b    0.770830  0.713649
3   b   -1.841257  1.770243
1   c   -0.514284  0.610397
2   c   -0.804532  1.362790
3   c   -0.430893  1.799304

                A         B
num abc                    
1   a    1.219370  0.167672
    b    1.051342 -0.810769
    c   -0.514284  0.610397
2   a    0.232690  0.147475
    b    0.770830  0.713649
    c   -0.804532  1.362790
3   a   -1.886220  0.024700
    b   -1.841257  1.770243
    c   -0.430893  1.799304
```





---

跨层次索引

- 比如选择level0的a列和level1的1列需要 指定level为（0，1） 指定选取列为（”a”,”1”）
- `python (data.xs(("a","1"),axis=0,level=(0,1))) `

使用切片来选取==多个层次==的索引：

- 1 创建idx对象`syx = IndexSlice["a",["1","2"]]` => `loc[syx,:]`
- 2 slice(None)来代替":"
  - (slice(),slice())进行第一层和第二层的切片
  - 示例：`syy = (slice("a"),slice(["1","2"]))` => `loc[syy,:]`

调整level的顺序：

- `data = data.swaplevel("abc","num");print(data)`
- DataFrame.sortlevel(level=0, axis=0, ascending=True, inplace=False, sort_remaining=True)
  - pd现在的版本已经抛弃了这个方法，改成了sort_index by level

**备注：多层次索引给二维的DataFrame提供了处理高维数据的可能，我们只添加了少数的语法，就可以操纵多层索引的行列和值，这看起来很棒。==很多通用函数，比如sum、count等，都提供了level参数，传递这个参数可以对整个level进行数据计算==。**

```python
index.get_level_values(0)
df.columns.levels
df[['foo', 'qux']].columns.to_numpy()

new_mi = df[['foo', 'qux']].columns.remove_unused_levels()
new_mi.levels

s.reindex(index[:3])

df.loc[(slice('A1', 'A3'), ...), :]  #禁止：df.loc[(slice('A1', 'A3'), ...)]       

### 交换level
df[:5].swaplevel(0, 1, axis=0)

### level重新排序
df[:5].reorder_levels([1, 0], axis=0)

### index或MultiIndex重新命名
df.rename(columns={0: "col0", 1: "col1"})
df.rename(index={"one": "two", "y": "z"})
df.rename_axis(index=['abc', 'def'])
df.rename_axis(columns="Cols").columns


### MultiIndex排序
s.sort_index(level=0)

dfm.index.is_lexsorted()
```



# take方法⭐

```python
### take方法
# 整数index的列表
In [122]: index = pd.Index(np.random.randint(0, 1000, 10))

In [123]: index
Out[123]: Int64Index([214, 502, 712, 567, 786, 175, 993, 133, 758, 329], dtype='int64')

In [124]: positions = [0, 9, 3]

In [125]: index[positions]
Out[125]: Int64Index([214, 329, 567], dtype='int64')

In [126]: index.take(positions)
Out[126]: Int64Index([214, 329, 567], dtype='int64')
    
    
In [127]: ser = pd.Series(np.random.randn(10))

In [128]: ser.iloc[positions]
Out[128]: 
0   -0.179666
9    1.824375
3    0.392149
dtype: float64

In [129]: ser.take(positions)
Out[129]: 
0   -0.179666
9    1.824375
3    0.392149
dtype: float64
    
    
# 给定的索引应该是一维列表或ndarray：指定行或列的位置
In [130]: frm = pd.DataFrame(np.random.randn(5, 3))

In [131]: frm.take([1, 4, 3])
Out[131]: 
          0         1         2
1 -1.237881  0.106854 -1.276829
4  0.629675 -1.425966  1.857704
3  0.979542 -1.633678  0.615855

In [132]: frm.take([0, 2], axis=1)
Out[132]: 
          0         2
0  0.595974  0.601544
1 -1.237881 -1.276829
2 -0.767101  1.499591
3  0.979542  0.615855
4  0.629675  1.857704


## 注意：不适用于布尔索引，并且可能返回意外结果
In [133]: arr = np.random.randn(10)

In [134]: arr.take([False, False, True, True])
Out[134]: array([-1.1935, -1.1935,  0.6775,  0.6775])

In [135]: arr[[0, 1]]
Out[135]: array([-1.1935,  0.6775])

In [136]: ser = pd.Series(np.random.randn(10))

In [137]: ser.take([False, False, True, True])
Out[137]: 
0    0.233141
0    0.233141
1   -0.223540
1   -0.223540
dtype: float64

In [138]: ser.iloc[[0, 1]]
Out[138]: 
0    0.233141
1   -0.223540
dtype: float64
    
    
### 性能
# take方法处理的输入范围较窄，因此它可以提供比花哨的索引快很多的性能
In [139]: arr = np.random.randn(10000, 5)

In [140]: indexer = np.arange(10000)

In [141]: random.shuffle(indexer)

In [142]: %timeit arr[indexer]
   .....: %timeit arr.take(indexer, axis=0)
   .....: 
153 us +- 499 ns per loop (mean +- std. dev. of 7 runs, 10000 loops each)
53.2 us +- 154 ns per loop (mean +- std. dev. of 7 runs, 10000 loops each)

In [143]: ser = pd.Series(arr[:, 0])

In [144]: %timeit ser.iloc[indexer]
   .....: %timeit ser.take(indexer)
   .....: 
69.2 us +- 303 ns per loop (mean +- std. dev. of 7 runs, 10000 loops each)
60.1 us +- 240 ns per loop (mean +- std. dev. of 7 runs, 10000 loops each)
```





# Index类型⭐

## CategoricalIndex 



## Int64Index和RangeIndex



## Float64Index 



## IntervalIndex

```python
In [194]: df = pd.DataFrame({'A': [1, 2, 3, 4]},
   .....:                   index=pd.IntervalIndex.from_breaks([0, 1, 2, 3, 4]))
   .....: 

In [195]: df
Out[195]: 
        A
(0, 1]  1
(1, 2]  2
(2, 3]  3
(3, 4]  4
 
## .loc
In [196]: df.loc[2]
Out[196]: 
A    2
Name: (1, 2], dtype: int64

In [197]: df.loc[[2, 3]]
Out[197]: 
        A
(1, 2]  2
(2, 3]  3
 
## intervall中包含的标签
In [198]: df.loc[2.5]
Out[198]: 
A    3
Name: (2, 3], dtype: int64

In [199]: df.loc[[2.5, 3.5]]
Out[199]: 
        A
(2, 3]  3
(3, 4]  4
 
## 只适配完全匹配项 => 子集
In [200]: df.loc[pd.Interval(1, 2)]
Out[200]: 
A    2
Name: (1, 2], dtype: int64
```



## cut qcut

均返回Categorical对象

```python
### 
In [204]: c = pd.cut(range(4), bins=2)

In [205]: c
Out[205]: 
[(-0.003, 1.5], (-0.003, 1.5], (1.5, 3.0], (1.5, 3.0]]
Categories (2, interval[float64]): [(-0.003, 1.5] < (1.5, 3.0]]

In [206]: c.categories
Out[206]: 
IntervalIndex([(-0.003, 1.5], (1.5, 3.0]],
              closed='right',
              dtype='interval[float64]')
                
                
### cut:参数bins
In [207]: pd.cut([0, 3, 5, 1], bins=c.categories)
Out[207]: 
[(-0.003, 1.5], (1.5, 3.0], NaN, (-0.003, 1.5]]
Categories (2, interval[float64]): [(-0.003, 1.5] < (1.5, 3.0]]
                                                     
                                                     
```



## pd.interval_range(start=0, end=5)

- pd.interval_range(start=0, periods=5, freq=1.5)
- pd.interval_range(start=pd.Timestamp('2017-01-01'), periods=4, freq='W')
- pd.interval_range(start=pd.Timedelta('0 days'), periods=3, freq='9H')
- 参数closed：指定间隔在哪一侧闭合





# [其他索引常见问题](https://pandas.pydata.org/pandas-docs/stable/user_guide/advanced.html#miscellaneous-indexing-faq)

