[link: 原文链接](https://mp.weixin.qq.com/s/v5lFbcQRmh-Beq4OHCYyRA)



# 深度学习调参

[link: 知乎|京东白条](https://www.zhihu.com/question/25097993/answer/651617880)

- 王惠东，京东数字科技-个人服务群组-**个人风险管理中心**-智能模型实验室-算法工程师



很多刚开始接触深度学习朋友，会感觉深度学习调参就像玄学一般，有时候参数调的好，模型会快速收敛，参数没调好，可能迭代几次loss值就直接变成Nan了。



示例：用tensorflow构建了一个十分简单的只有一个输入层和一个softmax输出层的Mnist手写识别网络

- 第一次我对权重矩阵W和偏置b采用的是正态分布初始化，一共迭代了20个epoch，当迭代完第一个epoch时，预测的准确度只有10%左右（和随机猜一样，Mnist是一个十分类问题），当迭代完二十个epoch，精度也仅仅达到了60%的样子。
- 然后我仅仅是将权重矩阵W初始化方法改成了全为0的初始化，其他的参数均保持不变，结果在训练完第一个epoch后预测精度就达到了85%以上，最终20个epoch后精度达到92%。



示例：回归问题的预测

- 当时采用的SGD优化器，一开始学习率设定的0.1，模型可以正常训练，只是训练速度有些慢，我试着将学习率调整到0.3，希望可以加速训练速度，结果没迭代几轮loss就变成Nan了





上述问题产生的原因也很好理解，对于参数初始化，因为我们学习的本来就是权重W与偏置b，如果初始化足够好，直接就初始化到最优解，那都不用进行训练了。

- 良好的初始化，可以让参数更接近最优解，这可以大大提高收敛速度，也可以防止落入局部极小。
- 对于学习率，学习率如果取太大，会使模型训练非常震荡，可以想象我们最小化一个二次抛物线，选取一个很大的学习率，那么迭代点会一直在抛线的两边震荡，收敛不到最小值，甚至还有螺旋上升迭代点的可能。





## 1 激活函数选择

常用的激活函数有relu、leaky-relu、sigmoid、tanh等。

对于输出层：

- 多分类任务选用softmax输出
- 二分类任务选用sigmoid输出
- 回归任务选用线性输出。

而对于中间隐层，则优先选择relu激活函数（relu激活函数可以有效的解决sigmoid和tanh出现的梯度弥散问题，==多次实验表明它会比其他激活函数以更快的速度收敛==）

另外，构建序列神经网络（RNN）时要优先选用tanh激活函数。



## 2 **学习率设定**

一般学习率从0.1或0.01开始尝试。

学习率设置太大会导致训练十分不稳定，甚至出现Nan，设置太小会导致损失下降太慢。



学习率一般要随着训练进行衰减。

衰减系数设0.1，0.3，0.5均可，衰减时机：

- 可以是验证集准确率不再上升时
- 或固定训练多少个周期以后自动进行衰减。



## 3 防止过拟合

一般常用的防止过拟合方法有：

- 使用L1正则项、L2正则项
- dropout
- 提前终止
- ==数据集扩充==等。

如果模型在训练集上表现比较好但在测试集上表现欠佳，可以选择增大L1或L2正则的惩罚力度（L2正则经验上首选1.0，超过10很少见），或增大dropout的随机失活概率（经验首选0.5）；



或者当随着训练的持续在测试集上不增反降时，使用提前终止训练的方法。



当然最有效的还是增大训练集的规模，实在难以获得新数据也可以使用数据集增强的方法，比如CV任务可以对数据集进行裁剪、翻转、平移等方法进行==数据集增强==，这种方法往往都会提高最后模型的测试精度。





## 4 优化器选择

如果数据是稀疏的，就用==自适应方法==，即 Adagrad, Adadelta, RMSprop, Adam。

整体来讲，Adam 是最好的选择。

SGD 虽然能达到极小值，但是比其它算法用的时间长，而且可能会被困在鞍点。

如果需要更快的收敛，或者是训练更深更复杂的神经网络，需要用一种自适应的算法。





## 5 残差块与BN层

如果你希望训练一个更深更复杂的网络，那么残差块绝对是一个重要的组件，它可以让你的网络训练的更深。



BN层具有加速训练速度，有效防止梯度消失与梯度爆炸，具有防止过拟合的效果，所以构建网络时最好要加上这个组件。





## 6 自动调参方法

（1）Grid Search：网格搜索，在所有候选的参数选择中，通过循环遍历，尝试每一种可能性，表现最好的参数就是最终的结果。其原理就像是在数组里找最大值。

- 缺点是太费时间了，特别像神经网络，一般尝试不了太多的参数组合。



（2）Random Search：==经验上，Random Search比Gird Search更有效==。实际操作的时候，一般也是先用Gird Search的方法，得到所有候选参数，然后每次从中随机选择进行训练。

- 另外Random Search往往会和由粗到细的调参策略结合使用，即在效果比较好的参数附近进行更加精细的搜索



（3）Bayesian Optimization：贝叶斯优化，考虑到了不同参数对应的   实验结果值，因此更节省时间

- 贝叶斯调参比Grid Search迭代次数少，  速度快；
- 而且其针对非凸问题依然稳健。



## 7 参数随机初始化与数据预处理

参数初始化很重要，它决定了模型的训练速度与是否可以躲开局部极小。

- relu激活函数初始化推荐使用He normal
- tanh初始化推荐使用Glorot normal，其中Glorot normal也称作Xavier normal初始化；
- 数据预处理方法一般也就采用数据归一化即可。







# CNN为主

[link: 知乎|杨军](https://www.zhihu.com/question/25097993/answer/127374415)

出于个人习性，我并不习惯于通过单纯的trial-and-error的方式来调试经常给人以”black-box”印象的Deep Learning模型，所以在工作推进过程中，花了一些时间去关注了深度学习**模型调试以及可视化**的资料（==可视化与模型调试存在着极强的联系==，所以在后面我并没有对这两者加以区分），这篇文章也算是这些工作的一个阶段性总结。

我本人是计算机体系结构专业出身，中途转行做算法策略，所以实际上我倒是在大规模机器学习系统的开发建设以及训练加速方面有更大的兴趣和关注。

不过机器学习系统这个领域跟常规系统基础设施（比如**Redis**/LevelDB以及一些分布式计算的基础设施等）还有所区别，虽然也可以说是一种基础设施，但是它跟跑在这个基础设施上的业务问题有着更强且直接的联系，所以我也会花费一定的精力来关注数据、业务建模的技术进展和实际问题场景。



文章的主要内容源于Stanford CS231n Convolutional Neural Networks for Visual Recognition课程[1]里介绍的一些通过可视化手段，调试理解CNN网络的技巧，在[1]的基础上我作了一些沿展阅读，算是把[1]的内容进一步丰富系统化了一下。



## 1 Visualize Layer Activations

通过将神经网络隐藏层的激活神经元以矩阵的形式可视化出来，能够让我们看到一些有趣的insights。

在[8]的头部，嵌入了一个web-based的CNN网络的demo，可以看到每个layer activation的可视化效果。 

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210412153530.webp)



**原则上来说，比较理想的layer activation应该具备sparse和localized的特点。**

**如果训练出的模型，用于预测某张图片时，发现在卷积层里的某个feature map的activation matrix可视化以后，基本跟原始输入长得一样，基本就表明出现了一些问题，因为这意味着这个feature map没有学到多少有用的东西。**



## 2 Visualize Layer Weights

除了可视化隐藏层的activation以外，可视化隐藏层的模型weight矩阵也能帮助我们获得一些insights。这里是AlexNet的第一个卷积层的weight可视化的示例：

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210412153649.webp)

通常，我们期望的良好的卷积层的weight可视化出来会具备smooth的特性（在上图也能够明显看到smooth的特点），参见下图（源于[13]）： 

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210412153700.webp)

