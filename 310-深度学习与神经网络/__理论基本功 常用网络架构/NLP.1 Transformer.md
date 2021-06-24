-  Encoder
- Decoder
- Attention

> Transformer模型是目前机器翻译等NLP问题最好的解决办法，比RNN有大幅提高

# 一、[通俗理解](https://www.bilibili.com/video/BV1Zz4y127h1/?spm_id_from=333.788.recommend_more_video.0)

## 机器翻译

1 RNN

单词的先后顺序会影响句子的意义 

RNN擅长捕捉序列关系：但是输入、输出数量有限制

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210613152639.png" alt="image-20210613152638614" style="zoom: 50%;" />





2 Seq2Seq：Encoder + Decoder

关键在于“意义单元”这一**中介**

- +解决了两端单词数不对等的情况
- -意义单元**能够存储的信息**是有限的 => 如果句子太长，翻译精度会下降

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210613152847.png" alt="image-20210613152846694" style="zoom: 50%;" />



3 Attention

在Seq2Seq的基础上：生成每个单词时，都有意识地**从原始句子中，提取生成该单词时**最需要的信息

- +成功摆脱了输入序列的长度限制
- -计算方式太慢：RNN需要逐个看过句子中的单词，才能给出输出

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210613153058.png" alt="image-20210613153058091" style="zoom: 50%;" />



4 Self Attention

1）先提取每个单词的意义 => $C_i$

2）再依据生成顺序，选取所需要的信息

+支持并行计算，效率更高

+接近人类的翻译方式💡

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210613153242.png" alt="image-20210613153241882" style="zoom: 50%;" />



备注：已经脱离了RNN => ==Transformer: 完全基于Self Attention，由Encoder、Decoder构成的网络==

效果|横扫NLP领域：

- 文本摘要
- Chatbot⭐
- 万能：
  - 图像分类
  - 语音识别
  - 音乐生成
  - 股价预测



评论：Word2vec

- 思想没有过时 现在推荐领域很多方法都借鉴了word2vec的思想
- 13年火的模型，中间都经历了很多变体。Word2vec考虑的信息不够全面



# 二、[经典详解](https://www.bilibili.com/video/BV1jv41167M2)⭐

#### **前言**

