在默认主机里面加上location或者你希望能访问到的主机里面加上如下配置：
```
location /status 
{
        stub_status on;
        access_log off;            
}
```

重启nginx
在浏览器中输入nginx的地址：`http://127.0.0.1/status`，即可查看nginx的状态信息:

![image](https://github.com/user-attachments/assets/fc1219ba-c5f5-4739-b3a3-5fc8b297cf60)

**Active connections – 活跃的连接数量**
server accepts handled requests — 总共处理了7个连接 , 成功创建7次握手, 总共处理了36个请求。

- reading — 正在读取http请求头的连接总数。也有解释为：nginx读取到客户端的Header信息数，倾向前者的解释
- writing — 正在向客户端发送响应的连接总数。也有解释为：nginx返回给客户端的Header信息数，倾向前者的解释
- waiting — 当前空闲的http keep-alive连接总数。开启 keep-alive 的情况下,这个值等于 active – (reading+writing), 意思就是nginx已经处理完成，正在等候下一次请求的驻留连接（空闲持久连接）。

对以上waiting连接数的理解：

HTTP/1.1（以及 HTTP/1.0 的各种增强版本）允许 HTTP 设备在事务处理结束之后将 TCP 连接保持在打开状态，以便为未来的 HTTP 请求重用现存的连接。在事务处理结束之后仍然保持在打开状态的 TCP 连接被称为持久连接。非持久连接会在每个事务结束之后关闭。持久连接会在不同事务之间保持打开状态，直到客户端或服务器决定将其关闭为止。重用已对目标服务器打开的空闲持久连接，就可以避开缓慢的连接建立阶段。而且，已经打开的连接还可以避免慢启动的拥塞适应阶段，以便更快速地进行数据的传输。

所以，在访问效率高，请求很快被处理完毕的情况下，Waiting数比较多是正常的。

如果reading + writing数较多，则说明并发访问量非常大，正在处理过程中，尤其是writing比较大的时候，需要特别注意。
实际的连接上，现在都是并行连接+持久连接。并行的数量一般是个位数，比如4-6之间，是一个经验值。

waiting数量太高也不大好，毕竟是实在的开销。

管理持久连接时要特别小心，不然就会累积出大量的空闲连接，耗费本地以及远程客户端和服务器上的资源。

网络上有人分享的：

如果reading或writing的值很高,说明正在处理的数据量很大,可能后端的程序处理慢(MYSQL等后端处理),另一个原因很可能就是IO慢,或者客户端的网络慢。

困惑：writing是nginx已经从后端拿到全部响应数据了再返回给客户端，还是后端数据还没全部返回给nginx？这个应该是分段发送的，tcp流。
