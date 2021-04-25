## 背景

==深度学习调参==是一门玄学，有时候想改进自己的模型，比如把LSTM替换为Transformer，模型框架照着论文搭好了，超参数也和论文一致，照理说acc、auc等各种评价指标应该远远超过LSTM，理想是丰满的，现实却是惨不忍睹的

在自己的数据集上跑时，有时效果反而还不如原本已经fine tune过的简单模型性能好，我要这模型有何用？

下面我来教你如何解放双手，让它来帮你找到最优参数。

本文介绍超参数优化框架Optuna，并用示例代码简要展示其在pytorch上的使用。



# Optuna

Optuna是一种自动超参优化框架，专为机器学习而设计。

它具有命令式，按运行定义的用户API。

使用Optuna编写的代码具有很高的模块性，Optuna的用户可以**动态构建超参数的搜索空间**。

它目前支持的部分库有(全部请参见官网https://optuna.org)：

- XGBoost
- LightGBM
- ==Sklearn==
- Keras
- TensorFlow
- tf.keras
- MXNet
- ==PyTorch==
- FastAI



```shell
pip install optuna
```



# 实例

数据的加载、训练等函数都将略去

只需要添加两个函数define_model和objective就能将Optuna框架插入到我们现有的代码中去。

```python
import optuna
from torch import optim, nn

# 这个定义的model只是个示例，实际并没什么用
# 注意是伪代码！
class model(nn.Module):

    def __init__(self, input_size, output_size, hidden_size=200, dropout=p):
        super(model, self).__init__()
        self.embedding = nn.Embedding(num_embeddings=input_size,
                                      embedding_dim=hidden_size)
        self.linear = nn.Linear(in_features=hidden_size, out_features=output_size)
        self.dropout = nn.Dropout(dropout)
    def forward(self, x):
     	x = self.dropout(self.embedding(x))
        outputs = self.linear(x)
        return outputs

def define_model(trial):
    # 在100到200之间搜索hidden_size
    hidden_size = trial.suggest_int('hidden_size', 100, 200)
    # 在0.2到0.5之间搜索dropout rate
    p = trial.suggest_uniform('dropout', 0.2, 0.5)
    # 假设vocab_size, output_size都已经定义了
    m = model(input_size=vocab_size, output_size=output_size, 
              hidden_size=hiddensize, dropout=p)
    return m
  
def objective(trial):
    # 尝试不同的optimizer
    optimizer_name = trial.suggest_categorical('optimizer', 
                                               ['Adam', 'RMSprop', 'SGD'])
    # 搜索学习率
    lr = trial.suggest_uniform('lr', 1e-5, 1e-1)
    m = define_model(trial)
    optimizer = getattr(optim, optimizer_name)(m.parameters(), lr=lr)
    # 这里省略了run函数，内部应该将数据喂给model训练，训练完成后在验证集上测试，计算并返回acc
    acc = run(m, optimizer=optimizer)
    return acc

if __name__ == '__main__':
    # 创建一个学习实例，因为objective返回的评价指标是acc，因此目标是最大化，如果是loss就该是minimize
    study = optuna.create_study(direction='maximize')
    # n_trials代表搜索100种，n_jobs是并行搜索的个数，-1代表使用所有的cpu核心
    study.optimize(objective, n_trials=100, n_jobs=-1)
    print('Number of finished trials: ', len(study.trials))
    print('Best trial:')
    trial = study.best_trial
    print('  Value: ', trial.value)
    print('  Params: ')
    for key, value in trial.params.items():
        print('    {}: {}'.format(key, value))
```









# #参考文献

[link: AI有道|天宏NLP  2020.02.14](https://mp.weixin.qq.com/s?__biz=MzIwOTc2MTUyMg==&mid=2247496203&idx=4&sn=9ec4708e8e793360c971e4a8c9a41ba1&chksm=976c5796a01bde8027c6c96be6ccb4290ccc2cbc114f922f32c593dec435a86dd3d5bf36b1e8&mpshare=1&scene=24&srcid=&sharer_sharetime=1581630872346&sharer_shareid=04169dcd804a29a99e43aea044294546&key=611a107e17292a919cf6e1bb4db964de83854046599062edfa218feefe9ca38064a1f00ed0717dcfa24c652b01728acbc5f224e39f098be18a7844c0475880366c668847c1ccc00f51f7902129b92af1e309210b656b62dcb7ddecd7e3117fb6f4301abce2c7d77100b096549640d91a0b0c57baa3ea4a00ce8600b6c2bbc017&ascene=14&uin=MTUwNzc5NjY4MA%3D%3D&devicetype=Windows+10+x64&version=62090529&lang=zh_CN&exportkey=AQpicxdoKEHtZpEIB1D7dfQ%3D&pass_ticket=DoO9qDhsFmuwOzqK10F8fFG3NziR2LngikJk6zKwzuc5i%2FrGey4y%2FobLkAfj94gb&wx_header=0)

- 更详细的使用请参见官方网站