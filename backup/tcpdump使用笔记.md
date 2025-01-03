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

## 极客时间课程笔记
虽然 tcpdump 有完备的文档和命令手册，但如果你不经常抓包，并不能掌握得特别熟 练。我个人的经验是，抓包技术课是一门实践课，不是理论课。既然是实践课，就要多多实践，从鲜活的案例中积累起来的经验无比宝贵。

![image](https://github.com/user-attachments/assets/cad3a9e5-5f53-4f44-9888-a13f0c69e409)

用法：
```
tcpdump host 10.10.10.10  and port 22
```

更多参数：
```
-w 文件名，可以把报文保存到文件；
-c 数量，可以抓取固定数量的报文，这在流量较高时，可以避免一不小心抓取过多报 文；
-s 长度，可以只抓取每个报文的一定长度，后面我会介绍相关的使用场景；
-n，不做地址转换（比如 IP 地址转换为主机名，-nn 表示port 80 转换为 http）；
-v/-vv/-vvv，可以打印更加详细的报文信息；
-e，可以打印二层信息，特别是 MAC 地址；
-p，关闭混杂模式。所谓混杂模式，也就是嗅探（Sniffering），就是把目的地址不是本机地址的网络报文也抓取下来。
```

有时候你想看 TCP 报文里面的具体内容，比如应用层的数据，那么你可以用 -X 这个参 数，以 ASCII 码来展示 TCP 里面的数据：
```
sudo tcpdump port 80 -X
```

如何过滤报文？

最近我们有个实际的需求，要统计我们某个 HTTPS VIP 的访问流量里，TLS 版本（现在主 要是 TLS1.0、1.1、1.2、1.3）的分布。为了控制抓包文件的大小，我们又不想什么 TLS 报文都抓，只想抓取 TLS 版本信息。这该如何做到呢？要知道，针对这个需求，tcpdump 本身没有一个现成的过滤器。

其实，BPF 本身是基于偏移量来做报文解析的，所以我们也可以在 tcpdump 中使用这种 偏移量技术，实现我们的需求。下面这个命令，就可以抓取到 TLS 握手阶段的 Client Hello 报文：
```
tcpdump -w file.pcap 'dst port 443 && tcp[20]==22 && tcp[25]==1'
```

- dst port 443：这个最简单，就是抓取从客户端发过来的访问 HTTPS 的报文。
- tcp[20]==22：这是提取了 TCP 的第 21 个字节（因为初始序号是从 0 开始的），由于 TCP 头部占 20 字节，TLS 又是 TCP 的载荷，那么 TLS 的第 1 个字节就是 TCP 的第 21 个字节，也就是 TCP[20]，这个位置的值如果是 22（十进制），那么就表明这个是 TLS 握手报文。
- tcp[25]==1：同理，这是 TCP 头部的第 26 个字节，如果它等于 1，那么就表明这个是 Client Hello 类型的 TLS 握手报文。

![image](https://github.com/user-attachments/assets/53a4f057-e6b5-4a44-a368-ddbe9282e035)

报文里是16进制。

不过看到这里，你会不会忽然觉得 tcpdump 有点陌生了？怎么使用门槛这么高了呢？

还好，不是每个过滤条件都要这么“艰难”的。对一些常见的过滤场景，tcpdump 也预定义了一些相对方便的过滤器。比如我们想要过滤出 TCP RST 报文，那么可以用下面这种写法，相对来说比用数字做偏移量的写法，要更加容易理解和记忆：
```
tcpdump -w file.pcap 'tcp[tcpflags]&(tcp-rst) != 0'
```

等于偏移量的写法：
```
tcpdump -w file.pcap 'tcp[13]&4 != 0'
```

读取抓包文件并过滤：
```
tcpdump -r file.pcap 'tcp[tcpflags] & (tcp-rst) != 0'
```

过滤后转存：
```
tcpdump -r file.pcap 'tcp[tcpflags] & (tcp-rst) != 0' -w rst.pcap
```

抓包长度-s参数解释：

前面我提到过 -s 这个长度参数，它的使用场景其实就包括了延长抓包时间。我们给 tcpdump 加上 -s 参数，指定抓取的每个报文的最大长度，就节省抓包文件的大小，也就延长了抓包时间。

一般来说，帧头是 14 字节，IP 头是 20 字节，TCP 头是 20~40 字节。如果你明确地知道 这次抓包的重点是传输层，那么理论上，对于每一个报文，你只要抓取到传输层头部即可，也就是前 14+20+40 字节（即前 74 字节）：
```
tcpdump -s 74 -w file.pcap
```

而如果是默认抓取 1500 字节，那么生成的抓包文件将是上面这个抓包文件的 20 倍。反过 来说，使用同样的磁盘空间，上面这种方式，其抓包时间可以长达默认的 20 倍！

但是，如果你还想看 TLS 甚至更上层的应用层的信息，这么做就不行了。比如下面这个抓包，我就是用了 74 字节作为每个报文的抓取长度，所以第五层开始的信息，就看不到或者看不全了。

![image](https://github.com/user-attachments/assets/826e7800-1ce7-4790-bb8e-90c25cc02f04)

显然，TLS 只分到了 8 个字节，信息不完整，所以 Wireshark 也无能为力，没法告诉我们 这个 TLS 头部里的信息了。

那么，如果你怀疑 TLS 或者更上面的应用层也跟问题有关，我建议你就去掉 size 的限制， 还是抓取全部数据为好。

这是因为，应用层头部的长度跟二到四层的情况非常不同，应用层头部可能非常大，甚至超过了 TCP 的 MSS。比如 HTTP 头部，小的话可能只有几十个字节，大的话可能要几十个 KB，也就是好多个 TCP segment 才放得下。这种时候，要是我们还在纠结如何节省抓取的报文长度，却放过了可能真正对排查有关键价值的数据，就得不偿失了。

其它用法记录：

抓取 TCP SYN 包，偏移量方式
```
 tcpdump 'tcp[13] = 2' -w file.pcap
```

非偏移量：
```
tcpdump  'tcp[tcpflags] == tcp-syn'
```

只抓取到ip层：
```
tcpdump -s 34 -w file.pcap
```

tcptrace工具查看包文件：
```
sudo apt install tcptree
```

默认报文是按照时间线原样展示：

![image](https://github.com/user-attachments/assets/384498cd-af35-4948-9a53-ca3edcb99ec7)

报文都是按时间线原样展示的，缺乏逻辑关系，是不是难以组织起有效的分析？比如，要搞清楚里面有几条 TCP 连接都不太容易。这时候怎么办呢？

其实，还有一个工具能帮上忙，它就是 tcptrace。它不能用来抓包，但是可以用来分析。 知道这个工具的人不算很多，但其实 tcptrace 是一个挺“古老”的工具了。在 Wireshark 工具集（Wireshark 图形界面和 tshark 等命令行工具）还没一统江湖的时候，tcptrace 也 有其独到的价值，因为它不仅可以读取 pcap 格式的抓包文件，也可以读取 snoop 等其他 格式的抓包文件。

比如下面这样，tcptrace 告诉我们，这个抓包文件里有 2 个 TCP 连接，并且是以 RST 结束的：

![image](https://github.com/user-attachments/assets/89200138-e1b0-47ae-a17b-a367571c66a8)

## tcpdump和iptables的路径制约
tcpdump和iptables的关系，总结下来有如下2点。

- tcpdump可以抓取到被iptables在INPUT链上DROP掉的数据包。
- tcpdump不能抓取到被iptables在OUTPUT链上DROP掉的数据包。