[link: 小孩看懂系列|RNN 2019.09.29](https://mp.weixin.qq.com/s?__biz=MzIzMjY0MjE1MA==&mid=2247488290&idx=1&sn=427aee1a0afaf4c4f34b6d451de8bd3d&chksm=e890902bdfe7193d612a6c6516ad4c5ebae2936bf9428fa46edcfa631271fbe9a6c2be43d943&scene=21#wechat_redirect)

本文受以下两部视频所启发，但用了我最喜欢的 NBA 巨星哈登举例。

- Luis Serrano 的「A friendly introduction to RNNs」
- Brandon Rohrer 的「How RNNs and LSTM Work」



首先聊聊场景，哈登进攻有**三**招，**三分**，**扣篮**和**传球**。

![图片](https://mmbiz.qpic.cn/mmbiz_png/e4kxNicDVcCGHjLCia5GNhPfccD3pbUtbwMvdMvSSWqTqT423libkX8PibErZ6bshoGINCFy3NnNWAZVmeh1BJT6GA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



观众喝彩有**两**种：**NB** 和 **SB** (喝倒彩)。![图片](https://mmbiz.qpic.cn/mmbiz_png/e4kxNicDVcCGHjLCia5GNhPfccD3pbUtbwondZ7c8dV5QQ8ib5tiaEaJcew0CBQibAsMzDqjz3pFeqfSicIiaf0ctNdCA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



如下图所示，用向量来表示**三种招式**和两种喝彩。

![图片](https://mmbiz.qpic.cn/mmbiz_png/e4kxNicDVcCGHjLCia5GNhPfccD3pbUtbw1vdtbGMlB5ZlhOGOsjDuIiclwbOiadGL1WOBicrZ4ibyegLgOr56wnmYMA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



再往下继续看时，希望读者脑子里就记着以下对应关系：

- 三分：[1 0 0]
- 扣篮：[0 1 0]
- 传球：[0 0 1]
- NB：[1 0]
- SB：[0 1]





**2** 先看看一个最简单的例子：

1. 当观众喊 NB 时，哈登就拔三分
2. 当观众喊 SB 时，哈登就扣篮泄愤

![图片](https://mmbiz.qpic.cn/mmbiz_png/e4kxNicDVcCGHjLCia5GNhPfccD3pbUtbwjVdX6eVgiaOfbAhbPibv7TpnuBfKdichSibXhPIgiaZ5LBdSKtFkgJ6ehZQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



这样我们用一个神经网络来讲「观众的喝彩」和「哈登的招式」连接起来。

- NB → 三分
- SB → 扣篮



神经网络其实就是一堆**参数**，我们用矩阵来表示这些参数好不好？

具体公式见下图，大家来用矩阵乘以向量来验证一下上面两组联系。

![图片](https://mmbiz.qpic.cn/mmbiz_png/e4kxNicDVcCGHjLCia5GNhPfccD3pbUtbwiayfX4dib7FFhosfic9d7HQd55IB6PbxO4QRpHoxAIJCRIWl0xSSga7ibg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



为了进一步讲明「神经网络就是一堆参数」，我们可视化下神经网络，如下图。

关键记住：

- 矩阵的行代表第一层神经元，一行有 2 个元素，第一层有 2 个神经元
- 矩阵的列代表第二层神经元，一列有 3 个元素，第二层有 3 个神经元

![图片](https://mmbiz.qpic.cn/mmbiz_png/e4kxNicDVcCEFcV2ZX6OVbjXkwf1gbTzibIOibiciaF0XOqiad2fMTTHQqnL1r7OLxLCC0R0sniajAiazhkh1VJ7DWTJjQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



下面两图完整的可视化两种喝彩和招式的联系。

- NB → 三分
- SB → 扣篮

![图片](https://mmbiz.qpic.cn/mmbiz_png/e4kxNicDVcCGHjLCia5GNhPfccD3pbUtbw109lkNoxeJAkxM2mD2IOordPEUd4GyF00pCxE1OwTWwW8t1Z2FNeRQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/e4kxNicDVcCGHjLCia5GNhPfccD3pbUtbwLNS6XibPemjZiaMQ5MH4GqpLV112GEjayboh1duM7ib3W6ldaFbbFib2fw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



本小节介绍的就是全连接神经网络 (Fully Connected Neural Network, FCNN) 的极简版本。





**3** 但哈登**发招是连续的**，下图就是一系列发招顺序。



  三分 → 扣篮 → 传球 → 三分 → 扣篮 → 传球 ...

![图片](https://mmbiz.qpic.cn/mmbiz_png/e4kxNicDVcCEFcV2ZX6OVbjXkwf1gbTzibrWY955y5G4HHGmDB4uv8ic32nCLK7y6GicCdN4UNQhXica9lDFUDsAt1A/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



这些招式都是一个接一个哦，可以用循环神经网络连接它们呢。

![图片](https://mmbiz.qpic.cn/mmbiz_png/e4kxNicDVcCEFcV2ZX6OVbjXkwf1gbTzib9jAWUDztm9ZP9kVJ51CQ3oCsqUs1o1ibwCFbDjBZmyliba4B86TjL9og/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



还记得神经网络其实就是一堆**参数**吗？

那**循环神经网络**也就是另一堆参数嘛，就是下面这个矩阵。

![图片](https://mmbiz.qpic.cn/mmbiz_png/e4kxNicDVcCEFcV2ZX6OVbjXkwf1gbTzibTbBicp7YMwA2Vs4U2Xia1ul2cVUtZf3uCWMlMjN0YuFp3S9Hm9FZq4jQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



我们来可视化循环神经网络，注意我们故意**不画出权重为 0 的箭头** (太多看起来乱)，循环神经网络和普通神经网络最大的区别就是下面那条**回形线**。

本次输出可作为下次输入，就这么简单。

![图片](https://mmbiz.qpic.cn/mmbiz_png/e4kxNicDVcCEFcV2ZX6OVbjXkwf1gbTzibVicF4msl2uptb3oIuaqelxDpiciabmCfZNHufyOZtib4Kox9ZTv8AqsXGQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

好了，读者来验证下，用循环神经网络这个矩阵来乘以不同招式的向量是不是可以得到下一个招式的向量？



![图片](https://mmbiz.qpic.cn/mmbiz_png/e4kxNicDVcCEFcV2ZX6OVbjXkwf1gbTzibLOnQpxCH6dG7oXukTZ9GwTicjaeWUjHl65j9kjPG7pxtmePw4aeHvicw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



本小节介绍的是循环神经网络 (Recurrent Neural Network, RNN) 的**极简版本**，同样也是不现实的版本。





**4** 哈登是个人，不会像机器那样一直不停的「三分 → 扣篮 → 传球」机械发招。

哈登是有情绪的，他会根据观众喝彩来决定如何出招的：

观众喊 **NB**，那么哈登觉得自己不会改变打法，上招出什么下招还是出什么，好用呗。

![图片](https://mmbiz.qpic.cn/mmbiz_png/e4kxNicDVcCEFcV2ZX6OVbjXkwf1gbTzibBib5aFV0icEqyicAGgia6GoSTwRlUAj47QKZGgDBNm1MaWQzQJ8S0zlToQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



观众喊 **SB**，那么哈登就会陷入自我怀疑，是不是上招打法不合理，那么他会这样调整：

- 上招是**三分**，那么下招是**扣篮**
- 上招是**扣篮**，那么下招是**传球**
- 上招是**传球**，那么下招是**三分**

![图片](https://mmbiz.qpic.cn/mmbiz_png/e4kxNicDVcCEFcV2ZX6OVbjXkwf1gbTzibJpyoFiaw9zXpGdcSiaicDalibmJCB6PliakwntInGPFLmposKEOMOK89mTA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



所以带情绪的出招序列如下。

![图片](https://mmbiz.qpic.cn/mmbiz_png/e4kxNicDVcCEFcV2ZX6OVbjXkwf1gbTzib4RjbhVEWOGKKJWQfiaM9E5aicwyIST3Uib5TFXJntuxvibgZ7iaTTO26XPQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

---

现在我们来分析两个重要矩阵：**招式矩阵**和**喝彩矩阵**。

先看招式矩阵，它的**输入是本轮招式**，输出是「本轮招式 + 下轮招式」

![图片](https://mmbiz.qpic.cn/mmbiz_png/e4kxNicDVcCEFcV2ZX6OVbjXkwf1gbTzib8L7NRfutodLEDSGRTg2smoibctPMVobVkkn6kVLsM0PlE3UiaNV6IfUQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

再看喝彩矩阵，它就把观众的喝彩声拉长了些。

为什么要这么做呢？只是为了要把「输出向量的大小」和「招式矩阵矩阵的输出向量的大小」设计的一样，这样才能做之后的合并操作。

![图片](https://mmbiz.qpic.cn/mmbiz_png/e4kxNicDVcCEFcV2ZX6OVbjXkwf1gbTzibSZcSfWibPwwNS9Dtk04YSQiaicFCt5ibkPIFicwwY9YhqxVFDDSibgr0e5vQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



骚操作来了，让我们把两个矩阵水平合并。

![图片](https://mmbiz.qpic.cn/mmbiz_png/e4kxNicDVcCEFcV2ZX6OVbjXkwf1gbTzib9GsicXqaq3RgSkTiaG5H4Ernx4gu9GHKicVVEcDtbfdw1GpPvxfnrxNfg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



再乘以以 [招式 喝彩] 为格式(矩阵合并)的向量，比如

- [三分 SB] = [1 0 0 0 1]
- [扣篮 SB] = [0 1 0 0 1]
- [传球 NB] = [0 0 1 1 0]

等等。



以[三分 SB]输入为例：得到的向量是 [1 0 0 1 2 1]，看起来难以解释，但是我们注意

- 2 是里面最大的元素，是真正的信息，我们希望保留它
- 0 和 1 都是噪声，我们希望剔除它们



那么可以试一下非线性函数哦，比如 RELU？

![图片](https://mmbiz.qpic.cn/mmbiz_png/e4kxNicDVcCEFcV2ZX6OVbjXkwf1gbTzibASvr44Dv9riaibPjyDpSyuqcWeyPMo4TCzicMWC8LAk3V57ozibF29icJfw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



深度学习不是总用各种转换函数吗？这里用到的函数将**该向量中最大值变成 1，其余值变成 0**，得到 [0 0 0 0 1 0]。



最后再把 [0 0 0 0 1 0] **拆成两个小向量再加一起**，不就有点类似池化 (pooling) 吗？

- 但用的不是平均池化，不是最大池化，而是**加总池化**。



最后得到的输出是 [0 1 0]，这向量不就是我们的老朋友**扣篮**么？

![图片](https://mmbiz.qpic.cn/mmbiz_png/e4kxNicDVcCEFcV2ZX6OVbjXkwf1gbTzib4J2CLV7vodzniblAoLZicw0SaW5bmGic7fhpl5QXQuL38onckOhDs5ia9g/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



现在开始看一套完整的循环神经网络流程图，从输入开始。我们输入是**三分** + **SB**。

- 注意：合并矩阵
- 招式[1 0 0]对应合并矩阵中的第1和第5行(对应相乘元素为1)
- 喝彩[0 1]
  - 第一个元素对应合并矩阵中的前3行
  - 第二个元素对应合并矩阵中的后3行
- 非线性转换 => 转换为0、1
- 合并：折叠为[3元向量] => 最终招式

![图片](https://mmbiz.qpic.cn/mmbiz_png/e4kxNicDVcCEFcV2ZX6OVbjXkwf1gbTzib9LkGHY4z2mcq63ygMwAyYYufWJj6PYWUR3rWABcsyUmibxKSQLGJCPQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



**三分**和 **SB** 乘上对应的招式矩阵和喝彩矩阵得到两个 6 × 1 的向量，向量意思上面已经解释过。

- 将这两个向量相加。

- 再对加总的向量做非线性转化，去掉噪声只留下最显著的信息。

- 再把这个 6 × 1 的向量分拆成两个 3 × 1 的向量并加总，最后得到 [0 1 0]，那就是扣篮。

因此**哈登投完三分后，如果观众喊 SB，那么哈登下一动作就是扣篮**。

![图片](https://mmbiz.qpic.cn/mmbiz_png/e4kxNicDVcCEFcV2ZX6OVbjXkwf1gbTzibwQVicvsdDECXZ3MUe6Nn0PDwKbaMs7bNITupuLaL2mzic1E8Ym4GD6EA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



最后画出回形线使得该网络变成循环神经网络，这样哈登就可以一直==根据本轮的出招和观众的喝彩==来决定下轮的出招了。

![图片](https://mmbiz.qpic.cn/mmbiz_png/e4kxNicDVcCEFcV2ZX6OVbjXkwf1gbTzibYVc2xnNWF9DyxQcm8I2RHO8FDfYFNqf74TJ8nVhibqEamqrLvlg3HzQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)