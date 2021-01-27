# 1 字符串方法

```text
1. 将字符串 "abcd" 转成大写
2. 计算字符串 "cd" 在 字符串 "abcd"中出现的位置
3. 字符串 "a,b,c,d" ，请用逗号分割字符串，分割后的结果是什么类型的？
4. "{name}喜欢{fruit}".format(name="李雷") 执行会出错，请修改代码让其正确执行
5. string = "Python is good", 请将字符串里的Python替换成 python,并输出替换后的结果
6. 有一个字符串 string =  "python修炼第一期.html"，请写程序从这个字符串里获得.html前面的部分，要用尽可能多的方式来做这个事情
7. 如何获取字符串的长度？
8. "this is a book",请将字符串里的book替换成apple
9. "this is a book", 请用程序判断该字符串是否以this开头
10. "this is a book", 请用程序判断该字符串是否以apple结尾
11. "This IS a book"， 请将字符串里的大写字符转成小写字符
12. "This IS a book"， 请将字符串里的小写字符，转成大写字符
13. "this is a book\n"， 字符串的末尾有一个回车符，请将其删除
```

答案：

```python
1. "abcd".upper()
2. "abcd".find('cd')
3. "a,b,c,d".split(',')
4. "{name}喜欢{fruit}".format(name="李雷", fruit='苹果')
5. string.replace('Python', 'python')
6. string[0:string.find('.html')] 或者string[0:-5]
7. 使用len函数
8. "this is a book".replace('book', 'apple')
9. "this is a book".startswith('this')
10. "this is a book".endswith('apple')
11. "This IS a book".lower()
12. "This IS a book".upper()
13. "this is a book\n".strip()	#移除字符串头尾指定的字符：默认为空格或换行符
```





# 2 测试验证

```python
string = "Python is good"
1. string[1:20]	#切片时，不论是开始位置还是结束位置，超出索引范围都不会报错，我猜，大概是由于切片是一个范围操作，这个范围内有值就切出来，没值返回空字符串就好了。
2. string[20]
3. string[3:-4]
4. string[-10:-3]
5. string.lower()
6. string.replace("o", "0")
7. string.startswith('python')
8. string.split()
9. len(string)
10. string[30]
11. string.replace(" ", '')
```

答案：

```python
1. 'ython is good'
2. 报错
3. 'hon is '
4. 'on is g'
5. 'python is good'
6. 'Pyth0n is g00d'
7. False
8. ['Python', 'is', 'good']
9. 14
10. 报错
11. 'Pythonisgood'
```





# #参考文献

[Link: 酷Python](http://www.coolpython.net/python_primary/data_type/str_exercises.html)