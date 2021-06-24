# 一、卷积

1 三角变换公式 => 积分

2 关键在于分段函数在某个积分区间内保持不变，并非重叠区域的形状

- 错误理解：Aufgabe1中，尝试求解两个函数曲线的交点

 

小结：针对卷积，永远只有两个要点

1.反褶

2.平移

=> 在重叠的范围内(积分上下边界)，进行积分求和

 

# 二、傅里叶变换

1 卷积的优化计算 => 经过傅里叶变换转换到频域相乘@me: 程序的计算优化

2 采样频率 => 频率分辨率的关系：Δf = fA / N

 

# 三、数字化

1 FT对应表格

2 DTFT：采样(冲激响应) => @me|频率迁移的理解方式(也就是函数和冲激函数卷积的定义)

3 采样定理

 

重点：FT的实轴与虚轴

- 三维：实轴，虚轴，频率轴



概念|komplexen Spektrum

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210428205333.png" alt="image-20210428205333200" style="zoom:67%;" />

重点：

- 时域采样间隔是$T=\frac{1}{f_A}$ => 对应到频域中的采样间隔是$f_A$

- 时域乘以一系列dirac函数 => 频域卷积这一系列变换到频域中的dirac函数

## 知识点

### 1 Dirac冲激信号

$$
\int_{-\infty}^{\infty} \delta(x-x_0) \cdot f(x) dx = f(x_0) \\
\int_{-\infty}^{\infty} \delta(x-x_0) dx = 1 \\
$$

$$
(\delta * \delta)(t) =   \int_{-\infty}^{\infty} \delta(t-\tau) \cdot \delta(\tau) d\tau = \delta(t) \\
3\delta(t-2) * 2 \delta(t-3) = 6 \delta(t-5)
$$



 ## 2 Aliasing

本质：采样频率不够，信号的频率成分混叠在一起

1 针对采样率|提高采样频率

2 针对信号|低通滤波器

- 降低信号的频率，但也丢失了部分信息





 # 四、编程|滤波器

1 导入音频数据

- 音频立体声：左、右两个通道，只选择其中一个 `audio = audio.reshape(-1, 2)[:, 0]`
- `Audio(audio, rate=sample_rate)`

```python
# Verändern Sie diesen Block nicht
import numpy as np
import matplotlib.pyplot as plt
import wave
from IPython.display import Audio
with wave.open("Media1.WAV") as wav_file:
    sample_rate = wav_file.getframerate()
    n_samples = wav_file.getnframes()
    audio = np.frombuffer(wav_file.readframes(n_samples), dtype=np.int16)
audio = audio.reshape(-1, 2)[:, 0]  # Datei ist ursprünglich Stereo, wir brauchen nur einen Kanal
audio = audio.astype(np.double) / 32767
Audio(audio, rate=sample_rate)


#== 输出结果
print(f"sample_rate: {sample_rate}")	#44100
print(f"n_samples: {n_samples}")		#1764169
audio.shape	#(1764158,)
```



2 数据探索|可视化

- 简单的低通滤波器
  - 滤波器的本质：调整幅度 => 目标频率区域增大幅度@类似ML中调整权重的技巧 => 窗函数

```python
# Verändern Sie diesen Block nicht
freq_filter = np.zeros(1024)
x = np.linspace(-sample_rate / 2, sample_rate / 2, 1024)
freq_filter[abs(x) <= 1000] = 1 #中间一半的频率
plt.plot(x, freq_filter)
plt.show()
```

![image-20210428182958568](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210428182959.png)



频域窗函数 => 时域sinc函数

```python
# Verändern Sie diesen Block nicht
t = np.arange(1, 513)
f = np.sin(t * 1000 / (sample_rate / 2)) / (np.pi * t)

plt.plot(t, f)
plt.show()
```

![image-20210428192106402](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210428192106.png)



3 实现|卷积

1）初始版本：简便计算