这两张图都是将一个神经网络的第一个卷积层的filter weight可视化出来的效果图，**左图存在很多的噪点，右图则比较平滑。**

- 出现左图这个情形，往往意味着我们的模型训练过程出现了问题。



## 3 Retrieving Images That Maximally Activate a Neuron

先理解CNN里Receptive Field的概念，在[5][6]里关于Receptive Field给出了直观的介绍：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210412153736.webp" alt="图片" style="zoom:67%;" />

如果用文字来描述的话，就是对应于卷积核所生成的Feature Map里的一个neuron，在计算这个neuron的标量数值时，是使用卷积核在输入层的图片上进行卷积计算得来的，对于Feature Map的某个特定neuron，用于计算该neuron的输入层数据的local patch就是这个neuron的receptive field。



而对于一个特定的卷积层的Feature Map里的某个神经元，我们可以找到使得这个神经元的activation最大的那些图片，然后再从这个Feature Map neuron还原到原始图片上的receptive field，即可以看到是哪张图片的哪些region maximize了这个neuron的activation。

在[7]里使用这个技巧，对于某个pooling层的输出进行了activation maximization可视化的工作：



**不过，在[9]里，关于3提到的方法进行了更为细致的研究，在[9]里，发现，通过寻找maximizing activation某个特定neuron的方法也许并没有真正找到本质的信息。**因为即便是对于某一个hidden layer的neurons进行线性加权，也同样会对一组图片表现出相近的semantic亲和性，并且，这个发现在不同的数据集上得到了验证。



