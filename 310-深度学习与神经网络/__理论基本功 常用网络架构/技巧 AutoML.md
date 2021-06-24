# [通俗理解](https://www.bilibili.com/video/BV1Fw411f7G2)

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210617105036.png" alt="image-20210617105036323" style="zoom:67%;" />

## 完整的ML流程

1 数据准备

- 收集
- 清洗
- 增强

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210617104621.png" alt="image-20210617104621248" style="zoom:67%;" />

2 特征工程

- 选择
- 构建
- 提取

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210617104656.png" alt="image-20210617104656336" style="zoom:67%;" />

3 模型生成

- 选择模型
- 选择优化方法

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210617104742.png" alt="image-20210617104742476" style="zoom:67%;" />

4 模型评估

- 模型训练
- 模型调优

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210617104755.png" alt="image-20210617104755027" style="zoom:67%;" />





AutoML：根据任务目标，自动实现模型构建、筛选的技术

- 其中：模型评估是最难，也最重要的部分



原理：搭积木

1）搜索空间|确定有哪些零件可以用

- 可用零件编码：卷积，池化

2）搜索策略

- 在上述零件库中，找到最适合的零件
- 以及拼接零件的方法

许多ML方法：

- 遗传算法
- 神经网络架构搜索：基础是一个RNN网络
  - 通过该网络生成模型：不断评估这些模型在验证集上的准确率 => 反馈调整：提升RNN网络的生成能力

3）评价标准：测试搭建好的模型，能不能用，好不好用





平台：

- Google cloud autoML
- Azure machine learning
- easyDL

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210617105554.png" alt="image-20210617105554110" style="zoom:67%;" />



未来：可以在不了解ML，不会编程的前提下，直接使用AutoML完成任务



评论：

- automl+庞大算力+海亮数据＝燃烧的金钱
- 其中的玄妙，把握不住（话说沐神就用这个十行代码刷掉了榜上90% tql）
- 具体的automl实现工具autogluon