## sysstat与sar
sysstat是Linux系统中的常用工具包，而sar 是 Linux中sysstat工具包中的用于监控Linux系统性能的工具之一。

sysstat 工具包中包含两种类型的工具：即时查看工具（iostat、mpstat、sar）；累计统计工具（sar）。

因此sar命令，又叫做系统活动情况报告。**不仅可以实时查看服务器的性能，还可以做累计统计。**

安装：

`sudo yum/apt install sysstat`

然后启用数据收集：

```
sudo vi /etc/default/sysstat
change ENABLED="false" to ENABLED="true"
```

重新启动sysstat服务：

`sudo service sysstat restart
`

安装完成后，就可以利用这款强大的工具去监控我们的 Linux 操作系统了，macos下没有sysstat。

## 常用参数与示例
![image](https://github.com/user-attachments/assets/066bc653-8373-4868-84be-c7cb4c55bd7a)

统计CPU设备的负载信息，每次间隔2秒，共3次：

 [root@linuxcool ~]# sar -u 2 3 

统计硬盘设备的读写信息，每次间隔2秒，共3次：

 [root@linuxcool ~]# sar -d 2 3 

统计内存设备的读写信息，每次间隔2秒，共3次：

 [root@linuxcool ~]# sar -r 2 3 

统计内存设备的分页使用情况，每次间隔5秒，共3次：

 [root@linuxcool ~]# sar -B 5 3 

显示CPU使用率情况： [root@linuxcool ~]# sar -u 

显示系统负载情况： [root@linuxcool ~]# sar -q 

显示硬盘I/O和传输速率情况： [root@linuxcool ~]# sar -b 

显示网卡和网络情况： [root@linuxcool ~]# sar -n  DEV 

查看CPU负载：

sar -u 1 5   #每隔一秒刷新，刷新5次

查看队列长度和CPU平均负载：

sar -q 1 10    #每隔一秒刷新，刷新10次

查看磁盘读写负载情况：

sar -d 1 3    #每隔一秒刷新，刷新3次

查看内存负载：

sar -r 1 3    #每隔一秒刷新，刷新3次

查看I/O负载：

sar -b 1 10 #每隔一秒刷新，刷新10次

查看swap分区页面交换情况：

sar -W 1 3

