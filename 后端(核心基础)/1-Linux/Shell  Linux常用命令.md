# Shell | Linux常用命令

**声明**：以下内容主要基于[cnblogs | Linux基础](https://www.cnblogs.com/whatisfantasy/p/5941798.html)，根据理解做了些许改动。



### find

```
find / -name "yum.log"          #精确查找
find / -name "\*yum*"               #模糊查找1
find /var/log -name "*.log" #模糊查找2
find / -size +10M                     #查找大于10M的文件
find / -size +10M | xargs ls –lh        #使查询结果进行二次ls显示
find / -type f/d/c/b/s/l        #普通文件/目录/字符串/块设备/socket/链接
```

### cat

- cat /etc/passwd

Username : Password : Userid : Groupid : Comment : Home_directory : Shell

- - Username：唯一标识，一个用户账号，用户在登录时使用的就是它
  - Password：passwd中的密码是加密的，几乎不能被破解
  - Userid：Linux内部使用userid来标识用户。**0是系统管理员**，1-499是系统保留账号，500+是一般账号
  - Groupid：不同用户可属于同一用户组，具有用户组共有的权限

通过usermod –g [组名] [用户名]来修改

- - Comment：用于给账号做注释，可为空
  - Home_directory：主目录，这个目录属于该账号，root的主目录是/root，其他账号的主目录在/home下
  - Shell：登录使用的shell，就是对登录命令进行解析的工具

**注**：系统中还有一些默认的账号，如daemon、bin等这些账号有特殊用途，一般用于系统管理，其口令部分大部分用x表示，表示他们不能在登录时使用。





### uname

### lsb_release

### df

### free

### top

```
cat /proc/version       #查看正在运行的内核版本
cat /etc/issue      #显示发行版本的相关信息 
cat /etc/passwd

uname –a        #显示电脑以及操作系统的相关信息


lsb_release –a      #显示LSB和特定版本的一些信息
lsb_release –h      #查看帮助


df –h #查看磁盘使用情况


free –m #以m单位显示服务器内存的使用情况


top #动态显示进程信息，q退出
```

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1592922850263-397e305b-24f8-458a-b5a4-7fdc3ffd536a.png)

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1592922859766-97b08a6d-a3c7-46de-b3db-0f3a8dbe0a0a.png)





### mv

### mkdir

### grep

### tar

```
mv yum.log /tmp #一般把文件放入/tmp当做删除，而不直接使用rm

mkdir –p /a/b/c     #递归创建目录

grep max_children php-fpm.log   #抓取文件中的特定字符：grep+字符+文件
grep –n max_children php-fpm.log        #显示行数
vi php-fpm.log +65      #进入到文件的第65行进行编辑

tar zcvf demo.tar.gz ./* 把当前目录的所有文件打包成demo.tar.gz
tar zxvf demo.tar.gz ./* 解压demo.tar.gz文件
```





### 添加用户并增加sudo权限

1. useradd morra #增加用户morra
2. visudo
3. 找到root ALL=(ALL) ALL，在下面添加一行morra ALL=(ALL) ALL

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1592923227607-e905fbd0-a39f-4b09-ac18-f1cf33782c4a.png)





### 重定向

```
> ：会覆盖源文件
>> ：不会覆盖，只会向后追加
```





### inode

索引节点是文件在文件系统的唯一标识。

要读取文件，必须要经过目录记录的文件名来指向到正确的inode，进而找到block的位置：



文件名 -> inode -> device block

```
ls -i text.txt          #查看文件的inode号码
stat text.txt           #查看文件的inode信息
```

- inode包含文件的元信息

- - 文件的字节数
  - 文件拥有者的User ID
  - 文件的Group ID
  - 文件的读、写、执行权限
  - 文件的时间戳，共有三个：

- - - Change指inode上一次变动的时间
    - Modify指文件内容上一次变动的时间
    - Access指文件上一次打开的时间。

- - 链接数，即有多少文件名指向这个inode
  - 文件数据block的位

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1592923280751-09e8216a-0ded-4418-aef9-0bdce02e20b4.png)

- inode特殊用法

- - 有时，文件名包含特殊字符，无法正常删除。这时，直接删除inode节点，就能起到删除文件的作用。
  - 移动文件或重命名文件，只是改变文件名，不影响inode号码。
  - 打开一个文件以后，系统就以inode号码来识别这个文件，不再考虑文件名。因此，通常来说，系统无法从inode号码得知文件名。这使得软件更新变得简单，可以在**不关闭软件的情况下进行更新**，不需要重启。因为系统通过inode号码，识别运行中的文件，不通过文件名。更新的时候，新版文件以同样的文件名，生成一个新的inode，不会影响到运行中的文件。等到下一次运行这个软件的时候，文件名就自动指向新版文件，旧版文件的inode则被回收。





### 软链接和硬链接

#### 1）硬链接Hard Link

- ln [源文件] [目标文件]
- 实际链接与文本**共用同一个inode**，即多个文件名指向同一个inode号码
- 创建之后**链接数会加1**
- 删除实际链接或文本文件**都不会影响文件本身**（删除一个文件名，不影响另一个文件名的访问，除非两个都删除）
- 不能链接到目录（只针对文件有效）
- 不能**跨文件系统**使用链接
- 最大的好处是安全，相当于做文件备份
- 创建目录时，默认会生成两个目录项："."和".."。前者的inode号码就是当前目录的inode号码，等同于当前目录的"硬链接"；后者的inode号码就是当前目录的父目录的inode号码，**等同于父目录的"硬链接"**。所以，任何一个目录的"硬链接"总数，总是等于2加上它的子目录总数（含隐藏目录），这里的2是父目录对其的“硬链接”和当前目录下的".硬链接“。



#### 2）软连接Soft Link/符号连接Symbolic Link：

- ln -s [源文文件或目录] [目标文件或目录]
- 软连接就和**windows里的快捷方式**差不多



Linux文件目录结构

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1592923349367-be674a3b-c70e-4c51-96d8-2a86467d9187.png)



/：存放**系统程序**，也就是At&t开发的Unix程序。

/usr：存放Unix系统商（比如IBM和HP）开发的程序。

/usr/local：存放**用户自己安装**的程序。

/opt：在某些系统，用于存放第三方厂商开发的程序，所以取名为option，意为"选装"。

## 参考文献

[cnblogs | Linux基础](https://www.cnblogs.com/whatisfantasy/p/5941798.html)