## 4 Embedding the Hidden Layer Neurons with  t-SNE

这个方法描述起来比较直观，就是通过t-SNE[10]对**隐藏层进行降维**，然后以降维之后的两维数据分别作为x、y坐标（也可以使用t-SNE将数据降维到三维，将这三维用作x、y、z坐标，进行3d clustering），对数据进行clustering，人工review同一类图片在降维之后的低维空间里是否处于相邻的区域。

t-SNE降维以后的clustering图往往需要在较高分辨率下才能比较清楚地看到效果，这里我没有给出引用图，大家可以自行前往这里[15]里看到相关的demo图。

使用这个方法，可以让我们站在一个整体视角观察模型在数据集上的表现。



## 5 Occluding Parts of the Image

这个方法在[11]里被提出。我个人非常喜欢这篇文章，因为这篇文章写得非常清晰，并且给出的示例也非常直观生动，是那种非常适合推广到工业界实际应用场景的论文，能够获得ECCV 2014 best paper倒也算在意料之中。在[11]里，使用了[12]里提出的Deconvolutional Network，对卷积层形成的feature map进行reconstruction，将feature map的activation投影到输入图片所在的像素空间，从而提供了更直观的视角来观察每个卷积层学习到了什么东西，一来可以帮助理解模型；二来可以指导模型的调优设计。
[11]的工作主要是在AlexNet这个模型上做的，将Deconvolutional Network引入到AlexNet模型以后的大致topology如下： 





## 小结

卷积神经网络自从2012年以AlexNet模型的形态在ImageNet大赛里大放异彩之后，就成为了图像识别领域的标配，甚至现在文本和语音领域也开始在使用卷积神经网络进行建模了。

不过==以卷积神经网络为代表的深层神经网络一直被诟病“black-box”==，这对于DL模型在工业界的应用推广还是带来了一定的阻碍。



对于”black-box”这个说法，**一方面，我觉得确实得承认DL这种model跟LR、GBDT这些shallow model相比，理解、调试的复杂性高了不少**。

想像一下，理解一个LR或是GBDT模型的工作机理，一个没有受到过系统机器学习训练的工程师，只要对LR或GBDT的基本概念有一定认识，也大致可以通过ad-hoc的方法来进行good case/bad case的分析了。

而CNN这样的模型，理解和调试其的技巧，则往往需要资深的专业背景人士来提出，并且这些技巧也都还存在一定的局限性。

