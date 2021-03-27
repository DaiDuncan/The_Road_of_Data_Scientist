# 二分法

给一个有序数组和目标值，找第一次/最后一次/任何一次出现的索引，如果没有出现返回-1



## 模板四点要素

1、初始化：start=0、end=len-1

2、循环退出条件：start + 1 < end

3、比较中点和目标值：A[mid] ==、 <、> target

4、判断最后两个元素是否符合：A[start]、A[end] ? target



时间复杂度 O(logn)，使用场景一般是有序数组的查找





## 示例|[binary-search](https://leetcode-cn.com/problems/binary-search/)

```go
// 二分搜索最常用模板
func search(nums []int, target int) int {
    // 1、初始化start、end
    start := 0
    end := len(nums) - 1
    // 2、处理for循环
    for start+1 < end {
        mid := start + (end-start)/2
        // 3、比较a[mid]和target值
        if nums[mid] == target {
            end = mid
        } else if nums[mid] < target {
            start = mid
        } else if nums[mid] > target {
            end = mid
        }
    }
    // 4、最后剩下两个元素，手动判断
    if nums[start] == target {
        return start
    }
    if nums[end] == target {
        return end
    }
    return -1
}
```



二分查找还有一些其他模板如下图，大部分场景模板#3 都能解决问题，而且还能找第一次/最后一次出现的位置，应用更加广泛

![img](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210325203719.png)

如果是最简单的二分搜索，不需要找第一个、最后一个位置、或者是==没有重复元素==，可以使用模板#1，代码更简洁

```go
// 无重复元素搜索时，更方便
func search(nums []int, target int) int {
    start := 0
    end := len(nums) - 1
    for start <= end {
        mid := start + (end-start)/2
        if nums[mid] == target {
            return mid
        } else if nums[mid] < target {
            start = mid+1
        } else if nums[mid] > target {
            end = mid-1
        }
    }
    // 如果找不到，start 是第一个大于target的索引
    // 如果在B+树结构里面二分搜索，可以return start
    // 这样可以继续向子节点搜索，如：node:=node.Children[start]
    return -1
}
```



## 练习题

- [search-for-range](https://www.lintcode.com/problem/search-for-a-range/description)
- [search-insert-position](https://leetcode-cn.com/problems/search-insert-position/)
- [search-a-2d-matrix](https://leetcode-cn.com/problems/search-a-2d-matrix/)
- [first-bad-version](https://leetcode-cn.com/problems/first-bad-version/)
- [find-minimum-in-rotated-sorted-array](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/)
- [find-minimum-in-rotated-sorted-array-ii](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array-ii/)
- [search-in-rotated-sorted-array](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)
- [search-in-rotated-sorted-array-ii](https://leetcode-cn.com/problems/search-in-rotated-sorted-array-ii/)