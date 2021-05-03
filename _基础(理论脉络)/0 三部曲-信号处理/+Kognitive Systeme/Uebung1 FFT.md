## 1 音频IO

- sample_rate, samples
- 44.1KHz，16bit($2^{16}=65536$)

```python
import scipy
import scipy.io.wavfile
import numpy as np
import matplotlib.pyplot as plt
from IPython.display import Audio
sample_rate, samples = scipy.io.wavfile.read("MEDIA2.wav")
print(sample_rate)
Audio(samples, rate=sample_rate)
```



查看幅值：映射到[-1, 1]

```python
samples = samples.astype(np.double) / 32767
plt.plot(samples)
```

![image-20210429101325053](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210429101325.png)





## 2 DFT

1）初始版本：基于定义

```python
def dft_definition(samples):
    result = np.zeros(len(samples), dtype="complex128") # We initialize the output as complex 0s
    for k in range(len(samples)):
        for m in range(len(samples)):
            # These two nested loops is where the O(n^2) complexity comes from
            result[k] += samples[m] * np.exp(-2*np.pi*1j*m*k/len(samples))
    return result
```



2）Cooley-Turkey Algorithm

- 样本数量是2的幂次

```python
def fft_ct(samples):
    if len(samples) == 1:
        # The DFT of a single element is just that element
        # (You can check this from the definition)
        return samples.astype("complex128")
    evens = fft_ct(samples[::2])  # ::2 means to step through the array with a step size of two, given us the even...
    odds = fft_ct(samples[1::2])  # ... or odd indices
    result = np.zeros(len(samples), dtype="complex128")
    for k in range(len(samples)//2):
        factor = np.exp(-2*np.pi*1j*k/len(samples))
        result[k] = evens[k] + factor * odds[k]  ##递归返回的evens和odds就是result
        result[k+len(samples)//2] = evens[k] - factor * odds[k]  ##递归返回的evens和odds就是result
    return result 
```



加窗：因为整个样本太长

音频信号：假设声音在windows中(10-25ms)是平稳的

- 窗的size是2048：也就是48ms

```python
window_size = 2048
part_samples = samples[10000:10000+window_size]
```



## 3 对比效果

```python
%time dft = dft_definition(part_samples)	#Wall time: 38.6 s

%time fft_naive = fft_ct(part_samples)		#Wall time: 86.8 ms

%time fft = np.fft.fft(part_samples)		#Wall time: 0 ns => 基于C语言

np.allclose(dft, fft_naive), np.allclose(dft, fft), np.allclose(fft, fft_naive)	#(True, True, True)
```





## 4 结果|可视化

- 右半部分平移到左边：关于y轴对称(实数信号关于Y轴对称)
- 即便输入是实数：DFT之后，也会有虚部
  - 通常关注幅频响应
    - 频谱的幅度(绝对值)
    - 功率谱：频谱幅度的平方(下述代码)

```python
fft = abs(fft) ** 2
plt.plot(fft) #下标从0开始，到2047
```

![image-20210429102146057](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210429102146.png)

改进：

1 对称性

- 对于实数输入：正频率与负频率是共轭复数(两部分相等，但是成镜像对称)
  - 由于使用幅度/振幅：丢失虚部信息

```python
np.allclose(fft[1:window_size//2], fft[:window_size//2:-1])  # notice the step size -1 in the second argument
```

对称性：

- 0是中间的幅值

- `1:window_size//2`：表示1-1023
- `:window_size//2:-1`：表示2047-1025
  - 首先-1表示**从尾部开始**，一直到1024(不包括该点)



由于对称：可以丢掉右半部分 => 左半部分幅值*2

- 如果输入是实数，由于对称，不必计算右半部分 => `np.fft.rfft`





2 坐标刻度 => 频率成分@基于奈奎斯特频率

- 截止频率(信号最大频率)是采样频率的一半：也就是22.05kHz
  - The DFT outputs range from -22.05KHz to 22.05 KHz in 2048 steps
  - The DFT outputs range from 0 to 22.05 KHz in 1024 steps.
- ==频率分辨率==为$\frac{44.1kHz}{2048} = \frac{22.05kHz}{1024} $

