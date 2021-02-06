[Link: 官方文档|re库](https://docs.python.org/zh-cn/3/library/re.html#regular-expression-examples)

```python
import re
```

一般不会涉及特别复杂的规则，比如贪婪与懒惰，位置指定，因为这些规则在实际应用中很少见，绝大部分的正则表达式都很简单。



# 元字符/特殊字符

注意：single char是指一个字符

- 比如：`.`只代表任意一个字符

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1592747112651-23fe69e3-f8dd-4f6a-81f7-bf9d2302b24d.png)

反义：

| 代码/语法 | 说明                           |
| :-------- | :----------------------------- |
| \W        | 匹配任意不是字母和数字的字符   |
| \S        | 匹配任意不是空白符的字符       |
| \D        | 匹配任意非数字的字符           |
| \B        | 匹配不是单词开头或结束的位置   |
| [^x]      | 匹配除了x以外的任意字符        |
| [^a-z]    | 匹配除了小写字母意外的任意字符 |



| 备注  |                                |
| ----- | ------------------------------ |
| `\b`  | 匹配单词的开始或者结束         |
|       | `\bis\b`准确匹配单词is         |
| `.`   | 匹配==除换行符以外==的任意字符 |
| {n, } | 重复n次或更多次                |



记忆技巧：

首先，将这些量词分为两部分，一部分是有{}的，一部分是没有{}的，先来看有{}的，可以归结为3类

1. 规定重复次数的 {n}
2. 规定重复次数下限的 {n, }
3. 规定重复次数下限和上限的 {n, m}

再来看* + ?

1. \* 表示0次或多次，这个你只能是死记硬背
2. \+ 是在* 的基础上加1，就变成了1次或多次
3. ? 表示你也很懵逼，所以向老师提问，到底是重复0次还是1次呢？



## 示例|single char, 位置

| 匹配规则                 |                                                            |                                                 |
| ------------------------ | ---------------------------------------------------------- | ----------------------------------------------- |
| `\bl..e\b`<br/>          | 1. 单词长度是4<br/>2. 单词首字母是l<br/>3. 单词末尾字母是e | "like and love" => like 和 love                 |
| `\b\w\w\w\w\b`           | 4个\w表示4个字母或数字                                     | "this is a book" => this 和 book                |
| `a\sdream`               | 空白符包括空格，制表符（tab），换行符这三个很特殊的字符    | "I have a dream" => a dream                     |
| `\d\d\d\d\d\d\d\d\d\d\d` | 匹配字符串中的电话号                                       | "my phone number is 15801121234" => 15801121234 |
| `^this`                  | 验证字符串是不是以this开头                                 | "this is a book"                                |
| book$                    | 验证字符串是以book结尾                                     |                                                 |



## 示例|数量

| 匹配规则        |                                                              |                                                              |
| --------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| `\b\w*o\w*\b`   | 从单词中提取出包含字母o的单词(开头，结尾，在中间)<br>\* 表示重复0次或多次：<br>第一个`\w*`：如果\*是0次，表示在开头<br>第二个`\w*`：如果\*是0次，表示在开头 | "book  block  bottom like  love   over  too"                 |
| `\d+`           | \+ 表示重复1次或多次                                         | "今天吃饭花了300块钱，打车花了20块钱，清空淘宝购物车花了1500块钱" => 300 20 1500 |
| `(\+86)?\d{11}` | 提取电话号码<br>\+ 表示重复1次或更多次<br>`\+`表示转义<br>`()`表示分组：作为一个整体<br>? 表示重复0次或1次<br>{n}表示重复n次 | "我有两个电话号 15801121234  和 +8615801122345"              |
|                 | `(+86)?` 表示+86重复0次或1次<br>`\d{11}` 表示数字重复11次    |                                                              |



## 示例

| 匹配规则      |                          |      |
| ------------- | ------------------------ | ---- |
| `^[a-z0-9]+$` | 只能由小写字母和数字构成 |      |
| `^[^0-9]+$`   | 字符串里不能出现数字     |      |





## .  => 除了换行符以外的任何单个字符



## ^ => 只匹配起始字符

```python
temp1=re.findall('^morra','nsudi werwuirnmorra')
temp2=re.findall('^morra','morransudi werwuirn')

print(temp1)    #[]
print(temp2)    #['morra']
```



## $ => 只匹配结尾字符

```python
temp1=re.findall('morra$','nsudi werwuirnmorra')
temp2=re.findall('morra$','morransudi werwuirn')

print(temp1)    #['morra']
print(temp2)    #[]

^roar$    #准确锁定字符串roar
```



## * => 匹配0到多次，等同于{0,}

```python
temp1=re.findall('morra*','wwwmorr')
temp2=re.findall('morra*','wwwmorra')
temp3=re.findall('morra*','wwwmorraa')     #贪婪匹配, morr后面0个或多个字符a

print(temp1)    #['morr']
print(temp2)    #['morra']
print(temp3)    #['morraa']
```



## + => 匹配1到多次，等同于{1,}

```python
temp1=re.findall('morra+','wwwmorr')
temp2=re.findall('morra+','wwwmorra')
temp3=re.findall('morra+','wwwmorraa')  #贪婪匹配, morr后面1个或多个字符a

print(temp1)    #[]
print(temp2)    #['morra']
print(temp3)    #['morraa']
```



