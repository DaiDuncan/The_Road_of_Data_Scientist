类似cmd(pip)

```shell
# (自动提示)更新conda版本
$ conda update -n base -c defaults conda

#==# 查看已有环境
$ conda info --env
# conda environments:
#
base                     D:\JustPractice\1_MS_Geek_salute\PythonAnaconda
pytorch               *  D:\JustPractice\1_MS_Geek_salute\PythonAnaconda\envs\pytorch
tensorflow               D:\JustPractice\1_MS_Geek_salute\PythonAnaconda\envs\tensorflow


#==# 创建环境(初始化可自带python)
conda create --name python35 python=3.5	#代表创建一个python3.5的环境，命名为python35


#==# 激活环境
conda activate <python35>
conda deactivate	#退出当前环境

#==# 删除环境
conda remove -n <python35> --all
```



```shell
#==# 查看所有包
pip list #查看主目录下的包
conda list #查看当前环境的包

#==# 安装包
pip install <package>
conda install <package>
```



# me|环境管理

1 必要环境

| 目的                                                         | 目标环境                                                 | 目前     |
| ------------------------------------------------------------ | -------------------------------------------------------- | -------- |
| 基本操作：<br />1 测试专用<br />2 数据挖掘(包含numpy, pandas等必备库) | ⭐base                                                    | 358 pkgs |
| 专题：AI                                                     | ⭐pytorch(2021.05.17)                                     | 11 pkgs  |
|                                                              | ⭐tensorflow                                              | 99 pkgs  |
| 具体项目                                                     | ⭐具体针对⭐<br />小项目：base中测试<br />大项目：新建环境 |          |

