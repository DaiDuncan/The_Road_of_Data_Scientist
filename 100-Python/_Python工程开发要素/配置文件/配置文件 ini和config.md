ini配置文件以.ini结尾，config文件以.config结尾，他们的配置方式相同



# ini配置文件

```ini
[mysql]
host=127.0.0.1
port=3306
user=root
password=yourpassword
dbname=test


[redis]
host=127.0.0.1
port=6379
password=88888
db=0
```

配置文件由两部分组成sections与items

- sections 用来区分不同的配置块
- items 是sections下面的键值



## configparser解析ini文件

python3中提供了标准模块configparser，该模块下有一个ConfigParser类，可以用来解析ini文件

```python
from configparser import ConfigParser


cf = ConfigParser()
cf.read('db.ini')

print(cf.sections())        # ['mysql', 'redis']
print(cf.options('mysql'))  # 输出mysql下的所有配置项
print(cf.items('mysql'))    # 输出mysql下的所有键值对

print(cf.get('mysql', 'host'))  # 输出mysql 下配置项host的值
print(cf.getint('mysql', 'port'))  # 输出port


'''程序输出结果
['mysql', 'redis']
['host', 'port', 'user', 'password', 'dbname']
[('host', '127.0.0.1'), ('port', '3306'), ('user', 'root'), ('password', 'yourpassword'), ('dbname', 'test')]
127.0.0.1
3306
'''
```

ConfigParser的get方法可以根据section和option获取配置的值，这个方法返回的是字符串

此外还提供了getint和getfloat方法，分别获取int类型和float类型的配置项，如果你嫌麻烦，可以统一使用get方法

ConfigParser 也可以用来设置配置项，它提供了add_section方法和set方法，但实际工作中都是手动配置ini文件







