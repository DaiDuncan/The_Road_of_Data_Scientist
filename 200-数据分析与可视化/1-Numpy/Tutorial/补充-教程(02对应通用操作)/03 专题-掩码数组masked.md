在许多情况下，数据集可能不完整或因无效数据的存在而受到污染。 

- 例如，传感器可能无法记录数据或记录无效值。 

numpy.ma模块通过引入**掩码数组**提供了一种解决此问题的便捷方法。

masked数组是标准numpy.ndarray和 masked的组合。 

- 掩码是nomask，表示关联数组的值无效，或者是一个布尔数组，用于确定关联数组的每个元素是否有效。 
  - 当掩码的元素为False时，关联数组的相应元素有效，并且被称为未屏蔽。 
  - ==当掩码的元素为True时，相关数组的相应元素被称为被屏蔽（无效）==。

该包确保在计算中==不使用被屏蔽的条目==。

```python
>>> import numpy as np
>>> import numpy.ma as ma
>>> x = np.array([1, 2, 3, -1, 5])
# 如果我们希望-1被标记为无效，则可以：
>>> mx = ma.masked_array(x, mask=[0, 0, 0, 1, 0])
# 当计算平均值时，不会考虑无效
>>> mx.mean()
2.75
```

---

[link: 官方API|Masked array operations](https://numpy.org/doc/stable/reference/routines.ma.html)

- 常数

- 创建

- 逻辑检查数组元素

- mask数组的操作

  - 改变shape
  - 轴/维度的转换
  - 维度
  - 连接数组

- mask的操作

  - 创建mask
  - 获取mask
  - 查找masked的数据
  - 修改mask

- 比较运算符

  - 填充mask的数组

- mask数组的算术运算

  - 基本算术

  - 最大、最小值

  - 排序

  - 线性代数

  - 多项式拟合

  - 小数取整

    