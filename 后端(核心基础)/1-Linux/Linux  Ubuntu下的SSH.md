# Linux | Ubuntu下的SSH

**声明**：以下内容主要基于[cnblogs | ubuntu下的ssh](https://www.cnblogs.com/whatisfantasy/p/5950070.html)



ubuntu默认没有安装openssh-server



## 一 服务器端

### 1 安装

```
apt-get install openssh-server        #安装
ps -e | grep ssh        #检查服务是否开启，如果存在ssh-agent和sshd说明已开启服务
/etc/init.d/ssh restart        #重启ssh服务
```



### 2 ssh配置文件





```
vi /etc/ssh/sshd_config        #修改ssh-server的配置文件

Port 20 #ssh默认端口是22，需要的话，自行修改
PermitRootLogin no #ssh默认配置是允许root登录的，可以修改配置表禁止其登录：
```





## 二 客户端

### 1 安装

```
apt-get install openssh-client        #ubuntu默认已经安装了openssh-client
```

### 2 登录

```
ssh morra@192.168.1.112      #登录，ssh+用户名@IP
exit        #退出
```

第一次登录：

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1592924038745-54c93970-56c6-487d-a7f6-4c854458c1ea.png)

第二次登录：

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1592924060198-536a9684-741d-4353-8ed2-f1f302de197f.png)

当远程主机的公钥被接受以后，它就会被保存在文件$HOME/.ssh/known_hosts之中。

下次再连接这台主机，系统就会认出它的**公钥已经保存在本地**了，从而跳过警告部分，直接提示输入密码。

## 参考文献

[cnblogs | ubuntu下的ssh](https://www.cnblogs.com/whatisfantasy/p/5950070.html)