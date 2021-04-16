SQL：Structured Query Language

databese：以结构化的形式，查找、筛选和存储数据

```sql
-- 单行注释
/* 多行注释 */

-- 语句结尾是分号;
```

## 意义

企业：库存

政府：市民信息

图书馆：书籍

学校：学生



## 术语

| 术语                                                         |                                                              |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| database                                                     |                                                              |
| table<br>temporary table                                     | table视图                                                    |
| statement, clause                                            | 一条语句可以由多个短语clauses组成                            |
| parameter                                                    | database, table的name<br>column/field                        |
| column, field                                                | 列：字段                                                     |
| record                                                       | 行：记录                                                     |
| identifier                                                   | SELECT <identifier>                                          |
| expression                                                   | WHERE <expression>                                           |
| wildcard 通配符                                              | %：表示任意长度的字符串，类似shell中的*(\*在SELECT \*中被使用)<br>_：表示任意一个字符串，类似shell中的? |
|                                                              | @示例：'\_\_\_\_\_\_\_\_@%' => 可以用来表示邮件名<br>WHERE email LIKE '\_\_\_\_\_\_\_\_@%'; |
| operator                                                     | SET age = 24;<br>                                            |
|                                                              | #comparison: <br>WHERE age = 24;<br><<br>><br>!=<br>!><br>!< |
|                                                              | #logical:<br>BETWEEN 0 AND 99;<br>NOT BETWEEN 0 AND 99;<br/> |
|                                                              |                                                              |
| query                                                        | SELECT语句                                                   |
| aggregate func 代表了predefined processes, 但只用一个简单的函数就可以表示 | 计算整列的值，返回单个值single value                         |
| scalar func 基于给定的input(可包含一个或多个parameters)，返回一个或多个值<br>作用不等同于calculations |                                                              |



## 汇总|常见数据类型

| 数据类型       | 适用情境 |
| -------------- | -------- |
| NULL: no value |          |
|                |          |
| TEXT           | name     |
| INT            | age      |
| FLOAT          | height   |
| VARCHAR(20)    |          |
|                |          |
|                |          |
|                |          |
|                |          |
|                |          |



# 操作1|创建

```sql
CREATE DATABASE <db file>;
CREATE DATABASE <adventure>;	--示例


CREATE TABLE <table file>; --错误示例：至少添加一列
CREATE TABLE friends(
    name TEXT
	);
	
	
/* unique id */
-- 背景：当name重合时，产生冲突
CREATE TABLE friends(
    id INT NOT NULL, 
    name TEXT
	);
	
	
/* 添加不同的columns */
ALTER TABLE friends
ADD name VARCHAR(20) NOT NULL,	-- VARCHAR类似TEXT，但可以设定最大的长度(20)，以便节省空间
ADD age INT;
```





# 操作2|查 选/索引(含切片) 改 增 删

## 1 查

```sql
SELECT <key1>, <key2>, ... FROM <TABLE>;
SELECT * FROM <TABLE>;
SELECT name FROM friends;

SELECT name FROM friends WHERE name='Robin';	--常用单引号(有些数据库这样规定)


/* WHERE短语 */
WHERE <EXP1> AND OR NOT <EXP2>

/* LIKE */
SELECT * FROM friends WHERE name LIKE '%in%';	--对象中包含'in', 比如‘Robin’, 'Win'


/* 初始数据 */
WHOLE TABLE?	--字段：id, first_name, last_name, email, city
```





## 2 选/索引 => unique id

```sql

```





## 3 改

```sql
UPDATE friends
SET age = 12
WHERE name = 'Win';


```



## 4 增

```sql
INSERT INTO friends
VALUES (1, 'Robin', 42, 1.72);	-- id, name, age, height

-- 只添加部分值：其他值为默认值 或者 NULL
INSERT INTO friends (id, name)
VALUES (2, 'Win');	-- id, name, age, height
```



## 5 删

```sql
DELETE FROM friends
WHERE id = 2;

DROP TABLE <table file>


/* 删除column */
ALTER TABLE playist
DROP length;
```







# 操作3|遍历 排序 统计



## 1 遍历(迭代)





