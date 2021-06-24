模式分类的途径主要分为以下三种：

1 估计类条件概率密度 ![[公式]](https://www.zhihu.com/equation?tex=p%28x%7Cw_i%29) ，利用贝叶斯规则计算后验概率 ，然后通过最大后验概率做出决策。

可以采用两种方法对概率密度进行估计：

- 1a：概率密度参数估计：基于对![[公式]](https://www.zhihu.com/equation?tex=p%28x%7Cw_i%29)的**含参数的描述**
  - 主要有最大似然估计和贝叶斯估计

- 1b：概率密度非参数估计：基于对![[公式]](https://www.zhihu.com/equation?tex=p%28x%7Cw_i%29)**的非参数的描述**
  - 主要有==Parzen窗方法==



2 直接估计后验概率 ![[公式]](https://www.zhihu.com/equation?tex=p%28x%7Cw_i%29) 的

- 主要有K近邻方法等

 

3 直接计算判别函数，不需要估计 ![[公式]](https://www.zhihu.com/equation?tex=p%28x%7Cw_i%29) 

- 常见的方法有感知器等