## ? => 匹配0到1次，{0,1}

```python
temp1=re.findall('morra?','wwwmorr')
temp2=re.findall('morra?','wwwmorra')
temp3=re.findall('morra?','wwwmorraa')  #贪婪匹配, morr后面0个或1字符a

print(temp1)    #['morr']
print(temp2)    #['morra']
print(temp3)    #['morra']
```



## {} => 自定义匹配次数：{1}匹配1次，{1,2}匹配1到2次

```python
temp1=re.findall('morra{0}','wwwmorr')
temp2=re.findall('morra{2}','wwwmorra')
temp3=re.findall('morra{2}','wwwmorraa')  #贪婪匹配, morr后面有2个字符a

print(temp1)    #['morr']
print(temp2)    #[]
print(temp3)    #['morraa']


abc{2}     #ab后有2个字符c
abc{2,}    #ab后有2个或多个字符c
abc{2,5}     #ab后有2个至5个字符c
```



## [ ] => 字符集，只匹配其中的单个字符⭐

```python
temp2=re.findall('morr[ab]','wwwmorra')
temp3=re.findall('morr[ab]','wwwmorrab')

print(temp2)    #['morra']
print(temp3)    #['morra']


[0-9a-zA-Z\_]					#可以匹配一个数字、字母或者下划线
[a-zA-Z\_][0-9a-zA-Z\_]*		#可以匹配由字母或下划线开头，后接任意个由一个数字、字母或者下划线组成的字符串，也就是Python合法的变量；
[a-zA-Z\_][0-9a-zA-Z\_]{0, 19}	#更精确地限制了变量的长度是1-20个字符（前面1个字符+后面最多19个字符）


[aeiou]   	    #匹配一个元音
[a-z0-9]    	#匹配一个小写字母和数字
[a-fA-F0-9]     #匹配一个十六进制字符
[0-9]%    		#在%符号前是字符0-9
[0-5]  			#表示从0到5的字符
[+?]  			# 表示+ 或者 ? 在中括号里，特殊字符可以不使用字符转义
```

注意：**元字符**在字符集里就不再具有特殊意义，变成了普通字了，但是\d \w \s除外





## [\^] => 反向匹配⭐

```python
temp1=re.findall('[^1-9]','www.m12orr1234') #匹配除数字以外的所有字符
print(temp1) #['w', 'w', 'w', '.', 'm', 'o', 'r', 'r']

[^a-zA-Z]    #非字母
```



## () => 分组与捕获

()表示捕获分组，()会把每个分组里的匹配的值==保存起来==，使用**$n**(n是一个数字，表示第n个捕获组的内容)

(?:)表示**非捕获**分组，和捕获分组唯一的区别在于，非捕获分组匹配的值**不会保存起来**

```python
a(b|c)    #a后可能有b或c
a(bc)    #a后是bc
a(bc)*     #a后有0个或多个bc序列
a(?:bc)*    #?:表示取消捕捉bc
a(?<foo>bc)     #?<foo> 组命名：表示给组(bc)一个名称叫做foo


# 选择分组
$1  
\1
```



##  `\` => 转义字符

(反斜杠/ 没有任何意义)

