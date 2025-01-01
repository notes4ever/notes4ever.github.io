> **记录tcpdump常用命令**

## tcpdump常用实践命令
```
tcpdump -D 确认要抓的网卡 知道顺序即可

tcpdump -nni 3  src host 112.93.42.135 and dst port 443 -c 100

tcpdump -nni 3  src host 112.93.42.135 or dst port 80 -C 100 -W 3 -w abc
```

-C 1表示1M 文件大小 -W 表示文件数量，超过以后会覆盖之前的，生成的结果是abc0 abc1 

总体上还是推荐使用tshark。

## tcpdump常用参数与用法解释
tcpdump命令：监听网络流量

tcpdump命令的功能是监听网络流量，是一款数据嗅探工具，在 Linux系统中常用来抓取数据包，能够记录所有经过服务器的数据包信息。tcpdump 命令需要以管理员身份执行。
```
tcpdump -h 查看版本

sudo tcpdump –i eth0 –w packets.pcap -c20
```

-i 1 即为-D参数中排序1的网卡，如eth0。

![image](https://github.com/user-attachments/assets/1e1846e6-196d-43c4-85a0-0bca54cf7e86)

除了-i实用，-nni也很实用，禁用ip和端口的解析，-n禁ip解析，-nn禁ip和端口解析。

在Tcpdump中你可以在单引号里构造过滤表达式，然后附到命令的最后。

以下的命令依然会捕获和存储目的端口号是80的TCP流量：
```
tcpdump –nni eth0 –w packets.pcap 'tcp dst port 80'
```

当读取捕获文件时你也可以构造过滤器。

以下命令会显示packets.pcap中所有符合过滤表达式的包：
```
tcpdump –r packets.pcap 'tcp dst port 80'
```

如果没有在抓包的时候指明过滤器，那么你的捕获文件里通常会含有其他数据包。

读取这个捕获文件后，你仅仅在屏幕上限制了所打出来的内容。

那么如果你有一个包含大量各种类型数据包的捕获文件，而你又想把需要的数据包过滤出来另存为一个文件，可以结合使用-w和-r参数来解决：
```
tcpdump –r packets.pcap 'tcp dst port 80' –w http_packets.pcap
```

实践中可以都保存，然后再读取过滤n条，最好是保存到文件中分析。

bpf参数也可以从文件中读取：
```
tcpdump –nni eth0 –F dns_servers.bpf
```

文件读取：
```
tcpdump –r packets.pcap -c20
```

-c表示读取多少个包，从最开始算起。

tcpdump大部分的参数如下：

![image](https://github.com/user-attachments/assets/7b31ba73-e3ee-4c91-ab2a-38e0bbf96ff9)

我们知道，tcpdump 也是最常用的一个网络分析工具。它基于 libpcap ，利用内核中的 AF_PACKET 套接字，抓取网络接口中传输的网络包；并提供了强大的过滤规则，帮你从大量的网络包中，挑出最想关注的信息。

tcpdump 为你展示了每个网络包的详细细节，这就要求，在使用前，你必须要对网络协议有基本了解。而要了解网络协议的详细设计和实现细节， RFC 当然是最权威的资料。

不过，RFC 的内容，对初学者来说可能并不友好。如果你对网络协议还不太了解，推荐你去学《TCP/IP 详解》，特别是第一卷的 TCP/IP 协议族。这是每个程序员都要掌握的核心基础知识。

再回到 tcpdump 工具本身，它的基本使用方法，还是比较简单的，也就是 tcpdump [选项] [过滤表达式]。当然，选项和表达式的外面都加了中括号，表明它们都是可选的。

查看 tcpdump 的 手册 ，以及 pcap-filter 的手册，你会发现，tcpdump 提供了大量的选项以及各式各样的过滤表达式。不过不要担心，只需要掌握一些常用选项和过滤表达式，就可以满足大部分场景的需要了。

为了帮你更快上手 tcpdump 的使用，我在这里也帮你整理了一些最常见的用法，并且绘制成了表格，你可以参考使用。

首先，来看一下常用的几个选项。在上面的 ping 案例中，我们用过 -nn 选项，表示不用对 IP 地址和端口号进行名称解析。其他常用选项，我用下面这张表格来解释。

![image](https://github.com/user-attachments/assets/06525209-5e0b-49be-92be-c68d34b6c84c)

接下来，我们再来看常用的过滤表达式。刚刚用过的是 udp port 53 or host 35.190.27.188 ，表示抓取 DNS 协议的请求和响应包，以及源地址或目的地址为 35.190.27.188 的包。

其他常用的过滤选项，我也整理成了下面这个表格。

![image](https://github.com/user-attachments/assets/a8d6413d-839f-4126-836c-ce413aecee5e)

