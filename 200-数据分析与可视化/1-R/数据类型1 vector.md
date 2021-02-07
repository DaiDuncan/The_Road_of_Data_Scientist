# 简介

```R
my_sequence <- c(10:15)

x <- seq(10, 20, by=5)	#函数seq(): 10 15 20
```

## 索引(含切片)

不是以0为基准



## vector的属性

类似Series的index

```R
x <- c(75, 80, 83)
days <- c("Mon", "Tue", "Wed")
names(x) <- days

x
x["Wed"]
```







# 操作1|创建

```R
# 初始化变量
variable <- 5


# 创建集合collection: 又被称为atomic vectors
var <- 3	#单个元素的集合
var <- c(0, 1, 1, 2, 3)


# 数据类型
a <- c(1, 2)
b <- c("a", "bee", "cdefg")
c <- c(TRUE, FALSE)

class(a)	#输出 “numeric”
class(b)	#输出 “character”
class(c)	#输出 “logical”
```







# 操作2|查 选/索引(含切片) 改 增 删

## 1 查



## 2 选/索引



## 3 改

```R
### 连接字符串：类似Python中的字符串相加

paste(word, collapse=" ")	#用空格space隔开
```





## 4 增



## 5 删







# 操作3|遍历 排序 统计



## 1 遍历(迭代)



## 2 排序



## 3 统计

```R
length(<vec>)	
#length针对字符串，不统计单词和字符数量，而是字符串整体的数量

nchar()	# 统计字符的数量
```






