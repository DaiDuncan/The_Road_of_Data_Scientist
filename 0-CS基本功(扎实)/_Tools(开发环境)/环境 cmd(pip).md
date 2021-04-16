# 安装

```python
pip install <Module>
```





# 查看版本

```python
### python
python --version  #cmd环境


### pandas
import pandas as pd
pd.show_versions()
pd.__version__
```



Pandas: 直接从0.25.2升级到1.1.5  2020.12.16

# 更新

```python
pip list # 列出安装的库
pip list --outdated # 列出有更新的库
pip install --upgrade library_name # 升级库library_name


### 脚本：一键升级所有的库
pip list --outdated | grep '^[a-zA-Z0-9.-]* (' | cut -d " " -f 1 | sudo -H xargs pip install -U
```

如果在环境中同时安装了python3，应该替换为pip3命令



