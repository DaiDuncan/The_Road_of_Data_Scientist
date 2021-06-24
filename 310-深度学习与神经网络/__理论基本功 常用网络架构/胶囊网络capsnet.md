# 一、[通俗理解](https://www.bilibili.com/video/BV1ty4y1r7M8/?spm_id_from=333.788.recommend_more_video.13)

背景：图像特征的位置，旋转和内部元素的相对关系

CNN提取特征时，

- -忽视特征与特征之间的相对关系

- -很难理解事物的旋转和缩放

=> 需要非常多的数据：才能让CNN辨认出不同位置、不同视角下的同一事物

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210613155321.png" alt="image-20210613155320747" style="zoom:67%;" />



## 胶囊

一组打包好的神经元：

- 神经元会输出数据 => ==胶囊会输出向量==
- 向量的长度代表胶囊对特征的认可程度，携带了一定的姿态信息

底层的胶囊在获取基础特征后，会对图片中可能是什么作出预测。



技巧|Routing by Agreement

如果找到共同认可的高层胶囊特征，就认为更可能是这一胶囊代表的内容

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210613155644.png" alt="image-20210613155644090" style="zoom:67%;" />

关键：胶囊网络相比于CNN，能更好的地理解事物的组成、位置、姿态



## 效果

1 除了MNIST，没有在更大的数据集上，表现出更好的准确性

2 训练时间更长 => 动态路由过程消耗计算资源

