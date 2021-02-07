# index索引(含NA值)

```python
In [1]: s = pd.Series([1, 2, 3])
In [2]: mask = pd.array([True, False, pd.NA], dtype="boolean")

In [3]: s[mask]
Out[3]: 
0    1
dtype: int64
    
    
### 将NA值转变为True
In [4]: s[mask.fillna(True)]
Out[4]: 
0    1
2    3
dtype: int64
```









# 逻辑运算

这些操作是对称的，因此左右翻转不会影响结果

| Expression      | Result  |
| :-------------- | :------ |
| `True & True`   | `True`  |
| `True & False`  | `False` |
| `True & NA`     | `NA`    |
| `False & False` | `False` |
| `False & NA`    | `False` |
| `NA & NA`       | `NA`    |
| `True | True`   | `True`  |
| `True | False`  | `True`  |
| `True | NA`     | `True`  |
| `False | False` | `False` |
| `False | NA`    | `NA`    |
| `NA | NA`       | `NA`    |
| `True ^ True`   | `False` |
| `True ^ False`  | `True`  |
| `True ^ NA`     | `NA`    |
| `False ^ False` | `False` |
| `False ^ NA`    | `NA`    |
| `NA ^ NA`       | `NA`    |