> Artificial neural networks
>
> 所有的神经网络都基于ANN，一共有两大类
>
> 1）非监督学习(没有数据标签)：以Auto-encoder为代表的
>
> - 本质上还是以输入为标签 => 尽量接近输入
>
> 2）监督学习(有数据标签)：比如CNN, RNN(LSTM, GRU)等
>
> - 反向传播都是基于梯度下降进行优化 => me|所以我们==关注的重心是：**前向传播能不能实现某些功能**？==
>   - 比如CNN识别图像特征
>   - RNN预测序列输出 => LSTM及其变式改善长时记忆的问题
> - 而优化：统一交给反向传播梯度下降 => 前提：有标签的监督学习
>
> 
>
> 其他关注重心是：
>
> 0）模型训练的过程
>
> - 大量高质量数据
> - 减少过拟合
>   - 正则化
>     - 参数正则化
>     - 数据归一化Batch normalization
>   - dropout
>   - 提早结束
>
> 1）反向优化的过程有什么问题？
>
> - 指标关注模型的哪些性能？
>   - 不同的指标侧重不同
> - 梯度消失，梯度爆炸
> - 优化限于局部最小点
> - ……





# 概念要点

逻辑回归：0-1编码

感知器

- 激活函数：sigmoid, softmax

- 误差函数 => 梯度下降(优化方法)

交叉熵

神经网络架构

- 前向反馈 
- 误差函数 => 反向传播



# 神经网路计算模型

绿色模型更合适

![image-20210209110537992](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210209110538.png)

| 适合情境             |                                  |
| -------------------- | -------------------------------- |
| 监督学习：分类，回归 | 从名字中预测性别                 |
| 无监督学习           | 从兴趣推测友谊关系               |
|                      | 从新闻中，对相类似的主题进行分组 |



## 人脑机制

batch学习(机器) vs online学习(人脑)

online学习的缺点：模型性能的评估更难(每一次都是基于新的数据，增量改进)



人工神经网络实际上可以使用在线学习和批处理学习中的一个（或两个）

神经元是人脑中最简单的计算单元，它们的相互作用促进了诸如学习之类的大脑功能。

每个神经元计算一个简单的函数。尽管神经元计算的实际动力学很复杂，但对其简化的看法是它们可以整合并触发：也就是说，神经元使用其输入执行计算，然后在该计算通过特定阈值时触发。



ANN中神经元模型的基本思想是将神经元的输入（作为加权总和）组合为单个值v。然后，激活函数H（v），用于确定神经元是否触发。

对于身体神经元而言，发射是“全有或全无”。也就是说，它会触发或不会触发。(红色曲线)

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210209111312.png" alt="image-20210209111312359" style="zoom:67%;" />



神经元之间的连接（或突触）用于将信息从某些神经元的输出传递到其他神经元的输入。

- 并非每个神经元都与每个其他神经元相连，并且某些神经元与某些神经元之间的联系更紧密。

- 大约有$10^{11}$个神经元，有$10^{14}-10^{15}$个突触
  - 每个神经元的平均连接数在5000个左右

通过调整存在的连接以及连接的强度，人脑能够学习各种各样的复杂功能。

如果模型可以学习适当地调整这些连接的强度，则它可能能够接近人脑的力量。





## 数学推导

![image-20210608143647299](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210608143647.png)

![image-20210608143720207](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210608143720.png)



