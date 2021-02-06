| 函数           | 功能                                           |
| :------------- | :--------------------------------------------- |
| mean           | 算数平均值                                     |
| harmonic_mean  | 调和平均值                                     |
| median         | 中位数                                         |
| median_low     | 数据的第一个中位数（总数为偶数时有两个中位数） |
| median_high    | 数据的第二个中位数                             |
| median_grouped | 分组数据的中位数的均值                         |
| mode           | 离散数据的模式, 数据中最常见的值               |
| pstdev         | 数据总体的标准差                               |
| pvariance      | 数据总体的方差                                 |
| stdev          | ==数据样本的标准差==                           |
| variance       | 数据样本的方差                                 |



```python
import statistics

lst = [1, 4, 5, 7, 1, 3, 6, 9, 19]

print(statistics.mean(lst))             # 算数平均值
print(statistics.harmonic_mean(lst))    # 调和平均值
print(statistics.median(lst))       # 中位数
print(statistics.median_low(lst))   # 数据的第一个中位数（总数为偶数时有两个中位数）
print(statistics.median_high(lst))  # 数据的第二个中位数
print(statistics.median_grouped(lst))   # 分组数据的中位数的均值
print(statistics.mode(lst))         # 离散数据的模式, 数据中最常见的值

print(statistics.pstdev(lst))       # 数据总体的标准差
print(statistics.pvariance(lst))    # 数据总体的方差
print(statistics.stdev(lst))        # 数据样本的标准差
print(statistics.variance(lst))     # 数据样本的方差
```