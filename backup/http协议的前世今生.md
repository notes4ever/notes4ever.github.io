## HTTP版本演进
HTTP 的英文全称是 Hypertext Transfer Protocol，中文是超文本传输协议，它的奠基者 是英国计算机科学家蒂姆·博纳斯·李（Tim Berners-Lee）。1990 年，他为了解决任职的 欧洲核子研究组织（CERN）里，科学家们无法方便地分享文件和信息的问题，由此创造了 HTTP 协议。

实际上，在当时也有其他一些协议能实现信息共享的功能，比如 FTP、SMTP、NNTP 等，为什么还要另外创造 HTTP 呢？这是因为这些协议并不满足博纳斯·李的需求，比如：

- FTP 只是用来传输和获取文件，它无法方便地展示文本和图片；
- NNTP 用来传输新闻，但不适合展示存档资料；
- SMTP 是邮件传输协议，缺乏目录结构。

而博纳斯·李需要的是“图形化的、只要点击一下就能进入到其他资料的系统”。鉴于以上 协议无法实现，他就设计了 HTTP。也因为这个巨大的贡献，博纳斯·李获得了 2016 年的 图灵奖，可以说是图灵奖的一次“回国”。

在 2015 年之前，HTTP 先后有 0.9、1.0、1.1 三个版本，其中 HTTP/1.0 和 1.1 合称 HTTP/1.x。虽然谷歌在 2009 年就提出了 SPDY，但最终被接纳成为 HTTP/2，也已经是 2015 年的事了。最近几年蓬勃发展的还有 HTTP/3（也就是 QUIC 上的 HTTP/2）。但从语义上说，HTTP/2 跟 HTTP/1.x 是保持一致的。HTTP/2 不同，主要是在传输过程中， 在 TCP 和 HTTP 之间，增加了一层传输方面的逻辑。

补充：  RFC7540定义了 HTTP/2 的协议规范，而 HTTP/1.1 在 1999 年 6 月的RFC2616里已经确定了大部分内容。
什么叫做“语义上是一致的”呢？举个例子，在 HTTP/2 里面，header 和 body 的定义和 规则，就跟 HTTP/1.x 一样。比如 User-agent: curl/7.68.0 这样一个 header，在HTTP/1.x 里是代表了这次访问的客户端的名称和版本，而在 HTTP/2 里，依然是这个含 义，没有任何变化。从这一点上看，你甚至可以把 HTTP/2 理解为是在 HTTP/1.x 的语义的基础上，增加了一 个介于 TCP 和 HTTP 之间的新的“传输层”。也就是下图这样：

![image](https://github.com/user-attachments/assets/50580ad8-2d14-4915-8352-01441fa765f5)

目前最新的 HTTP/3 仍在讨论过程中，还未正式发布。它也依然保持了之前版本 HTTP 的语义，但在传输层上做了彻底的“革命”：把传输层协议从 TCP 换成了 UDP。根据 w3techs.com（一家网络技术调查网站）的数据，截至 2022 年 2 月 22 日，有 25.2% 的站点已经支持了 HTTP/3。

## HTTP/2 的多路复用

HTTP/2 的多路复用（Multiplexing）是其核心特性之一，它允许在单个 TCP 连接上并行交错地发送多个请求和响应，从而显著提高了性能和减少了延迟。下面是多路复用的工作方式：

- 单一TCP连接：在 HTTP/1.1 中，每个请求通常需要建立自己的 TCP 连接。而 HTTP/2 允许多个请求和响应在单一的 TCP 连接上进行传输。
- 流和帧：HTTP/2 将数据封装在“帧”中，而每个帧属于一个“流”。流是 HTTP/2 中用于传输数据的逻辑通道，每个流可以承载一个请求-响应序列。
- 交错传输：在 HTTP/2 中，服务器和客户端可以在同一个 TCP 连接上交错地发送帧。这意味着请求和响应的帧可以相互交织在一起，而不是像 HTTP/1.1 那样必须等待前一个请求完成后才能发送下一个请求。
- 流优先级：HTTP/2 允许为每个流设置优先级，这样浏览器就可以优先处理更重要的资源，如页面的首屏加载资源。
- 流量控制：虽然多路复用可以提高性能，但也可能导致某些流的数据被其他流的数据淹没。HTTP/2 通过流量控制机制来防止这种情况，允许接收方控制发送方发送数据的速度。
- 头压缩：HTTP/2 引入了 HPACK 压缩格式，对请求和响应的头部进行压缩。由于头部信息通常在多个请求中是相似的，这种压缩可以显著减少传输的数据量。
- 并行请求：客户端可以在建立 TCP 连接后立即发送多个请求，而不需要等待响应。这减少了连接建立的延迟，因为不需要为每个请求单独建立连接。
- 无阻塞：由于请求和响应是交错传输的，一个流的延迟不会阻塞其他流。这解决了 HTTP/1.1 中的队头阻塞问题，提高了资源加载的效率。
- 资源优化：服务器可以更有效地利用可用的带宽，因为它可以同时发送多个小文件，而不是一次只发送一个。
- 更好的缓存利用：由于请求和响应可以在同一个连接上并行传输，缓存的效率得到了提高，因为浏览器可以在等待一个资源加载完成的同时，开始加载其他资源。

总的来说，HTTP/2 的多路复用通过在单个连接上并行传输多个请求和响应，减少了延迟，提高了吞吐量，并优化了网络资源的使用。

## 参考资料

- https://coolshell.cn/articles/19840.html