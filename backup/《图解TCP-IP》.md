> **读书笔记**

![image](https://github.com/user-attachments/assets/821d2d2a-5db3-4c32-a15a-13cc353ed717)

![image](https://github.com/user-attachments/assets/9ee728fc-2c24-45ce-a0c3-019cd85506b4)

![image](https://github.com/user-attachments/assets/3eba23d3-f516-477f-a82a-47de0f29ba9c)

浏览器与服务端之间通信所用的协议是HTTP（HyperText Transfer Protocol）。所传输数据的主要格式是HTML（HyperText Markup Language）。WWW中的HTTP属于OSI应用层的协议，而HTML属于表示层的协议。

![image](https://github.com/user-attachments/assets/d4fda705-f74e-4a2a-86e4-647aa8e42833)

经过每个协议分层时，都必须有识别包发送端和接收端的信息。以太网会用MAC地址，IP会用IP地址，而TCP/UDP则会用端口号作为识别两端主机的地址。即使是在应用程序中，像电子邮件地址这样的信息也是一种地址标识。这些地址信息都在每个包经由各个分层时，附加到协议对应的包首部里边。

在全世界，MAC地址也并不总是唯一的。实际上，即使MAC地址相同，只要不是同属一个数据链路就不会出现问题。

以太网等数据链路中使用MAC地址传输数据帧。此时的一跳是指从源MAC地址到目标MAC地址之间传输帧的区间。也就是说它是主机或路由器网卡不经其他路由器而能直接到达的相邻主机或路由器网卡之间的一个区间。

**生存时间（TTL：Time To Live）**：
由8比特构成，它最初的意思是以秒为单位记录当前包在网络上应该生存的期限。然而，在实际中它是指可以中转多少个路由器的意思。每经过一个路由器，TTL会减少1，直到变成0则丢弃该包（TTL占8位，因此可以表示0～255的数字。因此一个包的中转路由的次数不会超过28 =256个。由此可以避免IP包在网络内无限传递的问题。） 。

有一款充分利用ICMP超时消息的应用叫做traceroute（在UNIX、MacOS中是这个命令，而在Windows中对等的命令叫做tracert。） 。它可以显示出由执行程序的主机到达特定主机之前历经多少路由器。它的原理就是利用IP包的生存期限从1开始按照顺序递增的同时发送UDP包，强制接收ICMP超时消息的一种方法。

![image](https://github.com/user-attachments/assets/e7ea54c1-3384-4859-948b-0da0cb794b93)
