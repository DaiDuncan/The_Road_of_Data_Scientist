[长短期记忆网络 – Long short-term memory | LSTM](https://easyai.tech/ai-definition/lstm/)

- [什么是 LSTM？](https://easyai.tech/ai-definition/lstm/#waht)
- [LSTM的核心思路](https://easyai.tech/ai-definition/lstm/#key)
- [百度百科+维基百科](https://easyai.tech/ai-definition/lstm/#baidu)
- [扩展阅读](https://easyai.tech/ai-definition/lstm/#links)

![一文看懂长短期记忆网络 - LSTM](https://easy-ai.oss-cn-shanghai.aliyuncs.com/2019-07-05-banner.png)



## 什么是 LSTM？

长短期记忆网络：通常被称为 LSTM，是一种特殊的 [RNN](https://easyai.tech/ai-definition/rnn/)，能够学习长期依赖性。

由 Hochreiter 和 Schmidhuber（1997）提出的，并且在接下来的工作中被许多人改进和推广。

LSTM 在各种各样的问题上表现非常出色，现在被广泛使用。

LSTM 被明确设计用来**避免长期依赖性问题**。

长时间记住信息实际上是 LSTM 的默认行为，而不是需要努力学习的东西！



所有递归神经网络都具有神经网络的链式重复模块。

在标准的 RNN 中，这个**重复模块具有非常简单的结构**，例如**只有单个 tanh 层**。

![RNN中，只有单个tanh层](https://easy-ai.oss-cn-shanghai.aliyuncs.com/2019-07-05-rnn-tanh.png)

LSTM 也具有这种类似的链式结构，但重复模块具有不同的结构。

不是一个单独的神经网络层，而是**四个，并且以非常特殊的方式进行交互**。

![LSTM有4个神经网络层](https://easy-ai.oss-cn-shanghai.aliyuncs.com/2019-07-05-lstm.png)

不要担心细节。



稍后我们将逐步浏览 LSTM 的图解。

现在，让我们试着去熟悉我们将使用的符号。

![不同符号的含义](https://easy-ai.oss-cn-shanghai.aliyuncs.com/2019-07-05-fuhao.png)

在上面的图中：

- 黄色框表示学习的神经网络层。
- 粉色圆圈表示逐点运算，如向量加法；
- 每行包含一个完整的向量，从一个节点的输出到其他节点的输入
- 行合并表示串联，
- 而分支表示其内容正在被复制，并且副本将转到不同的位置。

 

## LSTM的核心思路

==LSTM 的关键是细胞状态==，即图中**上方的水平线**。

细胞状态有点像传送带。

它贯穿整个链条，只有一些次要的线性交互作用。

信息很容易以不变的方式流过。

![LSTM 的关键是细胞状态，即图中上方的水平线](https://easy-ai.oss-cn-shanghai.aliyuncs.com/2019-07-05-xibao.png)



LSTM 可以通过**所谓“门”的精细结构**==向细胞状态添加或移除信息。==

==门可以选择性地以让信息通过。==

它们由 S 形神经网络层和逐点乘法运算组成。

![LSTM 可以通过所谓“门”的精细结构向细胞状态添加或移除信息](https://easy-ai.oss-cn-shanghai.aliyuncs.com/2019-07-05-men.png)

S 形网络的输出值介于 0 和 1 之间，表示有多大比例的信息通过。

- 0 值表示“没有信息通过”
- 1 值表示“所有信息通过”。

一个 LSTM 有三种这样的门用来保持和控制细胞状态。

如果对详细的技术原理感兴趣，可以看看这篇文章《[Illustrated Guide to LSTM’s and GRU’s: A step by step explanation](https://towardsdatascience.com/illustrated-guide-to-lstms-and-gru-s-a-step-by-step-explanation-44e9eb85bf21)》

 

## 百度百科+维基百科

百度百科版本

长短期记忆人工神经网络（Long-Short Term Memory,LSTM）论文首次发表于1997年。

由于**独特的设计结构**，LSTM适合于处理和预测**时间序列中间隔和延迟非常长的重要事件**。

LSTM的表现通常比时间递归神经网络RNN及隐马尔科夫模型（HMM）更好，比如用在不分段连续手写识别上。

2009年，用LSTM构建的人工神经网络模型赢得过ICDAR手写识别比赛冠军。

LSTM还普遍用于自主语音识别，2013年运用TIMIT自然演讲数据库达成17.7%错误率的纪录。

作为非线性模型，LSTM可作为**复杂的非线性单元**用于构造更大型深度神经网络。

[查看详情](https://baike.baidu.com/item/长短期记忆人工神经网络)



维基百科版本

长短期记忆（LSTM）单位是递归神经网络（RNN）的单位。

由LSTM单元组成的RNN通常称为LSTM网络（或仅称为LSTM）。

==公共LSTM单元由单元，输入门，输出门和忘记门组成==。

- 该单元记住**任意时间间隔内**的值，
- 并且三个门控制进出单元的**信息流**。

LSTM网络非常适合基于**时间序列数据进行分类，处理和预测**，因为在时间序列中的重要事件之间可能存在未知持续时间的滞后。

开发LSTM是为了处理在训练传统RNN时可能遇到的**爆炸和消失的梯度问题**。

==对于间隙长度的相对不敏感性==是LSTM相对于RNN，隐马尔可夫模型和其他序列学习方法在许多应用中的优势。

[查看详情](https://en.wikipedia.org/wiki/Long_short-term_memory)

 



# #参考文献

入门类文章（4）

[理解 LSTM 网络](https://www.cnblogs.com/xuruilong100/p/8506949.html)

[Illustrated Guide to LSTM’s and GRU’s: A step by step explanation](https://towardsdatascience.com/illustrated-guide-to-lstms-and-gru-s-a-step-by-step-explanation-44e9eb85bf21)

[Long Short-Term Memory: From Zero to Hero with PyTorch](https://blog.floydhub.com/long-short-term-memory-from-zero-to-hero-with-pytorch/)

[如何理解LSTM？](https://easyai.tech/blog/understand-lstm/)