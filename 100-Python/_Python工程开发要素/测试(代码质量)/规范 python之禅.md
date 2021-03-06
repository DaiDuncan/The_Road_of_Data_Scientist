# python之禅⭐

```python
### 交互式解释器里输入指令
import this	


The Zen of Python, by Tim Peters

Beautiful is better than ugly.
Explicit is better than implicit.
Simple is better than complex.
Complex is better than complicated.
Flat is better than nested.
Sparse is better than dense.
Readability counts.
Special cases aren't special enough to break the rules.
Although practicality beats purity.
Errors should never pass silently.
Unless explicitly silenced.
In the face of ambiguity, refuse the temptation to guess.
There should be one-- and preferably only one --obvious way to do it.
Although that way may not be obvious at first unless you're Dutch.
Now is better than never.
Although never is often better than *right* now.
If the implementation is hard to explain, it's a bad idea.
If the implementation is easy to explain, it may be a good idea.
Namespaces are one honking great idea -- let's do more of those!


优美胜于丑陋（Python 以编写优美的代码为目标）
明了胜于晦涩（优美的代码应当是明了的，命名规范，风格相似）
简洁胜于复杂（优美的代码应当是简洁的，不要有复杂的内部实现）
复杂胜于凌乱（如果复杂不可避免，那代码间也不能有难懂的关系，要保持接口简洁）
扁平胜于嵌套（优美的代码应当是扁平的，不能有太多的嵌套）
间隔胜于紧凑（优美的代码有适当的间隔，不要奢望一行代码解决问题）
可读性很重要（优美的代码是可读的）
即便假借特例的实用性之名，也不可违背这些规则（这些规则至高无上）
 
不要包容所有错误，除非你确定需要这样做（精准地捕获异常，不写 except:pass 风格的代码）
 
当存在多种可能，不要尝试去猜测
而是尽量找一种，最好是唯一一种明显的解决方案（如果不确定，就用穷举法）
虽然这并不容易，因为你不是 Python 之父（这里的 Dutch 是指 Guido ）
 
做也许好过不做，但不假思索就动手还不如不做（动手之前要细思量）
 
如果你无法向人描述你的方案，那肯定不是一个好方案；反之亦然（方案测评标准）
 
命名空间是一种绝妙的理念，我们应当多加利用（倡导与号召）
```



## 明了胜于晦涩

```python
### 用两种方式向百度发送一个get请求
import urllib.request
res = urllib.request.urlopen('http://www.baidu.com/')
print(res.read())


res = requests.get("http://www.baidu.com")	#代码意图更加明显，明确指明了要发送get请求
print(res.text)
```



## 简洁胜于复杂

```python
### 下面两段代码都是发送post请求
# 方式1
import urllib.parse
import urllib.request
import json

url = 'http://www.httpbin.org/post'
values = {'name' : 'Michael Foord'}

data = urllib.parse.urlencode(values).encode()
response = urllib.request.urlopen(url, data)
body = response.read().decode()
print(json.loads(body))



# 方式2
import requests

url = 'http://www.httpbin.org/post'
data = {'name' : 'Michael Foord'}

response = requests.post(url, data=data)
response.json()
```



## 可读性很重要

1 简单就好

```python
### 简单就好
for i in range(len(lst)):
    print(lst[i])
    
# 优化调整
for i in lst:
    print(i)
```

2 变量命名

```python
name_lst = ['小明', '小红']

# 优化调整
stu_dict = {'name': '小明', 'age': 20}	#习惯性的在变量名称中加上类型的标识
```

除了像age，count这样明显表示int类型数据的变量，我喜欢用 "i_" 开头， 表示它是int类型的数据，我认为这样做可以增强代码的可阅读性。



3 代码注释

好的注释，要解释代码做了什么

如果有必要，还要解释为什么要这样做，这对于维护者修改代码是非常有帮助的



## 扁平胜于嵌套 => 避免过度嵌套

嵌套最好控制在3层以内



for循环，while训话，if语句，这些都会产生嵌套，每产生一层嵌套，就会产生一次缩进，最终，就会产生类似下图的代码

![image-20210127222103604](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210127222103.png)





代码嵌套让代码变得难以阅读，尤其当嵌套层次超过3层以后，嵌套的太多不符合python扁平胜于嵌套的编程理念

### 1 使用生成式

```python
tmp = [4, 2, 1, 5, 7]

### 从列表tmp中找出大于3的数据
lst = [item for item in tmp if item > 3]
print(lst)
```





### 2 三元表达式

python没有三元表达式，但可以借助if else 来实现

```python
a = 5

b = 4 if a > 3 else 8
```





### 3 优化条件表达式(基于布尔逻辑)

```python
lst1 = [1, 2, 6, 4, 7, 8, 9]
lst2 = [14, 13, 11, 8, 6]

### 从两个列表中各取出一个数值，如果两个数值都大于5且他们的和是15，则输出这两个数值
for item1 in lst1:
    for item2 in lst2:
        if item1 > 5 and item2 > 5 and item1 + item2 == 15:
            print(item1, item2)
```





## 间隔胜于紧凑

```python
### 紧凑
# 看起来很简洁，但实际上并不利于阅读
def func_1(a, b):
    return a*b


def func_2(a, b):
    return a+b


def func(a, b):
    return func_1(a, b) if a < 10 else func_2(a, b)	# 紧凑



### 优化
def func(a, b):
    if a < 10:
        res = func_1(a, b)
    else:
        res = func_2(a, b)

    return res
```

从代码排版上来说，我喜欢：

- 在for循环，while循环，if条件语句前面加上一个空行，因为在这些语句之前，通常来说都是准备数据的代码
- 这些语句块结束后，也放一个空行，这样代码看起来就会显得错落有致



## 不要包容所有错误

```python
try:
    do something
except:
    处理异常
```

这种写法的一个好处就是，写代码的人，根本不需要关心代码里面发生了什么异常，来一个终极捕获，万事大吉了，剩下的事情就是记录异常信息

在实践中，我还没有遇到什么严重的问题，但我自己也很清楚，这样做是极不合理的，原因在于，自己写的代码，自己都不清楚可能会有什么异常产生，这不就等于像世人宣告，自己不了解自己写的代码么？



如果你了解自己的代码，怎么可能不知道可能发引发哪些异常呢？

你辩解说，那么多异常，谁知道会发生什么呢，可是你是技术人员，经验的积累难道还不足以让你评估代码潜在的异常么？

胡子眉毛一把抓式的捕获异常，并不能够让我们更加了解自己的程序，也不能让我们提高对代码的掌控能力，除了在时间紧任务重时帮我们快速完成工作外，这种做法不能带来任何其他收益。









# #参考文献

[Link: 酷python](http://www.coolpython.net/python_senior/project/code_quality_zen.html)

[Link: 翻译 python之禅](http://codingpy.com/article/designing-pythonic-apis/)