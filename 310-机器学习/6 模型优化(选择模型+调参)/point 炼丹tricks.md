[ link: AI蜗牛车|10大算法工程师炼丹Tricks](https://mp.weixin.qq.com/s?__biz=MzA4ODUxNjUzMQ==&mid=2247492126&idx=1&sn=a2e10a115d3c796faf4baf766100a08a&chksm=902a50c2a75dd9d476e066ab4dee86f4639a2f10352da0ee2dba2b7685c75c5d74921aa98049&scene=132#wechat_redirect)

- Cyclic LR： 每隔一段时间**重启学习率**，在单位时间内能收敛到多个局部最小值，可以**得到很多个模型做集成**
- 👌Focal Loss：降低容易样本的权重值，训练过程更加关注困难样本
- Warmup
- With Flooding：改变梯度方向
- 👌Dropout：随机丢弃，抑制过拟合
- 👌Normalization：针对单个神经元规范化(该神经元的均值和方差)
- 👌Group Normalization：替代深度学习里程碑式的工作Batch normalization
- Label Smoothing：将hard label转变成soft label，使网络优化更加平滑 => 减少训练DNN的过拟合问题并进一步提高分类性能
- 👌relu激活函数(缓解梯度消失)
- RAdam
- Adversarial Training
- Wasserstein GAN
- Skip Connection $F(x)=F(x)+x$：一种网络结构，提供恒等映射的能力，保证模型不会因网络变深而退化





## 学习率|Cyclic LR

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210609093144.webp)

每隔一段时间重启学习率，这样在==单位时间内能收敛到多个局部最小值==，可以得到很多个模型做集成。

```python
scheduler = lambda x: ((LR_INIT - LR_MIN) / 2) * (np.cos(PI * (np.mod(x-1, CYCLE) / (CYCLE) )) + 1) + LR_MIN
```



## 样本不平衡|Focal Loss

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210609093542.webp)

针对类别不平衡问题，**用预测概率对不同类别的loss进行加权**。

Focal loss对CE loss增加了一个**调制系数**来降低容易样本的权重值，使得训练过程更加关注困难样本。

```python
loss = -np.log(p) 
loss = (1 - p) ^ G * loss
```





## 训练过程|Warmup

Warmup有助于减缓模型**在初始阶段对mini-batch的提前过拟合现象**，保持分布的平稳，同时有助于保持**模型深层的稳定性**。

```python
warmup_steps = int(batches_per_epoch * 5)
warmup_lr = (initial_learning_rate * tf.cast(global_step, tf.float32) / tf.cast(warmup_steps, tf.float32))
return tf.cond(global_step < warmup_steps, lambda: warmup_lr, lambda: lr)
```





## 训练过程|With Flooding

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210609093147.webp)

当training loss大于一个阈值时，进行正常的梯度下降；

当training loss低于阈值时(上图中垂直线)，会反过来进行梯度上升，让training loss保持在一个阈值附近，让模型持续进行“random walk”，并期望模型能被==优化到一个平坦的损失区域==，这样发现test loss进行了double decent。

```python
flood = (loss - b).abs() + b
```









## 训练过程|Dropout

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210609093628.png)

随机丢弃，**抑制过拟合**，提高模型鲁棒性。





## 训练过程|Normalization

Batch Normalization 于2015年由 Google 提出，开 Normalization 之先河。

其规范化针对单个神经元进行，利用网络训练时一个 mini-batch 的数据来计算该神经元 ![图片](https://mmbiz.qpic.cn/mmbiz_svg/AhLk989Zrl2fbtqV8oeRC1evmdWseUSIhm4Uweb5SPiaMzR5BOaxAZ592AVKQdra6n0MzS0gz6WC0yySdXjv7aFibKt5fkE1wu/640?wx_fmt=svg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1) 的均值和方差,因而称为 Batch Normalization。

```python
x = (x - x.mean()) / x.std()
```





## 训练过程|Group Normalization

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210609093708.webp)



Face book AI research（FAIR）吴育昕-恺明联合推出重磅新作Group Normalization（GN），提出使用Group Normalization 替代深度学习里程碑式的工作Batch normalization。

一句话概括，Group Normbalization（GN）是一种新的深度学习归一化方式，可以替代BN。

```python
def GroupNorm(x, gamma, beta, G, eps=1e-5):
    # x: input features with shape [N,C,H,W]
    # gamma, beta: scale and offset, with shape [1,C,1,1]
    # G: number of groups for GN
    N, C, H, W = x.shape
    x = tf.reshape(x, [N, G, C // G, H, W])
    mean, var = tf.nn.moments(x, [2, 3, 4], keep dims=True)
    x = (x - mean) / tf.sqrt(var + eps)
    x = tf.reshape(x, [N, C, H, W])
    
    return x * gamma + beta
```





## 训练过程|Label Smoothing

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210609093735.png" alt="图片" style="zoom:67%;" />

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210609093744.png)

label smoothing将hard label**转变成soft label**，使网络优化更加平滑。

标签平滑是用于深度神经网络（DNN）的有效正则化工具，该工具通过在均匀分布和hard标签之间**应用加权平均值来生成soft标签**。

它通常用于==减少训练DNN的过拟合问题==，并进一步提高分类性能。

```python
targets = (1 - label_smooth) * targets + label_smooth / num_classes
```

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210609093817.png)





## 激活函数|Relu

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210609093656.png)

用极简的方式实现非线性激活，**缓解梯度消失**。

```python
x = max(x, 0)
```





## 梯度优化|RAdam

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210609093423.png)

RAdam 的核心在于用**指数滑动平均**去估计梯度每个分量的一阶矩(动量)和**二阶矩(自适应学习率)**，并==用二阶矩去 normalize 一阶矩==，得到每一步的更新量。

```python
from radam import *
```





## 梯度优化|Adversarial Training

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210609093505.png)

对抗训练（Adversarial Training），顾名思义，就是在训练过程中**产生一些攻击样本**，早期是FGSM和I-FGSM攻击，目前当前最优的攻击手段是PGD。

==对抗训练，相当于是加了一层正则化，给神经网络的随机梯度优化限制了一个李普希茨的约束==。

传统上认为，这个训练方式会牺牲掉一定的测试精度，因为卷积模型关注局部特性，会学到一些敏感于扰动的特征，对抗训练是一种去伪存真的过程，这是目前像素识别的视觉算法的局限性。

这里苏建林在kexue.fm里实现是很简单的，详情参看引用链接。

```python
# 写好函数后，启用对抗训练只需要一行代码
adversarial_training(model, 'Embedding-Token', 0.5)
```

------









## 网络结构|Wasserstein GAN

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210609093826.png)

- 彻底解决GAN训练不稳定的问题，**不再需要小心平衡**生成器和判别器的训练程度
- 基本解决了Collapse mode的问题，确保了生成样本的多样性
- 训练过程中终于有一个**像交叉熵、准确率这样的数值来指示训练的进程**，数值越小代表GAN训练得越好，代表生成器产生的图像质量越高
- 不需要精心设计的网络架构，最简单的多层全连接网络就可以做到以上3点。





## 网络结构|Skip Connection

 一种网络结构，==提供恒等映射的能力==，保证模型不会因网络变深而退化。

```python
F(x) = F(x) + x
```
