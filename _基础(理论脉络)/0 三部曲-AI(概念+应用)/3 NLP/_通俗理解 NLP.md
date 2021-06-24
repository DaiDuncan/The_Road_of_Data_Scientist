# [通俗理解|语音识别](https://www.bilibili.com/video/BV1Lx41177sY)

核心任务：将语音转换成文字

- ASR(Automatic Speech Recognition)：自动语音识别
- STT(Speech to text)：语音转文字

语言 <= 单词 <= 音素

- 将语音的声波，按帧切开 => 帧组成状态 => 状态组成音素 => 合成单词



其他任务：

- 声纹识别：识别说话者是谁

- 语音合成：text to speech



应用：

- Siri
- 智能音箱
- 车载设备



影响因素：口音，距离，噪声



# [通俗理解|NLP](https://www.bilibili.com/video/BV18x411L7dz)

语音识别：单纯将波形转换为文字

背景：人类语言太复杂，同一词语在不同情境下，对应着不同的含义

- I bought an Apple(是苹果还是手机？) => 涉及到==语义的理解==



机器翻译：将一种语言，转化为另一种语言

- 谷歌
- 有道翻译

中文自动分词：对中文文本进行词语切分

问答系统：Siri, 小冰

- 能够自动回答问题的对话系统

信息抽取：将海量文本数据进行结构化的信息抽取@me|爬虫整理资料

阅读理解：理解字里行间，传达的情绪的情感分析

自动摘要

文本分类

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210615144858.png" alt="image-20210615144858561" style="zoom:80%;" />





# [机器翻译](https://www.bilibili.com/video/BV1RW411C7Nv)

背景：

- 1954 机器翻译
- 1956 人工智能



发展阶段：

1 基于规则

- 先分析句子中单词的词性 

- -语言表达的方法太灵活：语法规则不能覆盖所有情况，也不一定包含常用的用法@me|语言：实用为主，语法有个框架印象即可

  <img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210615145701.png" alt="image-20210615145701233" style="zoom:80%;" />

2 基于统计

- 根据词/短语找到所有可能的结果：再在庞大的语料库中进行搜索 => 将出现概率最高的结果进行输出

- +有提升

- -对语料库依赖大

  <img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210615145720.png" alt="image-20210615145720355" style="zoom:80%;" />

3 基于神经网络

- 基于大量==成对==的语料：让机器自己学习语言的特征，端到端地输出结果

- -成对语料少 => 改用统计方法

  <img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210615145743.png" alt="image-20210615145743751" style="zoom:80%;" />

4 基于中间语言

没有语料：靠中间语料进行转换

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210615145816.png" alt="image-20210615145816188" style="zoom:80%;" />

5 基于实例

约定俗成的短语：基于实例

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210615145804.png" alt="image-20210615145804120" style="zoom:80%;" />



