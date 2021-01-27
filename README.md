# The_Road_of_Data_Scientist
数据科学基本功

# Files
<pre>
Workspace:.
│  .gitignore
│  files.txt
│  LICENSE
│  README.md
│  
├─000-技术底层
│  ├─1-分布式数据库
│  ├─2-大数据
│  └─3-云计算
├─000-理论基础
│  ├─1-贝叶斯学习与推断
│  ├─2-运筹学
│  └─3-博弈论
├─100-基本功扎实
│  │  _经验 性能分析与优化.md
│  │  
│  ├─0-C C++
│  ├─0-数据结构与算法
│  │      LeetCode  Two Sum.md
│  │      
│  ├─1-Shell
│  │      cmd-上下级目录.md
│  │      cmd-常用命令.md
│  │      _基本命令.md
│  │      
│  ├─2-Git
│  │      基本操作.md
│  │      
│  ├─Matlab
│  │      Matlab-常用  基本语法.md
│  │      Matlab工具箱  DSP.md
│  │      Simulink  buffer函数.md
│  │      工具  Matlab.md
│  │      
│  ├─SQL
│  └─_Tools
│          Markdown.md
│          平台 Win.md
│          思维导图.md
│          环境 Anaconda.md
│          环境 cmd(pip).md
│          环境 jupyter notebook.md
│          
├─200-数据分析
│  │  _模板 数据处理.md
│  │  
│  ├─0-Python基础
│  │  │  Python  绪论.md
│  │  │  Python-基础  内存.md
│  │  │  Python-基础  内置函数 关键字.md
│  │  │  Python-基础  变量.md
│  │  │  Python-基础  基本语法.md
│  │  │  Python-基础  编码.md
│  │  │  Python-进阶  异常处理.md
│  │  │  Python-进阶  玩转数据类型转换.md
│  │  │  Python-进阶  生成器与反射.md
│  │  │  Python-进阶 常用函数.md
│  │  │  基本功  编程规范.md
│  │  │  
│  │  ├─1-Python基本数据类型
│  │  │      Python dict.md
│  │  │      Python int.md
│  │  │      Python list.md
│  │  │      Python set.md
│  │  │      Python str.md
│  │  │      Python tuple.md
│  │  │      Python-基础  基本数据类型.md
│  │  │      
│  │  ├─2-Python字符串
│  │  │      Python 格式化输出.md
│  │  │      Python-基础  字符串.md
│  │  │      细节 r b u f含义.md
│  │  │      
│  │  ├─3-积累_实用技巧
│  │  │      (ing...)Python-积累  实用技巧.md
│  │  │      技巧  maxmin函数.md
│  │  │      技巧  range与xrange.md
│  │  │      
│  │  ├─4-积累_易混淆点
│  │  │      (ing...)Python-积累  易混淆点.md
│  │  │      
│  │  └─5-积累_细节
│  │          (ing...)Python-积累  细节.md
│  │          
│  ├─0-Python模块
│  │      Python  文件IO.md
│  │      Python  正则表达式re.md
│  │      Python标准模块(一)  系统.md
│  │      Python标准模块(三)  运维相关.md
│  │      Python标准模块(二)  文件IO.md
│  │      [转载]python开发大全、系列文章、精品教程.md
│  │      
│  ├─1-Numpy
│  │  │  [Numpy]常用的20个函数.md
│  │  │  _NumPy 纲要.md
│  │  │  
│  │  ├─Tutorial
│  │  │      基础 1数据类型.md
│  │  │      基础 2创建数组.md
│  │  │      基础 3numpy IO.md
│  │  │      基础 4indexing.md
│  │  │      基础 5广播机制.md
│  │  │      基础 6字节交换.md
│  │  │      基础 7结构化数组.md
│  │  │      基础 8编写数组容器.md
│  │  │      基础 9子类.md
│  │  │      拓展 1特殊值(含缺失值).md
│  │  │      拓展 2参考Matlab.md
│  │  │      拓展 3线性代数 n维数组.md
│  │  │      探索 1编译.md
│  │  │      探索 2接口.md
│  │  │      
│  │  └─_使用手册
│  │          1 绪论.md
│  │          2 通用操作+统计运算.md
│  │          
│  ├─1-Pandas
│  │  │  [Pandas]常用的20个函数.md
│  │  │  _Pandas 纲要.md
│  │  │  基础 基本函数1.md
│  │  │  
│  │  ├─Tutorial
│  │  │      _10 minutes to pandas.md
│  │  │      _Cookbook.md
│  │  │      _FAQ.md
│  │  │      基础 基本函数.md
│  │  │      基础 基本函数1.md
│  │  │      基础 数据结构.md
│  │  │      应用0 数据IO.md
│  │  │      应用0 统计+窗函数.md
│  │  │      应用1 要点Index.md
│  │  │      应用1 要点MultiIndex.md
│  │  │      应用2 表格整合Reshaping.md
│  │  │      应用3 可空bool数据.md
│  │  │      应用3 可空int数据.md
│  │  │      应用3 文本数据.md
│  │  │      应用4 Categorical数据.md
│  │  │      应用5 Time deltas数据.md
│  │  │      应用5 Time series-date数据.md
│  │  │      应用6 可视化.md
│  │  │      应用6.1 风格美化.md
│  │  │      应用6.2 选项设置.md
│  │  │      特性1 大规模数据集.md
│  │  │      特性2 稀疏数据结构.md
│  │  │      特性3 性能强化.md
│  │  │      补充1 布尔逻辑判断.md
│  │  │      
│  │  └─_使用手册
│  │          1 绪论+通用操作.md
│  │          2 Series.md
│  │          3 DataFrame.md
│  │          _本质+默认情形.md
│  │          专题 分组grouping.md
│  │          专题 合并merge,join,concatenate.md
│  │          专题 处理缺失值.md
│  │          专题 重构melt, pivot.md
│  │          
│  ├─2-可视化
│  │      可视化 探索性数据 总纲.md
│  │      可视化 探索性数据1 单变量.md
│  │      可视化 探索性数据2  双变量.md
│  │      可视化 探索性数据3  多变量.md
│  │      可视化 解释性数据.md
│  │      可视化-拓展  应用.md
│  │      可视化-汇总  图形属性设置.md
│  │      
│  ├─2.1-Matplotlib
│  │  │  _Matplotlib 纲要.md
│  │  │  _Matplotlib 纲要示例.md
│  │  │  
│  │  ├─Tutorial
│  │  │      _中级 1Artist对象.md
│  │  │      _中级 2Legend.md
│  │  │      _中级 3样式风格.md
│  │  │      _中级 4图形布局.md
│  │  │      _中级 5约束布局.md
│  │  │      _中级 6紧凑的布局.md
│  │  │      _中级 7imshow.md
│  │  │      _入门 1基本介绍.md
│  │  │      _入门 2Pyplot(加精).md
│  │  │      _入门 3示例.md
│  │  │      _入门 4图像处理.md
│  │  │      _入门 5实例(完整过程).md
│  │  │      _入门 6定制样式rcParams.md
│  │  │      _高级 Blitting动画.md
│  │  │      _高级 path.md
│  │  │      _高级 path效果.md
│  │  │      _高级 transform.md
│  │  │      属性 文本1 基本内容.md
│  │  │      属性 文本2 text属性和布局.md
│  │  │      属性 文本3 注释.md
│  │  │      属性 颜色.md
│  │  │      工具包 axes_grid1.md
│  │  │      工具包 axisartist.md
│  │  │      工具包 mplot3d(加精).md
│  │  │      拓展 交互图形.md
│  │  │      数学公式.md
│  │  │      
│  │  └─_使用手册
│  │          _汇总 API.md
│  │          
│  ├─2.1-Seaborn
│  │  │  _Seaborn 纲要.md
│  │  │  
│  │  ├─Tutorial
│  │  │      Overview.md
│  │  │      Plot Milti-Grids.md
│  │  │      Plot1 relationship.md
│  │  │      Plot2 distribution.md
│  │  │      Plot3 categorical.md
│  │  │      Plot4 regression.md
│  │  │      数据结构.md
│  │  │      美学-设置属性.md
│  │  │      
│  │  └─_使用手册
│  │          _汇总 5大类21种图.md
│  │          _汇总 API.md
│  │          
│  ├─2.2-其他可视化库
│  │  │  ggplot2.md
│  │  │  _纲要.md
│  │  │  自定义-big_screen.md
│  │  │  
│  │  ├─Bokeh
│  │  │      快速入门-教程.md
│  │  │      快速入门-案例.md
│  │  │      
│  │  ├─d3.js
│  │  │      _官网介绍+图形库.md
│  │  │      
│  │  ├─Plotly.js
│  │  │      _官网介绍+Dash.md
│  │  │      _官网介绍+图形库.md
│  │  │      快速入门-教程.md
│  │  │      
│  │  ├─商业软件 FineBI
│  │  ├─商业软件 Tableau
│  │  └─经典 The Grammar of Graphics
│  ├─_Python使用手册
│  │      混淆点 拷贝.md
│  │      
│  ├─模块SciPy
│  │  │  [SciPy]最常用的函数.md
│  │  │  _SciPy纲要.md
│  │  │  
│  │  └─库-signal
│  │          检测特定值.md
│  │          
│  └─模块statsmodels
│          _纲要.md
│          
├─300-机器学习
│  │  _纲要 AI汇总.md
│  │  
│  └─1-ScikitLearn
│      └─_使用手册
├─310-深度学习与神经网络
├─400-应用开发
│      HTTP抓取文本 & 文本处理.md
│      待定.md
│      数据IO  解析XML 提取字幕.md
│      
├─前端(核心基础)
│  ├─1-HTML(内容)
│  │  │  HTML-属性download.md
│  │  │  [Markdown]图片设置：大小 居中.md
│  │  │  _基础.md
│  │  │  
│  │  └─Tutorial
│  │          标签div.md
│  │          
│  ├─2-CSS(样式属性)
│  │      _基础.md
│  │      
│  ├─3-JavaScript(动态交互)
│  └─4-前端框架 Bootstrap
│          _纲要.md
│          
└─后端(核心基础)
    ├─0-数据库
    │      _数据库 纲要.md
    │      
    ├─1-Linux
    │      Linux  Ubuntu下的SSH.md
    │      Shell  Linux常用命令.md
    │      
    ├─2-Web应用框架
    │  │  Web  网页访问全过程.md
    │  │  _纲要 Web应用.md
    │  │  _纲要 Web应用框架.md
    │  │  
    │  ├─Dash(基于Flask)
    │  ├─Django
    │  │  │  _纲要 Django.md
    │  │  │  
    │  │  └─Tutorial
    │  ├─Flask
    │  │      _纲要.md
    │  │      
    │  └─Tornado
    ├─云服务
    ├─操作系统
    └─服务器
            PC DNS服务器.md
</pre>





# #附录




