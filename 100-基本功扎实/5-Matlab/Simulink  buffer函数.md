# Simulink | buffer函数

**意义**：在MATLAB中可以当做发射信号的缓冲，以提高系统的稳定性，从而防止数据的剧烈变化。



## 一 Buffer定义

Buffer：将输入数据数组依据**设定的长度**，进行重新分组。(可以混叠overlap)

关键：将信号缓存为数据帧(向量或矩阵)——注意输入 输出间隔时间





### 1 输入

#### 1）buffer size：控制buffer长度

针对每一个通道

#### 2）overlap size

#### 3）initial value：设置初始条件

对于非零延迟的情况，请指定块的初始输出值：标量，向量或矩阵



### 2 单通道信号（列数为1）

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1592852678498-bbb56025-4f86-4c15-84bc-d04e9436dbe3.png)

| 输入维度           | 输出维度    |
| ------------------ | ----------- |
| 1 x 1(标量)        | Moutput x 1 |
| Minput x 1(列向量) | Moutput x 1 |

#### 1）输入数据帧

- size = Mi 
- buffer size = Mo 
- overlap = L

#### 2）输出有效数：M0-L

一列有M0个数，其中L个数表示重叠。

当buffer满了才会输出该数组



#### 3）Zero-tasking latency

意味着 在t=0时刻，输出直接等于输入——>opt 'no delay'



### 3 多通道

| 输入维度         | 输出维度    |
| ---------------- | ----------- |
| 1 x N(行向量)    | Moutput x N |
| Minput x N(矩阵) | Moutput x N |

#### 1）示例：二通道(N=2)

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1592853181760-f38a29ae-026f-49f8-b0b9-d2352759a6bb.png)

- 注意：输出右边为**初始输出**t=0
- 延时8个采样时间后，每个通道独立发送数据(重叠数据长度L=1)
- 输入虽然为两个数组，但衔接起来是一个整体



#### 2）示例：四通道(N=4)

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1592853279640-345a739a-9bb0-4a9d-89e1-ccad228dc580.png)

- 延时4个采样时间，重叠量L=1(输出每列M0=3个数，有效数为M0-L=2)





## 二 Buffer实例

将一段长序列x分为每帧样点数为n，相邻之间重叠样点数为p的一帧帧信号。

其中x = 1:30, n = 7, p = 3

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1592853516876-281ec947-1508-43bf-8d31-8bbd672934b9.png)

### 1 y=buffer(x, n, p)

p表示重叠数overlap

- 每一列的有效数字为(**n减去p**)，例子中为4个(所以第一列前面3个为0，从1~4)
- 计算列数

- - n_x = length(x);
  - n_frame = ceil((n_x - n + p)/(n - p)) + 1;  %



#### 例1）y=buffer(x, n, 2)

x能被n-2=5整除

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1592853717140-8375a21b-ae73-44a7-aef5-9056cb58821d.png)

#### 例2）y=buffer(x, n, 4)

x能被n-4=3整除

第二列前4个数，与第一列后4个数重叠

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1592853742969-2ca28b95-4663-425f-ab24-ef55d0dc4c02.png)



#### 例3）y=buffer(x, n, -3)

中间跳过3个数

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1592853763312-c4ba36e9-51bd-444f-b5cb-c8dbd7653c5a.png)



### 2 y = buffer(x,n,p,opt) 

当p为负数时，opt的范围在[0, |p|]

#### 例1）y = buffer(1:30, 7, -3, 2)  

中间跳过3个数；开头跳过2个数——末尾缺1个数

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1592853821934-eda9a9ce-78ed-4f5e-9095-b19fcaf37871.png)

#### 例2）y = buffer(1:30,7,-3,0)  

中间跳过3个数，开头不跳——末尾缺3个数

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1592853842615-9acc71d0-61fc-489c-a1f5-a38532872dfe.png)

#### 例3）y = buffer(1:30,7,-4,2) 

最后一列缺的数在|p|以内，否则多出一列后(此处25-30共有6个数)，补零

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1592853865127-9ce48d10-67ee-4783-b3bd-eb9e7c335fcf.png)

####  @解释-opt

当p>0,opt 指定一个长度为p的向量插入在第一个信号的元素之前，opt取默认值时为zeros(p,1),取‘nodelay’时，直接在缓存矩阵中写入信号的第一个元素。

图示对比(右图表示**no delay**)：

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1592853972155-30528084-61d3-4e0d-9d0d-2f3dd8c10db4.png)

当p<0, opt是[0,-P]里面**的一个整数**，比如x=1:30,n=7,p=-3, opt=2

## 三 Unbuffer

Unbuffer：使用unbuffer模块将帧格式再转换为采样格式进行。

参数：只有一个初始条件

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1592853471802-0bbf5ee1-20de-4823-94c0-a7785042fbf9.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_10%2Ctext_6K-t6ZuALUREdW5jYW4%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

本文