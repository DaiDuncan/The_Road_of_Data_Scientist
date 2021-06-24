[link: 实验探索](https://lecture-demo.ira.uka.de/neural-network-demo/?preset=Binary%20Classifier%20for%20XOR)

- 数据集Binary Classifier for XOR

![image-20210508114652982](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210508114702.png)

![image-20210508114742679](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210508114743.png)

注意：上述隐藏层和输出层神经元是sigmoid

1 如果两个都是linear，无法完成正确分类

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
%matplotlib inline

#从输入层到隐藏层
mat_1 = np.array([[5.95, 6.72],[3.50, 3.60]])	
#从隐藏层到输出层
mat_2 = np.array([7.61, -8.36])
#从输入层到隐藏层的偏置
b_1 = np.array([-2.56, -5.41])
#从隐藏层到输出层的偏置
b_2 = np.array([-3.37])

xor_points = [np.array([0, 0]), np.array([0, 1]), np.array([1, 0]), np.array([1, 1])]
x = []
y = []
for point in xor_points:
    num1, num2 = (mat_1@point+b_1)[0], (mat_1@point+b_1)[1]
    print(num1, num2)
    x.append(num1)
    y.append(num2)
    
plt.scatter(x, y)


### 输出结果：对输入样本进行变换之后
-2.56 -5.41
4.16 -1.81
3.39 -1.9100000000000001
10.11 1.6899999999999995
```

![image-20210508115016969](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210508115021.png)

直观上看散点图：无法完成线性分类

而且从计算公式来看：从隐藏层到输出层仍然是矩阵 => ==矩阵本质是线性变换==

```python
plt.scatter([0,0,1,1], [0,1,0,1])
plt.plot([0, 0.5],[0.5,0])
plt.plot([0, 1.5],[1.5,0])
plt.show()
```

![image-20210508115548262](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210508115554.png)

针对蓝色、橙色的线：蓝色的函数曲线表示为$f(x, y)=x + y -\frac{1}{2}$

- 左下为负号，右上为正号

- 1）最初的直觉是：改变橙色线的正负号，然后中间区域都是正号，两边区域都是负号
- 2）实际操作：在橙色线的正负号改变之后，
  - 点(0,0)(应该是负号)，由于橙色线的关系，导致最后(0,0)以正号的“权重”为主，最终分类错误
  - 右上角的点(1,1)同理



## 分析|是否线性可分？



分析原因：

1. 分类错误的点的权重由于线性分界线的关系，离哪条分界线更近，影响更大
2. 改变分界线的正负号时：影响也和距离挂钩



==改进思考==|假如基于激活函数，非线性化 => 能否达成下列效果？

- 对于分类错误的点：距离越大，影响效果并不是那么明显(比如橙色分界线对(0,0)点的影响) => 还可以“抢救”挽回
- 即如下映射$g(D(x))$
  - D(x)表示矩阵的线性变换/映射
  - $g(\cdot)$表示激活函数(一般是非线性映射)：从而类似SVM的核函数，映射到高维空间，从而在高维空间“可分” 
    - 在下一层线性可分：输出层是linear
    - 在下一层非线性可分：配置激活函数







## 重点|矩阵表示

![image-20210508122654477](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210508122708.png)



# 补充|实验

聚焦隐藏层、输出层的激活函数

- linear
- sigmoid
- tanh
- ReLU
- leak ReLU
- softmax: 多输出/多分类

## 1 同一种激活函数

| 组合                   | 结果           |
| ---------------------- | -------------- |
| linear                 | 不可行         |
| sigmoid                | 可行           |
| tanh                   | 可行           |
| ReLU                   | 可行           |
| leak ReLU              | 可行           |
| softmax: 多输出/多分类 | 此数据集不适合 |





## 2 不同的激活函数

| 隐藏层   | 输出层      | 结果                                  |
| -------- | ----------- | ------------------------------------- |
| linear+  | sigmoid     | 不可行                                |
|          | tanh        | 不可行                                |
|          | relu        | 不可行                                |
|          | lrelu       | 不可行                                |
| sigmoid+ | linear      | 可行                                  |
|          | 其余均可行  |                                       |
| tanh+    | linear      | 可行                                  |
|          | ==sigmoid== | ==不可行== 说明tanh非线性化的能力有限 |
|          | 其余均可行  |                                       |
| relu+    | linear      | 可行                                  |
|          | 其余均可行  |                                       |
| lrelu+   | linear      | 可行                                  |
|          | ==tanh==    | ==不可行==                            |
|          | sigmoid     | 很纠结 => 最后过程突然学顺了          |





# 总结|非线性映射 => 线性可分

## 1 [双曲线(椭圆)](https://kknews.cc/zh-cn/code/8xzvk34.html?__cf_chl_jschl_tk__=00788ea0f33539080118ea5b505a9c97c5af50fe-1620470272-0-ASymJvMUSpgpTUsUfFRNV-ifAF_K2mZ_rglzlj1K1qmMafJ_zJ-ufy9vNLs-zUpwSwnrP8xQxzP5N_dUUmnv5z6musaTnSXxw0q2K5xSE-9yE0CjNUFs8tCjSQQOCDpBbyLoXhKOmKFF1DkqpqKhK55-evHEMFqHJw2mGnYf78tz6fipx4mSve21lBazh3CmaNiFzSpRtdLB4rPizAZka0PXJ_nqI3uC5jJ0n8gbqTZi9lqq92gYp2QLfCYRaItRJufnNLtln_eyqpmu6P9AmzLUgXiu3awiWja2kVx0PzkJ25hUzzmY0FNSjg7WtXAJcxhQWwUrojV7XDAYjiq8kcstG2190KSrOCQMZJBBwSW7PjfdNNab7LGd1T1u9chB0UapQC1gvEYLY-ZXhg4N_JWAqkWdhUTrhTPCF2_PWNgv)

![image-20210508123847472](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210508123859.png)

这种变换可以理解为引入了一个非线性变换函数∅（·）将$R^n$空间的样本X映射到$R^m$空间,其中n<<m

可以看到图左其实可以用一个二次曲线来进行划分，方程运算式可以写为：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210508124059.png" alt="image-20210508124051561" style="zoom:80%;" />

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210508124209.png" alt="image-20210508124151233" style="zoom:80%;" />

## 2 [直线到抛物线](http://fangs.in/post/thinkstats/svm1/)

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210508124600.png" alt="img" style="zoom:80%;" />



## 3 [理论SVM|西瓜书](https://zhuanlan.zhihu.com/p/156908124)

