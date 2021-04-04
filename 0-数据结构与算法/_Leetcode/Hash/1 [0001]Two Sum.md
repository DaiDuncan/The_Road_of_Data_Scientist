**1 [1]Two Sum👍**

- `array` 

- `hash-table`



## Q：



Given an array of integers, return indices of the two numbers such that they add up to a specific target.



You may assume that each input would have exactly one solution.



## Example：



```python
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```



### UPDATE (2016/2/13)：



The return format had been changed to zero-based indices. Please read the above updated description carefully.



# me



思路：



- 先建立hash表
- 考虑特殊情况：两个数相同(在字典中会被覆盖)

- - 所以defaultdict(list)：字典的元素默认为列表



```python
from collections import defaultdict
class Solution(object):
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """

        nums_dict = defaultdict(list)
        for index, num in enumerate(nums):
            nums_dict[num].append(index)


        for num in nums:
            index_1 =  nums_dict[num].pop()
            if nums_dict[target - num]:
                index_2 = nums_dict[target - num][0]

                return [index_1, index_2]
        return None
```



# 参考答案

思路：

- 一共两个数，先拎出一个数
- 剩下的差值在字典中寻找(值作为字典的key值)

字典特性：单个值Hash查找

```python
class Solution(object):
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        dict = {}
        for i in range(len(nums)):
            x = nums[i]
            if target - x in dict:
                return (dict[target - x], i)
            dict[x] = i
```







