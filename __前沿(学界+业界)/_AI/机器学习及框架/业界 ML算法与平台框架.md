[link: 华为云开发社区|你应该了解的机器学习算法及平台框架 2019.01.07](https://zhuanlan.zhihu.com/p/54253648)

【小宅按】机器学习（Machine Learning）是一门涉及统计学、系统辨识、逼近理论、神经网络、优化理论、计算机科学、脑科学等诸多领域的交叉学科，研究计算机怎样模拟或实现人类的学习行为，以获取新的知识或技能，重新组织已有的知识结构使之不断改善自身的性能，是人工智能技术的核心。这都是官话和套话，听得大家云里雾里的。通俗一点说，什么是机器学习呢？使用机器学习的目的是什么？

## **一、机器学习的本质**

==机器学习的最终目的就是实现分类==(回归的本质也是分类/分区间)，其实机器学习的本质就是一个非线性的分类器。

这里划重点，有两个重点：一个是非线性，另外一个分类器。

无论是回归算法、聚类算法，卷积神经网络算法，这一切都是为了分类而生的。

**现实世界的许多问题，都可以转换成分类问题**，就拿下围棋来说，也可以转换成分类问题，先转换落子概率问题，最终转化成输和赢（分类问题）



### **1、线性分类器**

要解释清楚非线性的分类器，那么就需要从线性分类器讲起。

刚开始的分类，就是使用简单的数学公式去分类。

例如：区分自然数中的奇数和偶数，数学公式就是自然数模2，如果结果等于0就是偶数，不等于0就是奇数。

后来公式复杂一些，就是Y = wX+b 函数去分类，位于直线上面的一类，位于直线下面的一类。例如下图：

![img](https://pic2.zhimg.com/80/v2-48eab498dadf3b8825efcdfeba63cd25_720w.jpg)

### **2、非线性分类器**

后来，由于特征差异非常细微，使用复杂的数学公式也不能分类了，那么机器学习算法就诞生了。

先用已经打好标签的数据做样本，训练出来一个模型，使用这个模型去分类。

这个模型就是一个非线性的分类器。

这种类型的分类器就是普通机器学习的分类器，包括**回归、分类、聚类**等各种算法。

例如下图就是一个典型的非线性分类。

![img](https://pic2.zhimg.com/80/v2-44fc5e285ec1dea3fe0fae7ac1e4ec05_720w.jpg)

### **3、深度卷积分类器**

最近比较流行的是==深度学习，其实也是一个非线性的分类器。==

因为普通的机器学习不能有效的区分现实世界里的物体，那么就需要更多维度的参数作为输入参数和多层卷积来训练模型，通过这个模型来分类，但本质还是为了精细化的分类，在更高纬度里把相似的物体区分开来。

![img](https://pic1.zhimg.com/80/v2-38496592dd9b203851f4a3ad3c949864_720w.jpg)



## **二、机器学习算法**

机器学习按照学习的方式分为：监督学习、非监督学习、强化学习。

下图主要列举出来常用的监督学习和非监督学习的算法。

![Which machine learning algorithm should I use?](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210423205256.png)

算法的详细实现过程，在Scikit_learning都有，并且还有详细的Example

链接：[http://scikit-learn.org/stable/](https://link.zhihu.com/?target=http%3A//scikit-learn.org/stable/)



## **三、业界机器学习平台框架的标杆企业**

有了机器学习算法，还需要**平台框架去承载**。

考虑到机器学习的算法代码都已经开源，本节内容重点关注一些框架训练平台。

![img](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210423210613.jpeg)

**1、普通机器学习平台库的标杆企业**

- ==Spark Machine Learning Library（ML）==提供了许多分类、回归和聚类算法的具体实现，它还支持降维，特征提取和一些优化方法。 
  - 该库是Spark平台的一部分，它提供了Java，Scala和**Python的API**。
- Scikit-learning为Python开发人员提供了各种机器学习功能和算法。 
  - 它支持分类，回归，聚类以及降维，模型选择和预处理的其他方法。
  - Scikit-learning是普通机器学习的标杆。
- Apache Mahout是开发可伸缩算法的数学环境。
  - 旨在让数学家，统计学家和数据科学家快速实现他们自己的算法。该环境可以**与MLlib集成**。



**2、深度学习平台框架的标杆企业**

- TensorFlow是一个以**图来表示计算的编程系统**。
  - TF是深度学习框架的标杆。
- AWS Deep Learning AMI集成战略，主流的深度学习框架全部集成（如 Apache MXNet 和 Gluon、TensorFlow、Microsoft Cognitive Toolkit、Caffe、Caffe2、Theano、Torch、**Pytorch**、Chainer 和 Keras ）。
  - 通过生态来聚拢用户，来提升AMI影响力，来对抗Google TensorFlow。
- Deeplearning4j(Deep Learning for Java)是一个为Java和Scala编写的开源深度学习框架是。 
  - 它可以与Spark和Hadoop集成，使其具有高度可扩展性并适合用于业务。 
  - 该框架已经在Apache 2.0许可下发布。



## **四、华为GTS使用机器学习算法**

GTS在网络优化、网络维护领域，使用了大量的机器学习算法，不是说这些机器学习算法拿来就直接能用，**需要和电信业务做大量的适配**，目前GTS AI项目组完成了60多种机器学习算法和业务做了适配。

后面会使用一个章节拿一个Case讲算法和业务如何适配。



下表列举出来GTS AI专题使用的机器学习算法：

![img](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210423210813.jpeg)



## **五、GTS搭建的机器学习训练平台**

那么有了机器学习算法，需要搭建一个训练平台，在GTS也是做了这方面的工作。

做了一个MANAS平台，平台能力包括：

![img](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210423210846.jpeg)

1、底层支持CPU/GPU的扩展、硬件资源的调度和分配、具有弹性计算能力。

2、数据的采集和数据解析，采集能力包括站点物理信息、网络设备的日志、告警、配置、运行态数据、Debug等数据，采集上来的数据一般都是压缩的数据，需要各种解析工具，解析成可以识别的数据。

3、集成了深度学习框架（TensorFlow）、Spark、机器学习。

4、模型的管理（模型加载、模型资源调度和分配、模型评估、模型部署）。

5、目前可以为站点勘测、质量验收、**网络规划**、网规网优、网络维护等领域的模型训练和模型部署。

6、平台加固和安全能力。



## **六、机器学习训练平台部署**

平台具备的能力是有了，那么这些平台如何部署？部署的原则是什么呢？

1、数据安全的地域属性原则：机器学习的平台部署，要**考虑网络安全（数据不能离开本地）**的问题，考虑地域属性问题。

2、覆盖原则，部署的位置能尽量覆盖一个区域的业务。

3、网络传输原则：部署区域的网络传输时延在一定的范围内，例如：100ms范围内。

基于以上三个原则，MANAS部署在“2+5”个节点。

- 其中的2代表是德国和贵州，用于训练态和运行态模型部署。

- 5代表俄罗斯、迪拜、南非、墨西哥、巴西，这几个节点主要是运行态，覆盖就近的国家。

运行态就是把训练好的模型直接拿来，运行业务模型，直接为业务服务。详细见下图

![img](https://pic1.zhimg.com/80/v2-983a8789532a3390483e41d1d6271408_720w.jpg)

可能大家对训练态和部署态有一些疑惑，为什么搞两种部署环境呢？做一套不行吗？这里做一个解释。

训练态大家都可以理解，就是考虑到模型训练需要大量的计算资源，深度学习需要GPU，所以需要训练态的环境。

那么为什么训练态的环境部署在中国（贵州）和德国呢？主要是从网络安全出发考虑的，因为**欧洲的数据不能出欧盟**，所以在德国部署一个点。



那为什么需要运行态的环境呢？在网络侧的人工智能，难道不是部署在客户的网络里吗？是的，这样理解是对的。

例如：网规网优、网络的故障预测预防，这都是和现网强相关，需要采集一些实时数据，是需要贴着现网部署。

但GTS-AI不仅仅是为了客户的网络服务的，有许多业务是我们内部效率改进的，例如：站点智能勘测、质量智能验收、网络智能设计等，都属于工程作业类的活动，这些就不是随着客户的网络部署的。



那么这就需要随着我们的工程作业去部署，部署在俄罗斯（覆盖大俄罗斯），迪拜（覆盖中亚和中东），南非（覆盖北非、东南非、西非），巴西（覆盖拉美南），墨西哥（覆盖拉美北、南美南），贵州（覆盖中国区、东南亚、南太），德国（覆盖欧盟）。

环境部署完毕，有了训练环境和运行环境，我们就可以在上面训练模型，部署模型，做业务。



那么这个MANAS平台还有哪些不足呢？或者将来需要补齐的一些能力，简单列举一下

1、**运行态的部署必须支持轻量化的部署，这就要求我们的MANAS平台能支持模块的裁剪，必须能做到各种模块的解耦**，

例如：执行引擎与资源调度框架的解耦，组件依赖解耦等。

只有做到这些，才能实现轻量化的部署。

其实，一些关键的业务（例如：预测预防），都是直接部署在现网的，有实时性要求的，既然是要求部署在现网上的，那么就必须考虑轻量化的部署，不要因为模块相互牵制和影响，平台非常厚重，再加上采集数据量非常大，做着做着，就做成一个大机房，做成一个数据中心了，这是客户吐槽多次的。

这一定要避免。这方面就多参考TensorFlow和Deeplearning4j，**看如何做到模块之间的解耦**。



2、**平台的易用性和低门槛**。

平台就是希望降低使用者的门槛，让更多的工程师使用这个平台。特别是服务一线海外的工程师，一线的诉求是最真实和迫切的，如果把一线的诉求传递给机关来做，时间不赶趟，另外实现的东西压根不是一线想要的东西，乒乓来乒乓去的太麻烦了。

如果一线工程师能在MANAS平台上自己解决，那么是最好不过的。



这就对平台的能力要求很高，==简单操作，易用的界面，逻辑合理==，而不是许多东西拐来拐去的，实现一个东西恨不能绕几个弯儿，这就不行。

==这方面需要长时间的积累，不是一时半会能搞定的。==

想要弥补这方面的短板，就必须加大测试力度，==让一线多试用，多反馈。==



降低人员能力要求的同时，简化业务开发流程。

详细高手在民间，民间的问题他们自己解决是最快速的。

机关只是搭建一个平台，但不是说你搭建一个平台就完了，让大家都来试用，才是最终目的。这就需要花大力气去优化和简化的核心诉求。