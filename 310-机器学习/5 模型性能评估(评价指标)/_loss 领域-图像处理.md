> 总的来说，损失函数的形式千变万化，但追究溯源还是万变不离其宗。
>
> 其本质便是给出一个能较全面合理的==描述两个特征或集合之间的相似性度量或距离度量==，
>
> - 针对某些特定的情况，如类别不平衡等，给予适当的惩罚因子进行权重的加减。
>
> 
>
> 大多数的损失都是基于**最原始的损失一步步改进的**，
>
> - 或提出更一般的形式，
> - 或提出更加具体实例化的形式。



# 指标|图像分类

@指标-分类

## 1 混淆矩阵⭐

- TP、TN、FP、FN => 准确率(Accuracy)、精确率(Precision)、召回率(Recall)、F1-score

- 灵敏度(Sensitivity)、特异度(Specificity) => 真正例率(TPR)、假正例率(FPR)

- ROC曲线、AUC



## 2 人脸识别算法评价指标

TAR（True Accept Rate）表示正确接受的比例 => 分类图左半部分 正类

- 对属于同一人的图像对进行比较，把它们当作是同一人的图像的比例。
- T为阈值，TAR越大越好。

![image-20210317144131066](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210317144131.png)



FAR（False Acceptance Rate）误识率 => 分类图右半部分

- 比较不同人的图象时，把其中的图像对当成同一个人图像的比例
- FAR越小越好

![image-20210317144152708](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210317144152.png)



FRR（False Rejection Rate）拒识率 => 分类图左半部分 负类

- FRR就是把相同的人的图像对，当成不同的人的了

![image-20210317144244276](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210317144244.png)



EER（Equal Error Rate）等误率

- 取某个T值时，使得FAR=FRR ，此时的FAR或FRR值





# 指标|目标检测

## 回归损失

### 1 IoU（Intersection over Union）

交并比，出自UnitBox，由旷视科技于ACM2016首次提出

IoU 计算的是 “预测的边框” 和 “真实的边框” 的交集和并集的比值。

- 在图像分割中这一指标通常比：直接计算每个像素的分类正确概率要低，也对错误分类更加敏感。

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210317145416.png" alt="image-20210317145416489" style="zoom:80%;" />

常规的Lx损失中，都是基于目标边界中的4个坐标点信息之间分别进行回归损失计算的。因此，这些边框信息之间是相互**独立**的。然而，直观上来看，这些边框信息之间必然是存在某种**相关性**的。

如下图(a)-(b)分别所示，绿色框代表Ground Truth，黑色框代表Prediction，可以看出，同一个Lx分数，预测框与真实框之间的拟合/重叠程度并不相同，显然**重叠度**越高的预测框是越合理的。IoU损失将候选框的四个边界信息作为一个**整体**进行回归，从而实现准确、高效的定位，具有很好的尺度不变性。

为了解决IoU**度量不可导**的现象，引入了负Ln范数来间接计算IoU损失。

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210608215403.webp)





### 2 GIoU

泛化的IoU损失，全称为Generalized Intersection over Union，由斯坦福学者于CVPR2019年发表的这篇论文（https://arxiv.org/abs/1902.09630）中首次提出

上面我们提到了IoU损失可以解决边界框坐标之间相互独立的问题，考虑这样一种情况，当预测框与真实框之间**没有任何重叠时，两个边框的交集（分子）为0**，此时IoU损失为0，因此IoU无法算出两者之间的距离（重叠度）

另一方面，由于IoU损失为零，意味着梯度无法有效地反向传播和更新，即出现梯度消失的现象，致使网络无法给出一个**优化的方向**。

此外，如下图所示，IoU对**不同方向的边框对齐**也是一脸懵逼的，所计算出来的值都一样

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210608215548.webp" alt="图片" style="zoom: 50%;" />

为了解决以上的问题，如下图公式所示，GIoU通过计算任意两个形状（这里以矩形框为例）A和B的一个**最小闭合凸面**C，

然后再计算C中排除掉A和B后的面积占C原始面积的比值，

最后再用原始的IoU减去这个比值得到泛化后的IoU值。

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210608215624.webp" alt="图片" style="zoom:50%;" />

GIoU具有IoU所拥有的一切特性，如对称性，三角不等式等。

GIoU ≤ IoU，特别地，0 ≤ IoU(A, B) ≤ -1, 而0 ≤ GIoU(A, B) ≤ -1；当两个边框的完全重叠时，此时GIoU = IoU = 1. 而当 |AUB| 与 最小闭合凸面C 的比值趋近为0时，即两个边框不相交的情况下，此时GIoU将渐渐收敛至 -1. 

同样地，我们也可以通过一定的方式去计算出两个矩形框之间的GIoU损失，具体计算步骤也非常简单，详情参考原论文。



### 3 DIoU

距离IoU损失，全称为Distance-IoU loss，由天津大学数学学院研究人员于AAAI2020所发表的这篇论文 （https://arxiv.org/abs/1911.08287）中首次提出。