适用对象：^ . [ $ ( ) | * + ? { \

不可打印的字符：

- \t  tab
- \n  换行
- \r  回车

### 1）后面跟元字符：去除其特殊功能

```python
temp1 = re.findall('\$', 'www.m12$orr1234')
print(temp1)    #['$']
```



### 2）后面跟普通字符 => 元字符/特殊字符

\d 匹配任何单个十进制数digit，等同于[0-9],如果想匹配多个可以使用\d\d,\d+,\d{2}

```python
temp1 = re.findall('\d', 'www.m12orr1234')
temp2 = re.findall('\d\d', 'www.m12orr1234')
temp3 = re.findall('\d\d\d', 'www.m12orr1234')
temp4 = re.findall('\d{2,3}', 'www.m12orr1234')  # 匹配2次，或3次

print(temp1)  # ['1', '2', '1', '2', '3', '4']
print(temp2)  # ['12', '12', '34'] 匹配完了，就截断
print(temp3)  # ['123']
print(temp4)  # ['12', '123']

\$\d    #符号$ 以及数字的序列
```

\D 匹配任何**非**数字字符，等同于[\^0-9]

\s 匹配任何空白字符(Tab、空格)，等同于[ \t\n\r\f\v]

\S 匹配任何**非**空白字符，等同于[\^ \t\n\r\f\v]

\w 匹配任何字母数字字符(字母、数字、下划线)，等同于[A-Za-z0-9]

\W 匹配任何**非**字母数字字符，等同于[\^A-Za-z0-9]

\b 匹配一个单词边界，也就是指单词和空格间的位置

```python
temp = re.findall(r"abc\b", "asdas abc 1231231")
print(temp)     #['abc']

temp = re.findall("abc\\b", "asdas abc 1231231")
print(temp)     #['abc']

temp = re.findall(r"abc\b", "asdas*abc*1231231")
print(temp)     #['abc']，同样也可以识别特殊字符和单词之间的边界

temp = re.findall(r"abc2\b", "asdas*abc2*31231")
print(temp)     #['abc2']，同样也可以识别特殊字符和单词之间的边界

temp = re.findall(r"\bI", "I MISS IOU")
print(temp)     #['I', 'I']


\babc\b    #精确匹配：比如从ab abc abcc babc中精确匹配abc(识别abc前后的空格)
```



### 3）分组编号：引用序号对应的字组所匹配的字符串

```python
temp = re.search(r'(alex)(eric)com\1', 'alexericcomalex').group()   #alexericcomalex，\1就是第一组等同于alex
temp2 = re.search(r'(alex)(eric)com\2', 'alexericcomeric').group()  #alexericcomeric，\2就是第二组等同于eric
```



## `|` 或

| 匹配规则                                                     |                                                              |                                                   |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------- |
| `\bthis\b|\bbook\b`                                          | 或者匹配得上this，或者匹配得上book                           | "this is a book"                                  |
| `(13[0-9]|14[5,7,9]|15[0-3,5-9]|166|17[0-3,5-8]|18[0-9]|19[8,9])\d{8}` | 匹配手机号的表达式<br>13[0-9] 前两位是13，第3位是0到9<br/>14[5,7,9] 前两位是14， 第3位是5， 7， 9<br/>15[0-3,5-9] 前两位是15，第3位是0到3或者5到9<br/>166<br/>17[0-3,5-8] 前两位是17，第3位是0到3或者5到8<br/>18[0-9] 前两位是18 第3位是0到9<br/>19[8,9] 前两位是19 第3位是8，9 | 这7个号码段有任意一个匹配上就可以,后8位则全是数字 |





# 贪婪/懒惰搜索

re库：在进行字符串匹配时会==默认采用“贪婪匹配”==，也就是说会返回符合条件的最长字符串/尽可能多的字符。

通过在操作符后增加一个“?”可以将正则式变成最小匹配：

![image](https://cdn.nlark.com/yuque/0/2020/png/1136179/1591176765814-e8e3f0f4-bc68-490f-9cc7-0796d00e4a50.png)

```python
<.+>    #.+表示任意长度的字符串
<.+?>     #只匹配div标签 ?表示非贪婪匹配
<[^<>]+> 
```

| 贪婪匹配：匹配所有符合项 | 非贪婪：匹配到一个即可 |
| ------------------------ | ---------------------- |
| *                        | ?                      |
| +                        |                        |





# 正则表达式的函数

## 0 正则表达式对象

| re.RegexObject | re.compile() 返回 RegexObject 对象                           |
| -------------- | ------------------------------------------------------------ |
| re.MatchObject | group() 返回被 RE 匹配的字符串<br>start() 返回匹配开始的位置<br>end() 返回匹配结束的位置<br>span() 返回一个**元组包含匹配** (开始,结束) 的位置 |

返回一个对象：match、search、finditer

返回一个列表：findall、split



## 0 编译 re.compile()

当我们在Python中使用正则表达式时，re模块内部会干两件事情：

1. ==编译正则表达式==，如果正则表达式的字符串本身不合法，会报错；
2. 用编译后的正则表达式去匹配字符串。

```python
>>> import re
# 编译:
>>> re_telephone = re.compile(r'^(\d{3})-(\d{3,8})$')	#分为区号-电话号
# 使用：
>>> re_telephone.match('010-12345').groups()
('010', '12345')
>>> re_telephone.match('010-8086').groups()
('010', '8086')
```

编译后生成Regular Expression对象，由于该对象自己包含了正则表达式，所以调用对应的方法时不用给出正则字符串。





对正则表达式进行编译，生成一个正则表达式对象

```python
re.compile(pattern[, flags])
	'''
	用于编译正则表达式，生成一个正则表达式（ Pattern ）对象，供 match() 和 search() 这两个函数使用。
	在多次批量调用的时候比较方便
	'''

text = "g123dwsdsam cool coolksf"
regex = re.compile(r'\w*oo\w*') 
temp = regex.findall(text)  # 查找所有包含'oo'的单词
print(temp)                #['cool', 'coolksf']
```



```python
### 示例
import re

pattern = re.compile('www')
res = pattern.match('www.coolpython.net')
if not res is None:
    print(res.span())  # 输出匹配的起始位置和结束位置
    
    
### 等价于
res = re.match('www', 'www.coolpython.net')
if not res is None:
    print(res.span())  # 输出匹配的起始位置和结束位置
    
    
'''源码等价'''
def match(pattern, string, flags=0):
    """Try to apply the pattern at the start of the string, returning
    a match object, or None if no match was found."""
    return _compile(pattern, flags).match(string)


def compile(pattern, flags=0):
    "Compile a regular expression pattern, returning a pattern object."
    return _compile(pattern, flags)
```

推荐：使用第一种写法，预先编译好正则表达式对象 => 方便修改





## 1 match

使用的比较多

从字符串起始位置开始匹配一个模式：

- 如果匹配成功，返回一个`Match`对象
- 如果模式不匹配，则返回None

```python
re.match(pattern, string, flags=0)
    flags：正则表达式修饰符 - 可选标志
           
def match(pattern, string, flags=0):
    """
    match(规则，字符串，标志位)
    flag(标志位)用于修改正则表达式的匹配方式，常见的工作方式如下：
    re.I    使匹配对大小写不敏感
    re.L    做本地化识别（local-aware）匹配
    re.M    多行匹配，影响^和$，不匹配换行符
    re.S    使.匹配包括换行在内的所有字符
    """
    
    """Try to apply the pattern at the start of the string, returning
    a match object, or None if no match was found."""
    return _compile(pattern, flags).match(string)
```

- 补充：flags

多个标志可以通过**按位 OR(|)** 它们来指定。如 re.I | re.M 被设置成 I 和 M 标志：

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1592746287758-3c0375ee-3137-4e13-abe8-02870d83df47.png)



| 符号汇总        |                                                              |
| --------------- | ------------------------------------------------------------ |
| g (global)      | 在第一个匹配项后不返回，而是从**上一个匹配项的结尾**重新开始后续搜索 |
| m (multi-line)  | 使用^和$会匹配一行的开始和结尾，而不是整个字符串             |
| i (insensitive) | 大小写不敏感/aBc/i  #也会匹配AbC                             |



### 相比于string.startswith()

使用match方法，可以匹配更为复杂的模式，这一点是字符串startswith方法所不能比拟的

- `()`表示分组，也叫子表达式



返回的结果如果不是None，则可以使用groups方法获得所匹配到的子表达式里的内容

- 可以使用group方法，指定组号
- 需注意的是，组号要从1开始，使用组号0得到的是所匹配的==整段字符串==

```python
import re

lst = [
    '小明的学号是3918324， 小班',
    '小红的学号是39384344， 中班',
    '小刚的生日是7月15日， 大班'
]

pattern = re.compile('(.+)的学号是(\d+)')
for item in lst:
    res = pattern.match(item)
    if res:
        print(res.groups())	#('小明', '3918324') ('小红', '39384344')
        print(res.group(1))	#小明  小红
        print(res.group(2))	#3918324  39384344
        print(res.group(0))	#结果：小明的学号是3918324 小红的学号是39384344
```







## 2 search

==非贪婪匹配==：扫描整个字符串，返回第一个成功的匹配，就不再往下匹配了。

- 即便有多个匹配，search方法也只会返回第一个
- 匹配的位置==可以是任何位置==，这一点与match不同

从前面可以看到**'\*','+'都是贪婪**的。但是一旦在后面加上?之后，就表示非贪婪匹配。

```python
# 扫描整个字符串并返回第一个成功的匹配
re.search(pattern, string, flags=0)

temp = re.search(r'a(\d+?)','a23b').group()   #a2
temp = re.search(r'a(\d+?)b','a23b').group()    #ab，当()在a、b中间的时候?的作用失效，整个匹配又成了贪婪模式
match()
```

匹配成功re.search方法**返回一个匹配的对象**，否则返回None。

我们可以使用group(num) 或 groups() 匹配对象的函数来获取匹配表达式。

​	.group()

​	.span(); .start(); .end()



```python
### 示例
import re

text = '我有3个电话号，分别是13343454523， 13341154523，13341152223'
pattern = re.compile('(\d{11})')
res = pattern.search(text)

print(res.span())
print(res.groups())


'''Output
(11, 22)
('13343454523',)
'''
```



### 调整：搜索范围

search方法，允许你设置搜索的范围，提供一个开始的位置和一个结束的位置，默认是从索引0开始搜索

想要获取全部的匹配，则需要使用一个循环，上一次匹配的结束位置作为下一次匹配的开始位置，这样，就能返回全部的匹配

```python
import re

text = '我有3个电话号，分别是13343454523， 13341154523，13341152223'
pattern = re.compile('(\d{11})')
res = pattern.search(text)

lst = []
while res:
    start, end = res.span()		#返回匹配字符串的start和end索引
    lst.append(res.group(1))	#匹配的第一个分组
    res = pattern.search(text, start+1)	#设置search开始的索引

print(lst)
'''Output
['13343454523', '13341154523', '13341152223']
'''
```





## 对比.match()和.search()

re.match()只匹配**字符串的开始**

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1592746475147-9aabf1d2-c34d-4746-97b9-f316c77eb0f8.png)



## 3 findall

在字符串中找到正则表达式所匹配的**所有子串**，并**返回一个列表**，如果没有找到匹配的，则返回空列表。

对string返回一个不重复的 pattern 的匹配列表， string 从左到右进行扫描，匹配按找到的顺序返回。

```python
re.findall(pattern, string, flags=0)  #findall(string[, pos[, endpos]])
```



### 1）findall的优先匹配原则

findall与search、match等不同的是，他会优先取组里的内容，可以使用?:来关闭

```python
temp = re.findall(r"www.(bing|google).com", "123 www.bing.com123www.google.com123")
print(temp)        #['bing', 'google']，只把组里的内容匹配到了

temp = re.findall(r"www.(?:bing|google).com", "123 www.bing.com123www.google.com123") 
print(temp)        #['www.bing.com', 'www.google.com'],#在(后面加上?:可以关闭findall的优先捕获功能
```



### 2）匹配顺序

一旦有字符被匹配到，就会**把匹配到的字符拿走**，然后再匹配剩下的字符，如下。

```python
n = re.findall("\d+\w\d+","a2b3c4d5")
print(n)    #['2b3', '4d5']，
```



### 3）匹配空值

当匹配规则为空时，如果没有匹配到也会把空值放到结果中：

```python
n = re.findall("","a2b3c4d5")
print(n)    #['', '', '', '', '', '', '', '', '']
```

因此在写python正则的时候，要尽量使正则不为空

```python
n = re.findall(r"(\w)+", "alex")    #推荐写法
print(n)  # ['x']

n = re.findall(r"(\w)*", "alex")    #不推荐写法
print(n)  # ['x','']
```



### 4）findall的分组匹配

```python
temp = re.findall('a(\d+)','a23b')    #['23']
temp = re.findall('a(\d+?)','a23b')    #['2']，非贪婪模式匹配
temp = re.findall(r'a(\d+?)b', 'a23b')  # ['23']，当()再a、b中间的时候?的作用失效，整个匹配又成了贪婪模式
match()
```

如果正则里有一个组，那么会把组里匹配到的元素加入到最终的列表里。

如果正则里有一个组，会把元素合并到一个元组里，然后作为列表的一个元素。

```python
n = re.findall(r"(\w)", "alex")
print(n)  # ['a', 'l', 'e', 'x']

n = re.findall(r"(\w)(\w)(\w)(\w)", "alex")
print(n)  # [('a', 'l', 'e', 'x')]，有几个括号就取几次

n = re.findall(r"(\w){4}", "alex")
print(n)  # ['x']，有几个括号就取几次

n = re.findall(r"(\w)*", "alex")
print(n)  # ['x','']，有几个括号就取几次
```



## 4 finditer

和 findall 类似，不同之处在于，finditer返回的是一个迭代器

```python

re.finditer(pattern, string, flags=0)


p = re.compile(r'\d+')
source = '12  dsnjfkbsfj1334jnkb3kj242'
w1 = p.finditer(source)
w2 = p.findall(source)
print(w1)       #<callable_iterator object at 0x102079e10>
print(w2)       #['12', '1334', '3', '242']

for match in w1:
    print(match.group(), match.span())
    """
    12 (0, 2)
    1334 (14, 18)
    3 (22, 23)
    242 (25, 28)
    """
    
p = re.compile(r'\d+')

iterate = p.finditer('')
for match in iterate:	#遍历迭代器
    match.group(), match.span()
```



## 5 分组匹配.group()

如果正则表达式中定义了组，就可以在`Match`对象上用`group()`方法提取出子串来

注意到`group(0)`永远是==原始字符串==，`group(1)`、`group(2)`……表示第1、2、……个子串。



返回匹配对象：比如第一个匹配对象

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1592746344684-b6ffed10-9e22-4532-8442-53f9e8305390.png)

