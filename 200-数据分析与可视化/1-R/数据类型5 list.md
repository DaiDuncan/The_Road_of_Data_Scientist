与vector的区别：可以保存不同类型的数据

- list还可以嵌套list

# 操作1|创建

```R
my_list <- list(2, "c", TRUE)
my_list


### 复杂容器
vec <- 1:6
mat <- matrix(1:6, nrow=2)
df <- data.frame(mat)
list(vec, mat, df)	#内容为[[1]]vec, [[2]]matrix, [[3]]data.frame
```





# 操作2|查 选/索引(含切片) 改 增 删

## 1 查



## 2 选/索引

```R
my_list <- list(int=2, vec=1:6, bool=TRUE)
my_list[[2]]

my_list$<name>
my_list$vec


my_list[[2]][3]	# my_list$vec[3]
```





## 3 改

```R
### 修改names属性
my_list <- list(vector=vec, matrix=mat, dframe=df)	#分别命名为vector, matrix, dframe
```





## 4 增

```R
my_list <- c(my_list, TRUE)
```





## 5 删



# 操作3|遍历 排序 统计



## 1 遍历(迭代)



## 2 排序



## 3 统计

