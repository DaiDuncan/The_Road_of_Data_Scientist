

## 语言模型

**语言模型就是用来计算一个句子的概率的模型，也就是判断一句话是否是人话的概率？**

> 给定个句子(词序列)：$S=W_1, W_2,..., W_k$
>
> 概率表示为：$P(S) = P(W_1, W_2,..., W_k) = P(W_1)P(W_2|W_1)...P(W_k|W_1, W_2,..., W_{k-1})$

也就是说在给定一句话的前k个词，我们希望语言模型可以预测第k+1个词是什么，即给出一个第k+1个词可能出现的概率的分布p(xk+1|x1x2...xk)

![image-20210609101903624](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210609101903.png)



perplexity困惑度是交叉熵的指数形式

perplexity和交叉熵一样都可以用来评价**语言模型**的好坏。

- 对于测试集其困惑度越小, 准确率也就越高，语言模型也就越好～



下面是一些 ngra­m 模型经 训练文本后在测试集上的困惑度值：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210609102000.jpeg" alt="img" style="zoom: 80%;" />