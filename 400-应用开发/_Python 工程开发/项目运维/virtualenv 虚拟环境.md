任何技术的存在与发展，都是为了解决实际的问题，如果没有这个前提，技术就没有立足之地，python虚拟环境这种技术，就是最好的证明。



# 第三方库

## 安装位置 `sys.path`



每当我们使用pip命令安装第三方库时，第三方库都会被安装到python的site-packages目录下

在python3.6的安装目录下，有一个lib文件夹，继续向下寻找

```text
/Library/Frameworks/Python.framework/Versions/3.6

lib/python3.6/site-packages
```

python脚本里引用第三方库时，解释器就会到这个目录下寻找第三方库





## 第三方库的不同版本

第三方库不会一成不变，开发者会对它进行升级改造，发布新的版本，而对于使用者来说，就产生了一个小小的麻烦。

我有两个项目，A 和 B，A是一个老项目，B是一个新开发的项目，巧合的是他们都用到了同一个第三方库C，但他们的版本不一致，A项目用的是C1.0，B项目用的是C2.0。

如果C2.0完全兼容1.0，在同一台机器上，我就可以安装好C2.0，这样A和B两个项目都可以正常运行

- 但如果2.0 不完全兼容1.0，那么安装了2.0 A项目就不能正常运行了
- 安装了1.0，B项目又不能正常运行





# 虚拟环境 virtualenv

在一台机器上，不同的项目需要使用同一个第三方库的不同版本，甚至需要不同的python版本，那么就可以使用虚拟环境技术。

虚拟环境技术将创建不同的环境

```shell
pip install virtualenv

mkdir mypro
cd mypro
virtualenv --no-site-packages venv
```

virtualenv 命令会根据基线环境创建出一个虚拟环境，venv就是虚拟环境所在的目录，这个基线环境如果你不指定，那么virtualenv命令会自己寻找

想要指定的话使用-p 进行设置，以我的电脑环境为例

```text
virtualenv -p /Library/Frameworks/Python.framework/Versions/3.6 --no-site-packages venv
```



如果你是windows电脑，则可以写成

```text
virtualenv -p c:\Python36\python.exe venv
```



基线环境base 要根据你电脑实际安装python的文件夹来定，加上--no-site-packages 这个配置，就表明，基线环境里的第三方库不会被复制到虚拟环境里，这样，虚拟环境就是一个非常干净的环境。



进入文件夹venv ，可以看到3个目录：

- bin     
- include 
- lib



这个虚拟环境，简直就是基线环境的复制品



## 进入虚拟环境

```text
source venv/bin/activate
```

为每个项目创建一个虚拟环境，安装适合项目的第三方库



## 退出虚拟环境

```text
deactivate
```





# Pycharm自带虚拟环境

每创建一个项目，就会为这个项目创建一个虚拟环境，原本是为了方便用户的，但很多初学者对于python的学习还不够深入，因此遇到很多麻烦。

每个项目都可以配置python解释器，当你在pycharm里运行代码时，就是这个python解释器在执行代码，默认使用的是虚拟环境里的python解释器。



如果你第三方库安装在基线环境下，而你的项目配置的解释器用的虚拟环境的，那么这个第三方库就无法使用了，这个时候你有两个选择：

- 第一个方法是将项目的python解释器修改成基线环境，或者说主环境的python解释器
- 第二个是通过pycharm在虚拟环境里安装第三方库



## 配置项目的python解释器

菜单栏Preferences... => project interpreter配置解释器

![image-20210128114602061](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210128114602.png)



## 通过pycharm安装第三方库

不管解释器配置成哪一个，你都可以通过pycharm来安装第三方库



