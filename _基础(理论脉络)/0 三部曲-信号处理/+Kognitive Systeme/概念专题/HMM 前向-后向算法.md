[HMM-前向后向算法理解与实现（python）](https://www.cnblogs.com/gongyanzh/p/12880387.html)
[HMM-维特比算法理解与实现（python）](https://www.cnblogs.com/gongyanzh/p/12878375.html)



## 基本要素

- 状态 N个
- 状态序列 S=s1,s2,...
- 观测序列 O=O1,O2,...
- λ(A,B,π)
  - 状态转移概率 $A=\{a_{ij}\}$
  - 发射概率 $B=\{b_{ik}\}$
  - 初始概率分布 $\pi=\{\pi_{i}\}$
- 观测序列生成过程
  - 初始状态
  - 选择观测
  - 状态转移
  - 返回step2

## HMM三大问题

- 概率计算问题（评估问题）

给定观测序列 O=O1O2...OT，模型 λ(A,B,π)，计算 P(O|λ)，即计算观测序列的概率

- 解码问题

给定观测序列 O=O1O2...OT，模型 λ(A,B,π)，找到对应的状态序列 S

- 学习问题

给定观测序列 O=O1O2...OT，==找到模型参数 λ(A,B,π)，以最大化== P(O|λ)



## 概率计算问题/评估问题

给定模型 λ 和观测序列 O，如何计算P(O|λ)？

### 1 **暴力枚举**

每一个可能的状态序列 S

- 对**每一个给定的状态**序列 $P(O|S, \lambda)$
- 一个状态序列的产生概率 $P(S| \lambda)$
- 联合概率 $P(O, S| \lambda)$
- 考虑所有的状态序列 $P(O| \lambda)$

O 可能由任意一个状态得到，所以需要将每个状态的可能性相加。

这样做什么问题？时间复杂度高达 $O(2T \cdot N^T)$。每个序列需要计算 2T 次，一共 $N^T$ 个序列。



### 2 前向算法

在时刻 t，状态为 i 时，前面的时刻观测到 O1,O2,...,Ot 的概率，记为 αi(t)：

![image-20210603143551366](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210603143551.png)

当 t=1 时，输出为 O1，假设有三个状态，O1 可能是==任意一个状态发出==，即

![image-20210603143606699](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210603143606.png)

![image-20200511202908264](https://gitee.com/gongyanzh/blogpic/raw/master/pictures/image-20200511202908264.png)

当 t=2 时，输出为 O1O2 ，O2 可能由任一个状态发出，同时产生 O2 对应的状态可以由 t=1时刻任意一个状态转移得到。假设 O2 由状态 `1` 发出，如下图

![image-20200511203749699](https://gitee.com/gongyanzh/blogpic/raw/master/pictures/image-20200511203749699.png)

![image-20210603143647206](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210603143647.png)



所以**前向算法**过程如下：

![image-20210603143711980](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210603143712.png)

相比暴力法，时间复杂度降低了吗？

当前时刻有 NN 个状态，每个状态可能由前一时刻 NN 个状态中的任意一个转移得到，所以单个时刻的时间复杂度为 $O(N^2)$,总**时间复杂度**为 $O(T \cdot N^2)$

#### **代码实现**

假设从三个 袋子 `{1,2,3}`中 取出 4 个球 `O={red,white,red,white}`，模型参数λ=(A,B,π)λ=(A,B,π) 如下，计算序列`O`出现的概率

```python
#状态 1 2 3
A = [[0.5,0.2,0.3],
	 [0.3,0.5,0.2],
	 [0.2,0.3,0.5]]

pi = [0.2,0.4,0.4]

# red white
B = [[0.5,0.5],
	 [0.4,0.6],
	 [0.7,0.3]]
```



```python
#前向算法
def hmm_forward(A,B,pi,O):
    T = len(O)
    N = len(A[0])
    #step1 初始化
    alpha = [[0]*T for _ in range(N)]
    for i in range(N):
        alpha[i][0] = pi[i]*B[i][O[0]]

    #step2 计算alpha(t)
    for t in range(1,T):
        for i in range(N):
            temp = 0
            for j in range(N):
                temp += alpha[j][t-1]*A[j][i]
            alpha[i][t] = temp*B[i][O[t]]
            
    #step3
    proba = 0
    for i in range(N):
        proba += alpha[i][-1]
    return proba,alpha

A = [[0.5,0.2,0.3],[0.3,0.5,0.2],[0.2,0.3,0.5]]
B = [[0.5,0.5],[0.4,0.6],[0.7,0.3]]
pi = [0.2,0.4,0.4]
O = [0,1,0,1]
hmm_forward(A,B,pi,O)  #结果为 0.06009
```

结果

![image-20200512195503450](https://gitee.com/gongyanzh/blogpic/raw/master/pictures/image-20200512195503450.png)

### 3 后向算法

在时刻 t，状态为 i 时，观测到 Ot+1,Ot+2,...,OT 的概率，记为 βi(t) ：

![image-20210603143851562](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210603143851.png)

当 t=T时，由于 T 时刻之后为空，没有观测，所以 βi(t)=1

当 t=T−1 时，观测 OT ，OT 可能由任意一个状态产生

![image-20210603143915111](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210603143915.png)



![image-20200511214910979](https://gitee.com/gongyanzh/blogpic/raw/master/pictures/image-20200511214910979.png)



![image-20210603144024380](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210603144024.png)



后向算法过程如下：

![image-20210603144012166](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210603144012.png)

- 时间复杂度$ O(N^2 \cdot T)$



#### **代码实现**

还是上面的例子

```python
#后向算法
def hmm_backward(A,B,pi,O):
    T = len(O)
    N = len(A[0])
    #step1 初始化
    beta = [[0]*T for _ in range(N)]
    for i in range(N):
        beta[i][-1] = 1
        
    #step2 计算beta(t)
    for t in reversed(range(T-1)):
        for i in range(N):
            for j in range(N):
                beta[i][t]  += A[i][j]*B[j][O[t+1]]*beta[j][t+1]
            
    #step3
    proba = 0
    for i in range(N):
        proba += pi[i]*B[i][O[0]]*beta[i][0]
    return proba,beta

A = [[0.5,0.2,0.3],[0.3,0.5,0.2],[0.2,0.3,0.5]]
B = [[0.5,0.5],[0.4,0.6],[0.7,0.3]]
pi = [0.2,0.4,0.4]
O = [0,1,0,1]
hmm_backward(A,B,pi,O)  #结果为 0.06009
```

结果：

![image-20200512195526215](https://gitee.com/gongyanzh/blogpic/raw/master/pictures/image-20200512195526215.png)



### 4 前向-后向算法|时刻t，状态i的概率

![image-20200511201506794](https://gitee.com/gongyanzh/blogpic/raw/master/pictures/image-20200511201506794.png)

回顾前向、后向变量：

- ai(t) 时刻 t，状态为 i ，观测序列为 O1,O2,...,Ot 的概率
- βi(t) 时刻 t，状态为 i，观测序列为 Ot+1,Ot+2,...,OT 的概率

![image-20210603144158071](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210603144158.png)



即在给定的状态序列中，t 时刻状态为 i 的概率。

使用前后向算法可以**计算隐状态**，记 $γ_i(t)=P(s_t=i|O,λ)$ 表示时刻 t **位于隐状态 i** 的概率

![image-20210603144307998](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210603144308.png)



#### Forward-Backward Algorithm@KS

> 方法本质相同：但使用目的不一样
>
> - 上述求的是t时刻，隐状态i的概率
> - 这里求的是t时刻，特定状态转移的概率

给定序列O，在t时刻，状态转移$s_i \rightarrow s_j$的概率

- 分子包含了给定序列O的信息：所以有分母如下(参考上述前-后向算法)

![image-20210603145013223](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210603145013.png)





# #references

[1] https://www.cs.sjsu.edu/~stamp/RUA/HMM.pdf

[2]https://www.cnblogs.com/fulcra/p/11065474.html

[3] https://www.cnblogs.com/sjjsxl/p/6285629.html

[4] https://blog.csdn.net/xueyingxue001/article/details/52396494

