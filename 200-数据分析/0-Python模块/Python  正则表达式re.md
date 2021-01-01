# Python | 正则表达式re

**声明**：以下内容主要来自[cnblogs | Python标准模块re](https://www.cnblogs.com/whatisfantasy/p/6014523.html)，根据理解做了些许调整。



## 一 元字符

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1592747112651-23fe69e3-f8dd-4f6a-81f7-bf9d2302b24d.png)

### 1 . |除了换行符以外的任何单个字符



### 2 ^ |只匹配起始字符

```
temp1=re.findall('^morra','nsudi werwuirnmorra')
temp2=re.findall('^morra','morransudi werwuirn')

print(temp1)    #[]
print(temp2)    #['morra']
```

### 3 $ |只匹配结尾字符

```
temp1=re.findall('morra$','nsudi werwuirnmorra')
temp2=re.findall('morra$','morransudi werwuirn')

print(temp1)    #['morra']
print(temp2)    #[]

^roar$    #准确锁定字符串roar
```

### 4 * |匹配0到多次，等同于{0,}

```
temp1=re.findall('morra*','wwwmorr')
temp2=re.findall('morra*','wwwmorra')
temp3=re.findall('morra*','wwwmorraa')     #贪婪匹配, morr后面0个或多个字符a

print(temp1)    #['morr']
print(temp2)    #['morra']
print(temp3)    #['morraa']
```

### 5 + |匹配1到多次，等同于{1,}

```
temp1=re.findall('morra+','wwwmorr')
temp2=re.findall('morra+','wwwmorra')
temp3=re.findall('morra+','wwwmorraa')  #贪婪匹配, morr后面1个或多个字符a

print(temp1)    #[]
print(temp2)    #['morra']
print(temp3)    #['morraa']
```

### 6 ? |匹配0到1次，{0,1}

```
temp1=re.findall('morra?','wwwmorr')
temp2=re.findall('morra?','wwwmorra')
temp3=re.findall('morra?','wwwmorraa')  #贪婪匹配, morr后面0个或1字符a

print(temp1)    #['morr']
print(temp2)    #['morra']
print(temp3)    #['morra']
```

### 7 {} |自定义匹配次数：{1}匹配1次，{1,2}匹配1到2次

```
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

### 8 [ ] |字符集，只匹配单个字符

```
temp2=re.findall('morr[ab]','wwwmorra')
temp3=re.findall('morr[ab]','wwwmorrab')

print(temp2)    #['morra']
print(temp3)    #['morra']

[aeiou]    #匹配一个元音
[a-z0-9]    #匹配一个小写字母和数字
[a-fA-F0-9]    #匹配一个十六进制字符
[0-9]%    #在%符号前是字符0-9
```

注意：**元字符**在字符集里就不再具有特殊意义，变成了普通字了，但是\d \w \s除外



### 9 ^ |反向匹配(在[]字符集中)

```
temp1=re.findall('[^1-9]','www.m12orr1234') #匹配除数字以外的所有字符
print(temp1) #['w', 'w', 'w', '.', 'm', 'o', 'r', 'r']

[^a-zA-Z]    #非字母
```

### 10 () |分组与捕获

()表示捕获分组，()会把每个分组里的匹配的值保存起来，使用**$n**(n是一个数字，表示第n个捕获组的内容)

(?:)表示**非捕获**分组，和捕获分组唯一的区别在于，非捕获分组匹配的值**不会保存起来**

```
a(b|c)    #a后可能有b或c
a(bc)    #a后是bc
a(bc)*     #a后有0个或多个bc序列
a(?:bc)*    #?:表示取消捕捉bc
a(?<foo>bc)     #?<foo> 表示给组(bc)一个名称叫做foo


