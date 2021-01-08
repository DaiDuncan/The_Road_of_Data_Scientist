# cmd | 常用命令

转载链接：https://www.jianshu.com/p/80c3ac7bea8f



> pwd 当前路径
>
> cd 当前路径
>
> cd path 切换路径
>
> 盘符:切换到盘符根目录
>
> cls清屏
>
> dir显示当前目录下或指定目录下文件列表
>
> del删除文件
>
> erase删除至少一个文件
>
> rd删除目录
>
> rmdir删除目录
>
> date显示或设置日期
>
> time显示或设置时间
>
> copy复制文件内容到另一个文件
>
> move将文件从一个目录移动到另一个目录
>
> echo str回显内容
>
> echo str > file 写入到文件，可以用来创建文本文件
>
> exit退出cmd
>
> ctrl+c终止当前任务
>
> md创建目录
>
> mkdir创建目录
>
> path显示或设置环境配置
>
> ren重命名文件
>
> replace替换文件
>
> title设置cmd当前回话标题
>
> type显示文本文件内容
>
> logoff注销
>
> shutdown -f -s -t seconds 关机 强制 执行关机 倒计时 s秒
>
> shutdown -a 关机 取消
>
> at time shutdown -s 定时 执行某个任务/可设置周期执行任务
>
> at /delete 清除at定时任务
>
> tasklist进程列表
>
> tasklist|findstr pid 查看指定进程pid的进程信息
>
> taskkill -f -im/-pid taskNameOrID -t 杀死 强制 按镜像名/按id 任务名或id 及子任务
>
> netstat -aon查看端口占用
>
> netstat -aon|findstr port 查看指定端口占用情况，找到占用端口的进程pid



- dir显示目录中的文件和子目录列表



```
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



- cd更改当前目录



```
cd mp3  #进入当前目录中的mp3目录
cd ..   #进入当前目录中的上级目录
cd/     #进入根目录
cd      #显示当前目录
cd /d d:/mp3 #可以同时更改盘符和目录
```



- md创建目录



```
md abc              #在当前目录里建立子目录 abc
md d:/a/b/c         #如果 d:/a 不存在，将会自动创建
```



- rd删除目录



```
rd abc              #删除当前目录里的 abc 子目录，要求为空目录
rd /s/q d:/temp     #删除 d:/temp 文件夹及其子文件夹和文件，不需要按 Y确认
```



- del删除文件



```
del d:/test.txt     #删除指定文件，不能是隐藏、系统、只读文件
del *.*删除当前目录里的所有文件，不包括隐藏、系统、只读文件，要求按 Y 确认
del /q/a/f d:/temp/*.*删除 d:/temp 文件夹里面的所有文件，包括隐藏、只读、系统文件，不包括子目录
del /q/a/f/s d:/temp/*.*删除 d:/temp 及子文件夹里面的所有文件，包括隐藏、只读、系统文件，不包括子目录
```



- ren文件重命名



```
ren 1.txt 2.bak     #把 1.txt 更名为 2.bak
ren *.txt *.ini     #把当前目录里所有.txt文件改成.ini文件
ren d:/temp tmp     #支持对文件夹的重命名
```



- type显示文件内容



```
type c:/boot.ini    #显示指定文件的内容，程序文件一般会显示乱码
type *.txt          #显示当前目录里所有.txt文件的内容
```



- copy拷贝文件



```
copy c:/test.txt d:/复制 c:/test.txt 文件到 d:/
copy c:/test.txt d:/test.bak复制 c:/test.txt 文件到 d:/ ，并重命名为 test.bak
copy c:/*.*复制 c:/ 所有文件到当前目录，不包括隐藏文件和系统文件不指定目标路径，则默认目标路径为当前目录
copy con test.txt从屏幕上等待输入，按 Ctrl+Z 结束输入，输入内容存为test.txt文件con代表屏幕，prn代表打印机，nul代表空设备
copy 1.txt + 2.txt 3.txt合并 1.txt 和 2.txt 的内容，保存为 3.txt 文件如果不指定 3.txt ，则保存到 1.txt
copy test.txt +复制文件到自己，实际上是修改了文件日期
```



- ver显示系统版本label 和 vol设置卷标



```
vol #显示卷标
label #显示卷标，同时提示输入新卷标
label c:system #设置C盘的卷标为 system
```



- date 和 time日期和时间



```
date          #显示当前日期，并提示输入新日期，按"回车"略过输入
date/t        #只显示当前日期，不提示输入新日期
time          #显示当前时间，并提示输入新时间，按"回车"略过输入
time/t        #只显示当前时间，不提示输入新时间
```



- find (外部命令)查找命令



```
find "abc" c:/test.txt在 c:/test.txt 文件里查找含 abc 字符串的行如果找不到，将设 errorlevel 返回码为1
find /i "abc" c:/test.txt查找含 abc 的行，忽略大小写
find /c "abc" c:/test.txt显示含 abc 的行的行数
```



- more (外部命令)逐屏显示



```
more c:/test.txt    #逐屏显示 c:/test.txt 的文件内容
```



- tree显示目录结构



```powershell
tree d:/            #显示D盘的文件目录结构
```