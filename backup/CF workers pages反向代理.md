> **cloudflare提供了workers和pages，可以用来做反向代理，而且提供了个人够用的免费套餐。**

## CF workers pages免费计划
![image](https://github.com/user-attachments/assets/ab5f40ec-aefb-42b5-86c2-719976a66d49)

## CF workers pages付费计划
![image](https://github.com/user-attachments/assets/98a3f355-0a31-4af3-85b5-845081696610)

总次数增加，并发数没有公开，但远远比免费的高。一般个人研究使用的话，免费计划是够用的了。

## 一些能科学上网的反代方案
### Cloudflare_vless_trojan
项目主页：
https://github.com/yonggekkk/Cloudflare_vless_trojan

个人使用最多的科学上网方式，很好用。

### BPB-Worker-Panel
项目主页：
https://github.com/bia-pain-bache/BPB-Worker-Panel

实测也能使用，但可用率没有上面那种高，可以作为备用方案。

### 反代科学上网之ProxyIP
用来访问使用了cf服务的网站服务。cf自己故意如此设计，只能用proxyip绕过。（ 由于cf bug，现在cf worker 不能直接访问cf 托管的网站。。所以需要配置一个中转ip。而有一些神奇的 ip，可以无条件转发所有 cf 的流量。proxyip就是指这些神奇的ip）
ProxyIP目前可用的有以下等：

```
cdn-all.xn–b6gac.eu.org
cdn.xn–b6gac.eu.org
cdn-b100.xn–b6gac.eu.org  
```

ProxyIP的隐患列举：

- 随时不能用的风险
- 没有加密，可靠性存疑
- 有一些ProxyIp会劫持网页跳广告  

ProxyIP与CF反代IP的区别：

也是一种反代， ProxyIP是承接从CF转发来的流量，CF反代是把接收到的流量转发给CF，两者方向是相反的，当然有些服务器双向都支持。也就是承接的流量类型不同，方向也不同。

### CF 反代IP
实际上cf反代ip大都可以用作ProxyIP。

ProxyIP需要请求的目的ip是CF。来源既可以是不限的，也可以仅限CF。

CF反代ip不能指定请求的目的ip，它们固定与一个CF的ip通信。请求来源则是不限的，除非故意屏蔽来自CF的请求。但基本上很少有这种限制。

可以发现，CF反代ip大多都符合做proxyip的条件。

### 优选IP
worker的运行区域发（执行区域），跟你连接的数据中心有关，而数据中心又跟你访问CF的IP有关。
所以可以通过优选IP的方式解决。

### 查看连接的CF数据中心
查看你连接的CF数据中心的办法有很多，之前在ech的笔记中有涉及。比如：https://freeyx.cloudflare88.eu.org/cdn-cgi/trace
colo= 后面的那三个字母就是数据中心，不知道啥意思可以搜机场三字码

扩展学习：[指定区域执行](https://blog.lyc8503.net/post/cloudflare-worker-region/)。

## 参考资料

- https://developers.cloudflare.com/pages/functions/get-started/
- https://xiaowangye.org/posts/using-cloudflare-worker-proxy-google/
- https://jiyiren.github.io/2024/06/16/cloudflare_vless/
