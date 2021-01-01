- [Overview](https://pandas.pydata.org/pandas-docs/stable/user_guide/timeseries.html#overview)
- [Timestamps vs. time spans](https://pandas.pydata.org/pandas-docs/stable/user_guide/timeseries.html#timestamps-vs-time-spans)
- [Converting to timestamps](https://pandas.pydata.org/pandas-docs/stable/user_guide/timeseries.html#converting-to-timestamps)
- [Generating ranges of timestamps](https://pandas.pydata.org/pandas-docs/stable/user_guide/timeseries.html#generating-ranges-of-timestamps)
- [Timestamp limitations](https://pandas.pydata.org/pandas-docs/stable/user_guide/timeseries.html#timestamp-limitations)
- [Indexing](https://pandas.pydata.org/pandas-docs/stable/user_guide/timeseries.html#indexing)
- [Time/date components](https://pandas.pydata.org/pandas-docs/stable/user_guide/timeseries.html#time-date-components)
- [DateOffset objects](https://pandas.pydata.org/pandas-docs/stable/user_guide/timeseries.html#dateoffset-objects)
- [Time series-related instance methods](https://pandas.pydata.org/pandas-docs/stable/user_guide/timeseries.html#time-series-related-instance-methods)
- [Resampling](https://pandas.pydata.org/pandas-docs/stable/user_guide/timeseries.html#resampling)
- [Time span representation](https://pandas.pydata.org/pandas-docs/stable/user_guide/timeseries.html#time-span-representation)
- [Converting between representations](https://pandas.pydata.org/pandas-docs/stable/user_guide/timeseries.html#converting-between-representations)
- [Representing out-of-bounds spans](https://pandas.pydata.org/pandas-docs/stable/user_guide/timeseries.html#representing-out-of-bounds-spans)
- [Time zone handling](https://pandas.pydata.org/pandas-docs/stable/user_guide/timeseries.html#time-zone-handling)

---

数据类型：pandas scikits.timeseries

- Numpy：datetime64，timedelta64

```python
In [1]: import datetime

In [2]: dti = pd.to_datetime(
   ...:     ["1/1/2018", np.datetime64("2018-01-01"), datetime.datetime(2018, 1, 1)]
   ...: )
   ...: 

In [3]: dti
Out[3]: DatetimeIndex(['2018-01-01', '2018-01-01', '2018-01-01'], dtype='datetime64[ns]', freq=None)
    
    
### 生成固定频率日期和时间范围的序列
In [4]: dti = pd.date_range("2018-01-01", periods=3, freq="H")  #间隔是1h
    
    
### 使用时区信息处理和转换日期时间
In [6]: dti = dti.tz_localize("UTC")
In [8]: dti.tz_convert("US/Pacific")
    
    
### 将时间序列重采样或转换为特定频率
In [9]: idx = pd.date_range("2018-01-01", periods=5, freq="H")
In [10]: ts = pd.Series(range(len(idx)), index=idx)
    
In [11]: ts
Out[11]: 
2018-01-01 00:00:00    0
2018-01-01 01:00:00    1
2018-01-01 02:00:00    2
2018-01-01 03:00:00    3
2018-01-01 04:00:00    4
Freq: H, dtype: int64

In [12]: ts.resample("2H").mean()
Out[12]: 
2018-01-01 00:00:00    0.5
2018-01-01 02:00:00    2.5
2018-01-01 04:00:00    4.0
Freq: 2H, dtype: float64
        
      
    
### 以绝对或相对时间增量执行日期和时间算术
In [13]: friday = pd.Timestamp("2018-01-05")

In [14]: friday.day_name()
Out[14]: 'Friday'

# Add 1 day
In [15]: saturday = friday + pd.Timedelta("1 day")  #1天的表示方法

In [16]: saturday.day_name()
Out[16]: 'Saturday'

# Add 1 business day (Friday --> Monday)
In [17]: monday = friday + pd.offsets.BDay() #加1个工作日的表示方法

In [18]: monday.day_name()
Out[18]: 'Monday'
```



# 概述

## 4个与时间相关的概念

| Concept                 | Scalar Class | Array Class      | pandas Data Type                         | Primary Creation Method             |
| :---------------------- | :----------- | :--------------- | :--------------------------------------- | :---------------------------------- |
| Date times<br>日期      | `Timestamp`  | `DatetimeIndex`  | `datetime64[ns]` or `datetime64[ns, tz]` | `to_datetime` or `date_range`       |
| Time deltas<br>增量     | `Timedelta`  | `TimedeltaIndex` | `timedelta64[ns]`                        | `to_timedelta` or `timedelta_range` |
| Time spans<br>跨度/间隔 | `Period`     | `PeriodIndex`    | `period[freq]`                           | `Period` or `period_range`          |
| Date offsets<br>偏移    | `DateOffset` | `None`           | `None`                                   | `DateOffset`                        |



```python
In [19]: pd.Series(range(3), index=pd.date_range("2000", freq="D", periods=3))
Out[19]: 
2000-01-01    0
2000-01-02    1
2000-01-03    2
Freq: D, dtype: int64
        
# 直接将时间作为数据
In [20]: pd.Series(pd.date_range("2000", freq="D", periods=3))
    
    
### datetime，timedelta 以及将Period均有相应的数据类型；DateOffset作为object数据存储
In [21]: pd.Series(pd.period_range("1/1/2011", freq="M", periods=3)) #dtype: period[M]

In [23]: pd.Series(pd.date_range("1/1/2011", freq="M", periods=3)) #dtype: datetime64[ns]
    
In [22]: pd.Series([pd.DateOffset(1), pd.DateOffset(2)])  #dtype: object
```





## 空的时间对象|pd.NaT

```python
In [24]: pd.Timestamp(pd.NaT)
Out[24]: NaT

In [25]: pd.Timedelta(pd.NaT)
Out[25]: NaT

In [26]: pd.Period(pd.NaT)
Out[26]: NaT

# Equality acts as np.nan would
In [27]: pd.NaT == pd.NaT
Out[27]: False
```







# Timestamps时间戳 vs. time spans

本质：时间点数据的不同表达方式

```python
In [28]: pd.Timestamp(datetime.datetime(2012, 5, 1))
Out[28]: Timestamp('2012-05-01 00:00:00')

In [29]: pd.Timestamp("2012-05-01")
Out[29]: Timestamp('2012-05-01 00:00:00')

In [30]: pd.Timestamp(2012, 5, 1)
Out[30]: Timestamp('2012-05-01 00:00:00')
    
    
### 参数Period
In [31]: pd.Period("2011-01")
Out[31]: Period('2011-01', 'M')

In [32]: pd.Period("2012-05", freq="D")
Out[32]: Period('2012-05-01', 'D')
```



## pd.Timestamp()和pd.Period()用作索引

自动强制转换DatetimeIndex 和PeriodIndex

```python
# Timestamp
In [33]: dates = [
   ....:     pd.Timestamp("2012-05-01"),
   ....:     pd.Timestamp("2012-05-02"),
   ....:     pd.Timestamp("2012-05-03"),
   ....: ]
   ....: 

In [34]: ts = pd.Series(np.random.randn(3), dates)

In [35]: type(ts.index)
Out[35]: pandas.core.indexes.datetimes.DatetimeIndex

In [36]: ts.index
Out[36]: DatetimeIndex(['2012-05-01', '2012-05-02', '2012-05-03'], dtype='datetime64[ns]', freq=None)

In [37]: ts
Out[37]: 
2012-05-01    0.469112
2012-05-02   -0.282863
2012-05-03   -1.509059
dtype: float64
    
    
# periods
In [38]: periods = [pd.Period("2012-01"), pd.Period("2012-02"), pd.Period("2012-03")]

In [39]: ts = pd.Series(np.random.randn(3), periods)

In [40]: type(ts.index)
Out[40]: pandas.core.indexes.period.PeriodIndex

In [41]: ts.index
Out[41]: PeriodIndex(['2012-01', '2012-02', '2012-03'], dtype='period[M]', freq='M')

In [42]: ts
Out[42]: 
2012-01   -1.135632
2012-02    1.212112
2012-03   -0.173215
Freq: M, dtype: float64
```





# .to_datetime()|转换为timestamp

```python
# Series/列表对象：元素可以是字符串，epochs，或者混合对象
In [43]: pd.to_datetime(pd.Series(["Jul 31, 2009", "2010-01-10", None]))
In [44]: pd.to_datetime(["2005/11/23", "2010.12.31"])
Out[44]: DatetimeIndex(['2005-11-23', '2010-12-31'], dtype='datetime64[ns]', freq=None)
    
    
### 格式：day在前面@欧洲
# 参数dayfirst：只适用于pd.to_datetime()，而不适用于pd.Timestamp()
In [45]: pd.to_datetime(["04-01-2012 10:00"], dayfirst=True)
Out[45]: DatetimeIndex(['2012-01-04 10:00:00'], dtype='datetime64[ns]', freq=None)

In [46]: pd.to_datetime(["14-01-2012", "01-14-2012"], dayfirst=True)
Out[46]: DatetimeIndex(['2012-01-14', '2012-01-14'], dtype='datetime64[ns]', freq=None)
    
    
    
### 直接构造DatetimeIndex
In [49]: pd.DatetimeIndex(["2018-01-01", "2018-01-03", "2018-01-05"])
Out[49]: DatetimeIndex(['2018-01-01', '2018-01-03', '2018-01-05'], dtype='datetime64[ns]', freq=None)
    
# 参数：frep='infer' 根据数据推断频率
In [50]: pd.DatetimeIndex(["2018-01-01", "2018-01-03", "2018-01-05"], freq="infer")
Out[50]: DatetimeIndex(['2018-01-01', '2018-01-03', '2018-01-05'], dtype='datetime64[ns]', freq='2D')
```



## 整数/浮点数epoch|默认是ns

基于基准时间，将四舍五入到最接近的纳秒

```python
### 参数unit：时间单位
In [58]: pd.to_datetime(
   ....:     [1349720105, 1349806505, 1349892905, 1349979305, 1350065705], unit="s"
   ....: )
   ....: 
Out[58]: 
DatetimeIndex(['2012-10-08 18:15:05', '2012-10-09 18:15:05',
               '2012-10-10 18:15:05', '2012-10-11 18:15:05',
               '2012-10-12 18:15:05'],
              dtype='datetime64[ns]', freq=None)

In [59]: pd.to_datetime(
   ....:     [1349720105100, 1349720105200, 1349720105300, 1349720105400, 1349720105500],
   ....:     unit="ms",
   ....: )
   ....: 
Out[59]: 
DatetimeIndex(['2012-10-08 18:15:05.100000', '2012-10-08 18:15:05.200000',
               '2012-10-08 18:15:05.300000', '2012-10-08 18:15:05.400000',
               '2012-10-08 18:15:05.500000'],
              dtype='datetime64[ns]', freq=None)


### 包含时区tz
In [60]: pd.Timestamp(1262347200000000000).tz_localize("US/Pacific")
Out[60]: Timestamp('2010-01-01 12:00:00-0800', tz='US/Pacific')

In [61]: pd.DatetimeIndex([1262347200000000000]).tz_localize("US/Pacific")
Out[61]: DatetimeIndex(['2010-01-01 12:00:00-08:00'], dtype='datetime64[ns, US/Pacific]', freq=None)
```

注意：浮动时期的转换可能导致不准确和意外的结果。 

- Python浮点数的十进制精度约为15位，从浮点数到高精度转换过程中的舍入Timestamp是不可避免的
- 实现精确精度的唯一方法是使用固定宽度类型（例如int64）



## 时间戳 => 整数/浮点数epoch

减去基准时间

```python
In [64]: stamps = pd.date_range("2012-10-08 18:15:05", periods=4, freq="D")
    
In [66]: (stamps - pd.Timestamp("1970-01-01")) // pd.Timedelta("1s")
Out[66]: Int64Index([1349720105, 1349806505, 1349892905, 1349979305], dtype='int64')
```



## 参数origin

创建新的基准点

```python
In [67]: pd.to_datetime([1, 2, 3], unit="D", origin=pd.Timestamp("1960-01-01"))
Out[67]: DatetimeIndex(['1960-01-02', '1960-01-03', '1960-01-04'], dtype='datetime64[ns]', freq=None)
    
### 默认origin='unix'  1970-01-01 00:00:00
In [68]: pd.to_datetime([1, 2, 3], unit="D")
Out[68]: DatetimeIndex(['1970-01-02', '1970-01-03', '1970-01-04'], dtype='datetime64[ns]', freq=None)
```







## 参数format|格式化

- 固定格式
- 提高效率

```python
In [51]: pd.to_datetime("2010/11/12", format="%Y/%m/%d")
Out[51]: Timestamp('2010-11-12 00:00:00')

In [52]: pd.to_datetime("12-11-2010 00:00", format="%d-%m-%Y %H:%M")
Out[52]: Timestamp('2010-11-12 00:00:00')
```



## 从DataFrame的多个列中提取日期

```python
In [53]: df = pd.DataFrame(
   ....:     {"year": [2015, 2016], "month": [2, 3], "day": [4, 5], "hour": [2, 3]}
   ....: )
   ....: 

In [54]: pd.to_datetime(df)
Out[54]: 
0   2015-02-04 02:00:00
1   2016-03-05 03:00:00
dtype: datetime64[ns]
    
    
### 指定列
# 要求：列名称标准化year，month，day
# 可选项：hour，minute，second，millisecond，microsecond，nanosecond
In [55]: pd.to_datetime(df[["year", "month", "day"]])
Out[55]: 
0   2015-02-04
1   2016-03-05
dtype: datetime64[ns]
```







## 无效数据/无法解析

```python
# 参数errors='raise'：报错
In [2]: pd.to_datetime(['2009/07/31', 'asd'], errors='raise')
ValueError: Unknown string format
    
# 参数errors='ignore'：返回原始输入
In [56]: pd.to_datetime(["2009/07/31", "asd"], errors="ignore")
Out[56]: Index(['2009/07/31', 'asd'], dtype='object')
    
# 参数errors='coerce'：显示NaT
In [57]: pd.to_datetime(["2009/07/31", "asd"], errors="coerce")
Out[57]: DatetimeIndex(['2009-07-31', 'NaT'], dtype='datetime64[ns]', freq=None)
```





# .date_range()|生成时间戳

- 参数date_range：日历日
- 参数bdate_range：工作日

```python
In [69]: dates = [
   ....:     datetime.datetime(2012, 5, 1),
   ....:     datetime.datetime(2012, 5, 2),
   ....:     datetime.datetime(2012, 5, 3),
   ....: ]
   ....: 

# Note the frequency information
In [70]: index = pd.DatetimeIndex(dates)

In [71]: index
Out[71]: DatetimeIndex(['2012-05-01', '2012-05-02', '2012-05-03'], dtype='datetime64[ns]', freq=None)

# Automatically converted to DatetimeIndex
In [72]: index = pd.Index(dates)

In [73]: index
Out[73]: DatetimeIndex(['2012-05-01', '2012-05-02', '2012-05-03'], dtype='datetime64[ns]', freq=None)
    
In [74]: start = datetime.datetime(2011, 1, 1)
In [75]: end = datetime.datetime(2012, 1, 1)
In [76]: index = pd.date_range(start, end) #数据长度366  freq='D'
In [78]: index = pd.bdate_range(start, end) #数据长度260   freq='B'
    
```



## 自定义频率范围

- 参数weekmask
- 参数holidays

```python
In [88]: weekmask = "Mon Wed Fri"
In [89]: holidays = [datetime.datetime(2011, 1, 5), datetime.datetime(2011, 3, 14)]

In [90]: pd.bdate_range(start, end, freq="C", weekmask=weekmask, holidays=holidays)
Out[90]: 
DatetimeIndex(['2011-01-03', '2011-01-07', '2011-01-10', '2011-01-12',
               '2011-01-14', '2011-01-17', '2011-01-19', '2011-01-21',
               '2011-01-24', '2011-01-26',
               ...
               '2011-12-09', '2011-12-12', '2011-12-14', '2011-12-16',
               '2011-12-19', '2011-12-21', '2011-12-23', '2011-12-26',
               '2011-12-28', '2011-12-30'],
              dtype='datetime64[ns]', length=154, freq='C')

In [91]: pd.bdate_range(start, end, freq="CBMS", weekmask=weekmask)
Out[91]: 
DatetimeIndex(['2011-01-03', '2011-02-02', '2011-03-02', '2011-04-01',
               '2011-05-02', '2011-06-01', '2011-07-01', '2011-08-01',
               '2011-09-02', '2011-10-03', '2011-11-02', '2011-12-02'],
              dtype='datetime64[ns]', freq='CBMS')
```



## 补充|时间戳的边界限制

```python
In [92]: pd.Timestamp.min
Out[92]: Timestamp('1677-09-21 00:12:43.145225')

In [93]: pd.Timestamp.max
Out[93]: Timestamp('2262-04-11 23:47:16.854775807')
```





# 索引

DatetimeIndex对象具有常规Index 对象的所有基本功能

- shift方法
- 快速访问日期字段year，month
- 正则化函数snap asof

```python
In [94]: rng = pd.date_range(start, end, freq="BM")
In [95]: ts = pd.Series(np.random.randn(len(rng)), index=rng)
In [96]: ts.index
    
In [97]: ts[:5].index
In [98]: ts[::2].index
    
    
```

## 部分字符串索引

```python
In [99]: ts["1/31/2011"]
Out[99]: 0.11920871129693428

In [100]: ts[datetime.datetime(2011, 12, 25):]
Out[100]: 
2011-12-30    0.56702
Freq: BM, dtype: float64
        
In [101]: ts["10/31/2011":"12/31/2011"]
Out[101]: 
2011-10-31    0.271860
2011-11-30   -0.424972
2011-12-30    0.567020
Freq: BM, dtype: float64
        
### 年份
In [102]: ts["2011"]
Out[102]: 
2011-01-31    0.119209
2011-02-28   -1.044236
2011-03-31   -0.861849
2011-04-29   -2.104569
2011-05-31   -0.494929
2011-06-30    1.071804
2011-07-29    0.721555
2011-08-31   -0.706771
2011-09-30   -1.039575
2011-10-31    0.271860
2011-11-30   -0.424972
2011-12-30    0.567020
Freq: BM, dtype: float64

### 月份
In [103]: ts["2011-6"]
Out[103]: 
2011-06-30    1.071804
Freq: BM, dtype: float64
```

部分字符串选择是标签切片的一种形式，因此将包括端点

注意：从pandas 1.2.0开始，不再建议使用frame[dtstring] => 对行进行索引还是选择列的模棱两可

将在以后的版本中删除。仍支持与.loc（例如frame.loc[dtstring]）等效的功能

- ```python
  In [106]: dft.loc["2013"]
  In [107]: dft["2013-1":"2013-2"]
  In [108]: dft["2013-1":"2013-2-28"]  #包括最后一天的所有时间
  Out[108]: 
                              A
  2013-01-01 00:00:00  0.276232
  2013-01-01 00:01:00 -1.087401
  2013-01-01 00:02:00 -0.673690
  2013-01-01 00:03:00  0.113648
  2013-01-01 00:04:00 -1.478427
  ...                       ...
  2013-02-28 23:55:00  0.850929
  2013-02-28 23:56:00  0.976712
  2013-02-28 23:57:00 -2.693884
  2013-02-28 23:58:00 -1.575535
  2013-02-28 23:59:00 -1.573517
  
  [84960 rows x 1 columns]
  
  
  
  In [109]: dft["2013-1":"2013-2-28 00:00:00"]  #指定了确切的停止时间
  Out[109]: 
                              A
  2013-01-01 00:00:00  0.276232
  2013-01-01 00:01:00 -1.087401
  2013-01-01 00:02:00 -0.673690
  2013-01-01 00:03:00  0.113648
  2013-01-01 00:04:00 -1.478427
  ...                       ...
  2013-02-27 23:56:00  1.197749
  2013-02-27 23:57:00  0.720521
  2013-02-27 23:58:00 -0.072718
  2013-02-27 23:59:00 -0.681192
  2013-02-28 00:00:00 -0.557501
  
  [83521 rows x 1 columns]
  ```

- MultiIndex

```python
In [111]: dft2 = pd.DataFrame(
   .....:     np.random.randn(20, 1),
   .....:     columns=["A"],
   .....:     index=pd.MultiIndex.from_product(
   .....:         [pd.date_range("20130101", periods=10, freq="12H"), ["a", "b"]]
   .....:     ),
   .....: )
   .....: 

In [112]: dft2
Out[112]: 
                              A
2013-01-01 00:00:00 a -0.298694
                    b  0.823553
2013-01-01 12:00:00 a  0.943285
                    b -1.479399
2013-01-02 00:00:00 a -1.643342
...                         ...
2013-01-04 12:00:00 b  0.069036
2013-01-05 00:00:00 a  0.122297
                    b  1.422060
2013-01-05 12:00:00 a  0.370079
                    b  1.016331

[20 rows x 1 columns]


In [113]: dft2.loc["2013-01-05"]
In [114]: idx = pd.IndexSlice

In [115]: dft2 = dft2.swaplevel(0, 1).sort_index()

In [116]: dft2.loc[idx[:, "2013-01-05"], :] #idx[:, "2013-01-05"]表示MultiIndex
Out[116]: 
                              A
a 2013-01-05 00:00:00  0.122297
  2013-01-05 12:00:00  0.370079
b 2013-01-05 00:00:00  1.422060
  2013-01-05 12:00:00  1.016331
```

- UTC偏移量

```python
In [117]: df = pd.DataFrame([0], index=pd.DatetimeIndex(["2019-01-01"], tz="US/Pacific"))
    
In [119]: df["2019-01-01 12:00:00+04:00":"2019-01-01 13:00:00+04:00"]
```



## 切片与精确匹配

```python
### 索引的分辨率
In [120]: series_minute = pd.Series(
   .....:     [1, 2, 3],
   .....:     pd.DatetimeIndex(
   .....:         ["2011-12-31 23:59:00", "2012-01-01 00:00:00", "2012-01-01 00:02:00"]
   .....:     ),
   .....: )
   .....: 

In [121]: series_minute.index.resolution
Out[121]: 'minute'
    
In [122]: series_minute["2011-12-31 23"]
Out[122]: 
2011-12-31 23:59:00    1
dtype: int64
    
In [123]: series_minute["2011-12-31 23:59"]  #注意：DF用frame.loc[] 避免与选择列发生歧义
Out[123]: 1
In [124]: series_minute["2011-12-31 23:59:00"]
Out[124]: 1
    
### 索引分辨率为秒
In [126]: series_second.index.resolution
Out[126]: 'second'
```



## 精确索引

```python
In [134]: dft[datetime.datetime(2013, 1, 1): datetime.datetime(2013, 2, 28)]
    
    
In [135]: dft[
   .....:     datetime.datetime(2013, 1, 1, 10, 12, 0): datetime.datetime(
   .....:         2013, 2, 28, 10, 12, 0
   .....:     )
   .....: ]
```





## 截断与花式索引Truncating & fancy indexing

```python
In [136]: rng2 = pd.date_range("2011-01-01", "2012-01-01", freq="W")

In [137]: ts2 = pd.Series(np.random.randn(len(rng2)), index=rng2)

### 截断索引
In [138]: ts2.truncate(before="2011-11", after="2011-12") #在这期间
Out[138]: 
2011-11-06    0.437823
2011-11-13   -0.293083
2011-11-20   -0.059881
2011-11-27    1.252450
Freq: W-SUN, dtype: float64

In [139]: ts2["2011-11":"2011-12"]
Out[139]: 
2011-11-06    0.437823
2011-11-13   -0.293083
2011-11-20   -0.059881
2011-11-27    1.252450
2011-12-04    0.046611
2011-12-11    0.059478
2011-12-18   -0.286539
2011-12-25    0.841669
Freq: W-SUN, dtype: float64
        
        
### 花式索引
In [140]: ts2[[0, 2, 6]].index
Out[140]: DatetimeIndex(['2011-01-02', '2011-01-16', '2011-02-13'], dtype='datetime64[ns]', freq=None)
```



# 汇总|Time/date成分表

| Property         | Description                                                  |
| :--------------- | :----------------------------------------------------------- |
| year             | The year of the datetime                                     |
| month            | The month of the datetime                                    |
| day              | The days of the datetime                                     |
| hour             | The hour of the datetime                                     |
| minute           | The minutes of the datetime                                  |
| second           | The seconds of the datetime                                  |
| microsecond      | The microseconds of the datetime                             |
| nanosecond       | The nanoseconds of the datetime                              |
| ==date==         | Returns datetime.date (does not contain timezone information) |
| ==time==         | Returns datetime.time (does not contain timezone information) |
| timetz           | Returns datetime.time as local time with timezone information |
| dayofyear        | The ordinal day of year                                      |
| day_of_year      | The ordinal day of year                                      |
| weekofyear       | The week ordinal of the year                                 |
| week             | The week ordinal of the year                                 |
| dayofweek        | The number of the day of the week with Monday=0, Sunday=6    |
| day_of_week      | The number of the day of the week with Monday=0, Sunday=6    |
| ==weekday==      | The number of the day of the week with Monday=0, Sunday=6    |
| quarter          | Quarter of the date: Jan-Mar = 1, Apr-Jun = 2, etc.          |
| days_in_month    | The number of days in the month of the datetime              |
| is_month_start   | Logical indicating if first day of month (defined by frequency) |
| is_month_end     | Logical indicating if last day of month (defined by frequency) |
| is_quarter_start | Logical indicating if first day of quarter (defined by frequency) |
| is_quarter_end   | Logical indicating if last day of quarter (defined by frequency) |
| is_year_start    | Logical indicating if first day of year (defined by frequency) |
| is_year_end      | Logical indicating if last day of year (defined by frequency) |
| is_leap_year     | Logical indicating if the date belongs to a leap year        |

datetime.dt. 访问这些属性

```python
In [141]: idx = pd.date_range(start="2019-12-29", freq="D", periods=4)

In [142]: idx.isocalendar()
Out[142]: 
            year  week  day
2019-12-29  2019    52    7
2019-12-30  2020     1    1
2019-12-31  2020     1    2
2020-01-01  2020     1    3

In [143]: idx.to_series().dt.isocalendar()
Out[143]: 
            year  week  day
2019-12-29  2019    52    7
2019-12-30  2020     1    1
2019-12-31  2020     1    2
2020-01-01  2020     1    3
```





# DateOffset对象

与Timedelta相似，代表时间的持续时间

- Timedelta一天将始终增加datetimes24小时
- 而由于夏时制，无论一天代表23、24或25小时，DateOffset一天都将增加datetimes至第二天的同一时间

所有的DateOffset子类都有(Hour, Minute, Second, Milli, Micro, Nano) 

DateOffset与dateutil.relativedelta（relativedelta文档）类似，将日期时间偏移指定的相应日历持续时间。算术运算符（+）或apply方法可用于执行移位

```python
# This particular day contains a day light savings time transition
In [144]: ts = pd.Timestamp("2016-10-30 00:00:00", tz="Europe/Helsinki")

# Respects absolute time
In [145]: ts + pd.Timedelta(days=1)
Out[145]: Timestamp('2016-10-30 23:00:00+0200', tz='Europe/Helsinki') #

# Respects calendar time
In [146]: ts + pd.DateOffset(days=1)
Out[146]: Timestamp('2016-10-31 00:00:00+0200', tz='Europe/Helsinki')

In [147]: friday = pd.Timestamp("2018-01-05")

In [148]: friday.day_name()
Out[148]: 'Friday'

# Add 2 business days (Friday --> Tuesday)
In [149]: two_business_days = 2 * pd.offsets.BDay()

In [150]: two_business_days.apply(friday)
Out[150]: Timestamp('2018-01-09 00:00:00')

In [151]: friday + two_business_days
Out[151]: Timestamp('2018-01-09 00:00:00')

In [152]: (friday + two_business_days).day_name()
Out[152]: 'Tuesday'
```



## 汇总|参数freq

| Date Offset                                                  | Frequency String  | Description                                                  |
| :----------------------------------------------------------- | :---------------- | :----------------------------------------------------------- |
| [`DateOffset`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.tseries.offsets.DateOffset.html#pandas.tseries.offsets.DateOffset) | None              | Generic offset class, defaults to absolute 24 hours          |
| [`BDay`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.tseries.offsets.BDay.html#pandas.tseries.offsets.BDay) or [`BusinessDay`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.tseries.offsets.BusinessDay.html#pandas.tseries.offsets.BusinessDay) | `'B'`             | business day (weekday)                                       |
| [`CDay`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.tseries.offsets.CDay.html#pandas.tseries.offsets.CDay) or [`CustomBusinessDay`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.tseries.offsets.CustomBusinessDay.html#pandas.tseries.offsets.CustomBusinessDay) | `'C'`             | custom business day                                          |
| [`Week`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.tseries.offsets.Week.html#pandas.tseries.offsets.Week) | `'W'`             | one week, optionally anchored on a day of the week           |
| [`WeekOfMonth`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.tseries.offsets.WeekOfMonth.html#pandas.tseries.offsets.WeekOfMonth) | `'WOM'`           | the x-th day of the y-th week of each month 每月第y周的第x天 |
| [`LastWeekOfMonth`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.tseries.offsets.LastWeekOfMonth.html#pandas.tseries.offsets.LastWeekOfMonth) | `'LWOM'`          | the x-th day of the last week of each month 每月最后一周的第x天 |
| [`MonthEnd`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.tseries.offsets.MonthEnd.html#pandas.tseries.offsets.MonthEnd) | `'M'`             | calendar month end 日历月末                                  |
| [`MonthBegin`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.tseries.offsets.MonthBegin.html#pandas.tseries.offsets.MonthBegin) | `'MS'`            | calendar month begin 日历月开始                              |
| [`BMonthEnd`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.tseries.offsets.BMonthEnd.html#pandas.tseries.offsets.BMonthEnd) or [`BusinessMonthEnd`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.tseries.offsets.BusinessMonthEnd.html#pandas.tseries.offsets.BusinessMonthEnd) | `'BM'`            | business month end 营业月底                                  |
| [`BMonthBegin`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.tseries.offsets.BMonthBegin.html#pandas.tseries.offsets.BMonthBegin) or [`BusinessMonthBegin`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.tseries.offsets.BusinessMonthBegin.html#pandas.tseries.offsets.BusinessMonthBegin) | `'BMS'`           | business month begin 营业月开始                              |
| [`CBMonthEnd`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.tseries.offsets.CBMonthEnd.html#pandas.tseries.offsets.CBMonthEnd) or [`CustomBusinessMonthEnd`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.tseries.offsets.CustomBusinessMonthEnd.html#pandas.tseries.offsets.CustomBusinessMonthEnd) | `'CBM'`           | custom business month end                                    |
| [`CBMonthBegin`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.tseries.offsets.CBMonthBegin.html#pandas.tseries.offsets.CBMonthBegin) or [`CustomBusinessMonthBegin`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.tseries.offsets.CustomBusinessMonthBegin.html#pandas.tseries.offsets.CustomBusinessMonthBegin) | `'CBMS'`          | custom business month begin                                  |
| [`SemiMonthEnd`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.tseries.offsets.SemiMonthEnd.html#pandas.tseries.offsets.SemiMonthEnd) | `'SM'`            | 15th (or other day_of_month) and calendar month end 第15个（或其他day_of_month）和日历月末 |
| [`SemiMonthBegin`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.tseries.offsets.SemiMonthBegin.html#pandas.tseries.offsets.SemiMonthBegin) | `'SMS'`           | 15th (or other day_of_month) and calendar month begin        |
| [`QuarterEnd`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.tseries.offsets.QuarterEnd.html#pandas.tseries.offsets.QuarterEnd) | `'Q'`             | calendar quarter end 日历季度末                              |
| [`QuarterBegin`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.tseries.offsets.QuarterBegin.html#pandas.tseries.offsets.QuarterBegin) | `'QS'`            | calendar quarter begin                                       |
| [`BQuarterEnd`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.tseries.offsets.BQuarterEnd.html#pandas.tseries.offsets.BQuarterEnd) | `'BQ`             | business quarter end                                         |
| [`BQuarterBegin`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.tseries.offsets.BQuarterBegin.html#pandas.tseries.offsets.BQuarterBegin) | `'BQS'`           | business quarter begin 商业季度开始                          |
| [`FY5253Quarter`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.tseries.offsets.FY5253Quarter.html#pandas.tseries.offsets.FY5253Quarter) | `'REQ'`           | retail (aka 52-53 week) quarter                              |
| [`YearEnd`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.tseries.offsets.YearEnd.html#pandas.tseries.offsets.YearEnd) | `'A'`             | calendar year end 日历年末                                   |
| [`YearBegin`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.tseries.offsets.YearBegin.html#pandas.tseries.offsets.YearBegin) | `'AS'` or `'BYS'` | calendar year begin                                          |
| [`BYearEnd`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.tseries.offsets.BYearEnd.html#pandas.tseries.offsets.BYearEnd) | `'BA'`            | business year end                                            |
| [`BYearBegin`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.tseries.offsets.BYearBegin.html#pandas.tseries.offsets.BYearBegin) | `'BAS'`           | business year begin                                          |
| [`FY5253`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.tseries.offsets.FY5253.html#pandas.tseries.offsets.FY5253) | `'RE'`            | retail (aka 52-53 week) year                                 |
| [`Easter`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.tseries.offsets.Easter.html#pandas.tseries.offsets.Easter) | None              | Easter holiday 复活节假期                                    |
| [`BusinessHour`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.tseries.offsets.BusinessHour.html#pandas.tseries.offsets.BusinessHour) | `'BH'`            | business hour                                                |
| [`CustomBusinessHour`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.tseries.offsets.CustomBusinessHour.html#pandas.tseries.offsets.CustomBusinessHour) | `'CBH'`           | custom business hour                                         |
| [`Day`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.tseries.offsets.Day.html#pandas.tseries.offsets.Day) | `'D'`             | one absolute day                                             |
| [`Hour`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.tseries.offsets.Hour.html#pandas.tseries.offsets.Hour) | `'H'`             | one hour                                                     |
| [`Minute`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.tseries.offsets.Minute.html#pandas.tseries.offsets.Minute) | `'T'` or `'min'`  | one minute                                                   |
| [`Second`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.tseries.offsets.Second.html#pandas.tseries.offsets.Second) | `'S'`             | one second                                                   |
| [`Milli`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.tseries.offsets.Milli.html#pandas.tseries.offsets.Milli) | `'L'` or `'ms'`   | one millisecond                                              |
| [`Micro`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.tseries.offsets.Micro.html#pandas.tseries.offsets.Micro) | `'U'` or `'us'`   | one microsecond                                              |
| [`Nano`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.tseries.offsets.Nano.html#pandas.tseries.offsets.Nano) | `'N'`             | one nanosecond                                               |

- rollforward() 移动日期向前
- rollback()  移动日期向后

```python
In [153]: ts = pd.Timestamp("2018-01-06 00:00:00")

In [154]: ts.day_name()
Out[154]: 'Saturday'

# BusinessHour's valid offset dates are Monday through Friday
In [155]: offset = pd.offsets.BusinessHour(start="09:00")

# Bring the date to the closest offset date (Monday)
# 将ts往前推移到一个最近的工作时间
In [156]: offset.rollforward(ts)
Out[156]: Timestamp('2018-01-08 09:00:00')

# Date is brought to the closest offset date first and then the hour is added
In [157]: ts + offset
Out[157]: Timestamp('2018-01-08 10:00:00')
    
    
    
In [158]: ts = pd.Timestamp("2014-01-01 09:00")
In [159]: day = pd.offsets.Day()

In [160]: day.apply(ts)
Out[160]: Timestamp('2014-01-02 09:00:00')
### 时间重置回午夜
In [161]: day.apply(ts).normalize()
Out[161]: Timestamp('2014-01-02 00:00:00')

In [162]: ts = pd.Timestamp("2014-01-01 22:00")

In [163]: hour = pd.offsets.Hour()
In [164]: hour.apply(ts)
Out[164]: Timestamp('2014-01-01 23:00:00')

In [165]: hour.apply(ts).normalize()
Out[165]: Timestamp('2014-01-01 00:00:00')

In [166]: hour.apply(pd.Timestamp("2014-01-01 23:30")).normalize()
Out[166]: Timestamp('2014-01-02 00:00:00')
```



## 参数：偏移量

```python
In [167]: d = datetime.datetime(2008, 8, 18, 9, 0)
In [168]: d
Out[168]: datetime.datetime(2008, 8, 18, 9, 0)

In [169]: d + pd.offsets.Week()
Out[169]: Timestamp('2008-08-25 09:00:00')

In [170]: d + pd.offsets.Week(weekday=4)
Out[170]: Timestamp('2008-08-22 09:00:00')

In [171]: (d + pd.offsets.Week(weekday=4)).weekday()
Out[171]: 4

In [172]: d - pd.offsets.Week()
Out[172]: Timestamp('2008-08-11 09:00:00')
    
    
### 参数normalize对+ - 均有效
In [173]: d + pd.offsets.Week(normalize=True)
Out[173]: Timestamp('2008-08-25 00:00:00')

In [174]: d - pd.offsets.Week(normalize=True)
Out[174]: Timestamp('2008-08-11 00:00:00')
    
    
### .YearEnd()
In [175]: d + pd.offsets.YearEnd()
Out[175]: Timestamp('2008-12-31 09:00:00')

In [176]: d + pd.offsets.YearEnd(month=6)
Out[176]: Timestamp('2009-06-30 09:00:00')
```



## 偏移量与Ser/DatetimeIndex

```python
### DatetimeIndex
In [177]: rng = pd.date_range("2012-01-01", "2012-01-03")
### Ser
In [178]: s = pd.Series(rng)

In [179]: rng
Out[179]: DatetimeIndex(['2012-01-01', '2012-01-02', '2012-01-03'], dtype='datetime64[ns]', freq='D')

In [180]: rng + pd.DateOffset(months=2)
Out[180]: DatetimeIndex(['2012-03-01', '2012-03-02', '2012-03-03'], dtype='datetime64[ns]', freq=None)

In [181]: s + pd.DateOffset(months=2)
Out[181]: 
0   2012-03-01
1   2012-03-02
2   2012-03-03
dtype: datetime64[ns]

In [182]: s - pd.DateOffset(months=2)
Out[182]: 
0   2011-11-01
1   2011-11-02
2   2011-11-03
dtype: datetime64[ns]
```

- 偏移类：是Timedelta（Day，Hour， Minute，Second，Micro，Milli，Nano）

```python
In [183]: s - pd.offsets.Day(2)
Out[183]: 
0   2011-12-30
1   2011-12-31
2   2012-01-01
dtype: datetime64[ns]
    
    
In [184]: td = s - pd.Series(pd.date_range("2011-12-29", "2011-12-31"))
    
In [185]: td
Out[185]: 
0   3 days
1   3 days
2   3 days
dtype: timedelta64[ns]

In [186]: td + pd.offsets.Minute(15)
Out[186]: 
0   3 days 00:15:00
1   3 days 00:15:00
2   3 days 00:15:00
dtype: timedelta64[ns]
```

注意，某些偏移量（例如`BQuarterEnd`）没有向量化的实现。它们仍然可以使用，但计算速度可能会大大降低，并且会显示`PerformanceWarning`

```python
In [187]: rng + pd.offsets.BQuarterEnd()
Out[187]: DatetimeIndex(['2012-03-30', '2012-03-30', '2012-03-30'], dtype='datetime64[ns]', freq=None)
```



## 自定义工作日

CDay或CustomBusinessDay类提供的参数 BusinessDay

```python
In [188]: weekmask_egypt = "Sun Mon Tue Wed Thu"

# They also observe International Workers' Day so let's
# add that for a couple of years
In [189]: holidays = [
   .....:     "2012-05-01",
   .....:     datetime.datetime(2013, 5, 1),
   .....:     np.datetime64("2014-05-01"),
   .....: ]
   .....: 

In [190]: bday_egypt = pd.offsets.CustomBusinessDay(
   .....:     holidays=holidays,
   .....:     weekmask=weekmask_egypt,
   .....: )
   .....: 

In [191]: dt = datetime.datetime(2013, 4, 30)

In [192]: dt + 2 * bday_egypt
Out[192]: Timestamp('2013-05-05 00:00:00')
    
    
### 映射到工作日
In [193]: dts = pd.date_range(dt, periods=5, freq=bday_egypt)

In [194]: pd.Series(dts.weekday, dts).map(pd.Series("Mon Tue Wed Thu Fri Sat Sun".split()))
Out[194]: 
2013-04-30    Tue
2013-05-02    Thu
2013-05-05    Sun
2013-05-06    Mon
2013-05-07    Tue
Freq: C, dtype: object
```

- 假期日历

```python
In [195]: from pandas.tseries.holiday import USFederalHolidayCalendar
    
In [196]: bday_us = pd.offsets.CustomBusinessDay(calendar=USFederalHolidayCalendar())

# Friday before MLK Day
In [197]: dt = datetime.datetime(2014, 1, 17)

# Tuesday after MLK Day (Monday is skipped because it's a holiday)
In [198]: dt + bday_us
Out[198]: Timestamp('2014-01-21 00:00:00')
```



## 营业时间

BusinessHour类提供在营业时间表示BusinessDay，允许使用特定的开始和结束时间

默认情况下，BusinessHour使用9:00-17:00作为工作时间

```python
In [203]: bh = pd.offsets.BusinessHour()

In [204]: bh
Out[204]: <BusinessHour: BH=09:00-17:00>

# 2014-08-01 is Friday
In [205]: pd.Timestamp("2014-08-01 10:00").weekday()
Out[205]: 4

In [206]: pd.Timestamp("2014-08-01 10:00") + bh
Out[206]: Timestamp('2014-08-01 11:00:00')

# Below example is the same as: pd.Timestamp('2014-08-01 09:00') + bh
In [207]: pd.Timestamp("2014-08-01 08:00") + bh
Out[207]: Timestamp('2014-08-01 10:00:00')

# If the results is on the end time, move to the next business day
In [208]: pd.Timestamp("2014-08-01 16:00") + bh
Out[208]: Timestamp('2014-08-04 09:00:00')

# Remainings are added to the next day
In [209]: pd.Timestamp("2014-08-01 16:30") + bh
Out[209]: Timestamp('2014-08-04 09:30:00')

# Adding 2 business hours
In [210]: pd.Timestamp("2014-08-01 10:00") + pd.offsets.BusinessHour(2)
Out[210]: Timestamp('2014-08-01 12:00:00')

# Subtracting 3 business hours
In [211]: pd.Timestamp("2014-08-01 10:00") + pd.offsets.BusinessHour(-3)
Out[211]: Timestamp('2014-07-31 15:00:00')
```

在工作时间外应用BusinessHour.rollforward和rollback会导致下一个工作时间开始或前一天的结束

```python
# This adjusts a Timestamp to business hour edge
In [223]: pd.offsets.BusinessHour().rollback(pd.Timestamp("2014-08-02 15:00"))
Out[223]: Timestamp('2014-08-01 17:00:00')

In [224]: pd.offsets.BusinessHour().rollforward(pd.Timestamp("2014-08-02 15:00"))
Out[224]: Timestamp('2014-08-04 09:00:00')

# It is the same as BusinessHour().apply(pd.Timestamp('2014-08-01 17:00')).
# And it is the same as BusinessHour().apply(pd.Timestamp('2014-08-04 09:00'))
In [225]: pd.offsets.BusinessHour().apply(pd.Timestamp("2014-08-02 15:00"))
Out[225]: Timestamp('2014-08-04 10:00:00')

# BusinessDay results (for reference)
In [226]: pd.offsets.BusinessHour().rollforward(pd.Timestamp("2014-08-02"))
Out[226]: Timestamp('2014-08-04 09:00:00')

# It is the same as BusinessDay().apply(pd.Timestamp('2014-08-01'))
# The result is the same as rollworward because BusinessDay never overlap.
In [227]: pd.offsets.BusinessHour().apply(pd.Timestamp("2014-08-02"))
Out[227]: Timestamp('2014-08-04 10:00:00')
```

BusinessHour将周六和周日视为假期。要使用任意假期，可以使用CustomBusinessHour偏移量



## 自定义营业时间

- CustomBusinessHour

```python
In [228]: from pandas.tseries.holiday import USFederalHolidayCalendar

In [229]: bhour_us = pd.offsets.CustomBusinessHour(calendar=USFederalHolidayCalendar())

# Friday before MLK Day
In [230]: dt = datetime.datetime(2014, 1, 17, 15)

In [231]: dt + bhour_us
Out[231]: Timestamp('2014-01-17 16:00:00')

# Tuesday after MLK Day (Monday is skipped because it's a holiday)
In [232]: dt + bhour_us * 2
Out[232]: Timestamp('2014-01-21 09:00:00')
```

- BusinessHour
- CustomBusinessDay

```python
In [233]: bhour_mon = pd.offsets.CustomBusinessHour(start="10:00", weekmask="Tue Wed Thu Fri")

# Monday is skipped because it's a holiday, business hour starts from 10:00
In [234]: dt + bhour_mon * 2
Out[234]: Timestamp('2014-01-21 10:00:00')
```



## 汇总|偏移量缩写

| Alias    | Description                                      |
| :------- | :----------------------------------------------- |
| B        | business day frequency                           |
| C        | custom business day frequency                    |
| D        | calendar day frequency                           |
| W        | weekly frequency                                 |
| M        | month end frequency                              |
| SM       | semi-month end frequency (15th and end of month) |
| BM       | business month end frequency                     |
| CBM      | custom business month end frequency              |
| MS       | month start frequency                            |
| SMS      | semi-month start frequency (1st and 15th)        |
| BMS      | business month start frequency                   |
| CBMS     | custom business month start frequency            |
| Q        | quarter end frequency                            |
| BQ       | business quarter end frequency                   |
| QS       | quarter start frequency                          |
| BQS      | business quarter start frequency                 |
| A, Y     | year end frequency                               |
| BA, BY   | business year end frequency                      |
| AS, YS   | year start frequency                             |
| BAS, BYS | business year start frequency                    |
| BH       | business hour frequency                          |
| H        | hourly frequency                                 |
| T, min   | minutely frequency                               |
| S        | secondly frequency                               |
| L, ms    | milliseconds                                     |
| U, us    | microseconds                                     |
| N        | nanoseconds                                      |

```python
In [235]: pd.date_range(start, periods=5, freq="B")
In [236]: pd.date_range(start, periods=5, freq=pd.offsets.BDay())

In [237]: pd.date_range(start, periods=10, freq="2h20min")
In [238]: pd.date_range(start, periods=10, freq="1D10U")
```



## 汇总|锚定的偏移量缩写

| Alias       | Description                                             |
| :---------- | :------------------------------------------------------ |
| W-SUN       | weekly frequency (Sundays). Same as ‘W’                 |
| W-MON       | weekly frequency (Mondays)                              |
| W-TUE       | weekly frequency (Tuesdays)                             |
| W-WED       | weekly frequency (Wednesdays)                           |
| W-THU       | weekly frequency (Thursdays)                            |
| W-FRI       | weekly frequency (Fridays)                              |
| W-SAT       | weekly frequency (Saturdays)                            |
| (B)Q(S)-DEC | quarterly frequency, year ends in December. Same as ‘Q’ |
| (B)Q(S)-JAN | quarterly frequency, year ends in January               |
| (B)Q(S)-FEB | quarterly frequency, year ends in February              |
| (B)Q(S)-MAR | quarterly frequency, year ends in March                 |
| (B)Q(S)-APR | quarterly frequency, year ends in April                 |
| (B)Q(S)-MAY | quarterly frequency, year ends in May                   |
| (B)Q(S)-JUN | quarterly frequency, year ends in June                  |
| (B)Q(S)-JUL | quarterly frequency, year ends in July                  |
| (B)Q(S)-AUG | quarterly frequency, year ends in August                |
| (B)Q(S)-SEP | quarterly frequency, year ends in September             |
| (B)Q(S)-OCT | quarterly frequency, year ends in October               |
| (B)Q(S)-NOV | quarterly frequency, year ends in November              |
| (B)A(S)-DEC | annual frequency, anchored end of December. Same as ‘A’ |
| (B)A(S)-JAN | annual frequency, anchored end of January               |
| (B)A(S)-FEB | annual frequency, anchored end of February              |
| (B)A(S)-MAR | annual frequency, anchored end of March                 |
| (B)A(S)-APR | annual frequency, anchored end of April                 |
| (B)A(S)-MAY | annual frequency, anchored end of May                   |
| (B)A(S)-JUN | annual frequency, anchored end of June                  |
| (B)A(S)-JUL | annual frequency, anchored end of July                  |
| (B)A(S)-AUG | annual frequency, anchored end of August                |
| (B)A(S)-SEP | annual frequency, anchored end of September             |
| (B)A(S)-OCT | annual frequency, anchored end of October               |
| (B)A(S)-NOV | annual frequency, anchored end of November              |

这些可以被用作参数date_range，bdate_range，构造函数DatetimeIndex，，以及在Pandas各种其它时间序列相关的功能。

```python
In [239]: pd.Timestamp("2014-01-02") + pd.offsets.MonthBegin(n=1)
Out[239]: Timestamp('2014-02-01 00:00:00')
### 如果给定的日期是一个锚点日期(月初，月末)
# 相加没影响，相减有影响
In [245]: pd.Timestamp("2014-01-01") + pd.offsets.MonthBegin(n=1)
Out[245]: Timestamp('2014-02-01 00:00:00')

In [240]: pd.Timestamp("2014-01-02") + pd.offsets.MonthEnd(n=1)
Out[240]: Timestamp('2014-01-31 00:00:00')
In [246]: pd.Timestamp("2014-01-31") + pd.offsets.MonthEnd(n=1)
Out[246]: Timestamp('2014-02-28 00:00:00')
    
In [241]: pd.Timestamp("2014-01-02") - pd.offsets.MonthBegin(n=1)
Out[241]: Timestamp('2014-01-01 00:00:00')
In [247]: pd.Timestamp("2014-01-01") - pd.offsets.MonthBegin(n=1)
Out[247]: Timestamp('2013-12-01 00:00:00')

In [242]: pd.Timestamp("2014-01-02") - pd.offsets.MonthEnd(n=1)
Out[242]: Timestamp('2013-12-31 00:00:00')
In [248]: pd.Timestamp("2014-01-31") - pd.offsets.MonthEnd(n=1)
Out[248]: Timestamp('2013-12-31 00:00:00')

In [243]: pd.Timestamp("2014-01-02") + pd.offsets.MonthBegin(n=4)
Out[243]: Timestamp('2014-05-01 00:00:00')
In [249]: pd.Timestamp("2014-01-01") + pd.offsets.MonthBegin(n=4)
Out[249]: Timestamp('2014-05-01 00:00:00')

In [244]: pd.Timestamp("2014-01-02") - pd.offsets.MonthBegin(n=4)
Out[244]: Timestamp('2013-10-01 00:00:00')
In [250]: pd.Timestamp("2014-01-31") - pd.offsets.MonthBegin(n=4)
Out[250]: Timestamp('2013-10-01 00:00:00')
    
    
    
### n=0时
In [251]: pd.Timestamp("2014-01-02") + pd.offsets.MonthBegin(n=0)
Out[251]: Timestamp('2014-02-01 00:00:00')

In [252]: pd.Timestamp("2014-01-02") + pd.offsets.MonthEnd(n=0)
Out[252]: Timestamp('2014-01-31 00:00:00')
# 给定的日期是锚点
In [253]: pd.Timestamp("2014-01-01") + pd.offsets.MonthBegin(n=0)
Out[253]: Timestamp('2014-01-01 00:00:00')

In [254]: pd.Timestamp("2014-01-31") + pd.offsets.MonthEnd(n=0)
Out[254]: Timestamp('2014-01-31 00:00:00')
```





## 假日

AbstractHolidayCalendar类

| Rule                   | Description                                          |
| :--------------------- | :--------------------------------------------------- |
| nearest_workday        | move Saturday to Friday and Sunday to Monday         |
| sunday_to_monday       | move Sunday to following Monday                      |
| next_monday_or_tuesday | move Saturday to Monday and Sunday/Monday to Tuesday |
| previous_friday        | move Saturday and Sunday to previous Friday”         |
| next_monday            | move Saturday and Sunday to following Monday         |

```python
In [255]: from pandas.tseries.holiday import (
   .....:     Holiday,
   .....:     USMemorialDay,
   .....:     AbstractHolidayCalendar,
   .....:     nearest_workday,
   .....:     MO,
   .....: )
   .....: 

In [256]: class ExampleCalendar(AbstractHolidayCalendar):
   .....:     rules = [
   .....:         USMemorialDay,
   .....:         Holiday("July 4th", month=7, day=4, observance=nearest_workday),
   .....:         Holiday(
   .....:             "Columbus Day",
   .....:             month=10,
   .....:             day=1,
   .....:             offset=pd.DateOffset(weekday=MO(2)),
   .....:         ),
   .....:     ]
   .....: 

In [257]: cal = ExampleCalendar()

In [258]: cal.holidays(datetime.datetime(2012, 1, 1), datetime.datetime(2012, 12, 31))
Out[258]: DatetimeIndex(['2012-05-28', '2012-07-04', '2012-10-08'], dtype='datetime64[ns]', freq=None)
```

注意：weekday = MO（2）等于2 * Week（weekday = 2）

```python
In [259]: pd.date_range(
   .....:     start="7/1/2012", end="7/10/2012", freq=pd.offsets.CDay(calendar=cal)
   .....: ).to_pydatetime()
   .....: 
Out[259]: 
array([datetime.datetime(2012, 7, 2, 0, 0),
       datetime.datetime(2012, 7, 3, 0, 0),
       datetime.datetime(2012, 7, 5, 0, 0),
       datetime.datetime(2012, 7, 6, 0, 0),
       datetime.datetime(2012, 7, 9, 0, 0),
       datetime.datetime(2012, 7, 10, 0, 0)], dtype=object)

In [260]: offset = pd.offsets.CustomBusinessDay(calendar=cal)

In [261]: datetime.datetime(2012, 5, 25) + offset
Out[261]: Timestamp('2012-05-29 00:00:00')

In [262]: datetime.datetime(2012, 7, 3) + offset
Out[262]: Timestamp('2012-07-05 00:00:00')

In [263]: datetime.datetime(2012, 7, 3) + 2 * offset
Out[263]: Timestamp('2012-07-06 00:00:00')

In [264]: datetime.datetime(2012, 7, 6) + offset
Out[264]: Timestamp('2012-07-09 00:00:00')
    
    
    
### AbstractHolidayCalendar的可用范围
In [265]: AbstractHolidayCalendar.start_date
Out[265]: Timestamp('1970-01-01 00:00:00')

In [266]: AbstractHolidayCalendar.end_date
Out[266]: Timestamp('2200-12-31 00:00:00')
    
    
### 属性值覆盖
In [267]: AbstractHolidayCalendar.start_date = datetime.datetime(2012, 1, 1)

In [268]: AbstractHolidayCalendar.end_date = datetime.datetime(2012, 12, 31)

In [269]: cal.holidays()
Out[269]: DatetimeIndex(['2012-05-28', '2012-07-04', '2012-10-08'], dtype='datetime64[ns]', freq=None)
```



```python
In [270]: from pandas.tseries.holiday import get_calendar, HolidayCalendarFactory, USLaborDay

In [271]: cal = get_calendar("ExampleCalendar")

In [272]: cal.rules
Out[272]: 
[Holiday: Memorial Day (month=5, day=31, offset=<DateOffset: weekday=MO(-1)>),
 Holiday: July 4th (month=7, day=4, observance=<function nearest_workday at 0x7fa919c66f70>),
 Holiday: Columbus Day (month=10, day=1, offset=<DateOffset: weekday=MO(+2)>)]

In [273]: new_cal = HolidayCalendarFactory("NewExampleCalendar", cal, USLaborDay)

In [274]: new_cal.rules
Out[274]: 
[Holiday: Labor Day (month=9, day=1, offset=<DateOffset: weekday=MO(+1)>),
 Holiday: Memorial Day (month=5, day=31, offset=<DateOffset: weekday=MO(-1)>),
 Holiday: July 4th (month=7, day=4, observance=<function nearest_workday at 0x7fa919c66f70>),
 Holiday: Columbus Day (month=10, day=1, offset=<DateOffset: weekday=MO(+2)>)]
```



# 与Time series相关的方法

## Shifting / lagging

```python
In [275]: ts = pd.Series(range(len(rng)), index=rng)
In [276]: ts = ts[:5]

### .shift()
In [277]: ts.shift(1)
Out[277]: 
2012-01-01    NaN
2012-01-02    0.0
2012-01-03    1.0
Freq: D, dtype: float64
        
# 参数freq：接受DateOffset类或其他类似timedelta对象，或者偏移的缩写
In [278]: ts.shift(5, freq="D")
In [279]: ts.shift(5, freq=pd.offsets.BDay())
In [280]: ts.shift(5, freq="BM")
```



## .asfreq()|改变频率

```python
In [281]: dr = pd.date_range("1/1/2010", periods=3, freq=3 * pd.offsets.BDay())
In [282]: ts = pd.Series(np.random.randn(3), index=dr)
Out[283]: 
2010-01-01    1.494522
2010-01-06   -0.778425
2010-01-11   -0.253355
Freq: 3B, dtype: float64
        
        
In [284]: ts.asfreq(pd.offsets.BDay())
Out[284]: 
2010-01-01    1.494522
2010-01-04         NaN
2010-01-05         NaN
2010-01-06   -0.778425
2010-01-07         NaN
2010-01-08         NaN
2010-01-11   -0.253355
Freq: B, dtype: float64
        
### 填补缺失值
# 另外的方法：.fillna()
In [285]: ts.asfreq(pd.offsets.BDay(), method="pad")
```



## 转换为python日期时间

datetime.datetime模块中的DatetimeIndex可以使用to_pydatetime方法



# .resample()

在金融应用程序中非常普遍

resample()方法可以直接从DataFrameGroupBy对象中使用

```python
In [286]: rng = pd.date_range("1/1/2012", periods=100, freq="S")

In [287]: ts = pd.Series(np.random.randint(0, 500, len(rng)), index=rng)
In [288]: ts.resample("5Min").sum()
Out[288]: 
2012-01-01    25103
Freq: 5T, dtype: int64

### groupby对象：sum，mean，std，sem， max，min，median，first，last，ohlc
In [289]: ts.resample("5Min").mean()
Out[289]: 
2012-01-01    251.03
Freq: 5T, dtype: float64

In [290]: ts.resample("5Min").ohlc()
Out[290]: 
            open  high  low  close
2012-01-01   308   460    9    205

In [291]: ts.resample("5Min").max()
Out[291]: 
2012-01-01    460
Freq: 5T, dtype: int64


# 参数closed=‘left’：端点开 闭
In [292]: ts.resample("5Min", closed="right").mean()
In [293]: ts.resample("5Min", closed="left").mean()

# 参数label=‘left’：指定结果是用时间间隔的开始还是结束标记
In [294]: ts.resample("5Min").mean()  # by default label='left'
Out[294]: 
2012-01-01    251.03
Freq: 5T, dtype: float64

In [295]: ts.resample("5Min", label="left").mean()
Out[295]: 
2012-01-01    251.03
Freq: 5T, dtype: float64
```

注意：对于参数closed和label，除了‘M’, ‘A’, ‘Q’, ‘BM’, ‘BA’, ‘BQ’, and ‘W’ 默认是 ‘right’，其他均默认为'left'

- 可能导致以后的值将被拉回到先前的时间

```python
In [296]: s = pd.date_range("2000-01-01", "2000-01-05").to_series()

In [297]: s.iloc[2] = pd.NaT

In [298]: s.dt.day_name()
Out[298]: 
2000-01-01     Saturday
2000-01-02       Sunday
2000-01-03          NaN
2000-01-04      Tuesday
2000-01-05    Wednesday
Freq: D, dtype: object

# default: label='left', closed='left'
In [299]: s.resample("B").last().dt.day_name()
Out[299]: 
1999-12-31       Sunday
2000-01-03          NaN
2000-01-04      Tuesday
2000-01-05    Wednesday
Freq: B, dtype: object
```

注意，星期日的值如何拉回到上一个星期五。要获得将星期日的值推到星期一的行为，改用：

```python
In [300]: s.resample("B", label="right", closed="right").last().dt.day_name()
Out[300]: 
2000-01-03       Sunday
2000-01-04      Tuesday
2000-01-05    Wednesday
Freq: B, dtype: object
```

参数axis：可以设置为0或1

参数kind：设置为'timestamp'或'period'以将结果索引与时间戳记和时间跨度表示法相互转换

参数convention：对周期数据进行重采样时将其设置为“开始”或“结束”，它指定了低频周期如何转换为高频周期。



## Upsampling

```python
# from secondly to every 250 milliseconds
In [301]: ts[:2].resample("250L").asfreq()
Out[301]: 
2012-01-01 00:00:00.000    308.0
2012-01-01 00:00:00.250      NaN
2012-01-01 00:00:00.500      NaN
2012-01-01 00:00:00.750      NaN
2012-01-01 00:00:01.000    204.0
Freq: 250L, dtype: float64

In [302]: ts[:2].resample("250L").ffill()
Out[302]: 
2012-01-01 00:00:00.000    308
2012-01-01 00:00:00.250    308
2012-01-01 00:00:00.500    308
2012-01-01 00:00:00.750    308
2012-01-01 00:00:01.000    204
Freq: 250L, dtype: int64

In [303]: ts[:2].resample("250L").ffill(limit=2)  #limit插值数量的限制
Out[303]: 
2012-01-01 00:00:00.000    308.0
2012-01-01 00:00:00.250    308.0
2012-01-01 00:00:00.500    308.0
2012-01-01 00:00:00.750      NaN
2012-01-01 00:00:01.000    204.0
Freq: 250L, dtype: float64
```



## Sparse resampling

```python
In [304]: rng = pd.date_range("2014-1-1", periods=100, freq="D") + pd.Timedelta("1s")

In [305]: ts = pd.Series(range(100), index=rng)
    
### 想对整个系列重新采样
In [306]: ts.resample("3T").sum()
    
### 只对部分组重新采样
In [307]: from functools import partial

In [308]: from pandas.tseries.frequencies import to_offset

In [309]: def round(t, freq):
   .....:     freq = to_offset(freq)
   .....:     return pd.Timestamp((t.value // freq.delta.value) * freq.delta.value)
   .....: 

In [310]: ts.groupby(partial(round, freq="3T")).sum()
Out[310]: 
2014-01-01     0
2014-01-02     1
2014-01-03     2
2014-01-04     3
2014-01-05     4
              ..
2014-04-06    95
2014-04-07    96
2014-04-08    97
2014-04-09    98
2014-04-10    99
Length: 100, dtype: int64
```



# 聚合|resampler对象

```python
In [311]: df = pd.DataFrame(
   .....:     np.random.randn(1000, 3),
   .....:     index=pd.date_range("1/1/2012", freq="S", periods=1000),
   .....:     columns=["A", "B", "C"],
   .....: )
   .....: 

In [312]: r = df.resample("3T")

In [313]: r.mean()
Out[313]: 
                            A         B         C
2012-01-01 00:00:00 -0.033823 -0.121514 -0.081447
2012-01-01 00:03:00  0.056909  0.146731 -0.024320
2012-01-01 00:06:00 -0.058837  0.047046 -0.052021
2012-01-01 00:09:00  0.063123 -0.026158 -0.066533
2012-01-01 00:12:00  0.186340 -0.003144  0.074752
2012-01-01 00:15:00 -0.085954 -0.016287 -0.050046
        
### 选择一列或多列
In [314]: r["A"].mean()
In [315]: r[["A", "B"]].mean()
    
### 聚合函数
In [316]: r["A"].agg([np.sum, np.mean, np.std])
In [317]: r.agg([np.sum, np.mean])
    
In [318]: r.agg({"A": np.sum, "B": lambda x: np.std(x, ddof=1)})
In [319]: r.agg({"A": "sum", "B": "std"})
In [320]: r.agg({"A": ["sum", "std"], "B": ["mean", "std"]})
    
    
### 指定datetime列
In [321]: df = pd.DataFrame(
   .....:     {"date": pd.date_range("2015-01-01", freq="W", periods=5), "a": np.arange(5)},
   .....:     index=pd.MultiIndex.from_arrays(
   .....:         [[1, 2, 3, 4, 5], pd.date_range("2015-01-01", freq="W", periods=5)],
   .....:         names=["v", "d"],
   .....:     ),
   .....: )
   .....: 

In [322]: df
Out[322]: 
                   date  a
v d                       
1 2015-01-04 2015-01-04  0
2 2015-01-11 2015-01-11  1
3 2015-01-18 2015-01-18  2
4 2015-01-25 2015-01-25  3
5 2015-02-01 2015-02-01  4

In [323]: df.resample("M", on="date").sum()
Out[323]: 
            a
date         
2015-01-31  6
2015-02-28  4

# level指定datetime列
In [324]: df.resample("M", level="d").sum()
Out[324]: 
            a
d            
2015-01-31  6
2015-02-28  4
```



## 遍历Resampler对象

```python
In [325]: small = pd.Series(
   .....:     range(6),
   .....:     index=pd.to_datetime(
   .....:         [
   .....:             "2017-01-01T00:00:00",
   .....:             "2017-01-01T00:30:00",
   .....:             "2017-01-01T00:31:00",
   .....:             "2017-01-01T01:00:00",
   .....:             "2017-01-01T03:00:00",
   .....:             "2017-01-01T03:05:00",
   .....:         ]
   .....:     ),
   .....: )
   .....: 

In [326]: resampled = small.resample("H")
In [327]: for name, group in resampled:
   .....:     print("Group: ", name)
   .....:     print("-" * 27)
   .....:     print(group, end="\n\n")
   .....: 
Group:  2017-01-01 00:00:00
---------------------------
2017-01-01 00:00:00    0
2017-01-01 00:30:00    1
2017-01-01 00:31:00    2
dtype: int64

Group:  2017-01-01 01:00:00
---------------------------
2017-01-01 01:00:00    3
dtype: int64

Group:  2017-01-01 02:00:00
---------------------------
Series([], dtype: int64)

Group:  2017-01-01 03:00:00
---------------------------
2017-01-01 03:00:00    4
2017-01-01 03:05:00    5
dtype: int64
```



## 使用origin或offset，设置bins的起点

```python
In [328]: start, end = "2000-10-01 23:30:00", "2000-10-02 00:30:00"
In [329]: middle = "2000-10-02 00:00:00"
In [330]: rng = pd.date_range(start, end, freq="7min")

In [331]: ts = pd.Series(np.arange(len(rng)) * 3, index=rng)
In [332]: ts
Out[332]: 
2000-10-01 23:30:00     0
2000-10-01 23:37:00     3
2000-10-01 23:44:00     6
2000-10-01 23:51:00     9
2000-10-01 23:58:00    12
2000-10-02 00:05:00    15
2000-10-02 00:12:00    18
2000-10-02 00:19:00    21
2000-10-02 00:26:00    24
Freq: 7T, dtype: int64


### origin='start_day' => '2000-10-02 00:00:00'
In [333]: ts.resample("17min", origin="start_day").sum()
Out[333]: 
2000-10-01 23:14:00     0
2000-10-01 23:31:00     9
2000-10-01 23:48:00    21
2000-10-02 00:05:00    54
2000-10-02 00:22:00    24
Freq: 17T, dtype: int64

In [334]: ts[middle:end].resample("17min", origin="start_day").sum()
Out[334]: 
2000-10-02 00:00:00    33
2000-10-02 00:17:00    45
Freq: 17T, dtype: int64

### origin='epoch' => '2000-10-02 00:00:00'
In [335]: ts.resample("17min", origin="epoch").sum()
Out[335]: 
2000-10-01 23:18:00     0
2000-10-01 23:35:00    18
2000-10-01 23:52:00    27
2000-10-02 00:09:00    39
2000-10-02 00:26:00    24
Freq: 17T, dtype: int64


In [336]: ts[middle:end].resample("17min", origin="epoch").sum()
Out[336]: 
2000-10-01 23:52:00    15
2000-10-02 00:09:00    39
2000-10-02 00:26:00    24
Freq: 17T, dtype: int64


### origin自定义
In [337]: ts.resample("17min", origin="2001-01-01").sum()
In [338]: ts[middle:end].resample("17min", origin=pd.Timestamp("2001-01-01")).sum()
    
    
### origin与offset
In [339]: ts.resample("17min", origin="start").sum()
Out[339]: 
2000-10-01 23:30:00     9
2000-10-01 23:47:00    21
2000-10-02 00:04:00    54
2000-10-02 00:21:00    24
Freq: 17T, dtype: int64

In [340]: ts.resample("17min", offset="23h30min").sum()
Out[340]: 
2000-10-01 23:30:00     9
2000-10-01 23:47:00    21
2000-10-02 00:04:00    54
2000-10-02 00:21:00    24
Freq: 17T, dtype: int64
```





# .period_range()|时间间隔

Pandas中的Period对象 => period_range

- PeriodIndex

freq代表的跨度Period，所以不能像“ -3D”那样为负数

```python
In [341]: pd.Period("2012", freq="A-DEC")
Out[341]: Period('2012', 'A-DEC')

In [342]: pd.Period("2012-1-1", freq="D")
Out[342]: Period('2012-01-01', 'D')

In [343]: pd.Period("2012-1-1 19:00", freq="H")
Out[343]: Period('2012-01-01 19:00', 'H')

In [344]: pd.Period("2012-1-1 19:00", freq="5H")
Out[344]: Period('2012-01-01 19:00', '5H')

In [345]: p = pd.Period("2012", freq="A-DEC")

In [346]: p + 1
Out[346]: Period('2013', 'A-DEC')

In [347]: p - 3
Out[347]: Period('2009', 'A-DEC')

In [348]: p = pd.Period("2012-01", freq="2M")

In [349]: p + 2
Out[349]: Period('2012-05', '2M')

In [350]: p - 1
Out[350]: Period('2011-11', '2M')

### 报错：Input has different freq=3M from Period(freq=2M)
In [351]: p == pd.Period("2012-01", freq="3M")


### Period频率是D，H，T，S，L，U，N，offsets和timedelta，可以相加(相同频率才能相加，否则报错)
In [352]: p = pd.Period("2014-07-01 09:00", freq="H")

In [353]: p + pd.offsets.Hour(2)
Out[353]: Period('2014-07-01 11:00', 'H')

In [354]: p + datetime.timedelta(minutes=120)
Out[354]: Period('2014-07-01 11:00', 'H')

In [355]: p + np.timedelta64(7200, "s")
Out[355]: Period('2014-07-01 11:00', 'H')

### 报错：freq=H
In [1]: p + pd.offsets.Minute(5)


In [356]: p = pd.Period("2014-07", freq="M")
In [357]: p + pd.offsets.MonthEnd(3)
Out[357]: Period('2014-10', 'M')

### 报错：freq=M
In [1]: p + pd.offsets.MonthBegin(3)


### period的差
In [358]: pd.Period("2012", freq="A-DEC") - pd.Period("2002", freq="A-DEC")
Out[358]: <10 * YearEnds: month=12>
```



## PeriodIndex和period_range

```python
In [359]: prng = pd.period_range("1/1/2011", "1/1/2012", freq="M")

In [360]: prng
Out[360]: 
PeriodIndex(['2011-01', '2011-02', '2011-03', '2011-04', '2011-05', '2011-06',
             '2011-07', '2011-08', '2011-09', '2011-10', '2011-11', '2011-12',
             '2012-01'],
            dtype='period[M]', freq='M')
            
In [361]: pd.PeriodIndex(["2011-1", "2011-2", "2011-3"], freq="M")
Out[361]: PeriodIndex(['2011-01', '2011-02', '2011-03'], dtype='period[M]', freq='M')

In [362]: pd.period_range(start="2014-01", freq="3M", periods=4)
Out[362]: PeriodIndex(['2014-01', '2014-04', '2014-07', '2014-10'], dtype='period[3M]', freq='3M')

### start, end
In [363]: pd.period_range(
   .....:     start=pd.Period("2017Q1", freq="Q"), end=pd.Period("2017Q2", freq="Q"), freq="M"
   .....: )
   .....: 
Out[363]: PeriodIndex(['2017-03', '2017-04', '2017-05', '2017-06'], dtype='period[M]', freq='M')


### PeriodIndex类似DatetimeIndex：索引Pandas对象
In [364]: ps = pd.Series(np.random.randn(len(prng)), prng)
# PeriodIndex支持相同频率的加法和减法Period
In [366]: idx = pd.period_range("2014-07-01 09:00", periods=5, freq="H")

In [367]: idx
Out[367]: 
PeriodIndex(['2014-07-01 09:00', '2014-07-01 10:00', '2014-07-01 11:00',
             '2014-07-01 12:00', '2014-07-01 13:00'],
            dtype='period[H]', freq='H')

In [368]: idx + pd.offsets.Hour(2)
Out[368]: 
PeriodIndex(['2014-07-01 11:00', '2014-07-01 12:00', '2014-07-01 13:00',
             '2014-07-01 14:00', '2014-07-01 15:00'],
            dtype='period[H]', freq='H')

In [369]: idx = pd.period_range("2014-07", periods=5, freq="M")

In [370]: idx
Out[370]: PeriodIndex(['2014-07', '2014-08', '2014-09', '2014-10', '2014-11'], dtype='period[M]', freq='M')

In [371]: idx + pd.offsets.MonthEnd(3)
Out[371]: PeriodIndex(['2014-10', '2014-11', '2014-12', '2015-01', '2015-02'], dtype='period[M]', freq='M')
```



## period.dtype

```python
In [372]: pi = pd.period_range("2016-01-01", periods=3, freq="M")

In [373]: pi
Out[373]: PeriodIndex(['2016-01', '2016-02', '2016-03'], dtype='period[M]', freq='M')

In [374]: pi.dtype
Out[374]: period[M]


### .astype()
# change monthly freq to daily freq
In [375]: pi.astype("period[D]")
Out[375]: PeriodIndex(['2016-01-31', '2016-02-29', '2016-03-31'], dtype='period[D]', freq='D')

# convert to DatetimeIndex
In [376]: pi.astype("datetime64[ns]")
Out[376]: DatetimeIndex(['2016-01-01', '2016-02-01', '2016-03-01'], dtype='datetime64[ns]', freq='MS')

# convert to PeriodIndex
In [377]: dti = pd.date_range("2011-01-01", freq="M", periods=3)
In [378]: dti
Out[378]: DatetimeIndex(['2011-01-31', '2011-02-28', '2011-03-31'], dtype='datetime64[ns]', freq='M')

In [379]: dti.astype("period[M]")
Out[379]: PeriodIndex(['2011-01', '2011-02', '2011-03'], dtype='period[M]', freq='M')
```



## PeriodIndex部分字符串索引

支持使用非单调索引进行部分字符串切片

```python
In [380]: ps["2011-01"]
Out[380]: -2.9169013294054507

In [381]: ps[datetime.datetime(2011, 12, 25):]
Out[381]: 
2011-12    2.261385
2012-01   -0.329583
Freq: M, dtype: float64

In [382]: ps["10/31/2011":"12/31/2011"]
Out[382]: 
2011-10    0.056780
2011-11    0.197035
2011-12    2.261385
Freq: M, dtype: float64
        
### 频率低于PeriodIndex
In [383]: ps["2011"]
In [384]: dfp = pd.DataFrame(
   .....:     np.random.randn(600, 1),
   .....:     columns=["A"],
   .....:     index=pd.period_range("2013-01-01 9:00", periods=600, freq="T"),
   .....: )
   .....: 

In [386]: dfp.loc["2013-01-01 10H"]
    
    
### 与DatetimeIndex一样，端点将包含在结果中    
In [387]: dfp["2013-01-01 10H":"2013-01-01 11H"]
Out[387]: 
                         A
2013-01-01 10:00 -0.308975
2013-01-01 10:01  0.542520
2013-01-01 10:02  1.061068
2013-01-01 10:03  0.754005
2013-01-01 10:04  0.352933
...                    ...
2013-01-01 11:55 -0.590204
2013-01-01 11:56  1.539990
2013-01-01 11:57 -1.224826
2013-01-01 11:58  0.578798
2013-01-01 11:59 -0.685496

[120 rows x 1 columns]
```



## 改变频率与PeriodIndex重采样

```
In [388]: p = pd.Period("2011", freq="A-DEC")

In [389]: p
Out[389]: Period('2011', 'A-DEC')

### .asfreq()
In [390]: p.asfreq("M", how="start")
Out[390]: Period('2011-01', 'M')

In [391]: p.asfreq("M", how="end")
Out[391]: Period('2011-12', 'M')



In [394]: p = pd.Period("2011-12", freq="M")

In [395]: p.asfreq("A-NOV")
Out[395]: Period('2012', 'A-NOV')
```

具有固定频率的周期转换对于处理经济学，商业和其他领域共有的各种季度数据特别有用

- Q-DEC 定义常规日历季度  `In [396]: p = pd.Period("2012Q1", freq="Q-DEC")`
- Q-MAR 定义三月结束的会计年度  `In [399]: p = pd.Period("2011Q4", freq="Q-MAR")`



# 转换时间的表示

- to_period
- to_timestamp

```python
In [402]: rng = pd.date_range("1/1/2012", periods=5, freq="M")

In [403]: ts = pd.Series(np.random.randn(len(rng)), index=rng)

In [404]: ts
Out[404]: 
2012-01-31    1.931253
2012-02-29   -0.184594
2012-03-31    0.249656
2012-04-30   -0.978151
2012-05-31   -0.873389
Freq: M, dtype: float64
        
        
In [405]: ps = ts.to_period()  #注意freq="M"
In [406]: ps  
Out[406]: 
2012-01    1.931253
2012-02   -0.184594
2012-03    0.249656
2012-04   -0.978151
2012-05   -0.873389
Freq: M, dtype: float64
        
In [407]: ps.to_timestamp()
Out[407]: 
2012-01-01    1.931253
2012-02-01   -0.184594
2012-03-01    0.249656
2012-04-01   -0.978151
2012-05-01   -0.873389
Freq: MS, dtype: float64
```

“ s”和“ e”可用于返回时间段开始或结束时的时间戳

```python
In [408]: ps.to_timestamp("D", how="s")
Out[408]: 
2012-01-01    1.931253
2012-02-01   -0.184594
2012-03-01    0.249656
2012-04-01   -0.978151
2012-05-01   -0.873389
Freq: MS, dtype: float64
        
        
In [409]: prng = pd.period_range("1990Q1", "2000Q4", freq="Q-NOV")

In [410]: ts = pd.Series(np.random.randn(len(prng)), prng)
In [411]: ts.index = (prng.asfreq("M", "e") + 1).asfreq("H", "s") + 9
    
In [412]: ts.head()
Out[412]: 
1990-03-01 09:00   -0.109291
1990-06-01 09:00   -0.637235
1990-09-01 09:00   -1.735925
1990-12-01 09:00    2.096946
1991-03-01 09:00   -1.039926
Freq: H, dtype: float64
```



# 表示跨界的范围

```python
In [413]: span = pd.period_range("1215-01-01", "1381-01-01", freq="D")

In [415]: s = pd.Series([20121231, 20141130, 99991231])
In [416]: s
Out[416]: 
0    20121231
1    20141130
2    99991231
dtype: int64

In [417]: def conv(x):
   .....:     return pd.Period(year=x // 10000, month=x // 100 % 100, day=x % 100, freq="D")
   
In [418]: s.apply(conv)
Out[418]: 
0    2012-12-31
1    2014-11-30
2    9999-12-31
dtype: period[D]

In [419]: s.apply(conv)[2]
Out[419]: Period('9999-12-31', 'D')


### 转换为PeriodIndex
In [420]: span = pd.PeriodIndex(s.apply(conv))

In [421]: span
Out[421]: PeriodIndex(['2012-12-31', '2014-11-30', '9999-12-31'], dtype='period[D]', freq='D')
```





# 时区

pandas使用pytz和dateutil库或datetime.timezone 标准库中的对象

```python
In [422]: rng = pd.date_range("3/6/2012 00:00", periods=15, freq="D")
### 默认pandas对象不了解时区
In [423]: rng.tz is None  
Out[423]: True
```

使用tz_localize方法或tz在关键字参数 date_range()，Timestamp或DatetimeIndex

Olson时区字符串将pytz默认返回时区对象。要返回dateutil时区对象，请dateutil/在字符串之前追加

- pytz：from pytz import common_timezones, all_timezones
- dateutil：使用操作系统时区

```python
In [424]: import dateutil

# pytz
In [425]: rng_pytz = pd.date_range("3/6/2012 00:00", periods=3, freq="D", tz="Europe/London")

In [426]: rng_pytz.tz
Out[426]: <DstTzInfo 'Europe/London' LMT-1 day, 23:59:00 STD>

# dateutil
In [427]: rng_dateutil = pd.date_range("3/6/2012 00:00", periods=3, freq="D")

In [428]: rng_dateutil = rng_dateutil.tz_localize("dateutil/Europe/London")

In [429]: rng_dateutil.tz
Out[429]: tzfile('/usr/share/zoneinfo/Europe/London')

# dateutil - utc special case
In [430]: rng_utc = pd.date_range(
   .....:     "3/6/2012 00:00",
   .....:     periods=3,
   .....:     freq="D",
   .....:     tz=dateutil.tz.tzutc(),
   .....: )
   .....: 

In [431]: rng_utc.tz
Out[431]: tzutc()

### 0.25.0新版本
# datetime.timezone
In [432]: rng_utc = pd.date_range(
   .....:     "3/6/2012 00:00",
   .....:     periods=3,
   .....:     freq="D",
   .....:     tz=datetime.timezone.utc,
   .....: )
   .....: 

In [433]: rng_utc.tz
Out[433]: datetime.timezone.utc
```



```python
In [434]: import pytz

# pytz
In [435]: tz_pytz = pytz.timezone("Europe/London")

In [436]: rng_pytz = pd.date_range("3/6/2012 00:00", periods=3, freq="D")

In [437]: rng_pytz = rng_pytz.tz_localize(tz_pytz)

In [438]: rng_pytz.tz == tz_pytz
Out[438]: True

# dateutil
In [439]: tz_dateutil = dateutil.tz.gettz("Europe/London")

In [440]: rng_dateutil = pd.date_range("3/6/2012 00:00", periods=3, freq="D", tz=tz_dateutil)

In [441]: rng_dateutil.tz == tz_dateutil
Out[441]: True
```



## 转换时区

```python
In [442]: rng_pytz.tz_convert("US/Eastern")
Out[442]: 
DatetimeIndex(['2012-03-05 19:00:00-05:00', '2012-03-06 19:00:00-05:00',
               '2012-03-07 19:00:00-05:00'],
              dtype='datetime64[ns, US/Eastern]', freq=None)
```

```python
In [443]: dti = pd.date_range("2019-01-01", periods=3, freq="D", tz="US/Pacific")

In [444]: dti.tz
Out[444]: <DstTzInfo 'US/Pacific' LMT-1 day, 16:07:00 STD>

In [445]: ts = pd.Timestamp("2019-01-01", tz="US/Pacific")

In [446]: ts.tz
Out[446]: <DstTzInfo 'US/Pacific' PST-1 day, 16:00:00 STD>
```

对于pytz时区，将时区对象直接传递到datetime.datetime构造函数中是不正确的

- 需要使用时区对象上的方法来本地化datetime

如果您使用的日期超出2038-01-18，则由于2038年问题引起的基础库中当前的不足，将不应用夏时制（DST）对时区感知日期的调整。如果底层库已修复，则将应用DST过渡



所有时间戳都存储在UTC中。来自时区的值 DatetimeIndex或Timestamp将其字段（天，小时，分钟等）本地化到时区的值

```python
In [452]: rng_eastern = rng_utc.tz_convert("US/Eastern")

In [453]: rng_berlin = rng_utc.tz_convert("Europe/Berlin")

In [454]: rng_eastern[2]
Out[454]: Timestamp('2012-03-07 19:00:00-0500', tz='US/Eastern', freq='D')

In [455]: rng_berlin[2]
Out[455]: Timestamp('2012-03-08 01:00:00+0100', tz='Europe/Berlin', freq='D')

In [456]: rng_eastern[2] == rng_berlin[2]
Out[456]: True
```

```python
In [457]: ts_utc = pd.Series(range(3), pd.date_range("20130101", periods=3, tz="UTC"))
In [458]: eastern = ts_utc.tz_convert("US/Eastern")
In [459]: berlin = ts_utc.tz_convert("Europe/Berlin")

In [460]: result = eastern + berlin
In [461]: result

In [462]: result.index
```

要删除时区信息，请使用tz_localize(None)或tz_convert(None)。 

- tz_localize(None)将删除产生当地时间表示的时区
- tz_convert(None)将转换为UTC时间后将删除时区

```python
In [463]: didx = pd.date_range(start="2014-08-01 09:00", freq="H", periods=3, tz="US/Eastern")

In [464]: didx
Out[464]: 
DatetimeIndex(['2014-08-01 09:00:00-04:00', '2014-08-01 10:00:00-04:00',
               '2014-08-01 11:00:00-04:00'],
              dtype='datetime64[ns, US/Eastern]', freq='H')
              
In [465]: didx.tz_localize(None)
In [466]: didx.tz_convert(None)

# tz_convert(None) is identical to tz_convert('UTC').tz_localize(None)
In [467]: didx.tz_convert("UTC").tz_localize(None)
Out[467]: 
DatetimeIndex(['2014-08-01 13:00:00', '2014-08-01 14:00:00',
               '2014-08-01 15:00:00'],
              dtype='datetime64[ns]', freq=None)
```



## 参数fold

由于夏令时，从夏季到冬季，一个挂钟时间可能会发生两次

通常，Timestamp.tz_localize()如果您需要直接控制不明确的日期时间，则建议在本地化时要依靠它们

```python
In [468]: pd.Timestamp(
   .....:     datetime.datetime(2019, 10, 27, 1, 30, 0, 0),
   .....:     tz="dateutil/Europe/London",
   .....:     fold=0,
   .....: )
   .....: 
Out[468]: Timestamp('2019-10-27 01:30:00+0100', tz='dateutil//usr/share/zoneinfo/Europe/London')

In [469]: pd.Timestamp(
   .....:     year=2019,
   .....:     month=10,
   .....:     day=27,
   .....:     hour=1,
   .....:     minute=30,
   .....:     tz="dateutil/Europe/London",
   .....:     fold=1,
   .....: )
   .....: 
Out[469]: Timestamp('2019-10-27 01:30:00+0000', tz='dateutil//usr/share/zoneinfo/Europe/London')
```



## 本地化时的歧义

tz_localize可能无法确定时间戳的UTC偏移量，因为本地时区中的夏令时（DST）导致一天中的某些时间发生两次（“时钟倒退”）

- `'raise'`：引发一个`pytz.AmbiguousTimeError`（默认行为）
- `'infer'`：尝试根据时间戳的单调性确定正确的偏移量
- `'NaT'`：用代替模糊的时间 `NaT`
- `bool`：`True`表示DST时间，`False`表示非DST时间。在`bool`一段时间内支持类似数组的值。



## 本地化时 不存在的时间

通过nonexistent参数控制对不存在时间的时间序列进行本地化的行为

- `'raise'`：引发一个`pytz.NonExistentTimeError`（默认行为）
- `'NaT'`：用替换不存在的时间 `NaT`
- `'shift_forward'`：将不存在的时间向前移动到最接近的实时时间
- `'shift_backward'`：将不存在的时间向后移至最接近的实时
- timedelta对象：将不存在的时间偏移timedelta持续时间





## 操作Time zone series

类型都是datetime64[ns]

```python
In [480]: s_naive = pd.Series(pd.date_range("20130101", periods=3))

In [481]: s_naive
Out[481]: 
0   2013-01-01
1   2013-01-02
2   2013-01-03
dtype: datetime64[ns]

In [482]: s_aware = pd.Series(pd.date_range("20130101", periods=3, tz="US/Eastern"))

In [483]: s_aware
Out[483]: 
0   2013-01-01 00:00:00-05:00
1   2013-01-02 00:00:00-05:00
2   2013-01-03 00:00:00-05:00
dtype: datetime64[ns, US/Eastern]

### .tz_localize
In [484]: s_naive.dt.tz_localize("UTC").dt.tz_convert("US/Eastern")

### .astype
# localize and convert a naive time zone
In [485]: s_naive.astype("datetime64[ns, US/Eastern]")


# make an aware tz naive
In [486]: s_aware.astype("datetime64[ns]")


# convert to a new time zone
In [487]: s_aware.astype("datetime64[ns, CET]")
```

注意：NumPy当前不支持时区

- Series.to_numpy() 

```python
In [488]: s_naive.to_numpy()
Out[488]: 
array(['2013-01-01T00:00:00.000000000', '2013-01-02T00:00:00.000000000',
       '2013-01-03T00:00:00.000000000'], dtype='datetime64[ns]')

In [489]: s_aware.to_numpy()
Out[489]: 
array([Timestamp('2013-01-01 00:00:00-0500', tz='US/Eastern', freq='D'),
       Timestamp('2013-01-02 00:00:00-0500', tz='US/Eastern', freq='D'),
       Timestamp('2013-01-03 00:00:00-0500', tz='US/Eastern', freq='D')],
      dtype=object)
# 如果要使用实际的NumPydatetime64[ns]数组（将值转换为UTC）而不是对象数组，则可以指定 dtype参数
In [491]: s_aware.to_numpy(dtype="datetime64[ns]")
Out[491]: 
array(['2013-01-01T05:00:00.000000000', '2013-01-02T05:00:00.000000000',
       '2013-01-03T05:00:00.000000000'], dtype='datetime64[ns]')
```







# #参考文献

[Link: 官方文档|Python datetime](https://docs.python.org/3/library/datetime.html#strftime-and-strptime-behavior)