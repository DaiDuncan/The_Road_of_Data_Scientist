# Python-基础 | 字符串

## 一、Python3 字符串的编码

| 编码          | 说明                                          |
| ------------- | --------------------------------------------- |
| ASCII 编码    | 只有大小写英文字母、数字和一些符号等127个字符 |
| (专用)        | 中文的GB2312编码 日文的Shift_JIS编码          |
| Unicode(统一) | u'字符串'  \u四位十六进制数                   |



Python 不支持单字符类型，单字符也在Python也是作为一个字符串使用

> `byt = b'Python'` bytes数据，只接受 ASCII 码这个范围的字符



- 编码
  需要将文件保存到外设或进行网络传输时，就要进行编码转换，将**字符转换为字节**，以提高效率@二进制文件(b' ')



> `.encode(encoding='UTF-8',errors='strict')`  将字符转换为字节 (默认编码是 UTF-8)
>
> `.decode(encoding='UTF-8',errors='strict')`  将字节转换为字符
>
> 说明：出错默认报ValueError,除非errors是ignore或replace



![image](https://cdn.nlark.com/yuque/0/2020/png/1136179/1591176765778-62260ef9-a1e4-4dba-870b-3d59a63619df.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_14%2Ctext_6K-t6ZuALUREdW5jYW4%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)



## 二、创建字符串

可以使用单引号、双引号、三单引号和三双引号

三引号可以定义**多行**字符串



> [注意](https://zhuanlan.zhihu.com/p/28879694)：当我们字符串中包含单引号（'）使用双引号创建字符串；
>
> 字符串包含双引号（"）使用单引号创建。三个引号（'''）里面能出现单引号双引号回车键等，三个引号中字符串会保持传入的格式



## 三、操作字符串

**注意**：所有的字符串函数都是有返回值的，默认inplace = False



### 1 特性

属于序列，具有序列的通用操作

- 索引(indexing)
- 切片(slicing)
- 迭代(iteration)
- 加(adding)
- 乘(multiplying)



### 2 查/改/增/删

#### 1）查

`str[start: end: step]`  切片

str[::2] 隔两个截取  

str[::-1]倒序

`find(sub [,start [,end]])`  返回索引值，没有返回值为-1

`rfind()`  从右向左查同find

`index(sub [,start [,end]])`  查不到会报错

`rindex()`  从右向左返回索引值同index





#### 2）改(替换)/增(拼接)

`replace``(old, new[, count])`  指定字符替换为新的字符，可指定个数



`expandtabs([n])`  **将字符串中(tab符号)\t转换成n个空格**

`format_map()` 格式化输出，以字典形式存储key-value数据

`maketrans()`+`translate(table [,deletechars])`  根据参数table=maketrans给出的表(包含 256 个字符)转换字符串的字符, 要**过滤掉的字符**放到deletechars参数中



- 拼接

```
.join([str1, str2])
```



#### 3）删

`strip()`  移除字符串头尾指定的字符（默认为空格或换行符）或字符序列(**多个字符**亦可)

```
## str.strip() 
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

`lstrip()`  移除字符串头

`rstrip`  移除字符串尾





### 3 排序/遍历/统计

#### 1）统计

```
len()
```

`.count(sub[, start[, end]])` **区间内**某个字符的个数



#### 2）内置函数：判断

```
in/not in
```

`isalnum()`  是否只包含字母或数字

`isalpha()`  是否只包含字母

`isdecimal()`  是否是十进制正整数

`isdigit()`  是否只包含数字

`isdentifier()`  是否是python中的标识符

`isupper()`  字符串字母是不是都是大写

`islower()`  字符串字母是不是都是小写

`isnumeric()`  是不是数字，无论中文，只要是数字就能判断

`isprintable()`  是否可打印

`isspace()`  是否都是空格，tab等

`istitle()`  是否所有字符首字母大写

`startswith(prefix[, start[, end]])`  是否是字符串或字符开头

`endswith(suffix[, start[, end]])`  是否是字符串或字符结尾



#### 3）内置函数：格式化字符串+转义



- 格式化

```
'%s%s'%(str1,str2)
'{}{}'.format(str1, str2)
```



`capitalize()`  字符串首字母大写

`title()`  字符串中所有单词的首字母大写

`upper()`  全部大写

`lower()`  全部小写

`swapcase()`  大小写互换



`center()`  居中对齐：指定宽度和填充字符(注意：如果fillchar超过1个长度或为非字符串或为汉字，则会报出异常)

`ljust()/rjust()`  指定字符宽度填充(左/右对齐)

`zfill()`  右对齐：不足宽度补零





- `%`之后

示例：str = format('%02d-%02d-%s' %(a, b, c))

`+` 右对齐，正数前加正号，负数前加负号

`-`  左对齐，正数前无符号，负数前加负号

空格 右对齐；正数前加空格，负数前加负号

`0`  右对齐，正数前无符号，负数前加负号，用0填充

`%s`  字符串

`%r`  字符串采用repr()显示

`%c`  单个字符

`%b`  二进制整数 bin

`%i`  十进制整数 int

`%o`  无符号八进制 oct

`%x`  无符号十六进制 hex

`%d`  整数

`%f`  浮点数

`%u`  无符号整数

`%e`  科学记数法



- **转义**

`r" "`或`u" "`  转义声明(大写R亦可)(多用于文件地址的输入和正则表达式)

`\(置于行尾)` 表示续行

`\\`  **表示\符号**

`\'`  **单引号**

`\"`  **双引号**

`\b`  退格(Backspace)

`\e`  转义

`\n`  **换行**

`\v`  纵向制表符

`\t`  **横向制表符Tab**

`\r`  回车(光标重新回到本行开头)

*(补充：空行为\r\n)*

`\f`  换页



说明：

linux只用\n换行

win下用\r\n表示换行

在python中存在继承了 回车符\r 和 换行符\n 两种标记： `.replace('\n', '').replace('\r', '')` 



- 内置函数：**分割**字符串(转化为列表)

`split([sep [,maxsplit]])`  默认空格作为分隔符

`rsplit()` 从右往左开始分割

`partition(sep)`  分割点为首次出现sep的地方

`rpartition()`  分割点为最后一次出现sep的地方

`splitlines([keepends])`  keepends是一个bool值，如果为真每行后而会**保留行分割符**



- 提取字符串数据(优先考虑正则表达式)

`int(''.join(char for char in example if str.isdigit()))` 提取数字

`(''.join(char for char in example if str.alpha()))` 提取字母



## 四、拓展



- 正则表达式模块re(Regular Expression)

`re.findall('\d+', str)`  寻找str中所有的数字及其1~n扩展



![image](https://cdn.nlark.com/yuque/0/2020/png/1136179/1591176765807-1438a98f-7ec2-4d20-96c4-89e4c80bb59b.png)

![image](https://cdn.nlark.com/yuque/0/2020/png/1136179/1591176765875-045d9511-b3fb-4820-a8e8-157310853685.png)



注意：re库在进行字符串匹配时会默认采用“贪婪匹配”，也就是说会返回符合条件的最长字符串。通过在操作符后增加一个“?”可以将正则式变成最小匹配：

![image](https://cdn.nlark.com/yuque/0/2020/png/1136179/1591176765814-e8e3f0f4-bc68-490f-9cc7-0796d00e4a50.png)





## 参考文献

[菜鸟教程 | python strip()方法](https://www.runoob.com/python/att-string-strip.html)