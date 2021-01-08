# Matlab工具箱 | DSP

## 一 FFT：快速傅里叶变换

- 采样率决定输入序列x=0:1/fs:1
- 横坐标(0:nfft-1)*fs/nfft

| fft(x)               | 计算向量x的DFT  得到频率向量f=fA•(-n/2:n/2)/n                |
| -------------------- | ------------------------------------------------------------ |
| ifft(x)              | 计算IDFT                                                     |
| fftshift(x)          | 调整DFT的频谱：使得关于零点对称(一般输出为0-fA部分)  调整后：-fA/2~fA/2 |
| linspace配合fftshift | x=linspace(-fs/2,fs/2-1,nfft);                               |
|                      |                                                              |





## 二 Filter：滤波



| filter(B, A, x) | 向量x经过滤波器B/A |
| --------------- | ------------------ |
| realimg         |                    |
| angleabs        |                    |





## 三 模块 | 可视化

| semilogxsemilogyloglog | 对数坐标轴              |
| ---------------------- | ----------------------- |
| subplot(n,m,z)         | 序数z：从上左到下右计算 |
| grid on/off            |                         |
| zoom on/off            |                         |
| hold on/off            |                         |
| title()                |                         |
| legend(x,y,…)          |                         |