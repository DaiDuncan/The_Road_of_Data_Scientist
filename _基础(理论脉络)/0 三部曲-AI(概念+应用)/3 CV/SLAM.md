SLAM(simultoneous localization and mapping) 同时定位与地图构建：机器进入一个陌生环境时，通过移动和观察快速地了解周围环境，并绘制出环境地图的技术。

- 定位：了解自己在哪
- 地图构建：描绘所在环境



# [通俗理解](https://www.bilibili.com/video/BV1PW411N7g4)

定位 & 地图构建 => 对环境有3D感知：需要深度信息

- 激光雷达：激光束获得**距离、速度信息**@多普勒效应
- 单目相机：对比机器在移动过程中拍摄到的**图像的变化**，来判断与物体之间的距离
- 双目相机：类似人眼，对比直接获取深度信息
- RGB-D深度相机：红外发射器&接收器 + ==结构光方法==

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210615140206.png" alt="image-20210615140206594" style="zoom:80%;" />



周围环境图像 & 深度信息 => 机器一边移动，一边**将图像拼接起来**，绘制出环境地图 





1988开始：超过30年时间的课题研究

机器人：比如扫地机器人

无人机避障

高级别的自动驾驶

AR/WR(2018年)



硬件性能：

- RGB-D的视野较小

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210615140538.png" alt="image-20210615140538618" style="zoom:80%;" />

 

基于硬件性能的发展方向：

1 小型、轻量 => 手机，嵌入式等小型设备

2 不考虑计算代价 => 自动驾驶：更精密的3D重建



回环检测：认出曾经经过的地方@me|本科毕设💡

- 估算位置会有误差 => 误差累积过多会产生错误