# 选择分组
$1  
\1
```

### 11 \ |转义字符

(反斜杠/ 没有任何意义)

适用对象：^ . [ $ ( ) | * + ? { \

不可打印的字符：

- \t  tab
- \n  换行
- \r  回车

#### 1）后面跟元字符就是去除其特殊功能

```
temp1 = re.findall('\$', 'www.m12$orr1234')
print(temp1)    #['$']
```

#### 2）后面跟普通字符可以实现一些特殊功能

\d 匹配任何单个十进制数digit，等同于[0-9],如果想匹配多个可以使用\d\d,\d+,\d{2}

```
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

\D 匹配任何**非****数字**字符，等同于[^0-9]

\s 匹配任何空白字符(Tab、空格)，等同于[ \t\n\r\f\v]

\S 匹配任何**非****空白**字符，等同于[^ \t\n\r\f\v]

\w 匹配任何字母数字字符(字母、数字、下划线)，等同于[A-Za-z0-9]

\W 匹配任何**非****字母数字**字符，等同于[^A-Za-z0-9]

\b 匹配一个单词边界，也就是指单词和空格间的位置

```
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

#### 3）编号：引用序号对应的字组所匹配的字符串

```
temp = re.search(r'(alex)(eric)com\1', 'alexericcomalex').group()   #alexericcomalex，\1就是第一组等同于alex
temp2 = re.search(r'(alex)(eric)com\2', 'alexericcomeric').group()  #alexericcomeric，\2就是第二组等同于eric
```



### 12 贪婪/懒惰搜索

贪婪：匹配所有符合项

非贪婪：匹配到一个即可

```
<.+>    #.+表示任意长度的字符串
<.+?>     #只匹配div标签 ?表示非贪婪匹配
<[^<>]+> 
```



## 二 标志符号

| g (global)      | 在第一个匹配项后不返回，而是从**上一个匹配项的结尾**重新开始后续搜索 |
| --------------- | ------------------------------------------------------------ |
| m (multi-line)  | 使用^和$会匹配一行的开始和结尾，而不是整个字符串             |
| i (insensitive) | 大小写不敏感/aBc/i  #也会匹配AbC                             |





## 三 正则表达式的函数

### 0 正则表达式对象

| re.RegexObject | re.compile() 返回 RegexObject 对象                           |
| -------------- | ------------------------------------------------------------ |
| re.MatchObject | group() 返回被 RE 匹配的字符串。  start() 返回匹配开始的位置  end() 返回匹配结束的位置  span() 返回一个**元组包含匹配** (开始,结束) 的位置 |



返回一个对象：match、search、finditer

返回一个列表：findall、split

### 1 match

从头匹配，使用的比较多

```
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

### 2 search

非贪婪匹配：匹配到一个后，就不再往下匹配了。

从前面可以看到**'\*','+'都是贪婪**的。但是一旦在后面加上?之后，就表示非贪婪匹配。

```
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



- 对比.match()和.search()

re.match()只匹配**字符串的开始**

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1592746475147-9aabf1d2-c34d-4746-97b9-f316c77eb0f8.png)



### 3 findall

findall(string[, pos[, endpos]])

在字符串中找到正则表达式所匹配的**所有子串**，并**返回一个列表**，如果没有找到匹配的，则返回空列表。



```
# 对string返回一个不重复的 pattern 的匹配列表， string 从左到右进行扫描，匹配按找到的顺序返回。
re.findall(pattern, string, flags=0)
```

#### 1）findall的优先匹配原则

findall与search、match等不同的是，他会优先取组里的内容，可以使用?:来关闭

```
temp = re.findall(r"www.(bing|google).com", "123 www.bing.com123www.google.com123")
print(temp)        #['bing', 'google']，只把组里的内容匹配到了

