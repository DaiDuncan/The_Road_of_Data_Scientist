Generative adversarial network生成对抗网络：生成判别，相互对抗，共同成长

> 生成器、判别器虽说是对抗，但更像是朋友：一开始大家都是无名之辈，随着不断地切磋升级 => 共同成为一代高手

# 一、[通俗理解|GAN](https://www.bilibili.com/video/BV1mp4y187dm/?spm_id_from=333.788.recommend_more_video.-1)

背景：人脸检测，图像识别，语音识别 => 都是在现有事物的基础上，做出描述或判断

- GAN：创造不存在的东西(却可以以假乱真)

1 生成

生成器负责依据随机向量，产生内容(图像，文字，音乐)

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210613214819.png" alt="image-20210613214819350" style="zoom:50%;" />



2 判别

判别器：判断接收到的内容是否真实

- 通常给出一个概率：代表内容的真实程度



生成器、判别器使用何种网络没有规定：CNN，FC(全连接)均可



3 对抗Adversarial：交替训练

示例：图像生成

1）生成器生成假图片 + 收集到的真图片 => 都交给判别器：学习区分两者

- 给真图片高分，假图片低分

2）在判别器熟练判断现有数据后，训练生成器，使得从判别器获得高分为目标

- 不断生成更好的“假图片”，直到能骗过判别器

3）重复以上两个过程 => 直到判别器对任何图片的预测概率都接近0.5 => 无法识别真假 => 停止训练





最终目标：获得==足够好的生成器== => 生成模型

- 玻尔兹曼机
- VAE：变分Auto-Encoder





# 一、[通俗理解|GAN家族](https://www.bilibili.com/video/BV1mU4y1b7wR/?spm_id_from=333.788.recommend_more_video.-1)

缺点：非常难训练 => 目标是骗过判别器：

1）模式崩溃：生成同样一种已经骗过判别器的结果

2）结构问题：钻空子

- 判别器根据有眼睛、毛发，判断狗是不是真的 => 生成器：生成三个眼睛的狗



## 1 WGAN|改变判断基准

W: Wasserstein距离 => 判别器以此为依据

- 能很好地判断生成内容与真实内容有多相似
- 为生成器的改进方向给出指导

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210613222827.png" alt="image-20210613222826566" style="zoom:50%;" />



## 2 DCGAN|改变网络类型

G、D均为CNN的GAN：但是让CNN的结构倒过来 => 开始生成高级特征(比如轮廓)，再生成低级特征

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210613223000.png" alt="image-20210613222959826" style="zoom:67%;" />



## 3 CycleGAN|生成器、判别器的数量

G1：负责根据简笔画，生成照片 => 让照片尽可能通过照片判别器DI的审查

G2：负责根据照片，生成简笔画 => 让简笔画尽可能通过简笔画判别器D2的审查

首尾相连 => 让经过两次转换后的内容与原始内容一致

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210613223203.png" alt="image-20210613223203133" style="zoom:67%;" />

## 4 Style GAN

能融合不同图片的风格，不断产生新图片

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210613223330.png" alt="image-20210613223330053" style="zoom:67%;" />

## 5 3D GAN

将2D图片，转换成3D模型

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210613223418.png" alt="image-20210613223417835" style="zoom:80%;" />



## 补充[|GAN zoo](https://www.findbestopensource.com/product/hindupuravinash-the-gan-zoo)

更多GAN的类型



# 二、[Udacity讲师](https://www.youtube.com/watch?v=8L11aMN5KY8&list=RDCMUCgBncpylJ1kiVaPyP-PZauQ&index=5)

> thispersondoesnotexist.com AI生成的图像
>
> [讲师源码](https://github.com/luisguiserrano/gans)

Generator, Discriminator

## 示例|2*2图像

- 1层神经网络

真实图像：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210613215810.png" alt="image-20210613215810071" style="zoom:50%;" />

假图像：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210613215835.png" alt="image-20210613215834558" style="zoom:50%;" />



### 构造Discriminator

0 idea

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210613220206.png" alt="image-20210613220205832" style="zoom: 50%;" />

- 阈值 => sigmoid 73%概率通过

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210613220308.png" alt="image-20210613220307792" style="zoom: 33%;" />



### 构造Generator

0 idea

随机数z

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210613220504.png" alt="image-20210613220503433" style="zoom:33%;" />



### 训练过程

1 损失函数error functions

- log-loss error function：使用log，而不是概率本身@HMM 概率相乘消失
  - prediction接近label => error小
  - prediction远离label => error大

- label是1：error = -ln(prediction)

  <img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210613220824.png" alt="image-20210613220823959" style="zoom:33%;" />

- label是0：error = -ln(1 - prediction)

  <img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210613220905.png" alt="image-20210613220904814" style="zoom:33%;" />

2 BP

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210613221009.png" alt="image-20210613221008561" style="zoom: 50%;" />

针对Discriminator

- 基于真图像

  <img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210613221934.png" alt="image-20210613221933108" style="zoom:80%;" />

- 基于假图像

  <img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210613221952.png" alt="image-20210613221951611" style="zoom: 67%;" />

针对Generator：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210613222120.png" alt="image-20210613222119842" style="zoom:80%;" />





3 训练过程@上述`通俗理解|GAN`

0）思维方式：理想的Generator, Discriminator是什么样子？

Discriminator：

- 假图(生成器生成的图像)应该判别为0

Generator：希望以假乱真 => 判别为0



1）两者的error functions不同：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210613221404.png" alt="image-20210613221403743" style="zoom: 50%;" />



2）按照上述`通俗理解|GAN`的三个步骤，基于BP，不断优化参数



4 最终结果

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210613221710.png" alt="image-20210613221709399" style="zoom:50%;" />





## 代码

[讲师源码](https://github.com/luisguiserrano/gans)



## 结果

error function plots

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210613222339.png" alt="image-20210613222338692" style="zoom:80%;" />

