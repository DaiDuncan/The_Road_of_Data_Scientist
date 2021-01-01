# [SciPy]最常用的函数

![image-20201205110228282](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201205110228.png)



## 1. stats

A module containing various statistical functions and distributions (continuous and discrete).

不同的统计分布函数：离散、连续

```python
# Normal distribution:
 
# plot Gaussian
x = np.linspace(-5,15,50)
plt.plot(x, sp.stats.norm.pdf(x=x, loc=5, scale=2))
 
# plot histogram of randomly sampling
np.random.seed(3)
plt.hist(sp.stats.norm.rvs(loc=5, scale=2, size=200),
                           bins=50, normed=True, color='red', alpha=0.5)
plt.show()
```



## 5. linalg

Among other things, this module contains linear algebra functions including inverse (linalg.inv), determinant (linalg.det), and matrix/vector norm (linalg.norm) along with eigenvalue tools e.g., linalg.eig.

线性代数函数：矩阵求逆，行列式，特征向量、特征值

```python
matrix = np.array([[4.3, 8.9],[2.2, 3.4]])
print(matrix)
print('')
 
# Find norm
norm = sp.linalg.norm(matrix)
print('norm =', norm)
# Alternate method
print(norm == np.square([v for row in matrix for v in row]).sum()**(0.5))
print('')
 
# Get eigenvalues and eigenvectors
eigvals, eigvecs = sp.linalg.eig(matrix)
print('eigenvalues =', eigvals)
print('eigenvectors =\n', eigvecs)
```

![image-20201205110453332](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201205110453.png)



## 6.  interpolate

A module containing splines and other interpolation tools.

差值函数

```python
# Spline fit for scattered points
 
x = np.linspace(0, 10, 10)
xs = np.linspace(0, 11, 50)
y = np.array([0.5, 1.8, 1.3, 3.5, 3.4,
5.2, 3.5, 1.0, -2.3, -6.3])
spline = sp.interpolate.UnivariateSpline(x, y)
plt.scatter(x, y); plt.plot(xs, spline(xs))
plt.show()
```

![image-20201205110556638](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201205110556.png)



## 8. signal

This module must be import directly. It contains tools for signal processing.

```python
# Fit noisy signal smooth line
import scipy.signal
np.random.seed(0)
 
# Create noisy data
x = np.linspace(0,6*np.pi,100)
y = [sp.special.sph_jn(n=3, z=xi)[0][0] for xi in x]
y = [yi + (np.random.random()-0.5)*0.7 for yi in y]
# y = np.sin(x)
 
# Get paramters for an order 3 lowpass butterworth filter
b, a = sp.signal.butter(3, 0.08)
 
# Initialize filter
zi = sp.signal.lfilter_zi(b, a)
 
# Apply filter
y_smooth, _ = sp.signal.lfilter(b, a, y, zi=zi*y[0])
 
plt.plot(x, y, c='blue', alpha=0.6)
plt.plot(x, y_smooth, c='red', alpha=0.6)
plt.title('Noisy spherical bessel function signal processing')
plt.savefig('noisy_signal_fit.png', bbox_inches='tight')
plt.show()
```

![image-20201205110753410](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201205110753.png)



## 10. misc

A module containing “utilities that don’t have another home”. Based on the google search results, people often use `misc.imread` and `mics.imsave` to **open and save pictures**.

```python
# Get a raccoon face
 
# Get the raccoon
# 两张图：第二张是灰度图
pics = sp.misc.face(), sp.misc.face(gray=True)
 
# Look at it
fig, axes = plt.subplots(1, 2, figsize=(10, 4))
for pic, ax in zip(pics, axes):
    ax.imshow(pic); ax.set_xticks([]); ax.set_yticks([])
plt.show()
```

![image-20201205110852136](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201205110852.png)

