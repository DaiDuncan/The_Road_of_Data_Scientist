```shell
rm -rf 
```

这个命令，恐怕是这个世界上最危险的命令，在每一次程序员删库跑路的事件中都扮演着关键角色。

在日常工作中，一不留神，就可能因一时疏忽而误删除了关键文件导致服务器出现故障或是服务不可用。

- 由于linux系统没有回收站功能，这导致使用rm删除的文件很难恢复。





trash-cli是一个实现了回收站功能的python库，使用它，你可以放心的执行rm命令而不必担心误删除的数据无法恢复

安装结束后，你可以使用which trash 来查看工具的安装目录

- 在我的机器上，安装目录是/opt/conda/bin 
- 使用ll /opt/conda/bin/trash* 命令可以查看到所有相关命令

```shell
which trash

/opt/conda/bin/trash                    # 删除文件， 同trash-put
/opt/conda/bin/trash-empty              # 清空回收站
/opt/conda/bin/trash-list               # 列出回收站里的文件
/opt/conda/bin/trash-put                # 删除文件
/opt/conda/bin/trash-restore            # 恢复回收站里的指定文件
/opt/conda/bin/trash-rm                 # 删除回收站里的指定文件
```





可以使用trash命令代替rm命令，更好的方法是设置rm命令的别名，修改.bashrc文件，增加下面这行

```text
alias rm="trash"
```

设置以后，记得执行source .bashrc 使配置生效，现在，你可以放心的使用rm命令了

当你想恢复某个文件时，执行trash-list ==列出回收站中的文件==，使用trash-restore 恢复你想要恢复的文件。



# 实现原理

## 被删除的文件去哪了

默认情况下，这些文件都被放在了 $HOME/.local/share/Trash 目录下：

- 两个文件夹，分别是files 和info
- files目录下存放的就是被删除的文件
- info目录下存放的是被删除文件的信息，包括被删除前所在目录和被删除时间

```text
[Trash Info]
Path=/home/jovyan/server.py
DeletionDate=2020-06-15T11:30:58
```

每一个被删除的文件或文件夹，都会有一个与之相对应的trashinfo文件，记录着被删除文件的关键信息。当使用trash-restore恢复文件时，就是根据这些信息将文件move到指定位置。



## 回收站的目录是否可设置

默认是`$HOME/.local/share/Trash` ，但可以进行修改，这一点，源码里说的很清楚

```python
class HomeTrashCan:
    def __init__(self, environ):
        self.environ = environ
    def path_to(self, out):
        if 'XDG_DATA_HOME' in self.environ:
            out('%(XDG_DATA_HOME)s/Trash' % self.environ)
        elif 'HOME' in self.environ:
            out('%(HOME)s/.local/share/Trash' % self.environ)
```

如果你希望更改回收站的目录，那么可以通过设置`XDG_DATA_HOME`环境变量来实现