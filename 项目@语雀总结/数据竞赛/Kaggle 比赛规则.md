[link: Kaggle竞赛宝典|数据竞赛规则](https://mp.weixin.qq.com/s?__biz=Mzk0NDE5Nzg1Ng==&mid=2247492527&idx=1&sn=c57b270396a578d8507c9ca66765ef6e&chksm=c32afa20f45d7336fdd15d83f991b79a7f9406708224e85a89dc0efbd5616e58037d04f3a2eb&scene=21#wechat_redirect)



# 赛制了解

进入一个竞赛，很多新手朋友会直接蒙头下载数据然后直接跑模型，这是很不错的，但是如果希望最终能拿到更好的效果，还不够。

至少需要做了解下面几件事情。

1 提交次数了解：很多竞赛会随着测试数据或者官方讨论的不同，提交次数会有所限制，有的竞赛**每日提交次数有五次**，但也有一些竞赛每日的提交次数是两次等，还有更加直接的，整个竞赛期间，选手一共只有两次提交机会等等。

1. 例如，在Riiid Answer Correctness Prediction竞赛中，选手每日都可以提交五次；

2. ![图片](https://mmbiz.qpic.cn/mmbiz_jpg/ZQhHsg2x8fibrf66TMsHEngWyMqMibicnTSBvw2PmgZibWU17Sa5eJWULRzCBzlADLaJ2STvXufBricLX0RibKPqknOw/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

3. - 例如，在15.071x - The Analytics Edge (Summer 2015)竞赛中，选手每日只能提交2次；

4. 

5. ![图片](https://mmbiz.qpic.cn/mmbiz_jpg/ZQhHsg2x8fibrf66TMsHEngWyMqMibicnTSDxwS5dtL3mrdibYW7Llvlfr6DsJGsEQ7dySnZjEtkAJx2SibweoicLHpw/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

2 平台稳定性调研：平台稳定性也是非常重要的，在kaggle竞赛平台目前还好，但是很多新的数据竞赛平台，例如早期在Colab上举办的会议赛就经常会出现在某个时间段内选手无法提交的情况，而因为**会议往往是不会延期举办的**，所以这些没法提交的次数也不会补回来，有的时候就直接草草结束，也有的时候会在后面时间段增加每日提交次数，所以如果是不稳定的平台，最好是尽早加入竞赛，防止出现上面的情况。



3 是否可以组队，组队截止日期以及组队与提交次数是否关联：

1. - 是否可以组队：

2. - 组队截止日期了解：在很多数据竞赛平台上，平台都会给一个组队截止日期，很多数据竞赛都经常会看到下面这种情况：
     - 比赛组队已经截止，很多选手还在问为什么没法组队，能不能给次机会之类的问题。当然，很多时候这是不被允许的，所以最好的方式就是在竞赛截止前12h就尽早组队，防止出现差错；

3. ![图片](https://mmbiz.qpic.cn/mmbiz_jpg/ZQhHsg2x8fibrf66TMsHEngWyMqMibicnTShtmtiagT3ymAYuydAHKmicQYjBViaAYicxSRvKuiaib2dZeFBibasNuTDPBAA/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

4. - 组队与提交次数是否关联：在kaggle最近两年的竞赛中，组队除了有组队时间的限制要求，还对**组队的团队的提交次数有限制**，这在许多国内的竞赛中也慢慢出现了类似的规则，是非常需要注意的一点。

5. ![图片](https://mmbiz.qpic.cn/mmbiz_jpg/ZQhHsg2x8fibrf66TMsHEngWyMqMibicnTSCLic8hg6YCtDSQtu2DMyoeDqjMX81JzIiaiaNdXGxWAXsvyE8rZGLxSYQ/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

4 是否可以使用外部数据：关于外部数据的使用，目前一般在kaggle的discussion区都有描述，而能不能使用外部数据在kaggle还是查的很严的，早起kaggle的图像生成比赛很多选手**使用了外部数据**从而被移除排名；也有很多竞赛，很多选手因为**懒于或者忘记收集外部数据**，而最终没能取与金牌失之交臂；所以外部数据的使用一定要研究好。

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/ZQhHsg2x8fibrf66TMsHEngWyMqMibicnTSTqhRo2vhc5RsrQxiaa7aSrLAfQbfibr7p2u0p9ycvwDF1JaZAe6NibGrA/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

5 是否可以对数据进行**过拟合**：之前很多竞赛都是训练集合和测试集合都给出的，这就带来一个问题，尤其是**时间序列相关的问题**，例如视频浏览预测之类的问题，选手可以通过同一个用户id浏览当前视频和下个视频的时间差来判断用户是否看完了上一个视频，这就带来了非常大的leak，这些leak能带来排行榜的提升，但是对实际业务并不能带来任何帮助，所以后来很多竞赛，包括2020年的pakdd竞赛都对leak信息的使用做了明确的规定。

6 AB榜的情况：关于AB榜单的情况，在kaggle重点需要关注的可能就是**AB榜单测试集数据的比例**，如果Public的榜单所使用测试数据较少，那么我们可能就需要更多的相信线下的CV结果，而如果Public的榜单所使用测试数据较多，有些时候我们可以直接相信Public的成绩。

但是也有一些平台经常会出现只有A榜的问题，那这个时候基本上能过拟合就过拟合了。



# 时间安排

因为每个数据竞赛都是一个数据相关的项目，所以还是要做好时间安排的。

因为每个人都不太一样，但一般下面几个点都是要准备好的。

- **模型融合的时间**：前期的模型融合后面会随着特征工程或者模型结构的改进**而毫无意义**，所以一般不太建议在比赛前期就进行模型融合，而是建议留到最后一周进行；
- **组队的时间**：建议一般在竞赛结束前7-10天就开始寻找队友，过早的组队可能并不能带来非常大的帮助，除非是非常好的，确定一起好好做的朋友；
- **其它**。



# 资料收集

在做所有数据竞赛，尤其是新手，我们都非常建议先收集资料，而资料这块主要包括三块：

1. 历史类似竞赛案例的==方案整理==；
2. 相关的**论文资料**；
3. 相关的**博客，知乎等资料**。