temp = re.findall(r"www.(?:bing|google).com", "123 www.bing.com123www.google.com123") 
print(temp)        #['www.bing.com', 'www.google.com'],#在(后面加上?:可以关闭findall的优先捕获功能
```

#### 2）匹配顺序

一旦有字符被匹配到，就会**把匹配到的字符拿走**，然后再匹配剩下的字符，如下。

```
n = re.findall("\d+\w\d+","a2b3c4d5")
print(n)    #['2b3', '4d5']，
```

#### 3）匹配空值

当匹配规则为空时，如果没有匹配到也会把空值放到结果中：

```
n = re.findall("","a2b3c4d5")
print(n)    #['', '', '', '', '', '', '', '', '']
```

因此在写python正则的时候，要尽量使正则不为空

```
n = re.findall(r"(\w)+", "alex")    #推荐写法
print(n)  # ['x']

n = re.findall(r"(\w)*", "alex")    #不推荐写法
print(n)  # ['x','']
```

#### 4）findall的分组匹配

```
temp = re.findall('a(\d+)','a23b')    #['23']
temp = re.findall('a(\d+?)','a23b')    #['2']，非贪婪模式匹配
temp = re.findall(r'a(\d+?)b', 'a23b')  # ['23']，当()再a、b中间的时候?的作用失效，整个匹配又成了贪婪模式
match()
```

如果正则里有一个组，那么会把组里匹配到的元素加入到最终的列表里。

如果正则里有一个组，会把元素合并到一个元组里，然后作为列表的一个元素。

```
n = re.findall(r"(\w)", "alex")
print(n)  # ['a', 'l', 'e', 'x']

n = re.findall(r"(\w)(\w)(\w)(\w)", "alex")
print(n)  # [('a', 'l', 'e', 'x')]，有几个括号就取几次

n = re.findall(r"(\w){4}", "alex")
print(n)  # ['x']，有几个括号就取几次

n = re.findall(r"(\w)*", "alex")
print(n)  # ['x','']，有几个括号就取几次
```



### 4 finditer

re.finditer(pattern, string, flags=0)

和 findall 类似，在字符串中找到正则表达式所匹配的**所有子串**，并把它们作为一个**迭代器**返回。



```
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
for match in iterate:
    match.group(), match.span()
```



### 5 分组匹配

返回匹配对象：比如第一个匹配对象

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1592746344684-b6ffed10-9e22-4532-8442-53f9e8305390.png)

```
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

#### 1）无分组

```
import re

origin = "hello morra bcd morra lge morra acd 19"
r = re.match("h\w+", origin)

print(r.group())    #hello，获取匹配到的所有结果
print(r.groups())   #()，获取模型中匹配到的分组结果
print(r.groupdict())    #{}，获取模型中匹配到的分组中所有执行了key的组
```

#### 2）有分组

```
import re

origin = "hello morra bcd morra lge morra acd 19"
r = re.match("h(\w+)", origin)

print(r.group())    #hello，获取匹配到的所有结果
print(r.groups())   #('ello',)，获取模型中匹配到的分组结果
print(r.groupdict())    #{}，获取模型中匹配到的分组中所有执行了key的组
```

#### 3）多个分组

```
import re

origin = "hello morra bcd morra lge morra acd 19"
r = re.match("(?P<n1>h)(?P<n2>\w+)", origin)

print(r.group())    #hello，获取匹配到的所有结果
print(r.groups())   #('h', 'ello')，获取模型中匹配到的分组结果
print(r.groupdict())    #{'n2': 'ello', 'n1': 'h'}，获取模型中匹配到的分组中所有执行了key的组
```



### 6 分割

re.split(pattern, string[, maxsplit=0, flags=0])

split 方法按照能够匹配的子串将字符串分割后**返回列表**

```
def split(pattern, string, maxsplit=0, flags=0):

    return _compile(pattern, flags).split(string, maxsplit)
#maxsplit=0，最多分割次数
```

#### 1）split优先匹配

由于split和findall一样结果返回的是所有元素的列表，因此他和findall一样具有优先匹配特性

#### 2）有组与无组