[应用](https://www.bilibili.com/video/BV1QW41167ea)

1 基础应用

- 翻译网页
- 商用翻译系统

2 与AI结合

- +OCR、AR => 实景翻译
- +语音识别 => 实时字幕@看电影
  - 大型会议现场：实时翻译系统
  - 通讯软件中：视频直接翻译
- +语音识别、语音合成 => 实体：随身携带的翻译机

3 生成古诗

💡翻译的本质：寻找源语言与目标语言之间的**对应关系**

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210615153537.png" alt="image-20210615153537020" style="zoom:80%;" />



<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210615153553.png" alt="image-20210615153552950" style="zoom: 67%;" />



# [信息抽取](https://www.bilibili.com/video/BV1aW411U7KR)

从自然语言文本中，抽取出特定的事件或事实信息 => 帮助将海量内容自动分类、提取和重构

- 信息检索⭐
- 问答系统
- 情感分析
- 文本挖掘

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210615153915.png" alt="image-20210615153915118" style="zoom:80%;" />

从新闻中抽取时间、地点、关键人物：
<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210615154000.png" alt="image-20210615154000039" style="zoom:67%;" />

从技术文档中抽取：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210615154035.png" alt="image-20210615154034813" style="zoom: 67%;" />





信息抽取 VS. 自动摘要

- 信息抽取更有目的性 => 输出的信息具有一定框架
- 自动摘要输出完整的自然语言句子 => 需要考虑语言的连贯和语法，甚至是逻辑
- 信息抽取也可以用来完成自动摘要

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210615154153.png" alt="image-20210615154153388" style="zoom:67%;" />

 

## [搜索引擎](https://www.bilibili.com/video/BV1bt411f7FH)

信息检索：起源于图书馆的资料查询和文摘索引

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210615154734.png" alt="image-20210615154733714" style="zoom:67%;" />

检索内容：文本，图片，音频，视频等信息



通常信息检索会包含一个Query：表达需求的查询字段

系统回复：包含所需要信息的文档列表



搜索引擎实最常见、规模最大的信息检索系统

- 通过**爬虫不断抓取、存储、更新**互联网中的网页内容

- 为这些内容建立像字典一样的索引目录

  - 输入关键词：通过关键词在网页中出现的次数和位置，来判断页面与Query的相关性

    <img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210615155044.png" alt="image-20210615155044050" style="zoom:67%;" />

  - 按照相关性由高到低的顺序：排列起来



每个步骤都有难度：

- 理解用户的Query
- 清除重复、低质量的页面
- **建立高效的索引**⭐

NLP支撑技术：

+ 分词

+ 信息提取

+ 文本分类

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210615155240.png" alt="image-20210615155240521" style="zoom:67%;" />



补充|搜索技巧：

- or
- intitle: 
- filetype:
- site:



## [情感分析](https://www.bilibili.com/video/BV15t411f7x9)

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210615155414.png" alt="image-20210615155414101" style="zoom:67%;" />

情感词典

- 讲句子拆分成词语：对照词典计算情感得分

- -构造一份全面的情感词典不容易 => 机器学习

  <img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210615155636.png" alt="image-20210615155635861" style="zoom:80%;" />

机器学习

- 输入大量带有label的评价数据 => 自动获取评价的特征

- 电商是最天然的评价数据的来源

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210615155755.png" alt="image-20210615155755584" style="zoom:67%;" />

1 情感倾向：正面，反面

2 细粒度情感：喜 怒 哀 乐 爱 恶 欲 

3 多模态情感分析

- 借助面部表情 & 声音 => 对视频中的人物进行情感分析@me|投资影响因素

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210615155933.png" alt="image-20210615155933195" style="zoom:67%;" />



## [关键词图谱](https://www.bilibili.com/video/BV1Tt411d7So)

0 微博关键词图谱：

- 抓取用户发送过的微博内容 => **分词**
- 去除停止词(频次高，却没有太多意义：比如的，也，了)
- 统计词频

=> 了解自己的状态、兴趣，与他人的相似度

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210616093504.png" alt="image-20210616093504030" style="zoom:67%;" />



1 分析文本主题 => 打上标签

- 分类
- 推荐



2 分析电商评论

- 商家了解商品情况和用户感受
- 给用户打上标签，分析他们的购买意愿 => 个性化营销

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210616094211.png" alt="image-20210616094211186" style="zoom:67%;" />



小结：以上均属舆情分析

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210616094336.png" alt="image-20210616094336702" style="zoom:67%;" />



## [问答系统|Chatbot](https://www.bilibili.com/video/BV1cb411P746)

应用：

- 搜索引擎
- 智能客服
- 无人驾驶
- 智能音箱



类型1）通用型

Siri，Cortana等个人助手

- 定闹钟
- 查天气
- 提问题
- 闲聊



类型2）领域型

- 电商导购
- 医疗导诊
- 餐馆推荐机器人



### chatbot原理

功能库+百科库+闲聊库

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210616094721.png" alt="image-20210616094720768" style="zoom:67%;" />



一般使用文字交流

语言交流：加上语音识别 + 语音合成模块

- +音箱 => 智能音箱等智能设备



### [开发Siri](https://www.bilibili.com/video/BV1Jb411c7EQ)

背景|开源：大公司/专注NLP的公司都开放了可供普通人开发Chatbot的入口

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210616094931.png" alt="image-20210616094931045" style="zoom:67%;" />

1 设计功能 & 性格

功能|领域型

- 理财产品推荐
- 餐馆推荐
- 医疗顾问



Siri倾向于工具型

小冰倾向于闲聊型

性格决定了应答方式和风格



2 设计交流策略 & 撰写语料

设计话术：问出基本信息 => 更好地推荐

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210616095138.png" alt="image-20210616095138282" style="zoom:67%;" />

通用型需要的语料更多 => 平台提供了很多对话模板、词典



3 测试 & 生成 => 反复打磨，生成API => 配置到想用的地方@微信



创意|设计一个能玩文字游戏的chatbot

- 成语接龙



# 专用|作文评分

1 基于规则：比较死板

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210616095454.png" alt="image-20210616095454219" style="zoom:67%;" />

2 基于深度学习：需要更多的标注数据

学习大量已经评好分数的作文 => 训练评分模型



机器人：

- 不会疲惫
- 客观 => 识别抄袭、模仿@文娱界：作品版权纠纷



# [社会计算](https://www.bilibili.com/video/BV1et411C7zf)💡

背景：1994年 => 我们能用计算机，模拟一个真实的人类社会吗？

> 关于这个Idea，总是有人先我一步：自己要加倍努力啊！

1 社会的计算化

随着互联网的发展， 根据海量痕迹/数据 => 量化计算

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210616100119.png" alt="image-20210616100119714" style="zoom:67%;" />



2 计算的社会化

每个人参与信息互动：

- 搜索信息
- 贡献信息
- 利用网络互相启发、竞争合作

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210616100228.png" alt="image-20210616100228085" style="zoom:80%;" />



信息来源 + 信息处理NLP技术：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210616100310.png" alt="image-20210616100310150" style="zoom:67%;" />



基于上述信息 => 对社会建模，重现社会场景

- 进行社会学研究，寻找人类社会的**活动特征**
- 利用社会模型，进行社会实验 => 预测社会现象及其后果



基于社交网络：分析结构，构建人物节点图谱，挖掘社群

- 对社交网络上的群体行为、感情作研究、分析

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210616100516.png" alt="image-20210616100515619" style="zoom:80%;" />