```python
def convolve(a, b):
    result = np.zeros(len(a)+len(b)-1)
    
    # Implementieren Sie hier die Faltung
    # zero-padding
    a_padding = np.concatenate((a, np.zeros(len(result) - len(a)))) 
    b_padding = np.concatenate((b, np.zeros(len(result) - len(b)))) 
    
    for i in range(len(result)):
        for j in range(0, i+1):
            result[i] += a_padding[j] * b_padding[i-j]

    return result
```



2）初始版本+1

![image-20210428211841787](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210428211841.png)

```python
def convolve_naive(a, b):
    if len(a) < len(b):	#前提|规范情景：保证a是长序列
        a, b = b, a
    result = np.zeros(len(a) + len(b) - 1)
    
    #1 一头向前：包括整个序列b
    #一般到result的index即可(i+1); 最后特殊情况 => 直到序列a的末尾
    for i in range(len(b)):	
        #序列a对应的j从0开头
        for j in range(min(len(a), i+1)):	
            result[i] += a[j]*b[i-j]
    #2 序列b和序列a相差的部分
    for i in range(len(b), len(a)):
        #序列a对应的j从序列b头部所在位置开始
        for j in range(i - len(b) + 1, min(len(a), i+1)):
            result[i] += a[j]*b[i-j]    
    #3 序列a和最终result序列相差的部分
    for i in range(len(a), len(a) + len(b) - 1):
        ##序列a对应的j从序列b头部所在位置开始，到a末端为止
        for j in range(i - len(b) + 1, len(a)):
            result[i] += a[j]*b[i-j]
    return result
```