| 对象                                            | 常用命名                                         | 符号                                                         |
| ----------------------------------------------- | ------------------------------------------------ | ------------------------------------------------------------ |
| 【前向网络】                                    |                                                  |                                                              |
| 输入数据/特征                                   | input_features                                   | $\vec{x}$                                                    |
| 中间输出                                        | hidden_features_pre_`i`                          | $\vec{h} = \vec{\bold{W}}^{i} \cdot \vec{x} + \vec{b}^{i}$   |
| 激活函数H(v)                                    | hidden_features_activ_`i`                        | $\hat{\vec{h}} = \sigma(\vec{h})$                            |
| 输出层                                          | output_pre_`i`                                   | $\vec{y} = \vec{\bold{W}}^{output} \cdot \hat{\vec{h}} + \vec{b}^{output}$ |
|                                                 | output_activ_`i`                                 | $\hat{\vec{y}} = \sigma(\vec{y})$                            |
| 【反馈传播】<br />目标：更新权重(L对权重的梯度) |                                                  |                                                              |
| 损失函数loss function                           | error/loss                                       | $L$                                                          |
| 到输出层的权重                                  | weights_gradients_`output`<br />weights_h_o⭐     | 公式(1)                                                      |
| 到输出层的偏置                                  | bias_gradients_`output`                          | 公式(2)                                                      |
| 中间隐藏层的权重                                | weights_gradients_`i`<br />weigths_h`i-1`\_h`i`⭐ | 公式(3)                                                      |
| 中间隐藏层的偏置                                | bias_gradients_`i`                               | 公式(4)                                                      |
| 更新权重                                        | weights_update                                   | 公式(5)                                                      |

$$
\frac{\partial L}{\vec{\bold{W}}^{output}} = \frac{\partial{L}}{\partial{\hat{\vec{y}}}} \cdot \frac{\partial{\hat{\vec{y}}}}{\partial{\vec{y}}} \cdot \frac{\partial{\vec{y}}}{\partial{\vec{\bold{W}}^{output}}}
$$

$$
\frac{\partial L}{\vec{b}^{output}} = \frac{\partial{L}}{\partial{\hat{\vec{y}}}} \cdot \frac{\partial{\hat{\vec{y}}}}{\partial{\vec{y}}} \cdot \frac{\partial{\vec{y}}}{\partial{b^{output}}} \\
\frac{\partial{\vec{y}}}{\partial{b^{output}}} = 1
$$


$$
\frac{\partial L}{\vec{\bold{W}}^{i}} = \frac{\partial{L}}{\partial{\hat{\vec{y}}}} \cdot \frac{\partial{\hat{\vec{y}}}}{\partial{\vec{y}}} \cdot \frac{\partial{\vec{y}}}{\partial{\hat{\vec{h}^{i}}}} \cdot \frac{\partial{\hat{\vec{h}^{i}}}}{\vec{h}^{i}} \cdot \frac{\vec{h}^{i}}{\partial{\vec{\bold{W}}^{i}}}
$$

$$
\frac{\partial L}{\vec{b}^{i}} = \frac{\partial{L}}{\partial{\hat{\vec{y}}}} \cdot \frac{\partial{\hat{\vec{y}}}}{\partial{\vec{y}}} \cdot \frac{\partial{\vec{y}}}{\partial{\hat{\vec{h}^{i}}}} \cdot \frac{\partial{\hat{\vec{h}^{i}}}}{\vec{h}^{i}} \cdot \frac{\vec{h}^{i}}{\partial{\vec{b}^{i}}} \\
\frac{\partial{\vec{h}^{i}}}{\partial{b^{i}}} = 1
$$


$$
W^{i+1} = W^{i} - \eta \cdot \frac{\partial L}{\vec{W}^{i}}
$$

```python
#==# 注意维度匹配(待补充 源码)
'''
网络结构：3*2*1
注意：此处不涉及偏置(可认为是常数0)

总结：反向传播的计算分两类
1 隐藏层的误差：维度是(num_hidden_layer,)
2 更新权重：维度是(num_hidden(i-1)_layer, num_hidden(i)_layer) => 需要用到np.outer()
'''
error = target - output	#(1,)
output_pre_error = error * output * (1-ouput)	#(1,)

hidden_activ_error = output_pre_error * weights_h_o	#(2,)
hidden_pre_error = hidden_activ_error * hidden_activ * (1-hidden_activ)	#(2,)

delta_w_h_o = learnrate * output_pre_error * hidden_activ	#(2,)
delta_w_i_h = learnrate * np.dot(x[:, None], hidden_pre_error.reshape(1, -1))	#(3, 1) * (1, 2) == (3, 2) 直接用np.outer()
```

