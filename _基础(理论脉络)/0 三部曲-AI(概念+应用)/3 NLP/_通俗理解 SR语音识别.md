只跟声音打交道

技术：

- 语音识别
- 语音合成
- 声纹识别
- NLP：文本的意义

应用：

- 语音导航
- 智能音箱
- 语音输入文字

问题：

- 鸡尾酒会|多人声音 => 如何分离出想要听到的音轨
- 远场识别|与声源太远
- 方言识别



# [语音识别](https://www.bilibili.com/video/BV1At411B7Dz)

0 大量声音数据 => 训练声学模型：将声音转化为声学符号，比如语素(zh i等)

大量文本数据 => 训练语言模型：为声学符号找到可能的文字表达

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210616102038.png" alt="image-20210616102037858" style="zoom:50%;" />

1 语音切割 => 声学模型识别：语音状态(声母、韵母的组成部分)

![image-20210616113732108](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210616113732.png)

2 声学模型：基于语音状态 => 最有可能的声学符号表达

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210616113752.png" alt="image-20210616113752428" style="zoom:80%;" />

3 语言模型：声学符号表达 => 最有可能的文字表达

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210616113921.png" alt="image-20210616113921029" style="zoom:80%;" />

## 方言

 将训练声学模型、语言模型的数据更换为方言数据 + 方言词典

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210616114016.png" alt="image-20210616114016698" style="zoom: 67%;" />







# 语音合成|导航

语音库：尽可能地覆盖语言中的元音、辅音、音调 => 录制的内容：需要经过一定的设计

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210616114357.png" alt="image-20210616114357025" style="zoom:67%;" />

1 预测文本的读音

将文本转换为音素序列

- 节奏，重音，数字、缩写等 => 让语音更加自然



2 合成声音

方法1）波形拼接

语音库中逐一寻找与目标一致的**音素**，将这些因素拼接起来

方法2）参数合成

将第一步预测的音素，**转换成每时每刻的语音参数**，加上从**语音库中学习到的特征** => 生成语音

方法3）端到端：深度学习

效果不如将前两种方法融合起来

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210616114718.png" alt="image-20210616114718446" style="zoom:67%;" />



特殊语句：左转，减速，掉头 => 通常是录好的内容

- 关键信息在长句子 => 是合成的



# 小结|语音识别+语音合成

本质：是对==信息的加工== => 理解(语音识别)和输出(语音合成)

功能：声纹识别，关键词唤醒

载体/场景：手机，汽车，显示器等

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210616115829.png" alt="image-20210616115829184" style="zoom:67%;" />

移动端：

- 语音输入法
- 朗读电子书籍和文章

语音助手

- 智能音箱、智能电视、智能车载设备等家居类
- 智能手表、智能眼镜等可穿戴设备
- 陪小朋友聊天、讲故事的智能陪伴机器人
- 拓展：智能客户，模拟用户声音的虚拟歌唱系统等

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210616120102.png" alt="image-20210616120102029" style="zoom:67%;" />



=> 声音的下一个爆发力应用场景？



# [声纹识别](https://www.bilibili.com/video/BV1dt411675e)

背景：发声器官千差万别

示例|苹果：

- 形状，颜色
- 边缘的弧度
- 叶子的锯齿



问题：

1 安全吗？

1）提前录制的声音：录制、播放过程声音衰减失真，很容易区分开

2）产品设计理念：让用户在规定时间内说出一组随机数字@支付宝

3）结合人脸识别，指纹识别





# [智能音箱](https://www.bilibili.com/video/BV1ut411z7Vi)

本质：语音识别 + chatbot + 语音合成

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210616115532.png" alt="image-20210616115532483" style="zoom:80%;" />

问题：

1 答非所问  => 可能是环境太嘈杂

2 恶意卖萌 => chatbot回应不符合喜好

3 无故被唤醒 => 不小心听到了和唤醒词很像的其他词语

4 网络不好



形象设计：

- 听觉形象；：音色，节奏，音调，响度
  - 沉稳缓慢的语调能表达出服务与尊敬
  - 戏谑、快节奏适合聊八卦
  - 软萌萌的声音：适合和小朋友聊天
- 唤醒：
  - 实体按键，虚拟按键
  - 唤醒词：3-4个字节左右最好
- 用户区分
  - 家庭设备：根据声纹识别，辨认出不同的用户



语音交互设计：

- 右侧是语音输入

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210616120431.png" alt="image-20210616120431308" style="zoom:80%;" />



