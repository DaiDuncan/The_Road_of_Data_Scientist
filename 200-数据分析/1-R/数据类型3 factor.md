背景：针对类别数据



# 操作1|创建

```R
os <- c("Android", "iOS", "Android")
os_factor <- factor(os)
os_factor


### 有序类别
length <- c("medium", "short", "long", "short", "medium")
l_fctr <- factor(length, order=TRUE, levels= c("short", "medium", "long"))
l_fctr
```





# 操作2|查 选/索引(含切片) 改 增 删

## 1 查



## 2 选/索引



## 3 改

```R
### 更改属性：类别名称
vector <- c("A_grp", "B_grp", "A_grp")
fctr <- factor(vector)
levels(fctr) <- c("A", "B")
```





## 4 增



## 5 删



# 操作3|遍历 排序 统计



## 1 遍历(迭代)



## 2 排序



## 3 统计

```R
summary(group_fac)
```





# #注意

无序类别数据不能用来比较大小