```python
a = '123abc456'
temp1 = re.search("([0-9]*)([a-z]*)([0-9]*)", a).group(0)   #123abc456
temp2 = re.search("([0-9]*)([a-z]*)([0-9]*)", a).group(1)   #123
temp3 = re.search("([0-9]*)([a-z]*)([0-9]*)", a).group(2)   #abc
temp4 = re.search("([0-9]*)([a-z]*)([0-9]*)", a).group(3)   #456
temp5 = re.search("([0-9]*)([a-z]*)([0-9]*)", a).group(1,3)   #('123', '456')
temp6 = re.search("([0-9]*)([a-z]*)([0-9]*)", a).group(1,2,3)  #('123', 'abc', '456')

print(temp1)
print(temp2)
print(temp3)
print(temp4)
print(temp5)
print(temp6)
```





match、search、findall都有两个匹配方式：简单匹配和分组匹配

### 1）无分组

```python
import re

origin = "hello morra bcd morra lge morra acd 19"
r = re.match("h\w+", origin)

print(r.group())    #hello，获取匹配到的所有结果
print(r.groups())   #()，获取模型中匹配到的分组结果
print(r.groupdict())    #{}，获取模型中匹配到的分组中所有执行了key的组
```



### 2）有分组

```python
import re

origin = "hello morra bcd morra lge morra acd 19"
r = re.match("h(\w+)", origin)

print(r.group())    #hello，获取匹配到的所有结果
print(r.groups())   #('ello',)，获取模型中匹配到的分组结果
print(r.groupdict())    #{}，获取模型中匹配到的分组中所有执行了key的组
```



