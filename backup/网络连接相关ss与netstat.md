## ss和netstat的区别
了解当前系统开放了哪些端口，并且要查看开放这些端口的具体进程和用户。

主要参考：https://blog.csdn.net/weixin_42816196/article/details/86580834

netstat和ss都是网络相关的监控，netstat在协议栈中每一层都可以做监控统计：TCP、路由表、网络接口、协议等，但是在Linux中，netstat已经不为推荐，取而代之的是ss。

![image](https://github.com/user-attachments/assets/3c78658b-7cf0-4b3a-8033-d67e6b248b45)

原理对比：

ss比netstat快的主要原因是，netstat是遍历/proc下面每个PID目录，ss直接读/proc/net下面的统计信息。所以ss执行的时候消耗资源以及消耗的时间都比netstat少很多。

当服务器的socket连接数量非常大时（如上万个），无论是使用netstat命令还是直接cat /proc/net/tcp执行速度都会很慢，相比之下ss可以节省很多时间。

ss快的秘诀在于，它利用了TCP协议栈中tcp_diag，这是一个用于分析统计的模块，可以获得Linux内核中的第一手信息。如果系统中没有tcp_diag，ss也可以正常运行，只是效率会变得稍微慢但仍然比netstat要快。

根据[这篇博客](http://www.cnblogs.com/wangkangluo1/archive/2012/05/15/2500844.html)的测试，同样的大量socket连接情况下，netstat和ss获取同样的统计数据时的耗时，ss明显比netstat少很多。

## ss常用参数
和netstat类似，如-anlp等。
```
-a显示所有的sockets
-n显示数字IP和端口，不通过域名服务器
-l显示正在监听的
-p显示使用socket的对应的程序
-t只显示TCP sockets
-u只显示UDP sockets
-4 -6 只显示v4或v6V版本的sockets
-s打印出统计信息。这个选项不解析从各种源获得的socket。对于解析/proc/net/top大量的sockets计数时很有效
-0 显示PACKET sockets
-w 只显示RAW sockets
-x只显示UNIX域sockets
-r尝试进行域名解析，地址/端口
```
常用之统计：
```
ss -s
```
常用之查看端口：
```
ss -anlptu | grep端口
```

![image](https://github.com/user-attachments/assets/8a6073f2-e695-4d03-aa7d-e8f3f3772e38)


## netstat常用参数

```
netstat -nap：显示系统端口使用情况
netstat -nupl：UDP类型的端口
netstat -ntpl：TCP类型的端口
netstat -na|grep ESTABLISHED|wc -l：统计已连接的，状态为"established"
netstat -l：只显示所有监听端口
netstat -lt：只显示所有监听tcp端口
netstat -rn：可以查看查看本机路由表、默认网关等信息
netstat -ntulp |grep 80: 查看所有80端口使用情况
netstat -nlp |grep LISTEN: 查看当前所有监听端口
```

netstat命令参数释义：

```
-r：--route，显示路由表信息
-g：--groups，显示多重广播功能群组组员名单
-s：--statistics，按照每个协议来分类进行统计。默认的显示IP、IPv6、ICMP、ICMPv6、TCP、TCPv6、UDP和UDPv6 的统计信息。
-M：--masquerade，显示网络内存的集群池统计信息
-v：--verbose，命令显示每个运行中的基于公共数据链路接口的设备驱动程序的统计信息
-W：--wide，不截断IP地址
-n：禁止使用域名解析功能。链接以数字形式展示(IP地址)，而不是通过主机名或域名形式展示
-N：--symbolic，解析硬件名称
-e：--extend，显示额外信息
-p：--programs，与链接相关程序名和进程的PID
-t：所有的 tcp 协议的端口
-x：所有的 unix 协议的端口
-u：所有的 udp 协议的端口
-o：--timers，显示计时器
-c：--continuous，每隔一个固定时间，执行netstat命令
-l：--listening，显示所有监听的端口
-a：--all，显示所有链接和监听端口
-F：--fib，显示转发信息库(默认)
-C：--cache，显示路由缓存而不是FIB
-Z：--context，显示套接字的SELinux安全上下文
```

其他表述，常用参数-anplt

```
-a 显示所有活动的连接以及本机侦听的TCP、UDP端口
-l 显示监听的server port
-n 直接使用IP地址，不通过域名服务器
-p 正在使用Socket的程序PID和程序名称
-r 显示路由表
-t 显示TCP传输协议的连线状况
-u 显示UDP传输协议的连线状况
-w 显示RAW传输协议的连线状况
```

注意：在macos上，用法有些许不同，直接用可能报错

![image](https://github.com/user-attachments/assets/c433eac8-4634-4464-a588-fca5358e973e)

意思是缺少协议。

解决方案在Mac上正确使用的方法是：即-f需要加上地址族，-p需要加上协议TCP或者UDP等

```
如果需要查询inet，netstat -anvf inet
如果需要查询TCP， netstat -anvp tcp
如果需要查询UDP，netstat -anvp udp
```

## 查看time_wait数量与端口
查看time_wait数量：
```
netstat -ant | grep -i "time_wait" | wc -l
```

结果：13364

更加推荐使用这种格式的命令，显示格式更友好。

查看time_wait close_wait established 数量：
```
netstat -n | awk '/^tcp/ {++state[$NF]} END {for(key in state) print key,"\t", state[key]}'
```

结果：

CLOSE_WAIT       15
ESTABLISHED      1901
TIME_WAIT        572

查看端口范围，与linux内核参数相关：
```
cat /proc/sys/net/ipv4/ip_local_port_range
```
或者：
```
sysctl -a | grep ip_local_port_range
```

结果：32768   60999

## 查看网络连接历史统计信息
```
netstat -s | ss -s
```

netstat 除了可以获取实时连接状况，还可以获取历史统计信息。

比如，你怀疑 一台机器的网络很不稳定，除了用 ping 做简单的测试，你还可以用 netstat -s 来获取更 加详细的统计信息。

比如，其中的 TCP 丢包和乱序计数值，就能帮助你判断传输层的状况。

以上是静态值，动态值可以配合watch命令查看：
```
watch -d netstat -s
```
这个命令会把发生变化的数值进行高亮，方便我们查看。

用ss替代netstat：
```
ss -s
```

```
watch -d ss -s
```

## netstate查看端口占用
```
lsof -i:80
```
效果与下面类似：
```
netstat -antp | grep 80
```

## 套接字socket相关
fconfig 和 ip 只显示了网络接口收发数据包的统计信息，但在实际的性能问题中，网络协议栈中的统计信息，我们也必须关注。

你可以用 netstat 或者 ss ，来查看套接字、网络栈、网络接口以及路由表的信息。

我个人更推荐，使用 ss 来查询网络的连接信息，因为它比 netstat 提供了更好的性能（速度更快）。

比如，你可以执行下面的命令，查询套接字信息：

![image](https://github.com/user-attachments/assets/0d19f0ba-39ca-420d-8f7e-b1291f545cad)

![image](https://github.com/user-attachments/assets/97e289b6-4c1c-4fbc-9419-d372eebe6e01)

netstat 和 ss 的输出也是类似的，都展示了套接字的状态、接收队列、发送队列、本地地址、远端地址、进程 PID 和进程名称等。

其中，接收队列（Recv-Q）和发送队列（Send-Q）需要你特别关注，它们通常应该是 0。当你发现它们不是 0 时，说明有网络包的堆积发生。当然还要注意，在不同套接字状态下，它们的含义不同。

当套接字处于连接状态（Established）时，

```
Recv-Q 表示套接字缓冲还没有被应用程序取走的字节数（即接收队列长度）。
而 Send-Q 表示还没有被远端主机确认的字节数（即发送队列长度）。
```

当套接字处于监听状态（Listening）时，

```
Recv-Q 表示 syn backlog 的当前值。
而 Send-Q 表示最大的 syn backlog 值。
```

而 syn backlog 是 TCP 协议栈中的半连接队列长度，相应的也有一个全连接队列（accept queue），它们都是维护 TCP 状态的重要机制。

顾名思义，所谓半连接，就是还没有完成 TCP 三次握手的连接，连接只进行了一半，而服务器收到了客户端的 SYN 包后，就会把这个连接放到半连接队列中，然后再向客户端发送 SYN+ACK 包。

而全连接，则是指服务器收到了客户端的 ACK，完成了 TCP 三次握手，然后就会把这个连接挪到全连接队列中。这些全连接中的套接字，还需要再被 accept() 系统调用取走，这样，服务器就可以开始真正处理客户端的请求了。

## 协议栈统计信息
类似的，使用 netstat 或 ss ，也可以查看协议栈的信息。

-s参数的时候，这两个命令区别比较大，并不能互相替代，也不能比较哪个好，尤其警惕。

![image](https://github.com/user-attachments/assets/11a35876-315b-4700-8c09-f7da086f0430)

![image](https://github.com/user-attachments/assets/4056e72a-1b0c-4a4f-81a4-afa6c61a70f2)

这些协议栈的统计信息都很直观。ss 只显示已经连接、关闭、孤儿套接字等简要统计，而 netstat 则提供的是更详细的网络协议栈信息。

比如，上面 netstat 的输出示例，就展示了 TCP 协议的主动连接、被动连接、失败重试、发送和接收的分段数量等各种信息。
