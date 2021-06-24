# 特性

## 线性？

logistic 回归的决策边界（decision boundary）=> 两者判别概率相等
$$
P(Y=1|x, \theta) = P(Y=0|x, \theta) \\
\frac{1}{1+e^{-\theta^T \cdot x}} = 1 - \frac{1}{1+e^{-\theta^T \cdot x}} \\
1 = e^{-\theta^T \cdot x} \\
\theta^T \cdot x = 0
$$


逻辑回归之所以说是线性模型，是因为其的分类boundary是线性的，直观的说，就是一条线把所有的点分成两类。

但是这不是说output是线性，logistic 回归函数的output也就是activate之后并传到下一层就是非线性的。

所有整个NN(使用sigmoid激活函数)就是非线性的。

分类模型主要还是看decision boundary是否为线性。

