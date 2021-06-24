Gated Recurrent Neural Network 门控循环单元

# 一、[通俗讲解GRU](https://www.bilibili.com/video/BV1o4411R7B1?p=3)

背景：LSTM门控网络结构过于复杂与冗余

- 遗忘门forgot ($1-z_t$) & 输入门input ($z_t$) => 更新门$z_t$
- 记忆单元C & 隐藏层h => 重置门reset 

![img](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210612232013.png)

每个隐藏单元都有独立的重置门、更新门

- z要记住
- r是隐藏状态h的备份

过程1 当重置门接近0($r_t=0$)时，隐藏状态被迫忽略先前的隐藏状态(@下述同或公式：隐藏状态$h_{t-1}$被迫为0)，**仅用当前输入**$x_t$进行复位

- 有效地==使隐藏状态可以丢弃 将来发现不相关的任何信息== => 从而允许更紧凑地表示
- 具体过程：
  - 当r=0时，提取了隐藏状态h=0部分的信息(这恰恰是之前要被忘记的不重要的信息)
  - 而之前记住的信息(h=1)，此时被丢弃 => 同或运算为0
  - 但是这些不重要的信息，由$\tilde{h}_t$提取出来之后呢？💡

过程2 更新门控制从前一个隐藏状态将**有多少信息转移到当前隐藏状态** => 类似于LSTM网络中的记忆单元：有助于RNN记住长期信息

- ![z_{t}](https://private.codecogs.com/gif.latex?z_%7Bt%7D)是属于要记住的，反过来![1- z_{t}](https://private.codecogs.com/gif.latex?1-%20z_%7Bt%7D)则是属于忘记的



## GRU的结构

![image-20210612223117528](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210612223117.png)

同或运算：同时为真，或者同时为假 => 输出为真

- 为a**同或**b=ab+a'b'（a'为非a），即a和b相同为真，不同为假



