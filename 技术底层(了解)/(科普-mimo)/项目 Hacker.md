信条：“Everything has a weakness.”



# what

hacking：找到计算机或者网络的漏洞

- 作为挑战的成就感
- 偷取数据
- 好奇心驱动



分类：

| 分类      |                       |
| --------- | --------------------- |
| black hat | chaos, money, revenge |
|           |                       |
|           |                       |



编程：

- 搜寻不安全的网页
- cracking password破解密码
- 击溃网络安全



# database

感兴趣的数据：

- 密码
- 社保安全号
- 邮件地址

## SQL





# 密码学

## caesar cipher凯撒密码

```python
# 密码移位
a-b-c 对应 b-c-d
```



## Atbash cipher倒序

```python
a-b-c 对应 z-y-x
```



## polybius square

二维矩阵：两个两个数字为一组 => 对应一个字母





## rail fence cipher

W型走向：

```python
# 原文：LET US HACK
L...S...K
.E.U.H.C.
..T...A..
```





# 加密方法

## SHA-1

- 长度：40个字符，比如21232f297a57a5a743894a0e4a801fc3



### MD5



### bcrypt、SHA-256: 更安全





# 攻击方法

## dictionary attack

限制方法：

- 登录表格时使用CAPTCHA验证码
- 限制尝试登陆的次数



## SQL注入injection

针对对象：

- contact form
- checkout form
- login form
- registration form



频繁的原因：

- 很直接
- 许多网站使用SQL数据库
- 有许多自动攻击的脚本



作用：

- 下载所有数据
- 成为数据库的管理员
- 篡改数据



### 使用`'`

```sql
-- 表格填写
Username: ' OR '1'='1

-- output
SELECT * FROM users WHERE username = '' OR '1'='1'	--相当于所有数据
```



```sql
-- 表格填写
Username: ''; DROP TABLE payment

-- output
SELECT * FROM users WHERE username = '';
DROP TABLE payment
```



```sql
SELECT *
FROM users
WHERE email = 'x' OR '1' = '1';	--判断条件永远正确


--挑战1
x'; UPDATE users SET id = 123 WHERE email_address = 'frank@dark.net

--挑战2
x'; UPDATE users SET id = 456 WHERE user_name = '%Alice%	--包含Alice字符

--挑战3
x'; INSERT INTO users ('email_address') VALUES ('me@me.com'); --
```





### 使用`--`



### sanitizing消毒

- 保证输入不含某些特定字符
- 拒绝空白符
- 使用re验证数据的有效性





### 应对|parameterized/prepared SQL

参数：指的是占位符placeholder

```sql
SELECT * FROM users WHERE username = @USERNAME
```





## path traversal

利用某些文件名进行网页的字符串查询 => private files：PDF, CGI, ZIP, TXT

- 服务器配置
- 支付数据
- 员工医疗记录

```python
domain.com/?file=../passwords.txt

xcorp.com/?get=../../../staff.csv
```



避免的有效方法：

- 存储到另一个服务器
- 存储在当前服务器的安全部分



`../`可以通过限制系统操作的权限的避免



- 路径中不合法字符模式的消毒
- 不依靠用户文件名的输入
- 更新服务器的OS



## 网络监听

- 不安全的wifi点
  - 非常奇怪的WiFi定位
  - 保证快速WiFi
  - 频繁掉线
- 使用sniffer来intercept侦听流量 => HTTP明文传输
  - 监测网络流量
  - 监测网络漏洞
  - 查看明文请求
- 在PC上安装恶意软件malware





## 社会工程学

假扮：

- 警官
- 公司同事
- PC技术支持

```python
#方式1
Please supply your password here.

#方式2
There has been a policy change.
For verification, your employee ID is 3823 and your SSN is...
```





pretexting|编故事：

- Email地址
- 生日
- 银行名字



phishing网络钓鱼

- 拒绝用户的PIN输入 => 查看对方输入不同的PIN





# 登录

## session ID

比如7AD4VtmWjb

- length
- random
- unique

```python
Trying 87326521 - HTTP 500 ERR
Trying 87326522 - HTTP 404 MISSING
Trying 87326523 - HTTP 301 MOVED
Trying 87326524 - HTTP 200 OK	#这个是合适的session ID
Trying 87326525 - HTTP 403 FORBID
```



通常由web框架生成：

- 防止session ID的重复使用
- 20min后失活session ID
- 在每一次请求时更改session ID







## magic cookie

cookies签名：防止篡改session ID

- user的授权
- session信息



如何签名：在cookie数据后面添加一串编码的文本 => crytography





# Project

`测试网站：https://coindex.co/`

- 开发者工具
- 查看网页源代码

1 在email和password末尾都添加'

- `hack@getmimo.com'`

- `password'`

一切正常



2 点击忘记密码：

- `hack@getmimo.com'` => 暴露服务器问题

如果末尾没有'，则可以正常显示

![image-20210119124412794](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210119124412.png)