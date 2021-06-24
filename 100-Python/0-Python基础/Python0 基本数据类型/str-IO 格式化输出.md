# 字符串|格式化输出

## 方式1|基于%符号

不推荐你用这种方法，因为这样写出来的==代码可阅读性较差==，更加友好的方式是使用字符串的format方法



格式符，分别是%s, %d。这些格式符为真实值预留了位置

使用 % 进行格式化时，后面紧跟一个元组

```python
%[(name)][flags][width].[precision]typecode

### 示例
str = format('%02d-%02d-%s' %(a, b, c))
```

- (name) 可选，用于变量字典中指定的key

- flags 可选，可供选择的值有:

| 符号 |                                                   |
| ---- | ------------------------------------------------- |
| \+   | 右对齐；正数前加正号，负数前加负号                |
| \-   | 左对齐；正数前无符号，负数前加负号；              |
| 空格 | 右对齐；正数前加空格，负数前加负号                |
| 0    | 右对齐；正数前无符号，负数前加负号；用0填充空白处 |

- width 可选，占有宽度

- .precision 可选，小数点后保留的位数
- ==typecode 必选==

| 符号  |                                                              |
| ----- | ------------------------------------------------------------ |
| ==s== | 获取传入对象的`__str__`方法的返回值，并将其格式化到指定位置  |
| r     | 获取传入对象的`__repr__`方法的返回值，并将其格式化到指定位置 |
| c     | 单个字符<br>整数：将数字转换成其unicode对应的值，10进制范围为 0 <= i <= 1114111（py27则只支持0-255）<br>字符：将字符添加到指定位置 |
| o     | 将整数转换成八进制表示，并将其格式化到指定位置               |
| ==x== | 将整数转换成十六进制表示，并将其格式化到指定位置             |
| X     | 格式化无符号十六进制数（大写）                               |
| b     | 二进制整数                                                   |
| ==d== | 将整数、浮点数转换成十进制表示，并将其格式化到指定位置       |
| i     | 十进制整数                                                   |
| u     | 格式化无符号整型                                             |
| e     | 将整数、浮点数转换成科学计数法，并将其格式化到指定位置（小写e） |
| E     | 将整数、浮点数转换成科学计数法，并将其格式化到指定位置（大写E） |
| ==f== | 将整数、浮点数转换成浮点数表示，并将其格式化到指定位置（默认保留小数点后6位） |
| F     | 同上                                                         |
| g     | 自动调整将整数、浮点数转换成浮点型或科学计数法表示（超过6位数用科学计数法），并将其格式化到指定位置（如果是科学计数则是e；） |
| G     | 自动调整将整数、浮点数转换成浮点型或科学计数法表示（超过6位数用科学计数法），并将其格式化到指定位置（如果是科学计数则是E；） |
| %     | 当字符串中存在格式化标志时，需要用 %%表示一个百分号          |

```python
# 注：Python中百分号格式化，不存在自动将整数转换成二进制表示的方式
tpl = "i am %s" % "morra"
tpl = "i am %s age %d" % ("morra", 18) #符号s对应'morra'

tpl = "i am %(name)s age %(age)d" % {"name": "alex", "age": 18}  #%(name)对应key"name"

tpl = "percent %.2f%%" % 99.97623     #percent 99.98%(保留百分号)
tpl = "percent %.2f%%" % 99.9       #percent 99.90%

tpl = "i am %(pp).2f" % {"pp": 123.425556, }
tpl = "i am %(pp).2f %%" % {"pp": 123.425556, }



print('%2d-%02d' % (3, 1))
print('%.2f' % 3.1415926)
```





## 方式2|.format()

字符串里希望被替换的内容，用大括号包裹起来

```python
string = "我喜欢 {color},{color}让人安静".format(color='绿色')
```

相比于使用%操作符，format方法更加直观和简单，尤其是在格式化时与字典，元组，对象相结合的情况下

​	使用{}来为真实值预留位置

​	大括号内你可以随意编写变量名称，然后在format方法里为这些变量赋予真实值

```python
[[fill]align][sign][#][0]:[width][,][.precision][type]
```

- fill 【可选】空白处填充的字符
- align 【可选】对齐方式（需配合width使用）