对于LR模型来说，我们可以清晰地描述一维特征跟目标label的关系（即便存在特征共线性或是交叉特征，也不难理解LR模型的行为表现），而DL模型，即便这几年在模型的可解释性、调试技巧方面有不少研究人员带来了新的进展，在我来看也还是停留在一个相对”rough”的控制粒度，对技巧的应用也还是存在一定的门槛。



**另一方面，我们应该也对学术界、工业界在DL模型调试方面的进展保持一定的关注**。

我自己的体会，DL模型与shallow model的应用曲线相比，目前还是存在一定的差异的。从网上拉下来一个pre-trained好的模型，应用在一个跟pre-trained模型相同的应用场景，能够快速地拿到7，80分的收益，但是，如果应用场景存在差异，或者对模型质量要求更高，后续的模型优化往往会存在较高的门槛（这也是模型调试、可视化技巧发挥用武之地的地方），而模型离线tune好以后，布署到线上系统的overhead也往往更高一些，不论是在线serving的latency要求（这也催生了一些新的商业机会，比如Nervana和寒武纪这样的基于软硬件协同设计技术的神经网络计算加速公司），还是对memory consumption的需求。

以前有人说过一句话“*现在是个人就会在自己的简历上写自己懂Deep Learning，但其实只有1%的人知道怎样真正design一个DL model，剩下的只是找来一个现成的DL model跑一跑了事*”。这话听来刺耳，但其实有几分道理。



回到我想表达的观点，**一方面我们能够看到DL model应用的门槛相较于shallow  model要高，另一方面能够看到这个领域的快速进展。**

所以对这个领域的技术进展保持及时的跟进，对于模型的设计调优以及在业务中的真正应用会有着重要的帮助。像LR、GBDT这种经典的shallow model那样，搞明白基本建模原理就可以捋起袖子在业务中开搞，不需要再分配太多精力关注模型技术的进展的工作方式，在当下的DL建模场景，我个人认为这种技术工作的模式并不适合。

也许未来随着技术、工具平台的进步，可以把DL也做得更为易用，到那时，使用DL建模的人也能跟现在使用shallow model一样，==可以从模型技术方面解放出更多精力，用于业务问题本身了==。



References:
[1]. Visualizing what ConvNets Learn. CS231n Convolutional Neural Networks for Visual Recognition
CS231n Convolutional Neural Networks for Visual Recognition
[2]. Matthew Zeiler. Visualizing and Understanding Convolutional Networks. Visualizing and Understanding Convolutional Networks.
[3]. Daniel Bruckner. deepViz: Visualizing Convolutional Neural Networks for Image Classification.
http://vis.berkeley.edu/courses/cs294-10-fa13/wiki/images/f/fd/DeepVizPaper.pdf
[4]. ConvNetJS MNIST Demo. ConvNetJS MNIST demo
[5]. Receptive Field. CS231n Convolutional Neural Networks for Visual Recognition
[6]. Receptive Field of Neurons in LeNet. deep learning
[7]. Ross Girshick. Rich feature hierarchies for accurate object detection and semantic segmentation
Tech report. Arxiv, 2011.
[8]. CS231n: Convolutional Neural Networks for Visual Recognition. Stanford University CS231n: Convolutional Neural Networks for Visual Recognition
[9]. Christian Szegedy. Intriguing properties of neural networks. Arxiv, 2013.
[10]. t-SNE. t-SNE – Laurens van der Maaten
[11]. Matthew D.Zeiler. Visualizing and Understanding Convolutional Networks. Arxiv, 2011.
[12]. Matthew D.Zeiler. Adaptive Deconvolutional Networks for Mid and High Level Feature Learning, ICCV 2011.
[13]. Neural Networks Part 3: Learning and Evaluation. CS231n Convolutional Neural Networks for Visual Recognition
[14]. ConvNetJS---Deep Learning in Your Browser.ConvNetJS: Deep Learning in your browser
[15]. Colah. Visualizing MNIST: An Exploration of Dimensionality Reduction. http://colah.github.io/posts/2014-10-Visualizing-MNIST/

