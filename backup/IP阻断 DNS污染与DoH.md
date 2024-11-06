> **GFW工作的时候，除了SNI阻断之外，常用基本手段还有IP阻断和DNS污染。**

## IP阻断
IP阻断在GFW中扮演着重要的角色，主要用于控制和限制用户访问特定网站和服务。IP阻断是GFW实施网络审查的重要手段之一，通过对特定IP地址和端口的控制，限制用户访问不符合政府规定的网站和服务。这一机制不仅影响了直接目标，还可能波及使用相同服务器资源的其他合法网站，造成更广泛的信息封锁。

IP阻断技术的应用场景有：

- GFW通过分光技术在出口路由器旁边捕获流量，并分析数据包内容。一旦检测到与黑名单中的IP地址匹配的数据包，便会采取相应措施，如发送TCP RST包以中断连接。
- 除了RST之外，还会选择性丢包。在检测到某些特定行为或内容时，GFW可以选择性地丢弃与特定IP地址相关的数据包。这种方法可以用于动态调整封锁策略，以应对不断变化的网络环境和用户行为。
- 端口封锁。GFW还会针对某些特定端口（如SSH的22端口、VPN的1723端口和SSL的443端口）进行封锁。这种措施会导致与这些端口相关的服务无法正常使用，从而影响用户访问被封锁网站的能力

## DNS污染
DNS cache pollution，DNS污染或DNS劫持，是一种破坏域名系统查询解析的行为。（污染一词可能取自域名系统域名解析之特性，若递归DNS解析器查询上游时收到错误回复，所有下游也会受影响。）

DNS劫持是一类旁观者攻击，攻击者借由其在网络拓扑中的特殊位置，发送比真实的DNS回应更早到达攻击目标的伪造DNS回应。 一部连上了互联网的电脑一般都会使用互联网服务提供商提供的递归DNS服务器，这个服务器通常都会将部分客户曾经请求过的域名暂存起来。缓存污染攻击就是针对这一特性，影响服务器的用户或下游服务。

所有经过GFW的在UDP的53端口上的域名查询，GFW都会进行IDS入侵检测，一经发现与黑名单关键词相匹配的域名查询请求，会马上伪装成目标解析服务器注入伪造的查询结果。攻击仅出现在DNS查询之路由经过防火长城时（中国大陆用户查询当地的DNS服务器收到错误结果是缓存所致，其并没有直接受到防火长城的DNS劫持攻击。）。伪造的查询结果中的IP地址不是一成不变的，在一段时间后会更新。

## 应对DNS污染
DNSSEC技术为DNS解析服务提供了解析数据验证机制，理论上可以有效抵御劫持。此外，DNSCrypt、DoT、DoH等方法通过将DNS请求封装于安全连接内，以保护DNS请求中的数据不被中间传输设备篡改。

以上各种应对的技术，DoH被认为是比较靠谱的技术，依赖的是https的加密与防篡改。

## DoT与DoH技术区别
![image](https://github.com/user-attachments/assets/daf98096-8849-4134-bd37-3e1e40e2fd05)

![image](https://github.com/user-attachments/assets/87e42923-c4c4-4c58-a5e6-560e1363c252)

DNS查询是明文的，因此DNS服务器可以轻易知道用户访问了什么网站（一些公司/集团的网关能知道员工访问的网站也就不足为奇了），这就带来了隐私泄漏。为了解决这个问题，DoT（DNS over TLS）和DoH（DNS over HTTPS）先后被提出来，目前看来是DoH胜出了，主流浏览器都支持DoH了。

## 总结
开启doh的作用：

- 安全，doh就是防止dns泄露和dns劫持的。
- 运营商和GFW可以通过墙ip,墙域名的方式,让你用不了某些服务商的doh。比如google的doh被墙，有的地方cf的doh不稳定。
- 可以通过自建doh的方式绕过墙。比如在hk/tw/jp区域自建doh服务。这种请求走普通https请求，墙不知道你在进行dns请求。

## 一些知名的DoH汇总
### 国内大厂
阿里: `https://dns.alidns.com/dns-query`  或者不使用域名直接用ip： `https://223.5.5.5/dns-query`
腾讯: `https://doh.pub/dns-query`
360: `https://doh.360.cn/dns-query`

主要用阿里就行了，这是阿里DoH的[文档](https://alidns.com/articles/6018321800a44d0e45e90d71)。

### 国外大厂
Cloudflare: `https://1.1.1.1/dns-query`
Google Public DNS: `https://dns.google/dns-query`   `https://8.8.8.8/dns-query`
Open DNS: `https://doh.opendns.com`
QUAD9 DNS: `https://dns.quad9.net/dns-query`

### 其他DoH参考

- https://github.com/zoonderkins/blahdns
- https://www.podipod.com/3868.html
