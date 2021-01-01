- “lattice” or “trellis” plotting：栅栏格子图@FacetGrid



# [`FacetGrid`](https://seaborn.pydata.org/generated/seaborn.FacetGrid.html#seaborn.FacetGrid)

- row
- col
- hue

应用：

- [`relplot()`](https://seaborn.pydata.org/generated/seaborn.relplot.html#seaborn.relplot)
- [`displot()`](https://seaborn.pydata.org/generated/seaborn.displot.html#seaborn.displot)
- [`catplot()`](https://seaborn.pydata.org/generated/seaborn.catplot.html#seaborn.catplot)

- [`lmplot()`](https://seaborn.pydata.org/generated/seaborn.lmplot.html#seaborn.lmplot)

```python
tips = sns.load_dataset("tips")
g = sns.FacetGrid(tips, col="time")
```