two-pointers | string<br />

## Q：
Return the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.
## Example:

```python
Input: haystack = "hello", needle = "ll"
Output: 2
    
Input: haystack = "aaaaa", needle = "bba"
Output: -1
    
Input: haystack = "", needle = ""
Output: 0
```
## 特殊的限制条件

- `0 <= haystack.length, needle.length <= 5 * 10`
- `haystack` and `needle` consist of only lower-case English characters.

# me

## 方法1|暴力匹配

- 79/79 cases passed (28 ms)
- Your runtime beats 30.29 % of python submissions
- Your memory usage beats 98.78 % of python submissions (13.5 MB)
```python
class Solution(object):
    def strStr(self, haystack, needle):
        """
        :type haystack: str
        :type needle: str
        :rtype: int
        """
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
            while(j <size_needle and haystack[i+j] == needle[j]):
                j += 1
            if j == size_needle:
                return i
        return -1
```

- 79/79 cases passed (16 ms)
- Your runtime beats 83.04 % of python submissions
- Your memory usage beats 57.65 % of python submissions (13.7 MB)
```python
            #while循环的修改：因为j<size_needle每次都要判断 => 干脆直接判断j==size_needle
    		while(haystack[i+j] == needle[j]):
                j += 1
                if j == size_needle:
                    return i
```


## 方法2|KMP(C++)

@主题+应用模版 `字符串匹配-KMP算法`(C++实现)

Accepted

- 79/79 cases passed (4 ms)
- Your runtime beats 77.04 % of cpp submissions => ==KMP算法还是有用的==
- Your memory usage beats 32.38 % of cpp submissions (6.8 MB)



## 方法3|Sunday(Python)⭐

Accepted

- 79/79 cases passed (16 ms)
- Your runtime beats 82.93 % of python submissions
- Your memory usage beats 98.67 % of python submissions (13.5 MB)



技巧1：用模式串的字典记录字符在模式串中的位置

技巧2：python字典取元素，dict.get(key, defaultValue)

技巧3：小的细节 以`“hello”+"ll"`为例

- `if j == len_needle:` 匹配后直接返回，否则j+=1，超出needle[j]下标越界
- `if i + len_needle == len_haystack:`末端对齐匹配：需要先判断匹配，如果不匹配，直接返回`-1`

```python
class Solution(object):
    def strStr(self, haystack, needle):
        #@@ sunday算法：两个for循环
        len_needle = len(needle)
        len_haystack = len(haystack)

        if len_needle == 0:
            return 0
        
        # 构造模式串的字典
        # TODO: 将模式串字符与位置存储为字典，value是位置的最大值(如果遇到相同字符)
        pos_needle = dict()
        for idx, chr in enumerate(needle):
            pos_needle[chr] = idx

        i = 0
        while (i <= len_haystack - len_needle):
            j = 0
            #test#print(i, j, -1)
            while(haystack[i+j] == needle[j]):
                #test#print(i, j, 2)
                j += 1

                if j == len_needle:
                    return i
            
            # 最后的匹配(需要判断是否匹配，然后再做决定 => 没有了下一个字符，返回1)
            if i + len_needle == len_haystack:
                return -1
            next_chr = haystack[i + len_needle]
            pos = pos_needle.get(next_chr, 0)
            # 更新移位i += len - pos
            i += len_needle - pos
                
        return -1
```

### 小结|Sunday本质(技巧的进一步抽象)

暴力匹配从前往后

Sunday匹配从后往前：需要的只是思考后面字符在不同情境下的处理应对



注意：最坏的情况

```
haystack: aaaaaaa
neddle: baa
```



# 参考答案|暴力匹配

时间复杂度：O(N*M)<br />空间复杂度：O(1)

Accepted

- 79/79 cases passed (20 ms)
- Your runtime beats 61.97 % of python submissions
- Your memory usage beats 6.47 % of python submissions (14.8 MB)

```python
def strStr(self, haystack, needle):
    if needle == "":
        return 0
    for i in range(len(haystack)-len(needle)+1):
        for j in range(len(needle)):
            if haystack[i+j] != needle[j]:
                break
            if j == len(needle)-1:
                return i
    return -1
```
## 奇葩方法

### 1 split⭐

Accepted

- 79/79 cases passed (20 ms)
- Your runtime beats 61.97 % of python submissions
- Your memory usage beats 77.83 % of python submissions (13.6 MB)

```python
def strStr(self, haystack: str, needle: str) -> int:
    if len(needle) == 0:
        return 0
    elif needle not in haystack:
        return -1
    else:
        return len(haystack.split(needle)[0])
```



### 2 整体匹配

```python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        haystack_len = len(haystack)
        needle_len = len(needle)

        if needle == '':
            return 0
        
        for i in range(haystack_len - needle_len + 1):
            # 字符串切片整体
            if haystack[i:i+len(needle)] == needle:
                return i
            
        return -1
```



```python
return 0 if needle=="" else -1 if needle not in haystack else haystack.index(needle)
```



```python
class Solution(object):
    def strStr(self, haystack, needle):
        if needle == " ":
            return 0
        # 字符串查找
        if needle in haystack:
            return haystack.index(needle)	#返回最先匹配的索引
        
        else:
            return -1
```



# #总结|关键点