《The Grammar of Graphics》：提出了一套图形语法，把图形元素抽象成可以自由组合的成分

- Hadley Wickham把这套想法在R中实现



## 作者介绍：Hadley Wickham

RStudio首席科学家，美国莱斯大学统计学助理教授，毕业于爱荷华州立大学统计系。

Hadley是R社区最活跃的人之一，代码风格独树一帜，致力于开发用于数据处理、分析、成像的工具，截至2012年已经开发了超过30个高质量的R软件包，比如ggplot2, lubridate,plyr, reshape2, stringr, httr等。



## ggplot2介绍

gplot2是R世界里相对还比较年轻的一个包，在它之前，官方R已经有自己的基础图形系统（graphics包）和网格图形系统（grid包），并且Deepayan Sarkar也开发了lattice包



### 设计理念

R的基础图形系统基本上是一个“纸笔模型”，即：一块画布摆在面前，你可以在这里画几个点，在那里画几条线，指哪儿画哪儿

后来lattice包的出现稍微改善了这种情况，你可以说，我要画散点图或直方图，并且按照某个分类变量给图中的元素上色，此时数据才在画图中扮演了一定的中心角色，我们不用去想具体这个点要用什么颜色（颜色会根据变量自动生成）

然而，lattice继承了R语言的一个糟糕特征，就是参数设置铺天盖地，足以让人窒息，光是一份xyplot()函数的帮助文档，恐怕就够我们消磨一天时间了，更重要的是，lattice仍然面向特定的统计图形，像基础图形系统一样，有直方图、箱线图、条形图等等，它没有一套可以让数据分析者说话的语法



- ==数据分析者是怎样说话的呢？==

他们从来不会说这条线用#FE09BE颜色，那个点用三角形状，他们只会说，把图中的线用数据中的职业类型变量上色，或图中点的形状对应性别变量。有时候他们画了一幅散点图，但马上他们发现这幅图太拥挤，最好是能具体看一下里面不同收入阶层的特征，所以他们会说，把这幅图拆成七幅小图，每幅图对应一个收入阶层。然后发现散点图的趋势不明显，最好加上回归直线，看看回归模型反映的趋势是什么，或者发现图中离群点太多，最好做一下对数变换，减少大数值对图形的主导性。

==从始至终，数据分析者都在数据层面上思考问题==，而不是拿着水彩笔和调色板在那里一笔一划作图，**而计算机程序员则倾向于画点画线**。Leland Wilkinson的著作在理论上改善了这种状况，他提出了一套图形语法，让我们在考虑如何构建一幅图形的时候不再陷在具体的图形元素里面，而是把图形拆分为一些互相独立并且可以自由组合的成分。这套语法提出来之后他自己也做了一套软件，但显然这套软件没有被广泛采用；幸运的是，Hadley Wickham在R语言中把这套想法巧妙地实现了。

为了说明这种语法的想法，我们考虑图形中的一个成分：坐标系。常见的坐标系有两种：笛卡尔坐标系和极坐标系。**在语法中，它们属于一个成分，可自由拆卸替换**。笛卡尔坐标系下的条形图实际上可以对应极坐标系下的饼图，因为条形图的高可以对应饼图的角度，本质上没什么区别。因此在ggplot2中，从一幅条形图过渡到饼图，只需要加极少量的代码，把坐标系换一下就可以了。如果我们用纸笔模型，则可以想象，这完全是不同的两幅图，一幅图里面要画的是矩形，另一幅图要画扇形。







### 发展过程

ggplot2是Hadley在爱荷华州立大学博士期间的作品，也是他博士论文的主题之一，实际上ggplot2还有个前身ggplot，但后来废弃了，某种程度上这也是Hadley写软件的特征，熟悉他的人就知道这不是他第一个“2”版本的包了（还有reshape2）

带2的包和原来的包在语法上会有很大的改动，基本上不兼容。尽管如此，他的R代码风格在R社区可谓独树一帜，尤其是他的代码结构很好，可读性很高，ggplot2是R代码抽象的一个杰作。

在用法方面，ggplot2也开创了一种奇特而绝妙的语法，那就是加号：一幅图形从背后的设计来说，是若干图形语法的叠加，从外在的代码来看，也是若干R对象的相加。这一点精妙尽管只是ggplot2系统的很小一部分，但我个人认为没有任何程序语言可比拟，它对作为泛型函数的加号的扩展只能用两个字形容：绝了。





### 目录

1.简介
2.从qplot开始入门
3.语法突破
4.==用图层构建图像==
5.工具箱
6.标度、坐标轴和图例
7.定位
8.精雕细琢
9.数据操作
10.减少重复性工作
附录A 不同语法间的转换
附录B 图形属性的定义
附录C 用grid操作图形





# 教程|ggplot2基本要素

