## 问题

一

1 Q：为什么均值估计不可以？直觉上可行



二

1 Q：==临界点都是后验概率相等== => 为什么呢？



---

# 一、最大似然

前提：

- 已知分布
- 从分布中**独立**提取样本 => 概率密度相乘



形式：

- likelihood $P(x_1, x_2, ..., x_n|\lambda) = \Pi_{i=1}^{n} P(x_i)$
  - 针对离散分布：几何分布 => 概率分布
  - 针对==连续分布：均匀分布 => 概率密度分布==
    - <img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210512174950.png" alt="image-20210512174949730" style="zoom: 50%;" />
    - 关键：理解似然函数 => 样本使得似然函数的概率最大 => 上述似然函数最大化：$\theta$取可能范围内的最小值 => 满足样本即可(也就是概率密度的定义域)
    - Q：为什么均值估计不可以？直觉上可行
- log-likelihood $log(P(x_1, x_2, ..., x_n|\lambda))$

- Maximum Likelihood：

![image-20210604154708545](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210604154708.png)



## Q|评价均匀分布的估计

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210519133639.png" alt="image-20210519133638266" style="zoom:67%;" />

[2]https://blogs.sas.com/content/iml/2017/11/27/method-of-moments-estimates-mle.html
[3]https://en.wikipedia.org/wiki/German_tank_problem  

## 总结|通用

### 1 常见应用|高斯分布估计参数

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210512164444.png" alt="image-20210512164444148" style="zoom: 50%;" />



### 2 复习|MSV@教科书

1 参数估计|贝叶斯估计

- MAP
- ML

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210512164614.png" alt="image-20210512164614183" style="zoom:67%;" />

![image-20210512164649347](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210512164649.png)

主要的两个因素：

- 参数b
- 噪声e

![image-20210519134326048](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210519134326.png)



# 二、分类错误的概率

Q：==临界点都是后验概率相等== => 为什么呢？

- $p(w_1|x)=p(w_2|x)$
- 针对风险决策：$R(w_1|x) = R(w_2|x)$

图解法：

- 当后验概率在交点的时候：错误决策(阴影面积最小)
  - 不妨将$\theta$左右移动 => 阴影面积必然增大

![image-20210519131716900](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210519131717.png)





重点：形象理解概念

- 先验概率 $p(w)$
- 条件/似然 $p(x|w)$
- 后验概率 $p(w|x)$

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210512165008.png" alt="image-20210512165008502" style="zoom:67%;" />



Q：分类号之后，样本数据出现很高的错误率 => 可能的原因？

1. 模型本身：数据集不符合模型假设(正态分布的假设)
2. 样本数据：样本数据量太少，导致模型参数的估计出现偏差 => 欠拟合



1 Minimum Risk classification ==风险决策：取决于具体的应用 => 聚焦哪一类错误==

> 理解描述：
>
> 问："Deswegen entscheiden Sie jetzt, dass einen Datenpunkt
> **fälschlicherweise in ω2 zu klassifizieren** um einen Faktor κ so schlimm ist, wie eine Fehlklassifikation in ω1"
>
> 答：we set the ***cost*** of misclassifying an ω1 datapoint **into ω2** as K times as large as the cost of misclassifying the other way around.
>
> me|也就是说，错误分类到w2(假阴性：有病却没有检测出来 => 判定为阴性，实际却是假的)的损失是k倍

风险的定义：某种决策/行为导致损失的期望值

![image-20210519134456394](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210519134456.png)





- 医院检测：警惕假阴性(有病，却没有检测出来) => 添加**惩罚因子**
- $\lambda_{21}$表示样本是1，分类为2(假阴性)导致的风险因子

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210512180618.png" alt="image-20210512180617535" style="zoom:50%;" />



## 总结|通用

1 **含参上下限的积分**及其导数





# 三、Perceptron

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210518202607.jpeg" alt="img" style="zoom:50%;" />

