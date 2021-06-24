[语音识别技术 – ASR丨Automatic Speech Recognition](https://easyai.tech/ai-definition/asr/)

> 语音识别是什么？他有什么价值，以及他的技术原理是什么？本文将解答大家对语音识别的常见疑问。

## 语音识别技术（ASR）是什么？

机器要与人实现对话，那就需要实现三步：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210501204446.png" alt="机器要与人对话，需要实现3步" style="zoom:50%;" />

对应的便是“耳”、“脑”、“口”的工作，机器要听懂人类说话，就离不开语音识别技术（ASR）。

![语音识别的使用场景](https://easy-ai.oss-cn-shanghai.aliyuncs.com/2019-04-12-yingyong.png)

语音识别已经成为了一种很常见的技术，大家在日常生活中经常会用到：

- 苹果的用户肯定都体验过 Siri ，就是典型的语音识别
- 微信里有一个功能是==”文字语音转文字”，也利用了语音识别==
- 最近流行的智能音箱就是以语音识别为核心的产品
- 比较新款的汽车基本都有语音控制的功能，这也是语音识别

 

## 语音识别技术讲解

语音识别技术拆分下来，主要可分为“输入——编码——解码——输出 ”4个流程。

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210501204506.png" alt="语音识别4个流程：输入-编码-解码-输出" style="zoom:50%;" />

**那语音识别是怎么工作的呢？**

首先声音的本身是一种波，就像我们常常用一段段波形来表示音频一样。 ![我们常用波段来表示音频](https://easy-ai.oss-cn-shanghai.aliyuncs.com/2019-04-12-boduan.png)



接下来按步骤：

1. 给音频进行信号处理后，便要**按帧（毫秒级）拆分**，并对拆分出的**小段波形按照人耳特征**变成==多维[向量](https://easyai.tech/ai-definition/vector/)信息==
2. 将这些帧信息**识别成状态**（可以理解为中间过程，一种比[音素](https://easyai.tech/ai-definition/phone/)还要小的过程）
3. 再将**状态组合形成音素**（通常3个状态=1个音素）
4. 最后将**音素组成字词**（dà jiā hǎo）并**串连成句** 。

于是，这就可以实现由语音转换成文字了。![将音素组成字词](https://easy-ai.oss-cn-shanghai.aliyuncs.com/2019-04-12-zuhe.png)

 

## 百度百科和维基百科

百度百科版本

语音识别技术，也被称为自动语音识别 Automatic Speech Recognition，(ASR)，其目标是将人类的语音中的词汇内容转换为计算机可读的输入，例如按键、二进制编码或者字符序列。

与说话人识别及说话人确认不同，后者尝试识别或确认发出语音的说话人而非其中所包含的词汇内容。

[查看详情](https://baike.baidu.com/item/语音识别技术/5732447)



维基百科版本

语音识别是计算语言学的跨学科子领域，其开发方法和技术，使得能够通过计算机识别和翻译口语。

它也被称为自动语音识别（ASR），计算机语音识别或语音到文本（STT）。

它融合了语言学，计算机科学和电气工程领域的知识和研究。

一些语音识别系统需要“训练”（也称为“登记”），其中个体说话者将文本或孤立的词汇读入系统。

系统分析人的特定声音并使用它来微调对该人的语音的识别，从而提高准确性。

不使用训练的系统称为“说话者无关” 系统。使用训练的系统称为“说话者依赖”。

[查看详情](https://en.wikipedia.org/wiki/Speech_recognition)

 

## 扩展阅读

入门类文章（3）

[语音识别的技术原理是什么？](https://www.zhihu.com/question/20398418/answer/18080841)

[语音识别如何处理汉字中的“同音字 ”现象？](https://www.zhihu.com/question/59050053/answer/166979858)

[CUI三部曲之语音识别——机器如何听懂你的话？](https://mp.weixin.qq.com/s?__biz=MzI1NDY4NzUxNg%3D%3D&mid=2247483768&idx=1&sn=33777e5032567698f2b72930516704b5&scene=45#wechat_redirect)



相关资源（1）

[绝佳的ASR学习方案：这是一套开源的中文语音识别系统](https://www.jiqizhixin.com/articles/021102)