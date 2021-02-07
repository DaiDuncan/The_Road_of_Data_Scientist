IntegerArray目前处于实验阶段。其API或实现可能会更改



NaN是浮点数，这会强制将具有任何缺失值的整数数组变为浮点，一般无关紧要。但是当整数列是一个标识符，将其强制转换为float可能会出现问题。



# 创建

- **pandas.NA**：<NA>

```python
In [1]: arr = pd.array([1, 2, None], dtype=pd.Int64Dtype())

In [2]: arr
Out[2]: 
<IntegerArray>
[1, 2, <NA>]
Length: 3, dtype: Int64
        
        
In [3]: pd.array([1, 2, np.nan], dtype="Int64")
Out[3]: 
<IntegerArray>
[1, 2, <NA>]
Length: 3, dtype: Int64
        
        
### 存储在ser或DF中
In [5]: pd.Series(arr)
Out[5]: 
0       1
1       2
2    <NA>
dtype: Int64
    
# pandas.array()
In [6]: pd.array([1, None])
Out[6]: 
<IntegerArray>
[1, <NA>]
Length: 2, dtype: Int64

In [7]: pd.array([1, 2])
Out[7]: 
<IntegerArray>
[1, 2]
Length: 2, dtype: Int64
        
        
# 建议明确指定dtype
In [10]: pd.array([1, None], dtype="Int64")
Out[10]: 
<IntegerArray>
[1, <NA>]
Length: 2, dtype: Int64

In [11]: pd.Series([1, None], dtype="Int64")
Out[11]: 
0       1
1    <NA>
dtype: Int64
```





# 运算

## 基本运算：算术，比较，索引等

```python
In [12]: s = pd.Series([1, 2, None], dtype="Int64")

# arithmetic
In [13]: s + 1
Out[13]: 
0       2
1       3
2    <NA>
dtype: Int64

# comparison
In [14]: s == 1
Out[14]: 
0     True
1    False
2     <NA>
dtype: boolean

# indexing
In [15]: s.iloc[1:3]
Out[15]: 
1       2
2    <NA>
dtype: Int64

# operate with other dtypes
In [16]: s + s.iloc[1:3].astype('Int8')
Out[16]: 
0    <NA>
1       4
2    <NA>
dtype: Int64

# coerce when needed
In [17]: s + 0.01
Out[17]: 
0    1.01
1    2.01
2     NaN
dtype: float64
```



## dtype可以merged & reshaped & casted & groupby 

```python
### dtypes
In [18]: df = pd.DataFrame({'A': s, 'B': [1, 1, 3], 'C': list('aab')})

In [19]: df
Out[19]: 
      A  B  C
0     1  1  a
1     2  1  a
2  <NA>  3  b

In [20]: df.dtypes
Out[20]: 
A     Int64
B     int64
C    object
dtype: object
    
### 合并concat
In [21]: pd.concat([df[['A']], df[['B', 'C']]], axis=1).dtypes
Out[21]: 
A     Int64
B     int64
C    object
dtype: object

In [22]: df['A'].astype(float)
Out[22]: 
0    1.0
1    2.0
2    NaN
Name: A, dtype: float64
        
        
        
### 分组
In [23]: df.sum()
Out[23]: 
A      3
B      5
C    aab
dtype: object

In [24]: df.groupby('B').A.sum()
Out[24]: 
B
1    3
3    0
Name: A, dtype: Int64
```



# 标量NA值

```python
In [25]: a = pd.array([1, None], dtype="Int64")

In [26]: a[1]
Out[26]: <NA>
```