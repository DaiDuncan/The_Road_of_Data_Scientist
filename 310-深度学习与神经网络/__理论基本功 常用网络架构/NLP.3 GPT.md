# 一、[通俗理解](https://www.bilibili.com/video/BV1Jv411a7RB/?spm_id_from=333.788.recommend_more_video.2)

背景：

- Transformer的Encoder => BERT
- Transformer的Decoder => GPT

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210613170323.png" alt="image-20210613170322372" style="zoom:50%;" />

1 GPT-1 = Langauage Model + Fine Tuning

起初效果不如BERT => OpenAI没有放弃



2 GPT-2

- 提高网络层数 *48
- 提高数据量 40G

机器翻译、文本摘要等基于原始文本的任务

新闻写作这类无中生有的生成类任务



3 GPT-3 == Language Model + Knowledge + Text understanding&Organization语境理解、语言组织  => 商用：没有开源，需要申请API

- 提高数据量 45TB
- 1750亿参数 => 模型大小超过700G



1）Zero Shot learning零样本学习

- 输入问题 => 直接获得答案
- 输入英文+提示字符 => 直接得到对应的翻译答案

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210613170718.png" alt="image-20210613170717518" style="zoom:67%;" />



2）Generation

结构决定功能：Decoder的每一步输出都基于上一步输出的内容 => ==生成==是GPT最强大的功能

- 给出开头 => 根据文字的风格和内容不断续写 => 创作歌词、小说
- 音乐
- 文本、图像 => ==依据描述，输出图像==







