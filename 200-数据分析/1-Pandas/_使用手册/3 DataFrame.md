像 Series 一样， DataFrame 的 values 属性返回一个包含在 DataFrame 中的数据的二维 ndarray(Pandas大部分数据类型的底层是ndarray)



含有两个 index，分别为 column index 和 row index，可以在构造时直接指定索引标签名

### #数据结构的特性

带标签的行和列的二维数据结构，可以存储很多类型的数据(类似Excel的电子表格)



# 操作1|创建



## 1 自定义

一般可以使用 columns 属性指定列顺序，index 属性指定行 index,

- 调用 columns 方法可以返回列的 index
- 调用 index 方法可以返回行的 index。



如果不提供构造 DataFrame 时的 columns，默认使用字典的 Key，

如果不提供 index，则自动使用 RangeIndex。注意，columns 可能按照 A-Z 进行排序（字典的 tuple 没有顺序）

### 1）用字典生成

- **Series字典**(含一般字典)

`dict = {'key1': pd.Series([]), index = [], 'key2': pd.Series([]), index = []}`
`df = pd.DataFrame(dict)`

> 列column：字典的key
>
> 行index：也就是Series的index
>
> Value：Series的Data作为DataFrame的数据
> ==没有Series.index时，从0开始计数作为行标签==	

注意：index必须与对应value的列表等长，否则报错



- **列表字典**(含多维数组) => **key 作为 Column，value 作为 Row - Value**

适合时间序列数据，在这种情况下列表是绝对齐的

`dict = {'key1':[], 'key2': []}`
`df = pd.DataFrame(dict, columns=[])`
+列：字典的key
+行：对应没有Series.index的情况，从0开始计数作为行标签(列表作为DataFrame的数据)

