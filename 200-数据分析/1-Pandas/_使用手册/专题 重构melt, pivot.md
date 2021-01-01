> ```python
> pd.melt(df, id_vars=None, value_vars=None, var_name=None, value_name='value', col_level=None)
> ```
>
> **说明**：将目标列“融化”，或**合并**，或**拆解**，形成新的特征列(适合单个个体多属性：统一只罗列一个属性)
>
> +df:要处理的数据集。
>
> +id_vars:不需要被转换的列名。
>
> +value_vars:需要转换的列名，如果剩下的列全部都要转换，就不用写了。
>
> +var_name和value_name是自定义设置对应的列名
>
> +col_level :如果列是MultiIndex，则使用此级别



```python
@示例：type1和type2合并成一个类型
# 新的列名type_level，相应的值作为新的列type
# 要改变的列
type_cols = ['type_1','type_2']
#不改变的列
non_type_cols = pokemon.columns.difference(type_cols)
pkmn_types = pokemon.melt(id_vars = non_type_cols, value_vars = type_cols, 
                          var_name = 'type_level', value_name = 'type').dropna()

pkmn_types.head()
```



![image](https://cdn.nlark.com/yuque/0/2020/png/1136179/1591174539172-06e3b9c2-dd1f-4ecd-8fd7-545d399eb0ac.png)

---



# pivot|index+columns+values

![image-20201216103216807](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201216103217.png)

## df的标准格式

```python
import pandas._testing as tm

def unpivot(frame):
    N, K = frame.shape
    data = {'value': frame.to_numpy().ravel('F'),  #展开为一位数组
            'variable': np.asarray(frame.columns).repeat(N),  #单个重复
            'date': np.tile(np.asarray(frame.index), K)}   #循环重复
    return pd.DataFrame(data, columns=['date', 'variable', 'value'])

df = unpivot(tm.makeTimeDataFrame(3))


In [1]: df
Out[1]: 
         date variable     value
0  2000-01-03        A  0.469112
1  2000-01-04        A -0.282863
2  2000-01-05        A -1.509059
3  2000-01-03        B -1.135632
4  2000-01-04        B  1.212112
5  2000-01-05        B -0.173215
6  2000-01-03        C  0.119209
7  2000-01-04        C -1.044236
8  2000-01-05        C -0.861849
9  2000-01-03        D -2.104569
10 2000-01-04        D -0.494929
11 2000-01-05        D  1.071804
```

tm.makeTimeDataFrame(3)：

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201216105700.png" alt="image-20201216105700540" style="zoom:80%;" />

```python
In [3]: df.pivot(index='date', columns='variable', values='value')
Out[3]: 
variable           A         B         C         D
date                                              
2000-01-03  0.469112 -1.135632  0.119209 -2.104569
2000-01-04 -0.282863  1.212112 -1.044236 -0.494929
2000-01-05 -1.509059 -0.173215 -0.861849  1.071804


### df['value']是一整张表
In [4]: df['value2'] = df['value'] * 2
In [5]: pivoted = df.pivot(index='date', columns='variable')
    
In [6]: pivoted
Out[6]: 
               value                                  value2                              
variable           A         B         C         D         A         B         C         D
date                                                                                      
2000-01-03  0.469112 -1.135632  0.119209 -2.104569  0.938225 -2.271265  0.238417 -4.209138
2000-01-04 -0.282863  1.212112 -1.044236 -0.494929 -0.565727  2.424224 -2.088472 -0.989859
2000-01-05 -1.509059 -0.173215 -0.861849  1.071804 -3.018117 -0.346429 -1.723698  2.143608
```

备注：pivot的index必须是unique

- 否则可以考虑使用pivot_table()



# (un)stack

- 同时适用于Series和DataFrame

- MultiIndex对象



## stack

针对column labels的一个level(一个level下可能有多个类别：整合到一列中)

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201216152204.png" alt="image-20201216152204611" style="zoom:67%;" />



## unstack

stack的逆操作

针对row index => column axis 

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201216152336.png" alt="image-20201216152336776" style="zoom:67%;" />

```python
### 生成元组的列表
# 生成元组zip(*[[],[]])
In [8]: tuples = list(zip(*[['bar', 'bar', 'baz', 'baz',
   ...:                      'foo', 'foo', 'qux', 'qux'],
   ...:                     ['one', 'two', 'one', 'two',
   ...:                      'one', 'two', 'one', 'two']]))
   ...: 

In [9]: index = pd.MultiIndex.from_tuples(tuples, names=['first', 'second'])

In [10]: df = pd.DataFrame(np.random.randn(8, 2), index=index, columns=['A', 'B'])

In [11]: df2 = df[:4]

In [12]: df2
Out[12]: 
                     A         B
first second                    
bar   one     0.721555 -0.706771
      two    -1.039575  0.271860
baz   one    -0.424972  0.567020
      two     0.276232 -1.087401
    
In [13]: stacked = df2.stack()  #默认是last level .stack(-1)
In [14]: stacked
Out[14]: 
first  second   
bar    one     A    0.721555
               B   -0.706771
       two     A   -1.039575
               B    0.271860
baz    one     A   -0.424972
               B    0.567020
       two     A    0.276232
               B   -1.087401
dtype: float64
```

unstack(1)/unstack('second') ：

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201216154131.png" alt="image-20201216154131686" style="zoom:67%;" />

unstack(0)/unstack('first') ：

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201216154208.png" alt="image-20201216154208101" style="zoom:67%;" />



注意：默认包含对index的排序

```python
In [19]: index = pd.MultiIndex.from_product([[2, 1], ['a', 'b']])

In [20]: df = pd.DataFrame(np.random.randn(4), index=index, columns=['A'])

In [21]: df
Out[21]: 
            A
2 a -0.370647
  b -1.157892
1 a -1.344312
  b  0.844885

In [22]: all(df.unstack().stack() == df.sort_index())
Out[22]: True
```



## Multiple levels|复习MultiIndex

传递一个列表的levels

```python
# names指的是这些levels的名称
In [23]: columns = pd.MultiIndex.from_tuples([
   ....:     ('A', 'cat', 'long'), ('B', 'cat', 'long'),
   ....:     ('A', 'dog', 'short'), ('B', 'dog', 'short')],
   ....:     names=['exp', 'animal', 'hair_length']
   ....: )
   ....: 

In [24]: df = pd.DataFrame(np.random.randn(4, 4), columns=columns)

In [25]: df
Out[25]: 
exp                 A         B         A         B
animal            cat       cat       dog       dog
hair_length      long      long     short     short
0            1.075770 -0.109050  1.643563 -1.469388
1            0.357021 -0.674600 -1.776904 -0.968914
2           -1.294524  0.413738  0.276662 -0.472035
3           -0.013960 -0.362543 -0.006154 -0.923061

In [26]: df.stack(level=['animal', 'hair_length'])
Out[26]: 
exp                          A         B
  animal hair_length                    
0 cat    long         1.075770 -0.109050
  dog    short        1.643563 -1.469388
1 cat    long         0.357021 -0.674600
  dog    short       -1.776904 -0.968914
2 cat    long        -1.294524  0.413738
  dog    short        0.276662 -0.472035
3 cat    long        -0.013960 -0.362543
  dog    short       -0.006154 -0.923061
    
# levels也可以是数值
# 等价于df.stack(level=[1, 2])
```



```python
In [28]: columns = pd.MultiIndex.from_tuples([('A', 'cat'), ('B', 'dog'),
   ....:                                      ('B', 'cat'), ('A', 'dog')],
   ....:                                     names=['exp', 'animal'])
   ....: 

In [29]: index = pd.MultiIndex.from_product([('bar', 'baz', 'foo', 'qux'),
   ....:                                     ('one', 'two')],
   ....:                                    names=['first', 'second'])
   ....: 

# 注意：index和columns分别用.MultiIndex.来定义
In [30]: df = pd.DataFrame(np.random.randn(8, 4), index=index, columns=columns)

In [31]: df2 = df.iloc[[0, 1, 2, 4, 5, 7]]  #原本有8行，每一个first的index对应2个second的Index

In [32]: df2
Out[32]: 
exp                  A         B                   A
animal             cat       dog       cat       dog
first second                                        
bar   one     0.895717  0.805244 -1.206412  2.565646
      two     1.431256  1.340309 -1.170299 -0.226169
baz   one     0.410835  0.813850  0.132003 -0.827317
foo   one    -1.413681  1.607920  1.024180  0.569605
      two     0.875906 -2.211372  0.974466 -2.006747
qux   two    -1.226825  0.769804 -1.281247 -0.727707

In [33]: df2.stack('exp')
Out[33]: 
animal                 cat       dog
first second exp                    
bar   one    A    0.895717  2.565646
             B   -1.206412  0.805244
      two    A    1.431256 -0.226169
             B   -1.170299  1.340309
baz   one    A    0.410835 -0.827317
             B    0.132003  0.813850
foo   one    A   -1.413681  0.569605
             B    1.024180  1.607920
      two    A    0.875906 -2.006747
             B    0.974466 -2.211372
qux   two    A   -1.226825 -0.727707
             B   -1.281247  0.769804
    
In [34]: df2.stack('animal')
Out[34]: 
exp                         A         B
first second animal                    
bar   one    cat     0.895717 -1.206412
             dog     2.565646  0.805244
      two    cat     1.431256 -1.170299
             dog    -0.226169  1.340309
baz   one    cat     0.410835  0.132003
             dog    -0.827317  0.813850
foo   one    cat    -1.413681  1.024180
             dog     0.569605  1.607920
      two    cat     0.875906  0.974466
             dog    -2.006747 -2.211372
qux   two    cat    -1.226825 -1.281247
             dog    -0.727707  0.769804
```



## 缺失值

- NaN对应float
- NaT对应datetimelike
- 整型自动转化为float => NaN

```python
In [35]: df3 = df.iloc[[0, 1, 4, 7], [1, 2]]

In [36]: df3
Out[36]: 
exp                  B          
animal             dog       cat
first second                    
bar   one     0.805244 -1.206412
      two     1.340309 -1.170299
foo   one     1.607920  1.024180
qux   two     0.769804 -1.281247

In [37]: df3.unstack()
Out[37]: 
exp            B                              
animal       dog                 cat          
second       one       two       one       two
first                                         
bar     0.805244  1.340309 -1.206412 -1.170299
foo     1.607920       NaN  1.024180       NaN
qux          NaN  0.769804       NaN -1.281247

# df3.unstack(fill_value=-1e9)
```



## 注意：MultiIndex

```python
In [39]: df[:3].unstack(0)
Out[39]: 
exp            A                   B                                      A          
animal       cat                 dog                cat                 dog          
first        bar       baz       bar      baz       bar       baz       bar       baz
second                                                                               
one     0.895717  0.410835  0.805244  0.81385 -1.206412  0.132003  2.565646 -0.827317
two     1.431256       NaN  1.340309      NaN -1.170299       NaN -0.226169       NaN

In [40]: df2.unstack(1)
Out[40]: 
exp            A                   B                                       A          
animal       cat                 dog                 cat                 dog          
second       one       two       one       two       one       two       one       two
first                                                                                 
bar     0.895717  1.431256  0.805244  1.340309 -1.206412 -1.170299  2.565646 -0.226169
baz     0.410835       NaN  0.813850       NaN  0.132003       NaN -0.827317       NaN
foo    -1.413681  0.875906  1.607920 -2.211372  1.024180  0.974466  0.569605 -2.006747
qux          NaN -1.226825       NaN  0.769804       NaN -1.281247       NaN -0.727707
```





# melt|类似stack

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201216205931.png" alt="image-20201216205930835" style="zoom:67%;" />

- 一列或多列作为identifier variables标识变量
  - first
  - last
- 其他列是measured variables
  - variable + value
  - 可以自定义这些列名
    - 参数：var_name，value_name



注意：

- index会被忽略
  - 除非ignore_index=False：保留原始的index



## .wide_to_long()

缺少灵活性，但更加user-friendly

```python
In [50]: dft = pd.DataFrame({"A1970": {0: "a", 1: "b", 2: "c"},
   ....:                     "A1980": {0: "d", 1: "e", 2: "f"},
   ....:                     "B1970": {0: 2.5, 1: 1.2, 2: .7},
   ....:                     "B1980": {0: 3.2, 1: 1.3, 2: .1},
   ....:                     "X": dict(zip(range(3), np.random.randn(3)))
   ....:                    })
   ....: 

In [51]: dft["id"] = dft.index  #复制index的值

In [52]: dft
Out[52]: 
  A1970 A1980  B1970  B1980         X  id
0     a     d    2.5    3.2 -0.121306   0
1     b     e    1.2    1.3 -0.097883   1
2     c     f    0.7    0.1  0.695775   2

In [53]: pd.wide_to_long(dft, ["A", "B"], i="id", j="year")
Out[53]: 
                X  A    B
id year                  
0  1970 -0.121306  a  2.5
1  1970 -0.097883  b  1.2
2  1970  0.695775  c  0.7
0  1980 -0.121306  d  3.2
1  1980 -0.097883  e  1.3
2  1980  0.695775  f  0.1
```



# 应用|结合GroupBy + 统计函数⭐

```python
DataFrame.groupby(by=None, axis=0, level=None, as_index=True, sort=True, group_keys=True, squeeze=<object object>, observed=False, dropna=True)
```

- level基于axis：可以是index，也可以是columns
  - 默认axis=0，表示index(跨行)
  - axis=1，表示columns

```python
In [54]: df  #In[20]-In[30]
Out[54]: 
exp                  A         B                   A
animal             cat       dog       cat       dog
first second                                        
bar   one     0.895717  0.805244 -1.206412  2.565646
      two     1.431256  1.340309 -1.170299 -0.226169
baz   one     0.410835  0.813850  0.132003 -0.827317
      two    -0.076467 -1.187678  1.130127 -1.436737
foo   one    -1.413681  1.607920  1.024180  0.569605
      two     0.875906 -2.211372  0.974466 -2.006747
qux   one    -0.410001 -0.078638  0.545952 -1.219217
      two    -1.226825  0.769804 -1.281247 -0.727707

### 补充 df.stack().mean(1)
first  second  animal
bar    one     cat       0.421958
               dog       0.886310
       two     cat      -0.158674
               dog      -0.077934
baz    one     cat      -0.187518
               dog      -0.840379
       two     cat       0.434360
               dog       0.567255
foo    one     cat       0.340021
               dog       0.692635
       two     cat      -0.243923
               dog      -0.205472
qux    one     cat       1.261984
               dog       0.718913
       two     cat      -0.696334
               dog      -0.065373
dtype: float64
    
In [55]: df.stack().mean(1).unstack()  #mean(1)表示跨列：每一行取均值
Out[55]: 
animal             cat       dog
first second                    
bar   one    -0.155347  1.685445
      two     0.130479  0.557070
baz   one     0.271419 -0.006733
      two     0.526830 -1.312207
foo   one    -0.194750  1.088763
      two     0.925186 -2.109060
qux   one     0.067976 -0.648927
      two    -1.254036  0.021048
    
    
# same result, another way
In [56]: df.groupby(level=1, axis=1).mean() #axis=1表示跨列：每一行取均值
Out[56]: 
animal             cat       dog
first second                    
bar   one    -0.155347  1.685445
      two     0.130479  0.557070
baz   one     0.271419 -0.006733
      two     0.526830 -1.312207
foo   one    -0.194750  1.088763
      two     0.925186 -2.109060
qux   one     0.067976 -0.648927
      two    -1.254036  0.021048

In [57]: df.stack().groupby(level=1).mean()
Out[57]: 
exp            A         B
second                    
one     0.071448  0.455513
two    -0.424186 -0.204486

# df.mean()：针对列
exp  animal
A    cat      -0.231657
B    dog      -0.153584
     cat       0.524626
A    dog       0.572572
dtype: float64

In [58]: df.mean().unstack(0)  #unstack()：row => column
Out[58]: 
exp            A         B
animal                    
cat     0.060843  0.018596
dog    -0.413580  0.232430
```

df.stack()：

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201216212204.png" alt="image-20201216212204175" style="zoom:80%;" />



# pivot_table()

参数：

- `data`:  DataFrame.
- `values`: a column or a list of columns to aggregate.
- `index`: a column, Grouper, array which has the same length as data, or list of them. Keys to group by on the pivot table index. If an array is passed, it is being used as the same manner as column values.
- `columns`: a column, Grouper, array which has the same length as data, or list of them. Keys to group by on the pivot table column. If an array is passed, it is being used as the same manner as column values.
- `aggfunc`: function to use for aggregation, defaulting to `numpy.mean`.

```python
In [59]: import datetime

In [60]: df = pd.DataFrame({'A': ['one', 'one', 'two', 'three'] * 6,
   ....:                    'B': ['A', 'B', 'C'] * 8,
   ....:                    'C': ['foo', 'foo', 'foo', 'bar', 'bar', 'bar'] * 4,
   ....:                    'D': np.random.randn(24),
   ....:                    'E': np.random.randn(24),
   ....:                    'F': [datetime.datetime(2013, i, 1) for i in range(1, 13)]
   ....:                    + [datetime.datetime(2013, i, 15) for i in range(1, 13)]})
   ....: 

In [61]: df
Out[61]: 
        A  B    C         D         E          F
0     one  A  foo  0.341734 -0.317441 2013-01-01
1     one  B  foo  0.959726 -1.236269 2013-02-01
2     two  C  foo -1.110336  0.896171 2013-03-01
3   three  A  bar -0.619976 -0.487602 2013-04-01
4     one  B  bar  0.149748 -0.082240 2013-05-01
..    ... ..  ...       ...       ...        ...
19  three  B  foo  0.690579 -2.213588 2013-08-15
20    one  C  foo  0.995761  1.063327 2013-09-15
21    one  A  bar  2.396780  1.266143 2013-10-15
22    two  B  bar  0.014871  0.299368 2013-11-15
23  three  C  bar  3.357427 -0.863838 2013-12-15

[24 rows x 6 columns]


### pivot tables 
In [62]: pd.pivot_table(df, values='D', index=['A', 'B'], columns=['C'])
Out[62]: 
C             bar       foo
A     B                    
one   A  1.120915 -0.514058
      B -0.338421  0.002759
      C -0.538846  0.699535
three A -1.181568       NaN
      B       NaN  0.433512
      C  0.588783       NaN
two   A       NaN  1.000985
      B  0.158248       NaN
      C       NaN  0.176180
        
        
In [63]: pd.pivot_table(df, values='D', index=['B'], columns=['A', 'C'], aggfunc=np.sum)
Out[63]: 
A       one               three                 two          
C       bar       foo       bar       foo       bar       foo
B                                                            
A  2.241830 -1.028115 -2.363137       NaN       NaN  2.001971
B -0.676843  0.005518       NaN  0.867024  0.316495       NaN
C -1.077692  1.399070  1.177566       NaN       NaN  0.352360

In [64]: pd.pivot_table(df, values=['D', 'E'], index=['B'], columns=['A', 'C'],
   ....:                aggfunc=np.sum)
   ....: 
Out[64]: 
          D                                                           E                                                  
A       one               three                 two                 one               three                 two          
C       bar       foo       bar       foo       bar       foo       bar       foo       bar       foo       bar       foo
B                                                                                                                        
A  2.241830 -1.028115 -2.363137       NaN       NaN  2.001971  2.786113 -0.043211  1.922577       NaN       NaN  0.128491
B -0.676843  0.005518       NaN  0.867024  0.316495       NaN  1.368280 -1.103384       NaN -2.128743 -0.194294       NaN
C -1.077692  1.399070  1.177566       NaN       NaN  0.352360 -1.976883  1.495717 -0.263660       NaN       NaN  0.872482
```

备注：

- 结果默认层级index：不论是rows还是columns
- 如果values没有指定：那么默认除了index和columns之外的所有列(能进行aggregated)



```python
### 参数：基于pd.Grouper的index
In [66]: pd.pivot_table(df, values='D', index=pd.Grouper(freq='M', key='F'),
   ....:                columns='C')
   ....: 
Out[66]: 
C                bar       foo
F                             
2013-01-31       NaN -0.514058
2013-02-28       NaN  0.002759
2013-03-31       NaN  0.176180
2013-04-30 -1.181568       NaN
2013-05-31 -0.338421       NaN
2013-06-30 -0.538846       NaN
2013-07-31       NaN  1.000985
2013-08-31       NaN  0.433512
2013-09-30       NaN  0.699535
2013-10-31  1.120915       NaN
2013-11-30  0.158248       NaN
2013-12-31  0.588783       NaN


In [67]: table = pd.pivot_table(df, index=['A', 'B'], columns=['C']) #也可用df.pivot_table()
In [68]: print(table.to_string(na_rep=''))  #NaN为空字符串
                D                   E          
C             bar       foo       bar       foo
A     B                                        
one   A  1.120915 -0.514058  1.393057 -0.021605
      B -0.338421  0.002759  0.684140 -0.551692
      C -0.538846  0.699535 -0.988442  0.747859
three A -1.181568            0.961289          
      B            0.433512           -1.064372
      C  0.588783           -0.131830          
two   A            1.000985            0.064245
      B  0.158248           -0.097147          
      C            0.176180            0.436241
```



```python
### 参数：margins=True
# 注意D列中All的值是bar和foo的平均值
In [69]: df.pivot_table(index=['A', 'B'], columns='C', margins=True, aggfunc=np.std)
Out[69]: 
                D                             E                    
C             bar       foo       All       bar       foo       All
A     B                                                            
one   A  1.804346  1.210272  1.569879  0.179483  0.418374  0.858005
      B  0.690376  1.353355  0.898998  1.083825  0.968138  1.101401
      C  0.273641  0.418926  0.771139  1.689271  0.446140  1.422136
three A  0.794212       NaN  0.794212  2.049040       NaN  2.049040
      B       NaN  0.363548  0.363548       NaN  1.625237  1.625237
      C  3.915454       NaN  3.915454  1.035215       NaN  1.035215
two   A       NaN  0.442998  0.442998       NaN  0.447104  0.447104
      B  0.202765       NaN  0.202765  0.560757       NaN  0.560757
      C       NaN  1.819408  1.819408       NaN  0.650439  0.650439
All      1.556686  0.952552  1.246608  1.250924  0.899904  1.059389
```



# 交叉表 crosstab()

参数：

- `index`: array-like, values to group by in the rows.
- `columns`: array-like, values to group by in the columns.
- `values`: array-like, optional, array of values to aggregate according to the factors.
- `aggfunc`: function, optional, If no values array is passed, computes a frequency table.
- `rownames`: sequence, default `None`, must match number of row arrays passed.
- `colnames`: sequence, default `None`, if passed, must match number of column arrays passed.
- `margins`: boolean, default `False`, Add row/column margins (subtotals)
- `normalize`: boolean, {‘all’, ‘index’, ‘columns’}, or {0,1}, default `False`. Normalize by dividing all values by the sum of values.

备注：除非为交叉列表指定行名或列名，否则任何传递的Series都将使用其name属性

```python
In [70]: foo, bar, dull, shiny, one, two = 'foo', 'bar', 'dull', 'shiny', 'one', 'two'

In [71]: a = np.array([foo, foo, bar, bar, foo, foo], dtype=object)

In [72]: b = np.array([one, one, two, one, two, one], dtype=object)

In [73]: c = np.array([dull, dull, shiny, dull, dull, shiny], dtype=object)

In [74]: pd.crosstab(a, [b, c], rownames=['a'], colnames=['b', 'c'])
Out[74]: 
b    one        two      
c   dull shiny dull shiny
a                        
bar    1     0    0     1
foo    2     1    1     0

### 也适用于Categorical对象
In [78]: foo = pd.Categorical(['a', 'b'], categories=['a', 'b', 'c'])

In [79]: bar = pd.Categorical(['d', 'e'], categories=['d', 'e', 'f'])

In [80]: pd.crosstab(foo, bar)
Out[80]: 
col_0  d  e
row_0      
a      1  0
b      0  1


# 如果想要包含所有数据类别：包括计数为0 => dropna=False
In [81]: pd.crosstab(foo, bar, dropna=False)
Out[81]: 
col_0  d  e  f
row_0         
a      1  0  0
b      0  1  0
c      0  0  0
```



```python
### 参数normalize
In [82]: pd.crosstab(df['A'], df['B'], normalize=True)
Out[82]: 
B    3    4
A          
1  0.2  0.0
2  0.2  0.6

# 针对行或列
In [83]: pd.crosstab(df['A'], df['B'], normalize='columns')
Out[83]: 
B    3    4
A          
1  0.5  0.0
2  0.5  1.0

# aggfunc作用与第三个Series(即values),该Seires受到前两个Series groupby的约束
In [84]: pd.crosstab(df['A'], df['B'], values=df['C'], aggfunc=np.sum)
Out[84]: 
B    3    4
A          
1  1.0  NaN
2  1.0  2.0
```



```python
### 参数margins
In [85]: pd.crosstab(df['A'], df['B'], values=df['C'], aggfunc=np.sum, normalize=True,
   ....:             margins=True)
   ....: 
Out[85]: 
B       3    4   All
A                   
1    0.25  0.0  0.25
2    0.25  0.5  0.75
All  0.50  0.5  1.00
```





# 分组cut|直方图：连续值 => 离散分类

```python
In [86]: ages = np.array([10, 15, 13, 12, 23, 25, 28, 59, 60])

In [87]: pd.cut(ages, bins=3)
Out[87]: 
[(9.95, 26.667], (9.95, 26.667], (9.95, 26.667], (9.95, 26.667], (9.95, 26.667], (9.95, 26.667], (26.667, 43.333], (43.333, 60.0], (43.333, 60.0]]
Categories (3, interval[float64]): [(9.95, 26.667] < (26.667, 43.333] < (43.333, 60.0]]
                                                                         
# 自定义bins的边界
In [88]: c = pd.cut(ages, bins=[0, 18, 35, 70])

In [89]: c
Out[89]: 
[(0, 18], (0, 18], (0, 18], (0, 18], (18, 35], (18, 35], (18, 35], (35, 70], (35, 70]]
Categories (3, interval[int64]): [(0, 18] < (18, 35] < (35, 70]]  
                                                        
### c.categories
IntervalIndex([(0, 18], (18, 35], (35, 70]],
              closed='right',
              dtype='interval[int64]')
                         
### 应用：判断数值的分组
pd.cut([25, 20, 50], bins=c.categories)
# result
[(18, 35], (18, 35], (35, 70]]
Categories (3, interval[int64]): [(0, 18] < (18, 35] < (35, 70]]
```



# 计算指示/虚拟变量indicator / dummy variables⭐

## .get_dummies()

```python
In [90]: df = pd.DataFrame({'key': list('bbacab'), 'data1': range(6)})

In [91]: pd.get_dummies(df['key'])
Out[91]: 
   a  b  c
0  0  1  0
1  0  1  0
2  1  0  0
3  0  0  1
4  1  0  0
5  0  1  0

# 为列名加上前缀 参数：prefix='key'

### 结合pd.cut
In [95]: values = np.random.randn(10)

In [96]: values
Out[96]: 
array([ 0.4082, -1.0481, -0.0257, -0.9884,  0.0941,  1.2627,  1.29  ,
        0.0824, -0.0558,  0.5366])

# 设置bins
In [97]: bins = [0, 0.2, 0.4, 0.6, 0.8, 1]

In [98]: pd.get_dummies(pd.cut(values, bins))
Out[98]: 
   (0.0, 0.2]  (0.2, 0.4]  (0.4, 0.6]  (0.6, 0.8]  (0.8, 1.0]
0           0           0           1           0           0
1           0           0           0           0           0
2           0           0           0           0           0
3           0           0           0           0           0
4           1           0           0           0           0
5           0           0           0           0           0
6           0           0           0           0           0
7           1           0           0           0           0
8           0           0           0           0           0
9           0           0           1           0           0
```

备注：

- Series.str.get_dummies()
- 也可传递DataFrame给.get_dummies()
  - 针对object或categorical dtype

```python
In [99]: df = pd.DataFrame({'A': ['a', 'b', 'a'], 'B': ['c', 'c', 'b'],
   ....:                    'C': [1, 2, 3]})
   ....: 

In [100]: pd.get_dummies(df)
Out[100]: 
   C  A_a  A_b  B_b  B_c
0  1    1    0    0    1
1  2    0    1    0    1
2  3    1    0    1    0

# 只针对特定的列进行.get_dummies()
In [101]: pd.get_dummies(df, columns=['A'])
Out[101]: 
   B  C  A_a  A_b
0  c  1    1    0
1  c  2    0    1
2  b  3    1    0
```



```python
### 参数：prefix
### 参数：prefix_sep
'''
1. string: 针对待编码的列，使用相同的prefix or prefix_sep
2. list：注意长度与待编码列的数量一致
3. dict：列名映射为prefix
'''

# 1 string：前缀
In [102]: simple = pd.get_dummies(df, prefix='new_prefix')

In [103]: simple
Out[103]: 
   C  new_prefix_a  new_prefix_b  new_prefix_b  new_prefix_c
0  1             1             0             0             1
1  2             0             1             0             1
2  3             1             0             1             0

# 2 list
In [104]: from_list = pd.get_dummies(df, prefix=['from_A', 'from_B'])

In [105]: from_list
Out[105]: 
   C  from_A_a  from_A_b  from_B_b  from_B_c
0  1         1         0         0         1
1  2         0         1         0         1
2  3         1         0         1         0


# 3 dict
In [106]: from_dict = pd.get_dummies(df, prefix={'B': 'from_B', 'A': 'from_A'})

In [107]: from_dict
Out[107]: 
   C  from_A_a  from_A_b  from_B_b  from_B_c
0  1         1         0         0         1
1  2         0         1         0         1
2  3         1         0         1         0
```



```python
### 参数drop_first：保留k-1个，避免collinearity共线性 
In [108]: s = pd.Series(list('abcaa'))

In [109]: pd.get_dummies(s)
Out[109]: 
   a  b  c
0  1  0  0
1  0  1  0
2  0  0  1
3  1  0  0
4  1  0  0

In [110]: pd.get_dummies(s, drop_first=True)
Out[110]: 
   b  c
0  0  0
1  1  0
2  0  1
3  0  0
4  0  0

# 如果一列只有一个值，会被忽略(因为值是单一的，换句话说，是确定的)
In [111]: df = pd.DataFrame({'A': list('aaaaa'), 'B': list('ababc')})

In [112]: pd.get_dummies(df)
Out[112]: 
   A_a  B_a  B_b  B_c
0    1    1    0    0
1    1    0    1    0
2    1    1    0    0
3    1    0    1    0
4    1    0    0    1

In [113]: pd.get_dummies(df, drop_first=True)
Out[113]: 
   B_b  B_c
0    0    0
1    1    0
2    0    0
3    1    0
4    0    1
```

备注：默认情况下，新列将具有np.uint8的数据类型

```python
### 数据类型
In [114]: df = pd.DataFrame({'A': list('abc'), 'B': [1.1, 2.2, 3.3]})

In [115]: pd.get_dummies(df, dtype=bool).dtypes
Out[115]: 
B      float64
A_a       bool
A_b       bool
A_c       bool
dtype: object
```



# 分解值Factorizing values

将一维值编码成枚举类型

```python
In [116]: x = pd.Series(['A', 'A', np.nan, 'B', 3.14, np.inf])

In [117]: x
Out[117]: 
0       A
1       A
2     NaN
3       B
4    3.14
5     inf
dtype: object

In [118]: labels, uniques = pd.factorize(x)

In [119]: labels
Out[119]: array([ 0,  0, -1,  1,  2,  3])

In [120]: uniques
Out[120]: Index(['A', 'B', 3.14, inf], dtype='object')
    
# 参数sort
In [1]: x = pd.Series(['A', 'A', np.nan, 'B', 3.14, np.inf])
In [2]: pd.factorize(x, sort=True)
Out[2]:
(array([ 2,  2, -1,  3,  0,  1]),
 Index([3.14, inf, 'A', 'B'], dtype='object'))


### np.unique()
In [3]: np.unique(x, return_inverse=True)[::-1]
Out[3]: (array([3, 3, 0, 4, 1, 2]), array([nan, 3.14, inf, 'A', 'B'], dtype=object))
```

返回值是一个tuple（元组）：

- 第一个元素是一个array，其中的元素是标称型元素映射为的数字
  - 'A'标记为0
  - NaN标记为-1
  - 依次类推……
- 第二个元素是Index类型，其中的元素是所有标称型元素，没有重复。

备注：factorize类似np.unique，但是处理NaN时有区别



# Examples

```python
### 细节
In [121]: np.random.seed([3, 1415])

In [122]: n = 20

In [123]: cols = np.array(['key', 'row', 'item', 'col'])

# //：表示除法，取整数部分
# +：广播机制
In [124]: df = cols + pd.DataFrame((np.random.randint(5, size=(n, 4))
   .....:                          // [2, 1, 2, 1]).astype(str))
   .....: 

In [125]: df.columns = cols

In [126]: df = df.join(pd.DataFrame(np.random.rand(n, 2).round(2)).add_prefix('val'))

In [127]: df
Out[127]: 
     key   row   item   col  val0  val1
0   key0  row3  item1  col3  0.81  0.04
1   key1  row2  item1  col2  0.44  0.07
2   key1  row0  item1  col0  0.77  0.01
3   key0  row4  item0  col2  0.15  0.59
4   key1  row0  item2  col1  0.81  0.64
..   ...   ...    ...   ...   ...   ...
15  key0  row3  item1  col1  0.31  0.23
16  key0  row0  item2  col3  0.86  0.01
17  key0  row4  item0  col3  0.64  0.21
18  key2  row2  item2  col0  0.13  0.45
19  key0  row2  item0  col4  0.37  0.70

[20 rows x 6 columns]
```



```python
### pivot 单个聚合函数
In [128]: df.pivot_table(
   .....:     values='val0', index='row', columns='col', aggfunc='mean')
   .....: 
Out[128]: 
col   col0   col1   col2   col3  col4
row                                  
row0  0.77  0.605    NaN  0.860  0.65
row2  0.13    NaN  0.395  0.500  0.25
row3   NaN  0.310    NaN  0.545   NaN
row4   NaN  0.100  0.395  0.760  0.24

# 参数：fill_value
# 参数：aggfunc='sum'，aggfunc='size'


### pivot 多个聚合函数
# aggfunc=['mean', 'sum']
# 针对单个values
In [132]: df.pivot_table(
   .....:     values='val0', index='row', columns='col', aggfunc=['mean', 'sum'])
   .....: 
Out[132]: 
      mean                              sum                        
col   col0   col1   col2   col3  col4  col0  col1  col2  col3  col4
row                                                                
row0  0.77  0.605    NaN  0.860  0.65  0.77  1.21   NaN  0.86  0.65
row2  0.13    NaN  0.395  0.500  0.25  0.13   NaN  0.79  0.50  0.50
row3   NaN  0.310    NaN  0.545   NaN   NaN  0.31   NaN  1.09   NaN
row4   NaN  0.100  0.395  0.760  0.24   NaN  0.10  0.79  1.52  0.24


# 针对多个values
In [133]: df.pivot_table(
   .....:     values=['val0', 'val1'], index='row', columns='col', aggfunc=['mean'])
   .....: 
Out[133]: 
      mean                                                           
      val0                             val1                          
col   col0   col1   col2   col3  col4  col0   col1  col2   col3  col4
row                                                                  
row0  0.77  0.605    NaN  0.860  0.65  0.01  0.745   NaN  0.010  0.02
row2  0.13    NaN  0.395  0.500  0.25  0.45    NaN  0.34  0.440  0.79
row3   NaN  0.310    NaN  0.545   NaN   NaN  0.230   NaN  0.075   NaN
row4   NaN  0.100  0.395  0.760  0.24   NaN  0.070  0.42  0.300  0.46


# 针对多个columns
In [134]: df.pivot_table(
   .....:     values=['val0'], index='row', columns=['item', 'col'], aggfunc=['mean'])
   .....: 
Out[134]: 
      mean                                                                   
      val0                                                                   
item item0             item1                         item2                   
col   col2  col3  col4  col0  col1  col2  col3  col4  col0   col1  col3  col4
row                                                                          
row0   NaN   NaN   NaN  0.77   NaN   NaN   NaN   NaN   NaN  0.605  0.86  0.65
row2  0.35   NaN  0.37   NaN   NaN  0.44   NaN   NaN  0.13    NaN  0.50  0.13
row3   NaN   NaN   NaN   NaN  0.31   NaN  0.81   NaN   NaN    NaN  0.28   NaN
row4  0.15  0.64   NaN   NaN  0.10  0.64  0.88  0.24   NaN    NaN   NaN   NaN
```



## 展开列表格式的元素⭐

- 列表

```python
In [135]: keys = ['panda1', 'panda2', 'panda3']

In [136]: values = [['eats', 'shoots'], ['shoots', 'leaves'], ['eats', 'leaves']]

In [137]: df = pd.DataFrame({'keys': keys, 'values': values})

In [138]: df
Out[138]: 
     keys            values
0  panda1    [eats, shoots]
1  panda2  [shoots, leaves]
2  panda3    [eats, leaves]


In [139]: df['values'].explode()
Out[139]: 
0      eats
0    shoots
1    shoots
1    leaves
2      eats
2    leaves
Name: values, dtype: object

In [140]: df.explode('values')
Out[140]: 
     keys  values
0  panda1    eats
0  panda1  shoots
1  panda2  shoots
1  panda2  leaves
2  panda3    eats
2  panda3  leaves


### Series.explode()
In [141]: s = pd.Series([[1, 2, 3], 'foo', [], ['a', 'b']])

In [142]: s
Out[142]: 
0    [1, 2, 3]
1          foo
2           []
3       [a, b]
dtype: object

In [143]: s.explode()
Out[143]: 
0      1
0      2
0      3
1    foo
2    NaN
3      a
3      b
dtype: object
```

- 逗号分隔

```python
In [144]: df = pd.DataFrame([{'var1': 'a,b,c', 'var2': 1},
   .....:                    {'var1': 'd,e,f', 'var2': 2}])
   .....: 

In [145]: df
Out[145]: 
    var1  var2
0  a,b,c     1
1  d,e,f     2


In [146]: df.assign(var1=df.var1.str.split(',')).explode('var1')
Out[146]: 
  var1  var2
0    a     1
0    b     1
0    c     1
1    d     2
1    e     2
1    f     2
```





# #参考文献

[Link: 官方文档](https://pandas.pydata.org/pandas-docs/stable/user_guide/reshaping.html)

