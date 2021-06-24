[自然语言处理-Natural language processing | NLP](https://easyai.tech/ai-definition/nlp/)

- [NLP 为什么重要？](https://easyai.tech/ai-definition/nlp/#important)
- [什么是自然语言处理 – NLP](https://easyai.tech/ai-definition/nlp/#waht)
- [NLP 的2大核心任务](https://easyai.tech/ai-definition/nlp/#2task)
- [NLP 的5个难点](https://easyai.tech/ai-definition/nlp/#nan)
- [NLP 的4个典型应用](https://easyai.tech/ai-definition/nlp/#aplication)
- [NLP 的 2 种途径、3 个核心步骤](https://easyai.tech/ai-definition/nlp/#steps)
- [总结](https://easyai.tech/ai-definition/nlp/#end)
- [百度百科版本+维基百科](https://easyai.tech/ai-definition/nlp/#baidu)

> 网络上有海量的文本信息，想要处理这些**非结构化的数据**就需要利用 NLP 技术。
>
> 本文将介绍 NLP 的基本概念，2大任务，4个典型应用和6个实践步骤。
>
> 
>
>  访问 NLP 专题，下载 59 页免费 PDF

 

## NLP 为什么重要？

> “==语言理解是人工智能领域皇冠上的明珠==” —— 比尔·盖茨

在人工智能出现之前，机器智能处理结构化的数据（例如 Excel 里的数据）。

但是网络中大部分的数据都是非结构化的，例如：文章、图片、音频、视频…

![结构化数据和非结构化数据](https://easy-ai.oss-cn-shanghai.aliyuncs.com/2019-07-23-2data.png)

在非结构数据中，**文本的数量是最多的**，他虽然没有图片和视频占用的空间大，但是他的信息量是最大的。

为了能够分析和利用这些文本信息，我们就需要利用 NLP 技术，让机器理解这些文本信息，并加以利用。

 

## 什么是自然语言处理 – NLP

每种动物都有自己的语言，机器也是！

自然语言处理（NLP）就是在机器语言和人类语言之间沟通的桥梁，以实现人机交流的目的。

人类通过语言来交流，狗通过汪汪叫来交流。

机器也有自己的交流方式，那就是数字信息。

![不同物种有自己的沟通方式](https://easy-ai.oss-cn-shanghai.aliyuncs.com/2019-07-23-lang.png)

不同的语言之间是无法沟通的，比如说人类就无法听懂狗叫，甚至不同语言的人类之间都无法直接交流，需要翻译才能交流。

而计算机更是如此，为了让计算机之间互相交流，**人们让所有计算机都遵守一些规则**，计算机的这些规则就是计算机之间的语言。

既然不同人类语言之间可以有翻译，那么人类和机器之间是否可以通过“翻译”的方式来直接交流呢？

NLP 就是人类和机器之间沟通的桥梁！

![NLP就是人类和机器之间沟通的桥梁](https://easy-ai.oss-cn-shanghai.aliyuncs.com/2019-07-23-nlp-qiaoliang.png)

**为什么是“自然语言”处理？**

自然语言就是大家平时在生活中常用的表达方式，大家平时说的「讲人话」就是这个意思。

> 自然语言：我背有点驼(非自然语言：我的背部呈**弯曲状**)
>
> 自然语言：宝宝的经纪人睡了宝宝的宝宝（微博上这种段子一大把）

 

## NLP 的2大核心任务

![NLP有2个核心任务：NLU和NLG](https://easy-ai.oss-cn-shanghai.aliyuncs.com/2019-07-23-nlu-nlg.png)



NLP 有2个核心的任务：

1. [自然语言理解 – NLU | NLI](https://easyai.tech/ai-definition/nlu/)
2. [自然语言生成 – NLG](https://easyai.tech/ai-definition/nlg/)

 

### 自然语言理解 – NLU|NLI

自然语言理解就是希望机器像人一样，具备正常人的语言理解能力，由于**自然语言在理解上有很多难点**(下面详细说明)，所以 [NLU](https://easyai.tech/ai-definition/nlu/) 是至今还远不如人类的表现。



**自然语言理解的5个难点：**

1. 语言的多样性
2. 语言的**歧义**性
3. 语言的鲁棒性
4. 语言的**知识依赖**
5. 语言的**上下文**

想要深入了解NLU，可以看看这篇文章《[一文看懂自然语言理解-NLU（基本概念+实际应用+3种实现方式）](https://easyai.tech/ai-definition/nlu/)》

 

### 自然语言生成 – NLG

[NLG](https://easyai.tech/ai-definition/nlg/) 是为了跨越人类和机器之间的沟通鸿沟，将**非语言格式的数据转换成人类可以理解的语言格式**，如文章、报告等。

**NLG 的6个步骤：**

1. 内容确定 – Content Determination
2. 文本结构 – Text Structuring
3. ==句子聚合 – Sentence Aggregation==
4. **语法化 – Lexicalisation**
5. 参考表达式生成 – Referring Expression Generation|REG
6. 语言实现 – Linguistic Realisation

想要深入了解NLG，可以看看这篇文章《[一文看懂自然语言生成 – NLG（6个实现步骤+3个典型应用）](https://easyai.tech/ai-definition/nlg/)》

 

## NLP 的5个难点

![NLP 的5个难点](https://easy-ai.oss-cn-shanghai.aliyuncs.com/2019-07-24-nandian.png)

1. 语言是没有规律的，或者说**规律是错综复杂的。**@德语
2. 语言是可以**自由组合的，可以组合复杂的语言表达**。
3. 语言是一个开放集合，我们**可以任意的发明创造一些新的表达方式**。
4. 语言需要联系到实践知识，==有一定的知识依赖==。
5. 语言的使用要==基于环境和上下文==。

 

## NLP 的4个典型应用

![NLP的4种典型应用](https://easy-ai.oss-cn-shanghai.aliyuncs.com/2019-07-23-yingyong.png)

**情感分析**

互联网上有大量的文本信息，这些信息想要表达的内容是五花八门的，但是他们抒发的情感是一致的：正面/积极的 – 负面/消极的。

通过情感分析，可以快速了解用户的舆情情况。

 

**聊天机器人**

过去只有 Siri、小冰这些机器人，大家使用的动力并不强，只是当做一个娱乐的方式。

但是最近几年智能音箱的快速发展让大家感受到了**聊天机器人的价值**。

而且未来随着智能家居，智能汽车的发展，聊天机器人会有更大的使用价值。

 

**语音识别**⭐

语音识别已经成为了全民级的引用，

- **微信里可以语音转文字**，
- **汽车中使用导航可以直接说目的地**，
- 老年人使用输入法也可以直接语音而不用学习拼音…

 

**机器翻译**

目前的机器翻译准确率已经很高了，大家使用 Google 翻译完全可以看懂文章的大意。

传统的人肉翻译未来很可能会失业。

 



## NLP 的 2 种途径、3 个核心步骤

NLP 可以使用传统的机器学习方法来处理，也可以使用深度学习的方法来处理。

2 种不同的途径也对应着不同的处理步骤。详情如下：



**方式 1：传统机器学习的 NLP 流程**

![传统机器学习的 NLP 流程](https://easy-ai.oss-cn-shanghai.aliyuncs.com/2019-08-12-ml-nlp.png)

1. 语料预处理
   1. 中文语料预处理 4 个步骤（下文详解）
   2. 英文语料预处理的 6 个步骤（下文详解）
2. 特征工程
   1. 特征提取
   2. 特征选择
3. 选择分类器

 

**方式 2：深度学习的 NLP 流程**

![深度学习的 NLP 流程](https://easy-ai.oss-cn-shanghai.aliyuncs.com/2019-08-12-dl-nlp.png)

1. 语料预处理
   1. 中文语料预处理 4 个步骤（下文详解）
   2. 英文语料预处理的 6 个步骤（下文详解）
2. 设计模型
3. 模型训练

 

**英文 NLP 语料预处理的 6 个步骤**

![**英文 NLP 语料预处理的 6 个步骤**](https://easy-ai.oss-cn-shanghai.aliyuncs.com/2019-08-12-6steps-en-1.png)

1. [分词 – Tokenization](https://easyai.tech/ai-definition/tokenization/)
2. [词干提取](https://easyai.tech/ai-definition/stemming-lemmatisation/) – [Stemming](https://easyai.tech/ai-definition/stemming-lemmatisation/)
3. [词形还原](https://easyai.tech/ai-definition/stemming-lemmatisation/) – Lemmatization
4. [词性标注 – Parts of Speech](https://easyai.tech/ai-definition/part-of-speech/)
5. [命名实体识别 – NER](https://easyai.tech/ai-definition/ner/)
6. 分块 – Chunking

 

**中文 NLP 语料预处理的 4 个步骤**

![**中文 NLP 语料预处理的 4 个步骤**](https://easy-ai.oss-cn-shanghai.aliyuncs.com/2019-08-12-4steps-cn-1.png)

1. [中文分词 – Chinese Word Segmentation](https://easyai.tech/ai-definition/tokenization/)
2. [词性标注 – Parts of Speech](https://easyai.tech/ai-definition/part-of-speech/)
3. [命名实体识别 – NER](https://easyai.tech/ai-definition/ner/)
4. 去除停用词

 

## 总结

自然语言处理（NLP）就是在机器语言和人类语言之间沟通的桥梁，以实现人机交流的目的。

 

**NLP的2个核心任务：**

1. 自然语言理解 – NLU
2. 自然语言生成 – NLG

 

**NLP 的5个难点：**

1. 语言是没有规律的，或者说规律是错综复杂的。
2. 语言是可以自由组合的，可以组合复杂的语言表达。
3. 语言是一个开放集合，我们可以任意的发明创造一些新的表达方式。
4. 语言需要联系到实践知识，有一定的知识依赖。
5. 语言的使用要基于环境和上下文。

 

**NLP 的4个典型应用：**

1. 情感分析
2. 聊天机器人
3. 语音识别
4. 机器翻译

 

**NLP 的6个实现步骤：**

1. 分词-[tokenization](https://easyai.tech/ai-definition/tokenization/)
2. 次干提取-stemming
3. 词形还原-lemmatization
4. 词性标注-pos tags
5. 命名实体识别-[ner](https://easyai.tech/ai-definition/ner/)
6. 分块-chunking

## 百度百科版本+维基百科

百度百科版本

自然语言处理是计算机科学领域与人工智能领域中的一个重要方向。它研究能实现人与计算机之间用自然语言进行有效通信的各种理论和方法。自然语言处理是一门融语言学、计算机科学、数学于一体的科学。因此，这一领域的研究将涉及自然语言，即人们日常使用的语言，所以它与语言学的研究有着密切的联系，但又有重要的区别。自然语言处理并不是一般地研究自然语言，而在于研制能有效地实现自然语言通信的计算机系统，特别是其中的软件系统。因而它是计算机科学的一部分。

自然语言处理（NLP）是计算机科学，人工智能，语言学关注计算机和人类（自然）语言之间的相互作用的领域。

[查看详情](https://baike.baidu.com/item/自然语言处理/365730)

 

维基百科版本

自然语言处理（NLP）是计算机科学，信息工程和人工智能的子领域，涉及计算机与人类（自然）语言之间的交互，特别是如何对计算机进行编程以处理和分析大量自然语言数据。自然语言处理中的挑战通常涉及语音识别，自然语言理解和自然语言生成。

[查看详情](https://en.wikipedia.org/wiki/Natural_language_processing)

 

## 扩展阅读

相关书籍（3）

《[统计自然语言处理基础](https://book.douban.com/subject/1224802/)》

《[自然语言处理综论](https://book.douban.com/subject/1390499/)》

《[Python自然语言处理》](https://book.douban.com/subject/5336893/)

开拓视野类文章（15）

[NLP领域中的迁移学习现状](https://mp.weixin.qq.com/s/xk53KWZvt_Bx1jEm95_V6Q)（2019-9）

[观点 | 认知智能的突围：NLP、知识图谱是AI下一个“掘金地”？](https://mp.weixin.qq.com/s/9FZngyvOCo4ak5Y6Dlw_VA)（2019-8）

[从发展滞后到不断突破，NLP已成为AI又一燃爆点？](https://mp.weixin.qq.com/s/XUFm0OEeuQNfa9ZBXMdTbg)（2019-7）

[【技术综述】深度学习在自然语言处理中的应用发展史](https://mp.weixin.qq.com/s/1z5RCYyEiHRbaERaXvgwQw)(2019-6)

[干货|最全自然语言处理attention综述](https://mp.weixin.qq.com/s/Vkvf2BIF7MVtXlyqUnAx6g)（2019-6）

[AI产品经理必备知识：8个最先进的NLP领域的预训练模型](https://mp.weixin.qq.com/s/xg4dMkSwTuV2HMcfxWpOaw)（2019-6）

[8种优秀预训练模型大盘点，NLP应用so easy！](https://mp.weixin.qq.com/s/AjMGrn39bcNvCVzSmtQvUA)(2019-5)

[从基于规则到深度学习，NLP 技术进阶三部曲](https://easyai.tech/blog/a-high-level-guide-to-natural-language-processing-techniques/)（2019-3）

[中文对比英文自然语言处理NLP的区别综述](https://mp.weixin.qq.com/s/GeQJ1subHxV7eXgFtvl7xw)（2019-3）

[百度发布NLP模型ERNIE，基于知识增强，在多个中文NLP任务中表现超越BERT](https://baijiahao.baidu.com/s?id=1628218763058589849&wfr=spider&for=pc)（2019-3）

[自然语言处理中注意力机制综述](https://mp.weixin.qq.com/s/6Qpf3amjMywet0eHKfBBlA)（2019-1）

[21种NLP任务激活函数大比拼：你一定猜不到谁赢了](https://www.jiqizhixin.com/articles/012701)

[深度好文：2018年NLP应用和商业化调查报告](https://easyai.tech/blog/2018-nlp-research/)

[5 分钟入门 Google 最强NLP模型：BERT](https://www.jianshu.com/p/d110d0c13063)

[NLP技术落地为何这么难？里面有哪些坑？](https://easyai.tech/blog/why-nlp-hard/)

[微软亚洲研究院：NLP将迎来黄金十年](https://www.jiqizhixin.com/articles/2018-11-25?from=synced&keyword=nlp)

[横扫13项中文NLP任务：香侬科技提出汉语字形表征向量Glyce+田字格CNN](https://easyai.tech/blog/xiangnong-glyce-cnn/)

[深度长文：中文分词的十年回顾](https://easyai.tech/blog/chinese-participle-10-years/)

[现有模型还“不懂”自然语言：20多位研究者谈NLP四大开放性问题](https://easyai.tech/blog/4-biggest-open-problems-in-nlp/)

[对话清华NLP实验室刘知远：NLP搞事情少不了知识库与图神经网络](https://www.jiqizhixin.com/articles/2019-02-07-6)

[中文分词十年又回顾: 2007-2017](https://arxiv.org/pdf/1901.06079v1.pdf)

实践相关文章（7）

[赛尔笔记 | 四种常见NLP框架使用总结](https://mp.weixin.qq.com/s/hcciozj47eU1lfUOSpUOjA)（2019-7）

[8种优秀预训练模型大盘点，NLP应用so easy！](https://mp.weixin.qq.com/s/8_UZeAUtBElaO9aY5iSo-g)（2019-4）

[NLP 分词的那些事儿](https://mp.weixin.qq.com/s/oF1-JCd7FvjKJvo6ipWV-w)

[自然语言处理是如何工作的？一步步教你构建 NLP 流水线](https://www.jiqizhixin.com/articles/081203)

[自然语言处理三大特征抽取器（CNN/RNN/TF）比较](https://zhuanlan.zhihu.com/p/54743941)

[Python 英文文本预处理：步骤、使用工具及示例](https://blog.csdn.net/dQCFKyQDXYm3F8rB0/article/details/86653477)

[NLP输出文本评估：使用BLEU需要承担哪些风险？](https://blog.csdn.net/dQCFKyQDXYm3F8rB0/article/details/87835014)

[初学者|NLP相关任务简介](https://mp.weixin.qq.com/s/1jSU4u2XcXColvcCt81uQQ)

相关资源（4）

[NPL学习者的福利来啦，GitHub上的NLP中文词库资源，拿走吧！](https://mp.weixin.qq.com/s/B4BxP-htuEd8CfTtYyeLSg)（2019-5）

[NLP领域最优秀的8个预训练模型（附开源地址）](https://mp.weixin.qq.com/s/Kydh-Xcw6UNUbyxq5Q9QZw)(2019-3)

[Facebook开源NLP迁移学习工具包，支持93种语言，性能最优](https://baijiahao.baidu.com/s?id=1623518621174932189)

[支持53种语言预训练模型，斯坦福发布全新NLP工具包StanfordNLP](https://www.jiqizhixin.com/articles/2019-01-31-9?from=synced&keyword=斯坦福发布全新NLP工具包StanfordNLP)