![image](https://cdn.nlark.com/yuque/0/2020/png/1136179/1591174539192-09293e68-7dfd-45f1-8902-c087f2f5573b.png)
![image](https://cdn.nlark.com/yuque/0/2020/png/1136179/1591174539084-c264a875-2c22-4895-8cb6-40c17e31051d.png)





- **嵌套字典的字典**(类似于二级嵌套的JSON)

适用于不像数据库那种某一个维度有大量条目（比如用户），而是只有少数条目（比如季度盈利），这种情况可能 Row 不齐，因此，不统一制定 Index 会更方面些

使用嵌套字典生成的 DataFrame，即便指定了 index 字段，其 index 不齐、不使用 Series、Series Index 不齐也不会报错，缺失值会被记录为 NaN

```python
data_t = {"marvin":{"s4":300,"s3":123},"lili":{"s1":123,"s2":112,"s3":998}}
DataFrame(data_t)

    lili    marvin
s1	123.0	NaN
s2	112.0	NaN
s3	998.0	123.0
s4	NaN	300.0
```





```python
### 示例
# 随机生成6*4数据集
data_raw = pd.DataFrame(np.random.rand(6,4),columns=list('ABCD'))
data_raw

# 使用字典来创建数据集(Series字典)
df2 = pd.DataFrame({'A': np.random.randn(3)})   #N(0, 1)分布
df2

# 使用字典来创建数据集(Series字典)
df3 = pd.DataFrame({'A': pd.Timestamp('20200101'), 'B': np.random.randn(3)})
df3
```

随机生成6*4数据集

![image-20201215142501785](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201215142502.png)

使用字典来创建数据集(Series字典)

![image-20201215142519472](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201215142519.png)

使用字典来创建数据集(Series字典)

![image-20201215142534063](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201215142534.png)



### 2）用多维数组生成

`a=np.array([[1,2,3],[4,5,6],[7,8,9]])`
`df1=pd.DataFrame(a,index=['row0','row1','row2'],columns=list('ABC'))`



### 3）用==嵌套字典的列表==生成

`list = [{'bikes': 20, 'pants': 30, 'watches': 35}, {'watches': 10, 'glasses': 50, 'bikes': 15, 'pants':5}]`
`df = pd.DataFrame(list)`

+列：所有的key
+行：**每个字典对应一行**，但没有行标签，自动生成数字标签(数据就是字典key对应的值)
![image](https://cdn.nlark.com/yuque/0/2020/png/1136179/1591174539201-25eebdc1-48d6-40a2-bf17-ffc967ce5aa3.png)



### 4）用嵌套字典的元组生成

![image](https://cdn.nlark.com/yuque/0/2020/png/1136179/1591174539205-231086a6-1c0e-4e1d-bc76-fbed6aeec069.png)

### 5）其余

`DataFrame.from_dict` 接收字典组成的字典或数组序列字典，并生成 DataFrame
`DataFrame.from_records` 构建器支持元组列表或结构数据类型（dtype）的多维数组





## 2 数据I/O

1. ndarray读写效率最高，但最费硬盘空间，比如`np.load()`, `np.save()`
2. csv其次，比如`pd.Dataframe.to_csv()`，`pd.load_csv()`。

3. txt读写，当然也可以很快，但是**需要频繁的split**，对格式规范的数据比较麻烦。

4. 至于简单的excel和word，可以用xlrd,xlwt来操作

numpy的速度比dataframe快，其次对于txt或者csv的数据，==用map，reduce，filter三个函数，自己写好function往里面套就行，稍微有点麻烦，但最重要的是这样可以**并行化**==，写好的东西可以直接放到map+reduce上跑，尤其是对于数据量比较大的时候。这样的写法另外一个好处是转向pyspark时非常的方便



> CSV (comma-separated values)
>
> ```python
> df.to_csv('data/foo.csv')
> pd.read_csv(filepath)
> ```
>
> 
>
> HDF5：快速
>
> ```python
> df.to_hdf('data/foo.h5', 'df')
> pd.read_hdf('data/foo.h5', 'df')
> ```
>
> 
>
> Excel
>
> ```python
> df.to_excel('data/foo.xlsx', sheet_name='Sheet1')
> pd.read_excel('data/foo.xlsx', 'Sheet1',index_col=None, na_values=['NA'])
> ```
>
> 
>
> .dat文件
>
> ```python
> data = pd.read_table(r'', encoding='gbk')
> ```
>
> 
>
> pickle模块
>
> 持久化：将对象以文件的形式存放在磁盘
>
> ```python
> .to_pickle('student.pickle')
> ```



### ==数据库操作==

```python
import sqlite3
### 从sqlite中读取数据
con = sqlite3.connect('user_information.sqlite')
sql = 'select * from user_information LIMIT 3'
df = pd.read_sql(sql, con)

df = pd.read_sql(sql, con, index_col='id')  #将id列设置为index
df = pd.read_sql(sql, con, index_col=['id', 'bank_id'])  #多个列设置为index

### 删除
con.execute('DROP TABLE IF EXISTS user_information')

### 保存到数据库
pd.io.sql.write_sql(df, 'user_information', con)

### 如果是MySQL
import MySQLdb
con = MySQLdb.connect(host='localhost', db='databasename')
```





# 操作2|查 选/索引 改 增 删



## 1 查

```python
### 属性
.dtypes 数据类型


.shape #(行, 列)
.ndim #秩
.size  #行*列==元素个数
.values  #数据data(Series.values或DataFrame.values)


### 数据表
df
df.head()
df.tail()

df.index
df.columns
df.values #返回ndarray

### 描述性统计
df.count()
df.describe()
df.value_counts()
df.unique()
```



- 查验Null

`empty()`、`any(iterable)`、`all()`、`bool()`把数据汇总简化为布尔值(empty：验证元素是否为空)

`df.isnull()`  返回一个大小和df一样的布尔型DataFrame，结合.sum() `df.isnull().sum().sum()` 第一个 sum()返回一个 Pandas.Series





## 2 选/索引

![image-20201220093133907](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201220093134.png)

在 DataFrame 中，区别于 Python 列表和 Series 对象：

- index 不可用
- index name 指 column index name（使用loc方法可以作用于row index name）`[str]->c`
- index的切片可用，并且作用于row index `[int:int]->r`
- index name的切片可用，作用于row index name `[str:str]->r`
- index的花式索引不可用
- index name的花式索引为column index name的索引 `[[str,str]]->c` 

这样的限制很多，比如我们不能对COLUMN进行组(片)选，不能对ROW进行单、多选，但==用loc和iloc方法可以解决这个问题==。一般而言，这样的使用方式其实不多，对于长格式数据，COLUMN一般为变量/特征，而ROW一般为数据编号。

本质：loc和iloc的两个参数相当于执行完第一个参数后生成temp的DataFrame后进行第二个参数的切片选择



小结：

- 对于 columns，只能使用 `[str], [[str,str]]` 
- 而对于 index，只能使用 `[int:int],[str:str]`

```python
a
    age	    name	qq	tt
0	3	1	2	3
1	4	2	2	3
2	5	3	2	3

a[["age","name"]][0:1] # 不可用 [1] 索引第一行
```



```python
'''
DF还可以接受Series作为DF[Series]这种形式进行选择，在这种情况下，S必须和DF的row index保持一致（即只能用column的Series对row进行筛选）
'''
print(data)
print(data[:2]) # 切片在DF中作用于index，而不是column
print(data['three'] > 5) # 这生成了一个Series
print(data[data['three'] > 5]) 
#索引使用了一个bool型Series对原数组进行选择，很自然的想到是对行进行选择。

#以上所有情况均只能对行/列操作，而不能同时对行和列进行操作
#print(data["one","Colorado"])  
#（虽然后面可以根据bool索引对不符合条件的行列进行排除，但依然无法直接同时选取行与列）

          one  two  three  four
Ohio        0    1      2     3
Colorado    4    5      6     7
Utah        8    9     10    11
New York   12   13     14    15

          one  two  three  four
Ohio        0    1      2     3
Colorado    4    5      6     7

Ohio        False
Colorado     True
Utah         True
New York     True
Name: three, dtype: bool

          one  two  three  four
Colorado    4    5      6     7
Utah        8    9     10    11
New York   12   13     14    15


### 基于loc和iloc
print(data.loc["Ohio","one"]) #注意loc并不是一个函数，是对其直接切片
print(data.loc["Ohio",["one","two"]]) 
# 同行/列的元素使用列表括住，不同的则使用逗号隔开，逗号表示不同维度，不能写成：
#print(data.loc["Ohio","Utah"]) 
# 而应该写成 print(data.loc[["Ohio","Utah"]])
print(data.loc['Colorado', ['two', 'three']])
# print(data.loc['Colorado', [1,2]]) 
# #必须使用iloc在列的基础上对行进行column index而不是column index name的选取
# print(data.loc["one"],data.loc["2"])  不能这样写，虽然之前的ix可以


0
one    0
two    1
Name: Ohio, dtype: int32

two      5
three    6
Name: Colorado, dtype: int32
        
        
        
print(data.iloc[2, [3, 0, 1]])
print(data.iloc[2])
print(data.iloc[[1, 2], [3, 0, 1]])
four    11
one      8
two      9
Name: Utah, dtype: int32

one       8
two       9
three    10
four     11
Name: Utah, dtype: int32

          four  one  two
Colorado     7    4    5
Utah        11    8    9
```







### 2.1 索引(可用切片操作)

注意：索引返回的是 DataFrame 的视图而非副本，使用 copy 命令创造一个副本

DataFrame 含有两个索引轴，默认是 column index

- row index（官方称为index）
- column index（官方称为column）

可以提取多列，提取列之后可以提取行，但是不能直接提取行(使用ix/loc/iloc方法提取行index)

columns 默认不支持列表名称选取后再切片 index(`frame[["name","student_id"]]["A"]`), 仅仅可以提取多个columns的数据: `frame[["name","student_id"]]`

```python
df[Col]
df[Col][Row]
# 得到行数据
df[Label]
df[Slice of Row]  

# 选择行/列
df.loc[Col, Row]
df.loc[:, [columns]]   #标签索引，选择列

df.iloc[Col_i, Row_i]
df.iloc[2,:]  #数值索引，查询第二行

# 选择某个元素
df.at[row, column]  #快速定位DataFrame的元素
df.iat[x,x]  #数值索引，快速定位
.ix[]  #先用loc的方式来索引，索引失败就转成iloc(将被剔除，尽量不要使用)
```




![image](https://cdn.nlark.com/yuque/0/2020/png/1136179/1591174539108-b5e8112b-74f1-4225-b13c-732897e81555.png)



### 2.2 布尔(逻辑判断) => 筛选

- &
- |

```python
indices = np.where(np.isnan(a))  #获取索引值
indices = np.where(pd.isnull(a)) #两者等价
np.where(np.isnan(df))  #返回tuple 第一个ndarray表示行的数值索引，第二个ndarray表示列的数值索引
np.take(a, indices, axis=None, out=None, mode='raise') #提取指定索引位置的数据,并以一维数组或者矩阵返回(主要取决axis)
```

![image](https://cdn.nlark.com/yuque/0/2020/png/1136179/1591174539060-706151af-fb57-4a05-9639-23a1e1f0dcf0.png)

```python
'''
df[BOOL]
df.loc[BOOL]
'''

df.loc[(df['C']>2) & (df['D']<10) ]
# 筛选特定的值
df.loc[df['C'].isin([3,5,6])]
df['D'] = np.where(df.A<3, 1, 0)  #如果A＜3，D为1；如果A≥3，D为0
```







### 2.3 选取部分表格

`pd.DataFrame(df, index = ['glasses', 'bike'], columns = ['Alice'])`



### 2.4 选取**某列**含有特殊数值的行 => 筛选

`df.loc[[index]]`
`df[df.Column > 0]`
`df1[df1['A'].isin([1])]` 选取df1中A列包含数字1的行
`df1=df1[~df1['A'].isin([1])]` 通过~取反，选取不包含数字1的行
`df.[df.index.isin(newindex)]`



### 2.5 选取**某行**含有特殊数值的列 => 筛选

`df[[Columns]]`
`cols=[x for i,x in enumerate(df2.columns) if df2.iat[0,i]==3]`
`df2=df2[cols]`



## 3 改

### `df.T` 转置 => 调换label和index



### 数据精度设置

`pd.set_option('precision', 1)`  小数点后一位



### 修改names

- table name
- index name
- columns name

```python
### series
f_t.name = "季度盈利表"; f_t.columns.name = "产品名";f_t.index.name = "季度";
print(f_t.values);f_t

产品名	lili	marvin
季度		
s1	123.0	NaN
s2	112.0	NaN
s3	998.0	123.0
s4	NaN	300.0
s5	NaN	NaN

### Dataframe
# 注意：如果是多层索引的话，需要调用names而不是name
df.index.name = "xxx" 
df.column.name = "yyy" 
```



### 修改列标签column

`df.columns = ['a', 'b']`



### 修改行标签index⭐

与Series不同，DataFrame 的 Index 不可直接修改替换，并且有多种类型，多种方法可供使用

其不可修改性保证了 index 在不同数组间的传递安全，但是也不容易修改 column 的名称，尽管我们可以在生成 frame 时设置、使用多种处理索引的方法，比如 rename、reset_index、set_index、reindex



`df.reset_index`

```python
### 用于删除/重置某一索引
# 参数drop：默认删除的索引自动变为列
# 参数inplace：默认生成新 DataFrame
# 参数level：针对多级索引，定义需要删除的索引级别
DataFrame.reset_index(level=None, drop=False, inplace=False, col_level=0, col_fill='')

### 补充：直接删除level0的索引，而不用 reset_index(level=0).drop("newcolumnname",axis=1)
```



`df.set_index(Column)`  将某列(的值)作为行标签/索引(常把日期/时间列作为行索引)

```python
### 将Column中的某列改变成为index,作用和 reset_index 正好相反
# keys 指定需要变换的列，可以是多列
# drop 用于处理当前index
DataFrame.set_index(keys, drop=True, append=False, inplace=False, verify_integrity=False)
```



<img src="https://cdn.nlark.com/yuque/0/2020/png/1136179/1591174539082-aca8027f-e6a2-47ae-b3b6-b85fd6cf1242.png" alt="image" style="zoom:67%;" />
`df.index = [index + '_5m' for index in df.index]` 直接修改：问题默认的index是pandas.indexes.base.Index，这个格式可能会默认index里面数据的长度是确定的，导致加_5m后缀失败
关键：修改index的类型list(df.index) list元素可变





### ==万能|修改label或index==

`df = df.rename(columns={'oldName1': 'newName1', 'oldName2': 'newName2'})`

```python
Frame.rename(mapper=None, axis=None, index=None, columns=None, copy=True, inplace=False, level=None)

### 传入 mapper 和 axis 以指定对于哪个轴进行函数映射
# 比如去除空格: 
df.rename(lambda x: x.trim(), axis=1)


df.rename({1: 2, 2: 4}, axis='index')
df.rename(str.lower, axis='columns')
# 可以直接使用mapper方法对索引名称进行操作
df.rename(index=str.lower, columns={"A": "a", "C": "c"})
```



`df.reindex(index=[], columns=[])`

Series的索引可以直接替换，而DF则不可以。

DF和Series使用reindex方法进行索引的重新界定，插入新的行和列、对缺失值进行处理，时间序列填充等，功能更为强大

```python
'''
reindex接受一个list，没有的值默认填充NaN, 你可以指定一个fill_value来覆盖NaN这个值
使用method方法填充时间序列值（只能为轴0也就是横向）
'''
obj = pd.Series([4.5, 7.2, -5.3, 3.6], index=['d', 'b', 'a', 'c']);print(obj)
obj2 = obj.reindex(["a","b","c","d","e"],fill_value=0.0)
obj2
d    4.5
b    7.2
a   -5.3
c    3.6
dtype: float64

a   -5.3
b    7.2
c    3.6
d    4.5
e    0.0  #没有的值默认填充NaN
dtype: float64
```

注意：

- reindex不能更改index的名称
- 最关键的一点是：reindex之后生成新的DataFrame，原始DataFrame不会改变

小结：

- reindex()用来将索引位置替换
- rename()用来映射索引关系，可以在保证索引位置不变的情况下对索引进行更名、应用函数等

和rename()较为相似，但是可以同时处理索引本身并且可以更改索引位置的操作是：使用 `df.column = ["zz","zz","zz"]` 直接替换索引，列表对应当前DF的顺序



### 修改数据类型

```python
df["cat_col"] = pd.Categorical(df["col"])` or `df["cat_col"] = df["col"].astype("category")
```



### 修改数据：替换/新建一整列

```python
a = DataFrame({"name":[1,2,3],"age":[3,4,5]})
a["qq"] = 2 # 广播
a["tt"] = [3,3,3] # 等长
a["rr"] = Series([4,5,6,7],index=[3,1,2,0]) # 自动处理索引映射

   age name qq  tt  rr
0	3	1	2	3	7
1	4	2	2	3	5
2	5	3	2	3	6
```





## 4 增

### 添加标签/索引
`pd.DataFrame(data, index = ['label 1', 'label 2', 'label 3'])`
`index = pd.MultiIndex.from_tuples(tuples, names=['first', 'second'])`  建立行索引
![image](https://cdn.nlark.com/yuque/0/2020/png/1136179/1591174539117-dccca51e-24f7-4f59-a535-bc898c5c0416.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_10%2Ctext_6K-t6ZuALUREdW5jYW4%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)



### 添加新的列
`df[new_column] = df[old_column1] + df[old_column2]`
`dataframe.insert(loc,label,data)`



### 添加新的行
首先创建新的 Dataframe，然后将其附加到原始 DataFrame 上
`pd.DataFrame(new_data, index = ['store 3'])`
新行附加到 DataFrame 后，列按照字母顺序排序





## 5 删

```python
del df[Column]

df2=df2.drop(cols,axis=1)  #利用drop方法将含有特定数值的行/列(指定axis)删除
df.pop(Column)  #仅允许我们删除列
```







# 操作3|遍历 排序 统计



## 1 遍历(迭代)

`items()` (Columns, Series)元组





## 2 排序

如果包含 NaN 值，那么其将会被放在最后，注意，对于任意的 ascending 都是如此



### 按索引排序
`Series.sort_index()`
`DataFrame.sort_index()`

```python
frame = pd.DataFrame(np.arange(8).reshape((2, 4)),
                     index=['three', 'one'],
                     columns=['d', 'a', 'b', 'c'])
print(frame.sort_index()) # axis = 0
print(frame.sort_index(axis=1))
frame.sort_index().sort_index(axis=1,ascending=False) # 行列均排序

       d  a  b  c
one    4  5  6  7
three  0  1  2  3

       a  b  c  d
three  1  2  3  0
one    5  6  7  4

        d   c   b   a
one     4   7   6   5  
three   0   3   2   1
```





### 按值排序

```python
# 参数 by
# 参数 axis
# 参数 ascending
# 参数 inplace
Series.sort_values()
DataFrame.sort_values() #参数 by 用于指定按哪列排序(一列或多列数据)

df.sort_values(by='C')
```





### 按索引与值排序



### 按多重索引的列排序



### 元素大小比较后，返回行列索引|idxmax() idxmin()

```python
day     a       b
01      12      23
02      122     123
03      15      13

idxmax(0) #0表示行
a 01
b 02

idxmax(1) #1表示列
01 b
02 b
03 a
```



### 排序|rank()

```python
# 参数 axis：针对行或列
# 参数 method
# 参数 numeric_only：是否只针对数值
# 参数 ascending 

obj = pd.Series([7, -5, 7, 4, 2, 0, 4])
obj.rank() #rank返回的值为index和排名

obj.rank(method='first')
#first含义为根据首次出现的排名为1，此外还有min，max，average（default）
obj.rank(ascending=False, method='max')
# 最大化排名，如果有两个相等值，则均为最大值。
obj.rank(ascending=False, method='min')
# 最小化排名，如果有两个相等值，则均为最小值。


### DataFrame
# 指定axis:column表示横向，index表示纵向
frame = pd.DataFrame({'b': [4.3, 7, -3, 2], 'a': [0, 1, 0, 1],
                      'c': [-2, 5, 8, -2.5]})
frame.rank(axis='columns')

   a    b    c
0  0  4.3 -2.0
1  1  7.0  5.0
2  0 -3.0  8.0
3  1  2.0 -2.5

	 a	 b	 c
0	2.0	3.0	1.0  # 从小到大排序
1	1.0	3.0	2.0
2	2.0	1.0	3.0
3	2.0	3.0	1.0
```







## 3 统计

```python
### 描述性统计
df.count()
df.describe()
df.value_counts()
df.unique()


### 判断索引是否唯一
obj = pd.Series(range(5), index=['a', 'a', 'b', 'b', 'c'])
obj.index.is_unique # False
```



- 统计学函数
  `df.mean()` 默认针对列
  `df,median()`
  `df.max()`
  `df.std()`
  `df.corr()`  相关系数矩阵
  `df..cov()` 协方差矩阵
  `df.corrwith()`  计算其列或行跟另一个Series或DataFrame之间的相关系数

![image](https://cdn.nlark.com/yuque/0/2020/png/1136179/1591174539244-42c32a64-f5ec-4e8d-b843-0bf378711f0c.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_10%2Ctext_6K-t6ZuALUREdW5jYW4%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)







# 专题|缺失值处理

### 快速检查NaN
`df.isnull().any()`
`np.nanmean(ndarray, axis=0)`  属于Numpy中不包括NaN按axis取平均值(看作没有NaN)



### 对NaN值的处理
```python
# 固定值替代
df.fillna(0)  #数值0替换
df.fillna('missing') #字符串替换

# 临近值替代
df.fillna(method = 'ffill', axis=0) #针对行进行前向填充
df.fillna(method = 'backfill', axis=0)  #针对行进行后向填充 参数limit=1表示限制替代的个数

# 统计值替代
df.fillna(df.mean())
df.fillna(df.mean([Columns]))

# 插值替代
# 如果index是数字。method='values'
# 如果index是时间，method='time'
df.interpolate(method = 'linear', axis)  #linear 插值

# 删除
df.dropna(axis= , inplace=False, how='any')  #删除

# 替换
ser.replace(a, b)
ser.replace(LIST, LIST)
ser.replace(DICT, DICT) #字典映射
df.replace(a, b)
.fill_value() #处理缺失值
```





# 专题|可视化

```python
### 散点图
plt = data.plt(kind='scatter', x='X', y='Y').get_figure()
plt.savefig('')


### 柱状图
# 一般柱状图
plt = data.plot(kind='bar').get_figure()
# 堆叠柱状图@百分比固定
plt = data.plot(kind='bar', stacked=True).get_figure()

### 箱形图：中位数，平均数，四分位数等
data.boxplot()
```





# 对象|字符串数据

```python
### ser.str.
ser.str.lower()
.upper()
.len()

.split()
.get(0)  #第一个字符

.replace(,) #替换字符

### 字符串匹配
pattern = r'[a-z][0-9]'
ser.str.contains(pattern)  #返回True, False, NaN保留
# 参数na：自定义匹配为True或者False
ser.str.contains(pattern, na=False)

# match方法严格匹配
ser.str.match(pattern, na=False)

### 判断开头结尾
ser.str.startswith(char, na=False)
ser.str.endwith(char, na=False)
```







# 对象|分类数据

分类数据

```
df[Column].astype("category")
```

`df[Column].cat.categories = []` 重命名分类名称

`df[Column].cat.categories.cat.set_categories([])` 在重命名的基础上添加新的类别名称



# 对象|时间序列数据

`pd.date_range(, periods=, freq=)` 创建时间序列数据，也可以作为后续的行索引index  freq='S'秒, 'D'天, 'M'月, 'Q-NOV'季度

`df.rolling(N).mean()`  计算 N 天期限的滚动均值

`pd.Series.resample()`  重采样

`pd.Series.tz_localize('UTC')`  时区

`pd.Series.tz_localize('UTC').tz_convert('US/Eastern')`  时区转换

```
pd.Series.to_period()
pd.Series.to_timestamp()
(pd.period_range.asfreq('M', 'end')).asfreq('H', 'start')
```

说明：+9 表示09:00

![image](https://cdn.nlark.com/yuque/0/2020/png/1136179/1591174539132-6fb90ec0-7f67-432e-9004-40a7e4e7b0c2.png)



# 特性|广播机制

运算符 

- 加减乘除add、sub、mul、div
- 通用函数 sum、dot



一个矩阵或者向量减去一个常数，那么通常是矩阵中的每一个元素减去这个常数

- axis = 0 跨行 => 每一列广播
- axis = 1 跨列 => 每一行广播

```python
### Series之间
s1 = Series(np.arange(5),index=list("abcde"));
s2 = Series(np.arange(6),index=list("adcbef"));
s1 + s2;
s3 = s1.add(s2,fill_value=0) #fll_value的意思不是最终将NaN替换为0，而是在进行运算前对于缺失的值设置为0
print(s3)

a    0.0
b    4.0
c    4.0
d    4.0
e    8.0
f    5.0
dtype: float64
    
    
### DF之间

### DF与Series
# 只要 index 对应即可进行，series 按照 index 顺序对 dataframe 进行遍历广播运算
arr = np.arange(12.).reshape((3, 4))
frame = pd.DataFrame(np.arange(12.).reshape((4, 3)),
                     columns=list('bde'),
                     index=['Utah', 'Ohio', 'Texas', 'Oregon'])
```







# 其他|小技巧

### 0-1变量比例统计

`np.sum(df['Column']) /df.shape[0]`



# #References

[Link: DT新纪元|Pandas使用手册](https://zhuanlan.zhihu.com/p/25184830)

[Link: Corkine's BlOG|Python数据处理学习笔记 - Series & Dataframe篇](https://blog.mazhangjing.com/2018/02/25/learn_pandas/)







