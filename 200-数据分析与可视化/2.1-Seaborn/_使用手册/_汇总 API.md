[Link: 附录|所有的API](https://seaborn.pydata.org/api.html)

## Relational plots[¶](https://seaborn.pydata.org/api.html#relational-plots)

| [`relplot`](https://seaborn.pydata.org/generated/seaborn.relplot.html#seaborn.relplot) | Figure-level interface for drawing relational plots onto a FacetGrid. |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [`scatterplot`](https://seaborn.pydata.org/generated/seaborn.scatterplot.html#seaborn.scatterplot) | Draw a scatter plot with possibility of several semantic groupings. |
| [`lineplot`](https://seaborn.pydata.org/generated/seaborn.lineplot.html#seaborn.lineplot) | Draw a line plot with possibility of several semantic groupings. |



## Distribution plots

| [`displot`](https://seaborn.pydata.org/generated/seaborn.displot.html#seaborn.displot) | Figure-level interface for drawing distribution plots onto a FacetGrid. |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [`histplot`](https://seaborn.pydata.org/generated/seaborn.histplot.html#seaborn.histplot) | Plot univariate or bivariate histograms to show distributions of datasets. |
| [`kdeplot`](https://seaborn.pydata.org/generated/seaborn.kdeplot.html#seaborn.kdeplot) | Plot univariate or bivariate distributions using kernel density estimation. |
| [`ecdfplot`](https://seaborn.pydata.org/generated/seaborn.ecdfplot.html#seaborn.ecdfplot) | Plot empirical cumulative distribution functions.            |
| [`rugplot`](https://seaborn.pydata.org/generated/seaborn.rugplot.html#seaborn.rugplot) | Plot marginal distributions by drawing ticks along the x and y axes. |
| [`distplot`](https://seaborn.pydata.org/generated/seaborn.distplot.html#seaborn.distplot) | DEPRECATED: Flexibly plot a univariate distribution of observations. |



## Categorical plots

| [`catplot`](https://seaborn.pydata.org/generated/seaborn.catplot.html#seaborn.catplot) | Figure-level interface for drawing categorical plots onto a FacetGrid. |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [`stripplot`](https://seaborn.pydata.org/generated/seaborn.stripplot.html#seaborn.stripplot) | Draw a scatterplot where one variable is categorical.        |
| [`swarmplot`](https://seaborn.pydata.org/generated/seaborn.swarmplot.html#seaborn.swarmplot) | Draw a categorical scatterplot with non-overlapping points.  |
| [`boxplot`](https://seaborn.pydata.org/generated/seaborn.boxplot.html#seaborn.boxplot) | Draw a box plot to show distributions with respect to categories. |
| [`violinplot`](https://seaborn.pydata.org/generated/seaborn.violinplot.html#seaborn.violinplot) | Draw a combination of boxplot and kernel density estimate.   |
| [`boxenplot`](https://seaborn.pydata.org/generated/seaborn.boxenplot.html#seaborn.boxenplot) | Draw an enhanced box plot for larger datasets.               |
| [`pointplot`](https://seaborn.pydata.org/generated/seaborn.pointplot.html#seaborn.pointplot) | Show point estimates and confidence intervals using scatter plot glyphs. |
| [`barplot`](https://seaborn.pydata.org/generated/seaborn.barplot.html#seaborn.barplot) | Show point estimates and confidence intervals as rectangular bars. |
| [`countplot`](https://seaborn.pydata.org/generated/seaborn.countplot.html#seaborn.countplot) | Show the counts of observations in each categorical bin using bars. |



## Regression plots

| [`lmplot`](https://seaborn.pydata.org/generated/seaborn.lmplot.html#seaborn.lmplot) | Plot data and regression model fits across a FacetGrid. |
| ------------------------------------------------------------ | ------------------------------------------------------- |
| [`regplot`](https://seaborn.pydata.org/generated/seaborn.regplot.html#seaborn.regplot) | Plot data and a linear regression model fit.            |
| [`residplot`](https://seaborn.pydata.org/generated/seaborn.residplot.html#seaborn.residplot) | Plot the residuals of a linear regression.              |



## Matrix plots

| [`heatmap`](https://seaborn.pydata.org/generated/seaborn.heatmap.html#seaborn.heatmap) | Plot rectangular data as a color-encoded matrix.             |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [`clustermap`](https://seaborn.pydata.org/generated/seaborn.clustermap.html#seaborn.clustermap) | Plot a matrix dataset as a hierarchically-clustered heatmap. |



## Multi-plot grids

### Facet grids

| [`FacetGrid`](https://seaborn.pydata.org/generated/seaborn.FacetGrid.html#seaborn.FacetGrid) | Multi-plot grid for plotting conditional relationships.      |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [`FacetGrid.map`](https://seaborn.pydata.org/generated/seaborn.FacetGrid.map.html#seaborn.FacetGrid.map) | Apply a plotting function to each facet’s subset of the data. |
| [`FacetGrid.map_dataframe`](https://seaborn.pydata.org/generated/seaborn.FacetGrid.map_dataframe.html#seaborn.FacetGrid.map_dataframe) | Like `.map` but passes args as strings and inserts data in kwargs. |

### Pair grids

| [`pairplot`](https://seaborn.pydata.org/generated/seaborn.pairplot.html#seaborn.pairplot) | Plot pairwise relationships in a dataset.                    |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [`PairGrid`](https://seaborn.pydata.org/generated/seaborn.PairGrid.html#seaborn.PairGrid) | Subplot grid for plotting pairwise relationships in a dataset. |
| [`PairGrid.map`](https://seaborn.pydata.org/generated/seaborn.PairGrid.map.html#seaborn.PairGrid.map) | Plot with the same function in every subplot.                |
| [`PairGrid.map_diag`](https://seaborn.pydata.org/generated/seaborn.PairGrid.map_diag.html#seaborn.PairGrid.map_diag) | Plot with a univariate function on each diagonal subplot.    |
| [`PairGrid.map_offdiag`](https://seaborn.pydata.org/generated/seaborn.PairGrid.map_offdiag.html#seaborn.PairGrid.map_offdiag) | Plot with a bivariate function on the off-diagonal subplots. |
| [`PairGrid.map_lower`](https://seaborn.pydata.org/generated/seaborn.PairGrid.map_lower.html#seaborn.PairGrid.map_lower) | Plot with a bivariate function on the lower diagonal subplots. |
| [`PairGrid.map_upper`](https://seaborn.pydata.org/generated/seaborn.PairGrid.map_upper.html#seaborn.PairGrid.map_upper) | Plot with a bivariate function on the upper diagonal subplots. |

### Joint grids

| [`jointplot`](https://seaborn.pydata.org/generated/seaborn.jointplot.html#seaborn.jointplot) | Draw a plot of two variables with bivariate and univariate graphs. |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [`JointGrid`](https://seaborn.pydata.org/generated/seaborn.JointGrid.html#seaborn.JointGrid) | Grid for drawing a bivariate plot with marginal univariate plots. |
| [`JointGrid.plot`](https://seaborn.pydata.org/generated/seaborn.JointGrid.plot.html#seaborn.JointGrid.plot) | Draw the plot by passing functions for joint and marginal axes. |
| [`JointGrid.plot_joint`](https://seaborn.pydata.org/generated/seaborn.JointGrid.plot_joint.html#seaborn.JointGrid.plot_joint) | Draw a bivariate plot on the joint axes of the grid.         |
| [`JointGrid.plot_marginals`](https://seaborn.pydata.org/generated/seaborn.JointGrid.plot_marginals.html#seaborn.JointGrid.plot_marginals) | Draw univariate plots on each marginal axes.                 |



## Themeing

| [`set_theme`](https://seaborn.pydata.org/generated/seaborn.set_theme.html#seaborn.set_theme) | Set multiple theme parameters in one step.                   |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [`axes_style`](https://seaborn.pydata.org/generated/seaborn.axes_style.html#seaborn.axes_style) | Return a parameter dict for the aesthetic style of the plots. |
| [`set_style`](https://seaborn.pydata.org/generated/seaborn.set_style.html#seaborn.set_style) | Set the aesthetic style of the plots.                        |
| [`plotting_context`](https://seaborn.pydata.org/generated/seaborn.plotting_context.html#seaborn.plotting_context) | Return a parameter dict to scale elements of the figure.     |
| [`set_context`](https://seaborn.pydata.org/generated/seaborn.set_context.html#seaborn.set_context) | Set the plotting context parameters.                         |
| [`set_color_codes`](https://seaborn.pydata.org/generated/seaborn.set_color_codes.html#seaborn.set_color_codes) | Change how matplotlib color shorthands are interpreted.      |
| [`reset_defaults`](https://seaborn.pydata.org/generated/seaborn.reset_defaults.html#seaborn.reset_defaults) | Restore all RC params to default settings.                   |
| [`reset_orig`](https://seaborn.pydata.org/generated/seaborn.reset_orig.html#seaborn.reset_orig) | Restore all RC params to original settings (respects custom rc). |
| [`set`](https://seaborn.pydata.org/generated/seaborn.set.html#seaborn.set) | Alias for [`set_theme()`](https://seaborn.pydata.org/generated/seaborn.set_theme.html#seaborn.set_theme), which is the preferred interface. |



## Color palettes

| [`set_palette`](https://seaborn.pydata.org/generated/seaborn.set_palette.html#seaborn.set_palette) | Set the matplotlib color cycle using a seaborn palette.      |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [`color_palette`](https://seaborn.pydata.org/generated/seaborn.color_palette.html#seaborn.color_palette) | Return a list of colors or continuous colormap defining a palette. |
| [`husl_palette`](https://seaborn.pydata.org/generated/seaborn.husl_palette.html#seaborn.husl_palette) | Get a set of evenly spaced colors in HUSL hue space.         |
| [`hls_palette`](https://seaborn.pydata.org/generated/seaborn.hls_palette.html#seaborn.hls_palette) | Get a set of evenly spaced colors in HLS hue space.          |
| [`cubehelix_palette`](https://seaborn.pydata.org/generated/seaborn.cubehelix_palette.html#seaborn.cubehelix_palette) | Make a sequential palette from the cubehelix system.         |
| [`dark_palette`](https://seaborn.pydata.org/generated/seaborn.dark_palette.html#seaborn.dark_palette) | Make a sequential palette that blends from dark to `color`.  |
| [`light_palette`](https://seaborn.pydata.org/generated/seaborn.light_palette.html#seaborn.light_palette) | Make a sequential palette that blends from light to `color`. |
| [`diverging_palette`](https://seaborn.pydata.org/generated/seaborn.diverging_palette.html#seaborn.diverging_palette) | Make a diverging palette between two HUSL colors.            |
| [`blend_palette`](https://seaborn.pydata.org/generated/seaborn.blend_palette.html#seaborn.blend_palette) | Make a palette that blends between a list of colors.         |
| [`xkcd_palette`](https://seaborn.pydata.org/generated/seaborn.xkcd_palette.html#seaborn.xkcd_palette) | Make a palette with color names from the xkcd color survey.  |
| [`crayon_palette`](https://seaborn.pydata.org/generated/seaborn.crayon_palette.html#seaborn.crayon_palette) | Make a palette with color names from Crayola crayons.        |
| [`mpl_palette`](https://seaborn.pydata.org/generated/seaborn.mpl_palette.html#seaborn.mpl_palette) | Return discrete colors from a matplotlib palette.            |

## Palette widgets

| [`choose_colorbrewer_palette`](https://seaborn.pydata.org/generated/seaborn.choose_colorbrewer_palette.html#seaborn.choose_colorbrewer_palette) | Select a palette from the ColorBrewer set.                   |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [`choose_cubehelix_palette`](https://seaborn.pydata.org/generated/seaborn.choose_cubehelix_palette.html#seaborn.choose_cubehelix_palette) | Launch an interactive widget to create a sequential cubehelix palette. |
| [`choose_light_palette`](https://seaborn.pydata.org/generated/seaborn.choose_light_palette.html#seaborn.choose_light_palette) | Launch an interactive widget to create a light sequential palette. |
| [`choose_dark_palette`](https://seaborn.pydata.org/generated/seaborn.choose_dark_palette.html#seaborn.choose_dark_palette) | Launch an interactive widget to create a dark sequential palette. |
| [`choose_diverging_palette`](https://seaborn.pydata.org/generated/seaborn.choose_diverging_palette.html#seaborn.choose_diverging_palette) | Launch an interactive widget to choose a diverging color palette. |

## Utility functions

| [`load_dataset`](https://seaborn.pydata.org/generated/seaborn.load_dataset.html#seaborn.load_dataset) | Load an example dataset from the online repository (requires internet). |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [`get_dataset_names`](https://seaborn.pydata.org/generated/seaborn.get_dataset_names.html#seaborn.get_dataset_names) | Report available example datasets, useful for reporting issues. |
| [`get_data_home`](https://seaborn.pydata.org/generated/seaborn.get_data_home.html#seaborn.get_data_home) | Return a path to the cache directory for example datasets.   |
| [`despine`](https://seaborn.pydata.org/generated/seaborn.despine.html#seaborn.despine) | Remove the top and right spines from plot(s).                |
| [`desaturate`](https://seaborn.pydata.org/generated/seaborn.desaturate.html#seaborn.desaturate) | Decrease the saturation channel of a color by some percent.  |
| [`saturate`](https://seaborn.pydata.org/generated/seaborn.saturate.html#seaborn.saturate) | Return a fully saturated color with the same hue.            |
| [`set_hls_values`](https://seaborn.pydata.org/generated/seaborn.set_hls_values.html#seaborn.set_hls_values) | Independently manipulate the h, l, or s channels of a color. |