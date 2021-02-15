# 导言|千禧年世纪难题

七个由美国克雷数学研究所于2000年5月24日公布的数学难题

这些难题是呼应1900年德国数学家大卫·希尔伯特在巴黎提出的23个历史性数学难题，经过一百年，许多难题已获得解答。

而千禧年大奖难题的破解，极有可能为密码学以及航天、通讯等领域带来突破性进展。

- P/NP问题（P versus NP）
- 霍奇猜想（The Hodge Conjecture）
- 庞加莱猜想（The Poincaré Conjecture）
- 黎曼猜想（The Riemann Hypothesis）：研究素数分布规律
- 杨-米尔斯存在性与质量间隙（Yang-Mills Existence and Mass Gap）
- 纳维-斯托克斯存在性与光滑性（Navier-Stokes existence and smoothness)
- 贝赫和斯维讷通-戴尔猜想（The Birch and Swinnerton-Dyer Conjecture）



## P与NP问题说明

> 假设你正在为400名大学生组织住宿，但是空间有限只有100名学生能在宿舍里找到位置。更复杂的是还给了你一份不相容学生的名单，并要求在你的最终选择中不要出现这份名单中的任何一对。
>
> 这是计算机科学家称之为NP问题的一个例子，因为很容易检查一个同事提出的一百个学生的给定选择是否令人满意，然而从头开始生成这样一个列表的任务似乎太难了以至于完全不切实际。
>
> 事实上从400名申请者中选择100名学生的方法总数比已知宇宙中的原子数量还要多！这类其答案可以被快速检查，但是通过任何直接的程序需要不可接受长度的时间来解决，比如300年或者更多…
>
> 斯蒂芬·库克和列昂尼德·莱文在1971年独立地提出了P(即容易找到)和NP(即容易检查)问题。



### P问题

all problems solvable, deterministically, in polynomial time
译：多项式时间内可解决的问题(当然在多项式时间是可验证的)



例子：

计算1-1000的连续整数之和：这个问题就比较简单，无论是编程还是使用高斯求和公式都可以在有限可接受的时间内完成，这种算是P类问题





### NP问题

不能在多项式时间内解决或不确定能不能在多项式时间内解决，但能在多项式时间验证的问题



non-deterministic Polynomial time
译：非确定性多项式时间可解决的问题



例子：

计算地球上所有原子个数之和：这个问题就很困难甚至无解，但是现在有个答案是300个，显然是错的，所以很容易验证但不容易求解，这种算NP类问题。





### 多项式时间

多项式时间看作是计算机解决问题的分水岭

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210208093003.png" alt="image-20210208093003031" style="zoom:80%;" />



### 现实中的NP类问题

旅行商问题 TSP 的定义：

> 旅行推销员问题 Travelling salesman problem是这样一个问题：给定一系列城市和每对城市之间的距离，求解==访问每一座城市一次并回到起始城市的最短回路==。
>
> 它是组合优化中的一个NP难问题，在运筹学和理论计算机科学中非常重要。
>
> 最早的旅行商问题的数学规划是由 Dantzig（1959）等人提出，并且是在最优化领域中进行了深入研究。许多优化方法都用它作为一个测试基准。尽管问题在计算上很困难，但已经==有了大量的启发式算法和精确方法来求解数量上万的实例，并且能将误差控制在1%内==。

![image-20210208093204374](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210208093204.png)

我们都知道在城市规模比较小时比如3个/5个 我们可以进行穷举来确定最优的路线，但是经过几次穷举我们发现穷举复杂度是O(n!)



NP问题不仅存在于运筹学，在医学领域的蛋白质折叠问题也属于NP问题，该问题对研究癌症、阿尔兹海默症、帕金森症等都有非常重大的现实意义。



# NP问题的进展

![image-20210208101713153](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210208101714.png)

从特征上看，我们可以知道P类问题属于NP问题，因为P类问题属于NP类问题中可在多项式时间验证并解决的问题，可以简单认为P类问题属于特例最基本简单的NP问题。



