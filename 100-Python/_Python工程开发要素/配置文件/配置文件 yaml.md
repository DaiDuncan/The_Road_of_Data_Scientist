yaml是专门用来写配置文件的标记语言，非常简洁和强大，比ini更加直观更方便

```yaml
### db.yaml
mysql:
    host: 127.0.1.1   # ip
    user: test        # 用户名
    port: 3306        # 端口
    db: test
    password: pw

redis:
    host: 127.0.0.1
    port: 3679
    db: 0
    pwd: 3435

db_ip:
    - 192.168.0.1
    - 192.168.0.2
    - 192.168.0.2
```

yaml对大小写敏感，使用缩进来表示层次关系，这一点与python相同

- 一个缩进可以是2个空格或者4个空格，注意不要使用tab键代替空格。



上面这份配置文件等价于python中的字典

```python
{'db_ip': ['192.168.0.1', '192.168.0.2', '192.168.0.2'],
 'mysql': {'db': 'test',
           'host': '127.0.1.1',
           'password': 'pw',
           'port': 3306,
           'user': 'test'},
 'redis': {'db': 0, 'host': '127.0.0.1', 'port': 3679, 'pwd': 3435}}
```





## 第三方库PyYaml

python没有提供专门的标准库来读取yaml文件，需要安装第三方库

```text
pip3 install PyYaml
```



读取示例如下：

```python
import yaml

f = open('db.yaml')
data = f.read()
yaml_reader = yaml.load(data)

print(yaml_reader['mysql'])    # 以字典的形式输出mysql下的所有配置
print(yaml_reader['redis']['port'])     # 输出某一项
print(yaml_reader['db_ip'])     # db_ip配置是一个数组


'''
{'host': '127.0.1.1', 'user': 'test', 'port': 3306, 'db': 'test', 'password': 'pw'}
3679
['192.168.0.1', '192.168.0.2', '192.168.0.2']
'''
```