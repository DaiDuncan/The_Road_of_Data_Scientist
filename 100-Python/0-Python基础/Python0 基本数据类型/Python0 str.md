```python
# -*- coding: utf-8 -*-
```

![image](https://cdn.nlark.com/yuque/0/2020/png/1136179/1591176765778-62260ef9-a1e4-4dba-870b-3d59a63619df.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_14%2Ctext_6K-t6ZuALUREdW5jYW4%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)



# 操作1|创建

可以使用单引号、双引号、三单引号和三双引号

- 三引号可以定义**多行**字符串



[注意](https://zhuanlan.zhihu.com/p/28879694)：当我们字符串中包含单引号（'）的双引号创建字符串；

- 字符串包含双引号（"）时，使用单引号创建。

- 三个引号（'''）里面能出现单引号，双引号，回车键等，三个引号中的字符串会保持传入的格式

```python
s = "morra"
s = str("morra")        #str()这种方法会自动找到str类里的_init_方法去执行

----------------------------------------------------
def __init__(self, value='', encoding=None, errors='strict'): 
    """
    str(object='') -> str
    str(bytes_or_buffer[, encoding[, errors]]) -> str
    
    Create a new string object from the given object. If encoding or
    errors is specified, then the object must expose a data buffer
    that will be decoded using the given encoding and error handler.
    Otherwise, returns the result of object.__str__() (if defined)
    or repr(object).
    encoding defaults to sys.getdefaultencoding().
    errors defaults to 'strict'.
    # (copied from class doc)
    """
    pass

----------------------------------------------------
s = str()
s = str("morra")
s = str("morra",encoding='utf-8')
```



# 操作2|查 选/索引(含切片) 改 增 删

**注意**：所有的字符串函数都是有返回值的，默认inplace = False

## 查

```python
find(sub [,start [,end]])  #返回索引值，没有返回值为-1
rfind()  #从右向左查同find

index(sub [,start [,end]])  #查不到会报错
rindex()  #从右向左返回索引值同index


in/not in
```



## 选/索引(含切片)

```python
### 索引：[) 左闭右开区间
s="hello"
print(s[0])  #h
print(s[1])  #e
print(s[2])  #l
print(s[3])  #l
print(s[4])  #o
print(s[5])  #报错

# 切片
str[start: end: step]  #切片
str[::2] 隔两个截取  
str[::-1]倒序

s="hello"
print(s[0:2])  #0<=X<2，输出“he”

```



### #练习题

```python
### 用切片实现.strip()
def trim(s):
    i, j = 0, 0
    for chr in s:
        if chr != ' ':
            break
        i += 1
    for chr in s[::-1]:
        if chr != ' ':
            break
        j += 1

    return s[i:len(s)-j]	#当末尾没有空格，j=0，但是len(s)比最后的index大1
    
    
### 测试：
if trim('hello  ') != 'hello':
    print('测试失败!')
elif trim('  hello') != 'hello':
    print('测试失败!')
elif trim('  hello  ') != 'hello':
    print('测试失败!')
elif trim('  hello  world  ') != 'hello  world':
    print('测试失败!')
elif trim('') != '':
    print('测试失败!')
elif trim('    ') != '':
    print('测试失败!')
else:
    print('测试成功!')
```





## 改|replace

```python
replace(old, new[, count])  #指定字符替换为新的字符，可指定个数

expandtabs([n])  #将字符串中(tab符号)\t转换成n个空格

format_map()  #格式化输出，以字典形式存储key-value数据

maketrans() + translate(table [,deletechars])  #根据参数table=maketrans给出的表(包含 256 个字符)转换字符串的字符, 要过滤掉的字符放到deletechars参数中
```





## 增|.join()

```python
.join([str1, str2])
```





## 删

- `strip()`  移除字符串头尾指定的字符（默认为空格或换行符）或字符序列(**多个字符**亦可)
- `lstrip()`  移除字符串头
- `rstrip`  移除字符串尾

```python
### str.strip() 
"···xyz···".strip()            # returns "xyz"  
"···xyz···".lstrip()           # returns "xyz···"  
"···xyz···".rstrip()           # returns "···xyz"  
"··x·y·z··".replace(' ', '')   # returns "xyz" 


# 注意：删除的字符序列 不论顺序
str = "123abcrunoob321"
print (str.strip( '12' ))   #returns 3abcrunoob3

str = "123abcrunoob3221"
print (str.strip( '32b1' )) #returnsabcrunoo

'''说明：
看起来像是参数中每个字符 3 2 b 1 都是独立的，
在开头从左向右依次查找字符，如果是这几个中的一个就删除，继续往下找，若不是，就终止，所以开头保留到a 。
在结尾从右往左查找，同上，而且结尾的 22 可以重复删除。
'''
```







# 操作3|遍历 排序 统计

```python
### 统计
# 长度
len(s)
.count(sub[, start[, end]])  #区间内某个字符的个数
```



# 操作|小结：字符串运算⭐

| 操作符 | 描述                                               |
| :----- | :------------------------------------------------- |
| +      | 字符串连接                                         |
| *      | 重复字符串                                         |
| []     | 通过索引访问指定索引的字符                         |
| [ : ]  | 切片操作，截取字符串指定范围                       |
| in     | 成员运算符 - 如果字符串中包含给定的字符返回 True   |
| not in | 成员运算符 - 如果字符串中不包含给定的字符返回 True |
| %      | 格式字符串                                         |







# 操作|补充：常用内置函数⭐

## 转换类

| 编号 | 方法名称                                                     | 功能描述                                                     |
| :--- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| 1    | [capitalize()](http://www.coolpython.net/method_topic/str/capitalize.html) | 将字符串的第一个字符转换为大写                               |
| 2    | [center](http://www.coolpython.net/method_topic/str/center.html) | 返回一个指定的宽度 width 居中的字符串，fillchar 为填充的字符，默认为空格 |
| 3    | [encode](http://www.coolpython.net/method_topic/str/encode.html) | 以 encoding 指定的编码格式编码字符串                         |
| 4    | [join(seq)](http://www.coolpython.net/method_topic/str/join.html) | 以指定字符串作为分隔符，将 seq 中所有的元素(的字符串表示)合并为一个新的字符串 |
| 5    | [len(string)](http://www.coolpython.net/method_topic/str/len.html) | 返回字符串长度                                               |
| 6    | [ljust(width[, fillchar\])](http://www.coolpython.net/method_topic/str/ljust.html) | 返回一个原字符串左对齐,并使用 fillchar 填充至长度 width 的新字符串，fillchar 默认为空格 |
| 7    | [rjust(width[, fillchar\])](http://www.coolpython.net/method_topic/str/rjust.html) | 返回一个原字符串右对齐,并使用fillchar(默认空格）填充至长度 width 的新字符串 |
| 8    | [lower()](http://www.coolpython.net/method_topic/str/lower.html) | 转换字符串中所有大写字符为小写                               |
| 9    | [upper()](http://www.coolpython.net/method_topic/str/upper.html) | 转换字符串中的小写字母为大写                                 |
| 10   | [lstrip()](http://www.coolpython.net/method_topic/str/lstrip.html) | 截掉字符串左边的空格或指定字符                               |
| 11   | [rstrip()](http://www.coolpython.net/method_topic/str/rstrip.html) | 删除字符串字符串末尾的空格                                   |
| 12   | [split(sep=None, maxsplit=-1)](http://www.coolpython.net/method_topic/str/split.html) | 以 sep为分隔符截取字符串，如果 maxsplit 有指定值，则仅截取 maxsplit+1 个子字符串 |
|      | [strip([chars\])](http://www.coolpython.net/method_topic/str/strip.html) | 在字符串上执行 lstrip()和 rstrip()                           |
| 13   | [replace(old, new[, count\])](http://www.coolpython.net/method_topic/str/replace.html) | 将字符串中的 old 替换成 new,如果 max 指定，则替换不超过 count 次 |
| 14   | [splitlines([keepends\])](http://www.coolpython.net/method_topic/str/splitlines.html) | 按照行('\r', '\r\n', \n')分隔，返回一个包含各行作为元素的列表，如果参数 keepends 为 False，不包含换行符，如果为 True，则保留换行符。 |
| 15   | [swapcase()](http://www.coolpython.net/method_topic/str/swapcase.html) | 将字符串中大写转换为小写，小写转换为大写                     |
| 16   | [zfill (width)](http://www.coolpython.net/method_topic/str/zfill.html) | 返回长度为 width 的字符串，原字符串右对齐，前面填充0         |





## 查询类

| 编号 | 方法名称                                                     | 功能描述                                                |
| :--- | :----------------------------------------------------------- | :------------------------------------------------------ |
| 1    | [count](http://www.coolpython.net/method_topic/str/count.html) | 返回子串出现的次数                                      |
| 2    | [find](http://www.coolpython.net/method_topic/str/find.html) | 查找子串sub在字符串中的位置，如果找不到返回-1           |
| 3    | [rfind(sub[, start[, end\]])](http://www.coolpython.net/method_topic/str/rfind.html) | 类似于 find()函数，不过是从右边开始查找                 |
| 4    | [index](http://www.coolpython.net/method_topic/str/index.html) | 跟find()方法一样，只不过如果sub不在字符串中会报一个异常 |
| 5    | [rindex(sub[, start[, end\]])](http://www.coolpython.net/method_topic/str/rindex.html) | 类似于 index()，不过是从右边开始                        |



## 验证类⭐

| 编号 | 方法名称                                                     | 功能描述                                                     |
| :--- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| 1    | [startswith(prefix[, start[, end\]])](http://www.coolpython.net/method_topic/str/startswith.html) | 检查字符串是否是以指定子字符串 prefix 开头                   |
| 2    | [endswith](http://www.coolpython.net/method_topic/str/endswith.html) | 检查字符串是否以 suffix 结束                                 |
| 3    | [isalnum](http://www.coolpython.net/method_topic/str/isalnum.html) | 如果字符串至少有一个字符并且所有字符都是字母或数字则返 回 True,否则返回 False |
| 4    | [isalpha](http://www.coolpython.net/method_topic/str/isalpha.html) | 如果字符串至少有一个字符并且所有字符都是字母则返回 True, 否则返回 False |
| 5    | [isdigit](http://www.coolpython.net/method_topic/str/isdigit.html) | 如果字符串只包含数字则返回 True 否则返回 False               |
| 6    | [isnumeric](http://www.coolpython.net/method_topic/str/isnumeric.html) | 如果字符串中只包含数字字符，则返回 True，否则返回 False      |
| 7    | [isspace()](http://www.coolpython.net/method_topic/str/isspace.html) | 如果字符串中只包含空白，则返回 True，否则返回 False.         |
| 8    | [isdecimal()](http://www.coolpython.net/method_topic/str/isdecimal.html) | 检查字符串是否只包含十进制字符，如果是返回 true，否则返回 false |
| 9    | [istitle()](http://www.coolpython.net/method_topic/str/istitle.html) | 如果字符串是标题化的(见 title())则返回 True，否则返回 False  |
| 9    | [isupper()](http://www.coolpython.net/method_topic/str/isupper.html) | 如果字符串中包含至少一个区分大小写的字符，并且所有这些(区分大小写的)字符都是大写，则返回 True，否则返回 False |
| 10   | [islower](http://www.coolpython.net/method_topic/str/islower.html) | 如果字符串中包含至少一个区分大小写的字符，并且所有这些(区分大小写的)字符都是小写，则返回 True，否则返回 False |



## #汇总

| **方法**                                                     | **描述**                                                     |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [string.capitalize()](https://www.runoob.com/python/att-string-capitalize.html) | 把字符串的第一个字符大写                                     |
| [string.center(width)](https://www.runoob.com/python/att-string-center.html) | 返回一个原字符串居中,并使用空格填充至长度 width 的新字符串   |
| **[==string.count(str, beg=0, end=len(string))==](https://www.runoob.com/python/att-string-count.html)** | 返回 str 在 string 里面出现的次数，如果 beg 或者 end 指定则返回指定范围内 str 出现的次数 |
| [==string.decode(encoding='UTF-8', errors='strict')==](https://www.runoob.com/python/att-string-decode.html) | 以 encoding 指定的编码格式解码 string，如果出错默认报一个 ValueError 的 异 常 ， 除非 errors 指 定 的 是 'ignore' 或 者'replace' |
| [string.encode(encoding='UTF-8', errors='strict')](https://www.runoob.com/python/att-string-encode.html) | 以 encoding 指定的编码格式编码 string，如果出错默认报一个ValueError 的异常，除非 errors 指定的是'ignore'或者'replace' |
| **[string.endswith(obj, beg=0, end=len(string))](https://www.runoob.com/python/att-string-endswith.html)** | 检查字符串是否以 obj 结束，如果beg 或者 end 指定则检查指定的范围内是否以 obj 结束，如果是，返回 True,否则返回 False. |
| [string.expandtabs(tabsize=8)](https://www.runoob.com/python/att-string-expandtabs.html) | 把字符串 string 中的 tab 符号转为空格，tab 符号默认的空格数是 8。 |
| **[==string.find(str, beg=0, end=len(string))==](https://www.runoob.com/python/att-string-find.html)** | 检测 str 是否包含在 string 中，如果 beg 和 end 指定范围，则检查是否包含在指定范围内，如果是返回开始的索引值，否则返回-1 |
| **[string.format()](https://www.runoob.com/python/att-string-format.html)** | 格式化字符串                                                 |
| **[string.index(str, beg=0, end=len(string))](https://www.runoob.com/python/att-string-index.html)** | 跟find()方法一样，只不过如果str不在 string中会报一个异常.    |
| [string.isalnum()](https://www.runoob.com/python/att-string-isalnum.html) | 如果 string 至少有一个字符并且所有字符都是字母或数字则返回 True,否则返回 False |
| [string.isalpha()](https://www.runoob.com/python/att-string-isalpha.html) | 如果 string 至少有一个字符并且所有字符都是字母则返回 True,否则返回 False |
| [string.isdecimal()](https://www.runoob.com/python/att-string-isdecimal.html) | 如果 string 只包含十进制数字则返回 True 否则返回 False.      |
| [string.isdigit()](https://www.runoob.com/python/att-string-isdigit.html) | 如果 string 只包含数字则返回 True 否则返回 False.            |
| [string.islower()](https://www.runoob.com/python/att-string-islower.html) | 如果 string 中包含至少一个区分大小写的字符，并且所有这些(区分大小写的)字符都是小写，则返回 True，否则返回 False |
| [string.isnumeric()](https://www.runoob.com/python/att-string-isnumeric.html) | 如果 string 中只包含数字字符，则返回 True，否则返回 False    |
| [string.isspace()](https://www.runoob.com/python/att-string-isspace.html) | 如果 string 中只包含空格，则返回 True，否则返回 False.       |
| [string.istitle()](https://www.runoob.com/python/att-string-istitle.html) | 如果 string 是标题化的(见 title())则返回 True，否则返回 False |
| [string.isupper()](https://www.runoob.com/python/att-string-isupper.html) | 如果 string 中包含至少一个区分大小写的字符，并且所有这些(区分大小写的)字符都是大写，则返回 True，否则返回 False |
| **[string.join(seq)](https://www.runoob.com/python/att-string-join.html)** | 以 string 作为分隔符，将 seq 中所有的元素(的字符串表示)合并为一个新的字符串 |
| [string.ljust(width)](https://www.runoob.com/python/att-string-ljust.html) | 返回一个原字符串左对齐,并使用空格填充至长度 width 的新字符串 |
| [string.lower()](https://www.runoob.com/python/att-string-lower.html) | 转换 string 中所有大写字符为小写.                            |
| [string.lstrip()](https://www.runoob.com/python/att-string-lstrip.html) | 截掉 string 左边的空格                                       |
| [==string.maketrans(intab, outtab)==](https://www.runoob.com/python/att-string-maketrans.html) | maketrans() 方法用于创建字符映射的转换表，对于接受两个参数的最简单的调用方式，第一个参数是字符串，表示需要转换的字符，第二个参数也是字符串表示转换的目标。 |
| [max(str)](https://www.runoob.com/python/att-string-max.html) | 返回字符串 *str* 中最大的字母。                              |
| [min(str)](https://www.runoob.com/python/att-string-min.html) | 返回字符串 *str* 中最小的字母。                              |
| **[string.partition(str)](https://www.runoob.com/python/att-string-partition.html)** | 有点像 find()和 split()的结合体,从 str 出现的第一个位置起,把 字 符 串 string 分 成 一 个 3 元 素 的 元 组 (string_pre_str,str,string_post_str),如果 string 中不包含str 则 string_pre_str == string. |
| **[string.replace(str1, str2, num=string.count(str1))](https://www.runoob.com/python/att-string-replace.html)** | 把 string 中的 str1 替换成 str2,如果 num 指定，则替换不超过 num 次. |
| [string.rfind(str, beg=0,end=len(string) )](https://www.runoob.com/python/att-string-rfind.html) | 类似于 find() 函数，返回字符串最后一次出现的位置，如果没有匹配项则返回 -1。 |
| [string.rindex( str, beg=0,end=len(string))](https://www.runoob.com/python/att-string-rindex.html) | 类似于 index()，不过是从右边开始.                            |
| [string.rjust(width)](https://www.runoob.com/python/att-string-rjust.html) | 返回一个原字符串右对齐,并使用空格填充至长度 width 的新字符串 |
| [string.rpartition(str)](https://www.runoob.com/python/att-string-rpartition.html) | 类似于 partition()函数,不过是从右边开始查找                  |
| [string.rstrip()](https://www.runoob.com/python/att-string-rstrip.html) | 删除 string 字符串末尾的空格.                                |
| **[string.split(str="", num=string.count(str))](https://www.runoob.com/python/att-string-split.html)** | 以 str 为分隔符切片 string，如果 num 有指定值，则仅分隔 num+ 个子字符串 |
| [string.splitlines([keepends\])](https://www.runoob.com/python/att-string-splitlines.html) | 按照行('\r', '\r\n', \n')分隔，返回一个包含各行作为元素的列表，如果参数 keepends 为 False，不包含换行符，如果为 True，则保留换行符。 |
| [string.startswith(obj, beg=0,end=len(string))](https://www.runoob.com/python/att-string-startswith.html) | 检查字符串是否是以 obj 开头，是则返回 True，否则返回 False。如果beg 和 end 指定值，则在指定范围内检查. |
| **[string.strip([obj\])](https://www.runoob.com/python/att-string-strip.html)** | 在 string 上执行 lstrip()和 rstrip()                         |
| [string.swapcase()](https://www.runoob.com/python/att-string-swapcase.html) | 翻转 string 中的大小写                                       |
| [string.title()](https://www.runoob.com/python/att-string-title.html) | 返回"标题化"的 string,就是说所有单词都是以大写开始，其余字母均为小写(见 istitle()) |
| **[string.translate(str, del="")](https://www.runoob.com/python/att-string-translate.html)** | 根据 str 给出的表(包含 256 个字符)转换 string 的字符,要过滤掉的字符放到 del 参数中 |
| [string.upper()](https://www.runoob.com/python/att-string-upper.html) | 转换 string 中的小写字母为大写                               |
| [string.zfill(width)](https://www.runoob.com/python/att-string-zfill.html) | 返回长度为 width 的字符串，原字符串 string 右对齐，前面填充0 |

针对大、小写：

- `capitalize()`  字符串首字母大写
- `title()`  字符串中所有单词的首字母大写
- `upper()`  全部大写
- `lower()`  全部小写
- `swapcase()`  大小写互换



针对对齐：

- `center()`  居中对齐：指定宽度和填充字符(注意：如果fillchar超过1个长度或为非字符串或为汉字，则会报出异常)
- `ljust()/rjust()`  指定字符宽度填充(左/右对齐)
- `zfill()`  右对齐：不足宽度补零



```python
### 分割字符串(转化为列表)
split([sep [,maxsplit]])  #默认空格作为分隔符
rsplit() #从右往左开始分割

partition("字符串"，"分割字符") #分割点为首次出现sep的地方
rpartition()  #分割点为最后一次出现sep的地方

splitlines([keepends])  #keepends是一个bool值，如果为真每行后而会**保留行分割符**



### split()
url = "www.google.com/login/ex"
a, b, c = url.split("/")
print(a, b, c)  #www.google.com login ex

x = url.split("/")
print(x)        #['www.google.com', 'login', 'ex']
p = url.split("/", -1)
print(p)        #['www.google.com', 'login', 'ex']

y = url.split("/")[-1]
print(y)        #ex

z = url.split("/", 1)
print(z)        #['www.google.com', 'login/ex']
```



## 应用|提取字符串数据(一般考虑正则表达式)

`int(''.join(char for char in example if str.isdigit()))` 提取数字

`(''.join(char for char in example if str.alpha()))` 提取字母





# #附录 换行符

| 平台       |        |
| ---------- | ------ |
| Windows    | '\r\n' |
| Unix/Linux | '\n'   |
| Mac        | '\r'   |
| python     | '\n'   |

1 注意：在''' '''中不想换行，添加\ @类似matlab

2 避免转移字符的误解  => r"C:**\n**otes"





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

# #参考文献

[Python基本数据类型之str](https://www.cnblogs.com/whatisfantasy/p/5956747.html)

[Link: 菜鸟教程|python 字符串](https://www.runoob.com/python/python-strings.html)