[link: 公众号|原文链接2020.04.24](https://mp.weixin.qq.com/s?__biz=MzIwODI2NDkxNQ==&mid=2247488260&idx=2&sn=479a4dd75c75f15ad342ba0506a8a8d3&chksm=970498b8a07311aee5d27f0a5e73e65f2f5be23bbcfa57374056ebfdba46c88fcdb58fe2c019&scene=21#wechat_redirect)



**大纲如下**：

- PageRank的简化模型和计算原理
- PageRank的随机浏览模型（解决等级沉没和等级泄露）
- PageRank给我们带来的启发很重要 （人脉杠杆）
- PageRank实战 （分析希拉里邮件里面的人物关系，看看谁和希拉里交往最活跃）

---

# 模型原理

提到一个词？想必大家一定不陌生，那就是从众心理。

> 比如，当你某一家饭店，或者理发店，或者小吃店里面的顾客非常多，每天爆满的时候，心理一定会想，这家店想必不错，要不然不可能这么多人，我也进去试试。 
>
> 或者，在淘宝上买某个商品的时候，肯定是喜欢挑人多的店铺，好评量高的店铺买的放心等等吧。 
>
> 所以当我们在生活中遇到艰难选择的时候，往往喜欢看看别人是怎么做的，一般都会选大部分人的选择。这其实就是一种从众。



这些店铺也好，选择也罢，其实都是通过很多人的投票进而提高了自己的影响力，再比如说，微博上如何去衡量一个人的影响力呢？ 

我们习惯看他的粉丝，如果他的粉丝多，并且里面都是一些大V，明星的话，很可能这个人的影响力会比较强。 

再比如说，职场上如何衡量一个人的影响力呢？我们习惯看与他交往的人物， 如果和他交往的都是像马云，王健林，马化腾这样的人物，那么这个人的影响力估计也小不了。

再比如说，如何判断一篇论文好？我们习惯看他的引用次数，或者影响因子，高的论文就比较好等等。

但是你只知道吗？其实我们的这种方式就在用PageRank算法的思想了，只不过我们没有发觉罢了，所谓的**算法来源于生活，并服务于生活**就是这个道理。



## 起源

在 1998 年之前，搜索引擎的体验并不好。

早期的搜索引擎，会遇到下面的两类问题：

1. 返回结果质量不高：搜索结果不考虑网页的质量，而是通过**时间顺序**进行检索；
2. 容易被人钻空子：搜索引擎是基于检索词进行检索的，页面中检索词出现的频次越高，匹配度越高，这样就会出现网页作弊的情况。有些网页为了增加搜索引擎的排名，故意增加某个检索词的频率。



基于这些缺陷，当时 Google 的创始人拉里·佩奇提出了 PageRank 算法，目的就是要找到优质的网页，这样 Google 的排序结果不仅能找到用户想要的内容，而且还会从众多网页中筛选出权重高的呈现给用户。

==其灵感就是论文影响力因子的启发==。



## 简化模型

假设一共有 4 个网页 A、B、C、D。它们之间的链接信息如图所示：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210414150115.webp" alt="图片" style="zoom: 50%;" />

首先先知道两个概念：

- 出链指的是链接出去的链接。
- 入链指的是链接进来的链接。
- 比如图中 A 节点有 2 个入链，3 个出链。



那么我们如何计算一个网页的影响力或者重要程度呢？

简单来说，一个网页的影响力 = 所有入链集合的页面的加权影响力之和，用公式表示为：

- u 为待评估的页面，
- Bu 为==页面 u 的入链集合==。

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210414150210.png)

针对入链集合中的任意页面 v，它能给 u 带来的影响力是其自身的影响力 PR(v) ==除以 v 页面的出链数量==，即页面 v 把影响力 PR(v) 平均分配给了它的出链，这样统计==所有能给 u 带来链接的页面 v==，得到的总和就是网页 u 的影响力，即为 PR(u)。



这是在说什么？ 只需要先明白两点：

- 如果一个页面，入链比较多，也就是跳转过去的概率比较大的时候，这个页面往往就比较重要，影响力高一些（这就类似于购买商品的人越多，这个商品就貌似越好一样）
- 如果某个页面本身的影响力很大，那么也会相应的增加它出链指向的页面影响力。（这个就类似于你的朋友都是大牛的话，你在别人心目中的印象也会提升）



