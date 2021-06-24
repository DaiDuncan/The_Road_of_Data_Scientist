# 一、MLP实现XOR

## 2D平面

[link: CSDN](https://blog.csdn.net/qq_36810398/article/details/88804585)

![image-20210603193549736](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210603193550.png)

备注：途中的阶梯符号表示阶跃函数heaviside()



![image-20210603193804090](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210603193804.png)

（a）当输入 (x1,x2)=(0,0)时：

隐层左perception输出为heaviside(-1.5+1×0+1×0)=0；
隐层右perception输出为heaviside(-0.5+1×0+1×0)=0；
输出层perception输出为heaviside(-0.5+(-1)×0+1×0)=0。
MLP输出0。

（b）当输入 (x1,x2)=(1,1)时：

隐层左perception输出为heaviside(-1.5+1×1+1×1)=1；
隐层右perception输出为heaviside(-0.5+1×1+1×1)=1；
输出层perception输出为heaviside(-0.5+(-1)×1+1×1)=0。
MLP输出0。

（c）当输入 (x1,x2)=(0,1)时：

隐层左perception输出为heaviside(-1.5+1×0+1×1)=0；
隐层右perception输出为heaviside(-0.5+1×0+1×1)=1；
输出层perception输出为heaviside(-0.5+(-1)×0+1×1)=1。
MLP输出1。

（d）当输入 (x1,x2)=(1,0)时：

隐层左perception输出为heaviside(-1.5+1×1+1×0)=0；
隐层右perception输出为heaviside(-0.5+1×1+1×0)=1；
输出层perception输出为heaviside(-0.5+(-1)×0+1×1)=1。
MLP输出1。



[link: 辅助参考-示意图|MLP中激活函数的作用](https://blog.csdn.net/sinat_28685897/article/details/85241977)

- 单层感知机是非线性模型，但其主要结构是线性的，其非线性是激活函数带来的，但这种**非线性的作用主要是整流，也即为模型分类增加确信度**
  - 在XOR问题中：没带来所需的非线性作用
- 我们所需的非线性作用是模型函数的单调性可变

@多元函数的单调性问题：以二元函数的单调性为例

![image-20210603194100609](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210603194100.png)

单层感知机不能解决非线性可分问题的原因是：

- (1)其主要模型结构是线性的，
- (2)激活函数作用是局限的，导致其表示的多元函数单调性是确定的，不可变。

对于两层感知机的单调性其单调性可变的原因也很简单：由于两层的结构，它将两个单层感知机的输出作为自身输入进行加权求和，其中一个感知机是在$\vec{AB}$方向上是单调增函数，另一个感知机在$\vec{AB}$方向上是单调减函数

根据函数单调性的性质，则得到了先增后减的这种异或问题所需的“非线性”性质。

我们可以给出一个观点：两层感知机能解决非线性可分问题的原因是其两层连接结构，使其整个模型所表示的多元函数单调性是不确定的，是可变的。

![image-20210603194831666](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210603194831.png)





## 3D立体

### 1）sigmoid函数求导 => 延伸|矩阵求导

![image-20210603195025185](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210603195025.png)



### me|思路

1 最初的想法：3个平面(中间拦腰，两个平行平面垂直于立方体的底面)

- 但是要加一层隐藏层来区分中间拦腰平面的上、下两种情况
  - 上面的情况：在两个平行平面外
  - 下面的情况：在两个平行平面内



=>  说明网络深度使得抽象能力大幅提升



2 基于2D的XOR问题：构造4个平面



### 参考答案

1 简单化|w全部设置为1 => 平行平面

直接写结果 => 隐藏层3个节点

me|对比思路：我想的是线性平面进行分割，一个神经元代表一个线性平面

- 答案：平行的平面(输入层到隐藏层的权重相同)“斜着”进行切割，再用后一层的权重来区分
  - 该平面法向量是(1,1,1) => 隐藏层基于输入的和来激活神经元
    - 输入的和有4种情况：分别是0，1，2，3
    - 对应三个神经元的激活情况是：
      - 0：0 0 0 
      - 1：1 0 0 
      - 2：1 1 0
      - 3：1 1 1 
    - 然后用隐藏层到输出层的权重来区分

![image-20210608095545115](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210608095545.png)



2 求导的条理性

- 一步一步来：激活函数单独列出来

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210608100838.png" alt="image-20210608100837793" style="zoom:80%;" />



## 知识点

### 1 激活函数

- 我觉得w是权重，并不是说激活函数的输入是x，激活函数自带w

![image-20210604104312407](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210604104312.png)



### 2 反向传播BP

矩阵求导@Matrix Cookbook





### 3 共享权重

![image-20210604104438677](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210604104439.png)



# 二、HMM-前向算法

## 已知条件

1 输出概率emission

![image-20210604104509312](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210604104509.png)

2 输出序列

$O_{Audio} = [x_1, x_2, x_3, x_4, x_5]$



3 模型$\lambda$

$\lambda_{Satz}$表示只有Satz对应的语音状态序列(序列的拓扑顺序符合Satz)，其余语音状态不存在



4 传递概率Transition

![image-20210604104803446](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210604104803.png)

## 前向算法

1 初始状态与结束状态(中间是状态转移)

@HMM示例代码：3个袋子取出白、红球

![image-20210604195645450](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210604195645.png)

@Forum中的思路

> 原文：Wie verstehe ich "Verwenden Sie ‘START’ als Startzustand s0. Die Übergangswahrscheinlichkeit von s0 zu ‘Z’ soll dabei 1,0 sein." ?

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210604195837.png" alt="image-20210604195836914" style="zoom:67%;" />

@参考答案的图示

![image-20210608104138885](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210608104139.png)



2 中间的状态转移概率

> 原文：“A2b: Was bedeutet 'die anderen Übergangswahrscheinlichkeiten ergeben sich durch [...] ODER werden auf 0.5 festgelegt'?”





3 不同的HMM模型⭐

Die HMMs "Satz" und "Katze" haben unterschiedliche ==Topologien==.

- bei $\lambda_{Satz}$ liegt A zwischen Z und T
- bei $\lambda_{Katze}$ liegt A zwischen K und T





4 符号标记$\alpha_T(s_n)$

- t表示时刻 => T表示最后的时刻
- $s_n$表示最终概率最大的状态
- 下述的第二个式子中：假如$s_j \ne s_n$，那么整个概率值为0

$$
\alpha_t(j) = P(o_1, o_2, ..., o_t, q_t = s_j|\lambda)  \\
\alpha_T(j) = P(O, q_T = s_j|\lambda)
$$





5 特殊情况|概率值一样怎么取舍？

两条路径都是viterbi路径





## 知识点

1 评价指标WER(Wort-Fehler-Rate)

> Hypothese: `Das ist ein Fest` => ASR系统的Hypothese => 识别结果
>
> Referenz: `Das ist ein Test` => Label bzw. "Soll" Ausgabe(目标值)
>
> #ref = 4, #subs = 1, #del = #ins = 0(改subs 增ins 删del)
>
> WER = 1/4





# 三、Sprachmodelle

"Karlsruher Institut fuer Technologie" => 3-gram Sprachmodell

![image-20210604104205331](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210604104205.png)



me|Sprachmodelle一般来自于语料本身(说话的习惯)

![image-20210604104941945](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210604104942.png)





# #参考文献

[KIT|Grundlagen der Automatischen Spracherkennung 2018.10.15](https://cs-k.it/master/lecture/Grundlagen-der-Automatischen-Spracherkennung.html#wer)
