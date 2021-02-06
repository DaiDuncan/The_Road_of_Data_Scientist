在1966年，Seymour Papert和Wally Feurzig发明了一种专门给儿童学习编程的语言——[LOGO语言](https://baike.baidu.com/item/logo/4689862)，它的特色就是通过编程指挥一个小海龟（turtle）在屏幕上绘图。



海龟绘图（Turtle Graphics）后来被移植到各种高级语言中，Python内置了turtle库，基本上100%复制了原始的Turtle Graphics的所有功能。





# 示例|指挥小海龟绘制一个长方形

```python
# 导入turtle包的所有内容:
from turtle import *

# 设置笔刷宽度:
width(4)

# 前进:
forward(200)
# 右转90度:
right(90)

# 笔刷颜色:
pencolor('red')
forward(100)
right(90)

pencolor('green')
forward(200)
right(90)

pencolor('blue')
forward(100)
right(90)

# 调用done()使得窗口等待被关闭，否则将立刻关闭窗口:
done()
```

在命令行运行上述代码，会自动弹出一个绘图窗口，然后绘制出一个长方形：

![image-20210130214851016](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210130214851.png)



- 调用`width()`函数可以设置笔刷宽度
- 调用`pencolor()`函数可以设置颜色

绘图完成后，记得调用`done()`函数，让窗口进入消息循环，等待被关闭。否则，由于Python进程会立刻结束，将导致窗口被立刻关闭。



## 循环|绘制5个五角星

```python
from turtle import *

def drawStar(x, y):
    pu()
    goto(x, y)
    pd()
    # set heading: 0
    seth(0)
    for i in range(5):
        fd(40)
        rt(144)

for x in range(0, 250, 50):
    drawStar(x, 0)

done()
```

![image-20210130214948798](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210130214948.png)







# 递归|分型树

```python
from turtle import *

# 设置色彩模式是RGB:
colormode(255)

lt(90)

lv = 14
l = 120
s = 45

width(lv)

# 初始化RGB颜色:
r = 0
g = 0
b = 0
pencolor(r, g, b)

penup()
bk(l)
pendown()
fd(l)

def draw_tree(l, level):
    global r, g, b
    # save the current pen width
    w = width()

    # narrow the pen width
    width(w * 3.0 / 4.0)
    # set color:
    r = r + 1
    g = g + 2
    b = b + 3
    pencolor(r % 200, g % 200, b % 200)

    l = 3.0 / 4.0 * l

    lt(s)
    fd(l)

    if level < lv:
        draw_tree(l, level + 1)
    bk(l)
    rt(2 * s)
    fd(l)

    if level < lv:
        draw_tree(l, level + 1)
    bk(l)
    lt(s)

    # restore the previous pen width
    width(w)

speed("fastest")

draw_tree(l, 4)

done()
```

![image-20210130215019303](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210130215019.png)

