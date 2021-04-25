[link: 知乎专栏 2020.04.03](https://zhuanlan.zhihu.com/p/124287962)

- [link: 基于kaggle的kernel学习Catboost](https://www.kaggle.com/abhinand05/catboost-a-deeper-dive)
- 更详细的原理：[link: 知乎专题|Catboost 马东什么](https://www.zhihu.com/search?type=content&q=catboost)



## 特点

测试结果：性能优于LightGBM, XGBoost, H2O

训练速度：

- 比其他gbdt算法稍慢
- 但是预测速度比其他集成算法要快13~16倍



# 代码



# 算法原理

数据集：按时序排序的10个数据点

> 如果数据没有时间戳，CatBoost会人工为每个数据创造一个时间戳

![image-20210420113328501](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210420113328.png)

- **Step1**:
  对于单一数据点，用除了这个数据点之外的数据作为建模模型来预测并计算残差。（比如我们要计算数据点x5的残差，我们可以用基于数据x1,x2,x3,x4的模型来建模计算残差）。按照这个步骤我们对于每个不同的数据点来建立不同的模型。**到最终那一步时，我们等于每个预测值等于都是基于其他数据来预测一个模型从来没有见过的值**

- **Step2**
  用第一步得到的残差来训练模型
- **Step3**:
  重复步骤1和2 n次



对于上面的玩具数据集，我们为了得到9个数据点的残差谁需要训练9个不同的模型。

这个计算量显然非常大。所以默认不是训练n个模型，而是log(n_samples)个模型。

所以一个模型如果根据n个数据点来建立，他将会被用于预测下n个数据点的残差。

- 根据第一个数据点训练出的模型，将用于训练下一个数据点的残差.
- 由此可得，根据第x1x2,x3,x4个数据点训练出的模型，将被用于预测x5,x6,x7,x8的残差。 上面介绍的算法我们一般叫做OrderedBoosting
- **(随机排列)Random Permutation**： **CatBoost**一般都会将训练数据集进行随机排列shuffle，然后对于这些shuffle过的数据来进行刚刚讲到的OrderedBoosting算法。
  - 引入这样的随机性我们可以更好的避免过拟合模型。
  - 我们可以根据`bagging_temperature`来控制随机性。



## 算法的特色

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210420113441.jpeg" alt="img" style="zoom:80%;" />

### 1 Ordered Target Statistic



### 2.1 类别特征|One-Hot Encoding

- 默认来说，CatBoost**会且仅会**对所有取值nunique==2的特征进行onehot编码。
- 如果你需要改变这个CatBoosta的机制，对于其他多取值的分类变量也进行onehot编码，你可以改变`one_hot_max_size`，将其设置为N



## 2.2 数值特征

- 对于这来特征CatBoost的处理方式与其他集成算法相同的





## 缺陷

数据中有太多数值型变量时，CatBoost的性能会没有lgb好





# 代码

## 1 首先创建一个ModelClass

```python
class ModelOptimizer:
    best_score = None
    opt = None
    
    def __init__(self, model, X_train, y_train, categorical_columns_indices=None, n_fold=3, seed=2405, early_stopping_rounds=30, is_stratified=True, is_shuffle=True):
        self.model = model
        self.X_train = X_train
        self.y_train = y_train
        self.categorical_columns_indices = categorical_columns_indices
        self.n_fold = n_fold
        self.seed = seed
        self.early_stopping_rounds = early_stopping_rounds
        self.is_stratified = is_stratified
        self.is_shuffle = is_shuffle
        
        
    def update_model(self, **kwargs):
        for k, v in kwargs.items():
            # 给类别更新属性
            setattr(self.model, k, v)
            
    def evaluate_model(self):
        pass
    
    def optimize(self, param_space, max_evals=10, n_random_starts=2):
        start_time = time.time()
        
        @use_named_args(param_space)
        def _minimize(**params):
            self.model.set_params(**params)
            return self.evaluate_model()
        
        opt = gp_minimize(_minimize, param_space, n_calls=max_evals, n_random_starts=n_random_starts, random_state=2405, n_jobs=-1)
        best_values = opt.x
        optimal_values = dict(zip([param.name for param in param_space], best_values))
        best_score = opt.fun
        self.best_score = best_score
        self.opt = opt
        
        print('optimal_parameters: {}\noptimal score: {}\noptimization time: {}'.format(optimal_values, best_score, time.time() - start_time))
        print('updating model with optimal values')
        self.update_model(**optimal_values)
        plot_convergence(opt)
        return optimal_values
class CatboostOptimizer(ModelOptimizer):
    def evaluate_model(self):
        validation_scores = catboost.cv(
        catboost.Pool(self.X_train, 
                      self.y_train, 
                      cat_features=self.categorical_columns_indices),
        self.model.get_params(), 
        nfold=self.n_fold,
        stratified=self.is_stratified,
        seed=self.seed,
        early_stopping_rounds=self.early_stopping_rounds,
        shuffle=self.is_shuffle,
#         metrics='auc',
        plot=False)
        self.scores = validation_scores
        test_scores = validation_scores.iloc[:, 2]
        best_metric = test_scores.max()
        return 1 - best_metric
```



## 2 Tunning CatBoost

1. `cat_features`: 传入这个参数中的分类特征才能被CatBoost用他那迷人的方式处理，==这个参数为空的话CatBoost和其他数据就没区别了==，所以是**最重要的特征！**
2. `one_hot_max_size`：catboost将会对所有unique值<=`one_hot_max_size`的特征进行独热处理。这个参数的调整因人而异
3. `learning_rate & n_estimators`:这个和其他gbdt算法一样，学习率越小需要的学习器就越多。
4. `max_depth` 的深度，这个没啥好多说
5. `subsample` 行采样参数，在`Bayesian Boosting Type`不可用
6. `colsample_bylevel`,`colsample_bytree`,`colsample_bynode` 列采样，不多说
7. `l2_leaf_reg`: l2正则化系数
8. `random_strength`: 每一次分裂都会有一个分数，这个参数会给分数+一个随机性，来进一步抵抗过拟合的问题。



### 1）**首先来一个默认参数**

```python
default_cb = catboost.CatBoostClassifier(loss_function='MultiClass',
                                         task_type='CPU',
                                         random_seed=12,
                                         silent=True
                                        )
default_cb_optimizer = CatboostOptimizer(default_cb, X, y)
default_cb_optimizer.evaluate_model()

>>> 0.996118622206826
```





### 2）贪心模式下参数调优

```python
greedy_cb = catboost.CatBoostClassifier(
    loss_function='MultiClass',
    task_type="CPU",
    learning_rate=0.01,
    iterations=2000,
    od_type="Iter",
    early_stopping_rounds=500,
    random_seed=24,
    silent=True
)

cb_optimizer = CatboostOptimizer(greedy_cb, X, y)
params_space = [Real(0.01, 0.8, name='learning_rate'),]
cb_optimal_values = cb_optimizer.optimize(params_space)

>>> optimal_parameters: {'learning_rate': 0.8}
optimal score: 0.9953029953040498
optimization time: 325.5685770511627
updating model with optimal values
```

![image-20210420113638947](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210420113639.png)



### 3）**一步调优**

```python
cb = catboost.CatBoostClassifier(n_estimators=4000,
                         one_hot_max_size=2,
                         loss_function='MultiClass',
                         eval_metric='WKappa',
                         task_type='CPU',                
                         random_seed=5, 
                         use_best_model=True,
                         silent=True
                        )

one_cb_optimizer = CatboostOptimizer(cb, X, y)
params_space = [Real(0.01, 0.8, name='learning_rate'), 
                Integer(2, 10, name='max_depth'), 
                Real(0.5, 1.0, name='colsample_bylevel'), 
                Real(0.0, 100, name='bagging_temperature'), 
                Real(0.0, 100, name='random_strength'), 
                Real(1.0, 100, name='reg_lambda')]
one_cb_optimal_values = one_cb_optimizer.optimize(params_space, max_evals=40, n_random_starts=4)

>>>
optimal_parameters: {'learning_rate': 0.8, 'max_depth': 2, 'colsample_bylevel': 0.8965613003072048, 'bagging_temperature': 5.151316517828518, 'random_strength': 0.0, 'reg_lambda': 4.165887305583868}
optimal score: 0.7898955281518537
optimization time: 359.9885940551758
updating model with optimal values
```

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210420113713.png" alt="image-20210420113713197" style="zoom:80%;" />



```python
one_cb_optimizer.model.get_params()

{'loss_function': 'MultiClass',
 'random_seed': 5,
 'use_best_model': True,
 'silent': True,
 'one_hot_max_size': 2,
 'eval_metric': 'WKappa',
 'task_type': 'CPU',
 'n_estimators': 4000,
 'learning_rate': 0.06704561234480398,
 'max_depth': 2,
 'colsample_bylevel': 0.5029186949806786,
 'bagging_temperature': 31.63773822656249,
 'random_strength': 6.591681669201289,
 'reg_lambda': 82.97997497668354}

def make_classifier():
    clf = catboost.CatBoostClassifier(
            n_estimators = 4000,
            task_type = 'CPU',
            one_hot_max_size = 2,
            random_seed = 31,
            loss_function = 'MultiClass',
            learning_rate = 0.8,
            max_depth = 6,
            colsample_bylevel = 0.5,
            bagging_temperature = 28.635664398579774,
            random_strength = 100.0,
            reg_lambda = 100.0,
            early_stopping_rounds=500,
    )
    return clf
oof = np.zeros(len(X))

from sklearn.model_selection import KFold
oof = np.zeros(len(X))
NFOLDS = 5
folds = KFold(n_splits=NFOLDS, shuffle=True, random_state=2019)

training_start_time = time.time()
for fold, (trn_idx, test_idx) in enumerate(folds.split(X, y)):
    start_time = time.time()
    print(f'Training on fold {fold+1}')
    clf = make_classifier()
    clf.fit(X.loc[trn_idx, all_features], y.loc[trn_idx], eval_set=(X.loc[test_idx, all_features], y.loc[test_idx]),
                          use_best_model=True, verbose=500, cat_features=cat_features)    
    oof[test_idx] = clf.predict(X.loc[test_idx, all_features]).reshape(len(test_idx))
    print('Fold {} finished in {}'.format(fold + 1, str(datetime.timedelta(seconds=time.time() - start_time))))
    
print('-' * 30)
print('OOF QWK:', qwk(y, oof))
print('-' * 30)



Training on fold 1
0:	learn: 1.2244565	test: 1.2367025	best: 1.2367025 (0)	total: 33.8ms	remaining: 2m 15s
500:	learn: 0.9629898	test: 1.2507881	best: 1.2049752 (22)	total: 18.9s	remaining: 2m 11s
Stopped by overfitting detector  (500 iterations wait)

bestTest = 1.204975188
bestIteration = 22

Shrink model to first 23 iterations.
Fold 1 finished in 0:00:19.873362
Training on fold 2
0:	learn: 1.2384268	test: 1.2523545	best: 1.2523545 (0)	total: 36.2ms	remaining: 2m 24s
500:	learn: 0.9697280	test: 1.2120219	best: 1.1847392 (71)	total: 16.3s	remaining: 1m 54s
Stopped by overfitting detector  (500 iterations wait)

bestTest = 1.184739159
bestIteration = 71

Shrink model to first 72 iterations.
Fold 2 finished in 0:00:19.346654
Training on fold 3
0:	learn: 1.2384703	test: 1.2341216	best: 1.2341216 (0)	total: 27ms	remaining: 1m 48s
500:	learn: 0.9734634	test: 1.1961753	best: 1.1690815 (57)	total: 15.4s	remaining: 1m 47s
Stopped by overfitting detector  (500 iterations wait)

bestTest = 1.169081537
bestIteration = 57

Shrink model to first 58 iterations.
Fold 3 finished in 0:00:17.009331
Training on fold 4
0:	learn: 1.2298037	test: 1.2082135	best: 1.2082135 (0)	total: 21.1ms	remaining: 1m 24s
500:	learn: 0.8219658	test: 1.0217308	best: 1.0029660 (164)	total: 12.8s	remaining: 1m 29s
Stopped by overfitting detector  (500 iterations wait)

bestTest = 1.002965956
bestIteration = 164

Shrink model to first 165 iterations.
Fold 4 finished in 0:00:17.538963
Training on fold 5
0:	learn: 1.2419635	test: 1.2458893	best: 1.2458893 (0)	total: 22.7ms	remaining: 1m 30s
500:	learn: 0.8121620	test: 1.0679569	best: 1.0542414 (251)	total: 13.9s	remaining: 1m 37s
Stopped by overfitting detector  (500 iterations wait)

bestTest = 1.054241402
bestIteration = 251

Shrink model to first 252 iterations.
Fold 5 finished in 0:00:21.132966
```





# [总结|kaggle竞赛宝典](https://mp.weixin.qq.com/s?__biz=Mzk0NDE5Nzg1Ng==&mid=2247494038&idx=2&sn=936c63d7f3eb8ad9dccc04e65395655d&chksm=c32af019f45d790fade45a8ce74c8d6ca8f35a60ab8e82c594c9ab2ab923eefa99d096697e2f&scene=21#wechat_redirect)

> CatBoost由俄罗斯公司Yandex设计，并于2017年在Github上开源。
>
> 在2017年刚刚开源的时候，CatBoost的效果并不理想，而且因为CatBoost在CPU上训练很慢，并不是很受大家的欢迎。
>
> 但随着CatBoost开始支持GPU训练，模型训练速度得到了大大提升，后来大家发现其经常在**类别特征较多**的竞赛中可以取得非常不错的效果而越加受欢迎。

CatBoost与XGBoost、LightGBM相比，又有哪些不一样的地方呢？

此处我们将其归纳为下面几点：

- CatBoost对类别特征的处理进行了进一步的细化与升级，我们无需对类别特征进行明显的预处理，Catboost可以**自动将类别特征处理为数值型特征**；
- CatBoost可以**自动化寻找类别特征的组合**，利用特征之间的关系，极大地丰富特征的维度；
- CatBoost采用ordered boosting的方法对抗训练集中的噪声点，避免梯度估计的偏差，更好地解决了预测偏移的问题；尤其是**在小数据集上**，Catboost的效果更加稳定。



早期的GBDT模型，在诸多传统表格型数据的建模问题上表现都非常不错，但早期版本的GBDT模型在模型训练预测方面的速度一般；

陈天奇对原始的GBDT模型进行了升级，将原先的**一阶导数扩展为二阶导数近似的形式**，加入了**新的正则**并设计了近似算法来加速树的生成，此外，还在系统的设计上面进行了很多改进，包括设计了**并行计算**的列模块；Cache-aware Access，内存外的计算等等来适用于目前的大数据的趋势。

再之后，LightGBM对XGBoost进行了进一步升级，

- 将XGBoost中树模型的生成方式从Level-wise变为Leaf-wise，
- 此外通过使用histogram的技巧加速了模型训练，在很多问题中也取得了更好的效果，
- 不仅如此，LightGBM中提出的GOSS和EFB算法在不影响精度的情况下，这两个算法分别减少了GBDT训练中所需的数据量和特征量，
- 另外LightGBM相较于XGBoost在特征和数据层面还进行了进一步的加速优化，所以**在数据量大的情况下**，LightGBM的效果和速度相较于XGboost往往更优。

CatBoost相较于XGBoost以及LightGBM，**在类别特征上进行了较大的优化**，

- 对类别特征引入了Target Encoding、
- 还设计了贪心的特征组合策略，
- 此外，通过Ordered Boosting的策略，CatBoost缓解了预测漂移的问题，在诸多小数据集的表现上也展现了非常好的效果。

目前三个模型互相借鉴，也都在之前的基础上进行了不同程度的改进，但三个模型训练融合依旧是在**传统数据**建模竞赛中的一大利器。

至于三个模型的更新细节大家可以关注对应的Github网站。