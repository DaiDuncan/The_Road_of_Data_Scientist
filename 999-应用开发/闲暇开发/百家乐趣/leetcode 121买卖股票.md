[博客园](https://www.cnblogs.com/gongyanzh/p/12881741.html)

# 买卖股票的最佳时机

LeetCode入口👉👉👉[No.121](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)

给定一个数组，它的第 *i* 个元素是一支给定股票第 *i* 天的价格。

如果你**最多只允许完成一笔交易**（即买入和卖出一支股票一次），设计一个算法来计算你所能获取的最大利润。

注意：你不能在买入股票前卖出股票。



**示例 1:**

```python
输入: [7,1,5,3,6,4]
输出: 5
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格；同时，你不能在买入前卖出股票。
```



**示例 2:**

```python
输入: [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。
```



**思路**

![image-20200513101058817](https://gitee.com/gongyanzh/blogpic/raw/master/pictures/image-20200513101058817.png)

最大收益即最大卖出价格与最低买入价格之差（最低点买入，最高点卖出）

使用两个变量，分别表示 目前为止得到的最大收益、目前为止的最低价格

```python
maxprofit = max(maxprofit, price - minprice)
minprice = min(minprice, price)
```

**coding**

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        if not prices:
            return 0
        maxprofit = 0
        minprice = prices[0]
        for price in prices:
            maxprofit = max(maxprofit, price - minprice)
            minprice = min(minprice, price)
        return maxprofit
```



# 买卖股票的最佳时机 II

LeetCode入口👉👉👉[No.122](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/)

给定一个数组，它的第 *i* 个元素是一支给定股票第 *i* 天的价格。

设计一个算法来计算你所能获取的最大利润。你可以**尽可能地完成更多的交易**（**多次买卖一支股票**）。

**注意：**你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

**示例 1:**

```python
输入: [7,1,5,3,6,4]
输出: 7
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 3 天（股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。
     随后，在第 4 天（股票价格 = 3）的时候买入，在第 5 天（股票价格 = 6）的时候卖出, 这笔交易所能获得利润 = 6-3 = 3 。
```

**示例 2:**

```python
输入: [1,2,3,4,5]
输出: 4
解释: 在第 1 天（股票价格 = 1）的时候买入，在第 5 天 （股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。
     注意你不能在第 1 天和第 2 天接连购买股票，之后再将它们卖出。
     因为这样属于同时参与了多笔交易，你必须在再次购买前出售掉之前的股票。
```



**思路**

- 峰谷法

在上一题只允许一次交易，最大利润为 C，如果可以多次交易，找到==连续的峰和谷==，一定满足 A+B>C （如下图）

$TotalProfit=∑_i (peak_i−valley_i)$



![image-20200513105405559](https://gitee.com/gongyanzh/blogpic/raw/master/pictures/image-20200513105405559.png)

- 贪心💡(高频交易的本质)

只要出现一次爬升就计算利润

![image-20200513111848361](https://gitee.com/gongyanzh/blogpic/raw/master/pictures/image-20200513111848361.png)

**coding**

```python
#峰谷法
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        if not prices:
            return 0
        maxprofit = 0
        i = 0
        while i < len(prices)-1:
            while i < len(prices)-1 and prices[i] >= prices[i+1]:
                i += 1
            valley = prices[i]  #is valley
            while i < len(prices)-1 and prices[i] <= prices[i+1]:
                i += 1    
            peak = prices[i]    #is peak
            
            maxprofit += peak - valley

        return maxprofit
#贪心
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        if not prices:
            return 0
        maxprofit = 0
        for i in range(1,len(prices)):
            if prices[i] > prices[i-1]:
                maxprofit += prices[i] - prices[i-1]

        return maxprofit
```



# 买卖股票的最佳时机 III

LeetCode入口👉👉👉[No.123](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iii/)

给定一个数组，它的第 *i* 个元素是一支给定的股票在第 *i* 天的价格。

设计一个算法来计算你所能获取的最大利润。你最多可以完成 **两笔 交易**。

**注意:** 你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

**示例 1:**

```python
输入: [3,3,5,0,0,3,1,4]
输出: 6
解释: 在第 4 天（股票价格 = 0）的时候买入，在第 6 天（股票价格 = 3）的时候卖出，这笔交易所能获得利润 = 3-0 = 3 。
     随后，在第 7 天（股票价格 = 1）的时候买入，在第 8 天 （股票价格 = 4）的时候卖出，这笔交易所能获得利润 = 4-1 = 3 。
```



**思路**：动态规划

状态定义：

```python
dp[i][k][0/1]   在第i天，最大交易次数为k，持有状态为0/1时的最大收益
```

状态转移：

```python
dp[i][k][0] = max(dp[i-1][k][0], dp[i-1][k][1] + prices[i])		#同i-1天的状态，或者第i天卖掉了
dp[i][k][1] = max(dp[i-1][k][1], dp[i-1][k-1][0] - prices[i])	#同i-1天的状态，或者第i-1天买入
```



初始化：

```python
#第0天，没有交易，没持有股票，收益为0；持有股票，不可能事件
dp[0][k][0] = 0
dp[0][k][1] = -infinity

#最大交易次数为0，没有交易，没持有股票，收益为0；持有股票，不可能事件
dp[i][0][0] = 0
dp[i][0][1] = -infinity
```

**coding**

```python
#动态规划
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        if not prices:
            return 0
        k = 2
        n = len(prices)
        dp = [[[0]*2 for _ in range(k+1)] for _ in range(n+1)] #(n+1)*(k+1)*2: 下标0作为辅助元素
        
        #init
        for i in range(n+1):#交易次数为0
            dp[i][0][0] = 0
            dp[i][0][1] = float('-inf')

        for j in range(k+1):#第0天
            dp[0][j][0] = 0
            dp[0][j][1] = float('-inf')
        
        #==# dp方法确实优雅
        for i in range(1,n+1):
            for j in range(1,k+1):
                #1 表示持有
                dp[i][j][0] = max(dp[i-1][j][0],dp[i-1][j][1]+prices[i-1])
                dp[i][j][1] = max(dp[i-1][j][1],dp[i-1][j-1][0]-prices[i-1])

        return dp[-1][-1][0]
```



# me|总结

涉及状态在序列中产生变化的的情境，都可以尝试用动态规划： => 形式很优雅

- 扔鸡蛋
- viterbi算法
- 买卖股票