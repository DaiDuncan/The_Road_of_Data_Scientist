SQLite是一种嵌入式数据库，它的数据库就是一个文件。

由于SQLite本身是C写的，而且体积很小，所以，经常被集成到各种应用程序中，甚至在iOS和Android的App中都可以集成。



Python就内置了SQLite3







# 操作

要操作关系数据库：

1. 首先需要连接到数据库，一个数据库连接称为`Connection`；

2. 连接到数据库后，需要打开游标，称之为`Cursor`，通过`Cursor`执行SQL语句

3. 然后，获得执行结果。



Python定义了一套操作数据库的API接口，任何数据库要连接到Python，只需要提供符合Python标准的数据库驱动即可。

```python
# 导入SQLite驱动:
>>> import sqlite3

# 连接到SQLite数据库
# 数据库文件是test.db
# 如果文件不存在，会自动在当前目录创建:
>>> conn = sqlite3.connect('test.db')

# 创建一个Cursor:
>>> cursor = conn.cursor()

# 执行一条SQL语句，创建user表:
>>> cursor.execute('create table user (id varchar(20) primary key, name varchar(20))')
<sqlite3.Cursor object at 0x10f8aa260>

# 继续执行一条SQL语句，插入一条记录:
>>> cursor.execute('insert into user (id, name) values (\'1\', \'Michael\')')
<sqlite3.Cursor object at 0x10f8aa260>

# 通过rowcount获得插入的行数:
>>> cursor.rowcount
1
# 关闭Cursor:
>>> cursor.close()

# 提交事务:
>>> conn.commit()
# 关闭Connection:
>>> conn.close()
```



再试试查询记录：

```python
>>> conn = sqlite3.connect('test.db')
>>> cursor = conn.cursor()
# 执行查询语句:
>>> cursor.execute('select * from user where id=?', ('1',))
<sqlite3.Cursor object at 0x10f8aa340>

# 获得查询结果集:
>>> values = cursor.fetchall()
>>> values
[('1', 'Michael')]
>>> cursor.close()
>>> conn.close()
```



使用Python的DB-API时，只要搞清楚`Connection`和`Cursor`对象，打开后一定记得关闭，就可以放心地使用。



## `Cursor`对象

使用`Cursor`对象执行`insert`，`update`，`delete`语句时，执行结果由`rowcount`返回影响的行数，就可以拿到执行结果。

使用`Cursor`对象执行`select`语句时，通过`fetchall()`可以拿到结果集。

- 结果集是一个`list`，每个元素都是一个`tuple`，对应一行记录。





如果SQL语句带有参数，那么需要把参数按照位置传递给`execute()`方法，有几个`?`占位符就必须对应几个参数，例如：

```python
cursor.execute('select * from user where name=? and pwd=?', ('abc', 'password'))
```

---

SQLite支持常见的标准SQL语句以及几种常见的数据类型

在Python中操作数据库时，要先导入数据库对应的驱动，然后，通过`Connection`对象和`Cursor`对象操作数据。

- 要确保打开的`Connection`对象和`Cursor`对象都正确地被关闭，否则，资源就会泄露 => `try:...except:...finally:...`







# 示例|编写函数

在Sqlite中根据分数段查找指定的名字：

```python
# -*- coding: utf-8 -*-
import os, sqlite3

# 需要在命令行执行.py文件 => __file__才有意义
db_file = os.path.join(os.path.dirname(__file__), 'test.db')
if os.path.isfile(db_file):
    os.remove(db_file)  # 删除掉重名的文件

# 初始数据: 初始化只需要执行一次(否则报错)
conn = sqlite3.connect(db_file) # 如果文件不存在，会自动在当前目录创建
cursor = conn.cursor()
cursor.execute('create table user(id varchar(20) primary key, name varchar(20), score int)')
cursor.execute(r"insert into user values ('A-001', 'Adam', 95)")
cursor.execute(r"insert into user values ('A-002', 'Bart', 62)")
cursor.execute(r"insert into user values ('A-003', 'Lisa', 78)")
cursor.close()
conn.commit()
conn.close()

def get_score_in(db_file, low, high):
    ' 返回指定分数区间的名字，按分数从低到高排序 '
    conn = sqlite3.connect(db_file) # 如果文件不存在，会自动在当前目录创建
    cursor = conn.cursor()
    cursor.execute('select name from user where score BETWEEN ? AND ? order by score', (low, high))
    # fetchall的结果集是一个list，每个元素都是一个tuple，对应一行记录
    values = cursor.fetchall()  #[('Adam',), ('Bart',), ('Lisa',)]

    cursor.close()
    conn.commit()
    conn.close()

    return [name[0] for name in values]

    

# 测试:
assert get_score_in(80, 95) == ['Adam'], get_score_in(80, 95)
assert get_score_in(60, 80) == ['Bart', 'Lisa'], get_score_in(60, 80)
assert get_score_in(60, 100) == ['Bart', 'Lisa', 'Adam'], get_score_in(60, 100)

print('Pass')
```



注意：对关键字大小写不敏感