### 3）多个分组

```python
import re

origin = "hello morra bcd morra lge morra acd 19"
r = re.match("(?P<n1>h)(?P<n2>\w+)", origin)

print(r.group())    #hello，获取匹配到的所有结果
print(r.groups())   #('h', 'ello')，获取模型中匹配到的分组结果
print(r.groupdict())    #{'n2': 'ello', 'n1': 'h'}，获取模型中匹配到的分组中所有执行了key的组
```





## 6 分割 .split()

字符串提供的split方法可以根据分割符对字符串进行分割，但是该方法一次只能使用一个分隔符，特定场景下，我们需要根据多个分割符进行分割

```python
def split(pattern, string, maxsplit=0, flags=0):
	'''split 方法按照能够匹配的子串将字符串分割后**返回列表**
	- maxsplit=0，最多分割次数
	'''
    
    return _compile(pattern, flags).split(string, maxsplit)
```



### 背景

列表里的3个字符串，需要根据小时，分，秒进行分割，得到其中的数字部分，这个功能，使用**字符串的split方法就无法完成(只是单个分隔符)** => 对于这样的需求，你可以==使用正则表达式的split方法==

```python
import re

lst = [
    '1小时3分15秒',
    '4分39秒',
    '54秒'
]

pattern = re.compile('小时|分|秒')	#可以使用小时分割，使用分进行分割，使用秒进行分割 => 实现了多个分隔符的分割

for time_str in lst:
    res = pattern.split(time_str)
    print(res)
```



