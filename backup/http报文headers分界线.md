HTTP 报文内容，分成了头部（headers）和载荷（Payload 或者 body）两部分；HTTP 规定，**头部和载荷的分界线是两次 CRLF**。

也就是在最后一个 header 之后，需要有两个 CRLF，这就是头部和载荷之间的分割线。

之后就是载荷（message body）的开始了。

可能引发 HTTP 400 的 PUT 请求，其 Authorization 后面也出现了两个 CRLF，这就会被认为是 headers 的结束，payload 的开始。

但实际上，后面跟的又是剩余的 HTTP 头部项，在最后一个头部之后，又是两个 CRLF。

所以这对于 Web 服务端来说就懵了：“你这说的可不是人话啊！我只能表示我不理解。” 

400 Bad Request 的根因，是**客户发送的 HTTP PUT 请求的格式出现了问题**。

它违背了 HTTP/1.1（RFC2616）的规定，在 Authorization 头部后面，错误地添加了两次回车（CRLF）。

这样就导致服务端认为，后续的数据都属于 payload，也就导致服务器无法正常读取这个请求，只能用 HTTP 400 来反馈这种状况了。

发送http header，为什么不标注为http请求，而是psh ack呢?

没有被标注为http的原因是，这个报文并不是完整的http请求，只是前一半（headers）。

**标记为PSH的话，是因为这个报文就是当时客户端的发送队列里最后一个报文，内核会对此加上PSH标志位，这个行为不是应用程序控制的**。