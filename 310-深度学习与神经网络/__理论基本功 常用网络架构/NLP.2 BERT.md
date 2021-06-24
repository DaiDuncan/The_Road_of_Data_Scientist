[link: 发展过程](https://zhuanlan.zhihu.com/p/67311422)

> 诞生于2018年：在NLP比赛中疯狂屠榜

# 一、[通俗理解](https://www.bilibili.com/video/BV11N41197nq/?spm_id_from=333.788.recommend_more_video.2)

背景：机器如何理解语言？

- 图像 => 像素：RGB数据



单词向量化：Word2Vec

1 为什么用向量？

- 词语之间有关联：(向量)距离可以表示词与词之间的关系

  <img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210613165215.png" alt="image-20210613165215597" style="zoom:50%;" />



2 怎么用向量表示？ => 机器学习来训练

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210613165241.png" alt="image-20210613165240177" style="zoom:50%;" />

BERT源自Transformer

- Encoder能生成语言的意义

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210613165500.png" alt="image-20210613165500372" style="zoom:50%;" />



独特的训练方式

1）有遮挡的训练：15%随机遮挡  => 更好地**依据语境**，作出预测

2）成组的句子 => 对**上下文关系**有更好的理解

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210613165638.png" alt="image-20210613165637883" style="zoom:67%;" />



应用：依据任务目标，增加不同功能的输出层，来进行联合训练

1）文本分类：BERT + 分类器

2）阅读理解：BERT + Softmax

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210613165750.png" alt="image-20210613165749514" style="zoom:50%;" />



评论：

==Bert任务本质是理解语言==，而在其他任务上如文本生成任务，Bert就无法直接去生成文本。
像常见的GPT这种利用**自回归训练**理解语言序列分布，还有生成文本的技能，如故事&摘要生成等。