### 1）split优先匹配

由于split和findall一样结果返回的是所有元素的列表，因此他和findall一样具有优先匹配特性



### 2）有组与无组

```python
origin = "hello alex bcd abcd lge acd 19"
n = re.split("a\w+",origin,1)
print(n)    #['hello ', ' bcd abcd lge acd 19']，无组分割，结果不含自己

origin = "hello alex bcd abcd lge acd 19"
n = re.split("(a\w+)",origin,1)
print(n)    #['hello ', 'alex', ' bcd abcd lge acd 19']，有组分割，结果会包含自己

origin = "hello alex bcd abcd lge acd 19"
n = re.split("a(\w+)",origin,1)
print(n)    #['hello ', 'lex', ' bcd abcd lge acd 19']，有组分割，结果会包含自己
```



### 3）特殊情况分析

```python
temp = re.split('\d+','one1two2three3four4')
print(temp)     #['one', 'two', 'three', 'four', '']

temp = re.split('\d+','1one1two2three3four4')
print(temp)     #['', 'one', 'two', 'three', 'four', '']

temp = re.split('[bc]', 'abcd')
print(temp)  # ['a', '', 'd']   #思考题
```



## 7 替换 .sub()/substitution⭐

### 背景

电话号属于隐私信息，需要替换成***，来保证用户的隐私，如果使用字符串的replace方法，很难完成这个要求(需要写明指定替换的子串，而不是re中的模式匹配)

sub方法根据正则表达式进行替换

```python
import re

text = '我有3个电话号，分别是13343454523， 13341154523，13341152223'

pattern = re.compile('\d{11}')
text = pattern.sub("***", text)

print(text)
'''
我有3个电话号，分别是***， ***，***
'''
```



### 1）.sub()

```python
#使用 repl 替换在 string 最左边非重叠出现的 pattern 而获得的字符串
re.sub(pattern, repl, string, count=0, flags=0)
    count : 模式匹配后替换的最大次数，默认 0 表示替换所有的匹配。

# 示例：将g.t字符串(不同时态)统一替换为have
temp = re.sub('g.t', 'have', 'I get A,I got B,I gut C')  # I have A,I have B,I have C

temp = re.sub('g.t', 'have', 'I get A,I got B,I gut C', count=0)  # I have A,I have B,I have C，count默认为0，全部替换

temp = re.sub('g.t', 'have', 'I get A,I got B,I gut C', count=1)  # I have A,I got B,I gut C

temp = re.sub('g.t', 'have', 'I get A,I got B,I gut C', count=2)  # I have A,I have B,I gut C
temp = re.sub('g.t', 'have', 'I get A,I got B,I gut C', 2)  # I have A,I have B,I gut C

temp = re.sub('g.t', 'have', 'I get A,I got B,I gut C', count=3)  # I have A,I have B,I have C
temp = re.sub('g.t', 'have', 'I get A,I got B,I gut C', 3)  # I have A,I have B,I have C
```



### 2）.subn()

subn最后会统计被替换的次数:

```python
temp = re.subn('g.t', 'have', 'I get A,I got B,I gut C')  #('I have A,I have B,I have C', 3)，一共被替换了三次
```







# 应用

## 1 计算器

```python
import re

source = "1 - 2 * (( 60-30 + (-9-2-5-2*3-5/3-40*4/2-3/5+6*3)*(-9-2-5-2*5/3 +7/3*99/4*2998 + 10 * 568 /14)) -(-4*3)/(16-3*2))"
temp = re.search("\([^()]+\)",source).group()       #匹配括号里的内容
print(temp)


source2 = '2**3'        #幂运算
re.search('\d+\.?\d*([*/]|\*\*)\d+\.?\d*',source2)      # \d+\.?\d* 匹配一个数（整数或浮点数），[*/]|\*\*匹配乘除,或幂运算
print(source2)

print(2**3)
```



## 2 匹配IP

```
source = "13*192.168.1.112\12321334"
temp= re.search(r"(([01]?\d?\d|2[0-4]\d|25[0-5])\.){3}([01]?\d?\d|2[0-4]\d|25[0-5])",source).group()

print(temp)     #192.168.1.112
```



## url分析|基于Java

