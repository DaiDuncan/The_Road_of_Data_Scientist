pudb提供了语法高亮，断点，调用栈，命令行等功能，在终端下可以非常方便的对python代码进行调试。

![image-20210123150755453](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210123150755.png)

- 1是代码区域
- 2是命令行区域，你可以在这里==主动查看各个变量的数据==，如果对这个命令行功能不满意，还可以通过`!`进入到python交互式解释器来进行操作
- 3是变量区域，可以观察变量的变化情况
- 4是栈区域，可以查看栈的信息

只要屏幕上的光标不在第2个区域，快捷键都可以使用



```python
### 安装环境
pip install pudb

### 启动工具调试代码
pudb3 test.py


### 前两行代码， set_trace()设置断点
from pudb import set_trace
set_trace()
lst = [1, 2, 3, 5, 7, 8, 10]

for item in lst:
    if item % 2 == 0:
        print(item)
```





## 使用ctrl + p 可以进入到工具设置界面

![image-20210123150855225](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210123150855.png)





## shift+? 可以进入help界面

详细的介绍了工具的快捷键，如何运行下一步，如果跳出当前函数等等

![image-20210123150913171](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210123150913.png)