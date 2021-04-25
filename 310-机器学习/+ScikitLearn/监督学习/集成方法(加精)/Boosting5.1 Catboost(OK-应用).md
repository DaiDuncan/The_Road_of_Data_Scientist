

# 应用

## GPU加速

就GPU内存使用而言，CatBoost至少与LightGBM一样有效，CatBoost的GPU实现可支持多个GPU，分布式树学习可以通过样本或特征进行并行化。



## sklearn参数

`sklearn`本身的文档当中并没有CatBoost的描述，[CatBoost python-reference_parameters-list](https://link.zhihu.com/?target=https%3A//catboost.ai/docs/concepts/python-reference_parameters-list.html)上面看到主要参数如下：

- `iterations`: 迭代次数， 解决机器学习问题能够构建的最大树的数目，default=1000

- `learning_rate`: 学习率，default=0.03

- `depth`: 树的深度，default=6

- `l2_leaf_reg`: $L_2$正则化数，default=3.0

- `model_size_reg`:模型大小正则化系数，数值越到，模型越小，==仅在有类别型变量的时候起作用==，取值范围从0到$+\infty$，GPU计算时不可用， default=None

- `rsm`: =None,

- `loss_function`: 损失函数，字符串 (分类任务，default=`Logloss`，回归任务，default=`RMSE`)

- `border_count`: 数值型变量的分箱个数

  - CPU：1～65535的整数，default=254
  - GPU：1～255的整数，default=128

- `feature_border_type`: 数值型变量分箱个数的初始量化模式，default=GreedyLogSum

  - Median
  - Uniform
  - UniformAndQuantiles
  - MaxLogSum
  - MinEntropy
  - GreedyLogSum

- `per_float_feature_quantization`: 指定特定特征的分箱个数，default=None,

- `input_borders`=None,

- `output_borders`=None,

- `fold_permutation_block`: 对数据集进行随机排列之前分组的block大小，default=1

- `od_pval`: 过拟合检测阈值，数值越大，越早检测到过拟合，default=0

- `od_wait`: **达成优化目标以后继续迭代的次数**，default=20

- `od_type`: 过拟合检测类型，default=IncToDec

  - IncToDec
  - Iter

- `nan_mode`: ==缺失值的预处理方法==，字符串类型，default=Min
  - `Forbidden`: 不支持缺失值
  - `Min`: 缺失值赋值为最小值
  - `Max`: 缺失值赋值为最大值

- `counter_calc_method`: 计算Counter CTR类型的方法，default=None

- `leaf_estimation_iterations`: 计算叶子节点值时候的迭代次数，default=None,

- `leaf_estimation_method`: 计算叶子节点值的方法，default=Gradient

  - Newton
  - Gradient

- `thread_count`: 训练期间的进程数，default=-1，进程数与部件的核心数相同

- `random_seed`: 随机数种子，default=0

- `use_best_model`: 如果有设置`eval_set`设置了验证集的话可以设为True，否则为False

- `verbose`: 是否显示详细信息，default=1

- `logging_level`: 打印的日志级别，default=None

- `metric_period`: 计算优化评估值的频率，default=1

- `ctr_leaf_count_limit`: 类别型特征最大叶子数，default=None

- `store_all_simple_ctr`: 是否忽略类别型特征，default=False

- `max_ctr_complexity`: 最大特征组合数，default=4

- `has_time`: 是否采用输入数据的顺序，default=False

- `allow_const_label`: 使用它为所有对象用具有相同标签值的数据集训练模型，default=None

- `classes_count`: 多分类当中类别数目上限，defalut=None

- `class_weights`: 类别权重，default=None

- `one_hot_max_size`: one-hot编码最大规模，默认值根据数据和训练环境的不同而不同

- `random_strength`: 树结构确定以后为分裂点进行打分的时候的随机强度，default=1

- `name`: 在可视化工具当中需要显示的实验名字

- `ignored_features`: 在训练当中需要排除的特征名称或者索引，default=None

- `train_dir`: 训练过程当中文件保存的目录

- `custom_loss`: 用户自定义的损失函数

- `custom_metric`: 自定义训练过程当中输出的评估指标，default=None

- `eval_metric`: 过拟合检测或者最优模型选择的**评估指标**
  - [loss-functions](https://link.zhihu.com/?target=https%3A//catboost.ai/docs/concepts/loss-functions.html%23loss-functions)

- `bagging_temperature`: 贝叶斯bootstrap强度设置，default=1

- `save_snapshot`: 训练中断情况下保存快照文件

- `snapshot_file`: 训练过程信息保存的文件名字

- `snapshot_interval`: 快照保存间隔时间，单位秒

- `fold_len_multiplier`: 改变fold长度的系数，default=2

- `used_ram_limit`: 类别型特征使用内存限制，default=None

- `gpu_ram_part`: GPU内存使用率，default=0.95

- `allow_writing_files`: 训练过程当中允许写入分析和快照文件，default=True

- `final_ctr_computation_mode`: Final CTR计算模式

- `approx_on_full_history`: 计算近似值的原则，default=False

- `boosting_type`: ==提升模式==
  - Ordered
  - Plain

- `simple_ctr`: 单一类别型特征的量化设置

  - CtrType
  - TargetBorderCount
  - TargetBorderType
  - CtrBorderCount
  - CtrBorderType
  - Prior

- `combinations_ctr`: 组合类别型特征的量化设置

  - CtrType
  - TargetBorderCount
  - TargetBorderType
  - CtrBorderCount
  - CtrBorderType
  - Prior

- `per_feature_ctr`: 以上几个参数的设置具体可以细看下面的文档

  - [Categorical features](https://link.zhihu.com/?target=https%3A//catboost.ai/docs/concepts/algorithm-main-stages_cat-to-numberic.html%23algorithm-main-stages_cat-to-numberic)

- `task_type`: 任务类型，CPU或者GPU，default=CPU

- `device_config`: =None

- `devices`: ==用来训练的GPU设备号==，default=NULL

- `bootstrap_type`: 自采样类型，default=Bayesian

  - Bayesian
  - Bernoulli
  - MVS
  - Poisson
  - No

- `subsample`: bagging的采样率，default=0.66

- `sampling_unit`: 采样模式，default=Object

  - Object
  - Group

- `dev_score_calc_obj_block_size`: =None,

- `max_depth`: 树的最大深度

- `n_estimators`: 迭代次数？？？

- `num_boost_round`: 迭代轮数

- `num_trees`: 树的数目

- `colsample_bylevel`: 按层抽样比例，default=None

- `random_state`: 随机数状态

- `reg_lambda`: 损失函数$L_2$范数，default=3.0

- `objective`: =同损失函数

- `eta`: 学习率

- `max_bin`: =同`border_coucnt`

- `scale_pos_weight`: 二分类任务当中1类的权重，default=1.0

- `gpu_cat_features_storage`: GPU训练时类别型特征的存储方式，default=GpuRam

- - CpuPinnedMemory
  - GpuRam

- `data_partition`: 分布式训练时数据划分方法

- - 特征并行
  - 样本并行

- `metadata`: =None
- `early_stopping_rounds`: 早停轮次，default=False
- `cat_features`: =指定类别型特征的名称或者索引
- `grow_policy`: 树的生长策略
- `min_data_in_leaf`: 叶子节点最小样本数，default=1
- `min_child_samples`: 叶子节点最小样本数，default=1
- `max_leaves`: 最大叶子数，default=31
- `num_leaves`: 叶子数
- `score_function`: 建树过程当中的打分函数
- `leaf_estimation_backtracking`: 梯度下降时回溯类型
- `ctr_history_unit`: =None
- `monotone_constraints`: =None

如果有遗漏，具体可以参阅[CatBoost python-reference_parameters-list](https://link.zhihu.com/?target=https%3A//catboost.ai/docs/concepts/python-reference_parameters-list.html)



区分具体的机器学习任务有：

## CatBoostClassifier

[CatBoostClassifier](https://link.zhihu.com/?target=https%3A//catboost.ai/docs/concepts/python-reference_catboostclassifier.html)

```python
class CatBoostClassifier(iterations=None,
                         learning_rate=None,
                         depth=None,
                         l2_leaf_reg=None,
                         model_size_reg=None,
                         rsm=None,
                         loss_function=None,
                         border_count=None,
                         feature_border_type=None,
                         per_float_feature_quantization=None,                         
                         input_borders=None,
                         output_borders=None,
                         fold_permutation_block=None,
                         od_pval=None,
                         od_wait=None,
                         od_type=None,
                         nan_mode=None,
                         counter_calc_method=None,
                         leaf_estimation_iterations=None,
                         leaf_estimation_method=None,
                         thread_count=None,
                         random_seed=None,
                         use_best_model=None,
                         verbose=None,
                         logging_level=None,
                         metric_period=None,
                         ctr_leaf_count_limit=None,
                         store_all_simple_ctr=None,
                         max_ctr_complexity=None,
                         has_time=None,
                         allow_const_label=None,
                         classes_count=None,
                         class_weights=None,
                         one_hot_max_size=None,
                         random_strength=None,
                         name=None,
                         ignored_features=None,
                         train_dir=None,
                         custom_loss=None,
                         custom_metric=None,
                         eval_metric=None,
                         bagging_temperature=None,
                         save_snapshot=None,
                         snapshot_file=None,
                         snapshot_interval=None,
                         fold_len_multiplier=None,
                         used_ram_limit=None,
                         gpu_ram_part=None,
                         allow_writing_files=None,
                         final_ctr_computation_mode=None,
                         approx_on_full_history=None,
                         boosting_type=None,
                         simple_ctr=None,
                         combinations_ctr=None,
                         per_feature_ctr=None,
                         task_type=None,
                         device_config=None,
                         devices=None,
                         bootstrap_type=None,
                         subsample=None,
                         sampling_unit=None,
                         dev_score_calc_obj_block_size=None,
                         max_depth=None,
                         n_estimators=None,
                         num_boost_round=None,
                         num_trees=None,
                         colsample_bylevel=None,
                         random_state=None,
                         reg_lambda=None,
                         objective=None,
                         eta=None,
                         max_bin=None,
                         scale_pos_weight=None,
                         gpu_cat_features_storage=None,
                         data_partition=None
                         metadata=None, 
                         early_stopping_rounds=None,
                         cat_features=None, 
                         grow_policy=None,
                         min_data_in_leaf=None,
                         min_child_samples=None,
                         max_leaves=None,
                         num_leaves=None,
                         score_function=None,
                         leaf_estimation_backtracking=None,
                         ctr_history_unit=None,
                         monotone_constraints=None)
```

## CatBoostRegressor

[CatBoostRegressor](https://link.zhihu.com/?target=https%3A//catboost.ai/docs/concepts/python-reference_catboostregressor.html)

```python
class CatBoostRegressor(iterations=None,
                        learning_rate=None,
                        depth=None,
                        l2_leaf_reg=None,
                        model_size_reg=None,
                        rsm=None,
                        loss_function='RMSE',
                        border_count=None,
                        feature_border_type=None,
                        per_float_feature_quantization=None,
                        input_borders=None,
                        output_borders=None,
                        fold_permutation_block=None,
                        od_pval=None,
                        od_wait=None,
                        od_type=None,
                        nan_mode=None,
                        counter_calc_method=None,
                        leaf_estimation_iterations=None,
                        leaf_estimation_method=None,
                        thread_count=None,
                        random_seed=None,
                        use_best_model=None,
                        best_model_min_trees=None,
                        verbose=None,
                        silent=None,
                        logging_level=None,
                        metric_period=None,
                        ctr_leaf_count_limit=None,
                        store_all_simple_ctr=None,
                        max_ctr_complexity=None,
                        has_time=None,
                        allow_const_label=None,
                        one_hot_max_size=None,
                        random_strength=None,
                        name=None,
                        ignored_features=None,
                        train_dir=None,
                        custom_metric=None,
                        eval_metric=None,
                        bagging_temperature=None,
                        save_snapshot=None,
                        snapshot_file=None,
                        snapshot_interval=None,
                        fold_len_multiplier=None,
                        used_ram_limit=None,
                        gpu_ram_part=None,
                        pinned_memory_size=None,
                        allow_writing_files=None,
                        final_ctr_computation_mode=None,
                        approx_on_full_history=None,
                        boosting_type=None,
                        simple_ctr=None,
                        combinations_ctr=None,
                        per_feature_ctr=None,
                        ctr_target_border_count=None,
                        task_type=None,
                        device_config=None,                        
                        devices=None,
                        bootstrap_type=None,
                        subsample=None,                        
                        sampling_unit=None,
                        dev_score_calc_obj_block_size=None,
                        max_depth=None,
                        n_estimators=None,
                        num_boost_round=None,
                        num_trees=None,
                        colsample_bylevel=None,
                        random_state=None,
                        reg_lambda=None,
                        objective=None,
                        eta=None,
                        max_bin=None,
                        gpu_cat_features_storage=None,
                        data_partition=None,
                        metadata=None,
                        early_stopping_rounds=None,
                        cat_features=None,
                        grow_policy=None,
                        min_data_in_leaf=None,
                        min_child_samples=None,
                        max_leaves=None,
                        num_leaves=None,
                        score_function=None,
                        leaf_estimation_backtracking=None,
                        ctr_history_unit=None,
                        monotone_constraints=None)
```



## 应用场景

作为GBDT框架内的算法，GBDT、XGBoost、LightGBM能够应用的场景CatBoost也都适用，并且在==**处理类别型特征具备独有的优势**==，比如广告推进领域。



## 效果展示

不同算法的对比：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210419111609.png" alt="image-20210419111608744"  />



**对比|ordered vs. plain**

- plain适合大数据集 => 方差小更重要
- ordered适合小数据集 => prediction shift更大

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210419111637.png" alt="image-20210419111637748" style="zoom:80%;" />



数据集大小的影响：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210419112002.png" alt="image-20210419112002316" style="zoom:80%;" />



不同排列的次数：

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210419112127.png" alt="image-20210419112127010" style="zoom:80%;" />