3）numpy优化/**切片提取向量**⭐

![image-20210428213118269](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210428213118.png)

```python
def convolve_with_numpy(a, b):
    if len(a) < len(b):	#保证a是长序列
        a, b = b, a
    result = np.zeros(len(a) + len(b) - 1)
    # 先反转序列b，利用numpy数组的运算：点乘求和 => 本质等同于一维向量的内积
    b = b[::-1]
    for i in range(len(a) + len(b) -1):
        j = i+1
        # 下列三种情况与第2）种方法一致
        if i < len(b):
            result[i] = np.sum(a[:j] * b[-j:])
        elif i < len(a):
            result[i] = np.sum(a[j-len(b):j] * b)
        else:
            result[i] = np.sum(a[j-len(b):] * b[:len(a)+len(b)-j])
    return result
```



结果：

```python
# Verändern Sie diesen Block nicht
your_result = convolve(audio[:1000], f)
your_result.shape	#(1511,)

# Verändern Sie diesen Block nicht
numpy_result = np.convolve(audio[:1000], f)	#np.convolve(audio[:1000], f).shape (1511,)
np.testing.assert_allclose(numpy_result, your_result)


# Verändern Sie diesen Block nicht
# 完整版音频
your_result = convolve(audio, f)
Audio(your_result, rate=sample_rate)
```





4 实现|基于FFT频域相乘 计算卷积

```python
def fft_convolve(a, b):
    # Implementieren Sie hier die Faltung
    n = len(a) + len(b) - 1
    N = 2 ** (int(np.log2(n)) + 1)
    # FFT
    a_fft = np.fft.fft(a, N)	#会自动补零
    b_fft = np.fft.fft(b, N)
    
    return np.fft.ifft(a_fft * b_fft)[:n]

#========
# 标准答案
def fft_convolve(a, b):
    l = len(a) + len(b) - 1
    a = np.fft.fft(a, l)	#自动按照2**np.ceil(np.log2(l))来补零
    b = np.fft.fft(b, l)
    return np.real(np.fft.ifft(a * b))	#只选取实数部分：我觉得没有道理，影响了幅值@注意看Forum问答
```



```python
# Verändern Sie diesen Block nicht
fft_result = fft_convolve(audio[:1000], f)
np.testing.assert_allclose(numpy_result, fft_result)

# Verändern Sie diesen Block nicht
fft_result = fft_convolve(audio, f)
Audio(fft_result, rate=sample_rate)
```





## FFT

典型的FFT图像：

- 左半边(正频率)是频率的正轴
- 右半边(负频率)向频率负轴平移一个$f_A$ => 和左半边基于零轴对称

![image-20210428185812143](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210428185812.png)

1 后面补零不影响结果：相当于频域插值，让频域更平滑，改善频域分辨率(直观视觉)(但与频域分辨力无关)

2 FFT实现算法：Divide and Conquer => Mergesort

- DFT公式：频率因子相同@`Uebung1 补充FFT`
  - 拆分为奇、偶两部分
  - ![image-20210428204201754](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210428204202.png)
- 单次递归完成后：该层级的频率因子都已经得到



3 如何理解频谱的复数？⭐

- 第一，从定义式上看，积分号里含有复数，积分结果是复数
- 第二，从傅立叶变换的物理意义上看：FT变换是将一个信号分解为多个信号之和的形式，并且是正弦或余弦信号叠加的形式
  - 决定一个正弦波的是其振幅和相位，二者缺一不可
  - 实数只能表示振幅或者相位，而复数是二维平面上的，可以同时表示振幅和相位，所以用复数表示
  - 频谱是复数形式，可以==分解为振幅谱和相位谱==，它们是实数形式
    - 复数的模就表示振幅，辐角表示相位
    - 振幅和相位都与频率一一对应，分别组成振幅谱和相位谱
    - 它们都包含在频谱里(复数谱)。也就是说频谱即有振幅谱信息，又有相位谱信息



## 技巧|补零

1 使FFT的样本数量是2的幂次

2 显示分辨率 vs. 基本频率分辨力

在时域波形的末端添加零并不能改善与时域信号相关的基本频率分辨率。

提高时域信号的频率分辨率的唯一方法是增加采集时间，获取更长的时间记录。



除了使样本总数为2的幂，以便通过使用快速傅里叶变换（FFT）实现更快的计算外，零填充可以导致频域内插的FFT结果，这可以产生更高的显示分辨率。



# 拓展

1 FFT原理理解@Roam: [[.FFT]]

2 [link: FFT实现的不同方法 2019.04.14](https://whjkm.github.io/2018/08/22/%E5%8D%B7%E7%A7%AF%E5%92%8C%E5%BF%AB%E9%80%9F%E5%82%85%E9%87%8C%E5%8F%B6%E5%8F%98%E6%8D%A2%EF%BC%88FFT%EF%BC%89%E7%9A%84%E5%AE%9E%E7%8E%B0/)

- [link: matlab官网|FFT](https://ww2.mathworks.cn/help/matlab/ref/fft.html)

3 [link: numpy官方|DFT](https://numpy.org/doc/stable/reference/routines.fft.html)

- [link: 输入是实数](https://numpy.org/doc/stable/reference/routines.fft.html#real-ffts)
- 输入是实数时：输出偶对称
- 输入离散：输出周期

4 [link: CSDN|共轭与共轭对称性](https://blog.csdn.net/Reborn_Lee/article/details/81136018?utm_medium=distribute.pc_aggpage_search_result.none-task-blog-2~aggregatepage~first_rank_v2~rank_aggregation-1-81136018.pc_agg_rank_aggregation&utm_term=%E5%82%85%E9%87%8C%E5%8F%B6%E5%8F%98%E6%8D%A2%E7%9A%84%E5%85%B1%E8%BD%AD%E5%AF%B9%E7%A7%B0%E6%80%A7&spm=1000.2123.3001.4430)

- 信号x为实数时，输出X满足共轭对称：$X(-jw) = X^*(jw)$

- 实数分解为奇函数+偶函数
  - 实偶函数cos(t)：变换为实偶函数
  - 实奇函数sin(t)：变换为纯虚 **奇函数**

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210428205333.png" alt="image-20210428205333200" style="zoom: 50%;" />



5 [link: 计算FFT|Overlap-Add-Methode](https://en.wikipedia.org/wiki/Overlap%E2%80%93add_method)

- 区块卷积 ( block convolution, sectioned convolution )，可以有效的计算一个很长的信号 x[n] 和一个 FIR 滤波器 h[n] 的离散卷积
- @代码|Uebung1答案