上面我们谈到GIoU通过引入最小闭合凸面来解决IoU无法对不重叠边框的优化问题。

但是，其仍然存在两大局限性：**边框回归还不够精确 & 收敛速度缓慢** 。

考虑下图这种情况，当目标框**完全包含**预测框时，此时GIoU**退化**为IoU。

显然，我们==希望的预测是最右边这种情况==。

因此，作者通过计算两个边框之间的**中心点归一化距离**，从而更好的优化这种情况。

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210608215734.webp" alt="图片" style="zoom:50%;" />

下图表示的是GIoU损失（第一行）和DIoU损失（第二行）的一个训练过程收敛情况。

其中绿色框为目标边框，黑色框为锚框，蓝色框和红色框则分别表示使用GIoU损失和DIoU损失所得到的预测框。可以发现，

- GIoU损失一般会**增加预测框的大小使其能和目标框重叠**，
- 而DIoU损失则直接使目标框和预测框之间的==**中心点归一化距离最小**==，即让预测框的中心快速的向目标中心收敛。

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210608215818.webp)

左图给出这三个IoU损失所对应的计算公式。

对于DIoU来说，如图右所示，其惩罚项由两部分构成：

- 分子为目标框和预测框中心点之间的欧式距离；
- 分母为两个框最小外接矩形框的两个**对角线距离**。

因此， 直接优化两个点之间的距离会使得模型**收敛得更快，同时又能够在两个边框不重叠的情况下给出一个优化的方向。**

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210608215955.webp" alt="图片" style="zoom: 67%;" />



### 4 CIoU

完整IoU损失，全称为Complete IoU loss，与DIoU出自同一篇论文。

上面我们提到GIoU存在两个缺陷，DIoU的提出解决了其实一个缺陷，即**收敛速度的问题**。

而一个好的边框回归损失应该同时考虑三个重要的几何因素，

- 即重叠面积（**Overlap area**）、中心点距离（C**entral point distance**）和高宽比（**Aspect ratio**）。

GIoU考虑到了重叠面积的问题，DIoU考虑到了重叠面积和中心点距离的问题，CIoU则在此基础上进一步的考虑到了**高宽比的问题**。

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210608220122.webp)

CIoU的计算公式如下所示，可以看出，其在DIoU的基础上加**多了一个惩罚项**$\alpha v$。

- 其中α为**权重为正数的重叠面积平衡因子**，在回归中被赋与更高的优先级，特别是在两个边框不重叠的情况下；
- 而v则用于**测量宽高比的一致性**。

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210608220201.webp" alt="图片" style="zoom:67%;" />

### 5 F-EIoU

Focal and Efficient IoU Loss是由华南理工大学学者最近提出的一篇关于**目标检测损失函数**的论文，文章主要的贡献是提升网络收敛速度和目标定位精度。

目前检测任务的损失函数主要有两个缺点：

(1)无法有效地描述**边界框回归的目标**，导致收敛速度慢以及回归结果不准确

(2)忽略了边界框回归中不平衡的问题。



F-EIou loss首先提出了一种有效的交并集（IOU）损失，它可以准确地测量边界框回归中的**重叠面积**、**中心点**和**边长**三个几何因素的差异：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210608220308.webp" alt="图片" style="zoom: 67%;" />

其次，基于对有效样本挖掘问题（EEM）的探讨，提出了Focal loss的回归版本，以使回归过程中专注于高质量的锚框：

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210608220340.webp)

最后，将以上两个部分结合起来得到Focal-EIou Loss：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210608220349.webp" alt="图片" style="zoom:67%;" />

其中，通过加入每个batch的权重和来避免网络在早期训练阶段收敛慢的问题。



### 6 CDIoU

Control Distance IoU Loss是由同济大学学者提出的，文章的主要贡献是在**几乎不增强计算量的前提下**有效提升了**边界框回归的精准度**。

目前检测领域主要两大问题：

（1）SOTA算法虽然有效但计算成本高

（2）边界框回归损失函数设计不够合理。

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210608220439.webp)

文章首先提出了一种对于Region Propasl（RP）和Ground Truth（GT）之间的新评估方式，即CDIoU。

可以发现，它虽然没有直接中心点距离和长宽比，但最终的计算结果是有反应出RP和GT的差异。计算公式如下：

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210608220454.webp)

对比以往直接计算中心点距离或是形状相似性的损失函数，CDIoU能更合理地评估RP和GT的差异并且有效地降低计算成本。

然后，根据上述的公式，CDIoU Loss可以定义为：$L_{CDIoU} = L_{IoU_s} + diou$

通过观察这个公式，可以直观地感受到，在权重迭代过程中，模型不断地将RP的四个顶点拉向GT的四个顶点，直到它们重叠为止，如下图所示：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210608220555.webp" alt="图片" style="zoom:50%;" />



### 7 AP、mAP

