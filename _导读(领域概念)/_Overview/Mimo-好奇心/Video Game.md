# 概念

要素：

- 声音
- 图像
- 物理建模
- 网络



FPS|帧frame每秒：好的游戏一般在30-60FPS

GPU：Graphics Processing Unit

- 渲染颜色，光影效果

CPU：Central Processing Unit

- 控制逻辑运算
- 物理：跳跃，打败敌人

渲染rendering：创建帧

- 基于颜色，位置和物理规律创建帧



输入、输出设备



# 游戏引擎engine

定义：处理常用任务细节相关的代码

- 模拟物理

- 渲染图像(基于GPU)



# 编程

- 逻辑
- 规则

```c++
// 每当渲染frame时，都会调用Update函数
// 1 检测游戏当前的状态：与物体互动，触发某些事情
void Update(){
    if (wallCollision == true)
        moveleft = true;
    ...
}
```



# GPU

## 图像分辨率：

- 1024*576
- 1280*720
- 1920*1080 => 1080p



## 像素pixel

由于像素是小方块，导致锯齿jaggies效应 => aliasing混叠

antialiasing：边缘模糊化 => 视觉效果更好



## screen tearing画面撕裂

现象：两个或以上的不同帧出现在同一时刻

原因：显示器的帧刷新率refresh rate与渲染的帧刷新率不同

- 比如渲染时120FPS，而显示器刷新率是60次/s



改善方法：vsync同步刷新率

