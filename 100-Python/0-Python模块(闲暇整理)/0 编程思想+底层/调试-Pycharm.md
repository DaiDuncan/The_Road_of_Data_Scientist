调试功能主要包括

- 设置断点, 单步执行
- 查看调用栈信息, 查看变量的值
- 调试功能让程序在某一行代码上暂停，通过对这一刻的上下文环境进行检查，可以定位程序出问题的原因

调试程序是程序员的日常，再牛逼的人，也不可能一次性把程序写对，因此，写完一个函数或者一段代码后，你需要测试自己的代码，如果有问题，就必须进行调试，也就是debug



# 断点

设置一个断点，程序运行到这里，就会停下来

之后，你就可以单步调试，所谓单步调试，就是你让程序执行一行，它就执行一行。



- 以pycharm为例，只需要左键点击代码左侧标识代码行数的区域即可

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210123102842.png" alt="image-20210123102842189" style="zoom:80%;" />



## debug模式运行

想要单步调试程序，需要进入debug模式运行程序

菜单栏 => Run => Debug...

![image-20210123103049061](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210123103049.png)



### 1 调试区域

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210123103121.png" alt="image-20210123103121418" style="zoom:80%;" />

| 5个功能键         |                                                              |
| ----------------- | ------------------------------------------------------------ |
| step over         | 点击一下，程序就会执行一行                                   |
| step into         | 如果这一行有函数，点击它就会进入到函数中                     |
| step into my code | 如果这一行有函数，既有第三方库的函数，也有自己编写的函数，那么会优先进入到自己的函数里 |
| step out          | 从函数里退出来                                               |
| run to cursor     | 直接运行到下一处断点                                         |



### 2 调用栈区域

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210123103242.png" alt="image-20210123103241837" style="zoom:80%;" />

左侧的两个红框内的按钮：

- 绿色箭头的按钮是resume progrom,是恢复程序执行，如果后续代码没有断点了，程序就会正常执行直到结束
- 红方块的按钮是 stop，停止程序

调用栈区域展示的，是调用栈的信息，通过它就能知道从程序开始运行到当前代码，==经过了哪些调用关系==





### 3 变量查看区域

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210123103339.png" alt="image-20210123103339131" style="zoom:80%;" />

查看当前程序里的每一个变量的值，非常的方便

还可以自己添加表达式，查看自己关注的值，比如绿色框内的1%2 就是我通过左侧红色框内的+按钮添加上去的，添加的时候，写完表达式后，要点击回车才能完成添加。



除了这里可以查看变量的值，在程序页面里，debug模式下，鼠标滑动到变量上，也可以查看变量的值



除了pycharm提供的调试工具

- 还可以使用python提供的pudb，不过是命令行的，不如图形界面友好
- 还有一个ipdb，比pdb高级一些，有高亮显示



# #参考文献

[Link: 酷python](http://www.coolpython.net/python_primary/exection/debug.html)