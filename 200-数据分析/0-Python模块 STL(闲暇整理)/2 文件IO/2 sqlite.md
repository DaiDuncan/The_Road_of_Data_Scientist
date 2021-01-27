一款非常流行的轻型数据库，它不需要像mysql等关系型数据库那样进行安装和配置，在任何操作系统下都==可以直接使用==，许多手机端的app就用sqlite作为应用的数据库



# 创建/连接

```python
import sqlite3
conn = sqlite3.connect('test.db')
```

如果test.db这个文件不存在，则上面的代码会创建它并连接，如果存在则直接连接。

除了创建文件外，还可以在内存中创建数据

```python
import sqlite3
conn = sqlite3.connect(":memory:")
```



## 创建表

创建一张user表

```python
import sqlite3
# 创建db文件
conn = sqlite3.connect('test.db')	
cursor = conn.cursor()

# sql命令
table_sql = """
create table user(
  id INTEGER PRIMARY KEY autoincrement NOT NULL ,	#主键，自动累加；限制非0
  name text NOT NULL,
  age INTEGER NOT NULL
)
"""

### 三部曲：
# 1 cursor执行
# 2 conn提交
# 3 conn关闭

# 执行命令：创建表 
cursor.execute(table_sql)
conn.commit()  # 一定要提交,否则不会执行sql
conn.close()
```

sqlite一共有5中数据类型可以定义：

| 类型     | 描述                                                      |
| :------- | :-------------------------------------------------------- |
| NULL     | NULL 值                                                   |
| INTEGER  | 带符号的整数,根据值的大小存储在 1、2、3、4、6 或 8 字节中 |
| REAL     | 浮点值，存储为 8 字节的 IEEE 浮点数字。                   |
| TEXT     | 字符串，使用数据库编码（UTF-8、UTF-16BE 或 UTF-16LE）存储 |
| ==BLOB== | blob 数据，完全根据它的输入存储                           |



# 查 改 增 删

## 增|insert插入数据

```python
import sqlite3
conn = sqlite3.connect('test.db')
cursor = conn.cursor()

sql_lst = [
    "insert into user(name, age)values('lili', 18)",
    "insert into user(name, age)values('poly', 19)",
    "insert into user(name, age)values('lilei', 30)"
]
for sql in sql_lst:
    cursor.execute(sql)
    conn.commit()	#每一条命令执行时，单独提交commit
conn.close()
```



## 查询

```python
import sqlite3
conn = sqlite3.connect('test.db')
# 新建行
conn.row_factory = sqlite3.Row
cursor = conn.cursor()

sql = "select * from user"
cursor.execute(sql)
rows = cursor.fetchall()  # 获取全部数据
for row in rows:
    print(row.keys(), tuple(row))

conn.close()


###output
['id', 'name', 'age'] (1, 'lili', 18)
['id', 'name', 'age'] (2, 'poly', 19)
['id', 'name', 'age'] (3, 'lilei', 30)
```

- row.keys() 返回列的名字
- tuple(row) 获取tuple形式的数据

想以==字典形式获取数据==，则需要指定工厂方法，也就是一个解析数据的函数，将元组类型数据转换为字典类型数据。

```python
import sqlite3
conn = sqlite3.connect('test.db')

def dict_factory(cursor, row):
    d = {}
    for idx, col in enumerate(cursor.description):	#？？？
        d[col[0]] = row[idx]
    return d

conn.row_factory = dict_factory
cursor = conn.cursor()

sql = "select * from user"
cursor.execute(sql)
rows = cursor.fetchall()  # 获取全部数据
for row in rows:
    print(row)

conn.close()
```





## 增|update

```python
import sqlite3
conn = sqlite3.connect('test.db')

def dict_factory(cursor, row):
    d = {}
    for idx, col in enumerate(cursor.description):
        d[col[0]] = row[idx]
    return d

conn.row_factory = dict_factory
cursor = conn.cursor()

# 修改数据
update_sql = "update user set age = 22 where id = 1"
cursor.execute(update_sql)
conn.commit()

sql = "select * from user"
cursor.execute(sql)
rows = cursor.fetchall()  # 获取全部数据
for row in rows:
    print(row)

conn.close()
```







## 删

```python
import sqlite3
conn = sqlite3.connect('test.db')

def dict_factory(cursor, row):
    d = {}
    for idx, col in enumerate(cursor.description):
        d[col[0]] = row[idx]
    return d

conn.row_factory = dict_factory
cursor = conn.cursor()

# 删除数据
delete_sql = "delete from user where id = 1"
cursor.execute(delete_sql)
conn.commit()

sql = "select * from user"
cursor.execute(sql)
rows = cursor.fetchall()  # 获取全部数据
for row in rows:
    print(row)

conn.close()
```





# 补充|批量写入数据executemany

有大量数据需要写入，那么不建议你使用execute，因为每一次执行execute，都要和数据库进行一次数据交互，而批量执行则可以免去这种频繁的数据交互。

```python
import sqlite3
conn = sqlite3.connect('test.db')

def dict_factory(cursor, row):
    d = {}
    for idx, col in enumerate(cursor.description):
        d[col[0]] = row[idx]
    return d

conn.row_factory = dict_factory
cursor = conn.cursor()

# 批量执行
sql = "insert into user(name, age)values(?, ?)"
user_lst = [('lili', 18), ('poly', 19), ('lilei', 30)]
cursor.executemany(sql, user_lst)

sql = "select * from user"
cursor.execute(sql)
rows = cursor.fetchall()  # 获取全部数据
for row in rows:
    print(row)

conn.close()
```







# #参考文献

[Link: 酷python](http://www.coolpython.net/python_primary/data_storage/sqlite.html)