## 2 排序

```sql
SELECT * FROM family ORDER BY id DESC; -- DESC: 降序; ASC: 升序(默认ASC)
```





## 3 统计

```sql
/* DISTINCT */
SELECT DISTINCT age FROM family;	-- id不能有重复值，但是其他columns可以有重复值


/* LIMIT 设定显示数量的限制 */
SELECT * FROM family LIMIT 2;


/* BETWEEN */
SELECT * FROM friends
WHERE age BETWEEN 0 AND 99;	--按照表格中原始的顺序排列
WHERE age NOT BETWEEN 0 AND 99;


/* IN */
SELECT * FROM friends
WHERE age IN ('75', '20');	--也可直接用 IN(75, 20); 单个值比如city = 'London' 等价于city IN 'London'
WHERE age NOT IN ('75', '20');


/* NULL */
SELECT * FROM friends
WHERE age IS NULL;
WHERE age IS NOT NULL;
```





```sql
/* 从句不接受统计aggregate函数 */
SELECT AVG(age) FROM people;
```





# 操作补充|运算符

```sql
/* assignment */
SELECT * FROM friends
SET age = 24;

/* comparison */
SELECT * FROM friends
WHERE age = 24
<
>
!=
!>
!<

/* logical */ 
AND NOT OR
```



## 小结|否定运算符

```sql
/* NOT */
NOT IN

/* ! */
!=
```



操作补充|函数

```sql
/* aggregate FUNC(col_name) */
SELECT COUNT(*) FROM people;
SELECT MAX(age) FROM people;
SELECT MIN(age) FROM people;
SELECT SUM(age) FROM people;
SELECT AVG(age) FROM people;





/* SCALAR FUNC(col_name) 不涉及到整个column, 只是基于input */ 
SELECT ROUND(AVG(age)) FROM people;	--默认向上取整

SELECT LENGTH(email) FROM people;

SELECT first_name, UCASE(last_name) FROM people;
```





# 属性|限制ALTER

```sql
/* 创建初始化：限制数据类型 */
CREATE TABLE playlist（
	id INT NOT NULL,
	song_title TEXT NOT NULL,
	length INT,
	album_id INT);
	
	
/* 设置属性：设置DEFAULT值 */
ALTER TABLE playist
ADD played BOOLEAN DEFAULT true;
   
   
/* 设置属性：设置主键 */
ALTER TABLE playist
ADD PRIMARY KEY(id)
	
	
/* 设置属性：改变数据类型 */
ALTER TABLE albums
ALTER COLUMN artist SET NOT NULL;

	
/* UNIQUE：不能重复 */	
ALTER TABLE playlist
ADD UNIQUE(id);


/* CHECK：符合特定条件 */	
ALTER TABLE playlist（
	ADD CONSTRAINT length	--length是一个字段field
	CHECK (length >= 3);
```





# 表格联合

- PRIMARY KEY主键只有一个
  - 是一个keyword关键字
  - table中的每一条record都有一个
  - 是独一无二的identifier
- FOREIGN KEY可以有多个，甚至可以是empty field
  - 另一个table的主键

```sql
CREATE TABLE albums(
	id INT PRIMARY KEY,	--主键
    name TEXT,
    year INT,
    artist TEXT);
    
/* 主表格：设置FOREIGN KEY */
ALTER TABLE playist
ADD FOREIGN KEY(album_id)	--外键与另一TABLE相联系
REFERENCES albums(id)
```







# #常见流程

```sql
-- 示例：创建 + 查询
CREATE TABLE team(
	number INT NOT NULL,
    name VARCHAR(20)
);

INSERT INTO team
VALUES (23, 'Michael');

SELECT number, name
FROM team
WHERE number =23;



-- 示例：年龄范围 + 排序
SELECT name 
FROM people
WHERE age BETWEEN 10 AND 30
ORDER BY name;


-- 示例：特异值 + 排序 + 限制数量
SELECT DISTINCT city FROM people
ORDER BY city
LIMIT 5;


-- 示例：选择最后一个元素 => 排序 + 限制数量
SELECT * FROM people 
LIMIT 1
ORDER BY id DESC;

```