> [np.outer(a, b)](https://numpy.org/doc/stable/reference/generated/numpy.outer.html) 输入的a, b会自动展开为1D数组
>
> a(M,), b(N,) => (M, N)：`out[i, j] = a[i] * b[j]`
>
> 等价形式：`einsum('i,j->ij', a.ravel(), b.ravel())`

- `None`等价于`np.newaxis`

![image-20210608152523882](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210608152524.png)



### 前向网络

[link: Evan|深度学习-bp神经网络](https://zhuanlan.zhihu.com/p/96810689)

![img](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210319082627.jpeg)

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210209111831.png" alt="image-20210209111831561" style="zoom:67%;" />

hypersurface $\vec{w} \cdot \vec{x} +b = 0$是**decision boundary**决策边界 => 线性分类器：该边界基于输入的线性组合。



@几何学

![image-20210209115335509](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210209115335.png)

$\vec{x} = (SAT score, GPA)$ => $\vec{w} = (1, 375)$

直线$ax+ by+c =0$ => 法向量是(a, b) =>直观呈现为直线 $y = - \frac{a}{b}x - \frac{c}{b}$



### 激活函数

![image-20210209115643936](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210209115644.png)

无论情况如何复杂，具有适当参数的足够大的ANN都可以对其进行建模。



# 完整代码

```python
import numpy as np

class NeuralNetwork():
    def __init__(self):
        '''网络结构：3*1(输出层激活函数是sigmoid函数)
        '''
        # seeding for random number generation
        np.random.seed(1)

        # converting weights to a 3 by 1 matrix with values from -1 to 1 and mean of 0
        self.synaptic_weights = 2 * np.random.random((3, 1)) - 1

 
    def sigmoid(self, x):
        #applying the sigmoid function
        return 1 / (1 + np.exp(-x))

    
    def sigmoid_derivative(self, x):
        #computing derivative to the Sigmoid function
        return x * (1 - x)

 
    def train(self, training_inputs, training_outputs, training_iterations):
        #training the model to make accurate predictions while adjusting weights continually
        for iteration in range(training_iterations):
            #siphon the training data via  the neuron
            output = self.think(training_inputs)

            #computing error rate for back-propagation
            error = training_outputs - output
			
            #==# 关键步骤：基于error，反向传播调整权重
            #performing weight adjustments
            adjustments = np.dot(training_inputs.T, error * self.sigmoid_derivative(output))

            self.synaptic_weights += adjustments

    def think(self, inputs):	
        '''前向传播
        '''
        #passing the inputs via the neuron to get output   
        #converting values to floats
        inputs = inputs.astype(float)
        output = self.sigmoid(np.dot(inputs, self.synaptic_weights))

        return output

if __name__ == "__main__":
    #initializing the neuron class
    neural_network = NeuralNetwork()

    print("Beginning Randomly Generated Weights: ")
    print(neural_network.synaptic_weights)

    #training data consisting of 4 examples--3 input values and 1 output
    training_inputs = np.array([[0,0,1],
                                [1,1,1],
                                [1,0,1],
                                [0,1,1]])

    training_outputs = np.array([[0,1,1,0]]).T

    #training taking place
    neural_network.train(training_inputs, training_outputs, 15000)
    print("Ending Weights After Training: ")
    print(neural_network.synaptic_weights)


    user_input_one = str(input("User Input One: "))
    user_input_two = str(input("User Input Two: "))
    user_input_three = str(input("User Input Three: "))

    print("Considering New Situation: ", user_input_one, user_input_two, user_input_three)
    print("New Output data: ")
    print(neural_network.think(np.array([user_input_one, user_input_two, user_input_three])))
	print("Wow, we did it!")
```

