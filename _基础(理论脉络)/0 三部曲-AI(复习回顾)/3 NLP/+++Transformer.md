[Transformer](https://easyai.tech/ai-definition/transformer/)

## 什么是 Transformer（知乎版本）？

![transformer结构图](https://easy-ai.oss-cn-shanghai.aliyuncs.com/2020-05-06-transformer.jpg)

如上图所示，咋一看，Transformer 的架构是不是有点复杂。。。

和经典的 [seq2seq](https://easyai.tech/ai-definition/encoder-decoder-seq2seq/) 模型一样，Transformer 模型中也采用了 encoer-[decoder](https://easyai.tech/ai-definition/encoder-decoder-seq2seq/) 架构。



上图的左半边用 **NX** 框出来的，就代表一层 [encoder](https://easyai.tech/ai-definition/encoder-decoder-seq2seq/)，其中论文里面的 encoder 一共有6层这样的结构。

上图的右半边用 **NX** 框出来的，则代表一层 decoder，同样也有6层。

- 定义输入序列首先经过 [word embedding](https://easyai.tech/ai-definition/word-embedding/)，
- 再和 positional encoding 相加后，输入到 encoder 中。
- 输出序列经过的处理和输入序列一样，然后输入到 decoder。

最后，==decoder 的输出经过一个线性层，再接 Softmax==。

这便是 Transformer 的整体框架，下面先来介绍 encoder 和 decoder。

[原文地址|知乎专栏 聊聊Transformer](https://zhuanlan.zhihu.com/p/47812375)

 

## 什么是 Transformer（微软研究院笨笨）？

Transformer在2017年由Google在题为《[Attention](https://easyai.tech/ai-definition/attention/) Is All You Need》的论文中提出。

Transformer是一个完全基于注意力机制的编解码器模型，它抛弃了之前其它模型引入注意力机制后仍然保留的**循环与卷积结构**，而采用了自注意力（Self-attention）机制，在任务表现、并行能力和易于训练性方面都有大幅的提高。

在 Transformer 出现之前，基于神经网络的机器翻译模型多数都采用了 [RNN](https://easyai.tech/ai-definition/rnn/)的模型架构，它们依靠循环功能进行有序的序列操作。

虽然 RNN 架构有较强的序列建模能力，但是存在训练速度慢，训练质量低等问题。



与基于 RNN 的方法不同，**Transformer 模型中没有循环结构**，而是

- 把==序列中的所有单词或者符号并行处理==，
- 同时借助自注意力机制对句子中所有单词之间的关系直接进行建模，而无需考虑各自的位置。

具体而言，如果要计算给定单词的下一个表征，Transformer 会将该单词与句子中的其它单词一一对比，并得出这些单词的注意力分数。

注意力分数决定其它单词对给定词汇的语义影响。

之后，注意力分数用作所有单词表征的平均权重，这些表征**输入全连接网络，生成新表征**。



由于 Transformer **并行处理所有的词**，以及每个单词都可以在多个处理步骤内与其它单词之间产生联系，它的训练速度比 RNN 模型更快，在翻译任务中的表现也比 RNN 模型更好。

除了计算性能和更高的准确度，Transformer 另一个亮点是可以对网络关注的句子部分进行可视化，尤其是在处理或翻译一个给定词时，因此可以深入了解信息是如何通过网络传播的。

之后，Google的研究人员们又对标准的 Transformer 模型进行了拓展，采用了一种新型的、注重效率的时间并行循环结构，让它具有通用计算能力，并在更多任务中取得了更好的结果。



改进的模型（Universal Transformer）在保留Transformer 模型原有并行结构的基础上，把 Transformer 一组几个各异的固定的变换函数替换成了一组由单个的、时间并行的循环变换函数构成的结构。

相比于 RNN一个符号接着一个符号从左至右依次处理序列，Universal Transformer 和 Transformer 能够一次同时处理所有的符号，但 Universal Transformer 接下来会根据自注意力机制对每个符号的解释做数次并行的循环处理修饰。

Universal Transformer 中时间并行的循环机制不仅比 RNN 中使用的串行循环速度更快，也让 Universal Transformer 比标准的前馈 Transformer 更加强大。

上面内容转载在公众号 微软研究院AI头条，[原文地址](https://mp.weixin.qq.com/s/bgfj5-RtoiWFCToHWVRTmw)

 

## 扩展阅读

入门类文章（6）

[从头开始了解Transformer](https://mp.weixin.qq.com/s/zSsvW0CLrylQyS4ovayfjg)(2018年 8 月)

[BERT大火却不懂Transformer？读这一篇就够了](https://zhuanlan.zhihu.com/p/54356280)

[可视化理解Transformer结构](https://zhuanlan.zhihu.com/p/59629215)

[《Attention is All You Need》浅读（简介+代码）](https://kexue.fm/archives/4765)

[详解Transformer （Attention Is All You Need）](https://zhuanlan.zhihu.com/p/48508221)

[深度学习中的注意力机制](https://cloud.tencent.com/developer/article/1143127)

[Attention? Attention!](https://lilianweng.github.io/lil-log/2018/06/24/attention-attention.html)

拓展视野类文章（6）

[理解NLP中网红特征抽取器Tranformer](https://mp.weixin.qq.com/s/_rP-0WgqRCyKq5toXLCEvw)(2019-7)

[为节约而生：从标准Attention到稀疏Attention](https://mp.weixin.qq.com/s/rrbwItXt-1EaGiqtDEGvog)（2019-7）

[OpenAI提出新方法Sparse Transformer，大幅度提高长程序列数据建模能力](https://mp.weixin.qq.com/s/b1RloCAcPl9l_PEcleoA2g)（2019-4）

[放弃幻想，全面拥抱Transformer！NLP三大特征抽取器（CNN/RNN/TF）比较](https://mp.weixin.qq.com/s/E7wygpWbSHoq6R7wlalFkA)（2019-3）

[经典算法·从seq2seq、attention到transformer](https://zhuanlan.zhihu.com/p/54368798)

[Google机器翻译新论文-更好更高效的演化transformer结构，提升机器翻译到新水平](https://easyai.tech/blog/evolved-transformer/)

实践类文章（2）

[Transformer 模型的 PyTorch 实现](https://juejin.im/post/5b9f1af0e51d450e425eb32d)

[搞懂Transformer结构，看这篇PyTorch实现就够了（上）](https://zhuanlan.zhihu.com/p/48731949)

相关资源（1）

[推理速度快千倍！谷歌开源语言模型Transformer-XL](http://www.chuangyejia.com/article-11842908.html)