| 符号 |                                                              |
| ---- | ------------------------------------------------------------ |
| \<   | 内容左对齐                                                   |
| >    | 内容右对齐(默认)                                             |
| ＝   | 内容右对齐，将符号放置在填充字符的左侧，==且只对数字类型有效==。 即使：符号+填充物+数字 |
| ^    | 内容居中                                                     |

- sign 【可选】有无符号数字

| 符号 |                      |
| ---- | -------------------- |
| +    | 正号加正，负号加负； |
| -    | 正号不变，负号加负； |
| 空格 | 正号空格，负号加负； |



- \# 【可选】对于二进制、八进制、十六进制，如果加上#，会显示 0b/0o/0x，否则不显示
- width 【可选】格式化位所占宽度
- , 【可选】为数字添加分隔符，如：1,000,000
- .precision 【可选】小数位保留精度
- type 【可选】格式化类型

| 符号                        |                                                     |
| --------------------------- | --------------------------------------------------- |
| 类型1）字符串类型           |                                                     |
| s                           | 格式化字符串类型数据                                |
| 空白                        | 未指定类型，则默认是None，同s                       |
|                             |                                                     |
| 类型2）整数类型             |                                                     |
| b                           | 将10进制整数自动转换成2进制表示然后格式化           |
| c                           | 将10进制整数自动转换为其对应的unicode字符           |
| d                           | 十进制整数                                          |
| o                           | 将10进制整数自动转换成8进制表示然后格式化；         |
| x                           | 将10进制整数自动转换成16进制表示然后格式化（小写x） |
| X                           | 将10进制整数自动转换成16进制表示然后格式化（大写X） |
|                             |                                                     |
| 类型3）**浮点型或小数**类型 |                                                     |
| e                           | 转换为科学计数法（小写e）表示，然后格式化           |
| E                           | 转换为科学计数法（大写E）表示，然后格式化           |
| f                           | 转换为浮点型（默认小数点后保留6位）表示，然后格式化 |
| F                           | 转换为浮点型（默认小数点后保留6位）表示，然后格式化 |
| g                           | 自动在e和f中切换                                    |
| G                           | 自动在E和F中切换                                    |
| %                           | 显示百分比（默认显示小数点后6位）                   |



```python
### 应用示例
"{:.2f}%" .format(float(num * 100))  #显示百分数，保留2位小数；冒号表示数值对象


a1 = "i am {}, age {}, {}".format("seven", 18, 'morra')

a2 = "i am {}, age {}, {}".format(*["seven", 18, 'morra'])

a3 = "i am {0}, age {1}, really {0}".format("seven", 18)

a4 = "i am {0}, age {1}, really {0}".format(*["seven", 18])  #列表前要加上*，和函数的动态参数类似

a5 = "i am {name}, age {age}, really {name}".format(name="seven", age=18)
print(a5)       #i am seven, age 18, really seven

a6 = "i am {name}, age {age}, really {name}".format(**{"name": "seven", "age": 18})
print(a6)       #i am seven, age 18, really seven

a7 = "i am {0[0]}, age {0[1]}, really {0[2]}".format([1, 2, 3], [11, 22, 33])
print(a7)       #i am 1, age 2, really 3

a8 = "i am {:s}, age {:d}, money {:f}".format("seven", 18, 88888.1)
print(a8)       #i am seven, age 18, money 88888.100000

a9 = "i am {:s}, age {:d}".format(*["seven", 18])
print(a9)       #i am seven, age 18

a10 = "i am {name:s}, age {age:d}".format(name="seven", age=18)
print(a10)      #i am seven, age 18

a11 = "i am {name:s}, age {age:d}".format(**{"name": "seven", "age": 18})
print(a11)      #i am seven, age 18

a12 = "numbers: {:b},{:o},{:d},{:x},{:X}, {:%}".format(15, 15, 15, 15, 15, 15.87623, 2)
print(a12)      #numbers: 1111,17,15,f,F, 1587.623000%

a13 = "numbers: {:b},{:o},{:d},{:x},{:X}, {:%}".format(15, 15, 15, 15, 15, 15.87623, 2)
print(a13)      #numbers: 1111,17,15,f,F, 1587.623000%

a14 = "numbers: {0:b},{0:o},{0:d},{0:x},{0:X}, {0:%}".format(15)
print(a14)      #numbers: 1111,17,15,f,F, 1500.000000%

a15 = "numbers: {num:b},{num:o},{num:d},{num:x},{num:X}, {num:%}".format(num=15)
print(a15)      #numbers: 1111,17,15,f,F, 1500.000000%
```



