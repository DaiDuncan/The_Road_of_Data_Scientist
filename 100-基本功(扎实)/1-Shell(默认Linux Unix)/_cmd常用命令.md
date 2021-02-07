# cmd | 常用命令

# 文件路径

```shell
# 注意：windows平台/ (而git是\)

pwd  #当前路径
cd  #当前路径
cd path #切换路径
盘符  #切换到盘符根目录
```





## 上下级目录

[转载链接](https://blog.csdn.net/chivalrousli/article/details/38701707)

### 表示上级目录

```shell
../   	#表示源文件所在目录的上一级目录
../../  #表示源文件所在目录的上上级目录，以此类推。
```



### 表示下级目录

引用下级目录的文件，直接写下级目录文件的路径即可。



### 绝对路径|从盘符开始的路径

```shell
C:/windows/system32/cmd.exe
```





### 相对路径|从当前路径开始的路径

> 假如当前路径为`C:/windows`： 要描述上述路径，只需输入`system32/cmd.exe`
>
> 严格相对路径写法：`./system32/cmd.exe`
>
> 其中，`.`表示当前路径，在通道情况下可以省略，只有在特殊的情况下不能省略。
>
> 
>
> 假如当前路径为`c:/program files`
>
> 要调用上述命令，则要输入 `../windows/system32/cmd.exe`
>
> 其中，`..`为父目录
>

```shell
### 当前路径如果为`c:/program files/common files` 则需要输入
../../windows/system32/cmd.exe
```



### 不包含盘符的特殊绝对路径

> 形如`/windows/system32/cmd.exe`
>
> 无论当前路径是什么，会自动地从当前盘的根目录开始查找指定的程序。







# 目录与文件

## dir|显示目录中的文件和子目录列表

```shell
dir                 #显示当前目录中的文件和子目录
dir /a              #显示当前目录中的文件和子目录，包括隐藏文件和系统文件
dir c: /a:d         #显示 C 盘当前目录中的目录
dir c:/ /a:-d       #显示 C 盘根目录中的文件
dir d:/mp3 /b/p     #逐屏显示 d:/mp3 目录里的文件，只显示文件名，不显示时间和大小
dir *.exe /s        #显示当前目录和子目录里所有的.exe文件其中 * 是通配符，
                            #代表所有的文件名，还一个通配符 ? 代表一个
                            #不存在，将返回 errorlevel 为1;
# 每个文件夹的 dir 输出都会有2个子目录 . 和 ... 代表当前目录.. 代表当前目录的上级目录
dir .               #显示当前目录中的文件和子目录
dir ..              #显示当前目录的上级目录中的文件和子目录
```





## cd|更改当前目录

```shell
cd mp3  #进入当前目录中的mp3目录
cd ..   #进入当前目录中的上级目录
cd/     #进入根目录
cd      #显示当前目录
cd /d d:/mp3 #可以同时更改盘符和目录
```





## md|创建目录

```shell
md abc              #在当前目录里建立子目录 abc
md d:/a/b/c         #如果 d:/a 不存在，将会自动创建
```





## rd|删除目录

```shell
rd abc              #删除当前目录里的 abc 子目录，要求为空目录
rd /s/q d:/temp     #删除 d:/temp 文件夹及其子文件夹和文件，不需要按 Y确认
```





## del|删除文件

```shell
del d:/test.txt     #删除指定文件，不能是隐藏、系统、只读文件
del *.*删除当前目录里的所有文件，不包括隐藏、系统、只读文件，要求按 Y 确认
del /q/a/f d:/temp/*.*删除 d:/temp 文件夹里面的所有文件，包括隐藏、只读、系统文件，不包括子目录
del /q/a/f/s d:/temp/*.*删除 d:/temp 及子文件夹里面的所有文件，包括隐藏、只读、系统文件，不包括子目录
```





## ren|文件重命名

```shell
ren 1.txt 2.bak     #把 1.txt 更名为 2.bak
ren *.txt *.ini     #把当前目录里所有.txt文件改成.ini文件
ren d:/temp tmp     #支持对文件夹的重命名
```





## type|显示文件内容

```shell
type c:/boot.ini    #显示指定文件的内容，程序文件一般会显示乱码
type *.txt          #显示当前目录里所有.txt文件的内容
```





## copy|拷贝文件

```shell
copy c:/test.txt d:/复制 c:/test.txt 文件到 d:/
copy c:/test.txt d:/test.bak复制 c:/test.txt 文件到 d:/ ，并重命名为 test.bak
copy c:/*.*复制 c:/ 所有文件到当前目录，不包括隐藏文件和系统文件不指定目标路径，则默认目标路径为当前目录
copy con test.txt从屏幕上等待输入，按 Ctrl+Z 结束输入，输入内容存为test.txt文件con代表屏幕，prn代表打印机，nul代表空设备
copy 1.txt + 2.txt 3.txt合并 1.txt 和 2.txt 的内容，保存为 3.txt 文件如果不指定 3.txt ，则保存到 1.txt
copy test.txt +复制文件到自己，实际上是修改了文件日期
```





# 字符串

```shell
echo str  #回显内容
echo str > file #写入到文件，可以用来创建文本文件
```







# 内存

## ver|显示系统版本label 和 vol设置卷标

```shell
vol #显示卷标
label #显示卷标，同时提示输入新卷标
label c:system #设置C盘的卷标为 system
```







# 时间

## date和time|日期和时间

```shell
date          #显示当前日期，并提示输入新日期，按"回车"略过输入
date/t        #只显示当前日期，不提示输入新日期
time          #显示当前时间，并提示输入新时间，按"回车"略过输入
time/t        #只显示当前时间，不提示输入新时间
```







# 其他

## find|(外部命令)查找命令

```shell
find "abc" c:/test.txt在 c:/test.txt 文件里查找含 abc 字符串的行如果找不到，将设 errorlevel 返回码为1
find /i "abc" c:/test.txt查找含 abc 的行，忽略大小写
find /c "abc" c:/test.txt显示含 abc 的行的行数
```



## more|(外部命令)逐屏显示

```shell
more c:/test.txt    #逐屏显示 c:/test.txt 的文件内容
```



## tree|显示目录结构

```shell
TREE [drive:][path] [/F] [/A]
# /F 不区分大小写   显示每个文件夹中文件的名称(带扩展名)
# /A 使用 ASCII 字符，而不使用扩展字符。(如果要显示中文，例如 tree /f /A >tree.txt)

tree d:/            #显示D盘的文件目录结构

# 示例
tree /f >tree.txt	# 不带/f，只涉及文件夹(不涉及具体文件)
```







# #参考文献

[Link: 转载链接](https://www.jianshu.com/p/80c3ac7bea8f)

[Link: cmd root权限](https://blog.csdn.net/zyw_anquan/article/details/7756499)