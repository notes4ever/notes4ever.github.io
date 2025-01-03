> **记录wireshark常用命令**

## wireshark配置用户组
tcpdump需要使用root，tshark可也需要root，普通用户会报错。

可以使用root将普通用户加入wireguard用户组，或者使用chmod +s 命令。

使用root运行：
```
usermod -a -G wireshark user
```

把普通用户添加进wireshar用户组，就能实现抓包（不用root）；可能需要重启才能生效，否则还是提示权限不够。如果不能正常获取权限，使用`tshark -D `不能获得网络接口。

tcpdump不能使用这种方式。可以用chmod  u+s 来临时解决。
```
which tcpdump
sudo chmod u+s /usr/bin/tcpdump
```

使用u+s类似的还有journalct 把root权限赋给普通用户。

参考：

- https://blog.csdn.net/qimengxingyuan/article/details/80872315
- https://www.cnblogs.com/lvdongjie/p/4076056.html

## 捕获过滤
捕获过滤器应用于libpcap/WinPcap，并使用Berkeley Packet Filter(BPF)语法。这个语法被广泛用于多种数据包嗅探软件，主要因为大部分数据包嗅探软件都依赖于使用BPF的libpcap/WinPcap库。


限定词 | 说明 | 例子
-- | -- | --
Type | 指出名字或数字所代表的意义 | host、net、port
Dir | 指明传输方向是前往还是来自名字或数字 | src、dst
Proto | 限定所要匹配的协议 | Ether、ip、tcp、udp、http、ftp

示例：
```
src host 192.168.0.10 && port 80

host 2001:db8:85a3:8a2e:370:7334

!ip6
```

## 显示过滤
### 找出所有tls client hello
tls.handshake.type==1

handshake.type更多类型可以参考：https://davidwzhang.com/2018/03/16/wireshark-filter-for-ssl-traffic/

### 显示所有的syn包不包括对syn包的ack
tcp.flags.syn==1 && tcp.flags.ack==0

### 显示所有的syn + ack包
tcp.flags.syn==1

### 找出RST报文
tcp.flags.reset==1

### 查看若干tcp端口
tcp.port in {80,443,8000..8005}

### 查看ip的地理位置
ip.geoip.dst_country == "China"

geoip的其他用法，参考：https://cloud.tencent.com/developer/article/2443136

## Time时间分析那些事
参考：https://blog.csdn.net/weixin_47627078/article/details/135338648

tcp.analysis.lost_segment：表明已经在抓包中看到不连续的序列号。报文丢失会造成重复的ACK，这会导致重传。

tcp.analysis.duplicate_ack：显示被确认过不止一次的报文。大量的重复ACK是TCP端点之间高延时的迹象。

tcp.analysis.retransmission：显示抓包中的所有重传。如果重传次数不多的话还是正常的，过多重传可能有问题。这通常意味着应用性能缓慢和/或用户报文丢失。

tcp.analysis.window_update：将传输过程中的TCP window大小图形化。如果看到窗口大小下降为零，这意味着发送方已经退出了，并等待接收方确认所有已传送数据。这可能表明接收端已经不堪重负了。

tcp.analysis.bytes_in_flight：某一时间点网络上未确认字节数。未确认字节数不能超过你的TCP窗口大小，为了最大化吞吐量你想要获得尽可能接近TCP窗口大小。如果看到连续低于TCP窗口大小，可能意味着报文丢失或路径上其他影响吞吐量的问题。

tcp.analysis.ack_rtt：衡量抓取的TCP报文与相应的ACK。如果这一时间间隔比较长那可能表示某种类型的网络延时（报文丢失，拥塞，等等）。

常见异常排查：

- Packet size limited during capture：标记了的包没抓全。
- TCP Previous segment not captured：Wireshark 发现后一个包的 Seq 大于 Seq+Len，就知道中间缺失了一段。
- TCP ACKed unseen segment：发现被 Ack 的那个包没被抓到，就会提示。
- TCP Out-of-Order：后一个包的 Seq 号小于前一个包的 Seq+Len 时。
- TCP Dup ACK：当乱序或丢包发生时，接收方会收到一些 Seq 号比期望值大的包。没收到一个这种包就会 Ack 一次期望的 Seq 值，提现发送方。
- TCP Fast Retransmission：三次DUP ACK之后出发快速重传。
- TCP Retransmission：发送方只好等到超时了再重传。
- TCP zerowindow：0窗口懂得都懂！没法再收。
- TCP window Full：窗口耗尽。没法再发！
- Time-to-live exceeded：分片无法正常组装