| 数据验证   | 例如，检查时间字符串是否格式正确                             |
| ---------- | ------------------------------------------------------------ |
| 数据抓取   | Web抓取，查找最终以特定顺序包含特定单词集的所有页面          |
| 数据转换   | 将数据从“原始”转换为另一种格式                               |
| 字符串解析 | 捕获所有URL GET参数，捕获一组括号内的文本                    |
| 字符串替换 | 将“;”替换为“，”使其变为小写，避免类型声明等                  |
| 其余       | 语法高亮显示，文件重命名，数据包嗅探以及许多其他涉及字符串的应用程序（其中数据不必是文本的） |

###    1）[捕获url](https://www.runoob.com/regexp/regexp-syntax.html)

#### 捕获

 ?: 不被缓存；用圆括号将所有选择项括起来，**相邻的选择项之间用|分隔**。但用圆括号会有一个副作用，使相关的匹配会被缓存，此时可用?:放在第一个选项前来消除这种副作用。



还有两个非捕获元是 ?= 和 ?! 前者为正向预查，在任何**开始匹配**圆括号内的正则表达式模式的位置来匹配搜索字符串，后者为负向预查，在任何开始**不匹配**该正则表达式模式的位置来匹配搜索字符串。

- ()表示捕获分组，缓存内容$1 \1提取
- (?: ) 表示不缓存/捕获
- (?= ) 正向预查，在任何**开始匹配**圆括号内的正则表达式模式的位置，来匹配搜索字符串
- (?! )  负向预查，在任何**开始不匹配**该正则表达式模式的位置，来匹配搜索字符串





#### 反向引用

对一个正则表达式模式或部分模式两边添加圆括号将导致相关匹配存储到一个临时缓冲区中，所捕获的每个子匹配都按照在正则表达式模式中从左到右出现的顺序存储。

**缓冲区编号**从 1 开始，最多可存储 99 个捕获的子表达式。每个缓冲区都可以使用 **\n** 访问，其中 n 为一个标识特定缓冲区的一位或两位十进制数。

反向引用的最简单的、最有用的应用之一，是提供查找文本中**两个相同的相邻单词**的匹配项的能力。



例子：句子中有多个重复的单词

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1592746929930-de95f219-8ef6-4075-b480-3155e9c1d881.png)

 [a-z]+ 查找一个或多个字母

​	\1 指定第一个子匹配项

​	正则表达式后面的全局标记 g 指定将该表达式应用到输入字符串中能够查找到的尽可能多的匹配。

表达式的结尾处的不区分大小写 i 标记指定不区分大小写





反向引用还可以将通用资源指示符 (URI) 分解为其组件。假定您想将下面的 URI 分解为协议（ftp、http 等等）、域地址和页/路径：

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1592746991972-66fde60c-3178-48c4-b603-3196bbcc82b9.png)

str.match(patt1) 返回一个数组，实例中的数组包含 5 个元素，索引 0 对应的是整个字符串，索引 1 对应第一个匹配符（括号内），以此类推。

​	第一个括号子表达式捕获 Web 地址的协议部分  (\w+)

 该子表达式匹配在冒号和两个正斜杠前面的任何单词  :\/\/

​	第二个括号子表达式捕获地址的域地址部分  ([^/:]+)

 子表达式匹配非 : 和 / 之后的一个或多个字符

​	第三个括号子表达式捕获端口号（如果指定了的话）  (:\d*)

 该子表达式匹配冒号后面的零个或多个数字。只能重复一次该子表达式。  