[link](https://zhuanlan.zhihu.com/p/139073511)



## 分类损失

### 1 Dice

骰子损失，出自V-Net https://arxiv.org/abs/1606.04797

一种==集合相似度==度量指标，通常用于计算两个样本的相似度，值的范围0~1

值越大表示两个值的相似度越高

- 分割结果最好时值为1
- 最差时值为0



对mask的内部填充比较敏感

![image-20210317145601480](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210317145601.png)

![image-20210317145612266](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210317145612.png)

第一个等式：

- 分子表示交集
- 分母$|\cdot|$表示像素点的个数
- 分子乘于2(分母除以2)保证域值范围在0~1之间，因为分母相加时会计算多一次重叠区间

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210608222012.webp" alt="图片" style="zoom:50%;" />

从右边公式也可以看出，其实==Dice系数是等价于F1分数的，优化Dice等价于优化F1值==。

此外，为了防止分母项为0，一般我们会在分子和分母处**同时加入一个很小的数作为平滑系数**，也称为拉普拉斯平滑项。

Dice损失由以下两个主要特性：

- ==有益于正负样本不均衡的情况==，侧重于对前景的挖掘；
- 训练过程中，在有较多小目标的情况下容易出现振荡；
- 极端情况下会出现**梯度饱和**的情况。

所以一般来说，我们都会结合交叉熵损失或者其他分类损失一同进行优化。





### 2 Focal

焦点损失，出自[何凯明的《Focal Loss for Dense Object Detection》](https://arxiv.org/abs/1708.02002)，出发点是解决目标检测领域中one-stage算法如YOLO系列算法准确率不高的问题。

作者认为样本的**类别不均衡**（比如前景和背景）是导致这个问题的主要原因。

比如在很多输入图片中，我们利用网格去划分小窗口，大多数的窗口是不包含目标的。

如此一来，如果我们**直接运用原始的交叉熵损失，那么负样本所占比例会非常大**，主导梯度的优化方向，即网络会偏向于将前景预测为背景。

即使我们可以使用OHEM（在线困难样本挖掘）算法来处理不均衡的问题，虽然其增加了误分类样本的权重，但也容易忽略掉易分类样本。

而Focal loss则是聚焦于==训练一个困难样本的稀疏集==，通过直接在标准的交叉熵损失基础上做改进，引进了两个惩罚因子，来减少易分类样本的权重，使得模型在训练过程中更专注于困难样本。

其基本定义如下：

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210608222242.webp)

其中：

- 参数 α 和 (1-α) 分别用于==控制正/负样本的比例==，其取值范围为[0, 1]。α的取值一般可通过**交叉验证来选择**合适的值。
- 参数 γ 称为聚焦参数，其取值范围为[0, +∞)，目的是通过**减少易分类**样本的权重，从而使模型在训练时更专注于困难样本。
  - 当 γ = 0 时，Focal Loss就退化为交叉熵损失，γ 越大，对易分类样本的惩罚力度就越大。

实验中，作者取（α=0.25，γ=0.2）的效果最好，具体还需要根据任务的情况调整。

由此可见，应用Focal-loss也会引入多了两个超参数需要调整，而一般来说很需要经验才能调好。



### 3 Tversky loss

Tversky loss，发表于[CVPR 2018上的一篇《Tversky loss function for image segmentation using 3D fully convolutional deep networks》文章](https://arxiv.org/abs/1706.05721)，是根据Tversky 等人于[1997年发表的《Features of Similarity》文章](http://www.ai.mit.edu/projects/dm/Tversky-features.pdf) 所提出的Tversky指数所改造的。

Tversky系数主要用于描述==两个特征（集合）之间的相似度==，其定义如下：

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210608222407.webp)

由上可知，它是结合了Dice系数（F1-score）以及Jaccard系数（IoU）的一种广义形式，如：

- 当 α = β = 0.5时，此时Tversky loss便退化为Dice系数（分子分母同乘于2）
- 当 α = β = 1时，此时Tversky loss便退化为Jaccard系数（交并比）

因此，我们只需控制 α 和 β 便可以控制**假阴性**和**假阳性**之间的平衡。

比如在医学领域我们要检测肿瘤时，更多时候我们是希望Recall值（查全率，也称为灵敏度或召回率）更高，因为我们不希望说将肿瘤检测为非肿瘤，即假阴性。

因此，我们可以通过增大 β 的取值，来提高网络对肿瘤检测的灵敏度。

其中，α + β 的取值我们一般会令其1。





# 指标|图像分割

## 1 Dice相似系数

@上述`指标|目标检测 分类损失 1 Dice`





## 2 Hausdorff距离

描述两组点集之间相似程度的一种量度，对==分割出的边界==比较敏感。

![image-20210317145636593](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210317145636.png)

![image-20210317145645197](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210317145645.png)





# 指标|图像生成

## 1 Inception Score

最早用于GAN生成图像多样性评估的指标，利用了Google的Inception模型来进行评估。





## 2 最大平均差异（Maximum Mean Discrepancy）

也是一个用于判断两个分布p和q是否相同的指标。





## 3 Wasserstein距离



## 4 Frechet Inception距离