界面截图1：

把所有超过200毫秒的确认都筛出来（当然筛出来的不一定全都是延迟确认，追求精确的话就逐个检查）
```
tcp.analysis.ack_rtt>0.2 and tcp.len==0
```

![image](https://github.com/user-attachments/assets/3b21905b-d693-4390-b196-c3cc5f781732)

在Edit→Preferences→Protocols→TCP菜单中勾上Relative Sequence Numbers

表达式1：(tcp.flags.reset==1)＆＆(tcp.seq==1)

从表面上看，它只是过滤出Seq号为1，且含有Reset标志的包，似乎与握手无关。但在启用Relative Sequence Numbers的情况下，这往往表示握手请求被对方拒绝了。

表达式2：(tcp.flags.syn==1)＆＆(tcp.analysis.retransmission)

这道表达式可以过滤出重传的握手请求。一个握手请求之所以要重传，往往是因为对方没收到，或者对方回复的确认包丢失了。这个重传特征正好用来过滤。

做运维的工程师们都知道，大规模DDoS（Distributed Denial of Service，分布式拒绝服务攻击）来临的时候最惊心动魄。

DDoS的形式有很多种，其中最流行的就是基于三次握手的SYN flood，其原理是从大量主机发送SYN请求给服务器，假装要建立TCP连接。这些SYN请求可能含有假的源地址，所以服务器响应后永远收不到Ack，就会留下half-open状态的TCP连接。由于每个TCP连接都会消耗一定的系统资源，如果攻击足够猛烈，此类连接越建越多，服务器的资源就会被耗光，真正的用户访问也会被拒绝。

Wireshark可以轻易地发现SYN flood。有时一打开包就很显眼了，密密麻麻都是SYN。假如干扰包太多，那就点击Analyze→Expert Info→Chats菜单，可以看到SYN的总数量统计。

我们可以把SYN flood看作TCP协议的设计缺陷，有办法可以防御，却无法根除。想知道大公司都是怎样防御的吗？手段有很多，其中有一些还可以在Wireshark中看出端倪。我假装攻击了全球最大的假药销售网站，然后把全过程的包抓下来。从图可见，对方很快就识别了我的不良意图，所以Reset（RST）了大多数握手请求。如果有兴趣去研究RST包里的细节，比如网络层的TTL和Identification，也许还能判断出究竟是流量清洗还是TCP握手代理之类的。

界面截图2：

找出RST报文
```
ip.addr eq 112.93.42.135 and tcp.flags.reset eq 1
```

![image](https://github.com/user-attachments/assets/7207d7f4-72b5-4565-a8b9-5136afef3a8b)

界面截图3：

找出post请求
```
http.request.method eq POST
```

![image](https://github.com/user-attachments/assets/e70ed01b-5dfd-4e76-98dd-97546c26ef37)

## 高级特性
包括端点和会话窗口、名称解析的细节、协议解析、数据流跟踪、IO图形化等。这些独特的图形功能在分析的不同阶段是非常有用的。

### 端点和会话
端点（endpoint）就是指网络上能够发送或者接收数据的一台设备。两个端点之间的通信被称之为会话（conversation）。Wireshark 根据交互的特性来标识端点会话，特别是在多种协议之间所使用的地址。

![image](https://github.com/user-attachments/assets/5e862a33-0e13-48eb-8455-6886417ab6be)

wireshark提供了端点统计和网络会话的统计。

### 协议分层统计
协议分层统计窗口就像一张快照，会让你直观地看到网络活动中的各种类型。例如，以太网流量占100%，IPv4流量占99.7%，TCP流量占98%，来自网页浏览的HTTP流量占13.5%。这些信息给我们提供了一个很好的测试网络的方式，特别是当你在脑海中对网络流量通常是什么样子有了大致的印象后。后续出现跟平时流量相差比较大的情形时，就大概心中有数了。

### 名称解析
网络名称解析可能会失败，尤其是当没有可用的DNS服务器时。

名称解析还会带来额外的处理开销。

对DNS名称解析的依赖会产生额外的数据包，也就是说你的捕获文件可能会被解析那些基于DNS地址的流量所占据。我们还可以再把问题想得复杂一些，如果在你分析的捕获文件中含有恶意IP地址，那么试图去解析它们会生成对攻击者控制的基础架构的查询，这样攻击者就有可能知道你的动作，甚至把你自己变成靶子。

### 流跟踪
现有4种类型的流可以被跟踪。

- TCP 流：重组使用TCP协议的数据，比如HTTP和FTP。
- UDP 流：重组使用UDP协议的数据，比如DNS。
- SSL 流：重组加密的协议，比如HTTPS。你必须提供密钥来解密流量。
- HTTP 流：从HTTP协议中重组和解压数据。当使用TCP流跟踪但又没有完全解码出HTTP数据时，这个功能就派上用场了。

### 数据包长度统计
一个或一组数据包的大小可以让你了解很多情况。在正常情况下，一个以太网上的帧最大长度为1518字节，除去以太网、IP以及TCP头，还剩下1460字节以供应用层协议的头或者数据使用。如果你知道报文传输的最小需求，那么我们就可以通过一个捕获文件中数据包长度的分布情况，做一些对流量的合理猜测。这个技巧对我们尝试理解捕获文件的组成结构十分重要。Wireshark提供了数据包长度窗口，帮助你查看数据包基于其长度的分布情况。

特别注意那些大小为1280～2559字节的数据包统计的行。这些较大的数据包通常表示数据传输，而较小的数据包则表示协议控制序列。

### IO图表
Wireshark的IO图窗口允许你对网络上的吞吐量进行绘图。你可以利用这些图，找到数据吞吐的峰值，找出不同协议中的性能时滞，以及比较实时数据流。

![image](https://github.com/user-attachments/assets/97dcce1f-7163-4cd8-8ae9-7ae3ed3922a7)

这个IO图窗口显示了数据流随时间变化的一个图形化视图。在这个例子中所显示的下载量可知，每个周期大约有500个数据包，其过程中在一定程度上保持不变并在最后逐渐减少。

以上快速下载的IO图基本上是稳定的。反之，慢速下载的IO图特别不稳定：

![image](https://github.com/user-attachments/assets/2586f266-4765-49d0-b69d-37fd8be5e495)

这个下载过程每秒传输的数据包为0～100个，并且浮动很大，其中也曾暂时接近每秒0个数据包。

并排查看多个IO图有助于发现它们之间的差异：

![image](https://github.com/user-attachments/assets/6684c1f6-191a-4eaa-8f8a-d1594182fdc6)

### 流量图/数据流图
TCP流图可以让我们更好地看到整个连接：

![image](https://github.com/user-attachments/assets/eac6ffa3-b809-440d-bc8e-813b22afa0fd)

### 专家信息
Wireshark中每个协议的解析器都有一些专家信息，可以提醒你该协议的数据包中的特定状态。这些状态可以分为4类。

- 对话：关于通信的基本信息。
- 注意：正常通信中的异常数据包。
- 警告：非正常通信中的异常数据包。
- 错误：数据包中的错误，或者解析器解析时的错误。

1．对话消息

窗口更新  由接收者发送，用来通知发送者TCP接收窗口的大小已被改变。

2．注意消息

TCP重传输  数据包丢失的结果。当收到重复的ACK，或者数据包的重传输计时器超时的时候产生。

重复ACK  当一台主机没有收到下一个期望序列号的数据包时，它会生成其最后收到的一个数据的重复ACK。

零窗口探查  在一个零窗口包被发送之后，用来监视TCP接收窗口的状态。

保持活动状态ACK  用来响应保持活动状态数据包。

零窗口探查ACK  用来响应零窗口探查数据包。

窗口已满  用来通知传输主机接收者的TCP接收窗口已满。

3．警告信息

上一段丢失  指明数据包丢失。当数据流中一个期望的序列号被跳过时产生。

收到丢失数据包的ACK  当一个数据包已经确认丢失但仍收到了其ACK数据包时产生。

保持连接状态  当一个连接的保持连接数据包出现时触发。

零窗口  当接收方已经达到TCP接收窗口大小时，会发出一个零窗口通知，要求发送方停止传输数据。

乱序  当数据包乱序被接收时，会利用序列号进行检测。

快速重传输  一次重传会在收到一个重复ACK的20ms内进行。

4．错误消息

没有错误消息

虽然本章中介绍的一些功能看上去只有在偶尔的情况下才会用到，但你可能会发现它们比你想象中要有用得多。你需要熟悉这些窗口和选项，这很重要，因为我会在之后的几个章节中频繁地提到它们。

## 其他用法汇总
来自《性能优化实战》使用上很标准，直接借鉴。（里面还有tcp wrk nginx的干货）

由于网络包的数量比较多，我们可以先过滤一下。比如，在选择一个包后，你可以单击右键并选择 “Follow” -> “TCP Stream”，如下图所示：

![image](https://github.com/user-attachments/assets/e0a2cc96-3b15-4da3-879c-3b47bc6497d4)

然后，关闭弹出来的对话框，回到 Wireshark 主窗口。这时候，你会发现 Wireshark 已经自动帮你设置了一个过滤表达式 tcp.stream eq 24。如下图所示（图中省去了源和目的 IP 地址）：

![image](https://github.com/user-attachments/assets/4bbfdde8-d631-4ab8-8ca8-81a3d9ba5294)

从这里，你可以看到这个 TCP 连接从三次握手开始的每个请求和响应情况。当然，这可能还不够直观，你可以继续点击菜单栏里的 Statics -> Flow Graph，选中 “Limit to display filter” 并设置 Flow type 为 “TCP Flows”：

![image](https://github.com/user-attachments/assets/31107fd1-b821-4752-80d7-15471339cd55)

注意，这个图的左边是客户端，而右边是 Nginx 服务器。通过这个图就可以看出，前面三次握手，以及第一次 HTTP 请求和响应还是挺快的，但第二次 HTTP 请求就比较慢了，特别是客户端在收到服务器第一个分组后，40ms 后才发出了 ACK 响应（图中蓝色行）。

看到 40ms 这个值，你有没有想起什么东西呢？实际上，这是 TCP 延迟确认（Delayed ACK）的最小超时时间。

这里我解释一下延迟确认。这是针对 TCP ACK 的一种优化机制，也就是说，不用每次请求都发送一个 ACK，而是先等一会儿（比如 40ms），看看有没有“顺风车”。如果这段时间内，正好有其他包需要发送，那就捎带着 ACK 一起发送过去。当然，如果一直等不到其他包，那就超时后单独发送 ACK。

因为案例中 40ms 发生在客户端上，我们有理由怀疑，是客户端开启了延迟确认机制。而这儿的客户端，实际上就是前面运行的 wrk。

查询 TCP 文档（执行 man tcp），你就会发现，只有 TCP 套接字专门设置了 TCP_QUICKACK ，才会开启快速确认模式；否则，默认情况下，采用的就是延迟确认机制：
```
TCP_QUICKACK (since Linux 2.4.4)

              Enable  quickack mode if set or disable quickack mode if cleared.  In quickack mode, acks are sent imme‐

              diately, rather than delayed if needed in accordance to normal TCP operation.  This flag is  not  perma‐

              nent,  it only enables a switch to or from quickack mode.  Subsequent operation of the TCP protocol will

              once again enter/leave quickack mode depending on internal  protocol  processing  and  factors  such  as

              delayed ack timeouts occurring and data transfer.  This option should not be used in code intended to be

              portable.
```

为了验证我们的猜想，确认 wrk 的行为，我们可以用 strace ，来观察 wrk 为套接字设置了哪些 TCP 选项。

比如，你可以切换到终端二中，执行下面的命令：

```
$ strace -f wrk --latency -c 100 -t 2 --timeout 2 http://192.168.0.30:8080/

...

setsockopt(52, SOL_TCP, TCP_NODELAY, [1], 4) = 0
...

```

这样，你可以看到，wrk 只设置了 TCP_NODELAY 选项，而没有设置 TCP_QUICKACK。这说明 wrk 采用的正是延迟确认，也就解释了上面这个 40ms 的问题。

不过，别忘了，这只是客户端的行为，按理来说，Nginx 服务器不应该受到这个行为的影响。那是不是我们分析网络包时，漏掉了什么线索呢？让我们回到 Wireshark 重新观察一下。

![image](https://github.com/user-attachments/assets/858e2b3c-30d1-4535-9667-0e436e11cc1f)

仔细观察 Wireshark 的界面，其中， 1173 号包，就是刚才说到的延迟 ACK 包；下一行的 1175 ，则是 Nginx 发送的第二个分组包，它跟 697 号包组合起来，构成一个完整的 HTTP 响应（ACK 号都是 85）。

第二个分组没跟前一个分组（697 号）一起发送，而是等到客户端对第一个分组的 ACK 后（1173 号）才发送，这看起来跟延迟确认有点像，只不过，这儿不再是 ACK，而是发送数据。

看到这里，我估计你想起了一个东西—— Nagle 算法（纳格算法）。进一步分析案例前，我先简单介绍一下这个算法。

Nagle 算法，是 TCP 协议中用于减少小包发送数量的一种优化算法，目的是为了提高实际带宽的利用率。

举个例子，当有效负载只有 1 字节时，再加上 TCP 头部和 IP 头部分别占用的 20 字节，整个网络包就是 41 字节，这样实际带宽的利用率只有 2.4%（1/41）。往大了说，如果整个网络带宽都被这种小包占满，那整个网络的有效利用率就太低了。

Nagle 算法正是为了解决这个问题。它通过合并 TCP 小包，提高网络带宽的利用率。Nagle 算法规定，一个 TCP 连接上，最多只能有一个未被确认的未完成分组；在收到这个分组的 ACK 前，不发送其他分组。这些小分组会被组合起来，并在收到 ACK 后，用同一个分组发送出去。

显然，Nagle 算法本身的想法还是挺好的，但是知道 Linux 默认的延迟确认机制后，你应该就不这么想了。因为它们一起使用时，网络延迟会明显。如下图所示：

![image](https://github.com/user-attachments/assets/652aa34b-e8bd-432b-b1e9-88f7a6b0e929)

- 当 Sever 发送了第一个分组后，由于 Client 开启了延迟确认，就需要等待 40ms 后才会回复 ACK。
- 同时，由于 Server 端开启了 Nagle，而这时还没收到第一个分组的 ACK，Server 也会在这里一直等着。
- 直到 40ms 超时后，Client 才会回复 ACK，然后，Server 才会继续发送第二个分组。

既然可能是 Nagle 的问题，那该怎么知道，案例 Nginx 有没有开启 Nagle 呢？

查询 tcp 的文档，你就会知道，只有设置了 TCP_NODELAY 后，Nagle 算法才会禁用。所以，我们只需要查看 Nginx 的 tcp_nodelay 选项就可以了。

```
TCP_NODELAY

              If set, disable the Nagle algorithm.  This means that segments are always sent as soon as possible, even

              if there is only a small amount of data.  When not set, data is buffered until  there  is  a  sufficient

              amount  to  send out, thereby avoiding the frequent sending of small packets, which results in poor uti‐

              lization of the network.  This option is overridden by TCP_CORK; however, setting this option forces  an

              explicit flush of pending output, even if TCP_CORK is currently set.
```

我们回到终端一中，执行下面的命令，查看案例 Nginx 的配置：
```
$ docker exec nginx cat /etc/nginx/nginx.conf | grep tcp_nodelay

    tcp_nodelay    off;
```

果然，你可以看到，案例 Nginx 的 tcp_nodelay 是关闭的，将其设置为 on ，应该就可以解决了。

改完后，问题是否就解决了呢？自然需要验证我们一下。修改后的应用，我已经打包到了 Docker 镜像中，在终端一中执行下面的命令，你就可以启动它：
```
# 删除案例应用

$ docker rm -f nginx


# 启动优化后的应用

$ docker run --name nginx --network=host -itd feisky/nginx:nodelay

```

接着，切换到终端二，重新执行 wrk 测试延迟：
```
$ wrk --latency -c 100 -t 2 --timeout 2 http://192.168.0.30:8080/

Running 10s test @ http://192.168.0.30:8080/

  2 threads and 100 connections

  Thread Stats   Avg      Stdev     Max   +/- Stdev

    Latency     9.58ms   14.98ms 350.08ms   97.91%

    Req/Sec     6.22k   282.13     6.93k    68.50%

  Latency Distribution

     50%    7.78ms

     75%    8.20ms

     90%    9.02ms

     99%   73.14ms

  123990 requests in 10.01s, 100.50MB read

Requests/sec:  12384.04

Transfer/sec:     10.04MB
```

果然，现在延迟已经缩短成了 9ms，跟我们测试的官方 Nginx 镜像是一样的（Nginx 默认就是开启 tcp_nodelay 的） 。

作为对比，我们用 tcpdump ，抓取优化后的网络包（这儿实际上抓取的是官方 Nginx 监听的 80 端口）。你可以得到下面的结果：

![image](https://github.com/user-attachments/assets/b7c72b8e-4ff1-4d81-ba0d-e4d63af120c4)

从图中你可以发现，由于 Nginx 不用再等 ACK，536 和 540 两个分组是连续发送的；而客户端呢，虽然仍开启了延迟确认，但这时收到了两个需要回复 ACK 的包，所以也不用等 40ms，可以直接合并回复 ACK。

## 其他笔记 wireshark 基本用法
Wireshark 既可以分析抓包文件，也可以直接用来抓包，起到跟 tcpdump 类似的作用。 而且比 tcpdump 方便的是，如果直接用 Wireshark 发起抓包，窗口里就直接显示抓取到 的报文了，这省去了 tcpdump 抓包后，再用 Wireshark 打开的小小的麻烦。

怎么知道抓包文件是在哪一端抓取的？

这是一个有趣的问题。尽管我们做抓包的时候很清楚是在哪端做了抓包，但是把文件传给 别人后，对方就未必知道这一点，甚至我们自己过段时间也迷糊了：我上次这个抓包文件 到底是在客户端上，还是服务端上抓取的？

要搞清楚这一点也很简单，我们可以利用 IP 的 TTL 属性。显然，无论是哪一端，它的报 文在发出时，其 TTL 就是原始值，也就是 64、128、255 中的某一个。而对端报文的 TTL，因为会经过若干个网络跳数，所以一般都会比 64、128、255 这几个数值要小一 些。

所以，我们只要看一下抓包文件中任何一个客户端报文（也就是源端口为高端口）的 TTL，如果它是 64、128 或 255，那说明这个抓包文件就是在客户端做的。反之，就是在服务端做的。

![image](https://github.com/user-attachments/assets/a51ee655-3f9d-4f03-a6b6-cf84a8ad409f)

如何定位到应用层的请求和返回的报文？

在众多报文中找到应用层请求和对应的响应报文，这个任务用人工去做的话是很繁琐的。 还好，用 Wireshark 就很方便。在 Wireshark 界面中，我们很容易找到请求和返回的报 文。比如这样：

![image](https://github.com/user-attachments/assets/8c5c06d3-da74-4de4-a2c2-b10cb1a38540)

我们只要选中请求报文，Wireshark 就会自动帮我们匹配到对应的响应报文，反过来也一 样。从图上看，应用层请求（这里是 HTTP 请求）是一个向右的箭头，表示数据是进来的 方向；应用层响应是一个向左的箭头，表示数据是出去的方向。

只截到报文的一部分，这个问题有什么影响吗？

![image](https://github.com/user-attachments/assets/efe713d5-b6a1-4895-9f26-b3c9a166e0f2)

实际上，这不是什么大的问题，并不影响整体的抓包分析。造成这个报错的原因是，当时 tcpdump 程序并不是用 Ctrl+C 等正常方式终止的，而是可能用了操作系统的 SIGKILL 信 号，比如用了 kill -9 命令。这样的话，tcpdump 就被强行终止了，导致一部分被抓取的报文还在内存中，还没来得及由 tcpdump 程序正常写入到 pcap 文件里。

要避免这个报错，我们可以在停止 tcpdump 时，用正常退出的方式，比如 Ctrl+C，或者 timeout 命令，或者 kill 但不要用 -9 作为参数。

乱序一定会引起问题吗？

乱序（Out-of-Order），也是我们时常能在 Wireshark 里看到的一类现象。那么，乱序一 定会引起问题吗？有一句话叫“脱离了剂量谈毒性就是耍流氓”。其实，乱序是否是问 题，也取决于乱序的严重程度。

那第二个问题随之就来了：乱序包达到什么程度，就会真的引起问题？

这个问题，跟实际场景的具体情况也有很大的关系，不同的操作系统以及 TCP 实现细节， 都可能有挺大的差异。不过，我还是想正面回答一下这个问题。我的经验是，如果乱序报文达到 10% 以上，就是严重的传输质量问题了，甚至可能导致传输失败，或者应用层的各种卡顿、报错等症状。所以，你可以统计一下乱序包的占比，如果它超过了 10%，就要重视了。
