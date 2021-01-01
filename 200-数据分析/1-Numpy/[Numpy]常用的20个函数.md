# [Numpy]常用的20个函数

![image-20201205104942043](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201205104942.png)

## 3. arange

Create an array of evenly spaced values between two limits.

```python
np.arange(start=1.5, stop=8.5, step=0.7, dtype=float)
```

![image-20201205105256305](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201205105256.png)



## 8. mean

Get mean of all values in list/array or along rows or columns.

```python
# [1, 2, 3, 4]*3 => [1, 2, 3, 4, 1, 2, 3, 4, 1, 2, 3, 4]
vals = np.array([1, 2, 3, 4]*3).reshape((3, 4))
print(vals)
print('')
print('mean entire array =', np.mean(vals))
print('mean along columns =', np.mean(vals, axis=0))
print('mean along rows =', np.mean(vals, axis=1))
```

![image-20201205105411795](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20201205105411.png)

