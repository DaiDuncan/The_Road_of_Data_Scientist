![image-20210511212144938](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210511212145.png)



![image-20210511212200004](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210511212200.png)

![image-20210511212215964](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210511212216.png)



# 要点

## 1 多元正态分布

### 方法：数值积分

累积分布函数（Cumulative Distribution Function，CDF）就是概率密度函数（Probability Density Function，PDF）的积分。

原函数可以用初等函数表示的函数为数不多，大部分的可积函数的积分无法用初等函数表示，甚至无法有解析表达式，比如在统计学上很重要的正态分布函数 ![[公式]](https://www.zhihu.com/equation?tex=%5CPhi%28t%29%3D%5Cint_%7B-%5Cinfty%7D%5E%7Bt%7D%5Cfrac%7B1%7D%7B%5Csqrt%7B2%5Cpi%7D%7De%5E%7B-%5Cfrac%7Bx%5E%7B2%7D%7D%7B2%7D%7Ddx) 就是这样一个无法用初等函数直接计算的函数。

- 数值积分法的复化辛普森公式
- 无穷级数的泰勒级数法求解这个积分





### 方法：库函数

#### 1 scipy.stats

[官网|scipy.stats.multivariate_normal()](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.multivariate_normal.html?highlight=multivariate_normal#scipy.stats.multivariate_normal)

```python
rv = multivariate_normal(mean=None, scale=1)
'''
x ： 数组，分位数，最后一个x轴标记组件。
mean：数组，可选的。分布的均值（默认为0）
cov ：数组，可选的。分布的协方差矩阵(默认为1)
'''

import matplotlib.pyplot as plt
from scipy.stats import multivariate_normal

x = np.linspace(0, 5, 10, endpoint=False)
y = multivariate_normal.pdf(x, mean=2.5, cov=0.5)
y
'''
array([ 0.00108914,  0.01033349,  0.05946514,  0.20755375,  0.43939129,
        0.56418958,  0.43939129,  0.20755375,  0.05946514,  0.01033349])
'''
```





```python
from scipy.stats import multivariate_normal
rv = multivariate_normal(mean, cov) #rv = multivariate_normal([0.5, -0.2], [[2.0, 0.3], [0.3, 0.5]])
prob = rv.pdf([0,1])


###
np.random.multivariate_normal(mean, cov, 5000).shape

mean = [0, 0]
cov = [[1, 0], [0, 100]]  # diagonal covariance 在第一个轴上方差为1，第二个轴方差为100


x, y = np.random.multivariate_normal(mean, cov, 5000).T  #随机生成5000个样本  => 2*5000 => x,y分别占一行
plt.plot(x, y, 'x')
plt.axis('equal')
plt.show()


### 
mn = np.random.randint(10, size=5)
flat_data = data.reshape(-1, 3)

np.einsum("Ni,Nj->Nij", diff, diff).shape	#(250000, 3, 3)
np.einsum("Ni,Nj->Nij", diff, diff).mean(axis=0) #3
```



#### 2 np.random

```python
import matplotlib.pyplot as plt
import numpy as np

mu, sigma = 0.5, 0.1
s = np.random.normal(mu, sigma, 1000)

# Create the bins and histogram
count, bins, ignored = plt.hist(s, 20, normed=True)

# Plot the distribution curve
plt.plot(bins, 1/(sigma * np.sqrt(2 * np.pi)) *
    np.exp( - (bins - mu)**2 / (2 * sigma**2) ),       linewidth=3, color='y')
plt.show()
```

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210518203701.png" alt="image-20210518203701369" style="zoom:80%;" />



## 2 图像处理模块 

0 matplotlib

```python
from matplotlib import pyplot as plt
plt.imshow(data, interpolation='nearest')
plt.show()
```



1 PIL

```python
from PIL import Image
import numpy as np

w, h = 512, 512
data = np.zeros((h, w, 3), dtype=np.uint8)
data[0:256, 0:256] = [255, 0, 0] # red patch in upper left
img = Image.fromarray(data, 'RGB')
img.save('my.png')
img.show()
```



问题：[TypeError: Cannot handle this data type](https://stackoverflow.com/questions/55319949/pil-typeerror-cannot-handle-this-data-type)

改正：

- x只包含uint数值 [0, 255]
  - me|x只包含0-4这几个整数

拓展|许多图形库(e.g. matplotlib, opencv, scikit-image)有两种方式展示图片

1. uint [0, 255]
2. float [0...1] => 更受欢迎(尤其是CV中)
   - However PIL seems to **not support it for RGB images**.

```python
from PIL import Image

x  # a numpy array representing an image, shape: (256, 256, 3)
Image.fromarray(x)	#报错

#==# 改正
im = Image.fromarray((x * 255).astype(np.uint8))
```

2 scipy.misc

```python
from scipy.misc import imshow
imshow(data)
```





# 总结|通用

## 1 打开图片

```python
%matplotlib notebook
import numpy as np
from PIL import Image
import matplotlib.pyplot as plt

image = Image.open("data.png")
# RGB 0-2一共3个通道 => 归一化到0-1浮点数
data = np.array(image)[:, :, :3].astype(float) / 255
print(data.shape)	#(500, 500, 3) 长、宽像素点均为500
image
```



## 2 搭建模型

### me|思路

```python
class GMM:
    def __init__(self, num_classes, num_dims):
        self.num_classes = num_classes #K classes
        self.num_dims = num_dims #3维色彩空间
        self.priors = np.empty(num_classes)
        self.means = np.empty((num_classes, num_dims)) #(row, column)=(K, RGB)
        self.covariances = np.empty((num_classes, num_dims, num_dims))
    
    def fit(self, data):
        print("data: {}" .format(data.shape))
        original_shape = data.shape #500*500*3
        assert data.shape[-1] == self.num_dims
        data = data.reshape(-1, self.num_dims)  #500*500图像变成一个长向量
        
        self.initialize_parameters(data)
        print("initial OK\n")
        
        log_likelihood = self.log_likelihood(data) / len(data) #reshape之后：len(data)==2500
        print("log_likelihood OK\n")
        
        while True:
            weights = self.e_step(data)
            print("e_step OK\n")
            assert weights.shape == (len(data), self.num_classes) #2500 * K
            assignments = np.argmax(weights, axis=1).reshape(original_shape[:-1])
            yield log_likelihood, assignments
            
            assert weights.shape == (len(data), self.num_classes)
            self.m_step(data, weights)
            print("m_step OK\n")
            new_likelihood = self.log_likelihood(data) / len(data)
            if new_likelihood < log_likelihood + 1e-3:
                break
            log_likelihood = new_likelihood
        yield new_likelihood, self.classify(data.reshape(original_shape))
    
    def classify(self, data):
        assert data.shape[-1] == self.num_dims
        flat_data = data.reshape(-1, self.num_dims)
        
        weights = self.e_step(flat_data)
        assignments = np.argmax(weights, axis=1)
        assignments = assignments.reshape(data.shape[:-1])
        print("Finally: classify OK\n")
        return assignments
    
    def initialize_parameters(self, data):
        self.priors[:] = 1 / self.num_classes  # alpha_k = 1/K
        self.means[:] = data[np.random.randint(len(data), size=self.num_classes)]  #维度是K类，数值范围在1-N以内
        
        global_mean = data.mean(axis=0)  #跨行：相当于列 => 也就是针对每个类别(所有样本)
        diff = data - global_mean
        global_covariance = np.einsum("Ni,Nj->Nij", diff, diff).mean(axis=0)
        self.covariances[:] = global_covariance
        #print("initial", global_covariance, '\n')
        #print("initial", self.covariances[1],"\n\n", self.covariances[2])

###==######==######==######==######==######==######==######==######==######==######==###me|答案
	def log_likelihood(self, data): #data是一个长向量 250000*3
        # Your code here
        def likelihood_one_data(one_data):  
            '''
            INPUTS:
                one_data: (3,)
            '''
            prob_one_data = 0
            for k in range(self.num_classes):
                prob_one_data += self.priors[k] * self.calc_normal_prob(one_data, self.means[k], self.covariances[k])
            return prob_one_data
        
        res = 0
        for one_data in data:
            res += np.log(likelihood_one_data(one_data))

        return res
    
    def e_step(self, data): #data是一个长向量 250000*3 => 返回权重weights 250000 * K
        '''
        INPUTS:
            data: (250000, 3)
        OUTPUTS:
            weights: (250000, 5)
        '''
        # Your code here
        # w_ik的公式
        #== 列表生成式：针对k个分布
        return np.array([self.calc_poster_prob(x) for x in data])  #需要从列表改为np.numpy吗？
    
    
    def m_step(self, data, weights): #data是一个长向量 250000*3 => 更新统计属性
        '''
        INPUTS:
            data: (250000, 3)
            weights: (250000, 5)
        UPDATES:
            self.priors: (5,)
            self.means: (5, 3)
            self.covariances: (5, 3, 3)
        '''
        # Your code here
        count_class_k = np.sum(weights, axis=0) #np.sum(weights, axis=0) => (5,)
        sums_class_k = np.matmul(weights.T, data) #=> (5, 3)

        self.priors = count_class_k / len(data) 
        #self.means = [raw_weights[i] / count_class_k[i] for i in range(self.classes)]

        for k in range(self.num_classes):
            self.means[k] = sums_class_k[k] / count_class_k[k]
            
            diff = data - self.means[k] #(250000, 3)
            cov_without_weights = np.einsum("Ni,Nj->Nij", diff, diff)    #(250000, 3, 3)
            self.covariances[k] = np.einsum("i,ijk->jk", weights[:, k], cov_without_weights) / count_class_k[k]


    def calc_normal_prob(self, x, mean, cov):
        '''
        INPUTS:
            x - x.shape is (3,) ein pixel from RGB 
            mean - (3,)
            cov - (3, 3)
        '''     
        rv = multivariate_normal(mean, cov) #rv = multivariate_normal([0.5, -0.2], [[2.0, 0.3], [0.3, 0.5]])
        return rv.pdf(x)
    
    def calc_poster_prob(self, x):
        '''
        INPUTS:
            x - x.shape is (self.num_dims,) => ein pixel from RGB 
        OUTPUTS:
              - shape is (self.num_classes, ) => A posteriori probability for x: the number of weights is the number of classes
        '''  
        zaehler = np.empty(self.num_classes)
        for k in range(self.num_classes):
            zaehler[k] = self.calc_normal_prob(x, self.means[k], self.covariances[k]) * self.priors[k]
        return zaehler / np.sum(zaehler)
```



### 参考答案解析

```python
self.num_classes = num_classes 	#K classes
self.num_dims = num_dims 		#3维色彩空间
self.priors = np.empty(num_classes)
self.means = np.empty((num_classes, num_dims)) 					#(row, column)=(K, RGB)
self.covariances = np.empty((num_classes, num_dims, num_dims))	#(k, 3D, 3D)
```



1-0 基本公式|正态分布的概率密度

| 维度(关注整体) 0                 | N=500*500=250000, k=5, dims=3                                |
| -------------------------------- | ------------------------------------------------------------ |
| 数据集$x$                        | (N, dims)                                                    |
| 数据点$x_i$                      | (dims,)                                                      |
| $\alpha_k$                       | (k, )                                                        |
| $\mu_k$                          | (k, dims): 关注类别\|每个类别有3D维度                        |
| $\sum_k$                         | (k, dims, dims): 关注类别\|每个类别的协方差矩阵都是3D*3D     |
| $\mathcal{N_{\mu_k, \sum_k}(x)}$ | 针对每一个类别k，每一个样本点i => 得到一个概率值             |
|                                  | 注意：具体的公式只针对一个样本点 => 要考虑整个数据集$x$：只要使用**元素运算**的函数即可 |
| $P(x)$整体(除掉求和符号)         | 针对每一个类别k，每一个样本点i => 得到一个加权的概率值       |
| $P(x)$整体(包含求和符号)         | 遍历所有类别，得到的概率值                                   |
|                                  | 注意：因为加权的影响，这个遍历后的概率值不会超过1            |

![image-20210519160602293](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210519160602.png)

```python
	def get_probs(self, data):	#data是一个长向量 250000*3
        #@@# 输入参数：(…, M, M) array_like
        inv_sigma = np.linalg.inv(self.covariances)	
        det_sigma = np.linalg.det(self.covariances)
        
        #@@# me|借鉴：维度与np.einsum()的运用
        # data[:, np.newaxis] => (250000, 1, 3)
        # self.means[np.newaxis]: (5, 3)=>(1, 5, 3)
        #@@# 基于广播机制 => (250000, 5, 3)
        diffs = data[:, np.newaxis] - self.means[np.newaxis]	
        assert diffs.shape == (len(data), self.num_classes, self.num_dims)	#维度是(250000, k=5, 3D)
        
        #@@#太妙了！！！
        '''me|思路: 琢磨单个样本点的运算关系
        不考虑样本集N => 只考虑单个样本点(3,): 三维向量
        不考虑类别K => 只考虑单个类别
        '''
        mahalanobis = np.einsum("NKi,Kij,NKj->NK", diffs, inv_sigma, diffs)	#维度是(250000, 5)
        probs = np.exp(-0.5 * mahalanobis) / np.sqrt((2 * np.pi)**self.num_dims * det_sigma)
        assert probs.shape == (len(data), self.num_classes)
        return probs	#(250000, 5)
    
    def get_probs_alternativ(self, data):
        # Ohne np.einsum
        inv_sigma = np.linalg.inv(self.covariances)
        det_sigma = np.linalg.det(self.covariances)
        probs = np.empty((len(data), self.num_classes))	#(250000, 5)
        
        #@@# 分k类：单独考虑一个类
        for k in range(self.num_classes):
            # self.means[k]: (3,)
            diffs = data - self.means[k]	#(250000, 3)
            '''太妙了！！！
            diffs[:,np.newaxis] (250000, 1, 3)
            
            inv_sigma[k] (3, 3)
            diffs[:,:,np.newaxis] (250000, 3, 1)
            后两者相乘|最后两个维度对应乘积 => (250000, 3, 1)
            
            最终结果是(250000, 1, 1)
            '''
            mahalanobis = np.matmul(diffs[:,np.newaxis],
                                    np.matmul(inv_sigma[k], diffs[:,:,np.newaxis]))
            assert mahalanobis.shape == (len(data), 1, 1)
            # 后面两个维度是1：所以这250000个结果就是整个数据集 属于类别k的概率(公式的一部分)
            mahalanobis = mahalanobis[:, 0, 0]	
            probs[:, k] = np.exp(-0.5 * mahalanobis) / np.sqrt((2*np.pi) ** self.num_dims * det_sigma[k])
        return probs	#(250000, 5)
```



1-2 e-step：计算权重矩阵(也就是后验概率密度$w_{ik}$)

- me|`stat.multivariate_normal()`直接计算概率密度 => 得到后验概率密度

![image-20210519151135576](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210519151135.png)
$$
\sum_{k=1}^K w_{ik} = 1
$$


| 维度(关注整体) 1-2         | 已知$\mathcal{N(\cdot)}$                    |
| -------------------------- | ------------------------------------------- |
| $w_{ik}$分母               | 遍历所有类别：得到样本点$x_i$的总加权概率值 |
| $w_{ik}$分子               | 类别k的加权概率值                           |
| => $w_{ik}$                | 本质上就是样本点 属于类别k的概率(后验概率)  |
| $\sum_{k=1}^K  w_{ik} = 1$ | 某个样本点：必定属于k类中的一类             |



```python
	def e_step(self, data):	#data是一个长向量 250000*3 => 返回权重weights 250000 * K
        class_probs = self.get_probs(data)	#正态分布的概率密度 => (250000, 5)
        combined = class_probs * self.priors
        assert combined.shape == (len(data), self.num_classes)
        
        #@@# axis=1 跨列求和(遍历类别) => (250000,)
        weights = combined / combined.sum(axis=1, keepdims=True)	#求和的技巧：向量化，而非逐次相加
        assert np.all(np.abs(weights.sum(1) - 1) < 1e-5)	#验证和为1
        return weights	#(250000, 5)
```



1-3 m-step：基于权重矩阵 => 更新分布参数


$$
N_k = \sum_{i=1}^{N} w_{ik}
$$
![image-20210519151435308](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210519151435.png)



| 维度(关注整体) 1-3  | 由1-2：已知$w_{ik}$                                          |
| ------------------- | ------------------------------------------------------------ |
| $N_k$               | 某个类别k下，所有样本概率的和                                |
| => $\alpha_k^{neu}$ | 表示类比k所占的比重(也就是该模型分布占的权重)                |
|                     |                                                              |
| $\mu_k^{neu}$       | 分子：所有样本点属于k类部分的权重和                          |
|                     | ==分母用$N_k$归一化==                                        |
| $\sum_k^{neu}$      | 针对某个类别k：考虑样本点i的权重$w_{ik}$, 计算方差(==分母用$N_k$归一化==) |

```python
	def m_step(self, data, weights):	#data是一个长向量 250000*3, weights (250000, 5) => 更新统计属性
        #@@# me|借鉴：@技巧 向量化求和
        # w_ik 遍历i => 5个类别/分布的权重
        total_weights = weights.sum(axis=0)	#（5，）
        self.priors[:] = total_weights / len(data)
        assert np.abs(self.priors.sum() - 1) < 1e-5
        
        '''
        np.einsum(): 对样本进行遍历 => 表示维度的符号是N
        total_weights[:, np.newaxis] (5, 1) => 使用广播机制 => 结果是(5, 3)
        '''
        self.means[:] = np.einsum("NK,Nd->Kd", weights, data) / total_weights[:, np.newaxis]
        
        #@@# 这个广播机制的构造很妙！！！
        diffs = data[:, np.newaxis] - self.means[np.newaxis]
        assert diffs.shape == (len(data), self.num_classes, self.num_dims)	#(250000, 5, 3)
        '''
        针对单个样本点 => 保留NK => ij外积展开形成3*3的协方差矩阵(250000, 5, 3, 3)
        weights: 构造广播机制 (250000, 5, 1, 1)
        '''
        self.covariances[:] = (np.einsum("NKi,NKj->NKij", diffs, diffs) * weights[:, :, np.newaxis, np.newaxis]).sum(0)	#结果(5, 3, 3)

        #@@# me|借鉴：基于np.newaxis (5, 1, 1) => 构造广播机制 (5, 3, 3)/(5, 1, 1)
        self.covariances /=  total_weights[:, np.newaxis, np.newaxis]
    
    def m_step_alternativ(self, data, weights):
        # Mit weniger np.einsum :)
        total_weights = weights.sum(axis=0)
        #@@# 始终没明白为什么要加[:] => forum询问
        self.priors[:] = total_weights / len(data)
        assert np.abs(self.priors.sum() - 1) < 1e-5
        
        for k in range(self.num_classes):
            self.means[k] = np.dot(weights[:,k], data) / total_weights[k]
            diffs = data - self.means[k]
            self.covariances[k] = (np.einsum("Ni,Nj->Nij", diffs, diffs) * weights[:, k, np.newaxis, np.newaxis]).sum(0) / total_weights[k]
```





2  损失函数：`log l(X)`

![image-20210519151111816](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210519151111.png)

```python
    def log_likelihood(self, data):	#data是一个长向量 250000*3
        probs = self.get_probs(data)
        
         #==# me|借鉴：@技巧 向量化求和
        logprob = np.sum(np.log(np.sum(self.priors * probs, axis=1)))
        return logprob
```





## 3 训练模型

```python
gmm = GMM(5, 3)
train_data = data
# train_data = data[::2,::2]
# train_data = data[::5,::5]
# train_data = data[::10,::10]
np.random.seed(1234)	#随机种子

print(f"Training mit {train_data.size // 3} Datenpunkten")
for i, (likelihood, assignment) in enumerate(gmm.fit(train_data)):	#fit()函数中：yield数据
    print(f"Iteration {i} | log-likelihood {likelihood:.3f}")
```



可视化结果：

```python
import matplotlib
from matplotlib import animation
from IPython.display import HTML
matplotlib.rcParams['animation.html'] = 'html5'

gmm = GMM(5, 3)
algorithm = gmm.fit(data)	#Q：返回的是yield
fig, (ax1, ax2) = plt.subplots(1,2, figsize=(10, 5))
plot1 = ax1.imshow(np.zeros((data.shape[0], data.shape[1]), int))	#都是0：黑色图像
plot2 = ax2.imshow(data)
fig.tight_layout()
assignments, likelihood = None, None

def draw(i):
    global assignments, likelihood
    likelihood, assignments = next(algorithm, (likelihood, assignments))	#回应上述Q：提取出assignments
    plot1.set_clim(vmax=assignments.max())
    plot1.set_data(assignments)
    plot2.set_data(gmm.means[assignments])
    return plot1, plot2

animation.FuncAnimation(fig, draw, frames=25, interval=1000, blit=False)
```

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210519145606.png" alt="image-20210519145605503" style="zoom:80%;" />

## 补充|可视化：等高线

[plt.contour()](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.contour.html)

```python
contour([X, Y,] Z, [levels], **kwargs)
'''
Z(M, N) array-like: The height values over which the contour is drawn.
'''
```



## 总结|技巧

1 `np.newaxis()` => 运用广播机制：比如(5, 3, 3) / (5, 1, 1) => (5, 3, 3)

```python
#==# data:(250000, 3)
#==# self.means[np.newaxis]: (5, 3)
diffs = data[:, np.newaxis] - self.means[np.newaxis]	
'''np.newaxis出现在哪个位置 => 该位置新增维度1
data[:, np.newaxis] => (250000, 1, 3)
self.means[np.newaxis]: (5, 3)=>(1, 5, 3)
'''


#==# me|借鉴：基于np.newaxis => 配凑广播机制
# data[:, np.newaxis] => (250000, 1, 3)
# self.means[np.newaxis]: (5, 3)=>(1, 5, 3)
diffs = data[:, np.newaxis] - self.means[np.newaxis]	#广播机制(250000, 5, 3)
assert diffs.shape == (len(data), self.num_classes, self.num_dims)	#维度是(250000, k=5, 3D)
mahalanobis = np.einsum("NKi,Kij,NKj->NK", diffs, inv_sigma, diffs)	#维度是(250000, 5)
```



2 向量化

- 求和`np.sum(axis=1)`



3 验证

```python
#==# 维度
assert diffs.shape == (len(data), self.num_classes, self.num_dims)	#维度是(250000, k=5, 3D)

#==# 100%成立的结论
assert np.all(np.abs(weights.sum(1) - 1) < 1e-5)	#验证和为1
```



# 知识点

1 数据维度

- Die priors (alpha) sind <Anzahl Klassen>, also 5
- Die Mittelwerte (mu) sind <Anzahl Klassen> x <Anzahl ==Dimensionen==> also 5 x 3
- Die Kovarianzen (Sigma) sind <Anzahl Klassen> x <Anzahl ==Dimensionen==> x <Anzahl =Dimensionen> also 5 x 3 x 3



# 总结

> 关键在于高维数据维度意义的理解