```python
fft = fft[:window_size//2+1]
step_size = sample_rate / window_size
x_scale = np.arange(window_size//2+1) * step_size
plt.plot(x_scale, fft)
```



2' Y轴尺度变换

- dB: 20log(电压比)，10log(功率比)

```python
plt.plot(x_scale, 10*np.log10(fft / fft.max()))
plt.xscale("log")	#直接改变x轴的尺度
```

![image-20210429105607378](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210429105607.png)

## 5 改进|选择不同类型的窗

1 汉明窗

- 窗口函数的背景噪声更少

```python
def hamming_window(size):
    return 25/46 - 21/46 * np.cos(2*np.pi*np.arange(size)/size)

plt.plot(x_scale, calculate_fft(part_samples * hamming_window(window_size)))
plt.xscale("log")
```

![image-20210429105836377](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210429105836.png)

另一个高级技术。我们从DFT中扔掉了一半的数据，随之而来的是一半的分辨率。如果有一种方法可以把它找回来呢？

为此，我们给我们的数据添加一些零，等于**实际数据点的数量**。

这使我们的数据量增加了一倍，因此我们的频率分辨率也增加了一倍。

```python
windowed_samples = part_samples * hamming_window(window_size)
padded_samples = np.concatenate([windowed_samples, np.zeros(window_size)])	#补零(一个窗口的size)
padded_x_scale = np.linspace(0, sample_rate//2, window_size+1)
plt.plot(padded_x_scale, calculate_fft(padded_samples))
plt.xscale("log")
```

![image-20210429110102010](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210429110102.png)

可以看到频谱图变得 "更平滑"，这是分辨率提高的结果。那么，零填充是否只是一种获得自由频率分辨率的方法？为什么只用2048个零来填充？为什么不是更多？



要回答这个问题，我们需要了解零填充的作用。

- 它对DFT所产生的频谱没有影响。
- 如果我们把频谱看作是一个被DFT采样的连续函数，我们可以通过使用零填充对其进行更多的采样。
- 然而，我们实际上并没有在频谱本身上得到更多的频率分辨率。

请看前面的图（零填充前）。第一个峰值实际上是两个几乎相同振幅的数据点。有可能这实际上是两个非常接近的峰，以至于我们无法用我们拥有的数据点的数量来区分它们。



清楚地解决这两个峰值的唯一方法是使用更多的真实数据点

使用零填充只是让我们在原始DFT产生的相同函数上获得更高的分辨率。如果该函数不能区分两个接近的峰值，那么再多的零填充也无济于事。



那么，零填充是无用的吗？不，再考虑一下前面的图。如果这个第一个峰实际上只有一个峰，但它位于这两个数据点之间的某个频率上，它的能量将根据它们与真正的峰的接近程度被分成这两个数据点（这就是为什么我们经常说的频率 "bin"，因为每个数据点实际上表达的是来自一系列频率的频率内容）。DFT实际上有足够的数据来找到峰值的频率，只是采样的分辨率太低。我们可以用零填充来解决这个问题。在零填充图中，可以清楚地看到峰值在哪里，因为我们现在有一个数据点正好在前面两个数据点的中间。