所以你能看到，出链会给被链接的页面赋予影响力，当我们统计了一个网页链出去的数量，也就是统计了这个网页的跳转概率。



在这个例子中，你能看到 A 有三个出链分别链接到了 B、C、D 上。

- 那么当用户访问 A 的时候，就有跳转到 B、C 或者 D 的可能性，跳转概率均为 1/3。

B 有两个出链，链接到了 A 和 D 上，跳转概率为 1/2。

这样，我们可以得到 A、B、C、D 这==四个网页的转移矩阵 M==：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210414150436.png" alt="image-20210414150436587" style="zoom:50%;" />

这个转移矩阵，每一行代表了每个节点==入链上的权重==（大家浏览别的页面的时候，有多大的概率能跳到我这来）。

每一列代表了每个节点对其他页面的影响力的赋予程度（也就是大家浏览我这，有多大的概率跳到别的页面上去）





有了这个转移矩阵，我们就可以计算每个页面的影响力是多少了。

比如上图A页面的影响力怎么计算呢？其实我们是通过他的**入链点B、C的影响力**去计算的，也就是我们的那个公式。

==开始我们假设四个页面的影响力相同，都是1/4==。则A第一次的影响力这么想，

- B有两条出链，那么会给我传过它影响力的1/2。

- C有一条出链，那么会把它的影响力全传给A。

故：PR(A) = 1/4 * 1/2 + 1/4
这是一次迭代。你发现了吗？这其实就是转移矩阵M的第一行 * 一个全为1/4的列向量得到的（向量乘法）





所以如果我们利用向量的乘法原理的话，只需要一个向量乘法就可以计算出每个页面的影响力了。

我们假设 A、B、C、D 四个页面的初始影响力都是相同的，即：$w_0=[1/4, 1/4, 1/4, 1/4]^T$

当进行第一次转移之后，各页面的影响力 w1 变为：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210414150711.png" alt="image-20210414150711060" style="zoom:80%;" />

然后我们再用转移矩阵乘以 w1 得到 w2 结果，直到第 n 次迭代后 wn 影响力不再发生变化，可以收敛到 (0.3333，0.2222，0.2222，0.2222），也就是对应着 A、B、C、D 四个页面最终平衡状态下的影响力。



你能看出 A 页面相比于其他页面来说权重更大，也就是 PR 值更高。

而 B、C、D 页面的 PR 值相等。

至此，我们模拟了一个简化的 PageRank 的计算过程，也就是PageRank简化模型的原理了。





但是你发现了吗？虽然这个模型简单，但是有问题的，

- 比如万一我有一个网页只有入链没有出链怎么办？ 
- 万一我有一个网页只有出链，没有入链怎么办？



上面的两个问题分别对应着**等级泄露和等级沉没**。

1. 等级泄露（Rank Leak）

如果一个网页没有出链，就像是一个黑洞一样，吸收了其他网页的影响力而不释放，最终会**导致其他网页的 PR 值为 0**。

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210414150911.jpeg" alt="图片" style="zoom:50%;" />



2. 等级沉没（Rank Sink）

如果一个网页只有出链，没有入链（如下图所示），计算的过程迭代下来，会**导致这个网页的 PR 值为** 0（也就是不存在公式中的 V）。

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210414151005.webp" alt="图片" style="zoom:50%;" />



> 这其实就和练功一样，如果其他所有人把功力传给一个人，而这个人只接收别人的功力并不传给其他人，那么这个人必定非常强大，其他人慢慢的就会功力丧失， 武侠小说里面的高手很多是这么炼成的吧（吸星大法这样的）。
>
> 如果一个人只往外传功力，不接收外来人的功力，比如给人疗伤的时候，那么这个人很快也就会功力丧失了。（武侠小说中疗伤的时候，某个大侠给人疗着疗着自己就昏过去了）





# 模型|随机浏览

为了解决简化模型中存在的等级泄露和等级沉没的问题，拉里·佩奇提出了 PageRank 的随机浏览模型。

他假设了这样一个场景：用户**并不都是按照跳转链接的方式来上网**，还有一种可能是不论当前处于哪个页面，都有概率访问到其他任意的页面，比如说用户就是要直接输入网址访问其他页面，虽然这个概率比较小。



所以他定义了阻尼因子 d，这个因子代表了用户按照跳转链接来上网的概率，通常可以取一个固定值 0.85，而 1-d=0.15 则代表了用户**不是通过跳转链接的方式来访问网页的**，比如直接输入网址。

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210414151044.webp)

