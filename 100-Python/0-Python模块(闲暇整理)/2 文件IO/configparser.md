文件IO => 文件格式化 => 配置文件解析器

configparser用于处理特定格式的文件，其**本质上是利用open**来操作文件。

### 1 指定格式

```python
# 注释1
;  注释2
 
[section1] # 节点
k1 = v1    # 值
k2:v2       # 值
 
[section2] # 节点
k1 = v1    # 值
```



### 2 获取所有节点

```python
import configparser
 
config = configparser.r()    #创建一个config对象
config.read('xxxooo', encoding='utf-8')     #把文件读到内存里
ret = config.sections()     #获取所有节点
print(ret)  #['section1', 'section2']
```



### 3 获取指定节点下所有的键值对

```python
import configparser
 
config = configparser.ConfigParser()
config.read('xxxooo', encoding='utf-8')
ret = config.items('section1')
print(ret)      #[('k1', 'v1'), ('k2', 'v2')]
```



### 4 获取指定节点下所有的键

```python
import configparser
 
config = configparser.ConfigParser()
config.read('xxxooo', encoding='utf-8')
ret = config.options('section1')
print(ret)  #['k1', 'k2']
```



### 5 获取指定节点下指定key的值

```python
import configparser
 
config = configparser.ConfigParser()
config.read('xxxooo', encoding='utf-8')
 
v = config.get('section1', 'k1')
# v = config.getint('section1', 'k1')
# v = config.getfloat('section1', 'k1')
# v = config.getboolean('section1', 'k1')
 
print(v)
```



### 6 检查、删除、添加节点

configparser的修改都是在内存里进行的，因此在改完之后需要把内存里的数据重新保存到文件中

```python
import configparser
 
config = configparser.ConfigParser()
config.read('xxxooo', encoding='utf-8')
 
# 检查
has_sec = config.has_section('section1')    #返回bool
print(has_sec)  
 
# 添加节点
config.add_section("SEC_1")
config.write(open('xxxooo', 'w'))   #文件保存
 
# 删除节点，并写入内容
config.remove_section("SEC_1")
config.write(open('xxxooo', 'w'))
```



7 检查、删除、设置指定组内的键值对

```python
import configparser
 
config = configparser.ConfigParser()
config.read('xxxooo', encoding='utf-8')
 
# 检查
has_opt = config.has_option('section1', 'k1')
print(has_opt)
 
# 删除
config.remove_option('section1', 'k1')
config.write(open('xxxooo', 'w'))
 
# 设置
config.set('section1', 'k10', "123")    
config.write(open('xxxooo', 'w'))   #如果key存在则修改，如果没有则在后面追加新的items
```