## 数据（Data）和映射（Mapping）

```R
require(ggplot2)
data(diamonds)
set.seed(42)
small <- diamonds[sample(nrow(diamonds), 1000), ]
head(small)


summary(small)
p <- ggplot(data = small, mapping = aes(x = carat, y = price))
p + geom_point()
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210106191704.png" alt="image-20210106191704246" style="zoom:80%;" />

```R
### cut 映射到形状属性
p <- ggplot(data=small, mapping=aes(x=carat, y=price, shape=cut)) 
p+geom_point()


### color映射颜色属性
p <- ggplot(data=small, mapping=aes(x=carat, y=price, shape=cut, colour=color))
p+geom_point()
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210106191818.png" alt="image-20210106191817981" style="zoom:67%;" />





## 几何对象（Geometric）

上面的例子中，各种属性映射由ggplot函数执行，只需要加一个图层，使用geom_point()告诉ggplot要画散点，于是所有的属性都映射到散点上

geom_point()完成的就是几何对象的映射

- 如geom_histogram用于直方图
- geom_bar用于画柱状图
- geom_boxplot用于画箱式图等等。



### 直方图

```R
ggplot(small)+geom_histogram(aes(x=price))

### 设置属性：填充颜色，cut
ggplot(small)+geom_histogram(aes(x=price, fill=cut))
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210106191955.png" alt="image-20210106191955078" style="zoom:67%;" />

### 柱状图/条形图

```R
ggplot(small)+geom_bar(aes(x=clarity))
```

柱状图两个要素，一个是分类变量，一个是数目



### **密度函数图**/kde

```R
ggplot(small)+geom_density(aes(x=price, colour=cut))

ggplot(small)+geom_density(aes(x=price,fill=clarity))
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210106192104.png" alt="image-20210106192103918" style="zoom:50%;" />



### 箱线图

数据量比较大的时候，用直方图和密度函数图是表示数据分布的好方法

在数据量较少的时候，比如很多的生物实验，很多时候大家都是使用柱状图+errorbar的形式来表示，不过这种方法的信息量非常低，被Nature Methods吐槽，这种情况推荐使用boxplot

```R
ggplot(small)+geom_boxplot(aes(x=cut, y=price,fill=color))
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210106192145.png" alt="image-20210106192145717" style="zoom:67%;" />

### 其他几何图形

```R
geom_abline 	geom_area 	
geom_bar 		geom_bin2d
geom_blank 		geom_boxplot 	
geom_contour 	geom_crossbar
geom_density 	geom_density2d 	
geom_dotplot 	geom_errorbar
geom_errorbarh 	geom_freqpoly 	
geom_hex 		geom_histogram
geom_hline 		geom_jitter 	
geom_line 		geom_linerange
geom_map 		geom_path 	
geom_point 		geom_pointrange
geom_polygon 	geom_quantile 	
geom_raster 	geom_rect
geom_ribbon 	geom_rug 	
geom_segment 	geom_smooth
geom_step 		geom_text 	
geom_tile 		geom_violin
geom_vline
```





## 标尺（Scale）

在对图形属性进行映射之后，使用标尺可以控制这些属性的显示方式，比如坐标刻度，可能通过标尺，将坐标进行对数变换；比如颜色属性，也可以通过标尺，进行改变

```R
### 风格：图层相加
# 设置scale的尺度与颜色
ggplot(small)+geom_point(aes(x=carat, y=price, shape=cut, colour=color))+scale_y_log10()+scale_colour_manual(values=rainbow(7))
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210106192250.png" alt="image-20210106192250514" style="zoom:80%;" />



## 统计变换（Statistics）

```R
ggplot(small, aes(x=carat, y=price))+geom_point()+scale_y_log10()+stat_smooth()
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210106192357.png" alt="image-20210106192357235" style="zoom:80%;" />

这里就不按颜色、切工来分了，不然ggplot会按不同的分类变量分别做回归，图就很乱，==如果我们需要这样做，我们可以使用分面==

aes所提供的参数，就通过ggplot提供，而不是提供给geom_point，因为ggplot里的参数，相当于全局变量

geom_point()和stat_smooth()都知道x,y的映射，如果只提供给geom_point()，则相当于是局部变量，geom_point知道这种映射，而stat_smooth不知道，当然你再给stat_smooth也提供x,y的映射，不过共用的映射，还是提供给ggplot好



### ggplot2的其他统计变换

```R
stat_abline       stat_contour      stat_identity     stat_summary
stat_bin          stat_density      stat_qq           stat_summary2d
stat_bin2d        stat_density2d    stat_quantile     stat_summary_hex
stat_bindot       stat_ecdf         stat_smooth       stat_unique
stat_binhex       stat_function     stat_spoke        stat_vline
stat_boxplot      stat_hline        stat_sum          stat_ydensity
```

统计变换是非常重要的功能，我们可以自己写函数，基于原始数据做某种计算





## 坐标系统（Coordinante）

坐标系统控制坐标轴，可以进行变换，例如XY轴翻转，笛卡尔坐标和极坐标转换，以满足我们的各种需求

- 坐标轴翻转由coord_flip()实现

```R
ggplot(small)+geom_bar(aes(x=cut, fill=cut))+coord_flip()
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210106192544.png" alt="image-20210106192544389" style="zoom:67%;" />

