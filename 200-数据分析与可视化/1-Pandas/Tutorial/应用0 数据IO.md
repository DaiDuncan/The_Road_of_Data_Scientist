| Format Type | Data Description                                             | Reader                                                       | Writer                                                       |
| :---------- | :----------------------------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| text        | ==[CSV](https://en.wikipedia.org/wiki/Comma-separated_values)== | [read_csv](https://pandas.pydata.org/pandas-docs/stable/user_guide/io.html#io-read-csv-table) | [to_csv](https://pandas.pydata.org/pandas-docs/stable/user_guide/io.html#io-store-in-csv) |
| text        | Fixed-Width Text File                                        | [read_fwf](https://pandas.pydata.org/pandas-docs/stable/user_guide/io.html#io-fwf-reader) |                                                              |
| text        | [JSON](https://www.json.org/)                                | [read_json](https://pandas.pydata.org/pandas-docs/stable/user_guide/io.html#io-json-reader) | [to_json](https://pandas.pydata.org/pandas-docs/stable/user_guide/io.html#io-json-writer) |
| text        | [HTML](https://en.wikipedia.org/wiki/HTML)                   | [read_html](https://pandas.pydata.org/pandas-docs/stable/user_guide/io.html#io-read-html) | [to_html](https://pandas.pydata.org/pandas-docs/stable/user_guide/io.html#io-html) |
| text        | Local clipboard                                              | [read_clipboard](https://pandas.pydata.org/pandas-docs/stable/user_guide/io.html#io-clipboard) | [to_clipboard](https://pandas.pydata.org/pandas-docs/stable/user_guide/io.html#io-clipboard) |
|             | [MS Excel](https://en.wikipedia.org/wiki/Microsoft_Excel)    | [read_excel](https://pandas.pydata.org/pandas-docs/stable/user_guide/io.html#io-excel-reader) | [to_excel](https://pandas.pydata.org/pandas-docs/stable/user_guide/io.html#io-excel-writer) |
| binary      | [OpenDocument](http://www.opendocumentformat.org/)           | [read_excel](https://pandas.pydata.org/pandas-docs/stable/user_guide/io.html#io-ods) |                                                              |
| binary      | ==[HDF5 Format](https://support.hdfgroup.org/HDF5/whatishdf5.html)== | [read_hdf](https://pandas.pydata.org/pandas-docs/stable/user_guide/io.html#io-hdf5) | [to_hdf](https://pandas.pydata.org/pandas-docs/stable/user_guide/io.html#io-hdf5) |
| binary      | [Feather Format](https://github.com/wesm/feather)            | [read_feather](https://pandas.pydata.org/pandas-docs/stable/user_guide/io.html#io-feather) | [to_feather](https://pandas.pydata.org/pandas-docs/stable/user_guide/io.html#io-feather) |
| binary      | [Parquet Format](https://parquet.apache.org/)                | [read_parquet](https://pandas.pydata.org/pandas-docs/stable/user_guide/io.html#io-parquet) | [to_parquet](https://pandas.pydata.org/pandas-docs/stable/user_guide/io.html#io-parquet) |
| binary      | [ORC Format](https://orc.apache.org/)                        | [read_orc](https://pandas.pydata.org/pandas-docs/stable/user_guide/io.html#io-orc) |                                                              |
| binary      | [Msgpack](https://msgpack.org/index.html)                    | [read_msgpack](https://pandas.pydata.org/pandas-docs/stable/user_guide/io.html#io-msgpack) | [to_msgpack](https://pandas.pydata.org/pandas-docs/stable/user_guide/io.html#io-msgpack) |
| binary      | [Stata](https://en.wikipedia.org/wiki/Stata)                 | [read_stata](https://pandas.pydata.org/pandas-docs/stable/user_guide/io.html#io-stata-reader) | [to_stata](https://pandas.pydata.org/pandas-docs/stable/user_guide/io.html#io-stata-writer) |
| binary      | [SAS](https://en.wikipedia.org/wiki/SAS_(software))          | [read_sas](https://pandas.pydata.org/pandas-docs/stable/user_guide/io.html#io-sas-reader) |                                                              |
| binary      | [SPSS](https://en.wikipedia.org/wiki/SPSS)                   | [read_spss](https://pandas.pydata.org/pandas-docs/stable/user_guide/io.html#io-spss-reader) |                                                              |
| binary      | [Python Pickle Format](https://docs.python.org/3/library/pickle.html) | [read_pickle](https://pandas.pydata.org/pandas-docs/stable/user_guide/io.html#io-pickle) | [to_pickle](https://pandas.pydata.org/pandas-docs/stable/user_guide/io.html#io-pickle) |
| SQL         | ==[SQL](https://en.wikipedia.org/wiki/SQL)==                 | [read_sql](https://pandas.pydata.org/pandas-docs/stable/user_guide/io.html#io-sql) | [to_sql](https://pandas.pydata.org/pandas-docs/stable/user_guide/io.html#io-sql) |
| SQL         | [Google BigQuery](https://en.wikipedia.org/wiki/BigQuery)    | [read_gbq](https://pandas.pydata.org/pandas-docs/stable/user_guide/io.html#io-bigquery) | [to_gbq](https://pandas.pydata.org/pandas-docs/stable/user_guide/io.html#io-bigquery) |



```python
from io import StringIO  #Python3
```





目录提纲：闲暇待完成

# CSV & text files

```python
pd.read_csv('data.csv',)  
```



## 解析选项 Parsing options 

- 基本参数 Basic 
  - filepath_or_buffer
    - 包含HTTP FTP
  - sep=',' 分隔符，默认为逗号，
    - @正则表达式
  - delimiterstr=None 参数sep的备选项
  - delim_whitespace=False 空白符是否包括(如' '或'\t')
- Column and index locations and names
  - header='infer' 
    - header=0表示从数据的第一行中推断出column names => ==也就是说第一行是标题行(index=0)，第二行开始才是数据(index=1)==
  - names=None  column names的列表
  - index_col=None 设置某一列作为index列
  - usecols=None 过滤列
  - squeeze=False
  - prefix=None
  - mangle_dupe_cols=True  重复的列名
- 一般解析配置 General parsing configuration
  - dtype=None
  - engine {'c', 'python'}
    - c更有效
    - python更完善  一般不用python引擎
  - converters=None
  - true_values=None 被看作True的值
  - false_values=None
  - skipinitialspace=False
  - skiprows=None 跳过开头的几行
  - skipfooter=0 跳过末尾几行
  - nrows=None
  - low_memory=True 打开低内存限制
  - memory_map
- 处理缺失值 NA and missing data handling 
  - na_values=None
  - keep_default_na
  - na_filter
  - verbose
  - skip_blank_lines
- 处理时间数据 Datetime handling
  - parse_dates
  - infer_datetime_format
  - keep_date_col
  - date_parser
  - dayfirst
  - cache_dates
- 遍历 Iteration
  - iterator
  - ==chunksize==
- 注释、压缩 Quoting, compression, and file format
  - compression
  - thousands
  - decimal 千分位的分隔符号
  - float_precision
  - lineterminator
  - quotechar
  - quoting
  - doublequote
  - escapechar
  - comment
  - encoding=None  'utf-8'
    - [Link: Python标准encodings](https://docs.python.org/3/library/codecs.html#standard-encodings)
  - dialect
- 处理错误 Error handling
  - error_bad_lines  = False 跳过错误行
  - warn_bad_lines



## 指定列数据类型 Specifying column data types



## 指定类别数据 Specifying categorical dtype





## 属性names

- 处理column names

  - header

    

## 解析重复的names

参数mangle_dupe_cols

- 过滤columns
  - usecols



## 注释与空行

- 注释
  - comment
  - skip_blank_lines=False

## 处理Unicode数据

参数encoding

## columns的Index   Index columns and trailing delimiters

参数index_col=False

```python
In [85]: data = ('a,b,c\n'
   ....:         '4,apple,bat,5.7\n'
   ....:         '8,orange,cow,10')
   ....: 

### 使用StringIO进行读写
In [86]: pd.read_csv(StringIO(data))
Out[86]: 
        a    b     c
4   apple  bat   5.7
8  orange  cow  10.0


In [87]: data = ('index,a,b,c\n'
   ....:         '4,apple,bat,5.7\n'
   ....:         '8,orange,cow,10')
   ....: 

In [88]: pd.read_csv(StringIO(data), index_col=0)
Out[88]: 
            a    b     c
index                   
4       apple  bat   5.7
8      orange  cow  10.0
```



- 示例|使用StringIO进行读写

```python
f = open("wind.data","r+").read()
### 数据集特点：分割符号为空格，但是空格数不总是三个
f2 = f.replace("   "," ").replace("  "," ").replace("    "," ")

import io
d = io.StringIO(f2)
data = pd.read_table(d,sep=" ")
```





## 处理日期Date

- 指定date的columns

  - parse_dates
  - date_parser：三种方式调用该参数
    - 1 infer_datetime_format=True
    - 2 pd.to_datetime(): date_parser=lambda x: pd.to_datetime(x, format=...)
    - 3 非标准格式：date_parser
  - keep_date_col

- 数据解析函数

- 基于mixed的时区timezones：解析CSV

  - date_parser=lambda col: pd.to_datetime(col, utc=True)

- 推断datetime格式

  - infer_datetime_format=True
    - dayfirst=True, it will guess “01/12/2011” to be December 1st.
    - dayfirst=False (default) => it will guess “01/12/2011” to be January 12th.

  >- “20111230”
  >- “2011/12/30”
  >- “20111230 00:00:00”
  >- “12/30/2011 00:00:00”
  >- “30/Dec/2011 00:00:00”
  >- “30/December/2011 00:00:00”

- 国际date格式

  - dayfirst
  - 美国：MM/DD/YYYY
    - 国际：DD/MM/YYYY

## 指定浮点型数据转换的方法

参数float_precision

## 千位分隔符

参数thousands

## NA值

```python
['-1.#IND', '1.#QNAN', '1.#IND', '-1.#QNAN', '#N/A N/A', '#N/A', 'N/A', 'n/a', 'NA', '<NA>', '#NA', 'NULL', 'null', 'NaN', '-NaN', 'nan', '-nan', '']

pd.read_csv('path_to_file.csv', na_values=[5])  #5 5.0都会被看作NaN
```

## 无穷inf

参数np.inf：正无穷

## 返回Series

参数squeeze

## 布尔值

- true_values 
- false_values

## 处理错误行

- error_bad_lines=False
- usecols=[0, 1, 2]

## 方言

- dialect=dia
- skipinitialspace

- 引号Quoting  和 转义符Escape Characters

  - escapechar='\\\\'

  - ```python
    In [156]: data = 'a,b\n"hello, \\"Bob\\", nice to see you",5'
    
    In [157]: print(data)
    a,b
    "hello, \"Bob\", nice to see you",5
    
    In [158]: pd.read_csv(StringIO(data), escapechar='\\')
    Out[158]: 
                                   a  b
    0  hello, "Bob", nice to see you  5
    ```

## 格式：固定宽度的列 => read_fwf()

- colspecs
- widths
- delimiter



## 索引Indexes

- 隐含index column的文件
- 基于MultiIndex，读取index
- 基于MultiIndex，读取column

## 自动识别分隔符delimiter

## 读取多个文件：生成单个DataFrame

## 文件遍历：chunk by chunk

## 指定解析引擎

## 读取远程csv文件

```python
df = pd.read_csv('https://download.bls.gov/pub/time.series/cu/cu.item',
                 sep='\t')

df = pd.read_csv('s3://pandas-test/tips.csv')
```



## 写数据⭐

- 写CSV
  - path_or_buf
  - sep
  - float_format
  - columns
  - header
  - index
  - index_label
  - mode
  - encoding
  - line_terminator
  - quoting
  - quotechar
  - doublequote
  - escapechar
  - chunksize
  - date_format
- 写入格式化的字符串
  - buf 
  - columns
  - col_space
  - na_rep
  - formatters
  - float_format
  - sparsify
  - index_names
  - index
  - header
  - justify=left
- Series.to_string
  - buf
  - na_rep
  - float_format
  - length 



# JSON

> **JSON = Python Dictionary** y

Series和DataFrame能转化为有效的JSON文件



## 写入JSON

- to_json
  - path_or_buf

  - orient 

    - Series 默认index：{split, records, index}

    - DataFrame 默认columns： {split, records, index, columns, values, table}

    - JSON字符串的格式

    - | `split`   | dict like {index -> [index], columns -> [columns], data -> [values]} |
      | --------- | ------------------------------------------------------------ |
      | `records` | list like [{column -> value}, … , {column -> value}]         |
      | `index`   | dict like {index -> {column -> value}}                       |
      | `columns` | dict like {column -> {index -> value}}                       |
      | `values`  | just the values array                                        |

  - date_format

  - double_precision

  - force_ascii 

  - date_unit 

  - default_handler 

  - lines 

注意：NaN NaT和None都会被转换为null；datetime对象将根据date_format和date_unit参数进行转换



- Orient选项
- 处理日期Date
- Fallback行为

## 读取JSON

- pd.read_json
  - filepath_or_buffer
  - ……



- 数据转换

- Numpy参数

- 正规化Normalization

- 行分隔的json

- 表格架构

  - | Pandas type     | Table Schema type |
    | :-------------- | :---------------- |
    | int64           | integer           |
    | float64         | number            |
    | bool            | boolean           |
    | datetime64[ns]  | datetime          |
    | timedelta64[ns] | duration          |
    | categorical     | any               |
    | object          | str               |



# HTML

## 读取HTML

- read_html()：

- ```python
  In [296]: url = 'https://www.fdic.gov/bank/individual/failed/banklist.html'
  In [297]: dfs = pd.read_html(url)
      
      
  In [299]: with open(file_path, 'r') as f:
     .....:     dfs = pd.read_html(f.read())
     .....: 
  
  In [301]: with open(file_path, 'r') as f:
     .....:     sio = StringIO(f.read())
     .....: 
  In [302]: dfs = pd.read_html(sio)
      
  match = 'Metcalf Bank'
  df_list = pd.read_html(url, match=match)
  ```



## 写入HTML

- .to_html()



## HTML解析“陷阱”

lxml的问题

- 优点
  - 效率高，非常快
  - 需要Cython
- 缺陷
  - 对其解析的结果不做任何保证，除非为其提供严格有效的标记
  - 允许您（用户）使用lxml后端，但是如果lxml无法解析，则此后端将使用html5lib
  - 强烈建议您同时安装BeautifulSoup4和html5lib，以便即使lxml失败，您仍将获得有效结果



基于lxml的BeautifulSoup4的问题

基于html5lin的BeautifulSoup4的问题

- 优点
  - html5lib比lxml宽容得多，因此以一种更为合理的方式处理现实生活中的标记，而不仅仅是例如在不通知您的情况下删除元素
  - html5lib是纯Python
- 缺陷
  - 网络上的许多表的数据量都不足以抵消进行解析的运行时间消耗
  - 瓶颈：Web的URL IO => 对于非常大的表：缺点没那么明显



# Excel

## 读取Excel

```python
# Returns a DataFrame
pd.read_excel('path_to_file.xls', sheet_name='Sheet1')

xlsx = pd.ExcelFile('path_to_file.xls')
df = pd.read_excel(xlsx, 'Sheet1')

with pd.ExcelFile('path_to_file.xls') as xls:
    df1 = pd.read_excel(xls, 'Sheet1')
    df2 = pd.read_excel(xls, 'Sheet2')
```



- 指定sheets
- 读取MultiIndex
- 解析指定的columns
- 解析dates
- 单元格转化
- 指定Dtype

## 写入Excel

```python
from io import BytesIO

bio = BytesIO()

# By setting the 'engine' in the ExcelWriter constructor.
writer = pd.ExcelWriter(bio, engine='xlsxwriter')
df.to_excel(writer, sheet_name='Sheet1')

# Save the workbook
writer.save()

# Seek to the beginning and read to copy the workbook to a variable in memory
bio.seek(0)
workbook = bio.read()
```



- Excel写入到硬盘
- Excel写入到内存
- Excel写引擎
- 风格与格式



# 开放电子表格|OpenDocument Spreadsheets

```python
# Returns a DataFrame
pd.read_excel('path_to_file.ods', engine='odf')
```



# 二进制Excel

```python
# Returns a DataFrame
pd.read_excel('path_to_file.xlsb', engine='pyxlsb')
```



# 剪贴板

read_clipboard()

to_clipboard()



# Pickling

.to_pickle()|Python’s cPickle module

```python
In [328]: df
Out[328]: 
c1         a   
c2         b  d
lvl1 lvl2      
a    c     1  5
     d     2  6
b    c     3  7
     d     4  8

In [329]: df.to_pickle('foo.pkl')
    
    
In [330]: pd.read_pickle('foo.pkl')
Out[330]: 
c1         a   
c2         b  d
lvl1 lvl2      
a    c     1  5
     d     2  6
b    c     3  7
     d     4  8
```



## 压缩的pickle文件

read_pickle()

DataFrame.to_pickle()

Series.to_pickle() 

以上三者均可读、写压缩的pickle文件 => 压缩格式为：gzip, bz2, xz

- zip文件格式仅支持读取：并且只能包含一个要读取的数据文件
- 压缩类型可指明，或者从文件扩展名推断：gzip, bz2, zip, or xz
- 压缩参数也可以是字典：method的key必须是 {'zip', 'gzip', 'bz2'}其中的选项

```python
In [331]: df = pd.DataFrame({
   .....:     'A': np.random.randn(1000),
   .....:     'B': 'foo',
   .....:     'C': pd.date_range('20130101', periods=1000, freq='s')})
   .....: 
    
In [333]: df.to_pickle("data.pkl.compress", compression="gzip")
In [334]: rt = pd.read_pickle("data.pkl.compress", compression="gzip")
    
    
In [336]: df.to_pickle("data.pkl.xz", compression="infer")
In [337]: rt = pd.read_pickle("data.pkl.xz", compression="infer")

    
In [339]: df.to_pickle("data.pkl.gz")
In [340]: rt = pd.read_pickle("data.pkl.gz")
    
    
In [342]: df["A"].to_pickle("s1.pkl.bz2")
In [343]: rt = pd.read_pickle("s1.pkl.bz2")
    
    
In [345]: df.to_pickle(
   .....:     "data.pkl.gz",
   .....:     compression={"method": "gzip", 'compresslevel': 1})
```





# msgpack

在1.0.0版中已删除了对msgpack的熊猫支持。 建议使用pyarrow进行Pandas对象的在线传输。

```python
>>> import pandas as pd
>>> import pyarrow as pa
>>> df = pd.DataFrame({'A': [1, 2, 3]})
>>> context = pa.default_serialization_context()
>>> df_bytestring = context.serialize(df).to_buffer().to_pybytes()
```



# HDF5 (PyTables)

HDFStore是字典对象

Pandas实用PyTables 读写HDF5文件

```python
n [346]: store = pd.HDFStore('store.h5')

In [347]: print(store)
<class 'pandas.io.pytables.HDFStore'>
File path: store.h5
    
    
In [348]: index = pd.date_range('1/1/2000', periods=8)

In [349]: s = pd.Series(np.random.randn(5), index=['a', 'b', 'c', 'd', 'e'])

In [350]: df = pd.DataFrame(np.random.randn(8, 3), index=index,
   .....:                   columns=['A', 'B', 'C'])
   .....: 

# store.put('s', s) is an equivalent method
In [351]: store['s'] = s

In [352]: store['df'] = df

In [353]: store
Out[353]: 
<class 'pandas.io.pytables.HDFStore'>
File path: store.h5
```





## 读写 API

```python
In [362]: df_tl = pd.DataFrame({'A': list(range(5)), 'B': list(range(5))})
In [363]: df_tl.to_hdf('store_tl.h5', 'table', append=True)
    
In [364]: pd.read_hdf('store_tl.h5', 'table', where=['index>2'])
Out[364]: 
   A  B
3  3  3
4  4  4


In [365]: df_with_missing = pd.DataFrame({'col1': [0, np.nan, 2],
   .....:                                 'col2': [1, np.nan, np.nan]})
   .....: 

In [366]: df_with_missing
Out[366]: 
   col1  col2
0   0.0   1.0
1   NaN   NaN
2   2.0   NaN

In [367]: df_with_missing.to_hdf('file.h5', 'df_with_missing',
   .....:                        format='table', mode='w')
   .....: 

In [368]: pd.read_hdf('file.h5', 'df_with_missing')
Out[368]: 
   col1  col2
0   0.0   1.0
1   NaN   NaN
2   2.0   NaN

In [369]: df_with_missing.to_hdf('file.h5', 'df_with_missing',
   .....:                        format='table', mode='w', dropna=True)
   .....: 

In [370]: pd.read_hdf('file.h5', 'df_with_missing')
Out[370]: 
   col1  col2
0   0.0   1.0
1   NaN   NaN
2   2.0   NaN
```



## fixed format

固定格式存储区提供的写入速度比表存储区快，读取速度稍快

```python
### 故障
>>> pd.DataFrame(np.random.randn(10, 2)).to_hdf('test_fixed.h5', 'df')
>>> pd.read_hdf('test_fixed.h5', 'df', where='index>5')
TypeError: cannot pass a where specification when reading a fixed format.
           this store must be selected in its entirety
```



## Table format

```python
In [371]: store = pd.HDFStore('store.h5')

In [372]: df1 = df[0:4]

In [373]: df2 = df[4:]

# append data (creates a table automatically)
In [374]: store.append('df', df1)

In [375]: store.append('df', df2)

In [376]: store
Out[376]: 
<class 'pandas.io.pytables.HDFStore'>
File path: store.h5

# select the entire object
In [377]: store.select('df')
Out[377]: 
                   A         B         C
2000-01-01  1.334065  0.521036  0.930384
2000-01-02 -1.613932  1.088104 -0.632963
2000-01-03 -0.585314 -0.275038 -0.937512
2000-01-04  0.632369 -1.249657  0.975593
2000-01-05  1.060617 -0.143682  0.218423
2000-01-06  3.050329  1.317933 -0.963725
2000-01-07 -0.539452 -0.771133  0.023751
2000-01-08  0.649464 -1.736427  0.197288

# the type of stored data
In [378]: store.root.df._v_attrs.pandas_type
Out[378]: 'frame_table'
```



## Hierarchical keys层次键

```python
In [379]: store.put('foo/bar/bah', df)

In [380]: store.append('food/orange', df)

In [381]: store.append('food/apple', df)

In [382]: store
Out[382]: 
<class 'pandas.io.pytables.HDFStore'>
File path: store.h5

# a list of keys are returned
In [383]: store.keys()
Out[383]: ['/df', '/food/apple', '/food/orange', '/foo/bar/bah']

# remove all nodes under this level
In [384]: store.remove('food')

In [385]: store
Out[385]: 
<class 'pandas.io.pytables.HDFStore'>
File path: store.h5
```



## 存储类型

- 混合类型
- MultiIndex DataFrames



## 查询

- 查询表格

  - `'index >= date'`
  - `"columns = ['A', 'D']"`
  - `"columns in ['A', 'D']"`
  - `'columns = A'`
  - `'columns == A'`
  - `"~(columns = ['A', 'B'])"`
  - `'index > df.index[3] & string = "bar"'`
  - `'(index > df.index[3] & index <= df.index[6]) | string = "bar"'`
  - `"ts >= Timestamp('2012-02-01')"`
  - `"major_axis>=20130101"`

- ```python
  In [401]: dfq = pd.DataFrame(np.random.randn(10, 4), columns=list('ABCD'),
     .....:                    index=pd.date_range('20130101', periods=10))
     .....: 
  
  In [402]: store.append('dfq', dfq, format='table', data_columns=True)
      
  In [403]: store.select('dfq', "index>pd.Timestamp('20130104') & columns=['A', 'B']")
  Out[403]: 
                     A         B
  2013-01-05 -1.083889  0.811865
  2013-01-06 -0.402227  1.618922
  2013-01-07  0.948196  0.183573
  2013-01-08 -1.043530 -0.708145
  2013-01-09  0.813949  1.508891
  2013-01-10  1.176488 -1.246093
  
  In [404]: store.select('dfq', where="A>0 or C>0")
  In [405]: store.select('df', "columns=['A', 'B']")
  ```

- 查询timedelta64[ns]

- 查询MultiIndex

- 索引Indexing

- 通过数据的列进行查询

- 遍历器Iterator

- 高级查询

  - 选择单列
  - 选择查询坐标/索引位置
  - 基于mask选择
  - 存储对象

- 多表格查询



## 从表格中删除



## 注意事项/警告

- 压缩
- ptrepack
- 注意事项



## 数据类型

| Type                                                 | Represents missing values |
| :--------------------------------------------------- | :------------------------ |
| floating : `float64, float32, float16`               | `np.nan`                  |
| integer : `int64, int32, int8, uint64,uint32, uint8` |                           |
| boolean                                              |                           |
| `datetime64[ns]`                                     | `NaT`                     |
| `timedelta64[ns]`                                    | `NaT`                     |
| categorical : see the section below                  |                           |
| object : `strings`                                   | `np.nan`                  |



- 类别数据
- 字符串的columns



## 外部兼容

## 性能





# Feather

为DataFrame提供二进制列式序列化：它旨在提高读写数据帧的效率，并使跨数据分析语言的数据共享变得容易。



# Parquet|Apache

为DataFrame提供分区的二进制列式序列化：它旨在提高读写数据帧的效率，并使跨数据分析语言的数据共享变得容易。

可以使用各种压缩技术来尽可能缩小文件大小，同时仍保持良好的读取性能。



## 处理indexes

## Parquet文件分区



# ORC

与Parquet类似，为DataFrame提供分区的二进制列式序列化：它旨在提高读写数据帧的效率，并使跨数据分析语言的数据共享变得容易。



# SQL 查询

| [`read_sql_table`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_sql_table.html#pandas.read_sql_table)(table_name, con[, schema, …]) | Read SQL database table into a DataFrame.              |
| ------------------------------------------------------------ | ------------------------------------------------------ |
| [`read_sql_query`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_sql_query.html#pandas.read_sql_query)(sql, con[, index_col, …]) | Read SQL query into a DataFrame.                       |
| [`read_sql`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_sql.html#pandas.read_sql)(sql, con[, index_col, …]) | Read SQL query or database table into a DataFrame.     |
| [`DataFrame.to_sql`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.to_sql.html#pandas.DataFrame.to_sql)(name, con[, schema, …]) | Write records stored in a DataFrame to a SQL database. |



## 写入DataFrames

- SQL数据类型
- Datetime
- 插入



## 读取表格

- 表格支持

- 查询

- 示例：引擎连接@Udacity ETL项目

- ```python
  from sqlalchemy import create_engine
  
  engine = create_engine('postgresql://scott:tiger@localhost:5432/mydatabase')
  
  engine = create_engine('mysql+mysqldb://scott:tiger@localhost/foo')
  
  engine = create_engine('oracle://scott:tiger@127.0.0.1:1521/sidname')
  
  engine = create_engine('mssql+pyodbc://mydsn')
  
  # sqlite://<nohostname>/<path>
  # where <path> is relative:
  engine = create_engine('sqlite:///foo.db')
  
  # or absolute, starting with a slash:
  engine = create_engine('sqlite:////absolute/path/to/foo.db')
  ```



## 高级SQLAlchemy查询

```python
In [538]: import sqlalchemy as sa

In [539]: pd.read_sql(sa.text('SELECT * FROM data where Col_1=:col1'),
   .....:             engine, params={'col1': 'X'})
   .....: 
Out[539]: 
   index  id                        Date Col_1  Col_2  Col_3
0      0  26  2010-10-18 00:00:00.000000     X   27.5      1
```



## Sqlite fallback

```python
import sqlite3
con = sqlite3.connect(':memory:')
```





# Google BigQuery

```shell
pip install pandas-gbq
```



# Stata format

## 写入

```python
In [546]: df = pd.DataFrame(np.random.randn(10, 2), columns=list('AB'))
In [547]: df.to_stata('stata.dta')
```



## 读取

- 类别数据



# SAS formats



# SPSS formats



# 其他文件格式

## netCDF



# 考虑性能|对比测试

```python
In [1]: sz = 1000000
In [2]: df = pd.DataFrame({'A': np.random.randn(sz), 'B': [1] * sz})

In [3]: df.info()
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 1000000 entries, 0 to 999999
Data columns (total 2 columns):
A    1000000 non-null float64
B    1000000 non-null int64
dtypes: float64(1), int64(1)
memory usage: 15.3 MB
    
    
### 测试对比
import numpy as np

import os

sz = 1000000
df = pd.DataFrame({'A': np.random.randn(sz), 'B': [1] * sz})

sz = 1000000
np.random.seed(42)
df = pd.DataFrame({'A': np.random.randn(sz), 'B': [1] * sz})

def test_sql_write(df):
    if os.path.exists('test.sql'):
        os.remove('test.sql')
    sql_db = sqlite3.connect('test.sql')
    df.to_sql(name='test_table', con=sql_db)
    sql_db.close()

def test_sql_read():
    sql_db = sqlite3.connect('test.sql')
    pd.read_sql_query("select * from test_table", sql_db)
    sql_db.close()

def test_hdf_fixed_write(df):
    df.to_hdf('test_fixed.hdf', 'test', mode='w')

def test_hdf_fixed_read():
    pd.read_hdf('test_fixed.hdf', 'test')

def test_hdf_fixed_write_compress(df):
    df.to_hdf('test_fixed_compress.hdf', 'test', mode='w', complib='blosc')

def test_hdf_fixed_read_compress():
    pd.read_hdf('test_fixed_compress.hdf', 'test')

def test_hdf_table_write(df):
    df.to_hdf('test_table.hdf', 'test', mode='w', format='table')

def test_hdf_table_read():
    pd.read_hdf('test_table.hdf', 'test')

def test_hdf_table_write_compress(df):
    df.to_hdf('test_table_compress.hdf', 'test', mode='w',
              complib='blosc', format='table')

def test_hdf_table_read_compress():
    pd.read_hdf('test_table_compress.hdf', 'test')

def test_csv_write(df):
    df.to_csv('test.csv', mode='w')

def test_csv_read():
    pd.read_csv('test.csv', index_col=0)

def test_feather_write(df):
    df.to_feather('test.feather')

def test_feather_read():
    pd.read_feather('test.feather')

def test_pickle_write(df):
    df.to_pickle('test.pkl')

def test_pickle_read():
    pd.read_pickle('test.pkl')

def test_pickle_write_compress(df):
    df.to_pickle('test.pkl.compress', compression='xz')

def test_pickle_read_compress():
    pd.read_pickle('test.pkl.compress', compression='xz')

def test_parquet_write(df):
    df.to_parquet('test.parquet')

def test_parquet_read():
    pd.read_parquet('test.parquet')
    
### 结果|写入
'''
test_feather_write
test_hdf_fixed_write
test_hdf_fixed_write_compress
'''
In [4]: %timeit test_sql_write(df)
3.29 s ± 43.2 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)

In [5]: %timeit test_hdf_fixed_write(df)
19.4 ms ± 560 µs per loop (mean ± std. dev. of 7 runs, 1 loop each)

In [6]: %timeit test_hdf_fixed_write_compress(df)
19.6 ms ± 308 µs per loop (mean ± std. dev. of 7 runs, 10 loops each)

In [7]: %timeit test_hdf_table_write(df)
449 ms ± 5.61 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)

In [8]: %timeit test_hdf_table_write_compress(df)
448 ms ± 11.9 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)

In [9]: %timeit test_csv_write(df)
3.66 s ± 26.2 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)

In [10]: %timeit test_feather_write(df)
9.75 ms ± 117 µs per loop (mean ± std. dev. of 7 runs, 100 loops each)

In [11]: %timeit test_pickle_write(df)
30.1 ms ± 229 µs per loop (mean ± std. dev. of 7 runs, 10 loops each)

In [12]: %timeit test_pickle_write_compress(df)
4.29 s ± 15.9 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)

In [13]: %timeit test_parquet_write(df)
67.6 ms ± 706 µs per loop (mean ± std. dev. of 7 runs, 10 loops each)


### 结果|读取
'''
test_feather_read
test_pickle_read
test_hdf_fixed_read
'''
In [14]: %timeit test_sql_read()
1.77 s ± 17.7 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)

In [15]: %timeit test_hdf_fixed_read()
19.4 ms ± 436 µs per loop (mean ± std. dev. of 7 runs, 10 loops each)

In [16]: %timeit test_hdf_fixed_read_compress()
19.5 ms ± 222 µs per loop (mean ± std. dev. of 7 runs, 10 loops each)

In [17]: %timeit test_hdf_table_read()
38.6 ms ± 857 µs per loop (mean ± std. dev. of 7 runs, 10 loops each)

In [18]: %timeit test_hdf_table_read_compress()
38.8 ms ± 1.49 ms per loop (mean ± std. dev. of 7 runs, 10 loops each)

In [19]: %timeit test_csv_read()
452 ms ± 9.04 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)

In [20]: %timeit test_feather_read()
12.4 ms ± 99.7 µs per loop (mean ± std. dev. of 7 runs, 100 loops each)

In [21]: %timeit test_pickle_read()
18.4 ms ± 191 µs per loop (mean ± std. dev. of 7 runs, 100 loops each)

In [22]: %timeit test_pickle_read_compress()
915 ms ± 7.48 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)

In [23]: %timeit test_parquet_read()
24.4 ms ± 146 µs per loop (mean ± std. dev. of 7 runs, 10 loops each)


### test.pkl.compress，test.parquet和test.feather占用的磁盘空间最少
# 单位：字节
29519500 Oct 10 06:45 test.csv
16000248 Oct 10 06:45 test.feather
8281983  Oct 10 06:49 test.parquet
16000857 Oct 10 06:47 test.pkl
7552144  Oct 10 06:48 test.pkl.compress
34816000 Oct 10 06:42 test.sql
24009288 Oct 10 06:43 test_fixed.hdf
24009288 Oct 10 06:43 test_fixed_compress.hdf
24458940 Oct 10 06:44 test_table.hdf
24458940 Oct 10 06:44 test_table_compress.hdf
```