其中 N 为网页总数，这样我们又可以重新迭代网页的权重计算了，因为加入了阻尼因子 d，一定程度上解决了等级泄露和等级沉没的问题。

通过数学定理（这里不进行讲解）也可以证明，最终 PageRank 随机浏览模型是可以收敛的，也就是可以得到一个稳定正常的 PR 值。



# 启发

PageRank 可以说是 Google 搜索引擎重要的技术之一，在 1998 年帮助 Google 获得了搜索引擎的领先优势，**现在 PageRank 已经比原来复杂很多**，但它的思想依然能带给我们很多启发。



比如，如果你想要自己的媒体影响力有所提高，就==尽量要混在大 V 圈中==；

如果想找到高职位的工作，就尽量结识公司高层，或者认识更多的猎头，因为猎头和很多高职位的人员都有链接关系。 



这也就是很重要的理论：**加人脉杠杆，搞定一人，撬动千人**。（好吧，这个只是逆袭学习理论中的一小小部分，后面打算也会把逆袭学习理论整理出来，大家共勉。）







# 项目实战

## 1 使用工具实现PageRank算法

PageRank 算法工具在 sklearn 中并不存在，我们需要找到新的工具包。

实际上有一个==关于图论和网络建模的工具叫 NetworkX==，它是用 Python 语言开发的工具，内置了常用的图与网络分析算法，可以方便我们进行网络数据分析。

上面举了一个网页权重的例子，假设一共有 4 个网页 A、B、C、D，它们之间的链接信息如图所示：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210414150115.webp" alt="图片" style="zoom: 50%;" />

```python
import networkx as nx
# 创建有向图
G = nx.DiGraph()
# 有向图之间边的关系
edges = [("A", "B"), ("A", "C"), ("A", "D"), ("B", "A"), ("B", "D"), ("C", "A"), ("D", "B"), ("D", "C")]
for edge in edges:
    G.add_edge(edge[0], edge[1])
pagerank_list = nx.pagerank(G, alpha=1)
print("pagerank值是：\n", pagerank_list)
```

结果如下：

pagerank值是：
{'A': 0.33333396911621094, 'B': 0.22222201029459634, 'C': 0.22222201029459634, 'D': 0.22222201029459634}



关键代码就nx.pagerank(G, alpha=1) 这一句话， 这里的alpha就是我们上面说的阻尼因子，代表用户按照跳转链接的概率。默认是0.85。

这里是1，表示我们都是用跳转链接，不直接输入网址的那种。





来看下 NetworkX 工具都有哪些常用的操作：

- **关于图的创建**图可以分为无向图和有向图，在 NetworkX 中**分别采用不同的函数**进行创建。
  - 无向图指的是不用节点之间的边的方向，使用 nx.Graph() 进行创建；
  - 有向图指的是节点之间的边是有方向的，使用 nx.**DiGraph**() 来创建。
  - 在上面这个例子中，存在 A→D 的边，但不存在 D→A 的边。
- **关于节点的增加、删除和查询**
  - 如果想在网络中增加节点，可以使用 G.add_node(‘A’) 添加一个节点，也可以使用 G.add_nodes_from([‘B’,‘C’,‘D’,‘E’]) 添加节点集合。
  - 如果想要删除节点，可以使用 G.remove_node(node) 删除一个指定的节点，也可以使用 G.remove_nodes_from([‘B’,‘C’,‘D’,‘E’]) 删除集合中的节点。
  - 那么该如何查询节点呢？如果你想要得到图中所有的节点，就可以使用 G.nodes()，也可以用 G.number_of_nodes() 得到图中节点的个数。
- **关于边的增加、删除、查询**
  - 增加边与添加节点的方式相同，使用 G.add_edge(“A”, “B”) 添加指定的“从 A 到 B”的边，也可以使用 add_edges_from 函数从边集合中添加。
  - 我们也可以做一个加权图，也就是说边是带有权重的，使用add_weighted_edges_from 函数从带有权重的边的集合中添加。
    - 在这个函数的参数中接收的是 1 个或多个三元组[u,v,w]作为参数，u、v、w 分别代表起点、终点和权重。
  - 另外，我们可以使用 remove_edge 函数和 remove_edges_from 函数删除指定边和从边集合中删除。
  - 另外可以使用 edges() 函数访问图中所有的边，使用 number_of_edges() 函数得到图中边的个数。



