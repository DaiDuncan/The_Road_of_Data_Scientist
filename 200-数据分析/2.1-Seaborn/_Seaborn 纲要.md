# _Seaborn 纲要

定位：seaborn是Python中基于matplotlib的具有更多可视化功能和更优美绘图风格的绘图模块。应强调的是，应该把Seaborn视为matplotlib的补充，而不是替代物。

[Link: 官方手册](https://seaborn.pydata.org/tutorial.html)



## API overview

### 1. Overview of seaborn plotting functions

- [Similar functions for similar tasks](https://seaborn.pydata.org/tutorial/function_overview.html#similar-functions-for-similar-tasks)
- [Figure-level vs. axes-level functions](https://seaborn.pydata.org/tutorial/function_overview.html#figure-level-vs-axes-level-functions)
- [Combining multiple views on the data](https://seaborn.pydata.org/tutorial/function_overview.html#combining-multiple-views-on-the-data)

[![img](https://seaborn.pydata.org/_images/function_overview_8_0.png)](https://seaborn.pydata.org/tutorial/function_overview.html)

1. 数据关系
   - 散点图
   - 线段/折线图
2. 数据分布
   - 直方图
   - 核密度估计图kernel density estimation
   - (经验累积)分布函数empirical cumulative distribution functions
   - 轴须图：用于绘制出一维数组中数据点实际的分布位置情况
3. ==数据分类==
   - 分布的散点图
   - 分簇散点图
   - 箱线图
   - 小提琴图
   - 点图
   - 条形图



### 2. Data structures accepted by seaborn

- [Long-form vs. wide-form data](https://seaborn.pydata.org/tutorial/data_structure.html#long-form-vs-wide-form-data)
- [Options for visualizing long-form data](https://seaborn.pydata.org/tutorial/data_structure.html#options-for-visualizing-long-form-data)
- [Options for visualizing wide-form data](https://seaborn.pydata.org/tutorial/data_structure.html#options-for-visualizing-wide-form-data)

[![](https://seaborn.pydata.org/_images/data_structure_19_0.png)](https://seaborn.pydata.org/tutorial/data_structure.html)



## Plotting functions

[![img](https://seaborn.pydata.org/_images/relational_51_0.png)](https://seaborn.pydata.org/tutorial/relational.html)

### 1. Visualizing statistical relationships

- [Relating variables with scatter plots](https://seaborn.pydata.org/tutorial/relational.html#relating-variables-with-scatter-plots)
- [Emphasizing continuity with line plots](https://seaborn.pydata.org/tutorial/relational.html#emphasizing-continuity-with-line-plots)
- [Showing multiple relationships with facets](https://seaborn.pydata.org/tutorial/relational.html#showing-multiple-relationships-with-facets)

------

[![img](https://seaborn.pydata.org/_images/distributions_66_0.png)](https://seaborn.pydata.org/tutorial/distributions.html)

### 2. Visualizing distributions of data

- [Plotting univariate histograms](https://seaborn.pydata.org/tutorial/distributions.html#plotting-univariate-histograms)
- [Kernel density estimation](https://seaborn.pydata.org/tutorial/distributions.html#kernel-density-estimation)
- [Empirical cumulative distributions](https://seaborn.pydata.org/tutorial/distributions.html#empirical-cumulative-distributions)
- [Visualizing bivariate distributions](https://seaborn.pydata.org/tutorial/distributions.html#visualizing-bivariate-distributions)
- [Distribution visualization in other settings](https://seaborn.pydata.org/tutorial/distributions.html#distribution-visualization-in-other-settings)

------

[![img](https://seaborn.pydata.org/_images/categorical_36_0.png)](https://seaborn.pydata.org/tutorial/categorical.html)

### 3. Plotting with categorical data

- [Categorical scatterplots](https://seaborn.pydata.org/tutorial/categorical.html#categorical-scatterplots)
- [Distributions of observations within categories](https://seaborn.pydata.org/tutorial/categorical.html#distributions-of-observations-within-categories)
- [Statistical estimation within categories](https://seaborn.pydata.org/tutorial/categorical.html#statistical-estimation-within-categories)
- [Plotting “wide-form” data](https://seaborn.pydata.org/tutorial/categorical.html#plotting-wide-form-data)
- [Showing multiple relationships with facets](https://seaborn.pydata.org/tutorial/categorical.html#showing-multiple-relationships-with-facets)

------

[![img](https://seaborn.pydata.org/_images/regression_37_0.png)](https://seaborn.pydata.org/tutorial/regression.html)

### 4. Visualizing regression models

- [Functions to draw linear regression models](https://seaborn.pydata.org/tutorial/regression.html#functions-to-draw-linear-regression-models)
- [Fitting different kinds of models](https://seaborn.pydata.org/tutorial/regression.html#fitting-different-kinds-of-models)
- [Conditioning on other variables](https://seaborn.pydata.org/tutorial/regression.html#conditioning-on-other-variables)
- [Controlling the size and shape of the plot](https://seaborn.pydata.org/tutorial/regression.html#controlling-the-size-and-shape-of-the-plot)
- [Plotting a regression in other contexts](https://seaborn.pydata.org/tutorial/regression.html#plotting-a-regression-in-other-contexts)



## Multi-plot grids

[![img](https://seaborn.pydata.org/_images/axis_grids_46_0.png)](https://seaborn.pydata.org/tutorial/axis_grids.html)

### 1. Building structured multi-plot grids

- [Conditional small multiples](https://seaborn.pydata.org/tutorial/axis_grids.html#conditional-small-multiples)
- [Using custom functions](https://seaborn.pydata.org/tutorial/axis_grids.html#using-custom-functions)
- [Plotting pairwise data relationships](https://seaborn.pydata.org/tutorial/axis_grids.html#plotting-pairwise-data-relationships)





## Plot aesthetics美学

[![img](https://seaborn.pydata.org/_images/aesthetics_24_0.png)](https://seaborn.pydata.org/tutorial/aesthetics.html)

### 1. Controlling figure aesthetics

- [Seaborn figure styles](https://seaborn.pydata.org/tutorial/aesthetics.html#seaborn-figure-styles)
- [Removing axes spines](https://seaborn.pydata.org/tutorial/aesthetics.html#removing-axes-spines)
- [Temporarily setting figure style](https://seaborn.pydata.org/tutorial/aesthetics.html#temporarily-setting-figure-style)
- [Overriding elements of the seaborn styles](https://seaborn.pydata.org/tutorial/aesthetics.html#overriding-elements-of-the-seaborn-styles)
- [Scaling plot elements](https://seaborn.pydata.org/tutorial/aesthetics.html#scaling-plot-elements)

------

[![img](https://seaborn.pydata.org/_images/color_palettes_22_0.png)](https://seaborn.pydata.org/tutorial/color_palettes.html)

### 2. Choosing color palettes

- [General principles for using color in plots](https://seaborn.pydata.org/tutorial/color_palettes.html#general-principles-for-using-color-in-plots)
- [Tools for choosing color palettes](https://seaborn.pydata.org/tutorial/color_palettes.html#tools-for-choosing-color-palettes)
- [Qualitative color palettes](https://seaborn.pydata.org/tutorial/color_palettes.html#qualitative-color-palettes)
- [Sequential color palettes](https://seaborn.pydata.org/tutorial/color_palettes.html#sequential-color-palettes)
- [Diverging color palettes](https://seaborn.pydata.org/tutorial/color_palettes.html#diverging-color-palettes)