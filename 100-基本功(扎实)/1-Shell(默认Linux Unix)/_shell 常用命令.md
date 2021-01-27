$：shell prompt提示符

## 术语

| 术语        |                                |
| ----------- | ------------------------------ |
| file        |                                |
| dir         | directory                      |
| src         | source                         |
| dest        | destination                    |
| target      |                                |
| > <output>  |                                |
| >> <output> | 追加内容                       |
| < <input>   | $ wc < <file> 显示file的wc属性 |
|             |                                |
|             |                                |



## Linux文件目录结构

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1592923349367-be674a3b-c70e-4c51-96d8-2a86467d9187.png)

/：存放**系统程序**，也就是At&t开发的Unix程序。

/usr：存放Unix系统商（比如IBM和HP）开发的程序。

/usr/local：存放**用户自己安装**的程序。

/opt：在某些系统，用于存放第三方厂商开发的程序，所以取名为option，意为"选装"。

---

# 目录与路径

- pwd
- cd
- mkdir
- touch
- ls -a -l -t

```shell
$ pwd 			#print working directory显示当前工作目录
$ cd <file>  	#默认在当前工作目录：等价于cd ./<file>

### 创建目录和文件
$ mkdir <dir> 	#创建目录：make dir
$ touch <file>  #创建文件

### ls
# ls -a 显示隐藏文件 该文件前缀为.
# ls -l 文件属性long-form：权限r/w, 修改时间, 
# ls -t 显示最近修改的文件
# ls -alt 三者结合
# 两者结合：-lt与-tl两者等价；-al也可两者结合
# ls <filename> 返回所有同名文件
$ ls <dir> 	#文件目录directory
```





## #常见流程

```shell
$ cd <path of dir>
$ mkdir <new dir>
$ cd <new dir>
$ touch <new file>
```







# 目录/文件操作⭐

- cp
- mv
- rmdir
- rm
- tar 压缩
- innode

```shell
### cp 拷贝copy
# 1如果是隐藏文件：file含前缀.
# 2如果在dir中存在同名文件，则覆盖
$ cp <file> <dest of dir>

* #匹配任意字符


### mv 移动move
$ mv <file> <dest of dir>


### rm 删除remove
$ rmdir		#删除空的dir
$ rm <file(s)>
$ rm -rf	#删除所有文件
```



## tar

```shell
$ mv yum.log /tmp #一般把文件放入/tmp当做删除，而不直接使用rm

$ mkdir –p /a/b/c     #递归创建目录

$ grep max_children php-fpm.log   #抓取文件中的特定字符：grep+字符+文件
$ grep –n max_children php-fpm.log        #显示行数
$ vi php-fpm.log +65      #进入到文件的第65行进行编辑

$ tar zcvf demo.tar.gz ./* #把当前目录的所有文件打包成demo.tar.gz
$ tar zxvf demo.tar.gz ./* #解压demo.tar.gz文件
```



## innode

索引节点是文件在文件系统的唯一标识。

要读取文件，必须要经过目录记录的文件名来指向到正确的inode，进而找到block的位置：

文件名 -> inode -> device block

```shell
$ ls -i text.txt          #查看文件的inode号码
$ stat text.txt           #查看文件的inode信息
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



## 软链接和硬链接

### 1）硬链接Hard Link

- ln [源文件] [目标文件]
- 实际链接与文本**共用同一个inode**，即多个文件名指向同一个inode号码
- 创建之后**链接数会加1**
- 删除实际链接或文本文件**都不会影响文件本身**（删除一个文件名，不影响另一个文件名的访问，除非两个都删除）
- 不能链接到目录（只针对文件有效）
- 不能**跨文件系统**使用链接
- 最大的好处是==安全，相当于做文件备份==
- 创建目录时，默认会生成两个目录项："."和".."。前者的inode号码就是当前目录的inode号码，等同于当前目录的"硬链接"；后者的inode号码就是当前目录的父目录的inode号码，**等同于父目录的"硬链接"**。所以，任何一个目录的"硬链接"总数，总是等于2加上它的子目录总数（含隐藏目录），这里的2是父目录对其的“硬链接”和当前目录下的".硬链接“。



### 2）软连接Soft Link/符号连接Symbolic Link：

- ln -s [源文文件或目录] [目标文件或目录]
- 软连接就和**windows里的快捷方式**差不多







# 重定向|input output

定义：改变inputs和outputs

- echo
- cat
- wc -l -w -c
- <
- \>
- \>\>
- |

```shell
$ echo <str> 			#单纯在console上显示字符
$ echo <str> > <file>   #将字符output到文件


