![image](https://github.com/user-attachments/assets/b7792b03-c8a2-4757-a877-1aeaad551d87)

> **值得向所有人推荐的浏览器**

## Firefox家族浏览器
基于Mozilla开源内核的浏览器很多，其中有不少优秀的代表，隐私与安全方面有Tor和Mullvad，性能与交互都很优秀的Zen，如果注重隐私与安全，又不牺牲太多便捷性，同时又简洁可靠不花哨，那[lirewolf](https://librewolf.net/)应该是不二之选。

lirewolf的特点列举：

- 界面简洁，稳定
- 注重隐私安全，开箱即用
- 配置简单，与原生firefox解耦做得很好，可热拔插

## librewolf配置

总体的配置思路与流程：

1. 开启firefox同步，登录firefox同步，这一步很大简化了初步的配置；同步完成关闭firefox同步功能。
2. 配置书签同步插件，与其他设备与浏览器保持书签同步。这一步至关重要，单独列出。
3. 配置其他插件与脚本。
4. 配置浏览器设置。

以下列举部分配置内容。

## about:config 配置
network.http.http3.enable          设置false，关闭quic。为什么关闭，有单独的笔记记录。
media.peerconnection.enabled          设置false，关闭WebRTC。
geo.enabled      设置false，关闭 Geolocation 地理信息。
dom.enable_resource_timing       设置false
dom.enable_user_timing              设置false
dom.performance.enable_user_timing_logging     设置false

## firefox账号同步
![image](https://github.com/user-attachments/assets/a3a68a21-af85-4ff9-8d4d-f30038d5f709)

同步之后，选择性禁用、删除一些插件，这一步的前提是firefox插件尽量保留比较全的常用插件。
目前librewolf启用的插件有十几款：
![image](https://github.com/user-attachments/assets/e1f41fb9-7b2d-4b3d-8be1-ced5090139be)

## 书签同步插件floccus
![image](https://github.com/user-attachments/assets/08b50fcf-74f6-4759-89e4-da97e45d2862)

非常好用的跨浏览器书签同步插件floccus，开源免费，更多信息请访问[github主页](https://github.com/floccusaddon/floccus)。这款插件肯定排在我必装浏览器插件的前三甲。

## 隐私安全配置
![image](https://github.com/user-attachments/assets/ca955ba9-4b88-4d3d-bea0-583c1724fd91)

这里开启会连接google的服务器，进行网址和文件识别， 在 LibreWolf 中，“谷歌安全浏览”服务默认是禁用的，以避免连接到谷歌服务。原生的firefox中，此处翻译成，“钓鱼保护”，默认是开启的。

## 历史与密码设置
关于cookie、历史记录、记住密码的设置，暂时配置如下：
![image](https://github.com/user-attachments/assets/faa78a76-780b-41b6-a5bd-d937479ed8b9)

![image](https://github.com/user-attachments/assets/652b0774-73bd-456f-99b9-e337a330a01e)

安全和隐私，和便利性之间要有取舍。

## 主页与搜索设置
startpage，搜索结果接近直接使用谷歌，可以理解为通过代理去访问谷歌，准确来说是反向代理。
![image](https://github.com/user-attachments/assets/ebae1de2-c0a0-4540-b7d5-92a4efb1901f)

startpage贴心地支持了自定义配置但免登录的url，可以自由地配置自己的startpage，这是我自己自定义的[URL](https://www.startpage.com/do/mypage.pl?prfe=5d00644d4131f7bd709fbdfa6f350c6c3d7cc9d76797ea664f08e48998435769fa5b29b30041d21703458d3831d573374b989bc3dd6c6c3619bd7c17b8899f21bba5b4d74396d27dc88640)。

搜索框的搜索也设置成startpage：
![image](https://github.com/user-attachments/assets/13611707-c374-4862-9e2c-4f7526c84ce9)

## Doh 设置
大陆的网络，不使用代理的话，可以使用**阿里的doh**: `https://223.5.5.5/dns-query`。
![image](https://github.com/user-attachments/assets/9d692249-d592-4c88-8f23-4bd295beb10a)

![image](https://github.com/user-attachments/assets/a83b0ae4-c800-4a79-bda7-b026b34d5f03)

如果使用代理，可以不设置浏览器的doh，但代理的DNS建议都要设置成doh，这个非常重要。
![image](https://github.com/user-attachments/assets/4923cf07-00ad-4055-8f36-79fa8cb92788)

如果想了解doh为什么那么重要，需要去google关于dns和https的相关知识，总结就是：doh可以保障一部分的隐私安全。

## 其他设置
![image](https://github.com/user-attachments/assets/17ee9416-ff1b-42fb-a9fc-bae57498c2f4)

![image](https://github.com/user-attachments/assets/1e8b0593-c550-411a-a3cc-fd2c66a87a1c)

![image](https://github.com/user-attachments/assets/3b93c856-e95c-49b1-bd52-d2b5553aeb3e)




