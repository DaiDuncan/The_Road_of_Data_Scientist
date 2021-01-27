布尔值：

- True
- False



# 特性

- 特性：Python数据隐含真假值(==注意容器==)

![image](https://cdn.nlark.com/yuque/0/2020/png/1136179/1591176558842-507e5715-cf44-4009-a986-fbaf72945b80.png)

- 特性：短路运算
  在计算a and b时，如果a是True，则计算结果取决于b，返回b；如果a是False，则直接返回a
  在计算a or b时，如果a是True，则直接返回a；否则返回b

```python
>>> bool(1)
True
>>> bool(0)
False

>>> int(True)
1
>>> int(False)
0
>>> float(True)
1.0
>>> float(False)
0.0
```





## 基本操作

- 布尔运算

```python
and or not
bool()  #检查变量的真假值
```

- 关系运算 => 条件判断

```python
==
!=
<
<=
>
>=
```

