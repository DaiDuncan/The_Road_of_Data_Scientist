# cmd | 上下级目录

 

### [转载链接](https://blog.csdn.net/chivalrousli/article/details/38701707)

## 表示上级目录



- `../`表示源文件所在目录的上一级目录
- `../../`表示源文件所在目录的上上级目录，以此类推。



## 表示下级目录



引用下级目录的文件，直接写下级目录文件的路径即可。



- 绝对路径：是从盘符开始的路径



> 形如 `C:/windows/system32/cmd.exe`



- 相对路径：是从当前路径开始的路径



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
> 
>
> 当前路径如果为`c:/program files/common files` 则需要输入
>
> ```
> ../../windows/system32/cmd.exe
> ```



- 另外，还有一种不包含盘符的特殊绝对路径



> 形如`/windows/system32/cmd.exe`
>
> 无论当前路径是什么，会自动地从当前盘的根目录开始查找指定的程序。