总而言之，零填充的好处是：[link: 补充理解|补零操作](https://zone.ni.com/reference/en-XX/help/371361R-01/lvanlsconcepts/lvac_zero_padding/)

- 解决一个峰值的确切频率，只要该峰值在没有零填充的情况下也能被检测到。
- 找到一个峰值的确切振幅，因为它的能量没有被分割到几个数据点上。
- 确保在某个频率上有一个你感兴趣的数据点。例如，如果你需要知道准确的100Hz的振幅，你可以使用零填充来控制DFT的输入样本数。如果你知道采样率，你可以确保有一个数据点正好在100赫兹处
- 填充你的输入到2的幂，以获得最有效的FFT算法



@Roam笔记：`补零`

- 时域补零：提高频域分辨率(频域插值)

频率分辨力：由Sa函数(**窗函数**频域)主瓣宽度决定，主瓣宽度为1/N的常数倍，N为截断信号的长度

- 栅栏效应(频率分辨力不够) <= 信息量不足：解决方案是要么增加数据量，要么提供先验信息（超分辨方法都属于这种）
  - 仍无法将两个频率成分分开(只有一个突起的峰)：缺少频率分辨力
- 补零不能减少频谱泄露，也不能缓解栅栏效应 => 只是让频谱看起来更平滑





最后，我们将画一个漂亮的频谱图。要做到这一点，我们对声音文件中的每个数据窗口按上述方法计算出频谱。在我们的例子中，我们每次将窗口右移2048步，所以我们有不重叠的窗口。

但在语音处理中，重叠的窗口也是很常见的。我们把这个（现在是二维）数据绘制成热图，其中较亮的颜色对应于较高的频率内容。X是时间轴，Y是频率轴。

```python
def spectrogram_column(start):
    windowed_samples = samples[start:start+window_size] * hamming_window(window_size)	#加窗：汉明窗
    return calculate_fft(windowed_samples)

# start是窗的起始index
# 按照思路：应该是水平方向上(时间窗)进行堆叠stack
spectrogram = np.stack([spectrogram_column(start) for start in range(0, len(samples) - window_size + 1, window_size)], 1)
plt.imshow(spectrogram, origin="lower", extent=(0, len(samples), 0, sample_rate//2))
```

![image-20210429110445070](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210429110445.png)





[link: 工具推荐|Audacity: 音频剪辑](https://www.audacityteam.org/)





# 拓展|可视化

1 导入库

```python
%matplotlib notebook
from ipywidgets import *
import numpy as np
import matplotlib.pyplot as plt
import scipy.linalg
```



2 可视化对象

- 
- 

```python
def last_index_leq(a, x):
    t = np.where(a<=x)[0]
    if len(t) == 0:
        return 0
    else:
        return t[-1]

def visualize_convolution(x, f, g):
    # 1 4种曲线
    assert len(x) == len(f) == len(g)
    t = scipy.linalg.toeplitz(np.concatenate([g, np.zeros(len(x) - 1)]), np.zeros(len(x)))
    mult_values = t * f[None,:]
    conv_values = mult_values.sum(1) * (x[1] - x[0])
    
    # 2 坐标轴范围
    xmin, xmax = np.min(x), np.max(x)
    newx = np.linspace(2 * xmin, 2 * xmax, 2 * len(x) - 1)
    fig = plt.figure(figsize=(10, 6))
    ax = fig.add_subplot(1, 1, 1)
    ax.set_ylim(np.min(mult_values)-0.5, np.max(mult_values)+1)
    ax.set_xlim(2 * xmin - (xmax - xmin), 2 * xmax + 1)
    ax.plot(x, f, label="$f$")
    shifted, = ax.plot(x, g, label="$g$")
    mult, = ax.plot(x, mult_values[0], label=r"$f(\tau)g(t - \tau)$")
    conv, = ax.plot(newx, conv_values, label=r"$f\ast g$")
    legend = ax.legend()
    
    lines = [f, shifted, mult, conv]
    lined = {}
    for legline, origline in zip(legend.get_lines(), lines):
        legline.set_picker(True)
        lined[legline] = origline
    
    #重点：更新动画
    def update(t):
        i = last_index_leq(x, t)
        j = last_index_leq(newx, t)
        shifted.set_xdata(t - x)
        mult.set_data(x, mult_values[j])
        conv.set_data(newx[:j], conv_values[:j])
        fig.canvas.draw()
    
    def on_pick(event):
        legline = event.artist
        origline = lined[legline]
        visible = not origline.get_visible()
        origline.set_visible(visible)
        legline.set_alpha(1.0 if visible else 0.2)
        fig.canvas.draw()
    fig.canvas.mpl_connect("pick_event", on_pick)
    
    #互动：响应update
    interact(update, t=widgets.FloatSlider(min=2*xmin, max=2*xmax, step=0.1, value=2*xmin))
    
```

![image-20210429115539422](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210429115539.png)



```python
l = 50
x = np.linspace(-0.5, 2, 2*l+l//2+1)
f = np.zeros(len(x))
f[l//2:l+l//2+1] = 2
g = np.zeros(len(x))
g[l//2:l] = 3*np.sin(x[l//2:l]*np.pi) ** 2
g[l:2*l+1] = 3
```



```python
visualize_convolution(x, f, f)
visualize_convolution(x, f, g)
```

