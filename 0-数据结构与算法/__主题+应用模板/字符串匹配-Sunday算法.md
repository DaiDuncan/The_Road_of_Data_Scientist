## Sunday算法|类中的独立函数

Accepted

- 79/79 cases passed (24 ms)
- Your runtime beats 39.59 % of python submissions
- Your memory usage beats 78.16 % of python submissions (13.7 MB)

```python
#
# @lc app=leetcode id=28 lang=python
#
# [28] Implement strStr()
#

# @lc code=start
class Solution(object):
    def findShiftPos(self, chr, needle):
        shiftPos = 0
        for chrNeedle in needle[::-1]:
            shiftPos += 1
            if chr == chrNeedle:
                return shiftPos
        return shiftPos

    def strStr(self, haystack, needle, index=0):
        """
        :type haystack: str
        :type needle: str
        :rtype: int
        """
        '''暴力匹配
        # 如果needle是空字符串
        size_needle = len(needle)
        size_haystack = len(haystack)

        if size_needle == 0:
            return 0
        
        # 下列第一个for循环直接跳出
        #elif size_haystack < size_needle:
        #    return -1
        # 两个for循环
        # 1：外循环i 范围：-len(needle)
        # 2：内循环j
        # 检测haystack[i+j] == needle[j]
        #i, j = 0, 0
        for i in range(size_haystack - size_needle + 1):
            j = 0
            while(haystack[i+j] == needle[j]):
                j += 1
                if j == size_needle:
                    return i
        return -1
        '''
        
		# Sunday算法
        if needle == "":
            return 0

        if index + len(needle) > len(haystack) :
            return -1

        idx = 0
        while(haystack[index + idx] == needle[idx]):
            idx += 1
            if idx == len(needle):  #完全匹配
                return index
        
        # 没有下一个char
        if index + len(needle)  >= len(haystack):
            return -1
        else:
            shiftPos = self.findShiftPos(haystack[index + len(needle)], needle)
            #print(shiftPos)
        return self.strStr(haystack, needle, index+shiftPos)

# @lc code=end
```



## 删除函数后

性能更差

Accepted

- 79/79 cases passed (24 ms)
- Your runtime beats 39.59 % of python submissions
- Your memory usage beats 56.94 % of python submissions (13.8 MB)

内存差的原因：可能是因为函数调用之后就删除了

