[词嵌入 | Word embedding](https://easyai.tech/ai-definition/word-embedding/)

- [文本表示（Representation）](https://easyai.tech/ai-definition/word-embedding/#representation)
- [独热编码 | one-hot representation](https://easyai.tech/ai-definition/word-embedding/#one-hot)
- [整数编码](https://easyai.tech/ai-definition/word-embedding/#number)
- [什么是词嵌入 | word embedding？](https://easyai.tech/ai-definition/word-embedding/#wordembedding)
- [2 种主流的 word embedding 算法](https://easyai.tech/ai-definition/word-embedding/#2suanfa)
- [百度百科和维基百科](https://easyai.tech/ai-definition/word-embedding/#baidu)

>  访问 NLP 专题，下载 59 页免费 PDF

## 文本表示（Representation）

文本是一种**非结构化的数据信息，是不可以直接被计算的**。

**文本表示的作用就是将这些非结构化的信息转化为结构化的信息**，这样就可以针对文本信息做计算，来完成我们日常所能见到的文本分类，情感判断等任务。

![文本表示将非结构化数据转化为结构化数据](https://easy-ai.oss-cn-shanghai.aliyuncs.com/2020-02-17-representation.png)



文本表示的方法有很多种，下面只介绍 3 类方式：

1. 独热编码 | one-hot representation
2. 整数编码
3. ==词嵌入 | word embedding==

![word embedding的关系](https://easy-ai.oss-cn-shanghai.aliyuncs.com/2020-02-17-guanxi.png)

 

## 独热编码 | one-hot representation

假如我们要计算的文本中一共出现了4个词：猫、狗、牛、羊。

向量里每一个位置都代表一个词。所以用 one-hot 来表示就是：

猫：［1，0，0，0］

狗：［0，1，0，0］

牛：［0，0，1，0］

羊：［0，0，0，1］

![one-hot编码](https://easy-ai.oss-cn-shanghai.aliyuncs.com/2020-02-17-one-hot.png)

但是在实际情况中，文本中很可能出现成千上万个不同的词，这时候向量就会非常长。其中99%以上都是 0。

one-hot 的缺点如下：

1. 无法表达词语之间的关系
2. 这种**过于稀疏的向量，导致计算和存储的效率都不高**

 

## 整数编码

这种方式也非常好理解，用一种数字来代表一个词，上面的例子则是：

猫：1

狗：2

牛：3

羊：4

![整数编码](https://easy-ai.oss-cn-shanghai.aliyuncs.com/2020-02-17-number.png)

将句子里的每个词拼起来就是可以表示一句话的向量。

整数编码的缺点如下：

1. ==无法表达词语之间的关系==
2. 对于**模型解释**而言，整数编码可能具有挑战性。

 

## 什么是词嵌入 | word embedding？

word embedding 是文本表示的一类方法。

跟 one-hot 编码和整数编码的目的一样，不过他有更多的优点。

词嵌入并不特指某个具体的算法，跟上面2种方式相比，这种方法有几个明显的优势：

1. 他可以==将文本通过一个低维向量来表达==，不像 one-hot 那么长。
2. ==语意相似的词在向量空间上也会比较相近==。
3. 通用性很强，可以用在不同的任务中。



再回顾上面的例子：

![word embedding：语意相似的词在向量空间上也会比较相近](https://easy-ai.oss-cn-shanghai.aliyuncs.com/2020-02-17-we.png)

 

## 2 种主流的 word embedding 算法

![2 种主流的 word embedding 算法](https://easy-ai.oss-cn-shanghai.aliyuncs.com/2020-02-17-2type.png)

### Word2vec

这是一种**基于统计方法**来获得词向量的方法，他是 2013 年由谷歌的 Mikolov 提出了一套新的词嵌入方法。

这种算法有2种训练模式：

1. 通过**上下文来预测**当前词
2. 通过**当前词来预测上下文**

想要详细了解 [Word2vec](https://easyai.tech/ai-definition/word2vec/)，可以看看这篇文章：《[一文看懂 Word2vec（基本概念+2种训练模型+5个优缺点）](https://easyai.tech/ai-definition/word2vec/)》

 

### GloVe

GloVe 是对 Word2vec 方法的扩展，它将**全局统计**和 **Word2vec 的基于上下文的学习**结合了起来。

想要了解 GloVe 的 三步实现方式、训练方法、和 w2c 的比较。可以看看这篇文章：《[GloVe详解](http://www.fanyeong.com/2018/02/19/glove-in-detail/)》

 

## 百度百科和维基百科

百度百科版本

词向量（Word embedding），又叫Word嵌入式自然语言处理（NLP）中的一组语言建模和特征学习技术的统称，其中来**自词汇表的单词或短语被映射到实数的向量**。

 从概念上讲，它涉及从每个单词一维的空间到具有更低维度的连续向量空间的数学嵌入。

生成这种映射的方法包括神经网络，单词共生矩阵的降维，概率模型，可解释的知识库方法，和术语的显式表示 单词出现的背景。

当用作底层输入表示时，单词和短语嵌入已经被证明可以提高NLP任务的性能，例如语法分析和情感分析。

[查看详情](https://baike.baidu.com/item/词向量)



维基百科版本

Word embedding 是自然语言处理中的重要环节，它是一些语言处理模型的统称，并不具体指某种算法或模型。

Word embedding 的任务是把词转换成可以计算的向量 。

从概念上讲，它涉及从每个单词一维的空间到具有更低维度的连续向量空间的数学嵌入。

生成这种映射的方法包括神经网络，单词共生矩阵的降维，概率模型，可解释的知识库方法，和术语的显式表示单词出现的上下文。

当用作底层输入表示时，单词和短语嵌入已经被证明可以提高NLP任务的性能，例如句法分析和情感分析。

[查看详情](https://en.wikipedia.org/wiki/Word_embedding)