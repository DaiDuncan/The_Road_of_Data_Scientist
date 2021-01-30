pyttsx3是一个文字转语音的python库 [Link: 官方网站](https://pyttsx3.readthedocs.io/en/latest/engine.html)

 这个库是跨平台的，在mac os, windows都可以使用，安装方式如下

```text
pip3 install pyttsx3
```



我在mac上实验该模块，python版本是3.6,不论中文还是英文都可以正常发出声音

```python
import pyttsx3
engine = pyttsx3.init()

engine.say('Sally sells seashells by the seashore.')
engine.runAndWait()     # 必须使用这行代码才能发出语音
engine.say('语音合成开始')
engine.runAndWait()
```