[原文链接](https://jalammar.github.io/illustrated-transformer/)。

在之前的[文章](https://jalammar.github.io/visualizing-neural-machine-translation-mechanics-of-seq2seq-models-with-attention/)中，Attention成了深度学习模型中无处不在的方法，它是种帮助提升NMT（Neural Machine Translation）的翻译效果的思想。Transformer该模型扩展Attention来加速训练，并且在特定任务上 transformer 表现比 Google NMT 模型还要好。

然而，其最大的好处是可并行。实际上[谷歌云](https://cloud.google.com/tpu/)推荐将Transformer作为云TPU的推导模型。现在我们将Transformer拆解开来看看它是如何工作的。

Transformer是在"[Attention is All You Need](https://arxiv.org/abs/1706.03762)"中提出的，其中的TF应用是[Tensor2Tensor](https://github.com/tensorflow/tensor2tensor)的子模块。

哈佛的NLP团队专门制作了对应的PyTorch的[指南说明](http://nlp.seas.harvard.edu/2018/04/03/attention.html)。本文旨在简化难度，一步一步地解释其中的概念，希望有助于初学者更容易地理解。



#### Transformer架构

我们先将整个模型视为黑盒，比如在机器翻译中，接收一种语言的句子作为输入，然后将其翻译成其他语言输出。

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210624094421.png" alt="img" style="zoom:67%;" />

细看下，其中由编码组件、解码组件和它们之间的连接层组成。

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210624094428.png" alt="img" style="zoom:67%;" />

编码组件是六层编码器**首尾相连**堆砌而成，解码组件也是六层解码器堆成的。

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210624094434.png" alt="img" style="zoom:67%;" />

编码器是**完全结构相同的，但是并不共享参数**，每一个编码器都可以拆解成以下两个组成部分。

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210624094532.png" alt="img" style="zoom:67%;" />

编码器的输入首先经过一个self-attention层，该层帮助编码器能够看到输入序列中的其他单词，当它编码某个词时。
self-attention的输出流向一个前向网络，每个输入位置对应的前向网络是独立互不干扰的。
**解码器同样也有这些子层**，但是在两个子层间**增加了attention层，该层有助于解码器能够关注到输入句子的相关部分**，与 [seq2seq model](https://jalammar.github.io/visualizing-neural-machine-translation-mechanics-of-seq2seq-models-with-attention/) 的Attention作用相似。

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210624094641.png" alt="img" style="zoom:67%;" />

#### 词向量 => 句子list

现在，我们解析下模型最主要的组件，从向量/Tensor开始，然后是它们如何流经各个组件们并输出的。
正如NLP应用的常见例子，先将输入单词使用[embedding algorithm](https://medium.com/deeper-learning/glossary-of-deep-learning-word-embedding-f90c3cec34ca)转成向量。

![img](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210624102536.png)

**每个词映射到512维向量上**，此处用box表示向量

词的向量化仅仅发生在最底层的编码器的输入时，这样每个编码器的都会接收到一个list（list的每个元素都是512维的词向量），只不过其他编码器的输入是前个编码器的输出。**list的尺寸是可以设置的超参，通常是训练集的最长句子的长度**。
在对输入序列做词的向量化之后，它们流经编码器的如下两个子层。

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210624102547.png" alt="img" style="zoom:67%;" />

这里能看到Transformer的一个关键特性，**每个位置的词仅仅流过它自己的编码器路径**。

在self-attention层中，这些路径两两之间是相互依赖的。

**前向网络层则没有这些依赖性**，但这些路径在流经前向网络时可以并行执行。



#### self-attention模块

正如之前所提，编码器**接收向量的list作输入**。

然后将其送入self-attention处理，再之后送入前向网络，最后将输入传入下一个编码器。

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210624102714.png" alt="img" style="zoom:67%;" />

每个位置的词向量被送入self-attention模块，然后是前向网络(对每个向量都是完全相同的网络结构)



#### Self-Attention的作用

不要被self-attention这个词迷惑了，看起来好像每个人对它都很熟悉，但是在我读到Attention is All You Need这篇文章之前，我个人都没弄懂这个概念。

下面我们逐步分解下它是如何工作的。
以下面这句话为例，作为我们想要翻译的输入语句“The animal didn’t cross the street because it was too tired”。句子中"it"指的是什么呢？“it"指的是"street” 还是“animal”？对人来说很简单的问题，但是对算法而言并不简单。

当模型处理单词“it”时，**self-attention允许将“it”和“animal”联系起来**。

当模型处理每个位置的词时，self-attention允许模型看到**句子的其他位置信息作辅助线索来更好地编码当前词**。

如果你对RNN熟悉，就能想到RNN的隐状态是如何**允许之前的词向量**来解释合成当前词的解释向量。

Transformer使用self-attention来==将相关词的理解==编码到当前词中。

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210624102844.png" alt="img" style="zoom:80%;" />

==当编码"it"时（编码器的最后层输出）==，部分attention集中于"the animal"，并将其表示合并进入到“it”的编码中

上图是[Tensor2Tensor notebook](https://colab.research.google.com/github/tensorflow/tensor2tensor/blob/master/tensor2tensor/notebooks/hello_t2t.ipynb)的可视化例子



#### **Self-Attention的底层原理**

我们先看下如何计算self-attention的向量，再看下如何以矩阵方式计算。

**第一步**，根据编码器的输入向量，生成三个向量，比如，对每个词向量，生成query-vec, key-vec, value-vec，生成方法为**分别乘以三个矩阵**，这些矩阵在训练过程中需要学习。

【注意：不是每个词向量独享3个matrix，而是==**所有输入**共享3个转换矩阵；**权重矩阵是基于输入位置的转换矩阵**==；有个可以尝试的点，如果每个词独享一个转换矩阵，会不会效果更厉害呢？】

注意到这些**新向量的维度**比输入词向量的维度要小（512–>64），并不是必须要小的，是为了让多头attention的计算更稳定。

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210624104751.png" alt="img" style="zoom:80%;" />

输入乘以W^q得到query



所谓的query/key/value-vec是什么？
这种提取对计算和思考attention是有益的，当读完下面attention是如何计算的之后，你将对这些向量的角色有更清晰的了解。

**第二步**，计算attention就是计算一个分值。

对“Thinking Matchines”这句话，对“Thinking”（pos#1）计算attention 分值。

我们需要计算**每个词与“Thinking”的评估分**，这个分决定着编码“Thinking”时（某个固定位置时），**每个输入词需要集中多少关注度**。

这个分，通过“Thing”对应query-vector与所有词的key-vec**依次做点积**得到。

所以当我们处理位置#1时，第一个分值是q1和k1的点积，第二个分值是q1和k2的点积。

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210624105040.png" alt="img" style="zoom:80%;" />

**第三步和第四步**，除以8（新向量维度$d_k$的平方根），这样==梯度会更稳定==。

然后加上softmax操作，归一化分值使得全为正数且加和为1。

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210624105048.png" alt="img" style="zoom:80%;" />

softmax分值决定着在这个位置，**每个词的表达程度（关注度）**。

很明显，这个位置的词应该**有最高的归一化分数**，但大部分时候总是有助于关注该词的相关的词。

**第五步**，将softmax分值与**value-vec**按位相乘。==保留关注词$v_1$==的value值，削弱**非相关词的value值**。
**第六步**，将所有加权向量加和，产生该位置的self-attention的输出结果。

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210624105149.png" alt="img" style="zoom:80%;" />

上述就是self-attention的计算过程，生成的向量流入前向网络。

在实际应用中，上述计算是以速度更快的矩阵形式进行的。下面我们看下在单词级别的矩阵计算。



#### Self-Attention的矩阵计算

**第一步**，计算query/key/value matrix，将所有输入词向量合并成输入矩阵X，并且将其分别乘以权重矩阵

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210624105236.png" alt="img" style="zoom:80%;" />

**输入矩阵X的每一行**：表示输入句子的**一个词向量**



**最后**，鉴于我们使用矩阵处理，将步骤2~6合并成一个计算self-attention层输出的公式。

- me|根据如下矩阵公式：说明上述图中的$z_1, z_2$均保留(只不过由于关注词是$x_1$，所以$z_1$更明显)

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210624105335.png" alt="img" style="zoom:67%;" />



#### +multi-headed的机制

> me|理解：避免过拟合，提供了一种更稳定的机制

论文进一步增加了multi-headed的机制到self-attention上，在如下两个方面提高了attention层的效果：

1. 多头机制扩展了模型**集中于不同位置的能力**。在上面的例子中，z1只包含了其他词的很少信息，仅由实际自己词决定。在其他情况下，比如翻译 “The animal didn’t cross the street because it was too tired”时，我们想知道单词"it"指的是什么。
2. 多头机制赋予attention多种**子表达方式**。像下面的例子所示，在多头下有**多组query/key/value-matrix**，而非仅仅一组（论文中使用8-heads）。每一组都是随机初始化，经过训练之后，==输入向量可以被映射到不同的子表达空间==中。

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210624105436.png" alt="img" style="zoom:67%;" />

每个head都有一组Q/K/V matrix

如果我们计算multi-headed self-attention的，分别有八组不同的Q/K/V matrix，我们得到**八个不同的矩阵**。

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210624105454.png" alt="img" style="zoom:67%;" />

这会带来点麻烦，**前向网络并不能接收八个矩阵**，而是希望输入是一个矩阵，所以要有种方式处理下八个矩阵合并成一个矩阵。

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210624105507.png" alt="img" style="zoom:67%;" />

上述就是多头自注意机制的内容，我认为还仅是一部分矩阵，下面尝试着将它们放到一个图上可视化如下。

- 1 $Z_i$表示不同Head对应的编码输出
- 2 最终把所有的Heads信息汇总 => 得到目标Z

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210624105608.png" alt="img" style="zoom:80%;" />

现在加入attention heads之后，重新看下当编码“it”时，哪些attention head会被集中。

![img](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210624105618.png)

编码"it"时，一个attention head集中于"the animal"，另一个head集中于“tired”，某种意义上讲，模型对“it”的表达合成了的“animal”和“tired”两者

如果我们将所有的attention heads都放入到图中，就很难直观地解释了。



#### 位置编码

截止到目前为止，我们还没有讨论如何理解输入语句中**词的顺序**。
为解决词序的利用问题，Transformer==新增了一个向量==对每个词，这些向量遵循模型学习的指定模式，来决定词的位置，或者序列中不同词的举例。

对其理解，增加这些值来提供词向量间的距离，当其映射到Q/K/V向量以及点乘的attention时。

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210624105855.png" alt="img" style="zoom:67%;" />

为了能够给模型提供词序的信息，**新增位置emb向量**，每个向量值都==遵循指定模式==

如果假设位置向量有4维，实际的位置向量将如下所示：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210624105948.png" alt="img" style="zoom:67%;" />



@一个只有4维的位置向量表示例子

所谓的指定模式是什么样的呢？

- 在下图中，**每一行表示一个位置的pos-emb**，所以第一行是我们将要加到句子第一个词向量上的vector。

- **每个行有512值**，每个值范围在[-1,1]，我们将要涂色以便于能够将模式可视化。

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210624110108.png" alt="img" style="zoom:80%;" />

一个真实的例子有20个词，每个词512维。可以观察中间显著的分隔，那是因为**左侧是用sine函数生成**，**右侧是用cosine生成**。

位置向量编码方法在论文的3.5节有提到，也可以看代码[get_timing_signal_ld()](https://github.com/tensorflow/tensor2tensor/blob/23bd23b9830059fbc349381b70d9429b5c40a139/tensor2tensor/layers/common_attention.py)，对位置编码而言并不只有一种方法。

需要注意的是，编码方法==必须能够处理未知长度的序列==。



#### **The Residuals**

编码器结构中值得提出注意的一个细节是，在每个子层中（slef-attention, ffnn），都有**残差连接**，并且紧跟着[layer-normalization](https://arxiv.org/abs/1607.06450)。

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210624110124.png" alt="img" style="zoom:80%;" />

如果我们可视化向量和layer-norm操作，将如下所示：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210624110131.png" alt="img" style="zoom: 80%;" />

在解码器中也是如此，假设两层编码器+两层解码器组成Transformer，其结构如下：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210624110213.png" alt="img" style="zoom: 67%;" />



#### Decoder

现在我们已经了解了编码器侧的大部分概念，也基本了解了解码器的工作方式，下面看下他们是如何共同工作的。
编码器从输入序列的处理开始，最后的编码器的**输出被转换为K和V**，它俩被每个解码器的"encoder-decoder atttention"层来使用，帮助解码器集中于输入序列的合适位置。

- 备注：编码器只有一个输出；分别经过计算，变成了解码器的key和value输入矩阵

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210624110254.gif" alt="img" style="zoom: 67%;" />

在编码之后，是解码过程；解码的**每一步输出一个元素**作输出序列

下面的步骤一直重复，**直到一个特殊符号出现表示解码器完成了翻译输出**。

每一步的输出被喂到下一个解码器中。正如编码器的输入所做的处理，==对解码器的输入增加位置向量==。

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210624110425.gif" alt="img" style="zoom:67%;" />

在解码器中的self attention层与编码器中的稍有不同，在解码器中，self-attention 层仅仅允许关注**早于当前输出的位置**。

- 在softmax之前，通过**遮挡未来位置**（将它们设置为-inf）来实现。

"Encoder-Decoder Attention "层工作方式跟multi-headed self-attention是一样的，除了一点，

- 它从前层获取输出转成query矩阵， => 只有输入query是在Decoder中
- 接收最后层编码器的key和value矩阵做key和value矩阵。



#### **The Final Linear and Softmax Layer**

解码器**最后输出浮点向量**，如何将它转成词？这是最后的线性层和softmax层的主要工作。
线性层是个简单的全连接层，将解码器的最后输出映射到==一个非常大的logits向量==上。假设模型已知有1万个单词（输出的词表）**从训练集中学习**得到。那么，logits向量就有1万维，每个值表示是某个词的可能倾向值。

softmax层将这些分数转换成概率值（都是正值，且加和为1），最高值对应的维上的词就是这一步的输出单词。

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210624110842.png" alt="img" style="zoom:67%;" />



#### **Recap of Training**

现在我们已经了解了一个训练完毕的Transformer的前向过程，顺道看下训练的概念也是非常有用的。
在训练时，模型将经历上述的前向过程，当我们在**标记训练集**上训练时，可以对比预测输出与实际输出。
为了可视化，**假设输出一共只有6个单词**（“a”, “am”, “i”, “thanks”, “student”, “”）

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210624110912.png" alt="img" style="zoom:67%;" />

模型的词表是**在训练之前的预处理中生成的**

一旦定义了词表，我们就能够构造一个**同维度的向量**来表示每个单词，比如one-hot编码，下面举例编码“am”。

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210624110930.png" alt="img" style="zoom:67%;" />

举例采用one-hot编码输出词表

下面让我们讨论下模型的loss损失，在训练过程中用来优化的指标，指导学习得到一个非常准确的模型。



#### **The Loss Function**

我们用一个简单的例子来示范训练，比如翻译“merci”为“thanks”。

那意味着输出的概率分布指向单词“thanks”，但是由于模型未训练是随机初始化的，不太可能就是期望的输出。

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210624110952.png" alt="img" style="zoom:67%;" />

由于模型参数是随机初始化的，未训练的模型输出随机值。

我们可以对比真实输出，然后**利用误差后传调整模型权重，使得输出更接近与真实输出**



如何对比两个概率分布呢？简单采用 [cross-entropy](https://colah.github.io/posts/2015-09-Visual-Information/)或者[Kullback-Leibler divergence](https://www.countbayesie.com/blog/2017/5/9/kullback-leibler-divergence-explained)中的一种。
鉴于这是个极其简单的例子，更真实的情况是，使用一个句子作为输入。

比如，输入是“je suis étudiant”，期望输出是“i am a student”。

在这个例子下，我们期望模型输出连续的概率分布满足如下条件：
1 每个概率分布都与词表**同维度**。
2 第一个概率分布对“i”具有最高的预测概率值。
3 第二个概率分布对“am”具有最高的预测概率值。
4 一直到第五个输出指向""标记。

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210624111028.png" alt="img" style="zoom:67%;" />

对一个句子而言，训练模型的目标概率分布

在足够大的训练集上训练足够时间之后，我们期望产生的概率分布如下所示：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210624111049.png" alt="img" style="zoom:67%;" />

训练好之后，模型的输出是我们期望的翻译。

当然，这并不意味着这一过程是来自训练集。注意，**每个位置都能有值**，即便与输出近乎无关，这也是softmax对训练有帮助的地方。

现在，因为模型**每步只产生一组输出**，假设模型选择最高概率，扔掉其他的部分，这是种产生预测结果的方法，叫做==greedy 解码==。

另外一种方法是beam search，每一步仅保留**最头部高概率的两个输出**，根据这俩输出再预测下一步，再保留头部高概率的两个输出，重复直到预测结束。top_beams是**超参可试验调整**。



#### **Go Forth And Transform**

希望本文能够帮助读者对Transformer的主要概念理解有个破冰效果，如果想更深入了解，建议如下步骤：
1 阅读 [Attention Is All You Need](https://arxiv.org/abs/1706.03762)paper，Transformer的博客文章[Transformer: A Novel Neural Network Architecture for Language Understanding](https://ai.googleblog.com/2017/08/transformer-novel-neural-network.html)，[Tensor2Tensor](https://ai.googleblog.com/2017/06/accelerating-deep-learning-research.html)使用说明。
2 观看"[Łukasz Kaiser’s talk](https://www.youtube.com/watch?v=rBCqOTEfxvg)"，梳理整个模型及其细节。
3 耍一下项目[Jupyter Notebook provided as part of the Tensor2Tensor repo](https://colab.research.google.com/github/tensorflow/tensor2tensor/blob/master/tensor2tensor/notebooks/hello_t2t.ipynb)
4 尝试下项目[Tensor2Tensor](https://github.com/tensorflow/tensor2tensor)



相关工作

1. [Depthwise Separable Convolutions for Neural Machine Translation](https://arxiv.org/abs/1706.03059)
2. [One Model To Learn Them All](https://arxiv.org/abs/1706.03059)
3. [Discrete Autoencoders for Sequence Models](https://arxiv.org/abs/1801.09797)
4. [Generating Wikipedia by Summarizing Long Sequences](https://arxiv.org/abs/1801.10198)
5. [Image Transformer](https://arxiv.org/abs/1802.05751)
6. [Training Tips for the Transformer Model](https://arxiv.org/abs/1804.00247)
7. [Self-Attention with Relative Position Representations](https://arxiv.org/abs/1803.02155)
8. [Fast Decoding in Sequence Models using Discrete Latent Variables](https://arxiv.org/abs/1803.03382)
9. [Adafactor: Adaptive Learning Rates with Sublinear Memory Cost](https://arxiv.org/abs/1804.04235)



致谢：
Thanks to Illia Polosukhin, Jakob Uszkoreit, Llion Jones , Lukasz Kaiser, Niki Parmar, and Noam Shazeer for providing feedback on earlier versions of this post.
Please hit me up on [Twitter](https://twitter.com/jalammar) for any corrections or feedback.





# 补充

1 [动画图解](https://www.bilibili.com/video/BV1U64y1D7NC/?spm_id_from=333.788.recommend_more_video.4)

- Word2Vec => 映射矩阵
- 训练 => 同一主题下映射的区域相近

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210613163719.png" alt="image-20210613163718552" style="zoom: 67%;" />



# #参考文献

0 [博客|形象理解Transformer](http://jalammar.github.io/illustrated-transformer/)

