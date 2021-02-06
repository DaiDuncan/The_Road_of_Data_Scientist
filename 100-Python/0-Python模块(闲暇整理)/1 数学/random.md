提供一系列方法可以让你获得随机数

| 函数 示例：random.random()    |                                                              |
| ----------------------------- | ------------------------------------------------------------ |
| .random()                     | 生成一个[0, 1.0)之间的随机浮点数                             |
| .uniform(a, b)                | 生成一个[a, b]之间的随机浮点数                               |
| .randint(a, b)                | 生成一个[a, b)之间的随机整数                                 |
| .choice([list])               | 从列表中随机返回一个元素                                     |
| .shuffle([list])              | 将列表中元素随机打乱                                         |
| .sample([list], k)            | 从指定列表中随机获取K个元素                                  |
| .randrange(start, stop, step) | [start, stop)间隔为step的一个随机整数<br>效果等价于random.choice(range(start, stop, step)) |

```python
import random

lst = [3, 4, 1, 6, 8]
print(random.choice(lst))

print(random.sample(lst, 3))

random.shuffle(lst)
print(lst)
```



# 1 随机数

```python
import random
print random.random()
print random.randint(1,2)
print random.randrange(1,10)
```







# 2 生成随机验证码

```python
import random
checkcode = ''
for i in range(4):
    current = random.randrange(0,4)
    if current != i:
        temp = chr(random.randint(65,90))
    else:
        temp = random.randint(0,9)
    checkcode += str(temp)
print checkcode
```





# # 参考文献

[Link: 官方文档](https://docs.python.org/3/library/random.html)