> **感知机算法（原始形式）**：
> 输入：训练数据集 ![[公式]](https://www.zhihu.com/equation?tex=T+%3D+%5Cleft%5C%7B+%5Cleft%28+x_%7B1%7D%2C+y_%7B1%7D+%5Cright%29%2C+%5Cleft%28+x_%7B2%7D%2C+y_%7B2%7D+%5Cright%29%2C+%5Ccdots%2C+%5Cleft%28+x_%7BN%7D%2C+y_%7BN%7D+%5Cright%29+%5Cright%5C%7D) ，其中 ![[公式]](https://www.zhihu.com/equation?tex=x_%7Bi%7D+%5Cin+%5Cmathcal%7BX%7D+%3D+R%5E%7Bn%7D%2C+y_%7Bi%7D+%5Cin+%5Cmathcal%7BY%7D+%3D+%5Cleft%5C%7B+%2B1%2C+-1+%5Cright%5C%7D%2C+i+%3D+1%2C+2%2C+%5Ccdots%2C+N+) ；学习率 ![[公式]](https://www.zhihu.com/equation?tex=%5Ceta+%5Cleft%28+0+%3C+%5Ceta+%5Cleq+1+%5Cright%29) 。
>
> 输出： ![[公式]](https://www.zhihu.com/equation?tex=w%2Cb) ；感知机模型 ![[公式]](https://www.zhihu.com/equation?tex=f+%5Cleft%28+x+%5Cright%29+%3D+sign+%5Cleft%28+w+%5Ccdot+x+%2B+b+%5Cright%29)
>
> 1. 选取初值 ![[公式]](https://www.zhihu.com/equation?tex=w_%7B0%7D%2Cb_%7B0%7D)
>
> 2. 在训练集中==遍历选取==数据 ![[公式]](https://www.zhihu.com/equation?tex=%5Cleft%28+x_%7Bi%7D%2C+y_%7Bi%7D+%5Cright%29)
>
> 3. 如果 ![[公式]](https://www.zhihu.com/equation?tex=y_%7Bi%7D+%5Cleft%28+w+%5Ccdot+x_%7Bi%7D+%2B+b+%5Cright%29+%5Cleq+0)(也就是分类错误的点)
>    ![[公式]](https://www.zhihu.com/equation?tex=%5Cbegin%7Balign%2A%7D+%5C%5C%26+w+%5Cleftarrow+w+%2B+%5Ceta+y_%7Bi%7D+x_%7Bi%7D+%5C%5C+%26+b+%5Cleftarrow+b+%2B+%5Ceta+y_%7Bi%7D+%5Cend%7Balign%2A%7D+%5C%5C)
>
> 4. 转至2，直至训练集中没有误分类点。

优化：对偶形式的感知机算法

通过上一节感知机模型的算法原始形式 ![[公式]](https://www.zhihu.com/equation?tex=w+%3D+w+%2B+%5Ceta+y_%7Bi%7Dx_%7Bi%7D%2Cb%3Db%2B%5Ceta+y_%7Bi%7D) 可以看出，我们每次梯度的迭代都是选择的一个样本来更新 ![[公式]](https://www.zhihu.com/equation?tex=w%2Cb) 向量。最终经过若干次的迭代得到最终的结果。对于从来都没有误分类过的样本，他被选择参与 ![[公式]](https://www.zhihu.com/equation?tex=w%2Cb) 迭代的次数是0，对于被多次误分类而更新的样本 ![[公式]](https://www.zhihu.com/equation?tex=j) ，它参与 ![[公式]](https://www.zhihu.com/equation?tex=w%2Cb) 迭代的次数我们设置为 ![[公式]](https://www.zhihu.com/equation?tex=n_i) 。如果令 ![[公式]](https://www.zhihu.com/equation?tex=w%2Cb) 向量初始值为0向量， ![[公式]](https://www.zhihu.com/equation?tex=w%2Cb) 修改n次，则 ![[公式]](https://www.zhihu.com/equation?tex=w%2Cb) 关于 ![[公式]](https://www.zhihu.com/equation?tex=%5Cleft%28+x_%7Bi%7D%2C+y_%7Bi%7D+%5Cright%29) 的增量分别是 ![[公式]](https://www.zhihu.com/equation?tex=%5Calpha_%7Bi%7D+y_%7Bi%7D+x_%7Bi%7D) 和 ![[公式]](https://www.zhihu.com/equation?tex=%5Calpha_%7Bi%7D+y_%7Bi%7D) ，其中 ![[公式]](https://www.zhihu.com/equation?tex=%5Calpha_%7Bi%7D+%3D+n_%7Bi%7D+%5Ceta) 。 ![[公式]](https://www.zhihu.com/equation?tex=w%2Cb) 可表示为

![[公式]](https://www.zhihu.com/equation?tex=%5Cbegin%7Balign%2A%7D+%5C%5C%26+w+%3D+%5Csum_%7Bi%3D1%7D%5E%7BN%7D+%5Calpha_%7Bi%7D+y_%7Bi%7D+x_%7Bi%7D+%5C%5C+%26+b+%3D+%5Csum_%7Bi%3D1%7D%5E%7BN%7D+%5Calpha_%7Bi%7D+y_%7Bi%7D+%5Cend%7Balign%2A%7D+%5C%5C)
其中， ![[公式]](https://www.zhihu.com/equation?tex=%5Calpha_%7Bi%7D+%5Cgeq+0%2C+i%3D1%2C2%2C+%5Ccdots%2C+N)

> **感知机算法（对偶形式）：**
> 输入：训练数据集 ![[公式]](https://www.zhihu.com/equation?tex=T+%3D+%5Cleft%5C%7B+%5Cleft%28+x_%7B1%7D%2C+y_%7B1%7D+%5Cright%29%2C+%5Cleft%28+x_%7B2%7D%2C+y_%7B2%7D+%5Cright%29%2C+%5Ccdots%2C+%5Cleft%28+x_%7BN%7D%2C+y_%7BN%7D+%5Cright%29+%5Cright%5C%7D) ，其中 ![[公式]](https://www.zhihu.com/equation?tex=x_%7Bi%7D+%5Cin+%5Cmathcal%7BX%7D+%3D+R%5E%7Bn%7D%2C+y_%7Bi%7D+%5Cin+%5Cmathcal%7BY%7D+%3D+%5Cleft%5C%7B+%2B1%2C+-1+%5Cright%5C%7D%2C+i+%3D+1%2C+2%2C+%5Ccdots%2C+N+) ；学习率 ![[公式]](https://www.zhihu.com/equation?tex=%5Ceta+%5Cleft%28+0+%3C+%5Ceta+%5Cleq+1+%5Cright%29) 。
>
> 输出： ![[公式]](https://www.zhihu.com/equation?tex=%5Calpha%2Cb) ；感知机模型 ![[公式]](https://www.zhihu.com/equation?tex=f+%5Cleft%28+x+%5Cright%29+%3D+sign+%5Cleft%28+%5Csum_%7Bj%3D1%7D%5E%7BN%7D+%5Calpha_%7Bj%7D+y_%7Bj%7D+x_%7Bj%7D+%5Ccdot+x+%2B+b+%5Cright%29) ，其中 ![[公式]](https://www.zhihu.com/equation?tex=%5Calpha+%3D+%5Cleft%28+%5Calpha_%7B1%7D%2C+%5Calpha_%7B2%7D%2C+%5Ccdots%2C+%5Calpha_%7BN%7D+%5Cright%29%5E%7BT%7D)
> \1. ![[公式]](https://www.zhihu.com/equation?tex=%5Calpha+%5Cleftarrow+0%2C+b+%5Cleftarrow+0)
> \2. 在训练集中选取数据 ![[公式]](https://www.zhihu.com/equation?tex=%5Cleft%28+x_%7Bi%7D%2C+y_%7Bi%7D+%5Cright%29)
> \3. 如果 ![[公式]](https://www.zhihu.com/equation?tex=y_%7Bi%7D+%5Cleft%28+%5Csum_%7Bj%3D1%7D%5E%7BN%7D+%5Calpha_%7Bj%7D+y_%7Bj%7D+x_%7Bj%7D+%5Ccdot+x_%7Bi%7D+%2B+b+%5Cright%29+%5Cleq+0)
> ![[公式]](https://www.zhihu.com/equation?tex=%5Cbegin%7Balign%2A%7D+%5C%5C%26+%5Calpha_%7Bi%7D+%5Cleftarrow+%5Calpha_%7Bi%7D+%2B+%5Ceta+%5C%5C+%26+b+%5Cleftarrow+b+%2B+%5Ceta+y_%7Bi%7D+%5Cend%7Balign%2A%7D+%5C%5C)
>
> \4. 转至2，直至训练集中没有误分类点。

注意：第三个判断误分类的形式里面是计算两个样本 ![[公式]](https://www.zhihu.com/equation?tex=x_i) 和 ![[公式]](https://www.zhihu.com/equation?tex=x_j) 的内积，而且==这个内积计算的结果在下面的迭代次数中可以重用==。

如果我们事先用矩阵运算计算出所有的样本之间的内积，即用 ![[公式]](https://www.zhihu.com/equation?tex=Gram) 矩阵存储实例间内积 ![[公式]](https://www.zhihu.com/equation?tex=G+%3D+%5Cleft%5B+x_%7Bi%7D+%5Ccdot+x_%7Bj%7D+%5Cright%5D_%7BN+%5Ctimes+N%7D) ，那么在算法运行时，仅仅一次的矩阵内积运算比多次的循环计算省时。

计算量最大的判断误分类这儿就省下了很多的时间，这也是对偶形式的感知机模型比原始形式优的原因。





```python
import pandas as pd
import numpy as np

class Perceptron(object):
    def __init__(self, init_weights, learning_step=1):
        self.learning_step = learning_step
        self.w = init_weights
    
    def is_classified(self, xp, label):
        x_input = np.concatenate(([1], xp))
        wx = sum([self.w[j] * x_input[j] for j in range(len(self.w))])
        if int(wx >= 0) == label:
            print("Output value{}. klassifikation|{}: yes." .format(wx, label))
            return True
        else:
            print("Output value{}. klassifikation|{}: no." .format(wx, 1-label))
            return False
    
    def update(self, xp):
        self.w = self.w + self.learning_step * np.concatenate(([1], xp))
        
        
#==#
# print('Start read data')
raw_data = pd.read_csv('perceptron.csv', header=0)
dataset = raw_data.values
print(dataset)

init_weights = np.zeros([dataset.shape[1],])
init_weights[0] = -4
print("initial weights：{}".format(init_weights))

p = Perceptron(init_weights, learning_step=1)


#==# 
print('Start training')
while (int(input("\nIf you want to end it, press 0.(Any other input to continue) \n")) != 0):
    for data in dataset:
        if(input("\nWait for any input, to quit press 'q'   ")=='q'):
            break
        xp = data[0:2]
        label = data[2]
        print("Datenpunkt|xp: {}, label: {}".format(xp, label))
        if not p.is_classified(xp, label):
            p.update(xp)
        print(p.w)
```



## 总结|通用

1 2D => 3D：仍用一个Perceptron来实现线性分类

![image-20210519144548753](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210519144549.png)

$x_c$是自定义的中间点：选择到它的距离 => 来区分绿色和红色两类点
$$
(x_1, x_2) \rightarrow (x_1, x_2, ||x_p - x_c||_2)
$$
<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210519144801.png" alt="image-20210519144800979" style="zoom:67%;" />

