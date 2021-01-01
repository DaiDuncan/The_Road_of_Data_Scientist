# Matplotlib 纲要

定位：Matplotlib能高度兼容numpy与pandas数据结构以及scipy与statsmodels等统计模块

```python
import matplotlib.pyplot as plt
%matplotlib inline
```



[Link: 完整版PDF](https://matplotlib.org/Matplotlib.pdf)

| Release: | 3.3.3             |
| :------- | ----------------- |
| Date:    | November 12, 2020 |

- ==User's Guide  [Link: 官方手册](https://matplotlib.org/users/index.html)==
  - [Installation Guide](https://matplotlib.org/users/installing.html)
  - ==[Tutorials](https://matplotlib.org/tutorials/index.html)==
    - Introductory
      - [Sample plots in Matplotlib](https://matplotlib.org/tutorials/introductory/sample_plots.html#)
        - [Line Plot](https://matplotlib.org/tutorials/introductory/sample_plots.html#line-plot)
        - [Multiple subplots in one figure](https://matplotlib.org/tutorials/introductory/sample_plots.html#multiple-subplots-in-one-figure)
        - [Images](https://matplotlib.org/tutorials/introductory/sample_plots.html#images)
        - [Contouring and pseudocolor](https://matplotlib.org/tutorials/introductory/sample_plots.html#contouring-and-pseudocolor)
        - [Histograms](https://matplotlib.org/tutorials/introductory/sample_plots.html#histograms)
        - [Paths](https://matplotlib.org/tutorials/introductory/sample_plots.html#paths)
        - [Three-dimensional plotting](https://matplotlib.org/tutorials/introductory/sample_plots.html#three-dimensional-plotting)
        - [Streamplot](https://matplotlib.org/tutorials/introductory/sample_plots.html#streamplot)
        - [Ellipses](https://matplotlib.org/tutorials/introductory/sample_plots.html#ellipses)
        - [Bar charts](https://matplotlib.org/tutorials/introductory/sample_plots.html#bar-charts)
        - [Pie charts](https://matplotlib.org/tutorials/introductory/sample_plots.html#pie-charts)
        - [Tables](https://matplotlib.org/tutorials/introductory/sample_plots.html#tables)
        - [Scatter plots](https://matplotlib.org/tutorials/introductory/sample_plots.html#scatter-plots)
        - [GUI widgets](https://matplotlib.org/tutorials/introductory/sample_plots.html#gui-widgets)
        - [Filled curves](https://matplotlib.org/tutorials/introductory/sample_plots.html#filled-curves)
        - [Date handling](https://matplotlib.org/tutorials/introductory/sample_plots.html#date-handling)
        - [Log plots](https://matplotlib.org/tutorials/introductory/sample_plots.html#log-plots)
        - [Polar plots](https://matplotlib.org/tutorials/introductory/sample_plots.html#polar-plots)
        - [Legends](https://matplotlib.org/tutorials/introductory/sample_plots.html#legends)
        - [TeX-notation for text objects](https://matplotlib.org/tutorials/introductory/sample_plots.html#tex-notation-for-text-objects)
        - [Native TeX rendering](https://matplotlib.org/tutorials/introductory/sample_plots.html#native-tex-rendering)
        - [EEG GUI](https://matplotlib.org/tutorials/introductory/sample_plots.html#eeg-gui)
        - [XKCD-style sketch plots](https://matplotlib.org/tutorials/introductory/sample_plots.html#xkcd-style-sketch-plots)
        - [Subplot example](https://matplotlib.org/tutorials/introductory/sample_plots.html#subplot-example)
    - Intermediate
    - Advanced
    - Colors
    - [Provisional](https://matplotlib.org/tutorials/index.html#provisional)
    - Text
    - [Toolkits](https://matplotlib.org/tutorials/index.html#toolkits)
  - ==[Interactive Figures](https://matplotlib.org/users/interactive.html)==
  - [What's new?](https://matplotlib.org/users/whats_new.html)
  - [What's new in Matplotlib 3.3.0](https://matplotlib.org/users/whats_new.html#what-s-new-in-matplotlib-3-3-0)
  - [History](https://matplotlib.org/users/history.html)
  - [GitHub Stats](https://matplotlib.org/users/github_stats.html)
  - [Previous What's New](https://matplotlib.org/users/whats_new_old.html)
  - [License](https://matplotlib.org/users/license.html)
  - [Citing Matplotlib](https://matplotlib.org/citing.html)
  - [Credits](https://matplotlib.org/users/credits.html)
- The Matplotlib FAQ
  - [Installation](https://matplotlib.org/faq/installing_faq.html)
  - [How-to](https://matplotlib.org/faq/howto_faq.html)
  - [Troubleshooting](https://matplotlib.org/faq/troubleshooting_faq.html)
  - [Environment Variables](https://matplotlib.org/faq/environment_variables_faq.html)
- API Overview
  - [Usage patterns](https://matplotlib.org/api/index.html#usage-patterns)
  - [Modules](https://matplotlib.org/api/index.html#modules)
  - [Toolkits](https://matplotlib.org/api/index.html#toolkits)
- ==External Resources==
  - [Books, Chapters and Articles](https://matplotlib.org/resources/index.html#books-chapters-and-articles)
  - [Videos](https://matplotlib.org/resources/index.html#videos)
  - [Tutorials](https://matplotlib.org/resources/index.html#tutorials)
- Third party packages
  - [Mapping toolkits](https://matplotlib.org/thirdpartypackages/index.html#mapping-toolkits)
  - [Declarative libraries](https://matplotlib.org/thirdpartypackages/index.html#declarative-libraries)
  - [Specialty plots](https://matplotlib.org/thirdpartypackages/index.html#specialty-plots)
  - [Animations](https://matplotlib.org/thirdpartypackages/index.html#animations)
  - [Interactivity](https://matplotlib.org/thirdpartypackages/index.html#interactivity)
  - [Rendering backends](https://matplotlib.org/thirdpartypackages/index.html#rendering-backends)
  - [Miscellaneous](https://matplotlib.org/thirdpartypackages/index.html#miscellaneous)
  - [GUI applications](https://matplotlib.org/thirdpartypackages/index.html#gui-applications)
- The Matplotlib Developers' Guide
  - [Contributing](https://matplotlib.org/devel/contributing.html)
  - [Developer's tips for testing](https://matplotlib.org/devel/testing.html)
  - [Writing documentation](https://matplotlib.org/devel/documenting_mpl.html)
  - [Developer's guide for creating scales and transformations](https://matplotlib.org/devel/add_new_projection.html)
  - [Working with *Matplotlib* source code](https://matplotlib.org/devel/gitwash/index.html)
  - [Pull request guidelines](https://matplotlib.org/devel/coding_guide.html)
  - [Release Guide](https://matplotlib.org/devel/release_guide.html)
  - [Minimum Version of Dependencies Policy](https://matplotlib.org/devel/min_dep_policy.html)
  - [Matplotlib Enhancement Proposals](https://matplotlib.org/devel/MEP/index.html)
- [Glossary](https://matplotlib.org/glossary/index.html)

- [Index](https://matplotlib.org/genindex.html)
- [Module Index](https://matplotlib.org/py-modindex.html)
- [Search Page](https://matplotlib.org/search.html)