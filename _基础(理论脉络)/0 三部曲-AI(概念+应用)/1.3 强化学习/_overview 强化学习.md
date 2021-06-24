[强化学习](https://easyai.tech/ai-definition/reinforcement-learning/)

> 强化学习是机器学习的一种学习方式，它跟监督学习、无监督学习是对应的。
>
> 本文将详细介绍强化学习的基本概念、应用场景和主流的强化学习算法及分类。

 

## 什么是强化学习？

强化学习并不是某一种特定的算法，而是一类算法的统称。

如果用来做对比的话，他跟监督学习，无监督学习是类似的，是一种统称的学习方式。

![强化学习是机器学习的学习方法之一](https://easy-ai.oss-cn-shanghai.aliyuncs.com/2019-04-17-rl-position.png)

强化学习算法的思路非常简单，以游戏为例，如果在游戏中采取某种策略可以取得较高的得分，那么就**进一步「强化」这种策略**，以期继续取得较好的结果。

这种==策略与日常生活中的各种「绩效奖励」非常类似。==

我们平时也常常用这样的策略来提高自己的游戏水平。



在 Flappy bird 这个游戏中，我们需要简单的点击操作来控制小鸟，躲过各种水管，飞的越远越好，因为飞的越远就能获得更高的积分奖励。

这就是一个典型的强化学习场景：

- 机器有一个明确的小鸟角色——代理
- 需要控制小鸟飞的更远——目标
- 整个游戏过程中需要躲避各种水管——环境
- 躲避水管的方法是让小鸟用力飞一下——行动
- 飞的越远，就会获得越多的积分——奖励

![游戏是典型的强化学习场景](https://easy-ai.oss-cn-shanghai.aliyuncs.com/2019-04-17-changjing.png)

你会发现，强化学习和监督学习、无监督学习 最大的不同就是==不需要大量的“数据喂养”==。

而是==通过自己不停的尝试==来学会某些技能。

 

## 强化学习的应用场景

强化学习目前还不够成熟，应用场景也比较局限。

最大的应用场景就是游戏了。



**游戏**

![强化学习在游戏领域应用最多](https://easy-ai.oss-cn-shanghai.aliyuncs.com/2019-04-17-game.png)

2016年：AlphaGo Master 击败李世石，使用强化学习的 [AlphaGo Zero](https://en.wikipedia.org/wiki/AlphaGo_Zero) 仅花了40天时间，就击败了自己的前辈 AlphaGo Master。

[《被科学家誉为「世界壮举」的AlphaGo Zero, 对普通人意味着什么？》](http://baijiahao.baidu.com/s?id=1581695084593629103&wfr=spider&for=pc)



2019年1月25日：[AlphaStar](https://en.wikipedia.org/wiki/AlphaStar) 在《星际争霸2》中以 10：1 击败了人类顶级职业玩家。

[《星际争霸2人类1:10输给AI！DeepMind “AlphaStar”进化神速》](https://baijiahao.baidu.com/s?id=1623615701462446493&wfr=spider&for=pc)



2019年4月13日：OpenAI 在《Dota2》的比赛中战胜了人类世界冠军。

[《2:0！Dota2世界冠军OG，被OpenAI按在地上摩擦》](https://www.huxiu.com/article/294169.html?f=wangzhan)

 

**机器人**

![强化学习在机器人领域也有不少应用](https://easy-ai.oss-cn-shanghai.aliyuncs.com/2019-04-17-robot%20-1-.png)

==机器人很像强化学习里的「代理」==，在机器人领域，强化学习也可以发挥巨大的作用。

[《机器人通过强化学习，可以实现像人一样的平衡控制》](https://baijiahao.baidu.com/s?id=1612651771159021082&wfr=spider&for=pc)

[《深度学习与强化学习相结合，谷歌训练机械臂的长期推理能力》](https://www.leiphone.com/news/201807/yt7WTAhrQ0zXnnA5.html)

[《伯克利强化学习新研究：机器人只用几分钟随机数据就能学会轨迹跟踪》](https://www.jiqizhixin.com/articles/2017-12-06-4)

 

**其他**

强化学习在**推荐系统**，对话系统，教育培训，广告，金融等领域也有一些应用：

《[强化学习与推荐系统的强强联合](https://blog.csdn.net/jiangjiang_jian/article/details/80674016)》

《[基于深度强化学习的对话管理中的策略自适应](https://www.jiqizhixin.com/articles/042405)》

《[强化学习在业界的实际应用](http://www.oreilly.com.cn/ideas/?p=1482)》

 

## 强化学习的主流算法

**免模型学习（Model-Free） vs 有模型学习（Model-Based）**

在介绍详细算法之前，我们先来了解一下强化学习算法的2大分类。

这2个分类的重要差异是：**智能体是否能完整了解或学习到所在环境的模型**

- 有模型学习（Model-Based）对环境有提前的认知，**可以提前考虑规划**，但是缺点是**如果模型跟真实世界不一致，那么在实际使用场景下会表现的不好**。

- 免模型学习（Model-Free）放弃了模型学习，在效率上不如前者，但是这种方式更加容易实现，也容易在真实场景下调整到很好的状态。
  - 所以**免模型学习方法更受欢迎，得到更加广泛的开发和测试。**

![主流的强化学习算法分类](https://easy-ai.oss-cn-shanghai.aliyuncs.com/2019-04-17-fenlei.png)



**免模型学习 – 策略优化（Policy Optimization）**

这个系列的方法将策略显示表示为： ![\pi_{\theta}(a|s)](https://spinningup.readthedocs.io/zh_CN/latest/_images/math/942a745636db4c6dc70f144df39fd02c4f2c98a6.svg) 

它们直接对性能目标 ![J(\pi_{\theta})](https://spinningup.readthedocs.io/zh_CN/latest/_images/math/0cf62abcaf58a3622b9cf898ec1689f7b4a462d8.svg) 进行梯度下降进行优化，或者间接地，对性能目标的**局部近似函数**进行优化。

优化基本都是基于 **同策略** 的，也就是说每一步更新只会用最新的策略执行时采集到的数据。

策略优化通常还包括学习出 ![V_{\phi}(s)](https://spinningup.readthedocs.io/zh_CN/latest/_images/math/04e3dd3212469f4c679250446641609d6115e420.svg) ，作为 ![V^{\pi}(s)](https://spinningup.readthedocs.io/zh_CN/latest/_images/math/e8cac649e08a2b01c72c546971fe8a2bd817075a.svg) 的近似，该函数用于确定如何更新策略。



基于策略优化的方法举例：

- [A2C / A3C](https://arxiv.org/abs/1602.01783), 通过梯度下降直接最大化性能
- [PPO](https://arxiv.org/abs/1707.06347) , 不直接通过最大化性能更新，而是最大化 **目标估计** 函数，这个函数是目标函数 ![J(\pi_{\theta})](https://spinningup.readthedocs.io/zh_CN/latest/_images/math/0cf62abcaf58a3622b9cf898ec1689f7b4a462d8.svg) 的近似估计。

 



**免模型学习 – [Q-Learning](https://easyai.tech/ai-definition/q-learning/)**

这个系列的算法学习最优行动值函数 ![Q^*(s,a)](https://spinningup.readthedocs.io/zh_CN/latest/_images/math/29face1cf6ea248f89bd640b6aafee497ba10a94.svg) 的近似函数： ![Q_{\theta}(s,a)](https://spinningup.readthedocs.io/zh_CN/latest/_images/math/71cd2d3b97d2b8c33ddad64c3347af683254dfa5.svg) 。

它们通常使用基于 [贝尔曼方程](https://spinningup.readthedocs.io/zh_CN/latest/spinningup/rl_intro.html#bellman-equations) 的目标函数。

优化过程属于 **异策略** 系列，这意味着每次更新可以使用任意时间点的训练数据，不管获取数据时智能体选择如何探索环境。

对应的策略是通过 ![Q^*](https://spinningup.readthedocs.io/zh_CN/latest/_images/math/d62537306703802cdc91716cba6a3728dc03c06e.svg)and ![\pi^*](https://spinningup.readthedocs.io/zh_CN/latest/_images/math/be64fb689087a153fbabb0b62fb14bbe7f4f22fb.svg) 之间的联系得到的。

智能体的行动由下面的式子给出：

![a(s) = \arg \max_a Q_{\theta}(s,a).](https://spinningup.readthedocs.io/zh_CN/latest/_images/math/47c920dfc6d7e3e0a3ae69960a388880a0ee361b.svg)

基于 Q-Learning 的方法

- [DQN](https://www.cs.toronto.edu/~vmnih/docs/dqn.pdf), 一个让深度强化学习得到发展的经典方法
- 以及 [C51](https://arxiv.org/abs/1707.06887), 学习关于回报的分布函数，其期望是 ![Q^*](https://spinningup.readthedocs.io/zh_CN/latest/_images/math/d62537306703802cdc91716cba6a3728dc03c06e.svg)

 

**有模型学习 – 纯规划**

这种最基础的方法，从来不显示的表示策略，而是纯使用规划技术来选择行动，

例如 [模型预测控制](https://en.wikipedia.org/wiki/Model_predictive_control) (model-predictive control, MPC)。

在模型预测控制中，智能体每次观察环境的时候，都会计算得到一个对于当前模型最优的规划，这里的规划指的是未来一个固定时间段内，智能体会采取的所有行动（通过学习值函数，规划算法可能会考虑到超出范围的未来奖励）。

智能体先执行规划的第一个行动，然后立即舍弃规划的剩余部分。

每次准备和环境进行互动时，它会计算出一个新的规划，从而避免执行小于规划范围的规划给出的行动。

- [MBMF](https://sites.google.com/view/mbmf) 在一些深度强化学习的标准基准任务上，基于学习到的环境模型进行模型预测控制

 

**有模型学习 – Expert Iteration**

纯规划的后来之作，使用、学习策略的显示表示形式： ![\pi_{\theta}(a|s)](https://spinningup.readthedocs.io/zh_CN/latest/_images/math/942a745636db4c6dc70f144df39fd02c4f2c98a6.svg) 

智能体在模型中应用了一种规划算法，类似蒙特卡洛树搜索(Monte Carlo Tree Search)，通过对当前策略进行采样生成规划的候选行为。

这种算法得到的行动比策略本身生成的要好，所以相对于策略来说，它是“专家”。

随后更新策略，以产生更类似于规划算法输出的行动。

- [ExIt](https://arxiv.org/abs/1705.08439) 算法用这种算法训练深层神经网络来玩 Hex
- [AlphaZero](https://arxiv.org/abs/1712.01815) 这种方法的另一个例子

 

除了免模型学习和有模型学习的分类外，强化学习还有其他几种分类方式：

- 基于概率 VS 基于价值
- 回合更新 VS 单步更新
- 在线学习 VS 离线学习

详细请查看《[强化学习方法汇总](https://morvanzhou.github.io/tutorials/machine-learning/reinforcement-learning/1-1-B-RL-methods/) 》

 

## 百度百科和维基百科

百度百科版本

强化学习(reinforcement learning)，又称再励学习、评价学习，是一种重要的机器学习方法，在智能控制机器人及分析预测等领域有许多应用。

但在传统的机器学习分类中没有提到过强化学习，而在**连接主义学习**中，把学习算法分为三种类型，即非监督学习(unsupervised learning)、监督学习(supervised leaning)和强化学习。

[查看详情](https://baike.baidu.com/item/强化学习)



维基百科版本

强化学习（RL）是机器学习的一个领域，涉及软件代理如何在环境中采取行动以最大化一些累积奖励的概念。

该问题**由于其一般性**，在许多其他学科中得到研究，如博弈论，控制理论，运筹学，信息论，基于仿真的优化，多智能体系统，群智能，统计和遗传算法。

在运筹学和控制文献中，强化学习被称为**近似动态规划**或**神经动态规划**。

[查看详情](https://en.wikipedia.org/wiki/Reinforcement_learning)

 

# #参考文献

入门类文章（5）

[【强化学习】从强化学习基础概念开始](https://mp.weixin.qq.com/s/84ZN_ctsWsjZSinZmJflRg)（2019-6）

[强化学习如何入门？看这篇文章就够了](https://easyai.tech/blog/introduction-to-reinforcement-learning/)

[强化学习通俗导论（一）：什么是强化学习](https://blog.csdn.net/qq_39521554/article/details/80715615)

「教程」[深度学习、强化学习进阶课程](https://www.youtube.com/playlist?list=PLqYmG7hTraZDNJre23vqCGIVpfZ_K2RZs)（YouTube视频，需要科学上网）

[强化学习扫盲](https://mp.weixin.qq.com/s/JcjAPGPxnlGOoqPQL0VK6g)



实践类文章（4）

[使用Python进行强化学习的编程方法](https://easyai.tech/blog/reinforcement-learning-with-python/)(2019-4-3)

[干货 | 强化学习中，如何从稀疏和不明确的反馈中学习泛化](https://mp.weixin.qq.com/s/luQCy9c81zgodA5m6-oOVA)(2019-3-1)

[一文了解强化学习的商业应用](https://www.jiqizhixin.com/articles/2018-11-09-7?from=synced&keyword=强化学习)

[深度解读AlphaGo Zero，教你训练一个“围棋高手”](https://baijiahao.baidu.com/s?id=1589717543327108180&wfr=spider&for=pc)



开拓视野类文章（4）

[强化学习在现实世界中的应用](https://easyai.tech/blog/applications-of-reinforcement-learning-in-real-world/)（2019-4-3）

[谷歌新方法加速深度强化学习的训练过程](https://mp.weixin.qq.com/s/qOtmhc1WUypnHlkIzo492w)（2019-3-27）

[强化学习遭遇瓶颈！分层RL将成为突破的希望](https://mp.weixin.qq.com/s/PBf-YrkhwhPYXuiOGyahxQ)（2019-3-23）

[好奇心驱动的强化学习](https://towardsdatascience.com/curiosity-driven-learning-made-easy-part-i-d3e5a2263359)（2018-10）（需要科学上网）

[让智能体主动交互，DeepMind提出用元强化学习实现因果推理](https://www.jiqizhixin.com/articles/021104)



相关资源（1）

[DeepMind开源强化学习库TRFL](https://www.jiqizhixin.com/articles/2018-10-18-15?from=synced&keyword=强化学习)