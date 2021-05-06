只要有递归，就会有回溯(一般DFS就是回溯)

- 二叉树：递归



纯暴力搜索，并不高效(但for循环写不出来)



# 应用场景

## 组合

集合元素1 2 3 4的两两组合



## 排列



## 切割

给一个字符串：切割，保持每一个子串都是回文字符串



## 子集

集合元素1 2 3 4的所有子集





## 棋盘

N皇后

数独





# 如何使用？

借助图形来理解

回溯法都能抽象为树形结构：N叉树

- 树的宽度：回溯法中集合的大小 => 一般for循环遍历
- 树的深度：递归的深度



## 模板⭐

```python
result = []
func backtrack(选择列表, 路径):
    if 满足结束条件:
        result.add(路径)	#收集 叶子节点的结果
        return
    for 选择 in 选择列表:	#相当于子节点(分类情况)
        做选择-处理节点
        backtrack(选择列表,路径)	#递归函数
        撤销选择				 #回溯的操作
```

- 回溯的操作
  - 比如组合：12的操作
    - 2弹出去，剩下1，加上3 => 也就是13





## 回溯三部曲

递归也有三部曲，回溯也是基于递归实现的：

- 1 递归函数的参数和返回值
- 2 确定递归的终止条件
- 3 单层搜索的逻辑：单层递归的逻辑



[代码讲解示例|组合问题](https://www.bilibili.com/video/BV1ti4y1L7cv)

1 这里是组合

如果要求排列，在2的分支里面就有134 => 叶节点有`21`一个新的排列



2 最后一个分支无效，可以剪枝

![image-20210328225340868](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210328225341.png)

```c++
二维数组result返回结果	//全局变量 or 参数中引用(函数中参数过多 影响可读性)
一维数组path		  //全局变量 or 参数中引用
    
void backtracking(n, k, startIndex){	//一般返回都是void
    /*参数
    n: 元素的个数
    k: 组合的大小(组合元素一共2个)
    startIndex
    */
    
    //终止条件
    if(path.size == k){
        result.add(path);
        return ;
    }
    for(i = startIndex; i<=n; i++){
        path.push(i);
        backtracking(n, k, i+1);
        path.pop();
    }
}	
```

me|手写笔记：确认一遍流程



## 剪枝

[link: B站](https://www.bilibili.com/video/BV1wi4y157er)

问题：

- n=4个元素
- k=4个组合的元素

1 for循环是遍历每一个节点/分支，也就是图中的1 2 3 4四个分支

```c++
for(i = startIndex; i<=n; i++){
    path.push(i);
    backtracking(n, k, i+1);
    path.pop();
}
```

2 剪枝 => 该分支直接拿掉(不进入循环或者直接break)，但是判断条件呢？

- 外面的for循环 `i < n-(k-path.size)+1`⭐
  - `path.size` 已经选取的元素个数
  - `k-path.size` 还需要选取的元素个数
  - `n-(k-path.size)+1` 至多需要添加的元素
    - 因为包含startIndex，所以需要+1
- 比如：n=4, k=3
  - **假设path.size是0**(还没有元素放进去)，则结果是2：即至多从2的位置开始
    - 比如`234`三个元素
    - 从1开始绝对没问题
  - 如果已经放进去了1个元素，那么结果就是3
    - 极限情况就是`X34`
    - 从2和1开始都没问题

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210329152056.png" alt="image-20210329152052238" style="zoom:80%;" />









# #参考文献

[link: 代码随想录|回溯算法-理论](https://www.bilibili.com/video/BV1cy4y167mM)

