[link: Pandas小技巧：万能转格式、轻松合并、压缩数据](https://mp.weixin.qq.com/s/ANtykijhrNU0SD2kJ3-tGg)

- 数据科学家 Roman Orac 分享了他在工作中相见恨晚的 Pandas 使用技巧



# 数据IO

在展示成果的时候，常常需要把 DataFrame 转成另一种格式

## DataFrame 转 HTML

```python
import numpy as np
import pandas as pd
import random

n = 10
df = pd.DataFrame(
    {
        "col1": np.random.random_sample(n),
        "col2": np.random.random_sample(n),
        "col3": [[random.randint(0, 10) for _ in range(random.randint(3, 5))] for _ in range(n)],
    }
)


df_html = df.to_html()
with open(‘analysis.html’, ‘w’) as f: f.write(df_html)
    
    
#== read_html 
```



## **DataFrame 转 LaTeX**

```python
df.to_latex()
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/YicUhk5aAGtByryePlAarLTWic0ybbZP3jjPq8owaKsSVlNrTycdo4m1UhTicd4BBsyynvCRHTRAljkX62U2ftCYA/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



## DataFrame 转 Markdown

```python
print(df.to_markdown())
```

注：这里还需要 tabulate 库



## **DataFrame 转 Excel**

```python
df.to_excel(‘analysis.xlsx’)

#== read_excel()
```

注意：**xlwt** 和 **openpyxl** 这两个工具包



## DataFrame 转字符串

```python
df.to_string()
```





# 5个鲜为人知的技巧

## 1、data_range

从外部 API 或数据库获取数据时，需要多次指定时间范围

```python
import pandas as pd
date_from = “2019-01-01”
date_to = “2019-01-12”
date_range = pd.date_range(date_from, date_to, freq=”D”)
print(date_range)
```

- freq = “D”/“M”/“Y”，该函数就会分别返回按天、月、年递增的日期。



## 2、合并数据

通过关键字“key”把两个DataFrame整合到一起

```python
df_merge = left.merge(right, on = ‘key’, how = ‘left’, indicator = True)
```



## 3、最近合并（Nearest merge）

在处理股票或者加密货币这样的财务数据时，价格会随着实际交易变化。

针对这样的数据，Pandas提供了一个好用的功能，merge_asof。

该功能可以通过最近的key（比如时间戳）合并DataFrame。



最新报价和交易之间可能有10毫秒的延迟，或者没有报价，在进行合并时，就可以用上 merge_asof。

```python
pd.merge_asof(trades, quotes, on=”timestamp”, by=’ticker’, tolerance=pd.Timedelta(‘10ms’), direction=‘backward’)
```





## 4、创建Excel报告

```python
import numpy as np
import pandas as pd

df = pd.DataFrame(np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]]), columns=["a", "b", "c"])

report_name = 'example_report.xlsx'
sheet_name = 'Sheet1'
writer = pd.ExcelWriter(report_name, engine='xlsxwriter')
df.to_excel(writer, sheet_name=sheet_name, index=False)



#== 不只是数据，还可以添加图表：这里需要 XlsxWriter 库
# define the workbook
workbook = writer.book
worksheet = writer.sheets[sheet_name]
# create a chart line object
chart = workbook.add_chart({'type': 'line'})
# configure the series of the chart from the spreadsheet
# using a list of values instead of category/value formulas:
#     [sheetname, first_row, first_col, last_row, last_col]
chart.add_series({
    'categories': [sheet_name, 1, 0, 3, 0],
    'values':     [sheet_name, 1, 1, 3, 1],
})
# configure the chart axes
chart.set_x_axis({'name': 'Index', 'position_axis': 'on_tick'})
chart.set_y_axis({'name': 'Value', 'major_gridlines': {'visible': False}})
# place the chart on the worksheet
worksheet.insert_chart('E2', chart)
# output the excel file
writer.save()
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/YicUhk5aAGtByryePlAarLTWic0ybbZP3jVVmaXYhbl9nU6cqBQ2WibClbmqqzbbwB3sjG5TCF4O7wUGP5ybqe8Fw/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



## **5、节省磁盘空间**

Pandas在保存数据集时，可以对其进行压缩，其后以压缩格式进行读取

先搞一个 300MB 的 DataFrame，把它存成 csv：

```python
df = pd.DataFrame(pd.np.random.randn(50000,300))
df.to_csv(‘random_data.csv’, index=False)

df.to_csv(‘random_data.gz’, compression=’gzip’, index=False)	#压缩后：文件就变成了136MB

#== gzip压缩文件可以直接读取
df = pd.read_csv(‘random_data.gz’)
```





