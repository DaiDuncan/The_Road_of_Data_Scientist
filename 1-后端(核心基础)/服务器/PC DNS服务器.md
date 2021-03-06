# PC|DNS服务器

[PC常见问题|DNS未响应怎么办?](https://zhuanlan.zhihu.com/p/43431800)

## 一、是什么|what？

0 背景问题

网络连接不上原因有很多，当联网出现异常，诊断出结果显示“DNS服务器未响应”怎么办?



1 DNS服务器是什么?

进行域名(domain name)和与之相对应的IP地址 (IP address)转换的服务器。

DNS中保存了一张域名(domain name)和与之相对应的IP地址 (IP address)的表，以解析消息的域名。



域名必须对应一个IP地址，一个IP地址可以有多个域名，而IP地址不一定有域名。

域名系统采用类似目录树的等级结构。域名服务器通常为客户机/服务器模式中的服务器方，它主要有两种形式：主服务器和转发服务器。将域名映射为IP地址的过程就称为“域名解析”。



## 二、为什么|why?

域名是Internet上某一台计算机或计算机组的名称，用于在数据传输时**标识计算机的电子方位**(有时也指地理位置)。域名是由一串用点分隔的名字组成的，通常包含组织名，而且始终包括两到三个字母的后缀，以指明组织的类型或该域所在的国家或地区。



## 三、怎么办|how?

### 1 安全软件|自带的网络修复工具

比如，360安全卫士



### 2 检查网络|CMD命令

1）打开网络和共享中心，选择当前所用的连接TCP/IPv4

在自动获取DNS一项中选择使用下面的DNS地址，可以使用8.8.8.8，然后看看能不能上网，如果不能请继续向下看。

2）cmd命令|ping 127.0.0.1

这是你当前主机的地址，如果成功，则表明说明TCP/IP协议没问题不需要重装，进行(3)步。否则你需要重新安装这个协议的驱动。

3）再输入ping 网关地址

路由器地址或者交换机的网关地址，一般为192.168.0(或者1).1。

网关具体获取方法是在命令行**输入ipconfig/all**，然后找到你当前连接网络类型对应的网关地址。

如果提示成功，则表明路由器连接正常，不需要重启或者设置。

4）如果不成功，则需要设置路由器

具体设置请搜索路由器设置引导，记得要选中DHCP。当然最简单的方法就是重启路由，这样一般的问题都会解决。



### 3 检查设置

1）机器：重启电脑

2）源头|网络服务提供商：网络运营商

可能是网络端dns配置错误 ==> 确认运营商DNS没有错误

3）使用ipconfig/all命令

查看下你的ipv4地址是多少，如果是以169开头，那这可能就是问题所在。

由于ip一般设置为自动获取，但是在DHCP未启动或者未更新的情况下，你的ip就只能使用系统默认设置的地址。这时候你需要在服务里面重启dhcp client服务，并设置为自动，然后再次重新获取ip。



### 4 手动设置

1） TCP/IP中手动设置你的ip和dns

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1599219756775-350930de-4d02-4428-85e6-b93d9210c117.png)

1中最后一个数字可以随便更改

2为你的路由地址或者网关

3为dns服务器地址，可以随便找一个



2）如果再不行的话，那可能是你的网卡坏了，或者运营商DNS的问题





## #小结

### 0 网络通道&硬件

网络服务提供商——>DNS服务(IP映射)——>TCP/IP设置——>路由器(DHCP 动态分配IP地址和子网掩码)——>PC网卡



### 1 技巧

1）ipconfig/all

查看IP地址，网关地址

2）ping IP地址

检查连接是否畅通