虽然目前 P=NP 问题还没有被证明或者证伪，但是经过多年的研究，学术界的一个方向性的共识是 P=NP 问题应该是不成立的，换句话说就是至少存在一个 NP类问题是无法在多项式时间内解决的。



## NPC问题

所有NP问题在多项式时间内都能约化(Reducibility)到它的NP问题，即解决了此NPC问题，所有NP问题也都得到解决



不由得问为什么会有这个不成立的倾向呢？因为很多学者做了很多研究之后，虽然没有解决问题，但是仍然取到了很大的进步提供了研究方向：NPC问题的发现。



NPC问题Non-deterministic Polynomial complete problem又称NP完全问题，NP问题就是大量的NP问题经过归约化而发现的终极bossNP问题。



### 实例|NPC问题

介绍逻辑电路问题。

这是第一个NPC问题。其它的NPC问题都是由这个问题约化而来的。因此，逻辑电路问题是NPC类问题的“鼻祖”。

逻辑电路问题是指的这样一个问题：给定一个逻辑电路，问是否存在一种输入使输出为True。

![image-20210208094843708](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210208094843.png)



逻辑电路问题属于NPC问题。这是有严格证明的。它显然属于NP问题，并且可以直接证明所有的NP问题都可以约化到它（不要以为NP问题有无穷多个将给证明造成不可逾越的困难）。

> 证明过程相当复杂，其大概意思是说任意一个NP问题的输入和输出都可以转换成逻辑电路的输入和输出（想想计算机内部也不过是一些 0和1的运算），因此对于一个NP问题来说，问题转化为了求出满足结果为True的一个输入（即一个可行解）。













### 约化 NP => NPC

《算法导论》上举了这么一个例子。

比如说，现在有两个问题：求解一个一元二次方程和求解一个一元一次方程。

那么我们说，前者可以约化为后者(问题扩大)，意即知道如何解一个一元二次方程那么一定能解出一元一次方程。我们可以写出两个程序分别对应两个问题，那么我们能找到一个“规则”，按照这个规则把解一元二次方程程序的输入数据变一下，用在解一元一次方程的程序上，两个程序总能得到一样的结果。

这个规则即是：两个方程的对应项系数不变，一元二次方程的二次项系数为0。按照这个规则把前一个问题转换成后一个问题，两个问题就等价了。



同样地，我们可以说，Hamilton回路可以约化为TSP问题(Travelling Salesman Problem，旅行商问题)：在Hamilton回路问题中，两点相连即这两点距离为0，两点不直接相连则令其距离为1，于是问题转化为在TSP问题中，是否存在一条长为0的路径。Hamilton回路存在当且仅当TSP问题中存在长为0的回路。



“问题A可约化为问题B”有一个重要的直观意义：B的时间复杂度高于或者等于A的时间复杂度。



