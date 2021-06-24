HMM模型，也叫做隐马尔科夫模型，是一种经典的机器学习序列模型，实现简单，计算快速，广泛用于**语音识别，中文分词等序列标注**领域。



下面通过一个村民看病的故事理解什么是HMM模型。

想象一个乡村诊所，村民的身体状况要么健康，要么发烧，他们只有问诊所的医生才能知道是否发烧。

医生通过**询问村民的感觉**去诊断他们是否发烧。村民自身的感觉有正常、头晕或冷。

假设一个村民每天来到诊所并告诉医生他的感觉。村民的感觉**只由他当天的健康状况**决定。

- 村民的健康状态有两种：健康和发烧，但医生不能直接观察到，这意味着健康状态对医生是不可见的。



每天村民会告诉医生自己有以下几种由他的健康状态决定的感觉的一种：正常、冷或头晕。

于是医生会得到一个村民的感觉的**观测序列**，例如这样：{正常，冷，冷，头晕，冷，头晕，冷，正常，正常}。

但是村民的健康状态这个序列是需要由医生**根据模型来推断的，是不可直接观测的**。



这个村民看病的故事中由**村民的健康状态序列**和**村民的感觉序列**构成的系统就是一个隐马尔科夫模型(HMM)。

其中村民的**健康状态序列构成一个马尔科夫链**。

- 其每个序列值只和前一个值有关，和其它值无关。
- 由于这个马尔科夫链是隐藏的，不可以被直接观测到，只能==由其关联的村民的感觉序列==来进行推断，因此叫做隐马尔科夫模型(HMM)。



# **一，HMM模型的上帝视角**

HMM模型是一个**生成模型**，==描述了两个相关序列的依赖关系==。

