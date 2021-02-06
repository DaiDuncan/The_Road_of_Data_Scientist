json数据更多用于数据的存储和传输，由于大多数语言都支持这种数据格式，因此也可以被用来做配置文件



使用json数据做配置文件有个弊端，无法写注释。

普通的json字符串内容紧凑，失去了key-value数据的可读性这一点可以通过格式优化来解决，有很多处理json字符串的网站可以将一个json字符串格式化成便于阅读的形式 [Link: 提高json可读性](http://www.kjson.com/)



下面是一个json配置文件示例，db.json

```json
{
    "mysql": {
        "host": "127.0.0.1",
        "port": 3306,
        "db": "test",
        "username": "test",
        "password": "123456"
    },
    "redis": {
        "host": "127.0.0.1",
        "port": 3679,
        "db": 0,
        "password": "43435"
    }
}
```





## json库

```python
import json

f = open('db.json')
data = json.load(f)
f.close()

print(data)
```