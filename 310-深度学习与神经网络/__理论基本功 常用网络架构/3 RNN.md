Recurrent Neural Networks

# 零、[通俗理解](https://www.bilibili.com/video/BV1Zi4y1L7LL/?spm_id_from=333.788.recommend_more_video.3)

背景：图像识别是孤立的

语言：序列数据

- “我吃苹果”：“吃”后面大概率是“食物”



RNN VS. ANN

- 多了一个“小盒子”：用来记录数据输入时，网络的状态 => 隐状态
  - 下一次输入时：网络必须要考虑小盒子中保存的信息



NLP：

- 机器翻译
- 文本生成：基于一个主题，按照一定规则，输出有逻辑的词语序列
- 改变两端数据类型
  - 看图说话：输入图片，输出句子
- 语音识别 & 语音生成
  - 语音：声音信号按时间顺序组成的序列

股票价格：受事件影响的序列 => 量化交易模型

缺陷：缺少长期记忆

![image-20210615095611701](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210615095612.png)



# [一、通俗理解|Udacity](https://www.youtube.com/watch?v=UNmqTiOnRfg&list=RDCMUCgBncpylJ1kiVaPyP-PZauQ&start_radio=1)

## 1 情境引导

1 厨师：

- input|晴天 => output|做apple pie
- 雨天：做hamburger

编码：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210611120030.png" alt="image-20210611120029449" style="zoom: 50%;" />



  神经网络 => 矩阵的线性变换

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210611135647.png" alt="image-20210611135647245" style="zoom: 50%;" />





2 厨师变式：序列化

- apple pie => burger => chicken

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210611135907.png" alt="image-20210611135907107" style="zoom: 50%;" />

`1 0 0 => 0 1 0`

`0 1 0 => 0 0 1`

`0 0 1 => 1 0 0`

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210611140007.png" alt="image-20210611140006923" style="zoom:50%;" />



3 两者结合：

- 天气好，不改变
- 天气不好，按照序列走

![image-20210611140125906](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210611140126.png)



NN结构示意图

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210611140158.png" alt="image-20210611140158287" style="zoom:50%;" />



## 2 数学描述

1 两部分组成

- Seq|食物
  - <img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210611140300.png" alt="image-20210611140300626" style="zoom: 50%;" />

- Input|天气
  - <img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210611140334.png" alt="image-20210611140334121" style="zoom:50%;" />
  - <img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210611140411.png" alt="image-20210611140411326" style="zoom:50%;" />





合并两者：

- 非线性映射



Add操作：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210611140445.png" alt="image-20210611140445020" style="zoom:50%;" />



<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210611140534.png" alt="image-20210611140534584" style="zoom:50%;" />



Merge操作：

示意：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210611140616.png" alt="image-20210611140616024" style="zoom:50%;" />

基于矩阵：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210611140641.png" alt="image-20210611140640864" style="zoom:50%;" />



2 RNN网络|单个输出

1）Food权重矩阵，Weather权重矩阵

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210611140818.png" alt="image-20210611140818131" style="zoom:50%;" />

2）Add + 非线性映射

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210611140913.png" alt="image-20210611140913385" style="zoom:50%;" />

3）Merge

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210611140933.png" alt="image-20210611140933416" style="zoom:50%;" />



3 RNN|这次的输出作为下次的输入

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210611141018.png" alt="image-20210611141018231" style="zoom:50%;" />



### 如何训练网络？💡

梯度下降

0 训练困难：

- 每一个time step就是一层网络
- 100个time step就相当于100层网络

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210611142241.png" alt="image-20210611142241179" style="zoom:50%;" />

结果：随着time step，信息衰减



1 技术|Gating units => LSTM, GRU

- 决定什么时候忘记某些信息

​        

其他的技术：

- Gradient clipping
- better optimizers
- steeper Gates



2 GPU

快了250倍：1天 VS. 8个月





## 3 应用