[Link: CMU课件|关于归约化的和npc问题的解释](https://www.cs.cmu.edu/~ckingsf/bioinfo-lectures/npcomplete.pdf)

归约化是解决复杂问题的一种思路工具，课件中展示了将问题Y归约化到问题X的过程，其中提到了多项式归约，如果我们找到了问题X的多项式时间解法，那么我们有理由相信问题Y同样可以找到多项式时间解法。

归约化具备传递性：问题A可归约为问题B，问题B可归约为问题C，那么问题A可归约为问题C，正是基于这个特性我们才能把很多小的NP类问题串起来，最终出现NPC问题。

![image-20210208093635397](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210208093635.png)

相比而言问题X比问题Y更难也更普遍，回到NP问题上来说，NP问题的归约化就是去寻找一个终极NP问题，这个问题更普遍更难但是可以cover很多小范围的NP问题，这类终极NP问题就是 NP完全问题。



所以 NPC 问题是研究的重点，其实 TSP 问题就是一个 NPC 问题，这里简单翻译下课件给出 NPC问题的两个基本特征定义：

- NPC问题属于NP问题
- 对于所有NP问题都可以归约化到它



## NP-Hard问题

所有NP问题在多项式时间内都能约化(Reducibility)到它的问题(不一定是NP问题)



NP-Hard问题是满足NPC问题定义的第二条，但不满足第一条，也就是说所有NP问题可以归约化到NPH问题，但是HP-Hard问题不一定是NP问题，所以NPH问题比NPC问题更难。

[NPH问题比NPC问题难理解一些](https://stackoverflow.com/questions/20523578/np-complete-vs-np-hard)





回到最初的问题P=NP是否成立呢？如果成立或者不成立，问题的集合该是怎么样的呢？

维基百科给出了**在P=NP成立和不成立情况下的集合关系**，如图(**敲黑板划重点**)：

![image-20210208093824479](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210208093824.png)









# me|意义

0 思维的启发

从宏观上对问题进行层次定位 => 同义词：抽象、战略



1 对问题进行定位：发现问题穷举是NP问题时，就不要穷尽心思找小技巧，而是看问题能不能转化为P问题



通常NOI和NOIP不会出不属于P类问题的题目。我们常见到的一些信息奥赛的题目都是P问题。道理很简单，一个用穷举换来的非多项式级时间的超时程序不会涵盖任何有价值的算法。



# #备注|常见的NP与P问题

[Link: 21个NPC问题](https://blog.csdn.net/l_mingo/article/details/54232382)

卡普的21个问题列表如下，多数以问题的原名，加上巢状排版表示出这些问题归约的方向。

举例，[背包问题](https://zh.wikipedia.org/wiki/背包問題)（Knapsack）是NP-完全问题的证明，是从[精确覆盖问题](https://zh.wikipedia.org/wiki/精确覆盖问题)归约到背包问题，因此背包问题列为精确覆盖问题的子项。

- 布尔可满足性问题（Satisfiability）：对于布尔逻辑内合取范式方程式的满足性问题（一般直接叫做SAT）
  - [0-1整数规划](https://zh.wikipedia.org/wiki/線性規劃#.E6.95.B4.E6.95.B8.E8.A6.8F.E5.8A.83)（0-1 integer programming）
  - 分团问题（Clique，参考独立集）
    - [Set packing](https://zh.wikipedia.org/wiki/Set_packing)（Set packing）
    - 最小顶点覆盖问题（Vertex cover）⭐
      - [集合覆盖问题](https://zh.wikipedia.org/wiki/集合覆盖问题)（Set covering）
      - [Feedback node set](https://zh.wikipedia.org/w/index.php?title=Feedback_vertex_set&action=edit&redlink=1)（Feedback node set） => 数据库问题⭐
      - [Feedback arc set](https://zh.wikipedia.org/w/index.php?title=Feedback_arc_set&action=edit&redlink=1)
      - 有向哈密顿循环（卡普命名，现称Directed Hamiltonian cycle）⭐
        - [无向哈密顿循环](https://zh.wikipedia.org/wiki/哈密頓路徑問題)（卡普命名，现称Undirected Hamiltonian cycle）⭐
  - 每句话至多3个变量的布尔可满足性问题（Satisfiability with at most 3 literals per clause, 3-SAT）
    - 图着色问题（Chromatic number）
      - [分团覆盖问题](https://zh.wikipedia.org/wiki/分團覆蓋問題)（Clique cover）
      - 精确覆盖问题（Exact cover）
        - [Hitting set](https://zh.wikipedia.org/w/index.php?title=Hitting_set&action=edit&redlink=1)（Hitting set）
        - [Steiner tree](https://zh.wikipedia.org/w/index.php?title=Steiner_tree&action=edit&redlink=1)（Steiner tree）
        - [三维匹配问题](https://zh.wikipedia.org/wiki/三维匹配问题)（3-dimensional matching）
        - 背包问题（Knapsack）⭐
          - [Job sequencing](https://zh.wikipedia.org/w/index.php?title=Job_sequencing&action=edit&redlink=1)（Job sequencing）⭐
          - 划分问题（Partition）
            - [最大割](https://zh.wikipedia.org/w/index.php?title=最大割&action=edit&redlink=1)（Max cut）⭐



## 布尔可满足性问题：

给定一个布尔方程， 判断是否存在一组布尔变量的真值指派==使整个方程为真==的问题@逻辑电路NPC问题



## 0-1整数规划

给定一个整数矩阵C和一个整数向量d，判断是否存在一个0-1向量x，使得Cx=d



## 分团问题

给定无向图G和正整数k，判定G中是否包含有==大小为k==的集团。



## Set packing

给定一个有限集合S和一些S的子集，求问是否可以其中的k个子集，它们两两不相交。



## 最小顶点覆盖问题⭐

给定图 G=(V,E)和数k，判定是否存在包含大小至多为k的顶点覆盖。



## 集合覆盖问题

给定全集U，以及一个包含n个集合且这n个集合的并集为全集的集合S。集合覆盖问题要找到S的一个最小的子集，使得他们的并集等于全集。



## 反馈点集问题

给定有向图H和整数k，判定是否存在集合R⊆V，其中有向图H中的每个有向环都包含R中的一个点。



## 反馈弧集问题

给定有向图H和整数k，判定是否存在集合S⊆E，其中有向图H中的每个有向环都包含E中的一个边。



## 有向哈密尔顿环⭐

给定有向图H，判定其是否经过图中每个顶点且仅一次的回路。



## 无向哈密尔顿环⭐

给定无向图G=(V,E)，判定其是否经过图中每个顶点且仅一次的回路。



## 每句话至多3个变量的布尔可满足性问题

给定三元合取范式f，对布尔表达式f中的布尔变量赋值，是否可使f的真知为真。



## 图着色问题

给定无向图G=(V,E),用k中颜色为V中的每一个定点分配一种颜色，使得不会有两个相邻定点具有同一种颜色。



## 分团覆盖问题

给定无向图G’和整数k，判定N’是k的联合，并且是较少团。



## 精确覆盖问题

给定了一个全集S以及它的m个子集S1、S2、..Sm以后，要求出一组子集，使这组子集的并等于原来的全集S，且各子集两两不交。



## Hitting set 问题

给定一个由 n 个元组组成的有限集合 S ={S1 , …, Sn}, 元组中的元素取自符号集 U ={u1 , …, um}, 判定集合U 是否存在一个子集 U′, U′≤k ,使得对于 S 中的任何一个元组 Si, 满足 Si ∩U′≠φ



## Steiner tree 问题

给定无向图 G=(V E)及子集 R ⊆V 图中每条边权值wr∈R+。找出连接R中所有顶点且边权值之和最小的树。



## 三维匹配问题

存在一个集合M⊆W×X×Y，并且W，X，Y为不想交的集合，|W|=|X|=|Y|=q。判定是否存在一个集合M'⊆M，使得|M'|=q，并且M'在W，X，Y三维上不存在交集。



## 背包问题⭐

给定的整数 Cj,j=1,2,...n和b，判定是否存在{1,2,...,n}的子集B，使得(C1+C2+...+Cj) =b



## Job sequence问题⭐

给定m个性能相同的处理器，n个作业J1,J2,…Jn，每个作业的运行时间t1,t2,…tn以及时间T，是否可以调度这m个处理器，使得他们最多在时间T里完成这n个作业。



## 划分问题

给定一个具有n个整数的集合S，是否能把S划分成两个子集S1和S2，使得S1中的整数之和等于S2中的整数之和



## 最大割问题⭐

给定无向图G，有权函数w：A->Z。判定是否有集合S ⊆N使得

![image-20210208095740824](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210208095740.png)

# #参考文献

[Link: 算法|NP=P问题](https://www.cxyxiaowu.com/9424.html)

本文也是笔者不熟悉但是还比较感兴趣的领域，最近一周花了一些时间去看这个话题，其中在B站的**妈咪说**和**遇见数学**的科普介绍很不错，在观看过程中也是反复看，但是对于其中细致的问题也捉襟见肘，因此文章可能存在问题，如果读者是这方面的大神可以直接私信我提出，我们一起看下。