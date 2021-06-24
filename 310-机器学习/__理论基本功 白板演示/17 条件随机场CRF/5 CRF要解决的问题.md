[B站视频](https://www.bilibili.com/video/BV19t411R7QU?p=6)

![image-20210607150503079](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210607150503.png)

学习：参数估计@`5.2似然概率最大化(最大化后验概率 P(y|X)，得到最优参数组合)`

- MLE：最大似然估计

推断：

- 边缘概率 => Evaluation：联合概率 => 积分得到边缘概率@`5.1 边缘概率(前向-后向算法)`
- 条件概率：以HMM为例
  - <img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210607145445.png" alt="image-20210607145445114" style="zoom:50%;" />
- MAP => decoding：以HMM为例，找到某个序列，使输出序列的可能性最大(后验概率最大)
  - `@HMM Viterbi算法`(本质上是动态规划的优化算法)
  - ![image-20210607145420565](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210607145421.png)



样本的说明：x、y均是T维 => 一个句子(T个单词)是一个样本

![image-20210607150608532](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210607150609.png)



注意：CRF是判别模型P(Y|X)

基于生成模型，可得到：

- 边缘概率
- 条件概率

