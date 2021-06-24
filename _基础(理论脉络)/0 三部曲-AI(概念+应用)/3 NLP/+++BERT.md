[BERT | Bidirectional Encoder Representation from Transformers](https://easyai.tech/ai-definition/bert/)

## 什么是 BERT？

BERT的全称是Bidirectional [Encoder](https://easyai.tech/ai-definition/encoder-decoder-seq2seq/) Representation from Transformers，即双向[Transformer](https://easyai.tech/ai-definition/transformer/)的Encoder，因为[decoder](https://easyai.tech/ai-definition/encoder-decoder-seq2seq/)是==不能获要预测的信息==。

模型的主要创新点都在[pre-train](https://easyai.tech/ai-definition/pre-train/)方法上，即用了Masked LM和Next Sentence Prediction两种方法分别捕捉词语和句子级别的**representation**。



从现在的大趋势来看，使用某种模型**预训练**一个语言模型看起来是一种比较靠谱的方法。

从之前AI2的 ELMo，到 OpenAI的fine-tune transformer，再到Google的这个BERT，全都是**对预训练的语言模型的应用**。



BERT这个模型与其它两个不同的是

1. 它在训练双向语言模型时以减小的概率把少量的词**替成了Mask或者另一个随机的词**。
   - 我个人感觉这个目的在于使模型被迫增加对上下文的记忆。
   - 至于这个概率，我猜是Jacob拍脑袋随便设的。
2. 增加了一个预测下一句的loss。这个看起来就比较新奇了。



BERT模型具有以下两个特点：

1. 是这个模型非常的深，12层，并不宽(wide），中间层只有1024，而之前的Transformer模型中间层有2048。
   - 这似乎又印证了计算机图像处理的一个观点——**深而窄** 比 浅而宽 的模型更好。
2. MLM（Masked Language Model），同时利用左侧和右侧的词语，这个在ELMo上已经出现了，绝对不是原创。
   - 其次，对于Mask（遮挡）在语言模型上的应用，已经被Ziang Xie提出了（我很有幸的也参与到了这篇论文中）：[1703.02573] Data Noising as Smoothing in Neural Network Language Models。
   - 这也是篇巨星云集的论文：Sida Wang，Jiwei Li（香侬科技的创始人兼CEO兼史上发文最多的NLP学者），Andrew Ng，Dan Jurafsky都是Coauthor。
   - 但很可惜的是他们没有关注到这篇论文。用这篇论文的方法去做Masking，相信BRET的能力说不定还会有提升。

内容来自：[【NLP】Google BERT详解](https://zhuanlan.zhihu.com/p/46652512) |[ [NLP自然语言处理\]谷歌BERT模型深度解析](https://blog.csdn.net/qq_39521554/article/details/83062188)

 

## 扩展阅读

入门类文章（3）

[ 深入浅出解析BERT原理及其表征的内容](https://mp.weixin.qq.com/s/9eAJMbdep0s1I4upxGzw1Q)（2019-8）

[NLP新秀 : BERT的优雅解读](https://www.jiqizhixin.com/articles/2019-02-18-12)（2019-2-18）

[【NLP】Google BERT详解](https://zhuanlan.zhihu.com/p/46652512)

[[NLP自然语言处理\]谷歌BERT模型深度解析](https://blog.csdn.net/qq_39521554/article/details/83062188)



扩展视野类文章（16）

[BERT王者归来！Facebook推出RoBERTa新模型，碾压XLNet 制霸三大排行榜](https://mp.weixin.qq.com/s/Pyjmjn5QSaMf8cftB7KGcg)（2019-7）

[Bert 改进： 如何融入知识](https://mp.weixin.qq.com/s/3b_2VbehBOYsnKCBSEio4w)（2019-7）

[详解BERT阅读理解](https://mp.weixin.qq.com/s/C5B4QO0fxWtBwA5iv3XzcA)(2019-7)

[XLNet:运行机制及和Bert的异同比较](https://zhuanlan.zhihu.com/p/70257427)（2019-6）

[站在BERT肩膀上的NLP新秀们（PART III）](https://mp.weixin.qq.com/s/yXcpgr6oxowOYdQr9UhW0A)（2019-6）

[站在BERT肩膀上的NLP新秀们（PART II）](https://mp.weixin.qq.com/s/RIm0S6Psm5cDA4yqDjP49A)（2019-6）

[站在BERT肩膀上的NLP新秀们（PART I）](https://mp.weixin.qq.com/s?__biz=MjM5ODkzMzMwMQ==&mid=2650409953&idx=1&sn=0674615cce719c17205b39b1ee867647&chksm=becd8dbb89ba04adf3c217658b8a4a894210eb9fbffea92f8c89e858e690250e26558648cca2&scene=21#wechat_redirect)（2019-6）

[BERT模型在NLP中目前取得如此好的效果，那下一步NLP该何去何从？](https://mp.weixin.qq.com/s/46sSKhhpJiQbSPu4VTjRxg)（2019-6）

[Bert时代的创新：Bert应用模式比较及其它](https://zhuanlan.zhihu.com/p/65470719)（2019-5）

[进一步改进GPT和BERT：使用Transformer的语言模型](https://zhuanlan.zhihu.com/p/64448382)(2019-5)

[76分钟训练BERT！谷歌大脑新型优化器LAMB加速大批量训练](https://www.jiqizhixin.com/articles/2019-04-03-7)（2019-4-3）

[知乎-如何评价 BERT 模型？](https://www.zhihu.com/question/298203515)

[从Word Embedding到Bert模型—自然语言处理中的预训练技术发展史](https://zhuanlan.zhihu.com/p/49271699)

[BERT 论文](https://arxiv.org/abs/1810.04805)

[深度长文：NLP的巨人肩膀（上）](https://blog.csdn.net/c9Yv2cf9I06K2A9E/article/details/84949040)

[NLP 的巨人肩膀（下）：从 CoVe 到 BERT](https://blog.csdn.net/c9Yv2cf9I06K2A9E/article/details/85085915)



实践类文章（15）

[美团BERT的探索和实践](https://mp.weixin.qq.com/s/qfluRDWfL40E5Lrp5BdhFw)(2019-11)

[加速 BERT 模型有多少种方法？从架构优化、模型压缩到模型蒸馏最新进展详解！](https://mp.weixin.qq.com/s/EarJXAvsvfMIQmkmLEqcSw)（2019-10）

[BERT, RoBERTa, DistilBERT, XLNet的用法对比](https://mp.weixin.qq.com/s/m4_Ev8hjtdexryhMjIPiEQ)（2019-9）

[一大批中文（BERT等）预训练模型等你认领！](https://mp.weixin.qq.com/s/zbkSw6VwmmbTQPSyRPlhEQ)（2019-6）

[【GitHub】BERT模型从训练到部署全流程](https://mp.weixin.qq.com/s/fB69RYOvo-NKjt_DBegG0w)（2019-6）

[Bert时代的创新：Bert在NLP各领域的应用进展](https://mp.weixin.qq.com/s/yPq1cGnhcbaNLOjadj91pw)（2019-6）

[BERT fintune 的艺术](https://mp.weixin.qq.com/s/dG4WEZw3UomUYUYJ4ZykqA)（2019-5）

[中文语料的 Bert finetune](https://zhuanlan.zhihu.com/p/57474455)(2019-5)

[BERT源码分析PART III](https://blog.csdn.net/Kaiyuan_sjtu/article/details/90298807)（2019-5）

[BERT源码分析PART II](https://blog.csdn.net/Kaiyuan_sjtu/article/details/90288178)（2019-5）

[BERT源码分析PART I（2019-5）](https://blog.csdn.net/Kaiyuan_sjtu/article/details/90265473)

[【干货】BERT模型的标准调优和花式调优](https://mp.weixin.qq.com/s/nVM2Kxc_Mn7BAC6-Pig2Uw)

[BERT fine-tune 终极实践教程](https://www.jianshu.com/p/aa2eff7ec5c1)

[详解谷歌最强NLP模型BERT（理论+实战）](https://blog.csdn.net/dQCFKyQDXYm3F8rB0/article/details/86570417)

[用BRET进行多标签文本分类（附代码）](https://easyai.tech/blog/multi-label-text-classification-using-bert-the-mighty-transformer/)