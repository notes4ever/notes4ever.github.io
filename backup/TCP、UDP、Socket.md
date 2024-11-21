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