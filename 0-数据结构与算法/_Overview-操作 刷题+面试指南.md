数据结构是一种数据的表现形式，如链表、二叉树、栈、队列等都是内存中一段数据表现的形式。 

算法是一种通用的解决问题的模板或者思路，**大部分数据结构都有一套通用的算法模板**，所以掌握这些通用的算法模板即可解决各种算法问题。



# 训练经验

备注

0 基本功@Udacity

- 找写得好的博客专栏也可以

1 分类总结的材料

- [link: 博客|github项目 算法专栏gitbok](https://algorithm.yuanbin.me/zh-hans/problem_misc/)
  - Part I为数据结构和算法基础，介绍一些基础的排序/链表/基础算法
  - Part II为 OJ 上的编程题目实战
  - Part III 为附录部分，包含如何写简历和其他附加材料如系统设计

- 书籍：labuladong的算法小抄
- 书籍：算法leetcode cookbook
- 经验汇总：公子龙刷题指南



## 如何练习算法？

- 代码块可为三大块：
  - ==异常处理（空串和边界处理）==
  - 主体
  - 返回
- 代码风格(可参考Google的编程语言规范)
  1. 变量名的命名(有意义的变量名)
  2. 缩进(语句块)
  3. ==空格(运算符两边)==
  4. ==代码可读性(即使if语句只有一句也要加花括号)==
- 《代码大全》中给出的参考



而对于实战算法的过程中，我们可以采取如下策略：

1. **总结归类**相似题目
2. 找出适合同一类题目的**模板程序**
3. 对基础题熟练掌握



## 资料参考(含平台)

[gitbook推荐资料](https://algorithm.yuanbin.me/zh-hans/)

- [LeetCode Online Judge](https://leetcode.com/) 
- [LintCode](http://www.lintcode.com/) 
- [hihoCoder](http://hihocoder.com/) 
- [LeetCode题解 - GitBook](https://www.gitbook.com/book/siddontang/leetcode-solution/details) 
- [VisuAlgo - visualising data structures and algorithms through animation](http://www.comp.nus.edu.sg/~stevenha/visualization/index.html)  相当厉害的数据结构和算法可视化网站
- [Data Structure Visualization](http://www.cs.usfca.edu/~galles/visualization/Algorithms.html) - 同上，非常好的动画演示！！涵盖了常用的各种数据结构/排序/算法。
- [HiredInTech](http://www.hiredintech.com/) - System Design 的总结特别适合入门。



百练poj：北京大学计算机系的内部练习平台

- ACM国际大学生
- NOI青少年信息学

# 刷题指南

[Github链接：刷题指南](https://github.com/greyireland/algorithm-pattern)

- 项目是自己找工作时，从 0 开始刷 LeetCode 的心得记录，通过各种刷题文章、专栏、视频等总结了一套自己的刷题模板。
- 从 4 月份找工作开始，从 0 开始刷 LeetCode，中间大概花了一个半月(6 周)左右时间刷完 240 题。

## 问题1|刷多少题？

面试到底要刷多少题，其实这个取决于你想进什么样公司，你定的目标如果是国内一线大厂，个人感觉大概 200 至 300 题基本就满足大部分面试需要了。



## 问题2|按照什么顺序刷题？⭐

- 先刷[基础算法题](https://greyireland.gitbook.io/algorithm-pattern/)
- 再刷 LeetCode 的[分类算法题](https://leetcode-cn.com/explore/)
- 最后再刷 LeetCode 中的[剑指 Offer](https://leetcode-cn.com/problemset/lcof/)



发现按题型刷题会舒服很多，基本一个类型的题目，一天能做很多，慢慢刷题也不再枯燥



按此[ repo 目录](https://github.com/greyireland/algorithm-pattern)刷一遍，如果中间有题目卡住了先跳过，然后刷题一遍 LeetCode 探索基础卡片，最后快要面试时刷题一遍剑指 offer。

- repo 里面的题目是按类型归类，都是一些常见的高频题，很有代表性，大部分都是可以用模板加一点变形做出来，刷完后对大部分题目有基本的认识。
- 然后刷一遍探索卡片，巩固一下一些基础知识点，总结这些知识点。
- 最后剑指 offer 是大部分公司的出题源头，刷完面试中基本会遇到现题或者变形题，基本刷完这三部分，大部分国内公司的面试题应该就没什么问题了~



如果打算准备面试了，建议前面两部分 一个半月 （6 周）时间刷完，最后剑指 offer 半个月刷完，边刷可以边投简历进行面试，遇到不会的不用着急，往模板上套就对了，如果面试官给你提示，那就好好做，不要错过这大好机会~



刷题注意点：如果为了找工作刷题，遇到 hard 的题如果有思路就做，没思路先跳过，先把基础打好，再来刷 hard 可能效果会更好~

很多事情你干了很多，但你还是不一定懂，只有靠你自己不断总结归纳，那些知识才最终属于你。



### 参考|刷题顺序

[github|C++后台开发工程师](https://github.com/youngyangyang04)

**数组->链表->哈希表->字符串->栈与队列->二叉树->回溯->==贪心->动态规划->图论==->高级数据结构**





## 参考|刷题记录

[leetcode|cmc: 八皇后DFS简便方法 2018.10.26](https://leetcode.com/cmc/)

![image-20210330115014440](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210330115014.png)



# 面试注意点

大多数时候，刷算法题可能都是为了准备面试，所以面试的时候需要注意一些点

- 快速定位到题目的知识点，找到知识点的**通用模板**，可能需要根据题目**特殊情况做特殊处理**。
- 先去朝一个解决问题的方向！**先抛出可行解**，而不是最优解！先解决，再优化！
- 代码的风格要统一，熟悉各类语言的代码规范。
  - 命名尽量简洁明了，尽量不用数字命名如：i1、node1、a1、b2
- 常见错误总结
  - 访问下标时，不能访问越界
  - 空值 nil 问题 run time error



## 流程|Udacity

1 Clarifying the Question

2 Confirming Inputs

3 Test Cases★

4 Brainstorming★

5 Runtime Analysis

6 Coding

7 Debugging