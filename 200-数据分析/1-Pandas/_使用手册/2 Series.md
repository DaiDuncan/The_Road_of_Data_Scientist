Series与Numpy中的**一维array**类似，二者与Python基本的数据结构List也很相近

区别是：List中的元素**可以是不同的数据类型**，而 Array 和 Series 中则==**只允许存储相同的数据类型**==，这样可以**更有效的使用内存，提高运算效率**



Series 本质是一个**含有索引的一维数组**

- 和 NumPy ndarray(n维)的区别：
  1. 每个元素分配索引标签(label 即index)
  2. 可以存储数值类型以外的数据

- 和字典的区别
  - Series 更快，其内部是向量化运行的，和迭代相比，使用 Series 可以获得显著的性能上的优势



### #数据结构的特性

> +可变
>
> +带标签的==一维数组==，可存储整数、浮点数、字符串、Python对象等类型的数据(轴标签统称为索引)
>
> +类似多维数组：操作与 ndarray 类似，支持大多数 NumPy 函数，还支持索引切片
>
> +类似==字典：必须提供索引==





# 操作1|创建

- 参数index：一般为列表

1. 通过list创建

   ```python
   pd.Series([list])
   # index 必须和 value 严格对应
   # 或者不提供索引：默认RangeIndex(从0开始编号)
   pd.Series(data = [30, 6, 'Yes', 'No'], index = ['eggs', 'apples', 'milk', 'bread'])
   ```

2. 通过字典

   ```python
   # 字典的key作为 index name
   n = Series({"a":2,"b":3}) # 自动 index
   
   a    2
   b    3
   dtype: int64
   
   p = Series({"a":2,"b":3}, index=["a","c","b"]) # 手动设置index
   
   a    2.0
   c    NaN
   b    3.0
   dtype: float64
       
   # in判断：只能判断index值
   ```

   

3. 特殊：时间序列
   `pd.data_range()`
   ![image](https://cdn.nlark.com/yuque/0/2020/png/1136179/1591174539183-9abd2a4b-94fd-4712-bd70-16d5563ebe3c.png)



# 操作2|查 选/索引 改 增 删



## 1 查

```python
### 属性
.shape
.ndim
.size
# 本质含有index的数组 => .index, .values
.values
.index

# 属性：name
ser.name
ser.index.name
```



## 2 选/索引

Series索引方法：

- [str]
- [int]
- [str:str] 右开
- [int:int] 右闭
- [bool] 

```python
### 索引
Series['label/index']
.loc  #标签索引
.iloc #数值索引

# 布尔索引
Series[BOOL]

### 筛选：逻辑判断
in
```



## 3 改

```python
### 索引重置与更换
reindex()
reindex_like()  #用align对齐多个对象

### 索引重命名
.Series.rename()  #支持按不同的轴基于映射（字典或 Series）调整标签

### 数据平移
pd.Series().shift(N)  #向下平移N行
```



## 4 增

```python
# 使用 append 合并两个 Series 结构时，区别于Python列表，返回了一个新的Series而不是就地修改
a = pd.Series([1,2,3])
b = pd.Series([4,5])
c = a.append(b)
```







## 5 删

```python
Series.drop(label, inplace=False)  #与 reindex 经常配合使用，该函数用于删除轴上的一组标签
```







# 操作3|遍历 排序 统计



## 1 遍历(迭代)

`.items()`  (Index, 标量值)元组



## 2 排序

1. 搜索排序
   `Series.searchsorted()`  这与`numpy.ndarray.searchsorted()` 的操作方式类似
2. 最大值与最小值
   `.nsmallest()` 与 `.nlargest()` 方法，本方法返回 N 个最大或最小的值

```python
### .sort_index()
obj = pd.Series(range(4), index=['d', 'a', 'b', 'c'])
obj.sort_index() # 默认 axis = 0，ascending = True
```





## 3 统计

### 缺失值统计和处理

```python
p.isnull()  #非null值可以使用 p.notnull()

a    False
c     True
b    False
dtype: bool

p[p.isnull()] #筛选空值

a    2.0
b    3.0
dtype: float64

p[-p.isnull()] #筛选非空值（numpy布尔矩阵切片语法）

c   NaN
dtype: float64
```





# 补充小技巧

## 1 数学运算(参考ndarray)

```python
### 与单个数字
# Series * 2 乘法对字符串可行
# Series / 2 除法对字符串不可行

Series + 2
Series - 2


### 数学函数
np.exp(Series)
np.sqrt(Series)
np.power(Series,2)
```



## 2 结合布尔运算

`list[True]`  最后一个数

`list[False]` 第一个数

`close_planets = time_light[[False, True, True, False, False]]`  只输入True对应的两个元素(第一个[]表示index，第二个[]表示列表)

注意：元素个数和True/False个数必须匹配





## 3 结合字符串方法

pd.Series.str.lower()