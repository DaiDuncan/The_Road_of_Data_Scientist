Fabric 是一个 Python (2.5-2.7) 的库和命令行工具，用来提高基于 SSH 的应用部署和系统管理效率。

- 一个让你通过 **命令行** 执行 **无参数 Python 函数** 的工具；
- 一个让通过 SSH 执行 Shell 命令更加 **容易** 、 **更符合 Python 风格** 的命令库（建立于一个更低层次的库）。



大部分用户把这两件事结合着用，使用 Fabric 来写和执行Python函数或task ，以实现与远程服务器的自动化交互



示例

- 定义 fabfile 任务，并用 [*fab*](https://fabric-chs.readthedocs.io/zh_CN/chs/usage/fab.html) 执行；
- 用 [`local`](https://fabric-chs.readthedocs.io/zh_CN/chs/api/core/operations.html#fabric.operations.local) 调用本地 shell 命令；
- 通过 [`settings`](https://fabric-chs.readthedocs.io/zh_CN/chs/api/core/context_managers.html#fabric.context_managers.settings) 修改 env 变量；
- 处理失败命令、提示用户、手动取消任务；
- 以及定义主机列表、使用 [`run`](https://fabric-chs.readthedocs.io/zh_CN/chs/api/core/operations.html#fabric.operations.run) 来执行远程命令。

```python
from __future__ import with_statement
from fabric.api import *
from fabric.contrib.console import confirm

env.hosts = ['my_server']

def test():
    with settings(warn_only=True):
        result = local('./manage.py test my_app', capture=True)
    if result.failed and not confirm("Tests failed. Continue anyway?"):
        abort("Aborting at user request.")

def commit():
    local("git add -p && git commit")

def push():
    local("git push")

def prepare_deploy():
    test()
    commit()
    push()

def deploy():
    code_dir = '/srv/django/myproject'
    with settings(warn_only=True):
        if run("test -d %s" % code_dir).failed:
            run("git clone user@vcshost:/path/to/repo/.git %s" % code_dir)
    with cd(code_dir):
        run("git pull")
        run("touch app.wsgi")
```







# #参考文献

[Link: 教程|官网](https://fabric-chs.readthedocs.io/zh_CN/chs/tutorial.html)