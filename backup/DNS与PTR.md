## 关于DNS与PTR基本说明
当你发现针对相同的网络服务，使用 IP 地址快而换成域名却慢很多时，就要想到，有可能是 DNS 在捣鬼。DNS 的解析，不仅包括从域名解析出 IP 地址的 A 记录请求，还包括性能工具帮你，“聪明”地从 IP 地址反查域名的 PTR 请求。

事实上，并非所有 IP 地址都会定义 PTR 记录，所以 PTR 查询很可能会失败。

在你使用 ping 时，如果发现结果中的延迟并不大，而 ping 命令本身却很慢，不要慌，有可能是背后的 PTR 在搞鬼。如何验证？可以使用tcpdump和wireshark抓包分析，这里省略。

知道问题后，解决起来就比较简单了，只要禁止 PTR 就可以。还是老路子，执行 man ping 命令，查询使用手册，就可以找出相应的方法，即加上 -n 选项禁止名称解析。

实际上，根据 IP 地址反查域名、根据端口号反查协议名称，是很多网络工具默认的行为， 而这往往会导致性能工具的工作缓慢。

所以，通常，网络性能工具都会提供一个选项（比如 -n 或者 -nn），来禁止名称解析。

比如ping、tcpdump等等：

```
ping -n -c3 geektime.org
tcpdump -nn udp port 53 or host 35.190.27.188
```

解释一下这条tcpdump命令。

- -nn ，表示不解析抓包中的域名（即不反向解析）、协议以及端口号。
- udp port 53 ，表示只显示 UDP 协议的端口号（包括源端口和目的端口）为 53 的包。
- host 35.190.27.188 ，表示只显示 IP 地址（包括源地址和目的地址）为 35.190.27.188 的包。
- 这两个过滤条件中间的“ or ”，表示或的关系，也就是说，只要满足上面两个条件中的任一个，就可以展示出来。

既然域名以分层的结构进行管理，相对应的，域名解析其实也是用递归的方式（从顶级开始，以此类推），发送给每个层级的域名服务器，直到得到解析结果。

不过不要担心，递归查询的过程并不需要你亲自操作，DNS 服务器会替你完成，你要做的，只是预先配置一个可用的 DNS 服务器就可以了。

当然，我们知道，通常来说，每级 DNS 服务器，都会有最近解析记录的缓存。当缓存命中时，直接用缓存中的记录应答就可以了。如果缓存过期或者不存在，才需要用刚刚提到的递归方式查询。

所以，系统管理员在配置 Linux 系统的网络时，除了需要配置 IP 地址，还需要给它配置 DNS 服务器，这样它才可以通过域名来访问外部服务。

cat /etc/resolv.conf （虚拟机示例）

![image](https://github.com/user-attachments/assets/a8f34656-a5a0-4cd3-beab-eb37ce468397)

DNS 服务通过资源记录的方式，来管理所有数据，它支持 A、CNAME、MX、NS、PTR 等多种类型的记录。比如：

- A 记录，用来把域名转换成 IP 地址；
- CNAME 记录，用来创建别名；
- 而 NS 记录，则表示该域名对应的域名服务器地址。

简单来说，当我们访问某个网址时，就需要通过 DNS 的 A 记录，查询该域名对应的 IP 地址，然后再通过该 IP 来访问 Web 服务。

比如，还是以极客时间的网站 time.geekbang.org 为例，执行下面的 nslookup 命令，就可以查询到这个域名的 A 记录，可以看到，它的 IP 地址是 39.106.233.176。

![image](https://github.com/user-attachments/assets/02ad54d4-e124-4919-baca-22fa441f75f5)

## DNS UDP TCP切换
DNS 协议在 TCP/IP 栈中属于应用层，不过实际传输还是基于 UDP 或者 TCP 协议（UDP 居多） ，并且域名服务器一般监听在端口 53 上。

IPv4 规范建议，报文长度应该控制在相对小的范围内，这个范围是 576 字节，相应的 UDP 载荷在 512 字节以内；像 DNS 解析，如果数据量超过 512 字节，也是会自动切换为 TCP 模式的，根本原因也是这个很早以前的规定。

## 查询域名dns信息
### dig
dig www.baidu.com:

![image](https://github.com/user-attachments/assets/75f2e90b-27fc-4151-9ad8-5600f22da0d8)

### nslookup
nslookup www.baidu.com:

![image](https://github.com/user-attachments/assets/ec6b4bdd-f855-4e99-8151-de8dc3df601f)

## DNS缓存服务
我们使用的主流 Linux 发行版，除了最新版本的 Ubuntu （如 18.04 或者更新版本）外，其他版本并没有自动配置 DNS 缓存。
所以，想要为系统开启 DNS 缓存，就需要你做额外的配置。比如，最简单的方法，就是使用 dnsmasq。

dnsmasq 是最常用的 DNS 缓存服务之一，还经常作为 DHCP 服务来使用。它的安装和配置都比较简单，性能也可以满足绝大多数应用程序对 DNS 缓存的需求。

## 常见的 DNS 优化方法

- 对 DNS 解析的结果进行缓存。缓存是最有效的方法，但要注意，一旦缓存过期，还是要去 DNS 服务器重新获取新记录。不过，这对大部分应用程序来说都是可接受的。
- 对 DNS 解析的结果进行预取。这是浏览器等 Web 应用中最常用的方法，也就是说，不等用户点击页面上的超链接，浏览器就会在后台自动解析域名，并把结果缓存起来。
- 使用 HTTPDNS 取代常规的 DNS 解析。这是很多移动应用会选择的方法，特别是如今域名劫持普遍存在，使用 HTTP 协议绕过链路中的 DNS 服务器，就可以避免域名劫持的问题。
- 基于 DNS 的全局负载均衡（GSLB）。这不仅为服务提供了负载均衡和高可用的功能，还可以根据用户的位置，返回距离最近的 IP 地址。