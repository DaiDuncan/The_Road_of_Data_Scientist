# Python-进阶 | 玩转数据类型转换

![image](https://cdn.nlark.com/yuque/0/2020/png/1136179/1591177097360-00895517-e6e5-48b7-96d9-7b9a4e0a575c.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_10%2Ctext_6K-t6ZuALUREdW5jYW4%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)



## 一、Python、NumPy、Pandas数据类型互转

| 行为输入     | 列为输出      |                   |                 |               |                                                              |
| ------------ | ------------- | ----------------- | --------------- | ------------- | ------------------------------------------------------------ |
| ————>        | str           | list              | tuple           | set           | dict (自定义索引(键))                                        |
| str          | /             | list(str)         | tuple(str)      | set(str)      |                                                              |
| list         | ''.join(list) | /                 | tuple(list)     | set(list)     |                                                              |
| tuple        |               | list(tuple)       | /               | set(tuple)    |                                                              |
| set          | ''.join(set)  | list(set)         | tuple(set)      | /             |                                                              |
| dict         |               |                   |                 |               | /                                                            |
| ndarray      |               | np.array.tolist() | tuple(np.array) | set(np.array) |                                                              |
| pd.Series    |               |                   |                 |               |                                                              |
| pd.DataFrame |               |                   |                 |               | 1)df.to_dict(orient='dict') 2)df.to_dict(orient='list') 3)df.to_dict(orient='series') 4)df.to_dict(orient='records') |



右续上表：

| 行为输入     | 列为输出                                                     |                                                              |                                                              |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| ————>        | ndarray                                                      | pd.Series(data, index)                                       | pd.DataFrame(dict) (dict含索引)                              |
| str          |                                                              |                                                              |                                                              |
| list         | np.array(list)                                               | pd.Series(list, index= ['one', 'two', 'three', 'four', 'five']) #list是多个数据(包括不同类型)的列表(该数据本身可以是列表列表的嵌套二维数组) | pd.DataFrame(list, index = ['one', 'two', 'three', 'four', 'five'], columns = ['year', 'state', 'pop']) #本质==热图 二维数据表 |
| tuple        |                                                              |                                                              |                                                              |
| set          | np.array(set)                                                |                                                              |                                                              |
| dict         |                                                              | pd.Series(dict) #dict中的key充当Series的索引index            | pd.DataFrame(dict) #key 充当DataFrame 的 columns             |
| ndarray      | /                                                            |                                                              | pd.DataFrame(np.array, index = ['one', 'two', 'three', 'four', 'five'], columns = ['year', 'state', 'pop']) |
| pd.Series    | 1)pd.Series.as_matrix(series) 2)series.as_matrix() #series表示某个Series类型的数据 | /                                                            |                                                              |
| pd.DataFrame | 1)pd.DataFrame.as_matrix(df) 2)df.as_matrix() 3)df.values 4)np.array(df) 5)df.as_matrix(['column']) |                                                              | /                                                            |



## 二、函数传递的参数类型



- 可哈希元素
  一旦函数使用过程中出现**可迭代对象所存储的元素不可哈希**，就会抛出TypeError: unhashable type set/list/dict 类似错误



> 解决方法
>
> ![image](https://cdn.nlark.com/yuque/0/2020/png/1136179/1591177097379-dd9a35cc-c9df-45cc-b3fb-3346ddab1060.png)

|                | 具体数据类型           |
| -------------- | ---------------------- |
| 可哈希的元素   | int、float、str、tuple |
| 不可哈希的元素 | list、set、dict        |



- 问题：为什么 list 是不可哈希的，而**tuple**是可哈希的？



> 1. 因为 list 在它的生命期内，是可变的，你可以在任意时间改变其内的元素值。
> 2. 所谓元素可不可哈希，意味着**是否使用 hash 进行索引**
> 3. list 不使用 hash 进行元素的索引，自然它对存储的元素没有可哈希的要求；而 tuple 使用 hash 值进行索引。