印象：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210611141545.png" alt="image-20210611141545521" style="zoom:50%;" />

1 一 => 多|image captioning图像说明：输入图像 => 输出描述该图像的文本

- input: 单个值
- output: 序列

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210611141722.png" alt="image-20210611141721920" style="zoom:50%;" />

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210612143728.png" alt="image-20210612143727854" style="zoom:80%;" />



2 多 => 一|document classification文本分类

- input: 序列
- output: 单个值

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210611141810.png" alt="image-20210611141809674" style="zoom:50%;" />



3 多 => 多|videos classify(frame by frame)

1）序列等长

用途比较狭窄：比如诗词文 => 作诗机器人

视频逐帧理解

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210611141853.png" alt="image-20210611141852389" style="zoom:50%;" />

@Auto-Encoder：输入与输出等长



2）输入、输出不等长(Seq2Seq)

RNN + Auto-Encoder构造翻译机器人：

- 输入等于输出
- 只不过：输入、输出用不同的语言来表示

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210612144509.png" alt="image-20210612144508833"  />

结构形式1|中间的隐藏层：$h_3$输出给h5-h8, $h_4$输入给h5

![image-20210612144831939](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210612144832.png)



4 注意力机制下的Seq2Seq

上述Seq2Seq的问题：Decoder的输入，都是Encoder的同一个输出 => 也就是不管输入的语句是什么，Encoder都会转换成同一个中间语义$h'$

改进：注意力机制 => 一句话是有重点的

- 输入不是直接的序列输入，而是经过Encoder之后转换的中间语义C
  - 不同输入位置的C各不相同
  - 每一个C都是由权重W和Encoder的隐藏层输出h(上图中隐藏层的输出)的加权组成

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210612145253.png" alt="image-20210612145253450" style="zoom:80%;" />

核心：基于注意力机制，找到一句话(直接的序列)的**重点** => 经过Encoder输出的$C_i$

中间语义转换过程|加权：

- $C_i$是Decoder的输入

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210612212742.png" alt="image-20210612212742010" style="zoom:80%;" />

- Encoder的输出与$h_i$(注意力机制关注的重点)越相近，那么输出的概率$w_i$越大

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210612213013.png" alt="image-20210612213013693" style="zoom:80%;" />





5 需求预测|供应链规划

- 含time delay

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210611141950.png" alt="image-20210611141949839" style="zoom:50%;" />





股票预测

序列生成

文本生成

语音识别



## 4 拓展

1 RNN网络堆叠

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210611142114.png" alt="image-20210611142114004" style="zoom:50%;" />



---



# 二、[算法工程师|RNN](https://www.bilibili.com/video/BV1o4411R7B1?p=1)

1 RNN与DNN(全连接)的区别

- RNN多了参数$h_0$(隐藏状态：比如单词序列)



2 以多输入、单输出的RNN结构为例

- 整个RNN结构共享一组权重(U, W, b) => 指的是隐藏层、输出层的参数都相同
  - 这也是梯度消失、梯度爆炸的来源：W是一样的

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210612143138.png" alt="image-20210612143138037" style="zoom:80%;" />

3 RNN结构的优势：序列数据 => 序列中就包含隐藏的信息

- 输入是多个，且有序的
  - 模拟人类阅读的顺序：去读取文本或者别的序列化数据
- 通过隐藏层神经元的编码：上一个隐藏层的信息可以传递到下一个隐藏层 
  - ==形成一定的记忆功能==：更好地理解序列化数据

 



# me|总结

0 网络结构的本质(联系实际情况)

- 序列本身有联系
- 基于输入：比如语音
  - 基于这一个区间的语音$x_t$和上一个序列的单词输出$h_{t-1}$来判断当前的输出$h_t$
    - 也就是说语音是输入，单词是输出





# #参考文献

[link: RNN应用](https://www.youtube.com/watch?v=_aCuOwF1ZjU)