- 转换成极坐标可以由coord_polar()实现

```R
ggplot(small)+geom_bar(aes(x=factor(1), fill=cut))+coord_polar(theta="y")
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210106192608.png" alt="image-20210106192608136" style="zoom:67%;" />

这也是为什么之前介绍常用图形画法时没有提及饼图的原因，饼图实际上就是柱状图，只不过是使用极坐标而已，柱状图的高度，对应于饼图的弧度，饼图并不推荐，因为==人类的眼睛比较弧度的能力比不上比较高度（柱状图）==

- 靶心图

```R
ggplot(small)+geom_bar(aes(x=factor(1), fill=cut))+coord_polar()
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210106192649.png" alt="image-20210106192649105" style="zoom:67%;" />

- 风玫瑰图(windrose)

```R
ggplot(small)+geom_bar(aes(x=clarity, fill=cut))+coord_polar()
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210106192711.png" alt="image-20210106192711006" style="zoom:67%;" />



## 图层（Layer）

photoshop流行的原因在于PS 3.0时引入图层的概念，ggplot的牛B之处在于使用+号来叠加图层，这堪称是==泛型编程的典范==

### 典例|[蝙蝠侠logo](http://ygc.name/2011/08/14/bat-man/)

```R
require(ggplot2)
f1data.frame(x=x,y=y)
	d -3*sqrt(33)/7,]
	return(d)
}
 
x1data.frame(x2=x2, y2=y2)
p2data.frame(x3=x3, y3=y3)
p3data.frame(x4=x4,y4=y4)
p4data.frame(x5=x5,y5=y5)
p5data.frame(x6=x6,y6=y6)
p6
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210106192805.png" alt="image-20210106192805037" style="zoom:67%;" />







## 分面（Facet）

按照某种给定的条件，对数据进行分组，然后分别画图

```R
ggplot(small, aes(x=carat, y=price))+geom_point(aes(colour=cut))+scale_y_log10() +facet_wrap(~cut)+stat_smooth()
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210106192841.png" alt="image-20210106192840869" style="zoom:67%;" />





## 主题（Theme）

对图进行定制，像title, xlab, ylab这些高频需要用到的，自不用说，ggplot2提供了ggtitle(), xlab()和ylab()来实现

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210106192859.png" alt="image-20210106192859479" style="zoom:80%;" />

但是这个远远满足不了需求，我们需要改变字体，字体大小，坐标轴，背景等各种元素，这需要通过theme()函数来完成

ggplot2提供一些已经写好的主题，比如theme_grey()为默认主题，我经常用的theme_bw()为白色背景的主题，还有theme_classic()主题，和R的基础画图函数较像



### ggthemes包提供的其他主题

```R
theme_economist theme_economist_white
theme_wsj 	 	theme_excel
theme_few 	 	theme_foundation
theme_igray 	theme_solarized
theme_stata 	theme_tufte

require(ggthemes)
p + theme_wsj()
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210106192950.png" alt="image-20210106192950690" style="zoom:50%;" />



## 二维密度图

使用diamonds数据集的一个子集，如果使用全集，==数据量太大，画出来散点就糊了==，这种情况可以使用二维密度力来呈现

```R
ggplot(diamonds, aes(carat, price))+ stat_density2d(aes(fill = ..level..), geom="polygon")+ scale_fill_continuous(high='darkred',low='darkgreen')
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210106193031.png" alt="image-20210106193031235" style="zoom:67%;" />



## ggplot2实战

```R
### 蝴蝶图
theta data.frame(x=radius*sin(theta), y=radius*cos(theta))
ggplot(dd, aes(x, y))+geom_path()+theme_null()+xlab("")+ylab("")
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210106193106.png" alt="image-20210106193105939" style="zoom:80%;" />



# #参考文献

[Link: 介绍ggplot2](https://www.plob.org/article/7264.html)

[Link: 豆瓣|ggplot2：数据分析与图形艺术](https://book.douban.com/subject/24527091/)

[Link: R语言应用系列 (共10册)](https://book.douban.com/series/26627),

- ==《数据科学中的并行计算》==
- 《空间数据分析与R语言实践》
- 《ggplot2：数据分析与图形艺术（第2版）》
- 《R语言在商务分析中的应用》
- 《R语言初学者指南》 等





