## 背景|第k大的数

[link: MIT|求Median](https://www.youtube.com/watch?v=EzeYI7p9MjU&t=3136s)

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210325155550.png" alt="larger  medians  smaller " style="zoom:50%;" />

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210325155501.png" alt="0(1),  for n < 140  for n style="zoom:50%;"  > 140 " />





## 背景|FFT

[link:   MIT视频 ]( https://www.youtube.com/watch?v=iTMn0Kt18tg&t=853s)





# 应用

先分别处理局部，再合并结果

适用场景

- 快速排序
- 归并排序
- 二叉树相关问题



分治法==模板==

- 递归返回条件
- 分段处理
- 合并结果

```go
func traversal(root *TreeNode) ResultType  {
    // nil or leaf
    if root == nil {
        // do something and return
    }

    // Divide：递归操作
    ResultType left = traversal(root.Left)
    ResultType right = traversal(root.Right)

    // Conquer
    ResultType result = Merge from left and right

    return result
}
```



## 示例|分治法遍历二叉树

注意：递归需要返回结果result用于合并

```go
// V2：通过分治法遍历二叉树
func preorderTraversal(root *TreeNode) []int {
    result := divideAndConquer(root)
    return result
}
func divideAndConquer(root *TreeNode) []int {
    result := make([]int, 0)
    // 返回条件(null & leaf)
    if root == nil {
        return result
    }
    // 分治(Divide)
    left := divideAndConquer(root.Left)	//先遍历左子树
    right := divideAndConquer(root.Right)
    // 合并结果(Conquer)
    result = append(result, root.Val)
    result = append(result, left...)	//第二个参数也是列表slice
    result = append(result, right...)
    return result
}
```



补充|go语言 append()

```go
var a []int
a = append(a, 1) // 追加1个元素
a = append(a, 1, 2, 3) // 追加多个元素, 手写解包方式
a = append(a, []int{1,2,3}...) // 追加一个切片, 切片需要解包

//需要注意的是，在使用 append() 函数为切片动态添加元素时，如果空间不足以容纳足够多的元素，切片就会进行“扩容”，此时新切片的长度会发生改变。


//第一个参数为切片，后面是该切片存储元素类型的可变参数
slice := append([]int{1,2,3},4,5,6)
fmt.Println(slice) //[1 2 3 4 5 6]

//第二个参数也可以直接写另一个切片，将它里面所有元素拷贝追加到第一个切片后面。要注意的是，这种用法函数的参数只能接收两个slice，并且末尾要加三个点
slice := append([]int{1,2,3},[]int{4,5,6}...)
fmt.Println(slice) //[1 2 3 4 5 6]
```



## 示例|归并排序merge

关键：merge()合并两个有序集合

```go
func MergeSort(nums []int) []int {
    return mergeSort(nums)
}
func mergeSort(nums []int) []int {
    if len(nums) <= 1 {
        return nums
    }
    // 分治法：divide 分为两段
    mid := len(nums) / 2
    left := mergeSort(nums[:mid])
    right := mergeSort(nums[mid:])
    // 合并两段数据
    result := merge(left, right)
    return result
}

//合并两个有序集合
func merge(left, right []int) (result []int) {
    // 两边数组合并游标
    l := 0
    r := 0
    // 注意不能越界
    for l < len(left) && r < len(right) {
        // 谁小合并谁
        if left[l] > right[r] {
            result = append(result, right[r])
            r++
        } else {
            result = append(result, left[l])
            l++
        }
    }
    // 剩余部分合并
    result = append(result, left[l:]...)
    result = append(result, right[r:]...)
    return
}
```





## 示例|快速排序quick

- 快排由于是==原地交换==，所以没有合并过程：传入的索引是存在的索引（如：0、length-1 等），越界可能导致崩溃

```go
func QuickSort(nums []int) []int {
    // 思路：把一个数组分为左右两段，左段小于右段，类似分治法没有合并过程
    quickSort(nums, 0, len(nums)-1)
    return nums

}
// 原地交换(指的是nums数组)，所以传入交换索引
func quickSort(nums []int, start, end int) {
    if start < end {
        // 分治法：divide
        pivot := partition(nums, start, end)	//中间的值换为用于比较的基准值
        quickSort(nums, 0, pivot-1)
        quickSort(nums, pivot+1, end)
    }
}
// 分区
func partition(nums []int, start, end int) int {
    p := nums[end]
    i := start
    for j := start; j < end; j++ {
        if nums[j] < p {
            swap(nums, i, j)
            i++
        }
    }
    // 把中间的值换为用于比较的基准值
    swap(nums, i, end)
    return i
}
func swap(nums []int, i, j int) {
    t := nums[i]
    nums[i] = nums[j]
    nums[j] = t
}
```





# 常见题目

1 给定一个二叉树，找出其最大深度

[maximum-depth-of-binary-tree](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)

```go
func maxDepth(root *TreeNode) int {
    // 返回条件处理
    if root == nil {
        return 0
    }
    // divide：分左右子树分别计算
    left := maxDepth(root.Left)
    right := maxDepth(root.Right)

    // conquer：合并左右子树结果
    if left > right {
        return left + 1
    }
    return right + 1
}
```



2 给定一个二叉树，判断它是否是高度平衡的二叉树

[balanced-binary-tree](https://leetcode-cn.com/problems/balanced-binary-tree/)

思路：分治法

左边平衡 && 右边平衡 && 左右两边高度 <= 1， 因为需要返回是否平衡及高度，要么返回两个数据，要么合并两个数据， 所以用-1 表示不平衡，>0 表示树高度（二义性：一个变量有两种含义）。

```go
func isBalanced(root *TreeNode) bool {
    if maxDepth(root) == -1 {
        return false
    }
    return true
}
func maxDepth(root *TreeNode) int {
    // check
    if root == nil {
        return 0
    }
    left := maxDepth(root.Left)
    right := maxDepth(root.Right)

    // 为什么返回-1呢？（变量具有二义性）
    if left == -1 || right == -1 || left-right > 1 || right-left > 1 {
        return -1
    }
    if left > right {
        return left + 1
    }
    return right + 1
}
```

一般工程中，==结果通过两个变量来返回==，不建议用一个变量表示两种含义



3 给定一个非空二叉树，返回其最大路径和

[binary-tree-maximum-path-sum](https://leetcode-cn.com/problems/binary-tree-maximum-path-sum/)

思路：分治法

分为三种情况：

- 左子树最大路径和最大
- 右子树最大路径和最大
- 左右子树最大加根节点最大



需要保存两个变量：一个保存子树最大路径和，一个保存左右加根节点和

然后比较这个两个变量选择最大值即可

```gO
type ResultType struct {
    SinglePath int // 保存单边最大值
    MaxPath int // 保存最大值（单边或者两个单边+根的值）
}
func maxPathSum(root *TreeNode) int {
    result := helper(root)
    return result.MaxPath
}
func helper(root *TreeNode) ResultType {
    // check
    if root == nil {
        return ResultType{
            SinglePath: 0,
            MaxPath: -(1 << 31),
        }
    }
    // Divide
    left := helper(root.Left)
    right := helper(root.Right)

    // Conquer
    result := ResultType{}
    // 求单边最大值
    if left.SinglePath > right.SinglePath {
        result.SinglePath = max(left.SinglePath + root.Val, 0)
    } else {
        result.SinglePath = max(right.SinglePath + root.Val, 0)
    }
    // 求两边加根最大值
    maxPath := max(right.MaxPath, left.MaxPath)
    result.MaxPath = max(maxPath,left.SinglePath+right.SinglePath+root.Val)
    return result
}
func max(a,b int) int {
    if a > b {
        return a
    }
    return b
}
```





4 给定一个二叉树, 找到该树中两个指定节点的最近公共祖先

[lowest-common-ancestor-of-a-binary-tree](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/)

思路：分治法



有左子树的公共祖先或者有右子树的公共祖先，就返回子树的祖先，否则返回根节点

```go
func lowestCommonAncestor(root, p, q *TreeNode) *TreeNode {
    // check
    if root == nil {
        return root
    }
    // 相等 直接返回root节点即可
    if root == p || root == q {
        return root
    }
    // Divide
    left := lowestCommonAncestor(root.Left, p, q)
    right := lowestCommonAncestor(root.Right, p, q)


    // Conquer
    // 左右两边都不为空，则根节点为祖先
    if left != nil && right != nil {
        return root
    }
    if left != nil {
        return left
    }
    if right != nil {
        return right
    }
    return nil
}
```