$ cat <file>			#显示文件内容
$ cat < <file>			#file作为cat的输入：与上面的表达式等价


### wc默认显示-lwc
$ wc -l -w -c <file>	#分别统计显示lines, words, characters; 最后还有filename
$ wc < <file>			#file作为input, 显示该file的属性


### 重定向> <file>
wc -l readme.txt > word_count.txt
cat word_count.txt


### 追加内容
$ echo <str> >> favorite_things.txt


### 管道
$ echo <str> | wc -c	#<input> | 另一个命令：左边作为另一个命令的input
```



## #常见流程

```shell
$ ls
# 显示output: readme.txt
$ touch trashme.txt
$ cat readme.txt > trashme.txt	#cat显示内容，直接重定向> output到trashme.txt中
$ cat trashme.txt				#在console中显示


### 显示/输出到另一个文件中
# 重定向> <file>
$ wc -l readme.txt > word_count.txt
$ cat word_count.txt



### 追加内容
$ echo <str1> > <file>
$ echo <str2> >> <file>
cat <file> | wc -w
```





## cat

```shell
$ cat /etc/passwd
# Username : Password : Userid : Groupid : Comment : Home_directory : Shell
# - Username：唯一标识，一个用户账号，用户在登录时使用的就是它
# - Password：passwd中的密码是加密的，几乎不能被破解
# - Userid：Linux内部使用userid来标识用户。**0是系统管理员**，1-499是系统保留账号，500+是一般账号
# - Groupid：不同用户可属于同一用户组，具有用户组共有的权限


# 通过usermod –g [组名] [用户名]来修改
# - Comment：用于给账号做注释，可为空
# - Home_directory：主目录，这个目录属于该账号，root的主目录是/root，其他账号的主目录在/home下
# - Shell：登录使用的shell，就是对登录命令进行解析的工具

$ cat /proc/version       #查看正在运行的内核版本
$ cat /etc/issue      #显示发行版本的相关信息 
$ cat /etc/passwd
```

**注**：系统中还有一些默认的账号，如daemon、bin等这些账号有特殊用途，一般用于系统管理，其口令部分大部分用x表示，表示他们不能在登录时使用。







# 权限

意义：文件需要用户权限(编辑)

- whoami
- su
  - sudo
- chgrp
- chown
- chmod

## [Link: 博客|详解Linux权限](https://xwjgo.github.io/2016/11/05/linux%E6%96%87%E4%BB%B6%E6%9D%83%E9%99%90/)

Linux的文件权限：

- **基本权限**
- **默认权限**
- **特殊权限**



用户与用户组：

- 文件所有者（user）
- 用户组（group）
- 其他人（others）

linux中所有的用户账号，包括root相关信息，都记录在`/etc/passwd`文件内。

至于用户账号的密码，则是记录在`/etc/shadow`这个文件中。

而所有的用户组信息则记录在`/etc/group`这个文件内



## [Link: 详解ls -l](https://www.jianshu.com/p/1c49bd94f6fd)

<img src="https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210114114945.png" alt="image-20210114114945485" style="zoom:80%;" />

1. 第一列代表这个文件的类型与权限
2. 第二列表示有多少文件名连接到此节点
3. 第三列表示这个文件（或目录）的“所有者账号”
4. 第四列表示这个文件的所属用户组
5. 第五列为这个文件的容量大小，默认单位为B
6. 第六列为这个文件的创建文件日期**或者**是最近的修改日期
7. 第七列为该文件名

```shell
### 如何改变文件属性与权限?	
$ chgrp [用户组] [文件名]					#改变所属用户组
$ chown [-R] [账号名称]:[用户组] [文件名]	 #改变文件所有者
$ chmod		#改变文件的权限 1数字 2符号类型
```



```shell
$ whoami	#显示当前user名称

# ls -l 显示文件权限reading, writing, executing
# -rwxr--r-- 10个字符：
#1 如果是d：表示dir, -则表示file
#2-4 rwx指向user
#5-7 r--指向group
#8-10 r--执行others


### 改变权限change mode
$ chmod g+w notes.txt	#增加w权限
$ ls -l


### superuser或者说root：拥有最高权限 => 需要root password
# open <file> 如果没有权限，会显示LSOpenURLWithRole() failed with error -5000之类的
$ su	#进入root模式
$ sudo rm <file>	#原本没有对file的写权限，sudo表示在su模式下的操作

