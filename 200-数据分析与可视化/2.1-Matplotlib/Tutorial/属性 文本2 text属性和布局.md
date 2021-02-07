[`matplotlib.text.Text`](https://matplotlib.org/api/text_api.html#matplotlib.text.Text)实例具有各种性质，可以通过关键字参数进行配置[`set_title`](https://matplotlib.org/api/_as_gen/matplotlib.axes.Axes.set_title.html#matplotlib.axes.Axes.set_title)，[`set_xlabel`](https://matplotlib.org/api/_as_gen/matplotlib.axes.Axes.set_xlabel.html#matplotlib.axes.Axes.set_xlabel)， [`text`](https://matplotlib.org/api/_as_gen/matplotlib.axes.Axes.text.html#matplotlib.axes.Axes.text)等

| Property                  | Value Type                                                   |
| ------------------------- | ------------------------------------------------------------ |
| ==alpha==                 | [`float`](https://docs.python.org/3/library/functions.html#float) |
| backgroundcolor           | any matplotlib [color](https://matplotlib.org/tutorials/colors/colors.html) |
| bbox                      | [`Rectangle`](https://matplotlib.org/api/_as_gen/matplotlib.patches.Rectangle.html#matplotlib.patches.Rectangle) prop dict plus key `'pad'` which is a pad in points |
| clip_box                  | a matplotlib.transform.Bbox instance                         |
| clip_on                   | bool                                                         |
| clip_path                 | a [`Path`](https://matplotlib.org/api/path_api.html#matplotlib.path.Path) instance and a [`Transform`](https://matplotlib.org/api/transformations.html#matplotlib.transforms.Transform) instance, a [`Patch`](https://matplotlib.org/api/_as_gen/matplotlib.patches.Patch.html#matplotlib.patches.Patch) |
| ==color==                 | any matplotlib [color](https://matplotlib.org/tutorials/colors/colors.html) |
| ==family==                | [ `'serif'` | `'sans-serif'` | `'cursive'` | `'fantasy'` | `'monospace'` ] |
| ==fontproperties==        | [`FontProperties`](https://matplotlib.org/api/font_manager_api.html#matplotlib.font_manager.FontProperties) |
| horizontalalignment or ha | [ `'center'` | `'right'` | `'left'` ]                        |
| label                     | any string                                                   |
| linespacing               | [`float`](https://docs.python.org/3/library/functions.html#float) |
| multialignment            | [`'left'` | `'right'` | `'center'` ]                         |
| name or fontname          | string e.g., [`'Sans'` | `'Courier'` | `'Helvetica'` ...]    |
| picker                    | [None\|float\|bool\|callable]                                |
| ==position==              | (x, y)                                                       |
| ==rotation==              | [ angle in degrees \| `'vertical'` | `'horizontal'` ]        |
| size or fontsize          | [ size in points \| relative size, e.g., `'smaller'`, `'x-large'` ] |
| style or fontstyle        | [ `'normal'` | `'italic'` | `'oblique'` ]                    |
| ==text==                  | string or anything printable with '%s' conversion            |
| transform                 | [`Transform`](https://matplotlib.org/api/transformations.html#matplotlib.transforms.Transform) subclass |
| variant                   | [ `'normal'` | `'small-caps'` ]                              |
| verticalalignment or va   | [ `'center'` | `'top'` | `'bottom'` | `'baseline'` ]         |
| visible                   | bool                                                         |
| weight or fontweight      | [ `'normal'` | `'bold'` | `'heavy'` | `'light'` | `'ultrabold'` | `'ultralight'`] |
| x                         | [`float`](https://docs.python.org/3/library/functions.html#float) |
| y                         | [`float`](https://docs.python.org/3/library/functions.html#float) |
| zorder                    | any number                                                   |

布局与定位的参数文本：

- horizontalalignment控制文本的x位置参数是否指示文本边界框的左侧，中央还是右侧

- verticalalignment控制文本的y位置参数是否指示文本边界框的底部，中心还是顶部
- multialignment，仅用于换行符分隔的字符串，控制不同的行是左对齐，居中对齐还是右对齐

transform=ax.transAxes整个代码的使用表示坐标是相对于轴边界框给出的，其中（0，0）是轴的左下角，而（1，1）是右上角。

```python
import matplotlib.pyplot as plt
import matplotlib.patches as patches

# build a rectangle in axes coords
left, width = .25, .5
bottom, height = .25, .5
right = left + width
top = bottom + height

fig = plt.figure()
ax = fig.add_axes([0, 0, 1, 1])

# axes coordinates: (0, 0) is bottom left and (1, 1) is upper right
p = patches.Rectangle(
    (left, bottom), width, height,
    fill=False, transform=ax.transAxes, clip_on=False
    )

ax.add_patch(p)

ax.text(left, bottom, 'left top',
        horizontalalignment='left',
        verticalalignment='top',
        transform=ax.transAxes)

ax.text(left, bottom, 'left bottom',
        horizontalalignment='left',
        verticalalignment='bottom',
        transform=ax.transAxes)

ax.text(right, top, 'right bottom',
        horizontalalignment='right',
        verticalalignment='bottom',
        transform=ax.transAxes)

ax.text(right, top, 'right top',
        horizontalalignment='right',
        verticalalignment='top',
        transform=ax.transAxes)

ax.text(right, bottom, 'center top',
        horizontalalignment='center',
        verticalalignment='top',
        transform=ax.transAxes)

ax.text(left, 0.5*(bottom+top), 'right center',
        horizontalalignment='right',
        verticalalignment='center',
        rotation='vertical',
        transform=ax.transAxes)

ax.text(left, 0.5*(bottom+top), 'left center',
        horizontalalignment='left',
        verticalalignment='center',
        rotation='vertical',
        transform=ax.transAxes)

ax.text(0.5*(left+right), 0.5*(bottom+top), 'middle',
        horizontalalignment='center',
        verticalalignment='center',
        fontsize=20, color='red',
        transform=ax.transAxes)

ax.text(right, 0.5*(bottom+top), 'centered',
        horizontalalignment='center',
        verticalalignment='center',
        rotation='vertical',
        transform=ax.transAxes)

ax.text(left, top, 'rotated\nwith newlines',
        horizontalalignment='center',
        verticalalignment='center',
        rotation=45,
        transform=ax.transAxes)

ax.set_axis_off()
plt.show()
```

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210105172950.png" alt="image-20210105172949841" style="zoom: 80%;" />







# 默认字体

由一组rcParams控制：要为数学表达式设置字体，请使用以rcParams开头mathtext

| rcParam          | usage                                                        |
| ---------------- | ------------------------------------------------------------ |
| `'font.family'`  | 字体名称的列表：`{'cursive', 'fantasy', 'monospace', 'sans', 'sans serif', 'sans-serif', 'serif'}`. |
| `'font.style'`   | 默认风格, ex `'normal'`, `'italic'`.                         |
| `'font.variant'` | Default variant, ex `'normal'`, `'small-caps'` (untested)    |
| `'font.stretch'` | Default stretch, ex `'normal'`, `'condensed'` (incomplete)   |
| `'font.weight'`  | Default weight. Either string or integer                     |
| `'font.size'`    | 默认字体大小. Relative font sizes (`'large'`, `'x-small'`) are computed against this size. |



| family alias别名                       | rcParam with mappings |
| -------------------------------------- | --------------------- |
| `'serif'`                              | `'font.serif'`        |
| `'monospace'`                          | `'font.monospace'`    |
| `'fantasy'`                            | `'font.fantasy'`      |
| `'cursive'`                            | `'font.cursive'`      |
| `{'sans', 'sans serif', 'sans-serif'}` | `'font.sans-serif'`   |





## 带有非拉丁字形的文本⭐

从v2.0开始，[默认字体](https://matplotlib.org/users/dflt_style_changes.html#default-changes-font)DejaVu包含许多西方字母的字形，但不包含其他脚本，例如中文，韩文或日文

要将默认字体设置为支持所需代码点的字体，请在字体名称之前添加`'font.family'`或在所需的别名列表之前添加

```python
### 解决中文显示方框问题
matplotlib.rcParams['font.sans-serif'] = ['Source Han Sans TW', 'sans-serif']
plt.rcParams['font.sans-serif'] = ['SimHei']  # 中文字体设置-黑体
plt.rcParams['axes.unicode_minus'] = False  # 解决保存图像是负号'-'显示为方块的问题
sns.set(font='SimHei')  # 解决Seaborn中文显示问题

### 设置在您的.matplotlibrc文件
font.sans-serif: Source Han Sans TW, Arial, sans-serif
```



在linux上，[fc-list](https://linux.die.net/man/1/fc-list)是发现字体名称的有用工具

```shell
$ fc-list :lang=zh family
Noto to Sans Mono CJK TC,Noto Sans Mono CJK TC Bold
Noto Sans CJK TC,Noto Sans CJK TC Medium
Noto Sans CJK TC,Noto Sans CJK TC DemiLight
Noto Sans CJK KR,Noto Sans CJK KR Black
Noto Sans CJK TC,Noto Sans CJK TC Black
Noto Sans Mono CJK TC,Noto Sans Mono CJK TC Regular
Noto Sans CJK SC,Noto Sans CJK SC Light
```