开源：都有语音识别、语音唤醒、语音合成的API可以调用

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210616120600.png" alt="image-20210616120600565" style="zoom:80%;" />



# 音乐

## [听歌识曲](https://www.bilibili.com/video/BV1tb411U7BW)

1 提取片段的特征

过去：尝试将音高的变化(音乐声音的强度)作为检索基础 => 效果不好



将音乐转化为频谱图：每隔几十ms提取一次标志点的特征，这些特征称为指纹

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210616132247.png" alt="image-20210616132246663" style="zoom:67%;" />



2 匹配：找到同样的指纹片段

问题：资料库中的乐曲太多，如何比对？ => 建立音乐库的搜索引擎💡

- 乐曲 = 网页
- 指纹 = 关键词query

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210616132402.png" alt="image-20210616132402287" style="zoom:67%;" />



<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210616132434.png" alt="image-20210616132433909" style="zoom:80%;" />



## 机器创作音乐

1 训练

用音乐作品来训练模型：学习乐曲的特征



2 生成

写一个乐段，随便几个音符 => 输入给已经训练好的模型：模型根据这个乐段，生成后续的音乐



3 打分

逐一聆听生成的作品，并将评分(人工参与，标注Label)反馈给机器 => 让机器更好地了解倾向

不断地打分、不断地生成：直到得到满意的作品

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210616132724.png" alt="image-20210616132724497" style="zoom: 67%;" />



拓展|想要生成摇滚乐

将第一步训练的作品换成摇滚乐



问题：AI能否取代人类写音乐作品吗？

- 类似姓名生成器，古诗生成器 => 可以作为**启发、参考**

+人工参与、优化 => 创作的事，还是交给人类





# 虚拟偶像

定义：人工创造的，在虚拟/现实世界进行偶像活动的虚拟形象

第一代虚拟偶像 2007.08.31 初音未来音源库发布

- 以Vocaloid为代表
- 以创作者上传歌曲为主
- 歌声依靠语料拼接：在Vocaloid软件中，安排好音素的位置 => 改变气声，嘴型等参数 => 以求接近人声

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210616133229.png" alt="image-20210616133229121" style="zoom:67%;" />



第二代 Vtuber 2016 绊爱爱酱KizunaAI

- 依靠动作捕捉设备，捕捉主播的动作来合成虚拟形象的动作
- 声音来自主播本人



第三代 CeVIO

- 非常接近TTS软件：输入歌曲、乐谱 => 生成歌声
- 官方Demo：非常接近真人
- 更加接近Vocaloid：需要创作者的创造力





# 问题|鸡尾酒会

1953年：鸡尾酒会交谈，语音信号重叠在一起 => 机器需要将它们分离成独立的信号

本质：与物体识别非常相似

- 有用信号：物体；想要听到的声音
- 噪声：背景；其他声音



思路1）基于单通道系统

依靠语音的频谱

- 将想听到的声音的时频元标注为1，其他时频元标注为0 => 机器学习训练模型：输出1的部分

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210616134625.png" alt="image-20210616134625369" style="zoom:67%;" />



思路2）基于多通道系统

在鸡尾酒会的不同位置，布置多个麦克风

- 利用空间属性，对声音进行分离

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210616134721.png" alt="image-20210616134721409" style="zoom:67%;" />



深度学习有突破，但没有真正解决这个问题



# 问题|远距离识别1-10m

问题：

- 距离远：收音效果差
- 中间混响
- 噪声



方法1）麦克风阵列(==大多数音箱的选择==)

- 两个以上的麦克风
- 直线，环形，球状



距离差 => 声波差异

- 了解声源的位置 => 定向增强：提升收音效果
- 抑制其他方向的声音：解决房间混响、噪声问题

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210616134959.png" alt="image-20210616134959397" style="zoom:67%;" />



语音识别往往使用近场语音数据生成模型，如果用**远场**语音数据，效果可能更好



方法2）深度学习降噪



# [拓展|多模态](https://www.bilibili.com/video/BV1Tb411J7sd)💡

总结语音识别SR

![image-20210616135239710](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210616135240.png)



多模态：多感官融合

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210616135413.png" alt="image-20210616135412837" style="zoom:67%;" />

1 语音+CV

- 声纹识别 + 人脸识别

2 判断视频中人们的行为

3 增加情感识别 => 更好地互动

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210616135441.png" alt="image-20210616135441232" style="zoom:67%;" />





# #参考文献

[评论|笔记汇总](https://blog.csdn.net/kodoshinichi/article/details/114329048)