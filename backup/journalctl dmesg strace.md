> **linux下常用的日志排查与进程追踪命令有journalctl dmesg strace等**

## journalctl
journalctl 命令来自英文词组 journal control 的缩写，其功能是查看指定的日志信息。

在 RHEL 7/CentOS 7 及以后版本的 Linux 系统中，systemd 服务统一管理了所有服务的启动日志， 带来的好处就是可以只用 journalctl 一个命令来查看全部的日志信息了。

![image](https://github.com/user-attachments/assets/ecf3b21c-8be2-4152-a528-bc74abe752d4)


查看默认的系统日志：

sudo journalctl

显示带有额外信息的最新日志：

sudo journalctl -xe

查看特定service日志：

sudo journalctl -u sshd 

实时刷新最新产生的日志：

sudo journalctl -f

## dmesg
用于显示或控制内核环缓冲区。内核环缓冲区是**操作系统内核用来记录系统启动过程中的消息和系统运行时的日志信息**的地方。

基本用法

查看日志: 执行 dmesg 命令可以查看系统启动时的日志信息。

实时更新: 使用 dmesg -w 可以实时查看内核消息，这在调试硬件问题时非常有用。

常用选项

- -h 或 --human-readable：以更易读的格式显示日志。
- -c：清除当前的内核消息缓冲区。
- -s SIZE：显示从缓冲区开始的指定数量的日志条目。
- -t：显示时间戳。
- -r 或 --raw：以原始格式显示日志。
- -w：等待并显示新的日志消息。

dmesg 是系统管理员和开发者常用的工具，用于诊断系统问题和监控系统状态。

## strace
strace 命令跟踪系统调用和信号。它提供了流程正在执行的操作的详细信息。 

strace 的一个简单用途是使用 PID 来跟踪正在运行的进程。

```
strace -p PID
strace -p PID -e system_call,system_call2
strace -o output.txt command_name
```

如果某个程序恰好挂起而找不到原因，则可以跟踪该程序正在进行的系统调用并揭示其卡住的原因。