# 工具

1 仪器本身是否正常工作？

- 例子：粮仓测温度，温度计显示都一样 => 按照贝叶斯定理：

  - 

  $$
  P(state|result)=\frac{P(result|state) \cdot P(state)}{\sum_i P(result | state_i)\cdot P(state_i)}
  $$

  - state: 表示粮食良好
  - result: 表示温度计温度一致
  - 常识：==P(result|state)的概率很低== => 独立概率相乘



