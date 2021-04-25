[link: 小孩看懂|Embedding 2019.12.04](https://mp.weixin.qq.com/s/E2un2YREG0_gmynDELBDxw)

本文受 Jay Alammar 2019年在 QCon 做的演讲启发，纯纯的致敬！

Embedding 中文是**嵌入**，一种把**对象** (object) **映射** (map) 为实数域**向量** (vector) 的技术。





**1** 例子|性格嵌入 (personality embedding) 

**场景**：斯蒂文的新工作面试到了最后一轮，拿到 offer 基本已定，只需要完成一个性格测试，从不同维度上给自己在 0 到 100 的范围打分。

![图片](https://mmbiz.qpic.cn/mmbiz_png/e4kxNicDVcCH46U8nibg28CXTlTAcssoEJKJ87K7yRHVxupicm6va2St5wZpAsrDqXs2qStiaff5g2nAAqu10pFuiaw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)





**2**

![图片](https://mmbiz.qpic.cn/mmbiz_png/e4kxNicDVcCH46U8nibg28CXTlTAcssoEJIR7w42Tuh0IWicicjjGwdAaQlU1bHx3QPuaHcfbtr9HOAdibETTsdudibQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



斯蒂文认为自己比较外向，给自己在「外向-内向」的维度上在打了 20 分。注意 0 分是极度外向，100 分是极度内向。

标准化得分使其在 -1 和 1 之间，得到的分数是 -0.4。解下列方程即可。

- 20/(100 - 0) = x/(1 - (-1))



这样斯蒂文在「外向-内向」维度由一个实数 -0.4 来表示，该维度可看成是描述斯蒂文性格的一个特征。





**3**

![图片](https://mmbiz.qpic.cn/mmbiz_png/e4kxNicDVcCH46U8nibg28CXTlTAcssoEJDoUKJ2icf3UWFcxCQHx2m4TUC4kgygPA31YSq2prU8q4YE6OLv7t68A/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

人是复杂的动物，一个特征不可能完全描述人的性格，斯蒂文在第二个特征上给自己打分为 0.8 (按同样的方法，先在 0~100 之间得分，再标准化)


现在斯蒂文的性格可以由 [-0.4, 0.8] 二维向量来表示。



**4**

![图片](https://mmbiz.qpic.cn/mmbiz_png/e4kxNicDVcCH46U8nibg28CXTlTAcssoEJCicWOa0y1hCAIMdBYcP0jtClX8gPM1klq92CAGOWAywIAYwrCw2qSQQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



如果现在斯蒂文放鸽子不去这个公司，公司想**找一个和斯蒂文性格类似的求职者**。

根据他们在前两个特征上的得分，公司应该用谁来代替斯蒂文？





**5**

![图片](https://mmbiz.qpic.cn/mmbiz_png/e4kxNicDVcCH46U8nibg28CXTlTAcssoEJqVxFicBMb2wfpAOGN5RKlnKwrhKYSNglYJtLUu0EVTtsXNa6fnXIl1g/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



公司应该选这个女生，因为比起另外那个男生，她的性格和斯蒂文的性格更接近。

这里向量**相似度**是用**余弦距离**来计算，其值范围在 -1 到 1 之间，值越大 (1) 越相似，值越小 (-1) 越相反。对数学感兴趣可以参考计算余弦距离的公式。

![图片](https://mmbiz.qpic.cn/mmbiz_png/e4kxNicDVcCH46U8nibg28CXTlTAcssoEJeezylWkibc5IxO0A0feEEpNeWlwBO8gSaaNI0cLYU9wZD087xSZwFkQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)





下图看三个人完整的性格测试得分，这种把**性格**转换成 **4 维向量**的技术可称为性格嵌入 (personality embedding)。

计算性格相似度后，还是会用这位女生来替代放鸽子的斯蒂文。

![图片](https://mmbiz.qpic.cn/mmbiz_png/e4kxNicDVcCH46U8nibg28CXTlTAcssoEJUR4ZaqQGfWhdGlIMj5fO8PUVozuwRGicomJKeXFqm11wg5Iy6lGjhIw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)





**总结**：我们可以将人和事物 (所有东西) 表示为代数向量，而计算机很容易能计算出这些向量之间的相似程度。

---

**6** **性格嵌入**讲完后，让我们来看看大名鼎鼎的**词嵌入** (word embedding)。

类比：

- **性格嵌入**是将**性格**转换成**向量**的技术
- **词嵌入**是将**词**转换成**向量**的技术



词嵌入的技术本帖当然不谈，要不小孩就看不懂了 (当然比性格嵌入的技术复杂)。先看看词嵌入的结果向量，以 King 举例。

![图片](https://mmbiz.qpic.cn/mmbiz_png/e4kxNicDVcCH46U8nibg28CXTlTAcssoEJsyia3I6IfttKyGKm87K09eAHtLQSZ26wQTjFHG6INjLVwJ5mAuibMtQQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

单词 King 由 50 个实数来表示，即用一个 50 维的向量能代表单词 King。

其他单词也可以用不同的 50 维的向量来表示 (具体维度大小根据实际问题来决定，**常用的是 300 维度**，这里用 50 维来举例)。



为了后文能快速找出不同词之间的同义，我们来可视化 50 维向量，比如蓝色代表负值 (越深越负)，红色代表正值 (越深越正)。

==**如果两个向量在很多维度上的颜色相同，那么这两个向量相似。**==



接下来让我们看看单词 King, Man 和 Woman 的向量可视化。

![图片](https://mmbiz.qpic.cn/mmbiz_png/e4kxNicDVcCH46U8nibg28CXTlTAcssoEJJcAYSkRqVQDBTE1cibTKgQtlFd6Y8VdZTbSJMSiaBHRWRPANXkfmU2WQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

不难看出，Man 和 Woman 之间相似度比起它们和 King 之间相似度更高。





**7** 接下来，我们看更多词向量可视化的例子。

人 vs 水

![图片](https://mmbiz.qpic.cn/mmbiz_png/e4kxNicDVcCH46U8nibg28CXTlTAcssoEJuKJz2j2U0NibdhKkYia8GA90cfcKR3JaZbnDO6PicTqeWL79PzmukY1XQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



从上图两点值得注意：

1. 黄色五角星对应的栏：女人、女孩、男孩、男人、国王和皇后是**深蓝色**，水是**浅蓝色**，==该栏对应的特征可能是**有无生命**。==

2. 红色五角星对应的栏：所有事物都是**深红色**，==该栏对应的特征可能是**名词**。==



女 (男) 人 vs 女 (男) 孩

![图片](https://mmbiz.qpic.cn/mmbiz_png/e4kxNicDVcCH46U8nibg28CXTlTAcssoEJMu7b9nZDel0I32R3UX6OKibcvic8fLicvnJz3ibKmcZdfHE0M3nceayqAQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

女人和女孩非常相似 (都是女性)，男人和男孩也非常相似 (都是男性)，而且两对词的相似点也很多重叠 (用黄色五角星标注了)，可能显示着「成年人和未成年人」的关系。



女孩 vs 男孩



![图片](https://mmbiz.qpic.cn/mmbiz_png/e4kxNicDVcCH46U8nibg28CXTlTAcssoEJqt3d7FrGftTPrlOyyWsATLJjctf6vLQvRmTEHPdYI9icKNgia1rIpGqg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

女孩和男孩非常相似，但是他们的相似处和女人和男人的相似处不太一样，可能就是「成年人和未成年人」的区别。



国王 vs 皇后



![图片](https://mmbiz.qpic.cn/mmbiz_png/e4kxNicDVcCH46U8nibg28CXTlTAcssoEJqSZKPhIaYicK1h4LpB2dcJGysQy2I9ApZ9UH2Vr9VOgia0puAoRcDnhA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



国王和皇后非常相似，但是它们的相似处 (用黄色五角星标注了) 在 (女人，男人)、(女孩，男孩) 这些「词对」中没有体现。该特征应该是有**皇家色彩 

![图片](https://mmbiz.qpic.cn/mmbiz_png/e4kxNicDVcCH46U8nibg28CXTlTAcssoEJg1Aib8SZzKlTfjW5zaDT45OttPcgmtfhJYicLucHvORwOcBd49rvN9ibQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



让人惊艳的东西来了，词嵌入或词向量可以发掘出**词与词的类比关系**。

我们可以在词向量上做加法和减法，最后得到一些有趣的结果。



最有名的结果是

- 国王 - 男人 + 女人 = 皇后





用 Python Gensim 库可以得到上面结果：

![图片](https://mmbiz.qpic.cn/mmbiz_png/e4kxNicDVcCE2DBoJ5kqrNVZFc4YmkX9EtErmSdnMe6zJ5mrnVhA6SfYoGNJ5oeEMbg4VXMBZa9s6WOvu7EyEjA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

皇后的概率最高，记住机器不是人，永远是**以概率的形式给出结果**。



重新调整上面公式可以得到不同表达式

1. 国王 - 男人 = 皇后 - 女人
2. 国王 - 皇后 = 男人 - 女人



1 式抽象出**皇家**的词向量，2 式抽象出**男性减去女性**的词向量。



词向量的类比关系应用还有很多，比如

- 中国 - 北京 = 法国 - 巴黎

- do - did = go - went





# **总结**


在自然语言处理 (Natural Language Processing, NLP) 中，**词嵌入**把词转换成实数向量，从 word 转到 vector，因此大家都也把词嵌入称为 word2vec，用到的技术就是神经网络。



==在推荐系统 (Recommender System, RS) 中，**物品嵌入**把推荐的物品转换成实数向量，item2vec。==



图神经网络 (Graph Neural Network, GNN) 存在很多通路，将各个节点连成一条线，这些连线蕴含着节点之间的相互关系，就如同句子中各个词语的关系一样，这样我们也可以用==**节点嵌入**的方法把节点 node 转成向量 vector，node2vec。==



**万物皆嵌入**

**everything2vec**



