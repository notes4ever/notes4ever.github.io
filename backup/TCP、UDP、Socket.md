> **核心网络知识，学习笔记汇总和摘抄**

## 三次握手示意图
![image](https://github.com/user-attachments/assets/446190eb-3b34-474e-8b42-e91ee2ae56b2)

## 四次挥手示意图
![image](https://github.com/user-attachments/assets/97335f72-044f-4c34-af70-ba882f12791f)

## 三次握手+四次挥手
![image](https://github.com/user-attachments/assets/445f9f37-268e-4865-8790-2c656756335c)

## 关于三次挥手
实际挥手过程可能只有三个包，而不是四个。之所以有三个包，是因为服务器端收到客户端的 FIN 后，服务器端同时也要关闭连接，这样就可以**把 ACK 和 FIN 合并到一起发送，节省了一个包**，变成了“三次挥手”。 

而**通常情况**下，服务器端收到客户端的 FIN 后，很可能还没发送完数据，所以就**会先回复客户端一个 ACK 包**。**稍等一会儿，完成所有数据包的发送后，才会发送 FIN 包**。这也就是四次挥手了。

## 更加详细的三次握手细节
![image](https://github.com/user-attachments/assets/7601c0db-5ee4-461a-aebe-12d9914f4c28)

关注：半连接队列与全连接队列。

## 连接队列满了的场景
![image](https://github.com/user-attachments/assets/65ff4037-cb06-4320-a340-47ecb4544cb0)

如果半连接队列满了，⽽且ipv4.tep_syncookies参数设置为0，那么来⾃客户端的握⼿包将goto drop， 意思就是直接丢弃！

SYN Flood攻击就是通过**耗光服务端上的半连接队列来使得正常的⽤户连接请求⽆法被响应**。不过在现在的Linux内核⾥只要打开tcp_ syncookies，半连接队列满了仍然可以保证正常握⼿的进⾏。

当半连接队列判断通过以后，紧接着还有全连接队列满的相关判断。如果满了，服务器对握⼿包的处理还是会goto drop，丢奔它。

技术细节总结，顺便回答问题：服务端半/全连接队列满了会怎么样？

- 服务端响应第一次握手的时候会进行半连接队列和全连接队列是否满的判断
    - 如果半连接队列满了且未开启tcp_syncookies，丢弃握手包
    - 如果全连接队列满了且存在young_acck，丢弃握手包
- 服务端响应第三次握手的时候会进行全连接队列是否满的判断
如果全连接队列满了则丢弃握手包

假如服务端第⼀次握⼿的时候出现了半/全连接队列溢出导致的丢包，那么我们的接口响应时间将⾄少是1秒以上（在某些⽼版本的内核上，SYN第⼀次的重试就需要等3 秒)，⽽正常的在同机房的情况下只是不到1毫秒的事情，整整⾼了1000倍左右。

如果连续两三次握⼿都失败，那七⼋秒就出去了，很可能Nginx等不及⼆次重试，这个⽤户访问直接就超时了，⽤户体验将会受到较⼤影响。

对于半连接队列来说，只要保证tcp syncookies这个内核参数是1就能保证不会有因为半连接队列满⽽发⽣的丢包。

全连接满了如何查看：
```
watch 'netstat -s 1 grep overﬂowed'
```

## 端口不足导致cpu飙升的场景
有一个场景是，connect系统调⽤的CPU⼤幅度上涨。又经过追查发现根本原因是事发当时可⽤端口⼜不是特别充⾜。端口数量和CPU消耗这⼆者貌似没啥关联呀，为啥端口不⾜会导致CPU消耗⼤幅上涨呢？

客户端在发起connect系统调⽤的时候，服务端主要⼯作就是端口选择。**在选择的过程中， 有个⼤循环，以ip_local_ port range的⼀个随机位置开始把这个范围遍历⼀遍，找到可⽤端口则退出循环。**

如果端口很充⾜，那么循环只需要执⾏少数⼏次就可以退出。但假设端口消耗掉很多已经不充⾜，或者⼲脆就没有可⽤的了，那么这个循环就得执⾏很多遍。

## 一台机器可以开多少个tcp连接
一台机器最多能支持多少条 TCP 连接? 这个公式是什么？怎么计算，64GB 的 Linux 服务器，可以支持多少条？

答：只说服务器端，一是受限于 Linux 里配置的**可打开文件句柄数等内核参数**，但这些都很好修改，调几个参数就行了。二就是**内存**了，这个是硬性限制。最少也得是 3.3 KB 左右。所以如果算极限情况下的TCP连接数，64 GB 除以 3.3KB 就行了。但一般都会给收发缓存区以及其它应用留一些 buffer。

总结：受限于机器的内核参数和机器的内存资源，**一条tcp大概是3.3KB**。

其他类似的问题：

一条 TCP 连接需要消耗多大的内存 ，像mysql这种连接数，和用户登录这种 session 信息，也是一条 tcp 连接吗？

答：TCP 连接在内核里消耗内存主要是两块，一是表示 TCP 连接的 socket，大约是 3.3K 左右。二是**接收和发送数据的缓存区**，这个可大可小，大了收发速度更快，小了更省内存。但在较新的版本里，只要收发结束，内存就都可以回收了。

你说的 mysql 的用户登录、session 等信息这个数据 TCP 里传输的数据。这个数据就看用户进程里是怎么存储的了，如果存在内存里就占内存，存到磁盘里就占磁盘。

## 应用程序与tcp协议栈交互
![image](https://github.com/user-attachments/assets/f33df279-0f68-4097-8492-9cff1b935da5)

## tcp_window_scaling
TCP窗口缩放机制在所有主要平台上都是**默认启用**的。

不过，**中间节点和路由器可以重写，甚至完全去掉这个选项**。

如果你的服务器或客户端的连接不能完全利用现有带宽，那往往该先查一查窗口大小。

在Linux中，可以通过如下命令检查和启用窗口缩放选项：
```
sysctl net.ipv4.tcp_window_scaling
```
临时设置，重启失效：
```
sysctl -w net.ipv4.tcp_window_scaling=1 
```

## TCP对数据包的应答细节
为了保证顺序性，每一个包都有一个 ID。在建立连接的时候，会商定起始的 ID 是什么，然后按照 ID 一个个发送。

为了保证不丢包，对于发送的包都要进行应答，但是这个应答也不是一个一个来的，而是会应答某个之前的 ID，表示都收到了，这种模式称为累计确认或者累计应答（cumulative acknowledgment）。

## tcp stream与datagram
![image](https://github.com/user-attachments/assets/07de5961-8ad8-45c6-8c8b-9fc671922565)

## TCP超时与重传
超时重传：

TCP 对于每条连接都维护了一个超时计时器，当数据发送出去后一定时限内还没有收到确认，就认为是发生了超时，然后重传这部分数据。

- RTO 的初始值是 1 秒（在发送 SYN 但未收到 SYN+ACK 阶段）。
- 在连接建立后，TCP 会动态计算出 TRO。
- RTO 有上限值和下限值，常见值分别为 2 分钟和 200ms。实际场景中，RTO 为 200ms 出头最为常见。

快速重传：

快速重传的触发条件是：收到 3 个或者 3 个以上的重复确认报文（DupAck）。

在快速重传中，SACK（选择性确认）也起到了避免一部分已经到达的数据被重传。不过，也由于 TCP 头部长度的限制，SACK 只能放置 4 个块，再多也不行了。

快速重传只要 3 个 DupAck 就可以触发，实际上我们还可能观察到远多于 3 个 DupAck 的情况，这也是正常现象。

还有一种Spurious 重传，对 TCP 传输的影响比快速重传和超时重传小很多，总体来说是一种影响不大的重传。

另外，在案例拆解的过程中，我们也进一步学习了 Wireshark 的使用技巧，包括：

- Wireshark 里的信息，是需要跟你“抓包发生在哪一侧”这个信息，结合起来解读的。这对你的排查会起到很关键的作用。
- 如何定位到被重传的原始报文的方法：先找到重传报文的序列号，然后到前面找到同样这个序列号的报文。
- 如果在专家信息里看到 New fragment overlaps old data (retransmission?)，这意味着多个报文之间的数据有重叠，但一般不是严重的问题。
- Wireshark 提示的 (suspected) fast retransmission 就是快速重传报文。
- Wireshark 提示的 (suspected) retransmission 就是超时重传报文。
- 如果发现有数据报文和 DupAck 数量不对等的情况，可以看一下是否有 TSO 的存在。

关于TSO：TCP Segmentation Offload（TSO）是一种利用网口硬件协助TCP大数据包分段分送的技术，通过将TCP分段过程从操作系统内核转移到网络适配器上，实现了数据传输的优化，减轻了CPU负担，提高了网络性能和系统吞吐量。

## 滑动窗口与拥塞窗口
顺序问题、丢包问题、流量控制都是通过滑动窗口来解决的，这其实就相当于你领导和你的工作备忘录，布置过的工作要有编号，干完了有反馈，活不能派太多，也不能太少；

拥塞控制是通过拥塞窗口来解决的，相当于往管道里面倒水，快了容易溢出，慢了浪费带宽，要摸着石头过河，找到最优值。

TCP **BBR 拥塞算法**。它企图找到一个平衡点，就是通过不断的加快发送速度，将管道填满，但是不要填满中间设备的缓存，因为这样时延会增加，在这个平衡点可以很好的达到高带宽和低时延的平衡。

![image](https://github.com/user-attachments/assets/3ba99961-47f1-4f1d-a3fb-16414dec34a1)

## 基于 TCP 和 UDP 协议的 Socket 编程
Socket 这个名字很有意思，可以作插口或者插槽讲。虽然我们是写软件程序，但是你可以**想象为弄一根网线，一头插在客户端，一头插在服务端，然后进行通信**。所以在通信之前，双方都要建立一个 Socket。

在建立 Socket 的时候，应该设置什么参数呢？Socket 编程进行的是端到端的通信，往往意识不到中间经过多少局域网，多少路由器，因而能够设置的参数，也只能是端到端协议之上**网络层和传输层**的。

在网络层，Socket 函数需要指定到底是 IPv4 还是 IPv6，分别对应设置为 AF_INET 和 AF_INET6。

在传输层，还要指定到底是 TCP 还是 UDP。还记得咱们前面讲过的，TCP 协议是基于数据流的，所以设置为 SOCK_STREAM，而 UDP 是基于数据报的，因而设置为 SOCK_DGRAM。

### 基于 TCP 的 Socket 程序函数调用过程
两端创建了 Socket 之后，接下来的过程中，TCP 和 UDP 稍有不同，我们先来看 TCP。

TCP 的服务端要先监听一个端口，一般是先调用 bind 函数，给这个 Socket 赋予一个 IP 地址和端口。

为什么需要端口呢？要知道，你写的是一个应用程序，当一个网络包来的时候，内核要通过 TCP 头里面的这个端口，来找到你这个应用程序，把包给你。

为什么要 IP 地址呢？有时候，一台机器会有多个网卡，也就会有多个 IP 地址，你可以选择监听所有的网卡，也可以选择监听一个网卡，这样，只有发给这个网卡的包，才会给你。

当服务端有了 IP 和端口号，就可以调用 listen 函数进行监听。在 TCP 的状态图里面，有一个 listen 状态，当调用这个函数之后，服务端就进入了这个状态，这个时候客户端就可以发起连接了。

在内核中，为每个 Socket 维护两个队列。一个是已经建立了连接的队列，这时候连接三次握手已经完毕，处于 established 状态；一个是还没有完全建立连接的队列，这个时候三次握手还没完成，处于 syn_rcvd 的状态。

接下来，服务端调用 accept 函数，拿出一个已经完成的连接进行处理。如果还没有完成，就要等着。

在服务端等待的时候，客户端可以通过 connect 函数发起连接。先在参数中指明要连接的 IP 地址和端口号，然后开始发起三次握手。

内核会给客户端分配一个临时的端口。一旦握手成功，服务端的 accept 就会返回另一个 Socket。

这是一个经常考的知识点，**就是监听的 Socket 和真正用来传数据的 Socket 是两个，一个叫作监听 Socket，一个叫作已连接 Socket。**

连接建立成功之后，双方开始通过 read 和 write 函数来读写数据，就像往一个文件流里面写东西一样。

这个图就是基于 TCP 协议的 Socket 程序函数调用过程。

![image](https://github.com/user-attachments/assets/a1a773a9-cbcd-4648-b68a-4b68779ccfdf)

说 TCP 的 Socket 就是一个文件流，是非常准确的。因为，Socket 在 Linux 中就是以文件的形式存在的。除此之外，还存在文件描述符。写入和读出，也是通过文件描述符。

在内核中，Socket 是一个文件，那对应就有文件描述符。每一个进程都有一个数据结构 task_struct，里面指向一个文件描述符数组，来列出这个进程打开的所有文件的文件描述符。文件描述符是一个整数，是这个数组的下标。

这个数组中的内容是一个指针，指向内核中所有打开的文件的列表。既然是一个文件，就会有一个 inode，只不过 Socket 对应的 inode 不像真正的文件系统一样，保存在硬盘上的，而是在内存中的。在这个 inode 中，指向了 Socket 在内核中的 Socket 结构。

在这个结构里面，主要的是两个队列，一个是发送队列，一个是接收队列。在这两个队列里面保存的是一个缓存 sk_buff。这个缓存里面能够看到完整的包的结构。看到这个，是不是能和前面讲过的收发包的场景联系起来了？

![image](https://github.com/user-attachments/assets/b7fb77f9-e693-429f-97d7-14324671a975)

### 基于 UDP 的 Socket 程序函数调用过程
对于 UDP 来讲，过程有些不一样。UDP 是没有连接的，所以不需要三次握手，也就不需要调用 listen 和 connect，但是，UDP 的的交互仍然需要 IP 和端口号，因而也需要 bind。

UDP 是没有维护连接状态的，因而不需要每对连接建立一组 Socket，而是只要有一个 Socket，就能够和多个客户端通信。也正是因为没有连接状态，每次通信的时候，都调用 sendto 和 recvfrom，都可以传入 IP 地址和端口。

这个图的内容就是基于 UDP 协议的 Socket 程序函数调用过程。

![image](https://github.com/user-attachments/assets/e12b7eb8-db6e-48ee-b593-3c1ebd77fc9a)

## 大量连接高并发处理
写一个能够支撑大量连接的高并发的服务端不容易，需要多进程、多线程，而 epoll 机制能解决 C10K 问题。

服务器通常固定在某个本地端口上监听，等待客户端的连接请求。

服务端端 TCP 连接四元组中只有对端 IP, 也就是客户端的 IP 和对端的端口，也即客户端的端口是可变的，因此，最大 TCP 连接数 = 客户端 IP 数×客户端端口数。

对 IPv4，客户端的 IP 数最多为 2 的 32 次方，客户端的端口数最多为 2 的 16 次方，也就是服务端单机最大 TCP 连接数，约为 2 的 48 次方。

当然，服务端最大并发 TCP 连接数远不能达到理论上限。首先主要是文件描述符限制，按照上面的原理，Socket 都是文件，所以首先要通过 ulimit 配置文件描述符的数目；另一个限制是内存，按上面的数据结构，每个 TCP 连接都要占用一定内存，操作系统是有限的。

上面基于进程或者线程模型的，其实还是有问题的。新到来一个 TCP 连接，就需要分配一个进程或者线程。

一台机器无法创建很多进程或者线程。有个C10K，它的意思是一台机器要维护 1 万个连接，就要创建 1 万个进程或者线程，那么操作系统是无法承受的。如果维持 1 亿用户在线需要 10 万台服务器，成本也太高了。

解决方案是：

在linux上是epoll。

epoll，它在内核中的实现不是通过轮询的方式，而是通过注册 callback 函数的方式，当某个文件描述符发送变化的时候，就会主动通知。

![image](https://github.com/user-attachments/assets/81fcfe22-9c35-444e-9bfb-10cd109814b8)

如图所示，假设进程打开了 Socket m, n, x 等多个文件描述符，现在需要通过 epoll 来监听是否这些 Socket 都有事件发生。其中 epoll_create 创建一个 epoll 对象，也是一个文件，也对应一个文件描述符，同样也对应着打开文件列表中的一项。

在这项里面有一个红黑树，在红黑树里，要保存这个 epoll 要监听的所有 Socket。当 epoll_ctl 添加一个 Socket 的时候，其实是加入这个红黑树，同时红黑树里面的节点指向一个结构，将这个结构挂在被监听的 Socket 的事件列表中。当一个 Socket 来了一个事件的时候，可以从这个列表中得到 epoll 对象，并调用 call back 通知它。

这种通知方式使得监听的 Socket 数据增加的时候，效率不会大幅度降低，能够同时监听的 Socket 的数目也非常的多了。上限就为系统定义的、进程打开的最大文件描述符个数。因而，epoll 被称为解决 C10K 问题的利器。


## 参考资料

- 《极客时间 趣谈网络协议 作者 刘超》
- 《深入理解Linux网络 作者 张彦飞》
- [TCP Fast Open](https://dbwu.tech/posts/network/what-is-tcp-fast-open/)
- [TCP 的那些事儿（上）](https://coolshell.cn/articles/11564.html)
- [TCP 的那些事儿（下）](https://coolshell.cn/articles/11609.html)
- [从一次经历谈 TIME_WAIT 的那些事](https://coolshell.cn/articles/22263.html)
- [TCP网络关闭的状态变换时序图](https://coolshell.cn/articles/1484.html)
- [TCP协议中三次握手和四次挥手](https://joytsing.cn/posts/42952/)
- [深入理解Linux网络学习笔记(一)](https://joytsing.cn/posts/20149/)
- [深入理解Linux网络学习笔记(二)](https://joytsing.cn/posts/36369/)
- [深入理解Linux网络学习笔记(三)](https://joytsing.cn/posts/7859/)
- [深入理解Linux网络学习笔记(四)](https://joytsing.cn/posts/34895/)
- [深入理解Linux网络学习笔记(五)](https://joytsing.cn/posts/36379/)
- [深入理解Linux网络学习笔记(六)](https://joytsing.cn/posts/11832/)
- [深入理解Linux网络学习笔记(七)](https://joytsing.cn/posts/48821/)