>**DoH和ECH尽量开启，可以保障一些隐私，尤其是请求的服务端支持ECH的时候。**

## chrome开启DoH和ECH
设置里面搜索dns，填入阿里的公共dns。

`https://223.5.5.5/dns-query` 或者 `https://dns.alidns.com/dns-query` 

![图片](https://github.com/user-attachments/assets/56251b0d-ab81-4797-8c1f-994fbb8623e7)

开启ECH：
导航到：`chrome://flags/#encrypted-client-hello`
启用ECH，设置为enable。
新版本（130版本以上）已经内置并且无法搜索和设置了。

## firefox开启DoH和ECH
设置里面进入隐私与安全，找到基于https的dns，选择或自定义修改。

![图片](https://github.com/user-attachments/assets/5a23ca33-c8e5-43d4-8614-cbb5a53cc3e8)

启用ECH：
在about:config页面中搜索echconfig并启用。

## Doh与ECH注意事项

- 选用合适的DoH，最好是大牌的值得信任的。
- 启用DoH前，请确保当前网络与设置域名是否能够正常的https交互；国外的很多已经被GFW封禁。
- 此外，如果启用了系统代理或者软件代理，请注意DNS的优先级，设置了DoH并不一定会真正使用到。
- 如果浏览器的DoH被代理绕过，请把代理也设置DoH（大陆DNS和远程DNS都设置成DoH，如果支持的话）。

## 在线验证已经开启ECH
有很多在线验证的方式，列举一些：

- https://v2ex.com/cdn-cgi/trace
- https://crypto.cloudflare.com/cdn-cgi/trace/
- https://defo.ie/ech-check.php
- https://tls-ech.dev/
- https://www.cloudflare.com/zh-cn/ssl/encrypted-sni/#results

## 验证结果解释
以CF提供的cdn trace工具为例。

![图片](https://github.com/user-attachments/assets/9cebd1e2-c116-48ad-ba96-4a779218d047)

- fl：Cloudflare 服务器实例
- h：网站域名
- ip：当前访问者的IP地址
- ts：时间戳，格式为“秒.毫秒”（bash中生成同款时间戳的命令为date +%s.%3N）
- visit_scheme：访问者使用的协议
- usg：访问者使用的UserAgent信息
- colo：被访问的Cloudflare数据中心的所在位置，此处是由[IANA定义的机场代码](https://en.wikipedia.org/wiki/IATA_airport_code)
- sliver：请求是否被拆分成多个部分进行处理或传输
- http：访问者使用的HTTP协议版本
- loc：访问者的所在地（国家）
- tls：访问者与服务器建立连接使用的TLS版本
- sni：SNI加密或明文传输
- warp：访问者是否使用了[Warp](https://cloudflarewarp.com/)服务
- gateway：访问者是否使用了[Cloudflare Gateway](https://www.cloudflare.com/teams/gateway/)服务
- rbi：访问者是否使用了[Cloudflares Remote Browser Isolation(RBI)](https://www.cloudflare.com/learning/access-management/what-is-browser-isolation/)服务
- kex：TLS密钥交换过程中使用的交换方式
