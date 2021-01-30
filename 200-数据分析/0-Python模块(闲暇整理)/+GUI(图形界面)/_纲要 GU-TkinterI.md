Python支持多种图形界面的第三方库，包括：

- Tk
- wxWidgets
- Qt
- GTK

Python自带的库是支持Tk的Tkinter，使用Tkinter，无需安装任何包



# Tkinter

编写的Python代码会调用内置的Tkinter，Tkinter封装了访问Tk的接口；

Tk是一个图形库，支持多个操作系统，使用Tcl语言开发；

Tk会==调用操作系统提供的本地GUI接口==，完成最终的GUI。

=> 代码只需要调用Tkinter提供的接口就可以了



## 示例|GUI程序

### 1 导入Tkinter包的所有内容

```python
from tkinter import *
```



### 2 从`Frame`派生一个`Application`类

这是所有Widget的父容器：

```python
class Application(Frame):
    def __init__(self, master=None):
        Frame.__init__(self, master)
        self.pack()
        self.createWidgets()

    def createWidgets(self):
        self.helloLabel = Label(self, text='Hello, world!')
        self.helloLabel.pack()
        self.quitButton = Button(self, text='Quit', command=self.quit)
        self.quitButton.pack()
```

在GUI中，每个Button、Label、输入框等，都是一个Widget。

Frame则是可以容纳其他Widget的Widget，所有的Widget组合起来就是一棵树。



`pack()`方法把Widget加入到父容器中，并实现布局。`pack()`是最简单的布局

`grid()`可以实现更复杂的布局。



在`createWidgets()`方法中，我们创建一个`Label`和一个`Button`，当Button被点击时，触发`self.quit()`使程序退出。



### 3 实例化`Application`，并启动消息循环：

```python
app = Application()
# 设置窗口标题:
app.master.title('Hello World')
# 主消息循环:
app.mainloop()
```

GUI程序的主线程负责监听来自操作系统的消息，并依次处理每一条消息。因此，如果消息处理非常耗时，就需要在新线程中处理。



运行这个GUI程序，可以看到下面的窗口：

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210130214443.png" alt="image-20210130214443097" style="zoom:80%;" />



### 改进|输入文本

加入一个文本框，让用户可以输入文本，然后点按钮后，弹出消息对话框。

```python
from tkinter import *
import tkinter.messagebox as messagebox

class Application(Frame):
    def __init__(self, master=None):
        Frame.__init__(self, master)
        self.pack()
        self.createWidgets()

    def createWidgets(self):
        self.nameInput = Entry(self)
        self.nameInput.pack()
        self.alertButton = Button(self, text='Hello', command=self.hello)
        self.alertButton.pack()

    def hello(self):
        name = self.nameInput.get() or 'world'
        messagebox.showinfo('Message', 'Hello, %s' % name)

app = Application()
# 设置窗口标题:
app.master.title('Hello World')
# 主消息循环:
app.mainloop()
```

当用户点击按钮时，触发`hello()`：

- 通过`self.nameInput.get()`获得用户输入的文本后
- 使用`tkMessageBox.showinfo()`可以弹出消息对话框。

![image-20210130214621763](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210130214621.png)

---

Python内置的Tkinter可以满足基本的GUI程序的要求

如果是非常复杂的GUI程序，建议用==操作系统原生支持的语言和库==来编写。