以上是关于图的基本操作，如果我们创建了一个图，并且对节点和边进行了设置，就可以找到其中有影响力的节点，原理就是通过 PageRank 算法，使用 nx.pagerank(G) 这个函数，函数中的参数 G 代表创建好的图。







## 实战|**用 PageRank 揭秘希拉里邮件中的人物关系**

数据集可以再这里下载：https://github.com/cystanford/PageRank

> 整个数据集由三个文件组成：Aliases.csv，Emails.csv 和 Persons.csv，其中 Emails 文件记录了所有公开邮件的内容，发送者和接收者的信息。
>
> Persons 这个文件统计了邮件中所有人物的姓名及对应的 ID。
>
> 因为姓名存在别名的情况，为了将邮件中的人物进行统一，我们还需要用 Aliases 文件来查询别名和人物的对应关系。
> 整个数据集包括了 9306 封邮件和 513 个人名，数据集还是比较大的。
>
> 不过这一次我们不需要对邮件的内容进行分析，只需要通过邮件中的发送者和接收者（对应 Emails.csv 文件中的 MetadataFrom 和 MetadataTo 字段）来绘制整个关系网络。
>
> 因为涉及到的人物很多，因此我们需要通过 PageRank 算法计算每个人物在邮件关系网络中的权重，最后筛选出来最有价值的人物来进行关系网络图的绘制。



流程步骤：

![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210414151455.webp)

1. 首先我们需要加载数据源；
2. 在准备阶段：我们需要对数据进行探索，在数据清洗过程中，因为**邮件中存在别名的情况**，因此我们需要统一人物名称。
   - 另外邮件的正文并不在我们考虑的范围内，只统计邮件中的发送者和接收者，因此我们筛选 MetadataFrom 和 MetadataTo 这两个字段作为特征。
   - 同时，发送者和接收者可能存在多次邮件往来，需要设置权重来统计两人邮件往来的次数。次数越多代表这个边（从发送者到接收者的边）的权重越高；
3. 在挖掘阶段：我们主要是对已经设置好的网络图进行 PR 值的计算，但邮件中的人物有 500 多人，有些人的权重可能不高，我们需要筛选 PR 值高的人物，绘制出他们之间的往来关系。
   - 在可视化的过程中，我们可以**通过节点的 PR 值来绘制节点的大小**，PR 值越大，节点的绘制尺寸越大。