```
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

#### 3）特殊情况分析

```
temp = re.split('\d+','one1two2three3four4')
print(temp)     #['one', 'two', 'three', 'four', '']

temp = re.split('\d+','1one1two2three3four4')
print(temp)     #['', 'one', 'two', 'three', 'four', '']

temp = re.split('[bc]', 'abcd')
print(temp)  # ['a', '', 'd']   #思考题
```



### 7 替换

#### 1）sub

```
#使用 repl 替换在 string 最左边非重叠出现的 pattern 而获得的字符串
re.sub(pattern, repl, string, count=0, flags=0)
    count : 模式匹配后替换的最大次数，默认 0 表示替换所有的匹配。

temp = re.sub('g.t', 'have', 'I get A,I got B,I gut C')  # I have A,I have B,I have C

temp = re.sub('g.t', 'have', 'I get A,I got B,I gut C', count=0)  # I have A,I have B,I have C，count默认为0，全部替换

temp = re.sub('g.t', 'have', 'I get A,I got B,I gut C', count=1)  # I have A,I got B,I gut C

temp = re.sub('g.t', 'have', 'I get A,I got B,I gut C', count=2)  # I have A,I have B,I gut C
temp = re.sub('g.t', 'have', 'I get A,I got B,I gut C', 2)  # I have A,I have B,I gut C

temp = re.sub('g.t', 'have', 'I get A,I got B,I gut C', count=3)  # I have A,I have B,I have C
temp = re.sub('g.t', 'have', 'I get A,I got B,I gut C', 3)  # I have A,I have B,I have C
```

#### 2）subn

subn最后会统计被替换的次数:

```
temp = re.subn('g.t', 'have', 'I get A,I got B,I gut C')  #('I have A,I have B,I have C', 3)，一共被替换了三次
```



### 8 编译 re.compile()

re.compile(pattern[, flags])

用于编译正则表达式，生成一个正则表达式（ Pattern ）对象，供 match() 和 search() 这两个函数使用。

在多次批量调用的时候比较方便

```
text = "g123dwsdsam cool coolksf"
regex = re.compile(r'\w*oo\w*') 
temp = regex.findall(text)  # 查找所有包含'oo'的单词
print(temp)                #['cool', 'coolksf']
```





## 四 应用

### 1 计算器

```
import re

source = "1 - 2 * (( 60-30 + (-9-2-5-2*3-5/3-40*4/2-3/5+6*3)*(-9-2-5-2*5/3 +7/3*99/4*2998 + 10 * 568 /14)) -(-4*3)/(16-3*2))"
temp = re.search("\([^()]+\)",source).group()       #匹配括号里的内容
print(temp)


source2 = '2**3'        #幂运算
re.search('\d+\.?\d*([*/]|\*\*)\d+\.?\d*',source2)      # \d+\.?\d* 匹配一个数（整数或浮点数），[*/]|\*\*匹配乘除,或幂运算
print(source2)

print(2**3)
```



### 2 匹配IP

```
source = "13*192.168.1.112\12321334"
temp= re.search(r"(([01]?\d?\d|2[0-4]\d|25[0-5])\.){3}([01]?\d?\d|2[0-4]\d|25[0-5])",source).group()

