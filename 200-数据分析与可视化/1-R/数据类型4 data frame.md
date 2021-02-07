与matrix类似，但是具有不同类型的元素/数据

# 操作1|创建

```R
### variable => columns, observations => rows
quantity <- c(200, 300, 100)		#num
crop <- c("corn", "leek", "pea")	#Factor：string默认存储为Factor
subsidy <- c(TRUE, FALSE, TRUE)		#logi
my_df <- data.frame(quantity, crop, subsidy)
my_df

# 上述my_df中的crop显示Factor，而不是字符串
my_df <- data.frame(quantity, crop, subsidy, stringAsFactors=FALSE)
class(my_df$crop)	#显示"character"


# 直接添加新的col
my_df$farmer <- c("Bob", "Sam", "Mike")
my_df
```





# 操作2|查 选/索引(含切片) 改 增 删

## 1 查

```R
### 查找属性
str(my_df)	#类似pandas中的df.info()
```





## 2 选/索引

```R
my_df[["quantity"]]
my_df[[1]]	#第一列

my_df[ , 3:4]	#选择第3和第4列

my_df[subsidy, ]
```





## 3 改



## 4 增



## 5 删



# 操作3|遍历 排序 统计



## 1 遍历(迭代)



## 2 排序

```R
### 默认是升序
order(my_df$quantity)	#输出结果是3 1 2
my_df[order(my_df$quantity), ]
```





## 3 统计

