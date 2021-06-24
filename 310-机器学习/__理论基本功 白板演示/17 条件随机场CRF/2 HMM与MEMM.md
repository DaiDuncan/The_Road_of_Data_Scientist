![image-20210606215107870](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210606215108.png)

观测独立的假设不合理：

- 上下文本身有关联 => MEMM

马尔科夫的假设不合理



HMM：生成式模型的公式 $P(X, Y|\lambda)$

- 注意初始项：$P(y_1|y_0)$ => 一般等价于$P(y_1)$



MEMM：判别模型 $P(Y|X, \lambda)$ 

- 贝叶斯网络：head to head
  - $y_t$给定时：$x_t, x_{t-1}$相关，不独立
- 观测的影响分成两部分
  - 一部分是整体观测的影响(彼此不独立)
  - 另一部分是局部观测的影响(临近的emission)
- 作用：关注词性标注的问题
  - 1）简单：不需要计算联合概率分布 => 仅条件概率即可
  - 2）假设更合理：没有观测独立的假设



