sysctl 命令来自英文词组 system control 的缩写，其功能是配置系统内核参数。

sysctl 命令能够在 Linux 系统运行时动态地配置系统内核参数，包含 TCP/IP 堆栈和虚拟内存系统等选项，可让有经验的系统管理员更好地优化整台服务器性能，但请注意，配置结果仅在当前生效， 系统重启后参数将恢复到初始状态 ， 要想永久生效 ，则需要将参数写入 /etc/sysctl.conf 系统文件。

查看所有：

`sysctl -a
`

查看特定参数：

`sysctl xxx
`

参数名与 /etc/sysctl.conf 等号左边部分一致。

![image](https://github.com/user-attachments/assets/5435e63a-c9a7-4197-b2d8-af91edc0abee)

示例：

如果希望屏蔽别人 ping 你的主机，则加入以下代码：

`net.ipv4.icmp_echo_ignore_all = 1
`

编辑完成后，请执行以下命令使变动立即生效：

`/sbin/sysctl -p
`

永久生效，重启机器后仍生效。

临时生效：

`/sbin/sysctl -w net.ipv4.route.flush=1
`

重启机器后失效。
