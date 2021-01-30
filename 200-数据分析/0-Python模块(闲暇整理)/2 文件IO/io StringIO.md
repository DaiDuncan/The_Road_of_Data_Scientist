很多时候，数据读写不一定是文件，也可以==在内存中==读写。



# StringIO

StringIO顾名思义就是在内存中==读写str==



## 写入

```python
>>> from io import StringIO
>>> f = StringIO()		#先创建一个StringIO
>>> f.write('hello')
5
>>> f.write(' ')
1
>>> f.write('world!')
6
>>> print(f.getvalue())	#用于获得写入后的str
hello world!
```





## 读取

```python
>>> from io import StringIO
>>> f = StringIO('Hello!\nHi!\nGoodbye!')	#用一个str初始化StringIO
>>> while True:
...     s = f.readline()	#一行一行读取，遇到\n即读取一行
...     if s == '':
...         break
...     print(s.strip())
...
Hello!
Hi!
Goodbye!
```





# BytesIO

StringIO操作的只能是str，如果要==操作二进制数据==，就需要使用BytesIO

BytesIO实现了在内存中读写bytes

## 写入

```python
>>> from io import BytesIO
>>> f = BytesIO()
>>> f.write('中文'.encode('utf-8'))	#注意，写入的不是str，而是经过UTF-8编码的bytes
6
>>> print(f.getvalue())
b'\xe4\xb8\xad\xe6\x96\x87'
```



## 读取

```python
>>> from io import BytesIO
>>> f = BytesIO(b'\xe4\xb8\xad\xe6\x96\x87')	#用一个bytes初始化BytesIO
>>> f.read()
b'\xe4\xb8\xad\xe6\x96\x87'
```





---

StringIO和BytesIO是在内存中操作str和bytes的方法，使得和读写文件具有一致的接口