# 参数设置

[link: 知乎专栏 2020.04.27](https://zhuanlan.zhihu.com/p/136697031)

- 作者不喜欢调参：比赛的超参数也基本是开源的kernel里直接借用的

长期下来发现了很多有意思的事情，那就是存在一些所谓“祖传”的超参，意思就是，直接使用这些超参数组合，然后人工针对拟合情况对个别超参数进行微调就可以达到很不错的模型结果，避免了前期把太多时间浪费在参数的调节，到最后实在没啥可做的时候再花时间调参。



https://www.kaggle.com/kyakovlev/ieee-catboost-baseline-with-groupkfold-cv

```python
########################### Model params
cat_params = {
                'n_estimators':5000,
                'learning_rate': 0.07,
                'eval_metric':'AUC',
                'loss_function':'Logloss',
                'random_seed':SEED,
                'metric_period':500,
                'od_wait':500,
                'task_type':'GPU',
                'depth': 8,
                #'colsample_bylevel':0.7,
                } 
```



https://www.kaggle.com/youwbpyr/ieee-cbt-9600-lb-solution?scriptVersionId=21484729

```python
cbt_model = cbt.CatBoostClassifier(iterations=10000,learning_rate=0.1,max_depth=7,verbose=100,
                                      early_stopping_rounds=500,task_type='GPU',eval_metric='AUC',
                                      cat_features=cat_list)
```



https://www.kaggle.com/seesee/concise-catboost-starter-ensemble-plb-0-06435

```python
    model = CatBoostRegressor(
        iterations=200, learning_rate=0.03,
        depth=6, l2_leaf_reg=3,
        loss_function='MAE',
        eval_metric='MAE',
        random_seed=i)
```



https://www.kaggle.com/wakamezake/starter-code-catboost-baseline

```python
model = CatBoostClassifier(loss_function="Logloss",
                           eval_metric="AUC",
                           task_type="GPU",
                           learning_rate=0.01,
                           iterations=10000,
                           random_seed=42,
                           od_type="Iter",
                           depth=10,
                           early_stopping_rounds=500
                          )
```



https://www.kaggle.com/nicapotato/simple-catboost

```python
cb_model = CatBoostRegressor(iterations=700,
                             learning_rate=0.02,
                             depth=12,
                             eval_metric='RMSE',
                             random_seed = 23,
                             bagging_temperature = 0.2,
                             od_type='Iter',
                             metric_period = 75,
                             od_wait=100)
```



https://www.kaggle.com/braquino/catboost-some-more-features

```python
def make_classifier(iterations=6000):
    clf = CatBoostClassifier(
                               loss_function='MultiClass',
                                eval_metric="WKappa",
                               task_type="CPU",
                               #learning_rate=0.01,
                               iterations=iterations,
                               od_type="Iter",
                                #depth=4,
                               early_stopping_rounds=500,
                                #l2_leaf_reg=10,
                                #border_count=96,
                               random_seed=42,
                                #use_best_model=use_best_model
                              )
        
    return clf
```



https://www.kaggle.com/zxspectrum/catboost-gpu

```python
model = CatBoostClassifier(loss_function="Logloss",
                           eval_metric="AUC",
                           task_type="GPU",
                           learning_rate=0.01,
                           iterations=70000,
                           l2_leaf_reg=50,
                           random_seed=432013,
                           od_type="Iter",
                           depth=5,
                           early_stopping_rounds=15000,
                           border_count=64
                           #has_time= True 
                          )
```



对于gbdt的调参，一点建议，

- tree的数量通过earlystopping的功能来决定即可，对于整个gbdt模型的影响最大的参数，
  - 一个是tree的数量，
  - 一个是max_depth深度，
  - 一个是行列采样的比例，可以说是立竿见影的影响交叉验证的结果，



其实用多了gbdt会发现很多超参数的设置对于最终模型效果的影响比较类似，有的参数稍微调整一点就变化很大，有的参数怎么调整变化都不太大，



目前我的方法基本是tree

- 用早停决定，
- max_depth设置为-1，
- 灵活调整行列采样比例。
- 关于max depth的问题，对于lightgbm来说，我在很多数据集熵发现max_depth如果设置为-1，虽然会导致训练集和测试集的训练指标差异较大也就是过拟合，但是这样得到的交叉验证结果往往是最好的，
  - 而对max depth的深度进行限制之后确实可以有效缓解过拟合的问题，但是交叉验证的结果往往变差了。



也就是所谓的“我们一定要解决过拟合问题吗”，==**使用一些超参数的方式约束模型的过拟合程度实际上也可能限制了模型对数据的学习能力**==

因此，还是得根据实际的情况来是灵活使用，用脑编程。



# 特殊情境

- 样本

- 特征
- 模型状态：过拟合、欠拟合