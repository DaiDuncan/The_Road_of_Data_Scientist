# Pandas最常用的函数

![image-20201205104741341](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201205104741.png)

## #不熟悉列表

> pd.date_range
>
> pd.util.testing
>
> pd.Timestamp
>
> pd.DatetimeIndex



## 1. pd.Dataframe

Creates a dataframe object.

``````python
# 参数data直接赋值
df = pd.DataFrame(data={'y': [1, 2, 3],
                        'score': [93.5, 89.4, 90.3],
                        'name': ['Dirac', 'Pauli', 'Bohr'],
                        'birthday': ['1902-08-08', '1900-04-25', '1885-10-07']}) 
print(type(df)) #python内置函数 结果显示<class ...>
print(df.dtypes)
df
``````

![image-20201205102751351](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201205102751.png)



## 6. pd.merge

Combine dataframes.

```python
### 参数data中：
# zip()两两组队 (('Dirac', True), ('Pauli', False), ('Bohr', True), ('Einstein', True))
# list(zip()): [('Dirac', True), ('Pauli', False), ('Bohr', True), ('Einstein', True)]
df_new = pd.DataFrame(data=list(zip(['Dirac', 'Pauli', 'Bohr', 'Einstein'],
                                    [True, False, True, True])),
                      columns=['name', 'friendly'])
 
# 以left=df的表格为基准 
df_merge = pd.merge(left=df, right=df_new, on='name', how='outer')
df_merge
```

![image-20201205103848844](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201205103848.png)



# Panda DataFrame最常用的函数

![image-20201205104657259](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201205104657.png)

## #不熟悉列表

> df.iterrows



## 1. df.columns

列索引/列标签/列名



## 2. df.index

行索引



# #参考文献

[Link: Blog|Top 20 Pandas, NumPy and SciPy functions on GitHub](https://galeascience.wordpress.com/2016/08/10/top-10-pandas-numpy-and-scipy-functions-on-github/)