```python
# -*- coding: utf-8 -*-
# 用 PageRank 挖掘希拉里邮件中的重要任务关系
import pandas as pd
import networkx as nx
import numpy as np
from collections import defaultdict
import matplotlib.pyplot as plt
# 数据加载
emails = pd.read_csv("./input/Emails.csv")
# 读取别名文件
file = pd.read_csv("./input/Aliases.csv")
aliases = {}
for index, row in file.iterrows():
    aliases[row['Alias']] = row['PersonId']
# 读取人名文件
file = pd.read_csv("./input/Persons.csv")
persons = {}
for index, row in file.iterrows():
    persons[row['Id']] = row['Name']
# 针对别名进行转换
def unify_name(name):
    # 姓名统一小写
    name = str(name).lower()
    # 去掉, 和 @后面的内容
    name = name.replace(",","").split("@")[0]
    # 别名转换
    if name in aliases.keys():
        return persons[aliases[name]]
    return name
# 画网络图
def show_graph(graph, layout='spring_layout'):
    # 使用 Spring Layout 布局，类似中心放射状
    if layout == 'circular_layout':
        positions=nx.circular_layout(graph)
    else:
        positions=nx.spring_layout(graph)
    # 设置网络图中的节点大小，大小与 pagerank 值相关，因为 pagerank 值很小所以需要 *20000
    nodesize = [x['pagerank']*20000for v,x in graph.nodes(data=True)]
    # 设置网络图中的边长度
    edgesize = [np.sqrt(e[2]['weight']) for e in graph.edges(data=True)]
    # 绘制节点
    nx.draw_networkx_nodes(graph, positions, node_size=nodesize, alpha=0.4)
    # 绘制边
    nx.draw_networkx_edges(graph, positions, edge_size=edgesize, alpha=0.2)
    # 绘制节点的 label
    nx.draw_networkx_labels(graph, positions, font_size=10)
    # 输出希拉里邮件中的所有人物关系图
    plt.show()
# 将寄件人和收件人的姓名进行规范化
emails.MetadataFrom = emails.MetadataFrom.apply(unify_name)
emails.MetadataTo = emails.MetadataTo.apply(unify_name)
# 设置遍的权重等于发邮件的次数
edges_weights_temp = defaultdict(list)
for row in zip(emails.MetadataFrom, emails.MetadataTo, emails.RawText):
    temp = (row[0], row[1])
    if temp notin edges_weights_temp:
        edges_weights_temp[temp] = 1
    else:
        edges_weights_temp[temp] = edges_weights_temp[temp] + 1
# 转化格式 (from, to), weight => from, to, weight
edges_weights = [(key[0], key[1], val) for key, val in edges_weights_temp.items()]
# 创建一个有向图
graph = nx.DiGraph()
# 设置有向图中的路径及权重 (from, to, weight)
graph.add_weighted_edges_from(edges_weights)
# 计算每个节点（人）的 PR 值，并作为节点的 pagerank 属性
pagerank = nx.pagerank(graph)
# 将 pagerank 数值作为节点的属性
nx.set_node_attributes(graph, name = 'pagerank', values=pagerank)
# 画网络图
show_graph(graph)

# 将完整的图谱进行精简
# 设置 PR 值的阈值，筛选大于阈值的重要核心节点
pagerank_threshold = 0.005
# 复制一份计算好的网络图
small_graph = graph.copy()
# 剪掉 PR 值小于 pagerank_threshold 的节点
for n, p_rank in graph.nodes(data=True):
    if p_rank['pagerank'] < pagerank_threshold:
        small_graph.remove_node(n)
# 画网络图,采用circular_layout布局让筛选出来的点组成一个圆
show_graph(small_graph, 'circular_layout')
```

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210414151617.webp" alt="图片" style="zoom:80%;" />





![图片](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210414151640.webp)



针对代码中的几个模块个简单的说明：

1. **函数定义**人物的名称需要统一，因此设置了 unify_name 函数，同时设置了 show_graph 函数将网络图可视化。
   - NetworkX 提供了多种可视化布局，这里使用 spring_layout 布局，也就是呈中心放射状。
   - 除了 spring_layout 外，NetworkX 还有另外三种可视化布局，
     - circular_layout（在一个圆环上均匀分布节点），
     - random_layout（随机分布节点 ），
     - shell_layout（节点都在同心圆上）。
2. **计算边权重**邮件的发送者和接收者的邮件往来可能不止一次，我们需要用两者之间邮件往来的次数计算这两者之间边的权重，所以用 edges_weights_temp 数组存储权重。
   - 而上面介绍过在 NetworkX 中添加权重边（即使用 add_weighted_edges_from 函数）的时候，接受的是 u、v、w 的三元数组，因此我们还需要对格式进行转换，具体转换方式见代码。
3. **PR 值计算及筛选** 使用 nx.pagerank(graph) 计算了节点的 PR 值。
   - 由于节点数量很多，我们设置了 PR 值阈值，即 pagerank_threshold=0.005，然后遍历节点，删除小于 PR 值阈值的节点，形成新的图 small_graph，
   - 最后对 small_graph 进行可视化（对应运行结果的第二张图）。





# 总结

今天用了一天的时间，学习PageRank算法的理论，并且通过理论做了一个实战，运用了一下，又抓紧记录，完成了一篇博客，心理还是很高兴的。

学习知识的过程就是这样，如果只是单纯的输入，没有一点输出的话，那么很快就会忘记，输出一遍，至少在大脑里面停留了片刻，这里也留下了自己的踪迹，就像那就话说的：天空中没有鸟的痕迹，但是我已经飞过。



**努力学习，快速感悟，敢于尝试**，这样我们才能做到永远年轻，永远热泪盈眶。加油！



# #参考文献

- http://note.youdao.com/noteshare?id=3568b0c7f636cb21f260cd9f83ea4817&sub=B90987F55A05445B8E07476DDFCB6D62
- https://time.geekbang.org/
- https://blog.csdn.net/u013007900/article/details/88961913
- https://blog.csdn.net/leadai/article/details/81230557