print(temp)     #192.168.1.112
```



### 3 补充：基于Java

| 数据验证   | 例如，检查时间字符串是否格式正确                             |
| ---------- | ------------------------------------------------------------ |
| 数据抓取   | Web抓取，查找最终以特定顺序包含特定单词集的所有页面          |
| 数据转换   | 将数据从“原始”转换为另一种格式                               |
| 字符串解析 | 捕获所有URL GET参数，捕获一组括号内的文本                    |
| 字符串替换 | 将“;”替换为“，”使其变为小写，避免类型声明等                  |
| 其余       | 语法高亮显示，文件重命名，数据包嗅探以及许多其他涉及字符串的应用程序（其中数据不必是文本的） |

####    1）[捕获url](https://www.runoob.com/regexp/regexp-syntax.html)

- 捕获

 ?: 不被缓存；用圆括号将所有选择项括起来，**相邻的选择项之间用|分隔**。但用圆括号会有一个副作用，使相关的匹配会被缓存，此时可用?:放在第一个选项前来消除这种副作用。



还有两个非捕获元是 ?= 和 ?! 前者为正向预查，在任何**开始匹配**圆括号内的正则表达式模式的位置来匹配搜索字符串，后者为负向预查，在任何开始**不匹配**该正则表达式模式的位置来匹配搜索字符串。

- - ()表示捕获分组，缓存内容$1 \1提取
  - (?: ) 表示不缓存/捕获
  - (?= ) 正向预查，在任何**开始匹配**圆括号内的正则表达式模式的位置，来匹配搜索字符串
  - (?! )  负向预查，在任何**开始不匹配**该正则表达式模式的位置，来匹配搜索字符串



- 反向引用

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

2 ^ 和 [^指定字符串] 之间的区别:

​	^ 指的是匹配字符串开始的位置

​	[^指定字符串] 指的是除指定字符串以外的其他字符串





## 五 补充

### 1 建议：在pattern前加上**r**

为了防止python语法和正则语法冲突，在正则匹配规则前面加前面建议加r

```
temp = re.findall(r"abc\b", "asdas abc 1231231")
print(temp)     #['abc']

temp = re.findall("abc\\b", "asdas abc 1231231")
print(temp)     #['abc']
```



### 2 拓展例子

var patt1 = /\b([a-z]+) \1\b/ig;

 / /无具体意义

​	\b \b表示边界

​	ig标志：i忽略大小写，g全局匹配

​	([a-z]+) \1 捕获单词([a-z]+) 空格 重复该单词\1





var patt1 = /(\w+):\/\/([^/:]+)(:\d*)?([^# ]*)/;

 (\w+)    (多个)数字/字母

​	([^/:]+)   除了/和:之外的所有数字/字母(+表示贪婪)

​	(:\d*)?   端口 :数字 (*表示贪婪匹配数字   ?表示匹配非贪婪)  

([^# ]*)  不包括#和空格之外的所有字符



url_regex = 'http[s]?://(?:[a-zA-Z]|[0-9]|[$-_@.&+]|[!*\(\),]|(?:%[0-9a-fA-F][0-9a-fA-F]))+'

http[s]?://   可能含有s

(?:  |  |  |  |)+  捕获逻辑为或，+表示贪婪(多个)

[a-zA-Z]

[0-9]

[$-_@.&+]  表示$到_，包含/

[!*\(\),]

(?:%[0-9a-fA-F][0-9a-fA-F])  %分号——%(0x)(0x) 0x表示十六进制数

[0-9a-fA-F]

[0-9a-fA-F]



- 应用：[URL识别](http://t.co/Ge9Lp7hpyG)
- 补充：[URL编码的字符](https://stackoverflow.com/questions/40797247/why-special-character-in-url-encoding-are-special)

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1592747454100-385655e9-4b0c-47f4-b733-1406ceb54159.png)

## 参考文献

[cnblogs | Python标准模块re](https://www.cnblogs.com/whatisfantasy/p/6014523.html)

[维基百科](https://en.wikipedia.org/wiki/Regular_expression)

[速查表格](https://medium.com/factory-mind/regex-tutorial-a-simple-cheatsheet-by-examples-649dc1c3f285)

[在线测试效果](https://regex101.com/r/cO8lqs/11)

[Turorial-中文](https://juejin.im/post/5b5db5b8e51d4519155720d2)

[Tutorial-Python官网](https://docs.python.org/zh-cn/3/library/re.html)

[Tutorial-菜鸟教程](https://www.runoob.com/python/python-reg-expressions.html)

[参考文档](https://ahkcn.github.io/docs/misc/RegEx-QuickRef.htm)