point[link: AI蜗牛车|10大算法工程师炼丹Tricks](https://mp.weixin.qq.com/s?__biz=MzA4ODUxNjUzMQ==&mid=2247492126&idx=1&sn=a2e10a115d3c796faf4baf766100a08a&chksm=902a50c2a75dd9d476e066ab4dee86f4639a2f10352da0ee2dba2b7685c75c5d74921aa98049&scene=132#wechat_redirect)

- Focal Loss：降低容易样本的权重值，训练过程更加关注困难样本👌
- Dropout：随机丢弃，抑制过拟合👌
- Normalization：针对单个神经元规范化(该神经元的均值和方差)👌
- relu激活函数(缓解梯度消失)👌
- Cyclic LR： 每隔一段时间重启学习率，在单位时间内能收敛到多个局部最小值，可以得到很多个模型做集成
- With Flooding：改变梯度方向
- Group Normalization：替代深度学习里程碑式的工作Batch normalization👌
- Label Smoothing：将hard label转变成soft label，使网络优化更加平滑 => 减少训练DNN的过拟合问题并进一步提高分类性能
- Wasserstein GAN
- Skip Connection $F(x)=F(x)+x$：一种网络结构，提供恒等映射的能力，保证模型不会因网络变深而退化





[link: AI蜗牛车|ML调参方法](https://mp.weixin.qq.com/s?__biz=MzA4ODUxNjUzMQ==&mid=2247492223&idx=1&sn=2eb58545fc55373b9cedb58794e66065&chksm=902a50a3a75dd9b57d7d347a4780f5c48564411913fe40d1856d713b6e9418040ce714ace974&scene=132#wechat_redirect)

1 网格搜索法(Grid search)

2 随机搜索法(Random search)

- sklearn中通过GridSearchCV方法进行网格搜索。
- sklearn中通过RandomizedSearchCV方法进行随机搜索

3 贝叶斯优化(Bayesian optimization)

一种基于高斯过程(Gaussian process)和贝叶斯定理的参数优化方法，近年来被广泛用于机器学习模型的超参数调优