​	第四个括号子表达式捕获 Web 地址指定的路径和 / 或页信息  ([^# ]*)/



**提取结果**：

​	第一个括号子表达式包含 http

​	第二个括号子表达式包含 www.runoob.com

​	第三个括号子表达式包含 :80

​	第四个括号子表达式包含 /html/html-tutorial.html

 

**注意**：

1 . 特殊字符在中括号表达式时 如 [.] 只会匹配 .字符，等价于 \.





# #补充及示例

## 1 建议：在pattern前加上**r**

为了防止python语法和正则语法冲突，在正则匹配规则前面加前面建议加r => 不用再考虑添加转义字符

```python
### \b表示边界
temp = re.findall(r"abc\b", "asdas abc 1231231")
print(temp)     #['abc']

# 两者一致：但没有r''时，要兼顾python的语法，\\b看起来不直观
temp = re.findall("abc\\b", "asdas abc 1231231")
print(temp)     #['abc']
```



## 附录|常用

`re.findall('\d+', str)`  寻找str中所有的数字及其1~n扩展

![image](https://cdn.nlark.com/yuque/0/2020/png/1136179/1591176765875-045d9511-b3fb-4820-a8e8-157310853685.png)







## 附录|拓展例子

```python
var patt1 = /\b([a-z]+) \1\b/ig;
'''
/   		/无具体意义
\b   		\b表示边界
ig标志   		i忽略大小写，g全局匹配
([a-z]+)   	\1 捕获单词([a-z]+) 空格 重复该单词\1
'''


var patt1 = /(\w+):\/\/([^/:]+)(:\d*)?([^# ]*)/;
'''
(\w+)			(多个)数字/字母
([^/:]+)   		除了/和:之外的所有数字/字母(+表示贪婪)
(:\d*)?  		端口 :数字 (*表示贪婪匹配数字   ?表示匹配非贪婪)  
([^# ]*)  		不包括#和空格之外的所有字符
'''
                                        
                                        
                                        
url_regex = 'http[s]?://(?:[a-zA-Z]|[0-9]|[$-_@.&+]|[!*\(\),]|(?:%[0-9a-fA-F][0-9a-fA-F]))+'
'''
http[s]?://   				可能含有s
(?:  |  |  |  |)+  			捕获逻辑为或，+表示贪婪(多个)

[a-zA-Z]
[0-9]
[$-_@.&+]  					表示$到_，包含/

[!*\(\),]
(?:%[0-9a-fA-F][0-9a-fA-F])  %分号——%(0x)(0x) 0x表示十六进制数

[0-9a-fA-F]
[0-9a-fA-F]
'''
```



- 应用：[URL识别](http://t.co/Ge9Lp7hpyG)
- 补充：[URL编码的字符](https://stackoverflow.com/questions/40797247/why-special-character-in-url-encoding-are-special)

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1592747454100-385655e9-4b0c-47f4-b733-1406ceb54159.png)



## 示例

### 匹配中文

```python
"匹配一段中文，this is a book ,非中文部分不要" => 匹配中文部分

[\u4e00-\u9fa5]+	#这种写法是unicode，在\u4e00 与 \u9fa5 之间，都是中文字符
'''
匹配一段中文
非中文部分不要
'''
```



### 匹配邮箱

邮箱分为两部分，A@B

- A部分是邮箱用户名部分，可以由大小写字母，数字，下划线，中划线构成

- B是域名部分，域名通常都是xxx.xxx的形式

```python
^[a-zA-Z0-9_-.]+@[a-z0-9]+.com$

'''验证邮箱
1. pythonlinks@163.com
2. 3984245@qq.com
3. python-001@gmail.com
4. bill.gates@microsoft.com
'''
```



- 从邮箱中提取姓名

```python
'''目标
<Tom Paris> tom@voyager.org => Tom Paris
bob@example.com => bob
'''

# -*- coding: utf-8 -*-
import re
def name_of_email(addr):
    pattern = re.compile(r'^<?([\s\w]+)>?[\s\w]*@[a-z0-9]+.*$')	
    res = pattern.match(addr)
    
    if res == None:
        return None
	return res.group(1)

name_of_email('<Tom Paris> tom@voyager.org')	#'Tom Paris'


# 测试:
assert name_of_email('<Tom Paris> tom@voyager.org') == 'Tom Paris'
assert name_of_email('tom@voyager.org') == 'tom'
print('ok')
```

注意：

- `<? >?`表示是否有尖括号
- `()`内如果是`.*` => 结果是`'Tom Paris> tom'` => 相当于贪婪匹配：`.*`也包括`>`，且一直延续到`@`符号
- 改成`[\s\w]+`就不会包括`>`，那么按照`>?`的匹配得到目标结果





### 匹配身份证号

以二代身份证为例，身份证号码是18位，最后一位可以是数字，也可以是大写的X(实际使用中为了防止大小写问题，还会加上x是小写的情况)

```python
^\d{17}(X|x)$

### 优化
^(\d{6})(\d{4})(\d{2})(\d{2})(\d{3})([0-9]|X|x)$


'''考虑到了身份证的编码规则：可以按照身份证的编码规则对身份证进行分割
前6位是地址码， 登记户口时所在地的行政区划代码
7到14位是出生年月日
15到17位是顺序码，给同年同月同日出生的人编制的顺序码，第17位奇数表示男性，偶数表示女性
第18位是校验码
'''

import re

content = '221282198608228771'
pattern = re.compile('^(\d{6})(\d{4})(\d{2})(\d{2})(\d{3})([0-9]|X|x)$')

res = pattern.findall(content)
print(res)	#[('221282', '1986', '08', '22', '877', '1')]
```





### 匹配URL

:// 之前是协议说明部分，http协议是最常见的协议，此外还有ftp，thunder 协议，比如迅雷经常使用的资源地址都是以 thunder:// 开头的

://后面的部分的构成则比较复杂，除了空格之外，其他的字符都可以

```text
https://www.baidu.com/
http://docs.jinkan.org/docs/flask/quickstart.html#
ftp://foolish.6600.org
```

```python
[a-zA-z]+://[^\s]*	#[^\s]非空格
```



### QQ号码

qq号码的规则：最长可以是11位，最短是5位，首位不能是0

```python
[1-9][0-9]{4,10}
```







# #参考文献

[cnblogs | Python标准模块re](https://www.cnblogs.com/whatisfantasy/p/6014523.html)

[维基百科](https://en.wikipedia.org/wiki/Regular_expression)

[速查表格](https://medium.com/factory-mind/regex-tutorial-a-simple-cheatsheet-by-examples-649dc1c3f285)

[在线测试效果](https://regex101.com/r/cO8lqs/11)

[Turorial-中文](https://juejin.im/post/5b5db5b8e51d4519155720d2)

[Tutorial-Python官网](https://docs.python.org/zh-cn/3/library/re.html)

[Tutorial-菜鸟教程](https://www.runoob.com/python/python-reg-expressions.html)

[参考文档](https://ahkcn.github.io/docs/misc/RegEx-QuickRef.htm)