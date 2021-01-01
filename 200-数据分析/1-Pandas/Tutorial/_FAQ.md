- [DataFrame memory usage](https://pandas.pydata.org/pandas-docs/stable/user_guide/gotchas.html#dataframe-memory-usage)
- [Using if/truth statements with pandas](https://pandas.pydata.org/pandas-docs/stable/user_guide/gotchas.html#using-if-truth-statements-with-pandas)
- [NaN, Integer NA values and NA type promotions](https://pandas.pydata.org/pandas-docs/stable/user_guide/gotchas.html#nan-integer-na-values-and-na-type-promotions)
- [Differences with NumPy](https://pandas.pydata.org/pandas-docs/stable/user_guide/gotchas.html#differences-with-numpy)
- [Thread-safety](https://pandas.pydata.org/pandas-docs/stable/user_guide/gotchas.html#thread-safety)
- [Byte-ordering issues](https://pandas.pydata.org/pandas-docs/stable/user_guide/gotchas.html#byte-ordering-issues)

---

# DF内存使用情况

- .info()
  - 配置：display.memory_usage 

```python
In [1]: dtypes = ['int64', 'float64', 'datetime64[ns]', 'timedelta64[ns]',
   ...:           'complex128', 'object', 'bool']
   ...: 

In [2]: n = 5000

In [3]: data = {t: np.random.randint(100, size=n).astype(t) for t in dtypes}

In [4]: df = pd.DataFrame(data)

In [5]: df['categorical'] = df['object'].astype('category')

In [6]: df.info()
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 5000 entries, 0 to 4999
Data columns (total 8 columns):
 #   Column           Non-Null Count  Dtype          
---  ------           --------------  -----          
 0   int64            5000 non-null   int64          
 1   float64          5000 non-null   float64        
 2   datetime64[ns]   5000 non-null   datetime64[ns] 
 3   timedelta64[ns]  5000 non-null   timedelta64[ns]
 4   complex128       5000 non-null   complex128     
 5   object           5000 non-null   object         
 6   bool             5000 non-null   bool           
 7   categorical      5000 non-null   category       
dtypes: bool(1), category(1), complex128(1), datetime64[ns](1), float64(1), int64(1), object(1), timedelta64[ns](1)
memory usage: 289.1+ KB
    
    
### memory_usage='deep'
# 启用更准确的内存使用情况报告，说明所包含对象的全部使用情况
In [7]: df.info(memory_usage='deep')
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 5000 entries, 0 to 4999
Data columns (total 8 columns):
 #   Column           Non-Null Count  Dtype          
---  ------           --------------  -----          
 0   int64            5000 non-null   int64          
 1   float64          5000 non-null   float64        
 2   datetime64[ns]   5000 non-null   datetime64[ns] 
 3   timedelta64[ns]  5000 non-null   timedelta64[ns]
 4   complex128       5000 non-null   complex128     
 5   object           5000 non-null   object         
 6   bool             5000 non-null   bool           
 7   categorical      5000 non-null   category       
dtypes: bool(1), category(1), complex128(1), datetime64[ns](1), float64(1), int64(1), object(1), timedelta64[ns](1)
memory usage: 425.6 KB
    
    
    
###
In [8]: df.memory_usage()
Out[8]: 
Index                128
int64              40000
float64            40000
datetime64[ns]     40000
timedelta64[ns]    40000
complex128         80000
object             40000
bool                5000
categorical        10920
dtype: int64

# total memory usage of dataframe
In [9]: df.memory_usage().sum()
Out[9]: 296048
    
    
# 参数index=False
In [10]: df.memory_usage(index=False)
Out[10]: 
int64              40000
float64            40000
datetime64[ns]     40000
timedelta64[ns]    40000
complex128         80000
object             40000
bool                5000
categorical        10920
dtype: int64
```





# if/truth语句

```python
### 有歧义 => 报错
# 因为不是零长度 => True
# 因为有False => False
>>> if pd.Series([False, True, False]):
...     print("I was true")
Traceback
    ...
ValueError: The truth value of an array is ambiguous. Use a.empty, a.any() or a.all().


    
### any()，all()或empty()
# not None
>>> if pd.Series([False, True, False]) is not None:
...     print("I was not None")
I was not None

# .any()
>>> if pd.Series([False, True, False]).any():
...     print("I am any")
I am any



### bool()
In [11]: pd.Series([True]).bool()
Out[11]: True

In [12]: pd.Series([False]).bool()
Out[12]: False

In [13]: pd.DataFrame([[True]]).bool()
Out[13]: True

In [14]: pd.DataFrame([[False]]).bool()
Out[14]: False
    
    
    
### 按位比较
# ==或!=
>>> s = pd.Series(range(5))
>>> s == 4
0    False
1    False
2    False
3    False
4     True
dtype: bool
    
    
### 比较index用in
In [15]: s = pd.Series(range(5), index=list('abcde'))

In [16]: 2 in s  #不是比较value，而是index
Out[16]: False

In [17]: 'b' in s
Out[17]: True
    
    
### 比较元素值用.isin()
# in在Python字典上基于key，而不是value
# Series类似dict => 
In [18]: s.isin([2])
Out[18]: 
a    False
b    False
c     True
d    False
e    False
dtype: bool

In [19]: s.isin([2]).any()
Out[19]: True
```





# NA

## NA的表示

NumPy和Python中缺少对NA的支持 => 应对方法：用特殊标记值，比如NaN

- 另一种方法：Mask array

```python
### pandas整型
'''
Int8Dtype
Int16Dtype
Int32Dtype
Int64Dtype
'''


In [26]: s_int = pd.Series([1, 2, 3, 4, 5], index=list('abcde'),
   ....:                   dtype=pd.Int64Dtype())
   ....: 

In [27]: s_int
Out[27]: 
a    1
b    2
c    3
d    4
e    5
dtype: Int64

In [28]: s_int.dtype
Out[28]: Int64Dtype()

In [29]: s2_int = s_int.reindex(['a', 'b', 'c', 'f', 'u'])

In [30]: s2_int
Out[30]: 
a       1
b       2
c       3
f    <NA>
u    <NA>
dtype: Int64

In [31]: s2_int.dtype
Out[31]: Int64Dtype()
```



- reindex() => 数据类型转换

| Typeclass  | Promotion dtype for storing NAs |
| :--------- | :------------------------------ |
| `floating` | no change                       |
| `object`   | no change                       |
| `integer`  | cast to `float64`               |
| `boolean`  | cast to `object`                |



## 为什么Numpy不能像R一样？

- numpy的数据类型：层级式数据类型

| Typeclass               | Dtypes                                |
| :---------------------- | :------------------------------------ |
| `numpy.floating`        | `float16, float32, float64, float128` |
| `numpy.integer`         | `int8, int16, int32, int64`           |
| `numpy.unsignedinteger` | `uint8, uint16, uint32, uint64`       |
| `numpy.object_`         | `object_`                             |
| `numpy.bool_`           | `bool_`                               |
| `numpy.character`       | `string_, unicode_`                   |

相比之下，R语言只有少数内置数据类型： integer，numeric（浮点数）character，和 boolean





# Pandas与Numpy的区别

var()归一化：

- 对于Series和DataFrame对象，var()通过N-1进行归一化 以产生样本方差的==无偏估计==
- NumPy的 var通过进行归一化以N来衡量样本的方差



pandas和NumPy中均cov()通过N-1进行归一化





# 线程安全性

Pandas不是100％线程安全的，原因与copy()方法有关

如果要DataFrame对线程之间共享的对象进行大量复制 ，建议您在**发生数据复制的线程内部**持有锁。





# 字节顺序问题

有时，需要处理在一台计算机上创建的数据，该计算机的字节顺序与运行Python的字节顺序不同。

此问题的常见症状是类似以下错误

```python
Traceback
    ...
ValueError: Big-endian buffer not supported on little-endian compiler
    
### 解决方法
# 将基础NumPy数组传递给Series或DataFrame之前，先转换为本机系统字节顺序
In [32]: x = np.array(list(range(10)), '>i4')  # big endian
In [33]: newx = x.byteswap().newbyteorder()  # force native byteorder

In [34]: s = pd.Series(newx)
```

