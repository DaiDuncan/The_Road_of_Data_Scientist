序列化在数据传输和数据存储方面应用极为广泛

将数据转换为可以==通过网络传输==，或者可以==存储到本地磁盘==的数据格式（xml,json或特定格式字符串）的过程称之为序列化，反之，称之为反序列化。



不同编程语言有各自的数据类型，同样是int类型，不同的编程语言里，其存储方式是不一样的。

那么不同编程语言编写的系统如果想进行通信，就必须使用一种==双方都认可的数据格式传输数据，当前，最为流行的就是json数据格式==



# pickle

pickle是python的原生模块，它实现了对python对象的序列化和反序列化的二进制协议，由于采用的是二进制协议，因此，pickle序列化后的内容，是无法像json序列化后的内容一样正常查看的

dumps、loads用的最多，用法同json

## 序列化 pickle.dump(data, f)

```python
### 示例：序列化
import pickle

data = {
    'name': 'lilei',
    'age': 18
}

# 序列化到文本中
f = open('person', 'wb')
pickle.dump(data, f)
f.close()
```

虽然，pickle也提供了dumps方法，但返回的是byte类型数据，而不是字符串

经过实际测试，使用str函数将byte类型数据转成字符串的过程，可能会出错。这些byte数据不是从字符串转而来的，个别位置无法用utf-8进行编码，因此为了避免这类问题，不推荐你使用dumps方法。

```python
import pickle

data = {
    'name': 'lilei',
    'age': 18
}

byte_str = pickle.dumps(data)
print(byte_str)
string = str(byte_str, encoding='utf8')
```



## 反序列化 pickle.load(f)

```python
import pickle

f = open('person', 'rb')
data = pickle.load(f)
f.close()

print(data)

### output
{'name': 'lilei', 'age': 18}
```



## 序列化对象

pickle不只能对标准数据类型进行序列化，就连用户自定义的对象也可以进行序列化

```python
import pickle

class Person(object):
    def __init__(self):
        self.name = 'lilei'
        self.age = 18


person = Person()
# 序列化
f = open('person', 'wb')
pickle.dump(person, f)
f.close()

# 反序列化
f = open('person', 'rb')
data = pickle.load(f)
f.close()

print(type(data))
print(data.name)
print(data.age)


### output
<class '__main__.Person'>
lilei
18
```

有一个地方需要特别注意，使用pickle反序列化用户自定义对象时，必须==保证可以访问到用户自定义的类==，这样，pickle才能构造出这个类的对象。



# json

用于字符串和python数据类型间进行转换，不同语言之间兼容性好。

但是只能转换==字符串，字典，列表==等简单的数据类型。

| python          | json   |
| :-------------- | :----- |
| dict            | object |
| list, ==tuple== | array  |
| str             | string |
| int, float      | number |
| True            | true   |
| False           | false  |
| None            | null   |

Json模块提供了四个功能：

- dumps
- dump
- loads
- load

```python
import json

s = '{"k1": "v1", "k2": "v2"}'  #json在处理这种字符串的时候，目标字典元素必须是""，否则报错
result = json.loads(s)
print(result, type(result))  # {'k1': 'v1', 'k2': 'v2'} <class 'dict'>

result = json.load(open('db', 'r')) #读取db中的内容，并把字符串转变成dict
print(result)       #{'k1': 'v1', 'k2': 'v2'}



dic = {'k1': 'v1', 'k2': 'v2'}
result = json.dumps(dic)
print(result, type(result))  # {"k2": "v2", "k1": "v1"} <class 'str'>

dic = {'k1': 'v1', 'k2': 'v2'}
result = json.dump(dic,open('db','w')) #输出后写入db文件
```

注意：json提供的是一种**通用式**的序列化，因此它适用的**数据类型比较有限**，如list、dict，但是tuple就不支持，因为在其他语言中没有tuple这种数据类型。



## 序列化

```python
import json

data = {
    'name': 'lilei',
    'age': 18
}

f = open('person', 'w')
json.dump(data, f)
f.close()

string = json.dumps(data)
print(string)	#{"name": "lilei", "age": 18}
```

不论是转成字符串，还是序列化存储到文件中，其内容都是可以看得懂的



## 反序列化

```python
import json

data = '{"name": "lilei", "age": 18}'
data = json.loads(data)
print(data)

with open('person', 'r') as f:
    data = json.load(f)
    print(data)
    
    
### output
{'name': 'lilei', 'age': 18}
{'name': 'lilei', 'age': 18}
```







# [msgpack](https://msgpack.org/)

一种和json很相似，但是速度更快，体积更小的二进制序列化数据格式

```shell
pip3 install msgpack-python
```

```python
data = {
	'name': 'lili',
    'age': 18,
    'score': 100
}

# 和json进行序列化数据大小对比
msg_str = msgpack.packb(stu)
print(msg_str)
print(len(msg_str))

json_str = json.dumps(stu)
json_str = bytes(json_str, encoding = "utf8")
print(json_str)
print(len(json_str))

# 反序列化
data = msgpack.unpackb(msg_str)
print(data)


### output
b'\x83\xa4name\xa4lili\xa3age\x12\xa5scored'
23
b'{"name": "lili", "age": 18, "score": 100}'
41
{b'name': b'lili', b'age': 18, b'score': 100}
```

msgpack序列化后的数据，其体积比json序列化后的要小很多，这样，在网络传输上就会更快



# 对比|json与msgpack性能

## 数据大小

```python
import msgpack
import json

data = {
    'count': 100,
    'price': 29832.09,
    'desc': 'MessagePack is an efficient binary serialization format. It lets you exchange data among multiple languages like JSON. But it’s faster and smaller. Small integers are encoded into a single byte, and typical short strings require only one extra byte in addition to the strings themselves',
    'desc_cn': "MessagePack  是一个高效的二进制序列化格式。它让你像JSON一样可以在各种语言之间交换数据。但是它比JSON更快、更小。小的整数会被编码成一个字节，短的字符串仅仅只需要比它的长度多一字节的大小",
    'bool': True
}

data = [data for i in range(10)]
json_str = json.dumps(data)
print(len(json_str))

msg_str = msgpack.dumps(data)
print(len(msg_str))

### output
8740
6001
```

msgpack节省了30%的存储空间





## 序列化速度

```python
import msgpack
import json
import time

data = {
    'count': 100,
    'price': 29832.09,
    'desc': 'MessagePack is an efficient binary serialization format. It lets you exchange data among multiple languages like JSON. But it’s faster and smaller. Small integers are encoded into a single byte, and typical short strings require only one extra byte in addition to the strings themselves',
    'desc_cn': "MessagePack  是一个高效的二进制序列化格式。它让你像JSON一样可以在各种语言之间交换数据。但是它比JSON更快、更小。小的整数会被编码成一个字节，短的字符串仅仅只需要比它的长度多一字节的大小",
    'bool': True
}


data = [data for i in range(10)]
t1 = time.time()
for i in range(100000):
    json_str = json.dumps(data)

t2 = time.time()

for i in range(100000):
    msg_str = msgpack.dumps(data)
t3 = time.time()

print(t2-t1)
print(t3-t2)

### output
6.320836067199707
0.9859249591827393
```

速度上，msgpack快了6倍，更快的速度，更小的体积。