chronyc 命令：设置时间与时间服务器同步。

chronyc 命令来自英文词组 chrony command-line 的缩写，其功能是设置时间与时间服务器同步。

chrony 是一个用于保持系统时间与 NTP 时间服务器同步的服务，**其守护进程 chronyd 更为常见，而 chronyc 命令则是用户的配置工具**。

Chrony通常比传统的NTP更稳定，因为它可以更快地适应网络条件的变化，并且对时间的估计更加精确。

如果您的CentOS系统需要更精确的时间同步，可以考虑使用Chrony。

以下是一些基本的chronyc命令：

```
sudo yum install chrony

sudo systemctl start chronyd

sudo systemctl enable chronyd

#查看同步状态
chronyc tracking

#手动同步时间 这个命令会立即将系统时间调整到与NTP服务器同步
chronyc makestep

#查看NTP源 显示当前的NTP源和它们的同步状态
chronyc sources

#查看时间服务器的统计信息
chronyc sourcestats

#查看日志
chronyc log
```

添加或更改NTP源，修改配置：

编辑/etc/chrony.conf文件，添加或更改NTP服务器地址。

```
#在文件中添加或更改如下行
server ntp1.aliyun.com iburst

sudo systemctl restart chronyd
```
