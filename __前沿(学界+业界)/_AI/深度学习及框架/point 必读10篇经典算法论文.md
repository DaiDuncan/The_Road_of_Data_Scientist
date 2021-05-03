[link: 知乎专栏|深度学习必读10篇经典算法论文总结！2020.08.11](https://zhuanlan.zhihu.com/p/173776547)

前言

计算机视觉是将**图像和视频**转换成机器可理解的信号的主题。

利用这些信号，程序员可以基于这种高级理解来进一步控制机器的行为。

在许多计算机视觉任务中，图像分类是最基本的任务之一。

它不仅可以用于许多实际产品中，例如Google Photo的标签和AI内容审核，而且还为许多更高级的视觉任务（例如物体检测和视频理解）打开了一扇门。

自从深度学习的突破以来，由于该领域的快速变化，初学者经常发现它太笨拙，无法学习。

与典型的软件工程学科不同，没有很多关于使用DCNN进行图像分类的书籍，而了解该领域的最佳方法是阅读学术论文。

但是要读什么论文？我从哪说起呢？在本文中，我将介绍10篇最佳论文供初学者阅读。

通过这些论文，我们可以看到该领域是如何发展的，以及研究人员如何根据以前的研究成果提出新的想法。

但是，即使您已经在此领域工作了一段时间，对您进行大范围整理仍然很有帮助。



## 1998年：LeNet

**梯度学习在于文档识别中的应用**

![img](https://pic3.zhimg.com/80/v2-8402cb952255da5a414fb0659712f572_720w.jpg)

摘自“ [基于梯度的学习应用于文档识别](https://link.zhihu.com/?target=http%3A//yann.lecun.com/exdb/publis/pdf/lecun-01a.pdf)”

LeNet于1998年推出，为使用卷积神经网络进行未来图像分类研究奠定了基础。

许多经典的CNN技术（例如池化层，完全连接的层，填充和激活层）用于提取特征并进行分类。

借助**均方误差损失**功能和20个训练周期，该网络在MNIST测试集上可以达到99.05％的精度。

即使经过20年，仍然有许多最先进的分类网络总体上遵循这种模式。



## 2012年：AlexNet

**深度卷积神经网络的ImageNet分类**

![img](https://pic1.zhimg.com/80/v2-0891cd2223cc568bca6d24c1ae6b6198_720w.jpg)

摘自“ [具有深度卷积神经网络的ImageNet分类](https://link.zhihu.com/?target=https%3A//papers.nips.cc/paper/4824-imagenet-classification-with-deep-convolutional-neural-networks.pdf)”

尽管LeNet取得了不错的成绩并显示了CNN的潜力，但由于计算能力和数据量有限，该领域的发展停滞了十年。

看起来CNN只能解决一些简单的任务，例如数字识别，但是对于更复杂的特征（如人脸和物体），带有SVM分类器的HarrCascade或SIFT特征提取器是更可取的方法。



但是，在2012年ImageNet大规模视觉识别挑战赛中，Alex Krizhevsky提出了**基于CNN的解决方案**来应对这一挑战，并将ImageNet测试装置的top-5准确性从73.8％大幅提高到84.7％。

他们的方法继承了LeNet的多层CNN想法，但是大大增加了CNN的大小。

从上图可以看到，与LeNet的32x32相比，现在的输入为224x224，与LeNet的6相比，许多卷积内核具有192个通道。

尽管设计变化不大，但**参数变化了数百次**，但网络的捕获和表示复杂特征的能力也提高了数百倍。



为了进行大型模型训练，Alex使用了两个具有3GB RAM的GTX 580 GPU，这开创了GPU训练的先河。

同样，使用**ReLU非线性也有助于降低计算成本**。

除了为网络带来更多参数外，它还通过使用 **Dropout层探讨了大型网络带来的过拟合问题** 。

其局部响应归一化方法此后并没有获得太大的普及，但是**启发了其他重要的归一化技术（例如BatchNorm）来解决梯度饱和问题**。

综上所述，AlexNet定义了未来十年的实际分类网络框架： **卷积，ReLu非线性激活，MaxPooling和Dense层的组合。**





## 2014年：VGG

**超深度卷积网络用于大规模图像识别**



![img](https://pic1.zhimg.com/80/v2-22013bb48964a0420f803f28aec9e264_720w.jpg)





来自Quora“ [https://www.quora.com/What-is-the-VGG-neural-network](https://link.zhihu.com/?target=https%3A//www.quora.com/What-is-the-VGG-neural-network)”



在使用CNN进行视觉识别方面取得了巨大成功，整个研究界都大吃一惊，所有人都开始研究为什么这种神经网络能够如此出色地工作。

例如，在2013年发表的“可视化和理解卷积网络”中，Matthew Zeiler讨论了CNN如何获取特征并可视化中间表示。

突然之间，每个人都开始意识到**CNN自2014年以来就是计算机视觉的未来**。

在所有直接关注者中，Visual Geometry Group的VGG网络是最吸引眼球的网络。

在ImageNet测试仪上，它的top-5准确度达到93.2％，top-1准确度达到了76.3％。



遵循AlexNet的设计，VGG网络有两个主要更新： 

**1）VGG不仅使用了像AlexNet这样的更广泛的网络，而且使用了更深的网络。VGG-19具有19个卷积层，而AlexNet中只有5个。**

**2）VGG还展示了一些小的3x3卷积滤波器可以代替AlexNet的单个7x7甚至11x11滤波器，在降低计算成本的同时实现更好的性能。** 

由于这种优雅的设计，VGG也成为了其他计算机视觉任务中许多开拓性网络的骨干网络，例如用于语义分割的FCN和用于对象检测的Faster R-CNN。



随着网络的深入，从多层反向传播中梯度消失成为一个更大的问题。

为了解决这个问题，VGG还讨论了**预训练和权重初始化**的重要性。

这个问题限制了研究人员继续添加更多的层，否则，网络将很难融合。

但是两年后，我们将为此找到更好的解决方案。



## 2014年：GoogLeNet

**更深卷积**





![img](https://pic1.zhimg.com/80/v2-c9dff97ef8ab45b218e4723e9e656964_720w.jpg)



摘自“ [Going Deeper with Convolutions](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/1409.4842)”

VGG具有漂亮的外观和易于理解的结构，但在ImageNet 2014竞赛的所有决赛入围者中表现都不佳。

GoogLeNet（又名InceptionV1）获得了最终奖。

就像VGG一样，GoogLeNet的主要贡献之一就是 **采用22层结构来突破网络深度的限制** 。

这再次证明，进一步深入确实是提高准确性的正确方向。



与VGG不同，GoogLeNet试图直接解决计算和梯度递减问题，而不是提出具有更好的预训练模式和权重初始化的解决方法。



![img](https://pic2.zhimg.com/80/v2-89be2bb440ae1e4595634a491ac64765_720w.jpg)





Bottleneck Inception Module From “ [Going Deeper with Convolutions](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/1409.4842)”



首先，它 **使用称为Inception的模块探索了非对称网络设计的思想** （请参见上图）。

理想情况下，他们希望采用稀疏卷积或密集层来提高特征效率，但是现代硬件设计并非针对这种情况。

因此，他们认为，网络拓扑级别的稀疏性还可以在利用现有硬件功能的同时，帮助融合功能。



其次，它通过借鉴论文“网络中的网络”来解决高计算成本的问题。

基本上， **引入1x1卷积滤波器以在进行繁重的计算操作（如5x5卷积内核）之前减小特征的尺寸** 。

以后将该结构称为“ **Bottleneck** ”，并在许多后续网络中广泛使用。

类似于“网络中的网络”，它还使用平均池层代替最终的完全连接层，以进一步降低成本。



第三，为了帮助梯度流向更深的层次，GoogLeNet还对某些中间层输出或辅助输出使用了监督。

由于其复杂性，该设计后来在图像分类网络中并不十分流行，但是在计算机视觉的其他领域（如Hourglass网络）的姿势估计中越来越流行。



作为后续行动，这个Google团队为此Inception系列写了更多论文。

“批处理规范化：通过减少内部协变量偏移来加速深度网络训练”代表 **InceptionV2** 。

2015年的“重新思考计算机视觉的Inception架构”代表 **InceptionV3** 。

2015年的“ Inception-v4，Inception-ResNet和残余连接对学习的影响”代表 **InceptionV4** 。

每篇论文都对原始的Inception网络进行了更多改进，并取得了更好的效果。



## 2015年：Batch Normalization

**批处理规范化：通过减少内部协变量偏移来加速深度网络训练**



初始网络帮助研究人员在ImageNet数据集上达到了超人的准确性。

但是，作为一种统计学习方法， **CNN非常受特定训练数据集的统计性质的限制** 。

因此，为了获得更高的准确性，我们通常需要预先计算整个数据集的平均值和标准偏差，并使用它们首先对我们的输入进行归一化，以确保网络中的大多数层输入都紧密，从而转化为更好的激活响应能力。

这种近似方法非常麻烦，有时对于新的网络结构或新的数据集根本不起作用，因此深度学习模型仍然被认为很难训练。

为了解决这个问题，创建GoogLeNet的人Sergey Ioffe和Chritian Szegedy决定发明一种更聪明的东西，称为“ **批量标准化** ”。

![img](https://pic4.zhimg.com/80/v2-cff9bb4e1c952af423da0e8a0189986b_720w.jpg)





摘自“ [批量标准化：通过减少内部协变量偏移来加速深度网络训练](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/1502.03167)”



批量规范化的想法并不难：只要训练足够长的时间，我们就可以使用**一系列小批量的统计数据**来近似整个数据集的统计数据。

而且，代替手动计算统计信息，我们可以引入两个更多可学习的参数 **“缩放”** 和 **“移位”** ，以使网络学习如何单独对每一层进行规范化。



上图显示了计算批次归一化值的过程。

如我们所见，我们取整个小批量的平均值，并计算方差。

接下来，我们可以使用此最小批量均值和方差对输入进行归一化。

最后，通过比例尺和位移参数，网络将学会调整批标准化结果以最适合下一层，通常是ReLU。

一个警告是我们在推理期间没有小批量信息，因此一种解决方法是在训练期间计算移动平均值和方差，然后在推理路径中使用这些移动平均值。

这项小小的创新是如此具有影响力，所有后来的网络都立即开始使用它。



## 2015年：ResNet

**深度残差学习用于图像识别**



2015年可能是十年来计算机视觉最好的一年，我们已经看到很多伟大的想法不仅出现在图像分类中，而且还出现了各种各样的计算机视觉任务，例如对象检测，语义分割等。 

2015年属于一个名为ResNet或残差网络的新网络，该网络由Microsoft Research Asia的一组中国研究人员提出。



![img](https://pic3.zhimg.com/80/v2-fd4fc871eeaaec64e59a2012ca7fc022_720w.jpg)





摘自“ [用于图像识别的深度残差学习](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/1512.03385)”

正如我们之前在VGG网络中所讨论的，要变得更深，**最大的障碍是梯度消失问题**，即，当通过更深的层向后传播时，导数会越来越小，最终达到现代计算机体系结构无法真正代表的地步有意义地。

GoogLeNet尝试通过使用辅助监管和非对称启动模块来对此进行攻击，但只能在较小程度上缓解该问题。

如果我们要使用50甚至100层，是否会有更好的方法让渐变流过网络？ResNet的答案是使用残差模块。



![img](https://pic3.zhimg.com/80/v2-3c47f1e3b9a9cd0ee18c3974fa19212e_720w.jpg)





剩余的模块从“ [深残余学习图像识别](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/1512.03385)”

ResNet在输出中添加了身份标识快捷方式，因此每个残差模块至少都不能预测输入是什么，而不会迷失方向。

更为重要的是，残差模块不是希望每个图层都直接适合所需的特征映射，而是尝试了解输出和输入之间的差异，这使任务变得更加容易，因为所需的信息增益较小。

想象一下，您正在学习数学，对于每个新问题，都将得到一个类似问题的解决方案，因此您所要做的就是扩展此解决方案并使其起作用。

这比为您遇到的每个问题想出一个全新的解决方案要容易得多。或者像牛顿所说，我们可以站在巨人的肩膀上，身份输入就是剩余模块的那个巨人。



除了身份映射，ResNet还从Inception网络借用了**瓶颈和批处理规范化**。

最终，它成功构建了具有152个卷积层的网络，并在ImageNet上实现了80.72％的top-1准确性。

剩余方法也成为后来的许多其他网络（例如Xception，Darknet等）的默认选项。

此外，由于其简单美观的设计，如今它仍广泛用于许多生产视觉识别系统中。



通过追踪残差网络的炒作，还有更多不变式出现。

在“深层残差网络中的身份映射”中，ResNet的原始作者试图将激活放在残差模块之前，并获得了更好的结果，此设计此后称为ResNetV2。

同样，在2016年的论文《深度神经网络的聚合残差变换》中，研究人员提出了ResNeXt，该模型为残差模块添加了并行分支，以汇总不同变换的输出。





## 2016年：Xception

**Xception：深度学习与深度可分卷积**





![img](https://pic1.zhimg.com/80/v2-0cf7dfb8837f065a01a0516c67e18f98_720w.jpg)





摘自“ [Xception：深度学习与深度可分卷积](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/1610.02357)”

随着ResNet的发布，图像分类器中大多数低挂的水果看起来已经被抢走了。

研究人员开始考虑**CNN魔术的内部机制**是什么。由于跨通道卷积通常会引入大量参数，因此Xception网络选择调查此操作以了解其效果的全貌。



就像它的名字一样，Xception源自Inception网络。

在Inception模块中，将不同转换的多个分支聚合在一起以实现拓扑稀疏性。但是为什么这种稀疏起作用了？

Xception的作者，也是Keras框架的作者，将此想法扩展到了一种极端情况，在这种情况下，一个3x3卷积文件对应于最后一个串联之前的一个输出通道。

在这种情况下，这些并行卷积内核实际上形成了一个称为深度卷积的新操作。

![img](https://pic3.zhimg.com/80/v2-813e5835e7c33231ccdb2fb99384caee_720w.jpg)





摘自“ [深度卷积和深度可分离卷积](https://link.zhihu.com/?target=https%3A//medium.com/%40zurister/depth-wise-convolution-and-depth-wise-separable-convolution-37346565d4ec)”

如上图所示，与传统卷积不同，传统卷积包括所有通道以进行一次计算，深度卷积仅分别计算每个通道的卷积，然后将输出串联在一起。

这减少了通道之间的特征交换，但也减少了很多连接，因此导致具有较少参数的层。

但是，此操作将输出与输入相同数量的通道（如果将两个或多个通道组合在一起，则输出的通道数量将减少）。

因此，一旦合并了通道输出，就需要另一个常规1x1滤波器或逐点卷积，以增加或减少通道数，就像常规卷积一样。



这个想法最初不是来自Xception。在名为“大规模学习视觉表示”的论文中对此进行了描述，并且在InceptionV2中偶尔使用。

Xception进一步迈出了一步，并用这种新型卷积代替了几乎所有的卷积。实验结果非常好。它超越了ResNet和InceptionV3，成为用于图像分类的新SOTA方法。这也证明了CNN中跨通道相关性和空间相关性的映射可以完全解耦。

此外，由于与ResNet具有相同的优点，Xception也具有简单美观的设计，因此其思想还用于随后的许多其他研究中，例如MobileNet，DeepLabV3等。



## 2017年：MobileNet

**MobileNets：用于移动视觉应用的高效卷积神经网络**



Xception在ImageNet上实现了79％的top-1准确性和94.5％的top-5准确性，但是与以前的SOTA InceptionV3相比分别仅提高了0.8％和0.4％。

新图像分类网络的边际收益越来越小，因此研究人员开始将注意力转移到其他领域。

在资源受限的环境中，MobileNet推动了图像分类的重大发展。



(原图不存在)

![img](https://pic2.zhimg.com/80/undefined_720w.jpg)



“ MobileNets：针对移动视觉应用的高效卷积神经网络”中的MobileNet模块



与Xception相似，MobileNet使用与上面所示相同的深度可分离卷积模块，并着重于高效和较少参数。





![img](https://pic4.zhimg.com/80/v2-5a57f2c365764c64daac02cd690a2d13_720w.jpg)





“ MobileNets：用于移动视觉应用的高效卷积神经网络”中的参数比率



上式中的分子是深度可分离卷积所需的参数总数。分母是相似的规则卷积的参数总数。这里D [K]是卷积核的大小，D [F]是特征图的大小，M是输入通道数，N是输出通道数。由于我们将通道和空间特征的计算分开了，因此我们可以将乘法转换为相加，其量级较小。从该比率可以看出，更好的是，输出通道数越多，使用该新卷积节省的计算量就越多。



MobileNet的另一个贡献是宽度和分辨率乘数。MobileNet团队希望找到一种规范的方法来缩小移动设备的模型大小，而最直观的方法是减少输入和输出通道的数量以及输入图像的分辨率。为了控制此行为，比率alpha乘以通道，比率rho乘以输入分辨率（这也会影响要素图的大小）。因此，参数总数可以用以下公式表示：





![img](https://pic3.zhimg.com/80/undefined_720w.jpg)





“ MobileNets：用于移动视觉应用的高效卷积神经网络”



尽管这种变化在创新方面看似天真，但它具有巨大的工程价值，因为这是研究人员首次得出结论，可以针对不同的资源约束调整网络的规范方法。此外，它还总结了改进神经网络的最终解决方案：更大和更高的分辨率输入会导致更高的精度，更薄和更低的分辨率输入会导致更差的精度。



在2018年和2019年晚些时候，MobiletNet团队还发布了“ MobileNetV2：残差和线性瓶颈”和“搜索MobileNetV3”。在MobileNetV2中，使用了倒置的残留瓶颈结构。在MobileNetV3中，它开始使用神经体系结构搜索技术来搜索最佳体系结构组合，我们将在后面介绍。





## 2017年：NASNet

**学习可扩展的体系结构以实现可扩展的图像识别**



就像针对资源受限环境的图像分类一样，神经体系结构搜索是在2017年左右出现的另一个领域。借助ResNet，Inception和Xception，似乎我们已经达到了人类可以理解和设计的最佳网络拓扑，但是如果有的话一个更好，更复杂的组合，远远超出了人类的想象力？2016年的一篇论文《带有强化学习的神经体系结构搜索》提出了一种通过强化学习在预定搜索空间内搜索最佳组合的想法。众所周知，强化学习是一种以目标明确，奖励搜索代理商的最佳解决方案的方法。但是，受计算能力的限制，本文仅讨论了在小型CIFAR数据集中的应用。

![img](https://pic2.zhimg.com/80/v2-eb5da52378b441b8a24c4a4a2d2113ad_720w.jpg)





NASNet搜索空间。“ [学习可扩展的体系结构以实现可扩展的图像识别](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/1707.07012)”



为了找到像ImageNet这样的大型数据集的最佳结构，NASNet创建了针对ImageNet量身定制的搜索空间。它希望设计一个特殊的搜索空间，以便CIFAR上的搜索结果也可以在ImageNet上正常工作。首先，NASNet假设在良好的网络（如ResNet和Xception）中常用的手工模块在搜索时仍然有用。因此，NASNet不再搜索随机连接和操作，而是搜索这些模块的组合，这些模块已被证明在ImageNet上已经有用。其次，实际搜索仍在32x32分辨率的CIFAR数据集上执行，因此NASNet仅搜索不受输入大小影响的模块。为了使第二点起作用，NASNet预定义了两种类型的模块模板：Reduction和Normal。

![img](https://pic3.zhimg.com/80/v2-4f6bb0f1091fb973ea20f455d7cdf322_720w.jpg)


摘自“ [学习可扩展的体系结构以实现可伸缩的图像识别](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/1707.07012)”



尽管NASNet具有比手动设计网络更好的度量标准，但是它也有一些缺点。寻找最佳结构的成本非常高，只有像Google和Facebook这样的大公司才能负担得起。而且，最终结构对人类来说并没有太大意义，因此在生产环境中难以维护和改进。在2018年晚些时候，“ MnasNet：针对移动平台的神经结构搜索”通过使用预定义的链块结构限制搜索步骤，进一步扩展了NASNet的想法。此外，通过定义权重因子，mNASNet提供了一种更系统的方法来搜索给定特定资源限制的模型，而不仅仅是基于FLOP进行评估。



## 2019年：EfficientNet

**EfficientNet：卷积神经网络模型缩放的反思**



在2019年，对于CNN进行监督图像分类似乎不再有令人兴奋的想法。网络结构的急剧变化通常只会带来少许的精度提高。更糟的是，当同一网络应用于不同的数据集和任务时，以前声称的技巧似乎不起作用，这引发了人们的批评，即这些改进是否仅适合ImageNet数据集。另一方面，有一个技巧绝不会辜负我们的期望：使用更高分辨率的输入，为卷积层添加更多通道以及添加更多层。尽管力量非常残酷，但似乎存在一种按需扩展网络的原则方法。MobileNetV1在2017年提出了这种建议，但后来重点转移到了更好的网络设计上。





![img](https://pic2.zhimg.com/80/v2-55f56663551a7367de773d8f79a07ba1_720w.jpg)





摘自“ [EfficientNet：卷积神经网络的模型缩放思考](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/1905.11946)”



继NASNet和mNASNet之后，研究人员意识到，即使在计算机的帮助下，架构的改变也不会带来太多好处。因此，他们开始回落到扩展网络规模。EfficientNet只是建立在此假设之上的。一方面，它使用了mNASNet的最佳构建基块，以确保有良好的基础。另一方面，它定义了三个参数alpha，beta和rho来分别控制网络的深度，宽度和分辨率。这样，即使没有大型GPU池来搜索最佳结构，工程师仍可以依靠这些原则性参数根据他们的不同要求来调整网络。最后，EfficientNet提供了8种不同的变体，它们具有不同的宽度，深度和分辨率，并且无论大小模型都具有良好的性能。换句话说，如果要获得较高的精度，请使用600x600和66M参数的EfficientNet-B7。如果您想要低延迟和更小的模型，请使用224x224和5.3M参数EfficientNet-B0。问题解决了。



## 其他

如果您完成了10篇以上的论文的阅读，您应该对CNN的图像分类历史有了很好的了解。

如果您想继续学习这一领域，我还列出了一些其他有趣的论文供您阅读，这些论文在各自领域都很有名，并启发了世界上许多其他研究人员。



## 2014年：SPPNet

**深度卷积网络中的空间金字塔池用于视觉识别**



SPPNet从传统的计算机视觉特征提取中借鉴了特征金字塔的思想。该金字塔形成了一个具有不同比例的要素词袋，因此它可以适应不同的输入大小并摆脱固定大小的全连接层。这个想法还进一步启发了DeepLab的ASPP模块以及用于对象检测的FPN。



## 2016年：DenseNet

**紧密连接的卷积网络**



康奈尔大学的DenseNet进一步扩展了ResNet的想法。

它不仅提供各层之间的跳过连接，而且还具有来自所有先前各层的跳过连接。



## 2017年：SENet

**挤压和激励网络**



Xception网络证明，跨渠道关联与空间关联关系不大。但是，作为上届ImageNet竞赛的冠军，SENet设计了一个“挤压和激发”区并讲述了一个不同的故事。SE块首先使用全局池将所有通道压缩为较少的通道，然后应用完全连接的变换，然后使用另一个完全连接的层将其“激发”回原来的通道数量。因此，实质上，FC层帮助网络了解输入要素图上的注意力。



## 2017年：ShuffleNet

**ShuffleNet：一种用于移动设备的极其高效的卷积神经网络**



ShuffleNet构建在MobileNetV2的倒置瓶颈模块之上，他认为深度可分离卷积中的点式卷积会牺牲准确性，以换取更少的计算量。为了弥补这一点，ShuffleNet增加了一个额外的通道改组操作，以确保逐点卷积不会始终应用于相同的“点”。在ShuffleNetV2中，此通道重排机制也进一步扩展到ResNet身份映射分支，因此身份功能的一部分也将用于重排。



## 2018：Bag of Tricks

**使用卷积神经网络进行图像分类的技巧**



“技巧包”重点介绍在图像分类区域中使用的常见技巧。当工程师需要提高基准性能时，它可以作为很好的参考。有趣的是，诸如混合增强和余弦学习速率之类的这些技巧有时可以比新的网络体系结构实现更好的改进。





## 结论

随着EfficientNet的发布，ImageNet分类基准似乎即将结束。使用现有的深度学习方法，除非发生另一种模式转变，否则我们永远不会有一天可以在ImageNet上达到99.999％的准确性。因此，研究人员正在积极研究一些新颖的领域，例如用于大规模视觉识别的自我监督或半监督学习。同时，使用现有方法，对于工程师和企业家来说，找到这种不完美技术的实际应用已经成为一个问题。



## Reference

- Y. Lecun, L. Bottou, Y. Bengio, P. Haffner, [Gradient-based Learning Applied to Document Recognition](https://link.zhihu.com/?target=http%3A//yann.lecun.com/exdb/publis/pdf/lecun-01a.pdf)

- Alex Krizhevsky, Ilya Sutskever, Geoffrey E. Hinton, [ImageNet Classification with Deep Convolutional Neural Networks](https://link.zhihu.com/?target=https%3A//papers.nips.cc/paper/4824-imagenet-classification-with-deep-convolutional-neural-networks.pdf)

- Karen Simonyan, Andrew Zisserman, [Very Deep Convolutional Networks for Large-Scale Image Recognition](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/1409.1556)

- Christian Szegedy, Wei Liu, Yangqing Jia, Pierre Sermanet, Scott Reed, Dragomir Anguelov, Dumitru Erhan, Vincent Vanhoucke, Andrew Rabinovich, [Going Deeper with Convolutions](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/1409.4842)

- Sergey Ioffe, Christian Szegedy, [Batch Normalization: Accelerating Deep Network Training by Reducing Internal Covariate Shift](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/1502.03167)

- Kaiming He, Xiangyu Zhang, Shaoqing Ren, Jian Sun, [Deep Residual Learning for Image Recognition](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/1512.03385)

- François Chollet, [Xception: Deep Learning with Depthwise Separable Convolutions](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/1610.02357)



- Andrew G. Howard, Menglong Zhu, Bo Chen, Dmitry Kalenichenko, Weijun Wang, Tobias Weyand, Marco Andreetto, Hartwig Adam, [MobileNets: Efficient Convolutional Neural Networks for Mobile Vision Application](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/1704.04861)



- Barret Zoph, Vijay Vasudevan, Jonathon Shlens, Quoc V. Le, [Learning Transferable Architectures for Scalable Image Recognition](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/1707.07012)



- Mingxing Tan, Quoc V. Le, [EfficientNet: Rethinking Model Scaling for Convolutional Neural Networks](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/1905.11946)



- Kaiming He, Xiangyu Zhang, Shaoqing Ren, Jian Sun, [Spatial Pyramid Pooling in Deep Convolutional Networks for Visual Recognition](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/1406.4729)



- Gao Huang, Zhuang Liu, Laurens van der Maaten, Kilian Q. Weinberger, [Densely Connected Convolutional Networks](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/1608.06993)



- Jie Hu, Li Shen, Samuel Albanie, Gang Sun, Enhua Wu, [Squeeze-and-Excitation Networks](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/1709.01507)



- Xiangyu Zhang, Xinyu Zhou, Mengxiao Lin, Jian Sun, [ShuffleNet: An Extremely Efficient Convolutional Neural Network for Mobile Devices](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/1707.01083)



- Tong He, Zhi Zhang, Hang Zhang, Zhongyue Zhang, Junyuan Xie, Mu Li, [Bag of Tricks for Image Classification with Convolutional Neural Networks](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/1812.01187)



- [https://towardsdatascience.com/10-papers-you-should-read-to-understand-image-classification-in-the-deep-learning-era-4b9d792f45a7](https://link.zhihu.com/?target=https%3A//towardsdatascience.com/10-papers-you-should-read-to-understand-image-classification-in-the-deep-learning-era-4b9d792f45a7)