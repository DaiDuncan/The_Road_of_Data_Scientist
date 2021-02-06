直接使用python脚本作为配置文件也是一种比较常见的方式，方便之处在于，不用特意去读取配置了，只需要引入配置脚本就可以了



弊端在于这种配置显得很不正式，而且在引入脚本后可以随意修改配置项，不够安全，小一点的项目图省事这样操作是没问题的。



构建这样的配置脚本，一般有两种方法，一种是使用字典，一种是使用类



## 方式1）字典

```python
### db.py
mysql_config = {
    'host': '127.0.0.1',
    'port': 3306,
    'db': 'test',
    'username': 'test',
    'password': '123456'
}

redis_config = {
    'host': '127.0.0.1',
    'port': 3679,
    'db': 0,
    'password': '43435'
}
```



使用时，直接引入想要使用的字典

```python
from db import mysql_config
```



## 方式2）类

相比字典，用的时候代码写起来更方便

```python
class MySqlConfig:
    host = '127.0.0.1'
    port = 3306
    db = 'test'
    username = 'test'
    password = 'test'


class RedisConfig:
    host = '127.0.0.1'
    port = 3679
    db = 0
    password = '4355'
```