### 与字典结合的用法

真实值存储在一个字典里，且模板里预留了好多位置需要填充真实值

- 这两种格式化的方式都可行，个人推荐使用第二种

```python
info = {
    'id':1,
    'type':'debug',
    'msg':u'测试连接'
}
log = "{0[id]}-{0[type]}-{0[msg]}".format(info)
print(log)

log = "{info[id]}-{info[type]}-{info[msg]}".format(info=info)
print(log)
```



### 与tuple结合的方法

```python
info = (1,'debug',u'测试连接')
log = "{0[0]}-{0[1]}-{0[2]}".format(info)
print(log)

log = "{info[0]}-{info[1]}-{info[2]}".format(info=info)
print(log)
```



### 与对象结合的方法

```python
class Info(object):
    def __init__(self, id, type, msg):
        self.id = id
        self.type = type
        self.msg = msg

info = Info(1,'debug',u'测试连接')
log = "{info.id}-{info.type}-{info.msg}".format(info=info)
print(log)
```







## 小结|两种方式的区别

- '==%==03.2f'  %  {}
- {==:==03.2f}  .format{}

```python
'%s%s' % (str1,str2)
'{}{}' .format(str1, str2)
```



## 方式3|f-string

python3.6加入的一种新技术，这种技术称之为字面量格式化字符串(formatted string literals)

本质上f-string不是字符串常量，而是一个可以在运行时运算求值的表达式

​	内部使用大括号为真实值预留位置

```python
color = '红色'
string = f'我喜欢{color}'
print(string)


color = 'red'
string = f"I like {color.upper()}"	#大括号里甚至可以传入表达式和函数
print(string)


info = {'languge': 'python', 'site': 'http://www.coolpython.net'}
print(f"我正在学习info['languge'], 使用的教程网址是info['site']")
```

自动将前面的变量内容填充到字符串中以达到格式化字符串的目的









# #附录 转义字符

| 转义字符       | 描述                                                  |
| :------------- | :---------------------------------------------------- |
| `r" "`或`u" "` | 转义声明(大写R亦可)(多用于文件地址的输入和正则表达式) |
| `\`(在行尾时)  | 续行符                                                |
| `\\`           | 反斜杠符号                                            |
| `\'`           | 单引号                                                |
| `\"`           | 双引号                                                |
| \a             | ==响铃==                                              |
| \b             | 退格(Backspace)                                       |
| \e             | 转义                                                  |
| \000           | 空                                                    |
| \n             | 换行                                                  |
| \v             | 纵向制表符                                            |
| \t             | 横向制表符                                            |
| \r             | 回车(光标重新回到==本行==开头)<br/>空行为\r\n         |
| \f             | 换页                                                  |
| \oyy           | 八进制数，yy代表的字符，例如：\o12代表换行            |
| \xyy           | 十六进制数，yy代表的字符，例如：\x0a代表换行          |
| \other         | 其它的字符以普通格式输出                              |

注意：

写文件要用write方法，但是这个方法是不会主动添加换行符的

- 如果你把代码里的f.write(word + "\n") 修改成f.write(word)，文件里最终只有一行数据

```python
lst = ['book', 'python', 'good']

with open('data', 'w')as f:
    for word in lst:
        f.write(word + "\n")
```



读取文件时，要去掉换行符

- 每一行的末尾的换行符也会被读取，但这个换行符是没有什么作用的，因此需要删除

```python
with open('data', 'r')as f:
    for line in f:
        print(line.strip())
        
        
#output
book
python
good
```



说明：

- linux只用\n换行

- win下用\r\n表示换行

在python中存在继承了 回车符\r 和 换行符\n 两种标记： `.replace('\n', '').replace('\r', '')` 



## 参考文献

[[转摘\]cnblogs | 格式化输出](https://www.cnblogs.com/whatisfantasy/p/5975582.html)

[Tutorial | 官网教程](https://docs.python.org/3/library/string.html#format-examples)

[官网|str.format()](https://docs.python.org/3/library/stdtypes.html#str.format)