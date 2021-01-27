# 字符串前加r|作用是==去除==转义字符

```python
str1= 'input\n'
str= r'input\n'

print(str1) 
#input(+换行符)

print(str) 
#input\n
```



# 字符串前加f|支持{var}

```python
import time

t0 = time.time()

time.sleep(1)
name = 'processing'

# 以 f开头表示在字符串内支持大括号内的python 表达式
print(f'{name} done in {time.time() - t0:.2f} s') 
```

输出结果：

processing done in 1.00 s

- {name}表示变量name
- {表达式}



# 字符串前加b|表示bytes型数据

```python
response = b'<h1>Hello World!</h1>'     # b' ' 表示这是一个 bytes 对象

### python3中：bytes与str的相互转换
str.encode('utf-8')
bytes.decode('utf-8')
```

作用：b" "前缀表示：后面字符串是bytes 类型

用处：网络编程中，服务器和浏览器只认bytes 类型数据。

- 如：send 函数的参数和 recv 函数的返回值都是 bytes 类型



# 字符串前加u|表示中文字符串

例：u"我是含有中文字符组成的字符串。"



作用：

后面字符串==以 Unicode 格式进行编码==，一般用在中文字符串前面，防止因为源码储存格式问题，导致再次使用时出现乱码。





# #参考文献

https://zhuanlan.zhihu.com/p/110102003