```





## #常见流程

```shell
### 示例
$ ls -l	#-rwxr--r-- 1 user staff 42 Jun 24 22:00 readme.txt 表示owner(user)和group(staff)

###
$ cd Movies						#更改到Movies目录
$ chmod u-w hellofriend.mov		#去除user的write权限
$ chmod o+w hellofriend.mov		#增加other的write权限
$ ls -l hellofriend.mov			#显示文件的权限



### 添加用户并增加sudo权限
$ useradd morra #增加用户morra
$ visudo
$ #找到root ALL=(ALL) ALL，在下面添加一行morra ALL=(ALL) ALL
```

添加用户并增加sudo权限：

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1592923227607-e905fbd0-a39f-4b09-ac18-f1cf33782c4a.png)





# 环境

- enc
- echo $VAR
- which <command><br>export <VAR>=XXX





内容分为三大项：

- user name
- 工作目录
- 语言

```shell
$ echo $USER
$ echo $PATH	#特殊的环境变量：可执行文件的路径汇总(必须大写)
$ echo $HOME	#HOME是环境变量名称


### 显示所有的环境变量
$ env	#返回key-value


### 命令所在路径
$ which echo


### 设置环境变量
$ export <VAR>=XXX	#只会在这个session改变，退出终端后，恢复为默认值
$ export PATH=$PATH:/Users/user	#给环境变量PATH设置为路径user


### 持久修改环境变量(保存)
# 重定向输出到配置文件
$ echo "export PATH=$PATH:/User/user" >> ~/.bash_profile
```





## #常见流程

```shell
$ export API_KEY=123	#设置环境变量API_KEY的值
$ export API_KEY=987	#更改环境变量API_KEY的值
$ echo $API_KEY
$ echo "export $API_KEY" >> ~/.bash_profile	#保存：更改后环境变量API_KEY的值
```





# 系统及其进程管理

- ps<br/>ps x
- uname
- lsb_release
- df
- free
- top

```shell
### 进程
$ ps		#当前session的进程
$ ps x		#所有active的进程

$ uname –a	#显示电脑以及操作系统的相关信息


$ lsb_release –a      #显示LSB和特定版本的一些信息
$ lsb_release –h      #查看帮助

$ df –h #查看磁盘使用情况

$ free –m #以m单位显示服务器内存的使用情况

$ top #动态显示进程信息，q退出
```

grep

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1592922850263-397e305b-24f8-458a-b5a4-7fdc3ffd536a.png)

free -m

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1592922859766-97b08a6d-a3c7-46de-b3db-0f3a8dbe0a0a.png)





# 基本操作1|查(改增删)

- grep  #返回内容行
- find  #返回文件名
- cat => less

```shell
### 查找文件
# grep输出txt文件中包含改文本的行(如果某文件 存在该文本)
$ grep <text/str> <files>
$ grep Mimo *.txt		#在所有的.txt文件中，查找文本Mimo

# find最后返回文件名
$ find . <text>			#返回当前目录中，包含text的所有文件名
$ find . -name "*.txt"	#.表示当前目录；-name属性表示文件名



### 显示文本内容
# less: 相比于cat更加适合长文本 => 能滚动浏览，Q键退出
$ less favorite_thins.txt
```





## find

```shell
find / -name "yum.log"        	  #精确查找
find / -name "\*yum*"         	  #模糊查找1
find /var/log -name "*.log" 	  #模糊查找2
find / -size +10M              	  #查找大于10M的文件
find / -size +10M | xargs ls –lh  #使查询结果进行二次ls显示
find / -type f/d/c/b/s/l          #普通文件/目录/字符串/块设备/socket/链接
```











# 基本操作2|遍历 排序 统计

- head<br/>tail
- sort
- uniq

```shell
$ head -<number of lines> <file>	#显示文本文件开头的行，默认行数为10
$ tail -<number of lines> <file>	#显示文本文件结尾的行


### sort排序
$ sort <file>
$ sort fruits.txt


### uniq特异性
$ uniq <file>
```





## #常见流程

```shell 
$ tail -3 friends.txt | sort
```







# 附加功能

- man <command>

```shell
### 查找手册：相当于help(func)
$ man ls	#manual
```



## #常见流程











# #参考文献

[cnblogs | Linux基础](https://www.cnblogs.com/whatisfantasy/p/5941798.html)