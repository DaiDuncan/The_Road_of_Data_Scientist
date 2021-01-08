# Python 基本操作 | 格式化输出

## 一 %符号

%[(name)][flags][width].[precision]typecode

- (name) 可选，用于选择指定的key
- flags 可选，可供选择的值有:

- - \+ 右对齐；正数前加正号，负数前加负号
  - \- 左对齐；正数前无符号，负数前加负号；
  - 空格 右对齐；正数前加空格，负数前加负号；
  - 0 右对齐；正数前无符号，负数前加负号；用0填充空白处

- width 可选，占有宽度
- .precision 可选，小数点后保留的位数
- typecode 必选

- - s，获取传入对象的__str__方法的返回值，并将其格式化到指定位置
  - r，获取传入对象的__repr__方法的返回值，并将其格式化到指定位置
  - c，整数：将数字转换成其unicode对应的值，10进制范围为 0 <= i <= 1114111（py27则只支持0-255）；字符：将字符添加到指定位置
  - o，将整数转换成 八 进制表示，并将其格式化到指定位置
  - x，将整数转换成十六进制表示，并将其格式化到指定位置
  - d，将整数、浮点数转换成 十 进制表示，并将其格式化到指定位置
  - e，将整数、浮点数转换成科学计数法，并将其格式化到指定位置（小写e）
  - E，将整数、浮点数转换成科学计数法，并将其格式化到指定位置（大写E）
  - f， 将整数、浮点数转换成浮点数表示，并将其格式化到指定位置（默认保留小数点后6位）
  - F，同上
  - g，自动调整将整数、浮点数转换成 浮点型或科学计数法表示（超过6位数用科学计数法），并将其格式化到指定位置（如果是科学计数则是e；）
  - G，自动调整将整数、浮点数转换成 浮点型或科学计数法表示（超过6位数用科学计数法），并将其格式化到指定位置（如果是科学计数则是E；）
  - %，当字符串中存在格式化标志时，需要用 %%表示一个百分号  

```
# 注：Python中百分号格式化不存在 自动将整数转换成二进制表示的方式
tpl = "i am %s" % "morra"
tpl = "i am %s age %d" % ("morra", 18)

tpl = "i am %(name)s age %(age)d" % {"name": "alex", "age": 18}

tpl = "percent %.2f%%" % 99.97623     #percent 99.98%
tpl = "percent %.2f%%" % 99.9       #percent 99.90%

tpl = "i am %(pp).2f" % {"pp": 123.425556, }
tpl = "i am %(pp).2f %%" % {"pp": 123.425556, }
```





## 二 Format方式

[[fill]align][sign][#][0][width][,][.precision][type]

- fill 【可选】空白处填充的字符
- align 【可选】对齐方式（需配合width使用）

- - <，内容左对齐
  - \>，内容右对齐(默认)
  - ＝，内容右对齐，将符号放置在填充字符的左侧，且只对数字类型有效。 即使：符号+填充物+数字
  - ^，内容居中

  

- sign 【可选】有无符号数字

- - +，正号加正，负号加负；
  - -，正号不变，负号加负；
  - 空格 ，正号空格，负号加负；



- \# 【可选】对于二进制、八进制、十六进制，如果加上#，会显示 0b/0o/0x，否则不显示

- width 【可选】格式化位所占宽度

- , 【可选】为数字添加分隔符，如：1,000,000

- .precision 【可选】小数位保留精度

- type 【可选】格式化类型

  1）传入” **字符串类型** “的参数：

  - s，格式化字符串类型数据
  - 空白，未指定类型，则默认是None，同s

  

  2）传入“ **整数类型** ”的参数

  - b，将10进制整数自动转换成2进制表示然后格式化
  - c，将10进制整数自动转换为其对应的unicode字符
  - d，十进制整数
  - o，将10进制整数自动转换成8进制表示然后格式化；
  - x，将10进制整数自动转换成16进制表示然后格式化（小写x）
  - X，将10进制整数自动转换成16进制表示然后格式化（大写X） 

  

  3）传入“ **浮点型或小数****类型** ”的参数
  - e， 转换为科学计数法（小写e）表示，然后格式化；
  - E， 转换为科学计数法（大写E）表示，然后格式化;
  - f ， 转换为浮点型（默认小数点后保留6位）表示，然后格式化；
  - F， 转换为浮点型（默认小数点后保留6位）表示，然后格式化；
  - g， 自动在e和f中切换
  - G， 自动在E和F中切换
- %，显示百分比（默认显示小数点后6位）

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



## 参考文献

[[转摘\]cnblogs | 格式化输出](https://www.cnblogs.com/whatisfantasy/p/5975582.html)

[Tutorial | 官网教程](https://docs.python.org/3/library/string.html)