这两个相关序列称为状态序列![[公式]](https://www.zhihu.com/equation?tex=X_1%2CX_2%2CX_3%2C%E2%80%A6%E2%80%A6%2CX_T) 和 观测序列$O_1, O_2, ..., O_T$

其中**状态序列**在t时刻的值只和t-1时刻状态序列的取值有关，观测序列在t时刻的值只和t时刻观测序列的取值有关。

![img](https://pic3.zhimg.com/80/v2-7501446b6aa06b9919e41c8da7b1a48a_720w.jpg)

其联合概率函数如下：@Informationsfusion-贝叶斯网络

![[公式]](https://www.zhihu.com/equation?tex=P%28O_1%2CO_2%2CO_3%2C%E2%80%A6%E2%80%A6O_T%2CX_1%2CX_2%2CX_3%2C%E2%80%A6%E2%80%A6%2CX_T%29++%5C%5C)![[公式]](https://www.zhihu.com/equation?tex=%3D+P%28X_1%29+P%28O_1%7CX_1%29+P%28X_2%7CX_1%29+P%28O_2%7CX_2%29+P%28X_3%7CX_2%29+P%28O_3%7CX_3%29%E2%80%A6%E2%80%A6P%28X_T%7CX_%7BT-1%7D%29+P%28O_T%7CX_T%29+%5C%5C)![[公式]](https://www.zhihu.com/equation?tex=%3D+P%28X_1%29+P%28O_1%7CX_1%29++%5Cprod_%5Climits%7Bt%3D2%7D%5E%7BT%7D+%28P%28X_t%7CX_%7Bt-1%7D%29+P%28O_t%7CX_t%29%29+%5C%5C)

如果能够确定**联合概率函数中的各个参数**，那么HMM模型也就完全地确定了，我们就拥有了HMM模型描述的这个体系的上帝视角，可以用来计算任何关心的事件的概率，从而解决我们感兴趣的问题。



# **二，HMM的三大假设**

1，马尔科夫性假设：t时刻的状态出现的概率只和t-1时刻的状态有关。

![[公式]](https://www.zhihu.com/equation?tex=P%28X_t%7CX_%7Bt-1%7DX_%7Bt-2%7D%E2%80%A6%E2%80%A6X_1%29+%3D+P%28X_t%7CX_%7Bt-1%7D%29+%5C%5C)

2，**齐次性**假设：可以理解为==时间平移不变性==

- me|理解：==状态转移矩阵是不变/稳定的==

![[公式]](https://www.zhihu.com/equation?tex=P%28X_t%7CX_%7Bt-1%7D%29+%3D+P%28X_s%7CX_%7Bs-1%7D%29+%5C%5C)![[公式]](https://www.zhihu.com/equation?tex=%28%E5%A6%82%E6%9E%9C+X_t%3D%3DX_s+%E4%B8%94+X_%7Bt-1%7D%3D%3DX_%7Bs-1%7D%29++%5C%5C)

3，观测独立性假设：**某个时刻t的观测值**只依赖于该时刻的状态值，与任何其它时刻的观测值和状态值无关。

![[公式]](https://www.zhihu.com/equation?tex=P%28O_t%7CX_t+X_%7Bt-1%7DX_%7Bt-2%7D%E2%80%A6%E2%80%A6X_1%2CO_%7Bt-1%7DO_%7Bt-2%7D%E2%80%A6%E2%80%A6O_1%29+%3D+P%28O_t%7CX_t%29+%5C%5C)

上述HMM的**联合概率函数**中，实际上就用到了HMM的三大假设。

![[公式]](https://www.zhihu.com/equation?tex=P%28O_1%2CO_2%2CO_3%2C%E2%80%A6%E2%80%A6O_T%2CX_1%2CX_2%2CX_3%2C%E2%80%A6%E2%80%A6%2CX_T%29+%5C%5C)![[公式]](https://www.zhihu.com/equation?tex=%3D+P%28X_1%2CO_1%2CX_2%2CO_2%2CX_3%2CO_3%2C%E2%80%A6%E2%80%A6%2CX_T%2CO_T%29+%5C%5C)![[公式]](https://www.zhihu.com/equation?tex=%3D+P%28X_1%29P%28O_1%7CX_1%29P%28X_2%7CX_1+O_1%29P%28O_2%7CX_2+X_1+O_1%29%E2%80%A6%E2%80%A6P%28X_T%7CX_%7BT-1%7D+O_%7BT-1%7D%E2%80%A6%E2%80%A6X_1O_1%29P%28O_T%7CX_T+O_%7BT-1%7DX_%7BT-1%7D%E2%80%A6%E2%80%A6O_1+X_1%29+%5C%5C)![[公式]](https://www.zhihu.com/equation?tex=%3D+P%28X_1%29+P%28O_1%7CX_1%29+P%28X_2%7CX_1%29+P%28O_2%7CX_2%29+P%28X_3%7CX_2%29+P%28O_3%7CX_3%29%E2%80%A6%E2%80%A6P%28X_T%7CX_%7BT-1%7D%29+P%28O_T%7CX_T%29+%5C%5C)



## ==me钻研|关键公式==：

1）条件概率 $P(A,B) = P(A)\cdot P(B|A) = P(B)\cdot P(A|B)$

2）马尔科夫假设|状态值只跟前一个状态相关：$P(X_2|X_1O_1) = P(X_2|X_1)$

3）观测独立性假设|观测值只依赖于该时刻的状态值：$P(O_2|X_2X_1O_1) = P(O_2|X_2)$



以下列的前向算法递推公式为例：

![image-20210605105305068](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210605105305.png)

- 第一行：生成概率的遍历
- 第二行：条件概率拆分生成概率 => 提取递推项

$$
\alpha_{t+1}(X_{t+1}) = \sum_{x_t} P(O...O_{t+1}, X_t, X_{t+1}) \\
= \sum P(O...O_{t}, X_t) \cdot P(O_{t+1}, X_{t+1}|O...O_{t}, X_t) \\
$$

针对$P(O_{t+1}, X_{t+1}|O...O_{t}, X_t)$，有**两种条件概率拆分的方式**：

1 $P(O_{t+1}|...) \cdot P(X_{t+1}|...,O_{t+1})$

- 第一项就是$P(O_{t+1})$本身：与之前的观测序列和之前的状态无关
- 第二项是$P(X_{t+1}|X_t, O_{t+1})$

2 $P(X_{t+1}|...) \cdot P(O_{t+1}|...,X_{t+1})$

- 第一项就是$P(X_{t+1}|X_t)$ 转移概率
- 第一项就是$P(O_{t+1}|X_{t+1})$ 发生概率







# **三，HMM的三要素$\lambda(\pi, A, B)$**⭐

观察HMM的联合概率函数：

![[公式]](https://www.zhihu.com/equation?tex=P%28O_1%2CO_2%2CO_3%2C%E2%80%A6%E2%80%A6O_T%2CX_1%2CX_2%2CX_3%2C%E2%80%A6%E2%80%A6%2CX_T%29++%5C%5C)![[公式]](https://www.zhihu.com/equation?tex=%3D+P%28X_1%29+P%28O_1%7CX_1%29+%5CPi_%7Bt%3D2%7D%5E%7BT%7D+P%28X_t%7CX_%7Bt-1%7D%29+P%28O_t%7CX_t%29+%5C%5C)

可以看到只依赖于**三种概率值参数**

![[公式]](https://www.zhihu.com/equation?tex=P%28X_1%29), ![[公式]](https://www.zhihu.com/equation?tex=P%28X_t%7CX_%7Bt-1%7D%29), ![[公式]](https://www.zhihu.com/equation?tex=P%28O_t%7CX_t%29)

- 初始状态概率
- 状态值**转移概率**
- 观测值输出概率(发射概率)

这就是HMM的三要素，也就是HMM的全部参数，确定了这三种概率，HMM模型就完全确定下来了。

对于状态值取值和观测值取值为离散值的情况下，这**三种概率可以表示为矩阵**。



假定状态值可能的取值为 ![[公式]](https://www.zhihu.com/equation?tex=x_1%2Cx_2%2C%E2%80%A6%E2%80%A6%2Cx_M)，一共有M种可能取值。 

观测值可能的取值为 ![[公式]](https://www.zhihu.com/equation?tex=o_1%2Co_2%2C%E2%80%A6%E2%80%A6%2Co_N)，一共有N种可能取值。



则HMM的全部参数可以表示为三个矩阵 ![[公式]](https://www.zhihu.com/equation?tex=%5Clambda+%3D+%28%5Cpi%2C+A%2C+B%29)

-  ![[公式]](https://www.zhihu.com/equation?tex=%5Cpi)叫做初始概率矩阵，是一个M维向量， ![[公式]](https://www.zhihu.com/equation?tex=%5Cpi_i+%3D+P%28x_i%29)

- ![[公式]](https://www.zhihu.com/equation?tex=A)叫做转移概率矩阵transition，是一个M×M维矩阵， ![[公式]](https://www.zhihu.com/equation?tex=A_%7Bij%7D+%3D+P%28x_j%7Cx_i%29)

- ![[公式]](https://www.zhihu.com/equation?tex=B)叫做发射概率矩阵emission，是一个M×N维矩阵， ![[公式]](https://www.zhihu.com/equation?tex=B_%7Bij%7D+%3D+P%28o_j%7Cx_i%29)



以上面村民看病的例子为例，假设这三个矩阵分别为：

```python
pi = {'Healthy': 0.6, 'Fever': 0.4} #初始状态矩阵


A = {
   'Healthy' : {'Healthy': 0.7, 'Fever': 0.3},	#状态转移归一律
   'Fever' : {'Healthy': 0.4, 'Fever': 0.6},
   } #状态矩阵

B =  {
   'Healthy' : {'normal': 0.5, 'cold': 0.4, 'dizzy': 0.1},
   'Fever' : {'normal': 0.1, 'cold': 0.3, 'dizzy': 0.6},
} # 发射矩阵
```

![image-20210601105855519](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210601105855.png)

# **四，HMM的三个基本问题**⭐

HMM模型相关的应用问题一般可以归结为这三个基本问题中的一个：

1，Evaluation评估问题：已知模型参数 ![[公式]](https://www.zhihu.com/equation?tex=%5Clambda+%3D+%28A%2CB%2C%5Cpi%29)，和观测序列 ![[公式]](https://www.zhihu.com/equation?tex=O+%3D+O_1%2CO_2%2CO_3%2C%E2%80%A6%E2%80%A6%2CO_T), 计算**观测序列出现的概率**。

以村民看病问题为例, 计算一个村民连续出现 {正常，冷，头晕} 感觉的概率。

- 评估问题一般使用前向算法或者后向算法进行解决，其中前向算法相对简单，容易理解一些。后向算法较难理解。
- 设想有两个描述两人语音的HMM模型，那么给一个新的语音序列，利用前向算法或者后向算法就可以**计算这个语音序列更可能是哪个人的**。



2，Decoding预测问题：也叫做解码问题。已知模型参数 ![[公式]](https://www.zhihu.com/equation?tex=%5Clambda+%3D+%28A%2CB%2C%5Cpi%29)，和观测序列 ![[公式]](https://www.zhihu.com/equation?tex=O+%3D+O_1%2CO_2%2CO_3%2C%E2%80%A6%E2%80%A6%2CO_T), 计算**该观测序列**对应的**最可能的状态序列**。以村民看病问题为例，假设一个病人连续出现 {正常，冷，头晕} 的感觉，计算病人对应的**最可能的健康状态序列**。

- 预测问题一般使用贪心近似算法或者维特比算法解决。
- 其中贪心近似算法相对简单一些，但不一定能找到全局最优解。**维特比算法可以找到全局最优，是一种动态规划算法**。



3，学习问题：模型参数 ![[公式]](https://www.zhihu.com/equation?tex=%5Clambda+%3D+%28A%2CB%2C%5Cpi%29) 未知，推断**模型参数**。

- 有两种可能的场景，一种是监督学习的场景，已知诸多观测序列和对应的状态序列，推断模型参数。监督学习的场景，学习方法相对简单。
- 第二种是非监督学习的场景，只知道诸多观测序列，推断模型参数。非监督学习的场景，一般使用==EM期望最大化方法==进行迭代求解。





# **五，三个基本问题的解决方法**

## **1，评估问题**👌

已知模型参数 ![[公式]](https://www.zhihu.com/equation?tex=%5Clambda+%3D+%28A%2CB%2C%5Cpi%29)，和观测序列 ![[公式]](https://www.zhihu.com/equation?tex=O+%3D+O_1%2CO_2%2CO_3%2C%E2%80%A6%E2%80%A6%2CO_T), 计算观测序列出现的概率。

评估问题一般使用前向算法或者后向算法进行解决，其中前向算法相对简单。

### 1）暴力求解

这个概率可以计算如下:

![[公式]](https://www.zhihu.com/equation?tex=P%28O_1%2CO_2%2CO_3%2C%E2%80%A6%E2%80%A6%2CO_T%29%3D++%5Csum_%7BX_1+%3D+x_1%7D%5E%7Bx_M%7D%5Csum_%7BX_2+%3D+x_1%7D%5E%7Bx_M%7D%5Csum_%7BX_3+%3D+x_1%7D%5E%7Bx_M%7D%E2%80%A6%5Csum_%7BX_T+%3D+x_1%7D%5E%7Bx_M%7D++P%28X_1%29+P%28O_1%7CX_1%29++%5Cprod_%5Climits%7Bt%3D2%7D%5E%7BT%7D+%28P%28X_t%7CX_%7Bt-1%7D%29+P%28O_t%7CX_t%29%29+%5C%5C)

计算复杂度大约为 ![[公式]](https://www.zhihu.com/equation?tex=T%2AM%5ET)

- T表示输出序列的长度
- M表示隐含状态的数量

本质：遍历满足输出序列前提下，所有可能的状态序列@KS课件

![image-20210603090142043](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210603090142.png)



### 2）前向算法

是一种**递推算法**，可以大大减少重复计算，降低计算复杂度。

==构造序列==![[公式]](https://www.zhihu.com/equation?tex=%5Calpha_t%28X_t%29+%3D+P%28O_1%2CO_2%2C%E2%80%A6%E2%80%A6%2CO_t%2CX_t%29)

则初始值如下：(注意：$\pi, A, B$都是概率矩阵)

![[公式]](https://www.zhihu.com/equation?tex=%5Calpha_1%28X_1%29+%3D+P%28O_1%2CX_1%29+%3D+P%28X_1%29P%28O_1%7CX_1%29+%3D+%5Cpi%28X_1%29+B%28X_1%2CO_1%29)

而：重点理解⭐

![[公式]](https://www.zhihu.com/equation?tex=%5Calpha_2%28X_2%29+%3D+P%28O_1%2CO_2%2CX_2%29+%3D+%5Csum_%7BX_1+%3D+x_1%7D%5E%7BX_M%7D+P%28O_1%2CX_1%29+P%28X_2%7CX_1%29+P%28O_2%7CX_2%29+%5C%5C)![[公式]](https://www.zhihu.com/equation?tex=%3D+%5Csum_%7BX_1+%3D+x_1%7D%5E%7BX_M%7D+%5Calpha_1%28X_1%29+A%28X_1%2CX_2%29+B%28X_2%2CO_2%29+%5C%5C)

me|注意符号标记的含义：
$$
A(X_1, X_2) = P(X_2|X_1) \\
B(X_2, O_2) = P(O_2|X_2)
$$




不难发现存在递推公式如下：me|注意$\alpha_t(X_t)$的定义式

![[公式]](https://www.zhihu.com/equation?tex=%5Calpha_%7Bt%2B1%7D+%28X_%7Bt%2B1%7D%29+%3D+P%28O_1%2CO_2%2C%E2%80%A6%E2%80%A6%2CO_%7Bt%2B1%7D%2CX_%7Bt%2B1%7D%29+%3D+%5Csum_%7BX_%7Bt%7D+%3D+x_1%7D%5E%7BX_M%7D+%5Calpha_%7Bt%7D%28X_%7Bt%7D%29+P%28X_%7Bt%2B1%7D%7CX_t%29+P%28O_%7Bt%2B1%7D%7CX_%7Bt%2B1%7D%29+%5C%5C)

![[公式]](https://www.zhihu.com/equation?tex=%3D+%5Csum_%7BX_t+%3D+x_1%7D%5E%7BX_M%7D+%5Calpha_t%28X_t%29+A%28X_t%2CX_%7Bt%2B1%7D%29+B%28X_%7Bt%2B1%7D%2CO_%7Bt%2B1%7D%29+%5C%5C)



通过![[公式]](https://www.zhihu.com/equation?tex=%5Calpha_t%28X_t%29), 我们可以计算

- **求联合概率：遍历状态**$X_T$

![[公式]](https://www.zhihu.com/equation?tex=P%28O_1%2CO_2%2CO_3%2C%E2%80%A6%E2%80%A6%2CO_T%29+%3D+%5Csum_%7BX_T+%3D+x_1%7D%5E%7BX_M%7D+%5Calpha_T%28X_T%29+%5C%5C)

计算复杂度已经降低为 ![[公式]](https://www.zhihu.com/equation?tex=T%2AM%5E2)

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210602172225.png" alt="image-20210602172225195" style="zoom: 80%;" />



### 3）后向算法

已知模型参数 ![[公式]](https://www.zhihu.com/equation?tex=%5Clambda+%3D+%28A%2CB%2C%5Cpi%29)，和观测序列 ![[公式]](https://www.zhihu.com/equation?tex=O+%3D+O_1%2CO_2%2CO_3%2C%E2%80%A6%E2%80%A6%2CO_T), 计算观测序列出现的概率。

除了前向算法，还有一种后向算法，功能和前向算法相当，也是使用**递推法**实现的，但没有前向算法那么直观。

构造序列![[公式]](https://www.zhihu.com/equation?tex=%5Cbeta_t%28X_t%29+%3D+P%28O_%7Bt%2B1%7D%2C%E2%80%A6%E2%80%A6%2CO_%7BT-1%7D%2CO_T%7CX_t%29)

- me|直观：基于状态$X_t$，探寻**之后**所有输出序列的概率 => 在最后一个状态$X_T$，不存在之后的输出序列
- ==我们规定 ![[公式]](https://www.zhihu.com/equation?tex=%5Cbeta_T%28X_T%29+%3D+1) 对任何![[公式]](https://www.zhihu.com/equation?tex=X_T)都成立。== => 此时$\beta_t(X_t) = \beta_T(X_T) = P(O_{T+1}|X_T)=1$



类似地我们可以发现后向递推关系：

me-理解公式：

- 第一行：全概率公式
  - 情境含义：遍历状态$X_{t+1}$
- 第二行：条件概率展开
  - 情境含义：提取状态转移概率B
- 第三行：me|**观测独立性假设**，输出序列与状态$X_t$无关，是多余的信息⭐
- 第四行：观测序列$O_i$彼此独立 => 形成递推的形式
  - 情境含义：提取输出概率A => 同时，形成递推的形式

![[公式]](https://www.zhihu.com/equation?tex=%5Cbeta_t%28X_t%29+%3D++P%28O_%7Bt%2B1%7D%2C%E2%80%A6%E2%80%A6%2CO_%7BT-1%7D%2CO_T%7CX_t%29+%3D+%5Csum_%7BX_%7Bt%2B1%7D+%3D+x_1%7D%5E%7Bx_M%7D+P%28X_%7Bt%2B1%7D%2C+O_%7Bt%2B1%7D%2C%E2%80%A6%E2%80%A6%2CO_%7BT-1%7D%2CO_T%7CX_t%29+%5C%5C)![[公式]](https://www.zhihu.com/equation?tex=%3D+%5Csum_%7BX_%7Bt%2B1%7D+%3D+x_1%7D%5E%7Bx_M%7D+P%28+O_%7Bt%2B1%7D%2C%E2%80%A6%E2%80%A6%2CO_%7BT-1%7D%2CO_T%7CX_%7Bt%2B1%7D%2CX_t%29P%28X_%7Bt%2B1%7D%7CX_t%29+%5C%5C)![[公式]](https://www.zhihu.com/equation?tex=%3D+%5Csum_%7BX_%7Bt%2B1%7D+%3D+x_1%7D%5E%7Bx_M%7D+P%28+O_%7Bt%2B1%7D%2C%E2%80%A6%E2%80%A6%2CO_%7BT-1%7D%2CO_T%7CX_%7Bt%2B1%7D%29P%28X_%7Bt%2B1%7D%7CX_t%29+%5C%5C)![[公式]](https://www.zhihu.com/equation?tex=%3D+%5Csum_%7BX_%7Bt%2B1%7D+%3D+x_1%7D%5E%7Bx_M%7D+P%28+O_%7Bt%2B2%7D%2C%E2%80%A6%E2%80%A6%2CO_%7BT-1%7D%2CO_T%7CX_%7Bt%2B1%7D%29+P%28+O_%7Bt%2B1%7D%7CX_%7Bt%2B1%7D%29+P%28X_%7Bt%2B1%7D%7CX_t%29+%5C%5C)![[公式]](https://www.zhihu.com/equation?tex=%3D+%5Csum_%7BX_%7Bt%2B1%7D+%3D+x_1%7D%5E%7Bx_M%7D+%5Cbeta_%7Bt%2B1%7D%28X_%7Bt%2B1%7D%29+B%28X_%7Bt%2B1%7D%2CO_%7Bt%2B1%7D%29+A%28X_t%2CX_%7Bt%2B1%7D%29+%5C%5C)



通过$\beta_1(X_1) = P(O_{2}, ..., O_T|X_1)$, 我们可以计算：

- 遍历状态$X_1$

![[公式]](https://www.zhihu.com/equation?tex=P%28O_1%2CO_2%2CO_3%2C%E2%80%A6%E2%80%A6%2CO_T%29+%3D+%5Csum_%7BX_1+%3D+x_1%7D%5E%7BX_M%7D+P%28X_1%29P%28O_1%7CX_1%29+%5Cbeta_1%28X_1%29+%5C%5C)![[公式]](https://www.zhihu.com/equation?tex=%3D+%5Csum_%7BX_1+%3D+x_1%7D%5E%7BX_M%7D+%5Cpi%28X_1%29B%28X_1%2CO_1%29+%5Cbeta_1%28X_1%29+%5C%5C)

![image-20210602172320847](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210602172321.png)



### me|梳理重点💡

> 学习、理解的四个阶段
> 1. 感性认识 => 有初步印象：大概明白是什么(原理)，有什么用(实用主义)
> 2. 抽象理解 => 在脑海中构建清晰、直观的流程
> 3. 结合抽象理解 => 洞悉数学公式的精准表达
> 4. 最后|彻底消化整合：返回直观理解(有扎实的基础理论/数学公式做支撑：可溯源)



以HMM评估问题为例：

1 感性认识：基于HMM模型$\lambda(\pi, A, B)$和已有的输出序列，计算隐含状态序列(多种可能性)的概率

2 抽象理解：前向、后向的计算流程图

3 数学公式：容易理解为什么这样变换 => 提取转移概率、输出概率

- 本质上：正常人的思考模式**从直观的抽象理解出发**(也就是计算流程图)，再去探究理论背后的支撑依据 => 最后反过来优化抽象理解

4 整合消化 => 梳理重点



0 现在，脑海中有了KIT的那张流程图：

- 状态变换时序图
- 含有输出序列@Uebung3 解题过程

1 暴力求解：时序图中每一步都进行状态遍历

2 前向算法left to right：逐渐向后传递，最后遍历状态$X_T$

3 后向算法right to left：逐渐向前传递，最后遍历状态$X_1$

4 前向算法 VS. 后向算法

- 没有本质区别：视具体情况而定，比如起始状态很简单，可以考虑前向算法。同理，限定最后状态，可以用后向算法



跳出细节 => 整体鸟瞰 & 回溯理论根基

- HMM的三大假设：观测序列概率独立用在了后向算法中
- 三要素中频繁使用转移概率A和输出概率B





## **2，预测问题**

### 1）贪心算法

已知模型参数 ![[公式]](https://www.zhihu.com/equation?tex=%5Clambda+%3D+%28A%2CB%2C%5Cpi%29)，和观测序列 ![[公式]](https://www.zhihu.com/equation?tex=O+%3D+O_1%2CO_2%2CO_3%2C%E2%80%A6%E2%80%A6%2CO_T), 计算该观测序列对应的**==最可能==的状态序列**@me|评估问题中选择一条最可能的路径



预测问题一般使用贪心近似算法或者维特比算法解决。

较常用的是维特比算法。但贪心近似算法更加简单，很多时候就已经足够使用。



其基本思想是贪心思想，==每一个步骤都取相应的![[公式]](https://www.zhihu.com/equation?tex=X_i), 使得对应输出的概率最大==。

- me|脑海中的状态时序流程图

![[公式]](https://www.zhihu.com/equation?tex=X_1+%3D+%5Cmathop%7Bargmax%7D_%7BX_i%7D+P%28X_i%29P%28O_1%7CX_i%29)

![[公式]](https://www.zhihu.com/equation?tex=X_2+%3D+%5Cmathop%7Bargmax%7D_%7BX_i%7D+P%28X_i%7CX_1%29P%28O_2%7CX_i%29)

……

![[公式]](https://www.zhihu.com/equation?tex=X_T+%3D+%5Cmathop%7Bargmax%7D_%7BX_i%7D+P%28X_i%7CX_%7BT-1%7D%29P%28O_T%7CX_i%29)

### 2）Viterbi

已知模型参数 ![[公式]](https://www.zhihu.com/equation?tex=%5Clambda+%3D+%28A%2CB%2C%5Cpi%29)，和观测序列 ![[公式]](https://www.zhihu.com/equation?tex=O+%3D+O_1%2CO_2%2CO_3%2C%E2%80%A6%E2%80%A6%2CO_T), 计算该观测序列对应的**最可能的状态序列**。

解决这一预测问题较常用的方法是维特比算法，是一种**动态规划**算法，也可以理解成一种**搜索空间的剪枝方法，可以保证找到全局最优路径**。

不同于贪心近似算法在每个步骤只保留一条当前最优路径，维特比算法**在每个步骤会保留若干条当前最优路径**，这些最优路径和每个步骤的**最后一个隐含状态值的可能取值相对应**@ODS-从后往前遍历，如果状态值有**M个可能取值，则每个步骤保留M条当前最优路径**。



由于HMM的马尔科夫性质，之后的概率计算**只和最后一个隐藏状态取值相关**，因此全局的最优路径必定在这M条当前最优路径中，**如此递推**不断向前寻找M个隐状态值对应的M条当前最优路径，最后取最终得到的M条当前最优路径中**概率最大的那条**作为全局最优路径。

![image-20210603102019174](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210603102019.png)

@HMM Viterbi算法



## **3， 学习问题**

模型参数 ![[公式]](https://www.zhihu.com/equation?tex=%5Clambda+%3D+%28A%2CB%2C%5Cpi%29) 未知，推断模型参数，使得$P(O|\lambda)$最大化

- 不存在全局最优



### 1）直接法|概率统计

监督学习的场景，学习方法相对简单。

已知诸多观测序列和对应的状态序列，推断模型参数。

这种情况下可以**统计相应的频率**作为![[公式]](https://www.zhihu.com/equation?tex=%5Cpi), ![[公式]](https://www.zhihu.com/equation?tex=A), ![[公式]](https://www.zhihu.com/equation?tex=B)中各个概率的估计值。







### 2）EM期望最大化

模型参数 ![[公式]](https://www.zhihu.com/equation?tex=%5Clambda+%3D+%28A%2CB%2C%5Cpi%29) 未知，推断模型参数。

这是一个**存在隐变量的概率模型的参数估计问题**，==一般使用EM期望最大化算法==进行求解。@Baum-Welch(Forward-Backward算法)

原始问题可以定义为：

![[公式]](https://www.zhihu.com/equation?tex=%5Cmathop%7Bargmax%7D_%7B%5Clambda%7D+%5Cmathop%7B%5Cln%7D+P%28O_1%2CO_2%2CO_3%2C%E2%80%A6%E2%80%A6%2CO_T%3B%5Clambda%29+%5C%5C)

根据期望最大化算法的算法原理，可以得到**迭代条件**如下：

![[公式]](https://www.zhihu.com/equation?tex=%5Cpmb%7B%5Clambda%7D%5E%7B%5C%7Bn%2B1%5C%7D%7D+%3D+%5Cmathop%7Bargmax%7D_%7B%5Cpmb%7B%5Clambda%7D%7D%5Csum_%7BX_1%7D%E2%80%A6%E2%80%A6%5Csum_%7BX_T%7DP%28X_1%2CX_2%2C%E2%80%A6%E2%80%A6%2CX_T%5C%2C%7C%5C%2CO_1%2CO_2%2C%E2%80%A6%E2%80%A6%2CO_T%3B%5Cpmb%7B%5Clambda%7D%5E%7B%5C%7Bn%5C%7D%7D%29+%5Cln+P%28X_1%2CX_2%2C%E2%80%A6%E2%80%A6%2CX_T%2CO_1%2CO_2%2C%E2%80%A6%E2%80%A6%2CO_T%3B%5Cpmb%7B%5Clambda%7D%29+%5C%5C)![[公式]](https://www.zhihu.com/equation?tex=%3D+%5Cmathop%7Bargmax%7D_%7B%5Cpmb%7B%5Clambda%7D%7D%5Csum_%7BX_1%7D%E2%80%A6%E2%80%A6%5Csum_%7BX_T%7DP%28X_1%2CX_2%2C%E2%80%A6%E2%80%A6%2CX_T%5C%2C%7C%5C%2CO_1%2CO_2%2C%E2%80%A6%E2%80%A6%2CO_T%3B%5Cpmb%7B%5Clambda%7D%5E%7B%5C%7Bn%5C%7D%7D%29+%5Cln+%28P%28X_1%29+%5Cprod_%5Climits%7Bt%3D2%7D%5E%7BT%7D+P%28X_t%7CX_%7Bt-1%7D%29++%5Cprod_%5Climits%7Bt%3D1%7D%5E%7BT%7D++P%28O_t%7CX_t%29%29++%5C%5C)![[公式]](https://www.zhihu.com/equation?tex=%3D+%5Cmathop%7Bargmax%7D_%7B%5Cpmb%7B%5Clambda%7D%7D%5Csum_%7BX_1%7D%E2%80%A6%E2%80%A6%5Csum_%7BX_T%7DP%28X_1%2CX_2%2C%E2%80%A6%E2%80%A6%2CX_T%5C%2C%7C%5C%2CO_1%2CO_2%2C%E2%80%A6%E2%80%A6%2CO_T%3B%5Cpmb%7B%5Clambda%7D%5E%7B%5C%7Bn%5C%7D%7D%29+%5Cln+%28%5Cpi%28X_1%29+%5Cprod_%5Climits%7Bt%3D2%7D%5E%7BT%7D+A%28X_%7Bt-1%7D%2CX_t%29++%5Cprod_%5Climits%7Bt%3D1%7D%5E%7BT%7D++B%28X_t%2CO_t%29%29++%5C%5C)![[公式]](https://www.zhihu.com/equation?tex=%3D+%5Cmathop%7Bargmax%7D_%7B%5Cpmb%7B%5Clambda%7D%7D%5Csum_%7BX_1%7D%E2%80%A6%E2%80%A6%5Csum_%7BX_T%7DP%28X_1%2CX_2%2C%E2%80%A6%E2%80%A6%2CX_T%5C%2C%7C%5C%2CO_1%2CO_2%2C%E2%80%A6%E2%80%A6%2CO_T%3B%5Cpmb%7B%5Clambda%7D%5E%7B%5C%7Bn%5C%7D%7D%29+%5Cln+%5Cpi%28X_1%29+%5C%5C)![[公式]](https://www.zhihu.com/equation?tex=%2B+%5Cmathop%7Bargmax%7D_%7B%5Cpmb%7B%5Clambda%7D%7D%5Csum_%7BX_1%7D%E2%80%A6%E2%80%A6%5Csum_%7BX_T%7DP%28X_1%2CX_2%2C%E2%80%A6%E2%80%A6%2CX_T%5C%2C%7C%5C%2CO_1%2CO_2%2C%E2%80%A6%E2%80%A6%2CO_T%3B%5Cpmb%7B%5Clambda%7D%5E%7B%5C%7Bn%5C%7D%7D%29++%5Csum_%7Bt%3D2%7D%5E%7BT%7D+%5Cln+A%28X_%7Bt-1%7D%2CX_t%29++%5C%5C)![[公式]](https://www.zhihu.com/equation?tex=%2B+%5Cmathop%7Bargmax%7D_%7B%5Cpmb%7B%5Clambda%7D%7D%5Csum_%7BX_1%7D%E2%80%A6%E2%80%A6%5Csum_%7BX_T%7DP%28X_1%2CX_2%2C%E2%80%A6%E2%80%A6%2CX_T%5C%2C%7C%5C%2CO_1%2CO_2%2C%E2%80%A6%E2%80%A6%2CO_T%3B%5Cpmb%7B%5Clambda%7D%5E%7B%5C%7Bn%5C%7D%7D%29++%5Csum_%7Bt%3D1%7D%5E%7BT%7D++%5Cln+B%28X_t%2CO_t%29%29++%5C%5C)



于是可以得到三个参数 ![[公式]](https://www.zhihu.com/equation?tex=%5Cpi%2C+A%2CB) 的 迭代条件：

![[公式]](https://www.zhihu.com/equation?tex=%5Cpi%5E%7B%5C%7Bn%2B1%5C%7D%7D+%3D+%5Cmathop%7Bargmax%7D_%7B%5Cpi%7D%5Csum_%7BX_1%7D%E2%80%A6%E2%80%A6%5Csum_%7BX_T%7DP%28X_1%2CX_2%2C%E2%80%A6%E2%80%A6%2CX_T%5C%2C%7C%5C%2CO_1%2CO_2%2C%E2%80%A6%E2%80%A6%2CO_T%3B%5Cpmb%7B%5Clambda%7D%5E%7B%5C%7Bn%5C%7D%7D%29+%5Cln+%5Cpi%28X_1%29+%5C%5C)![[公式]](https://www.zhihu.com/equation?tex=A%5E%7B%5C%7Bn%2B1%5C%7D%7D+%3D+%5Cmathop%7Bargmax%7D_%7B%5Cpi%7D%5Csum_%7BX_1%7D%E2%80%A6%E2%80%A6%5Csum_%7BX_T%7DP%28X_1%2CX_2%2C%E2%80%A6%E2%80%A6%2CX_T%5C%2C%7C%5C%2CO_1%2CO_2%2C%E2%80%A6%E2%80%A6%2CO_T%3B%5Cpmb%7B%5Clambda%7D%5E%7B%5C%7Bn%5C%7D%7D%29++%5Csum_%7Bt%3D2%7D%5E%7BT%7D+%5Cln+A%28X_%7Bt-1%7D%2CX_t%29++%5C%5C)![[公式]](https://www.zhihu.com/equation?tex=B%5E%7B%5C%7Bn%2B1%5C%7D%7D+%3D+%5Cmathop%7Bargmax%7D_%7B%5Cpi%7D%5Csum_%7BX_1%7D%E2%80%A6%E2%80%A6%5Csum_%7BX_T%7DP%28X_1%2CX_2%2C%E2%80%A6%E2%80%A6%2CX_T%5C%2C%7C%5C%2CO_1%2CO_2%2C%E2%80%A6%E2%80%A6%2CO_T%3B%5Cpmb%7B%5Clambda%7D%5E%7B%5C%7Bn%5C%7D%7D%29++%5Csum_%7Bt%3D2%7D%5E%7BT%7D+%5Cln+B%28X_%7Bt-1%7D%2CX_t%29++%5C%5C)

其中 ![[公式]](https://www.zhihu.com/equation?tex=P%28X_1%2CX_2%2C%E2%80%A6%E2%80%A6%2CX_T%5C%2C%7C%5C%2CO_1%2CO_2%2C%E2%80%A6%E2%80%A6%2CO_T%3B%5Cpmb%7B%5Clambda%7D%5E%7B%5C%7Bn%5C%7D%7D%29) **不含待优化参数，求导为0**，考虑==概率之和为1的约束==，可以**构造拉格朗日乘子法**进行求解，过程较为繁琐，从略。





### 3.1）Forward-Backward algorithm @KS

也叫做Baum-Welch reestimation  

1 状态转移概率：基于$\alpha, \beta$

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210603102627.png" alt="image-20210603102627394" style="zoom:67%;" />

2 Baum-Welch Reestimation  

![image-20210603102705739](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210603102706.png)

⭐小结|算法流程

1. 初始化 $\lambda = (A, B)$
2. 计算$\alpha, \beta, \xi$
3. 基于$\xi$，估计$\bar{\lambda} = (\bar{A}, \bar{B})$
4. $\bar{\lambda} \rightarrow \lambda$
5. 如果不收敛：返回步骤2，迭代计算
   - 终止条件：除非$\bar{\lambda} = \lambda$，否则只要$P(O|\bar{\lambda}) > P(O|\lambda)$，那么就终止迭代 => 说明找到了参数，使得概率更高



💡核心：

- $\xi$的计算公式

- $\bar{A}, \bar{B}$的重新估计公式



### 3.2）Viterbi训练

1. 基于现有模型，计算Viterbi-path(也就是基于初始化的参数)
2. 由Viterbi算法得到labels => 类似监督学习，重新估计参数@上述直接法|统计概率@Uebung3 编程练习



# #参考文献

[一站式解决：隐马尔可夫模型（HMM）全过程推导及实现](https://zhuanlan.zhihu.com/p/85454896)

[ML HMM模型原理机器实战](https://www.cnblogs.com/zhangxinying/p/12071061.html)

[概率图模型体系：HMM、MEMM、CRF](https://zhuanlan.zhihu.com/p/33397147)

![概率图模型体系：HMM、MEMM、CRF](https://pic4.zhimg.com/v2-48dd591b8bc4775b95dd032983c5e729_1440w.jpg?source=172ae18b)



[link: ausführliche Tutorial von Lawrence Rabiner  1989](https://www.cs.ubc.ca/~murphyk/Bayes/rabiner.pdf)

[link:  Diese Einführung von Dan Jurafsky ](https://web.stanford.edu/~jurafsky/slp3/A.pdf)

- 详细介绍了三个算法：前向，Viterbi，前向-后向算法(但不够深入)