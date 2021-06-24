[link: 菜鸟教程|input()函数](https://www.runoob.com/python/python-func-input.html)

Python3.x 中 input() 函数接受一个标准输入数据，返回为 string 类型。

> raw_input() 将所有输入作为字符串看待，返回字符串类型。
>
> input() 在对待纯数字输入时具有自己的特性，它返回所输入的数字的类型（ int, float ）。

```python
>>>a = input("input:")
input:123                  # 输入整数
>>> type(a)
<class 'str'>              # 字符串
>>> a = input("input:")    
input:runoob              # 正确，字符串表达式
>>> type(a)
<class 'str'>             # 字符串
```





## 示例|接收多个值

```python
#!/usr/bin/python
#输入三角形的三边长
a,b,c = (input("请输入三角形三边的长：").split())
a= int(a)
b= int(b)
c= int(c)

#计算三角形的半周长p
p=(a+b+c)/2

#计算三角形的面积s
s=(p*(p-a)*(p-b)*(p-c))**0.5

#输出三角形的面积
print("三角形面积为：",format(s,'.2f'))



### 交互
请输入三角形三边的长：3 4 5
三角形面积为： 6.00
```



与 print() 函数能输出多个参数不同，input() 函数的参数只能为单个字符串。

如果需要输出多个参数作为提示信息，可以用 print() 代替。

也可以使用格式化字符串把参数塞进字符串中：

```python
>>> lis = []
>>> for i in range(2):
...     lis.append(int(input(f'你想输入的第{i+1}个数字是？')))
... 
你想输入的第1个数字是？55
你想输入的第2个数字是？56
>>> lis 
[55, 56]
```

