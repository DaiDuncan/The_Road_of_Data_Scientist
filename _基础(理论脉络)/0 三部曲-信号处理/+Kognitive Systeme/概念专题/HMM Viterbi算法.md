[link: 原文链接 2020.05.13](https://www.cnblogs.com/gongyanzh/p/12878375.html)

[HMM-前向后向算法理解与实现（python）](https://www.cnblogs.com/gongyanzh/p/12880387.html)
[HMM-维特比算法理解与实现（python）](https://www.cnblogs.com/gongyanzh/p/12878375.html)



## 解码问题Decode

- 给定观测序列 $O=O_1,O_2,...,O_T$，模型 λ(A,B,π)，找到最可能的状态序列 $I^∗={i^∗_1,i^∗_2,...i^∗_T}$



### 1 近似算法/贪心算法

- 无法保证得到的解是全局最优解

在每个时刻 t 选择最可能的状态，得到对应的状态序列

根据[HMM-前向后向算法](https://www.cnblogs.com/gongyanzh/p/12880387.html)计算时刻 t 处于状态 $i^∗_t $的概率：

![image-20210603105749453](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210603105749.png)





### 2 维特比算法|理解

维特比算法的基础/动态规划，可以概括为下面三点（来源于吴军：数学之美）：

1. 如果概率最大的路径经过篱笆网络的某点，则从起始点到该点的**子路径也一定是从开始到该点路径中概率最大的**。

2. 假定第 `t` 时刻有 `k` 个状态，从开始到 `t` 时刻的 `k` 个状态有==`k` 条最短路径==，而最终的最短路径必然经过其中的一条。

3. 根据上述性质，在计算第 `t+1` 时刻的最短路径时，只需要考虑从开始到当前的`k`个状态值的最短路径和当前状态值到第 `t+1` 时刻的最短路径即可。

   如求`t=3`时的最短路径，等于求`t=2`时,从起点到当前时刻的所有状态结点的最短路径加上`t=2`到`t=3`的各节点的最短路径。

![image-20200512214719644](https://gitee.com/gongyanzh/blogpic/raw/master/pictures/image-20200512214719644.png)



#### 通俗理解

对上面三点加深理解

假如你从S和E之间找一条最短的路径，最简单的方法就是列出所有可能的路径 ($O(T^N)$)，选出最小的，显然时间复杂度太高。怎么办？（摘自[3]）

使用维特比算法：

![image-20200512223610958](https://gitee.com/gongyanzh/blogpic/raw/master/pictures/image-20200512223610958.png)

S到A列的路径有三种可能：`S-A1,S-A2,S-A3`，如下图

![image-20200513202915071](https://gitee.com/gongyanzh/blogpic/raw/master/pictures/image-20200513202915071.png)

`S-A1,S-A2,S-A3` 中必定有一个属于全局最短路径。



继续往右，到了B列。对B1：

![image-20200513202742395](https://gitee.com/gongyanzh/blogpic/raw/master/pictures/image-20200513202742395.png)

会产生3条路径：

- S-A1-B1
- S-A2-B1
- S-A3-B1

假设`S-A3-B1`是最短的一条，删掉其他两条。得到

![image-20200513203551041](https://gitee.com/gongyanzh/blogpic/raw/master/pictures/image-20200513203551041.png)

对B2:

![image-20200513203743119](https://gitee.com/gongyanzh/blogpic/raw/master/pictures/image-20200513203743119.png)

会产生3条路径：

- S-A1-B2
- S-A2-B2
- S-A3-B2

假设`S-A1-B2`是最短的一条，删掉其他两条。得到

![image-20200513203847969](https://gitee.com/gongyanzh/blogpic/raw/master/pictures/image-20200513203847969.png)

对B3，得到：

![image-20200513204233084](https://gitee.com/gongyanzh/blogpic/raw/master/pictures/image-20200513204233084.png)



现在我们看看对B列的每个节点有哪些，回顾维特比算法第二点

> 假定第 `t` 时刻有 `k` 个状态，从开始到 `t` 时刻的 `k` 个状态有 `k` 条最短路径，而最终的最短路径必然经过其中的一条

B列有三个节点，所以会有三条最短路径，==最终的最短路径==一定会经过其中一条。

如下图：

![image-20200513204552391](https://gitee.com/gongyanzh/blogpic/raw/master/pictures/image-20200513204552391.png)

同理，对C列，会得到三条最短路径，如下图

![image-20200513205546888](https://gitee.com/gongyanzh/blogpic/raw/master/pictures/image-20200513205546888.png)

到目前为止，仍然无法确定哪条属于全局最短。

最后，我们继续看E节点

![image-20200513205723395](https://gitee.com/gongyanzh/blogpic/raw/master/pictures/image-20200513205723395.png)

最终发现最短路径为`S-A1-B2-C3-E`





#### **数学描述**

在上述过程中，对每一列（每个时刻）会得到对应状态数的最短路径。

在数学上如何表达？记录路径的**最大概率值** $\delta_t(i)$ 和对应路径**经过的节点** $ψ_t(i)$

==定义==在时刻 t 状态为 i 的所有单条路径中概率最大值为

![image-20210603110446901](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210603110447.png)

递推公式：me|==需要遍历==状态i 所有的转移可能性

![image-20210603110502058](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210603110502.png)

定义在时刻t，状态为 i 的所有单条路径中，概率最大路径的第 t−1 个节点为

![image-20210603110724938](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210603110725.png)





### 3 维特比算法|具体步骤

step1：初始化

- 初始化(小细节：此处时刻1作为开始)包含所有状态

![image-20210603110836579](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210603110836.png)



💡step2：递推，对 t=2,3,...,T

- 由前一个时刻(t-1)所有的状态j到当前时刻的状态i
- $\delta_t(i)$是概率值：注意分两部分，一个是转移状态的遍历，另一个是转移状态的输出概率(中间用括号隔开)
  - 表明在当前状态i所能得到的最优概率(由前一个时刻(t-1)的某个状态j转移得到)
- $\psi_t(i)$是记录路径(下标arg)
  - 记录前一个时刻(t-1)的“最优转移”状态j
  - 对于状态i来说，输出概率都一致：所以可省略

![image-20210603110929649](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210603110929.png)



step3：最终的时刻

计算时刻 T 最大的 $δ_T(i)$ ,即为==最可能隐藏状态序列==出现的概率。

计算时刻T最大的 $ψ_T(i)$,即为时刻T==最可能的隐藏状态==。

![image-20210603111021682](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210603111021.png)



step4：最优路径回溯，对t=T−1,...,1

- 注意：$\psi_t(i)$记录的是，**前一个时刻(t-1)**的“最优转移”状态j⭐

![image-20210603111608940](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210603111609.png)



## **代码实现**

假设从三个 袋子 `{1,2,3}`中 取出 4 个球 `O={red,white,red,white}`，模型参数λ=(A,B,π)如下，计算状态序列，即取出的球来自哪个袋子

```python
def hmm_viterbi(A,B,pi,O):
    '''
    A: transition
    B: emission
    O: outpue sequens
    '''
    T = len(O)
    N = len(A[0])
    
    delta = [[0]*N for _ in range(T)]	#T个时刻的概率值 维度：[时刻，状态]
    psi = [[0]*N for _ in range(T)]		#T个时刻的路径记录 维度：[时刻，状态]
    
    #step1: init
    for i in range(N):	#遍历不同状态
        delta[0][i] = pi[i] * B[i][O[0]]	#B是二维矩阵[状态，输出序列的索引] => O[0]是0时刻的输出的索引
        psi[0][i] = 0
        
    #step2: iter
    for t in range(1,T):	#遍历所有时刻
        for i in range(N):	#遍历所有状态
            temp,maxindex = 0,0
            for j in range(N):	#遍历所有的转移状态
                res = delta[t-1][j] * A[j][i]	#临时值，后续提取最大值
                if res > temp:
                    temp = res
                    maxindex = j

            delta[t][i] = temp * B[i][O[t]]	#记录t时刻的最优概率值
            psi[t][i] = maxindex	#记录t-1时刻的“最优转移”状态

    #step3: end
    p = max(delta[-1])	#最后时刻的状态的最优概率值
    for i in range(N):	#最后时刻的最优概率值对应的状态
        if delta[-1][i] == p:
            i_T = i	

    #step4：backtrack
    path = [0] * T
    i_t = i_T
    for t in reversed(range(T-1)):	#回溯
        i_t = psi[t+1][i_t]
        path[t] = i_t
    path[-1] = i_T	#补全最后时刻的最优路径
    
    return delta,psi,path

#状态:3个袋子 1 2 3
A = [[0.5,0.2,0.3],
	 [0.3,0.5,0.2],
	 [0.2,0.3,0.5]]

pi = [0.2,0.4,0.4]

# red white的概率
B = [[0.5,0.5],	#第一个袋子取出红球、白球的概率
	 [0.4,0.6],
	 [0.7,0.3]]

#观测序列：4个球
O = [0,1,0,1]
hmm_viterbi(A,B,pi,O)
```

结果：辨别状态的序列 => 也就是取球袋子的顺序

- $\delta: (4,3)$ => 4个时序，3中隐含状态 => 输出序列的概率值

  - 行的角度：每一个时序，概率最大则对应最有可能的袋子。比如第二行，最有可能来自第二个袋子(index是1) => 貌似第三行数据有问题，最有可能是第三个袋子(index是2)

- $\psi: (4, 3)$ => 

  - 第一行：所有状态的前一个最优状态是0(初始化)
  - 第二行：t=1时刻最有可能的状态是2
  - 第三行：t=2时刻，对三个状态而言，前一个最有可能的状态分别是1，1，2
  - 第四行：同理

- path => 最优路径：最可能的袋子的顺序

  - > me|结合矩阵B和输出序列O的直观理解
    >
    > - 第一个球是红球，很可能来自红球概率高的第三个袋子(index=2)
    > - 第二个球是白球，很可能来自红球概率高的第二个袋子(index=1)
    > - 之后的概率考虑到转移概率和初始随机化概率，无法直接预测

![image-20200513231008945](https://gitee.com/gongyanzh/blogpic/raw/master/pictures/image-20200513231008945.png)

结果的图示@脑海中的时序流程图

![image-20210603123856641](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210603123856.png)

以上基于$\psi$得到所有的最优路径：最终的最优路径 => 最后时刻的最优状态是s=1(参考代码实现细节) => **回溯**得到path [2,1,1,1]

- 注意到红色边框：虽然在t=3时刻，s=2概率最高，但是==由于最后t=4时刻s=1的概率最高==，回溯过程的路径也就是[2,1,1,1]

- 也能发现：概率值随着t时刻步数的增加，越来越小，最后趋近于0





# #references

[1]https://www.cnblogs.com/kaituorensheng/archive/2012/12/04/2802140.html

[2] https://blog.csdn.net/hudashi/java/article/details/87875259

[3] https://www.zhihu.com/question/20136144