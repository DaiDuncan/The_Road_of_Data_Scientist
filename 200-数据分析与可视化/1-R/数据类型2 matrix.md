```R

```



# 操作1|创建

```R
data <- c(1, 2, 3, 4, 5, 6)
my_matrix <- matrix(data, nrow=2, ncol=3, byrow=TRUE)	#按row来填充，byrow=FALSE
my_matrix	#注意：下标不是以0为基准

matrix(1:6)

# 错误示例
matrix(1:4, nrow=1, ncol=1)	#元素的个数4个与nrow * ncol维度不一致

### 属性：colnames()
colnames(my_matrix) <- c("run", "cycle")
### 属性：rownames()
rownames(my_matrix) <- c("Anne", "Luke", "Emma")
```





# 操作2|查 选/索引(含切片) 改 增 删

## 1 查



## 2 选/索引

```R
### 提取第二列
my_matrix[ , 2]
```





## 3 改



## 4 增

```R
### 添加行
rbind(my_matrix, c(7, 8))

### 添加列
cbind(my_matrix, c(9, 8, 7))
```





## 5 删



# 操作3|遍历 排序 统计



## 1 遍历(迭代)



## 2 排序



## 3 统计

```R
rowSums(my_matrix)
colSums(my_matrix)
```

