> **systemctl是很常用的命令，以sshd服务进程为例记录**

## 查看ssh是否安装，以及安装方法
查看ssh是否安装：
```
rpm -qa | grep ssh
```
如有显示类似下面这些就说明安装了
```
libssh2-1.4.3-10.el7.x86_64
openssh-server-6.6.1p1-22.el7.x86_64
openssh-clients-6.6.1p1-22.el7.x86_64
openssh-6.6.1p1-22.el7.x86_64
```
安装命令：
```
sudo yum install openssh-server
```

## 查看ssh服务状态

方法一：
```
/etc/init.d/sshd status
```
方法二：
```
systemctl status sshd.service
```

## 启停命令
1、开启SSH服务
```
service sshd start
```
2、停止SSH服务
```
service sshd stop
```
3、重启SSH服务
```
service sshd restart
```
4、验证服务是否启动成功
```
netstat -antp | grep sshd
```

## 开机自启配置
1、检查SSH是否在本运行级别下设置为开机启动
```
chkconfig --list sshd
```
执行结果 sshd               0:off    1:off    2:on    3:on    4:on    5:on    6:off

2、如果没有设置可通过下面命令设置运行级别
```
chkconfig --level 2345 sshd on
```
指定运行级别为2,3,4,5
等级0表示：表示关机
等级1表示：单用户模式
等级2表示：无网络连接的多用户命令行模式
等级3表示：有网络连接的多用户命令行模式
等级4表示：不可用
等级5表示：带图形界面的多用户模式
等级6表示：重新启动

3、设置开机启动信息
```
chkconfig sshd on
```
也可以通过以下命令设置
```
systemctl enable sshd
```

## 查看服务器所有设置开机启动的服务
```
systemctl list-unit-files --state=enabled
```