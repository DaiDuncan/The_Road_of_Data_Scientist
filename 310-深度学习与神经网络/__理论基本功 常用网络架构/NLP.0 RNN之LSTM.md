Long-Short Term Memory Networks 长短期记忆

[原文链接|CSDN RNN到LSTM](https://blog.csdn.net/v_JULY_v/article/details/89894058)

# **前言**

提到LSTM，之前学过的同学可能最先想到的是Christopher Olah的博文《[理解LSTM网络](https://colah.github.io/posts/2015-08-Understanding-LSTMs/)》，这篇文章确实厉害，网上流传也相当之广，而且当你看过了网上很多关于LSTM的文章之后，你会发现这篇文章确实经典。不过呢，如果你是第一次看LSTM，则原文可能会给你带来不少障碍：

- 一者，一上来就干LSTM，不少读者可能还没理解好RNN。基于此，我们可以从最简单的单层网络开始学起；
- 二者，原文没有对LSTM的三个门解释的足够细致，包括**三个不同的sigmoid函数**用的同一个符号σ（有错么？没错，看多了就会明白这是习惯用法）；
- 三者，**不同的权值也用的同一个符号w**，而当把图、公式、关系一一对应清楚后，初学就不会一脸懵逼了。甚至我们把各个式子的计算过程用有方向的水流表示出来，则会好懂不少，这个时候就可以上动图。

而我自己就是这么经历过来的，虽然学过了不少模型/算法，但此前对LSTM依然很多不懂，包括一开始反复看Christopher Olah博文《理解LSTM网络》，好在和我司AI Lab陈博士反复讨论之后，终于通了！

侧面说明，当一个人冥思苦想想不通时，十之八九是因为看的资料不够通俗，如果还是不行，则问人，结果可能瞬间领悟，这便是教育的意义，也是我们做[七月在线](https://www.julyedu.com/)的巨大价值。



众所周知，我们已经把SVM、CNN、xgboost、LSTM等很多技术，写的/讲的国内最通俗易懂了。接下来，我们要把BERT等技术也写的/讲的国内最通俗易懂，成为入门标准，而且不单单是从NNLM ![\rightarrow](https://private.codecogs.com/gif.latex?%5Crightarrow) Word2Vec ![\rightarrow](https://private.codecogs.com/gif.latex?%5Crightarrow) Seq2Seq ![\rightarrow](https://private.codecogs.com/gif.latex?%5Crightarrow) Seq2Seq with Attention ![\rightarrow](https://private.codecogs.com/gif.latex?%5Crightarrow) Transformer ![\rightarrow](https://private.codecogs.com/gif.latex?%5Crightarrow) Elmo ![\rightarrow](https://private.codecogs.com/gif.latex?%5Crightarrow) GPT ![\rightarrow](https://private.codecogs.com/gif.latex?%5Crightarrow) BERT，我们希望给所有AI初学者铺路：一步一个台阶，而不是出现理解断层。

本文在ChristopherOlah的博文及@Not_GOD 翻译的[译文](https://www.jianshu.com/p/9dc9f41f0b29/)等文末参考文献的基础上做了大量便于理解的说明/注解（这些说明/注解是在其他文章里不轻易看到的），一切为更好懂。



# **一、RNN**👌

## **1.1 从单层网络到经典的RNN结构**
在学习LSTM之前，得先学习RNN，而在学习RNN之前，首先要了解一下最基本的单层网络，它的结构如下图所示：

![img](https://julyedu-img-public.oss-cn-beijing.aliyuncs.com/Public/Image/Question/1515688775_462.jpg)

输入是x，经过变换Wx+b和激活函数f，得到输出y。

在实际应用中，我们还会遇到很多序列形的数据：

![img](https://julyedu-img-public.oss-cn-beijing.aliyuncs.com/Public/Image/Question/1515688806_178.png)


如：

1. 自然语言处理问题。x1可以看做是第一个单词，x2可以看做是第二个单词，依次类推。
2. 语音处理。此时，x1、x2、x3……是每帧的声音信号。
3. 时间序列问题。例如每天的股票价格等等。

而其中，**序列形的数据**就不太好用原始的神经网络处理了。



为了建模序列问题，RNN引入了隐状态h（hidden state）的概念，**h可以对序列形的数据提取特征**，接着再转换为输出。

先从![h_{1}](https://private.codecogs.com/gif.latex?h_%7B1%7D)的计算开始看：

![img](https://julyedu-img-public.oss-cn-beijing.aliyuncs.com/Public/Image/Question/1515688853_805.png)


图示中记号的含义是：
a）圆圈或方块表示的是向量。
b）一个箭头就表示对该向量做一次变换。如上图中![h_{0}](https://private.codecogs.com/gif.latex?h_%7B0%7D)和![x_{1}](https://private.codecogs.com/gif.latex?x_%7B1%7D)分别有一个箭头连接，就表示对![h_{0}](https://private.codecogs.com/gif.latex?h_%7B0%7D)和![x_{1}](https://private.codecogs.com/gif.latex?x_%7B1%7D)各做了一次变换。
在很多论文中也会出现类似的记号，初学的时候很容易搞乱，但只要把握住以上两点，就可以比较轻松地理解图示背后的含义。

![h_{2}](https://private.codecogs.com/gif.latex?h_%7B2%7D)的计算和![h_{1}](https://private.codecogs.com/gif.latex?h_%7B1%7D)类似。但有两点需要注意下：

1. 在计算时，每一步使用的参数U、W、b都是一样的，也就是说==每个步骤的参数都是共享的，这是RNN的重要特点==，一定要牢记；
2. 而下文马上要看到的==LSTM中的权值则不共享==，因为它是在两个不同的向量中。而RNN的权值为何共享呢？很简单，因为RNN的权值是**在同一个向量中**，只是不同时刻而已。

![img](https://julyedu-img-public.oss-cn-beijing.aliyuncs.com/Public/Image/Question/1515688897_608.png)


依次计算剩下来的（使用相同的参数U、W、b）：

![img](https://julyedu-img-public.oss-cn-beijing.aliyuncs.com/Public/Image/Question/1515688915_964.png)


我们这里为了方便起见，只画出序列长度为4的情况，实际上，这个计算过程可以无限地持续下去。

我们目前的RNN还没有输出，得到输出值的方法就是直接通过h进行计算：

![img](https://julyedu-img-public.oss-cn-beijing.aliyuncs.com/Public/Image/Question/1515688949_285.png)


正如之前所说，一个箭头就表示对对应的向量做一次类似于f(Wx+b)的变换，这里的这个箭头就表示对h1进行一次变换，得到输出y1。

剩下的输出类似进行（使用和y1同样的参数V和c）：

![img](https://julyedu-img-public.oss-cn-beijing.aliyuncs.com/Public/Image/Question/1515688967_947.png)

OK！大功告成！这就是最经典的RNN结构，是x1, x2, .....xn，输出为y1, y2, ...yn，也就是说，输入和输出序列必须要是等长的。



## **1.2 RNN的应用**
人类并不是每时每刻都从一片空白的大脑开始他们的思考。

在你阅读这篇文章时候，你都是**基于已经拥有的对先前所见词的理解**来推断当前词的真实含义。

我们不会将所有的东西都全部丢弃，然后用空白的大脑进行思考。我们的思想拥有持久性。



传统的神经网络并不能做到这点，看起来也像是一种巨大的弊端。

例如，假设你希望对电影中的**每个时间点的时间类型**进行分类。

传统的神经网络应该很难来处理这个问题：使用电影中先前的事件推断后续的事件。循环神经网络RNN解决了这个问题。



通过上文第一节我们已经知道，RNN是包含循环的网络，在这个循环的结构中，每个神经网络的模块![img](https://julyedu-img.oss-cn-beijing.aliyuncs.com/quesbase64155706782035394808.svg)，读取某个输入![img](https://julyedu-img.oss-cn-beijing.aliyuncs.com/quesbase64155706782964392779.svg)，并输出一个值![img](https://julyedu-img.oss-cn-beijing.aliyuncs.com/quesbase64155706783840956477.svg)（注：输出之前由y表示，从此处起，改为隐层输出h表示)，然后不断循环。

循环可以使得信息可以从当前步传递到下一步。

![img](https://julyedu-img.oss-cn-beijing.aliyuncs.com/quesbase64155693928363631689.png)

这些循环使得RNN看起来非常神秘。

然而，如果你仔细想想，这样也不比一个正常的神经网络难于理解。

RNN可以被看做是**同一神经网络的多次复制**，每个神经网络模块会把==消息传递给下一个==。

所以，如果我们将这个循环展开：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210611174633.png" alt="img" style="zoom: 33%;" />

链式的特征揭示了RNN本质上是与序列和列表相关的。

他们是对于这类数据的最自然的神经网络架构。



## **1.3 RNN的局限：长期依赖（Long-TermDependencies）问题**

RNN的关键点之一就是他们可以用来连接先前的信息到当前的任务上，例如使用过去的视频段来推测对当前段的理解。

如果RNN可以做到这个，他们就变得非常有用。

但是真的可以么？答案是，还有很多依赖因素。

有时候，我们仅仅需要知道先前的信息来执行当前的任务。

例如，我们有一个语言模型用来基于先前的词来预测下一个词。

如果我们试着预测“the clouds are in the sky”最后的词，我们并不再需要其他的信息，因为很显然下一个词应该是sky。

在这样的场景中，相关的信息和预测的词位置之间的间隔是非常小的，RNN可以学会使用先前的信息。

![img](https://julyedu-img.oss-cn-beijing.aliyuncs.com/quesbase64155693965228582737.png)


但是同样会有一些更加复杂的场景。假设我们试着去预测“I grew up in France...I speak fluent French”最后的词。

当前的信息建议下一个词可能是一种语言的名字，但是如果我们需要弄清楚是什么语言，我们是需要先前提到的离当前位置**很远的France的上下文的**。

这说明相关信息和当前预测位置之间的间隔就肯定变得相当的大。

不幸的是，==在这个间隔不断增大时，RNN会丧失学习到连接如此远的信息的能力==。
![img](https://julyedu-img.oss-cn-beijing.aliyuncs.com/quesbase6415569396815538507.png)

在理论上，RNN绝对可以处理这样的长期依赖问题。

人们可以**仔细挑选参数**来解决这类问题中的最初级形式，但在实践中，RNN肯定不能够成功学习到这些知识。

==Bengio,etal.(1994)等人对该问题进行了深入的研究，他们发现一些使训练RNN变得非常困难的相当根本的原因。==



换句话说， RNN 会受到**短时记忆的影响**。

如果一条序列足够长，那它们将很难将信息从较早的时间步传送到后面的时间步。

因此，如果你正在尝试处理一段文本进行预测，RNN 可能从一开始就会遗漏重要信息。

在反向传播期间（[反向传播](https://www.julyedu.com/question/big/kp_id/26/ques_id/2921)是一个很重要的核心议题，本质是通过不断缩小误差去更新权值，从而不断去修正拟合的函数），RNN 会**面临梯度消失的问题**。

因为梯度是用于更新神经网络的权重值（新的权值 = 旧权值 - 学习率*梯度），梯度会随着时间的推移不断下降减少，而==当梯度值变得非常小时，就不会继续学习==。

![img](https://julyedu-img.oss-cn-beijing.aliyuncs.com/quesbase64155697973940593061.jpg)

换言之，在递归神经网络中，获得小梯度更新的层会停止学习—— 那些**通常是较早的层**。 

由于这些层不学习，RNN 可以忘记它在较长序列中看到的内容，因此具有短时记忆。

而**梯度爆炸则是因为计算的难度越来越复杂导致**。

然而，幸运的是，有个RNN的变体——LSTM，可以**在一定程度上**解决梯度消失和梯度爆炸这两个问题！

 

 

# **二、LSTM网络**
Long ShortTerm 网络——一般就叫做LSTM——是一种RNN特殊的类型，可以学习长期依赖信息。

当然，LSTM和RNN并没有特别大的结构不同，但是它们**用了不同的函数来计算隐状态**。

LSTM的“记忆”我们叫做细胞/cells，你可以直接把它们想做黑盒，这个黑盒的输入为前状态![img](https://julyedu-img.oss-cn-beijing.aliyuncs.com/quesbase64155697400547675284.png)和当前输入![img](https://julyedu-img.oss-cn-beijing.aliyuncs.com/quesbase64155697401981281809.png)。

**这些“细胞”会决定哪些之前的信息和状态需要保留/记住，而哪些要被抹去**。

实际的应用中发现，这种方式可以有效地保存很长时间之前的关联信息。





## **2.1 什么是LSTM网络**

举个例子，当你想在网上购买生活用品时，一般都会查看一下此前已购买该商品用户的评价。

![img](https://julyedu-img.oss-cn-beijing.aliyuncs.com/quesbase64155697981788721905.jpg)


当你浏览评论时，你的大脑下意识地只会记住重要的关键词，比如“amazing”和“awsome”这样的词汇，而不太会关心“this”、“give”、“all”、“should”等字样。

如果朋友第二天问你用户评价都说了什么，那你可能不会一字不漏地记住它，而是会说出但大脑里记得的主要观点，比如“下次肯定还会来买”，那**其他一些无关紧要的内容**自然会从记忆中逐渐消失。

![img](https://julyedu-img.oss-cn-beijing.aliyuncs.com/quesbase64155697985070108033.gif)

而这基本上就像是 LSTM 或 GRU 所做的那样，它们可以**学习只保留相关信息来进行预测**，并忘记不相关的数据。

简单说，因记忆能力有限，记住重要的，忘记无关紧要的。


LSTM由Hochreiter&Schmidhuber(1997)提出，并在近期被AlexGraves进行了改良和推广。

在很多问题，LSTM都取得相当巨大的成功，并得到了广泛的使用。
LSTM通过刻意的设计来避免长期依赖问题。

==记住长期的信息在实践中是LSTM的默认行为==，而非需要付出很大代价才能获得的能力！

所有RNN都具有一种重复神经网络模块的链式的形式。

在标准的RNN中，这个**重复的模块**只有一个非常简单的结构，例如一个tanh层。

![img](https://julyedu-img.oss-cn-beijing.aliyuncs.com/quesbase64155693985278218659.png)



==激活函数 Tanh 作用在于**帮助调节流经网络的值**，使得数值**始终限制在 -1 和 1 之间**==。💡

![img](https://julyedu-img.oss-cn-beijing.aliyuncs.com/quesbase64155697991770304386.gif)


LSTM同样是这样的结构，但是**重复的模块拥有一个不同的结构**。

具体来说，RNN是重复单一的神经网络层，LSTM中的==重复模块则包含四个交互的层，三个Sigmoid 和一个tanh层==，并以一种非常特殊的方式进行交互。

![img](https://julyedu-img.oss-cn-beijing.aliyuncs.com/quesbase64155693990149080320.png)



上图中，σ表示的Sigmoid 激活函数与 tanh 函数类似，不同之处在于 sigmoid 是把值压缩到0~1 之间而不是 -1~1 之间。



**这样的设置有助于更新或忘记信息**：

- 因为任何数乘以 0 都得 0，这部分信息就会剔除掉；
- 同样的，任何数乘以 1 都得到它本身，这部分信息就会完美地保存下来。

相当于要么是1则记住，要么是0则忘掉，所以还是这个原则：**因记忆能力有限，记住重要的，忘记无关紧要的**。

![img](https://julyedu-img.oss-cn-beijing.aliyuncs.com/quesbase64155698134018007950.gif)

此外，对于图中使用的各种元素的图标中，

- 每一条黑线传输着一整个向量，从一个节点的输出到其他节点的输入。

- 粉色的圈代表pointwise的操作，诸如向量的和，
- 而黄色的矩阵就是学习到的神经网络层。
- 合在一起的线表示向量的连接，分开的线表示内容被复制，然后分发到不同的位置。

![img](https://julyedu-img.oss-cn-beijing.aliyuncs.com/quesbase64155693992516615190.png)



## **2.2 LSTM的核心思想**
==LSTM的关键就是细胞状态，水平线在图上方贯穿运行==。

细胞状态类似于传送带。直接在整个链上运行，只有一些少量的线性交互。

信息在上面流传保持不变会很容易。

![img](https://julyedu-img.oss-cn-beijing.aliyuncs.com/quesbase64155694016793619292.png)

LSTM有通过**精心设计的称作为“门”的结构**来去除或者增加信息到细胞状态的能力。

门是一种让信息**选择式通过的方法**。他们包含一个sigmoid神经网络层和一个**pointwise乘法**的非线性操作。

如此，0代表“不许任何量通过”，1就指“允许任意量通过”！从而使得网络就能了解哪些数据是需要遗忘，哪些数据是需要保存。

![img](https://julyedu-img.oss-cn-beijing.aliyuncs.com/quesbase64155694019862092190.png)

LSTM拥有三种类型的门结构：**遗忘门/忘记门、输入门和输出门**，来保护和控制细胞状态。

 


# **三、逐步理解LSTM**
## **3.1 忘记门**

在我们LSTM中的第一步是决定我们会从细胞状态中丢弃什么信息。

这个决定通过一个称为“忘记门”的结构完成。

- 该忘记门会读取上一个输出![img](https://julyedu-img.oss-cn-beijing.aliyuncs.com/quesbase64155706754786217349.svg)和当前输入![img](https://julyedu-img.oss-cn-beijing.aliyuncs.com/quesbase64155706756757918470.svg)，**做一个Sigmoid 的非线性映射**，

- 然后输出一个向量![f_{t}](https://private.codecogs.com/gif.latex?f_%7Bt%7D)（该向量每一个维度的值都在0到1之间，1表示完全保留，0表示完全舍弃，相当于记住了重要的，忘记了无关紧要的），
- 最后**与细胞状态**![img](https://julyedu-img.oss-cn-beijing.aliyuncs.com/quesbase64155706757691155460.svg)相乘。

类比到语言模型的例子中，则是**基于已经看到的**，来预测下一个词。

在这个问题中，==**细胞状态可能包含当前主语的性别**，因此正确的代词可以被选择出来==。

当我们看到新的主语，我们希望**忘记旧的主语，进而决定丢弃信息**。

![img](https://julyedu-img.oss-cn-beijing.aliyuncs.com/quesbase64155694024495901500.png)

大部分初学的读者看到这，可能会有所懵逼，没关系，我们分以下两个步骤理解：

1. 对于上图右侧公式中的权值![W_{f}](https://private.codecogs.com/gif.latex?W_%7Bf%7D)，**准确的说其实是不共享，即是不一样的**。即：![f_{t} = \sigma (W_{fh}h_{t-1} + W_{fx} x_{t} + b_{f})](https://private.codecogs.com/gif.latex?f_%7Bt%7D%20%3D%20%5Csigma%20%28W_%7Bfh%7Dh_%7Bt-1%7D%20&plus;%20W_%7Bfx%7D%20x_%7Bt%7D%20&plus;%20b_%7Bf%7D%29)。

2. 至于右侧公式和左侧的图是怎样的一个一一对应关系呢？

   如果是用有方向的水流表示计算过程则将一目了然，上动图！红圈表示Sigmoid 激活函数，篮圈表示tanh 函数：

![img](https://julyedu-img.oss-cn-beijing.aliyuncs.com/quesbase64155698137271488143.gif)


## **3.2 输入门**

下一步是确定**什么样的新信息被存放在细胞状态中**。

这里包含两个部分：

- 第一，sigmoid层称“输入门层”决定什么值我们将要更新；
- 第二，==一个tanh层**创建一个新的候选值向量**![img](https://julyedu-img.oss-cn-beijing.aliyuncs.com/quesbase64155706765888087563.svg)，会被加入到状态中==。

下一步，我们会讲这两个信息来产生对状态的更新。

在我们语言模型的例子中，我们希望增加新的主语的性别到细胞状态中，来替代旧的需要忘记的主语，进而确定更新的信息。

![img](https://julyedu-img.oss-cn-beijing.aliyuncs.com/quesbase64155694032335903733.png)

继续分两个步骤来理解：

1. 首先，为便于理解图中右侧的两个公式，我们展开下计算过程，即![i_{t} = \sigma (W_{ih}h_{t-1} + W_{ix}x_{t} + b_{i})](https://private.codecogs.com/gif.latex?i_%7Bt%7D%20%3D%20%5Csigma%20%28W_%7Bih%7Dh_%7Bt-1%7D%20&plus;%20W_%7Bix%7Dx_%7Bt%7D%20&plus;%20b_%7Bi%7D%29)、![\tilde{C_{t}} = tanh(W_{Ch}h_{t-1} + W_{Cx}x_{t} + b_{C})](https://private.codecogs.com/gif.latex?%5Ctilde%7BC_%7Bt%7D%7D%20%3D%20tanh%28W_%7BCh%7Dh_%7Bt-1%7D%20&plus;%20W_%7BCx%7Dx_%7Bt%7D%20&plus;%20b_%7BC%7D%29)
2. 其次，上动图！

![img](https://julyedu-img.oss-cn-beijing.aliyuncs.com/quesbase641556981402794253.gif)


## **3.3 细胞状态**

现在是更新旧细胞状态的时间了，![img](https://julyedu-img.oss-cn-beijing.aliyuncs.com/quesbase64155706769058219176.svg)更新为![img](https://julyedu-img.oss-cn-beijing.aliyuncs.com/quesbase64155706770091173917.svg)。

前面的步骤已经决定了将会做什么，我们现在就是实际去完成。

- 我们把旧状态与![img](https://julyedu-img.oss-cn-beijing.aliyuncs.com/quesbase6415570677073428868.svg)相乘，==丢弃掉我们确定需要丢弃的信息==(与0相乘)。

- 接着加上![img](https://julyedu-img.oss-cn-beijing.aliyuncs.com/quesbase64155706771729335767.svg)。这就是**新的候选值**，根据我们决定更新每个状态的程度进行变化。

在语言模型的例子中，这就是我们实际根据前面确定的目标，丢弃旧代词的性别信息并添加新的信息的地方，类似更新细胞状态。

![img](https://julyedu-img.oss-cn-beijing.aliyuncs.com/quesbase64155694035490626607.png)

再次动图！

![img](https://julyedu-img.oss-cn-beijing.aliyuncs.com/quesbase64155698143296475738.gif)


## **3.4 输出门**

最终，我们需要确定输出什么值。

这个输出将会==基于我们的细胞状态，但是也是一个过滤后的版本==。

首先，我们**运行一个sigmoid层来确定细胞状态的哪个部分将输出出去**。

接着，我们把细胞状态通过tanh进行处理（得到一个在-1到1之间的值）并将它和sigmoid门的输出相乘，最终我们仅仅会输出**我们确定输出的那部分**。

==在语言模型的例子中，因为他就看到了一个代词，可能需要输出与一个动词相关的信息==。

例如，可能输出是否代词是单数还是负数，这样如果是动词的话，我们**也知道动词需要进行的词形变化**，进而输出信息。

![img](https://julyedu-img.oss-cn-beijing.aliyuncs.com/quesbase6415569403763141128.png)

依然分两个步骤来理解：

1. 展开图中右侧第一个公式，![o_{t} = \sigma (W_{oh}h_{t-1} + W_{ox}x_{t} + b_{o})](https://private.codecogs.com/gif.latex?o_%7Bt%7D%20%3D%20%5Csigma%20%28W_%7Boh%7Dh_%7Bt-1%7D%20&plus;%20W_%7Box%7Dx_%7Bt%7D%20&plus;%20b_%7Bo%7D%29)
2. 最后一个动图：

![img](https://julyedu-img.oss-cn-beijing.aliyuncs.com/quesbase6415569814619904217.gif)

 

## me|小结

sigmoid用来筛选

tanh用来映射




# **四、LSTM的变体**

我们到目前为止都还在介绍正常的LSTM。

但是不是所有的LSTM都长成一个样子的。

实际上，几乎所有包含LSTM的论文都采用了**微小的变体**，差异非常小，但是也值得拿出来讲一下。



## **4.1 peephole连接与coupled**
其中一个**流形的**LSTM变体，就是由Gers&Schmidhuber(2000)提出的，增加了“peephole connection”。

是说，我们让**门层也会接受细胞状态的输入**。



peephole连接：

![img](https://julyedu-img.oss-cn-beijing.aliyuncs.com/quesbase64155694039965747176.png)





上面的图例中，我们增加了peephole到每个门上，但是许多论文会**加入部分的peephole**，而非所有都加。



另一个变体是通过使用**coupled忘记和输入门**。

不同于之前是分开确定什么忘记和需要添加什么新的信息，这里是==一同做出决定==。

- 当我们将要输入在当前位置时，我们只在这个时候忘记。 => 基于新的输入，忘掉旧的记忆

- 我们仅仅输入新的值，到那些我们已经忘记旧的信息的那些状态。 => 新的输入，填补空缺记忆

![img](https://julyedu-img.oss-cn-beijing.aliyuncs.com/quesbase64155694042616920401.png)

 

## **4.2 GRU**

另一个改动较大的变体是Gated Recurrent Unit(GRU)，这是由Cho,etal.(2014)提出。

它==将忘记门和输入门合成了一个单一的更新门==。

同样还==混合了细胞状态和隐藏状态$h_i$==，和其他一些改动。

最终的模型比标准的LSTM模型要简单，也是非常流行的变体。

![img](https://julyedu-img.oss-cn-beijing.aliyuncs.com/quesbase64155694047372134166.png)

为了便于理解，我把上图右侧中的前三个公式展开一下

1. ![z_{t} = \sigma (W_{zh}h_{t-1} + W_{zx}x_{t})](https://private.codecogs.com/gif.latex?z_%7Bt%7D%20%3D%20%5Csigma%20%28W_%7Bzh%7Dh_%7Bt-1%7D%20&plus;%20W_%7Bzx%7Dx_%7Bt%7D%29)
2. ![r_{t} = \sigma (W_{rh}h_{t-1} + W_{rx}x_{t})](https://private.codecogs.com/gif.latex?r_%7Bt%7D%20%3D%20%5Csigma%20%28W_%7Brh%7Dh_%7Bt-1%7D%20&plus;%20W_%7Brx%7Dx_%7Bt%7D%29)
3. ![\tilde{h} = tanh(W_{rh}(r_{t}h_{t-1}) + W_{x}x_{t})](https://private.codecogs.com/gif.latex?%5Ctilde%7Bh%7D%20%3D%20tanh%28W_%7Brh%7D%28r_%7Bt%7Dh_%7Bt-1%7D%29%20&plus;%20W_%7Bx%7Dx_%7Bt%7D%29)

这里面有个小问题，眼尖的同学可能看到了，![z_{t}](https://private.codecogs.com/gif.latex?z_%7Bt%7D)和![r_{t}](https://private.codecogs.com/gif.latex?r_%7Bt%7D)都是对![h_{t-1}](https://private.codecogs.com/gif.latex?h_%7Bt-1%7D)、![x_{t}](https://private.codecogs.com/gif.latex?x_%7Bt%7D)做的Sigmoid非线性映射，那区别在哪呢？

原因在于GRU**把忘记门和输入门合二为一了**，而![z_{t}](https://private.codecogs.com/gif.latex?z_%7Bt%7D)是属于要记住的，反过来![1- z_{t}](https://private.codecogs.com/gif.latex?1-%20z_%7Bt%7D)则是属于忘记的，相当于对输入![h_{t-1}](https://private.codecogs.com/gif.latex?h_%7Bt-1%7D)、![x_{t}](https://private.codecogs.com/gif.latex?x_%7Bt%7D)做了一些更改/变化，而![r_{t}](https://private.codecogs.com/gif.latex?r_%7Bt%7D)则相当于先见之明的把输入![h_{t-1}](https://private.codecogs.com/gif.latex?h_%7Bt-1%7D)、![x_{t}](https://private.codecogs.com/gif.latex?x_%7Bt%7D)在![z_{t}](https://private.codecogs.com/gif.latex?z_%7Bt%7D)/![1- z_{t}](https://private.codecogs.com/gif.latex?1-%20z_%7Bt%7D)对其做更改/变化之前，**先事先存一份原始的**，最终在![\tilde{h}](https://private.codecogs.com/gif.latex?%5Ctilde%7Bh%7D)那做一个tanh变化。

这里只是部分流行的LSTM变体。



当然还有很多其他的，如Yao,etal.(2015)提出的DepthGated RNN。

还有用一些完全不同的观点**来解决长期依赖的问题**，如Koutnik,etal.(2014)提出的Clockwork RNN。

要问哪个变体是最好的？其中的差异性真的重要吗？==Greff,etal.(2015)给出了流行变体的比较，结论是他们基本上是一样的==。

Jozefowicz,etal.(2015)则在**超过1万种RNN架构上进行了测试**，发现一些架构在某些任务上也取得了比LSTM更好的结果。

 

 

# **五、LSTM相关面试题**

为帮助大家巩固以上的所学，且助力找AI工作的朋友，特从七月在线AI题库里抽取以下关于LSTM的典型面试题

更具体的答案参见：https://www.julyedu.com/search?words=LSTM（打开链接后，勾选“面试题”的tab）

1 [LSTM结构推导，为什么比RNN好？](https://www.julyedu.com/question/big/kp_id/26/ques_id/1003)

- [梯度消失(W<1)/爆炸(W>1)](https://www.bilibili.com/video/BV1Vx411j7xF?from=search&seid=7818633533404324294)
  - <img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210612105228.png" alt="image-20210612105226566" style="zoom: 33%;" />

[GRU是什么？GRU对LSTM做了哪些改动？](https://www.julyedu.com/question/big/kp_id/26/ques_id/2114)

[LSTM神经网络输入、输出究竟是怎样的？](https://www.julyedu.com/question/big/kp_id/26/ques_id/1715)

[为什么LSTM模型中既存在sigmoid又存在tanh两种激活函数，而不是选择统一一种sigmoid或者tanh？这样做的目的是什么？](https://www.julyedu.com/question/big/kp_id/26/ques_id/1048)

[如何修复梯度爆炸问题？](https://www.julyedu.com/question/big/kp_id/26/ques_id/1714)

[如何解决RNN梯度爆炸和弥散的问题？](https://www.julyedu.com/question/big/kp_id/26/ques_id/1049)

 

# me|理解

![enter image description here](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210601213321.png)

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210612105332.png" alt="image-20210612105330167" style="zoom:50%;" />

两张图的对应关系：

- 主线对应细胞状态 $C_{t-1}、C_{t}$
- 输入对应$x_t$
- 输出对应$h_t$
  - $h_{t-1}$是上一次的输出：遗忘次要信息，记住重点信息 => 参与提供输入



现在梳理一下对RNN和LSTM的理解：

LSTM它由三个门：

- 首先是忘记门，基于sigmoid函数，选择重要的信息。
  - 这个重要的信息就相当于是RNN里面，靠前的重要的信息
- 而后在输入门中
  - 并行通道之一：还是忘记次要的信息，选择记住重要的信息
  - 另外它的一个正常的tanh方法，保证了输出限制在-1到+1之间。
  - 两者结合，使得重要的信息放大，不重要的信息全都变为0。
  - 而后两者相加，更新了细胞状态：细胞状态相当于记录了重要的信息
- 最后在输出的过程中
  - 一方面又提炼了一遍重要的信息
  - 另一方面根据细胞状态进行了一个加权，最后得到一个输出，同时也保留更新的细胞状态



实际应用过程中：权重/参数的更新依然基于NN的梯度下降



 

# **#参考文献**

1. ChristopherOlah的博文《[理解LSTM网络](https://colah.github.io/posts/2015-08-Understanding-LSTMs/)》
2. @Not_GOD 翻译ChristopherOlah的博文《[理解LSTM网络](https://www.jianshu.com/p/9dc9f41f0b29/)》
3. [RNN是怎么从单层网络一步一步构造的？](https://www.julyedu.com/question/big/kp_id/26/ques_id/1717)
4. [通过一张张动图形象的理解LSTM](https://www.julyedu.com/question/big/kp_id/26/ques_id/2920)
5. [如何理解LSTM网络（超经典的Christopher Olah的博文之July注解版）](https://www.julyedu.com/question/big/kp_id/26/ques_id/1851)
6. LSTM相关的典型面试题：https://www.julyedu.com/search?words=LSTM（打开链接后，勾选“面试题”的tab）
7. [如何理解反向传播算法BackPropagation](https://www.julyedu.com/question/big/kp_id/26/ques_id/2921)





# 补充|[LSTM通俗理解](https://www.bilibili.com/video/BV1o4411R7B1?p=2)

## 1 RNN的结构缺陷

RNN结构共享一组参数(U, W, b) => 梯度爆炸/梯度消失



## 2 LSTM结构

内部结构：神经元的计算公式变了 => 外部结构没有任何变化

- 结论：之前提及的RNN各种结构，都能用LSTM来替换

![enter image description here](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210601213321.png)

1 遗忘门forgot

控制**输入($x_t$)和上一层隐藏层输出($h_{t-1}$)** 被遗忘的程度大小



2 输入门input

控制**输入($x_t$)和当前计算的状态$\tilde{C_t}= tanh(\cdot) $** 更新到记忆单元的程度大小



3 内部记忆单元

$C_{t-1}, i_t, \tilde{C_t} \rightarrow C_t$



4 输出门output

控制**输入($x_t$)**和**当前输出取决于当前记忆单元的程度大小$tanh(\cdot)$**



@公式参数解释：

![image-20210612221407729](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210612221408.png)



小结：

- 下述值接近1或者0：与==各自神经元的权重(U W b)有关== => 识别出输入序列的重要信息
  - U是隐藏层状态h的权重参数
  - W是输入x的权重参数

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210612221546.png" alt="image-20210612221546248" style="zoom:80%;" />





# 补充|LSTM review

背景：人类记忆遗忘机制

- 输入门 => 当前/短期记忆
- 遗忘门 => 并不全都是遗忘，而是针对长期记忆，有选择性地遗忘，同时也意味着有选择性的保留一些重要的长期记忆

![image-20210613100522446](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210613100523.png)





变式：改变上图中“小盒子”的内部结构

- MGU：minimal gated unit
- SRU：simple recurrent unit

- GRU: Gated recurrent unit
  - 更新门：遗忘门 + 输入门 => 决定丢弃哪些旧信息，添加哪些新信息
  - 重置门：决定写入多少上一时刻，网络的状态 => 捕捉短期记忆

![image-20210613151624674](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210613151625.png)

















