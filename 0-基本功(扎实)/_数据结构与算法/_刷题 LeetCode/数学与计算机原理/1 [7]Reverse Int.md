链接|题目描述：[link](https://leetcode.com/problems/reverse-integer/solution/)



## Q：







## Example:

```

```





## 特殊的限制条件











# me

思路1：没有理解题意(数范围的限制)

- 判断正负号 => 没有必要
- 指数次幂 => 也没有必要

```python
class Solution(object):
    def reverse(self, x):
        """
        :type x: int
        :rtype: int
        """
        if x < 0:
            flag = -1
        else:
            flag = 1

        data_abs = x * flag

        res = 0
        power = len(str(data_abs)) - 1
        while (data_abs != 0):    #divisor and remainder,power
            remainder = data_abs % 10
            res += remainder * (10 ** power)

            power -= 1
            data_abs = data_abs // 10

        return res * flag
```





==思路2==：参考C++标准答案之后(考虑 数表示范围的限制)

```python
# import sys
# sys.maxsize	在64位系统上是9223372036854775807
# 2 ** 31 == 2147483648


class Solution(object):
    def reverse(self, x):
        """
        :type x: int
        :rtype: int
        """
        if x < 0:
            sign = -1
            x = -x  #absolut value
        else:
            sign = 1

        res = 0
        MAX = 2 ** 31
        # divisor and remainder,power
        while (x != 0):    
            remainder = x % 10
            x = x // 10
            
            if ((sign == 1) and ((res > MAX/10) or (res == MAX / 10 and remainder > 7))): 
                return 0
            if ((sign == -1) and ((res > MAX/10) or (res == MAX / 10 and remainder > 8))):
                return 0

            res = res *10 + remainder  

        return res * sign
```





# 参考答案

python：巧妙利用了字符串

- 但没有考虑问题的数学、计算机原理难点

```python
class Solution:
    def reverse(self, x: int) -> int:
        # convert to string, reverse, convert to int?
        x = str(x)
        x = x[::-1]
        if '-' in x:
            x = x.strip('-')
            x = -1*int(x)
        else:
            x = int(x)
        if x >= (-2)**31 and x <= (2**31) - 1:
            return x
        else:
            return 0
```



C++：见总结|关键点1



# 总结|关键点

## 1 数表示范围的限制⭐

计算机无法直接表示这个数：需要在之前作判断，针对判断结果 巧妙地处理

![image-20210305093549926](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210305093550.png)

```c++
class Solution {
public:
    int reverse(int x) {
        int rev = 0;
        while (x != 0) {
            int pop = x % 10;
            x /= 10;
            if (rev > INT_MAX/10 || (rev == INT_MAX / 10 && pop > 7)) return 0;
            if (rev < INT_MIN/10 || (rev == INT_MIN / 10 && pop < -8)) return 0;
            rev = rev * 10 + pop;
        }
        return rev;
    }
};
```





## 2 套路|整数倒序

```python
res = 0
while (x != 0):    
	remainder = x % 10
	x = x // 10
	
	res = res * 10 + remainder  
```





# #补充

## 知识点

基本运算：//  % 

逻辑运算：python中是and or not(C++是&& ||) ===> typora笔记更新迭代==





1 英语词汇：divisor and remainder,power

2 [sys.maxsize: 官网](https://docs.python.org/3.1/library/sys.html#sys.maxsize)

```python
import sys
MAX_INT=sys.maxsize
print(MAX_INT)
```

3 python除法、取余

[link: 在线C++](https://c.runoob.com/compile/12)

```c++
//C++
#include <iostream>
using namespace std;

int main()
{
    int res;
	res = -321 / 10;
	cout << res;	//结果是-32，而在python中是-33
	
    return 0;
}
```



[link: CSDN|Python 关于整除以及负数取余遇到的问题](https://blog.csdn.net/sun___M/article/details/83142126)

Python的底层机制：r=a-n*[a//n]  这里r是余数，a是被除数，n是除数。

- 不过在“a//n”这一步，当a是负数的时候，我们上面说了，会==向下取整==，也就是说向负无穷方向取整
- -123%10 = -123 - 10 * (-123 // 10) = -123 - 10 * (-13) = 7

Python3中，除法有 “/” 以及 “//” 两种，这两个有着明显的区别

```python
print(18//10)			#1
print(12/10)			#1.2 单斜杠除法：结果是小数
print(-12/10)			#-1.2
print(12/-10)			#-1.2
print(12//-10)			#-2 向下取整

print(int(-12/10))		# -1：相当于-1.2再转化为整型
print(-13//10)			# -2


print(int(-123 % -10))	# -3：多出来的部分是负数
print(-123%10)			#7
print(-123%-10)			#-3
print(123%10)			#3

print(-123 // 10)		#-13
print(123%-10)			#-7
————————————————
版权声明：本文为CSDN博主「Samuel_0」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/sun___M/article/details/83142126
```