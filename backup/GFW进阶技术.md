> **除了基本的IP阻断、DNS污染和SNI阻断手段，GFW还采用了更为高阶的技术手段，并且会持续进化。**

## GFW架构
![image](https://github.com/user-attachments/assets/ea263040-4b8f-4e1c-a529-372f7cb7e7ef)

GFW工作在旁路（on-path），而非网关（in-path）。

GFW主要部署于北京、上海和广州的三个大型互联网交换中心，而非在国际出口，在省级部署的设备较少。

根据“GFW技术评论”网站2010年的估计，其硬件配置如下：

- 防火长城（北京）使用曙光4000L机群，Red Hat操作系统。
- 防火长城实验室（哈工大）使用曙光服务器，Red Hat操作系统。
- 防火长城（上海）使用Beowulf集群。
- 另外，防火长城使用的处理器指令集架构是x86-64。

## GFW建设参与者
![image](https://github.com/user-attachments/assets/034c7ed1-f145-4b17-a994-1b4ff6b3c669)

方滨兴、哈工大、360、启明星辰等等。

名人堂事迹：

![image](https://github.com/user-attachments/assets/af0b67ba-aa07-4c35-80e9-75b4dfa869ef)

## 进阶技术之被动探测
GFW通过被动流量检测技术，实时监控网络流量，而不需要主动发送探测包。这种方法使得GFW能够在不干扰用户的情况下，分析经过的加密流量。通过对数据包的特征（如长度、频率、熵等）进行统计，GFW能够识别出可能的翻墙协议，如Shadowsocks、VMess等。

### 启发规则
GFW并不直接定义什么是“完全加密流量”，而是应用了一系列启发式规则来判断流量的性质。这些规则基于多个因素，包括：

- 协议指纹：识别常见翻墙工具的特征。
- 数据包的熵：通过计算数据包中可打印ASCII字符的比例和位置，判断其随机性。
- 数据包长度分布：分析数据包长度的变化，以识别特定协议的流量模式。

这些启发式规则使得GFW能够在不解密流量内容的情况下，判断其是否为翻墙流量，并采取相应措施。

### TLS指纹
TLS指纹是一种通过分析TLS握手过程中各个参数（如支持的加密算法、扩展等）来识别特定客户端或服务器的方法。在某种程度上，GFW也利用类似于TLS指纹的技术来识别加密流量。例如，通过分析TLS握手中的特征，GFW可以判断某些连接是否使用了已知的翻墙工具。

### 重建字节流
GFW首先会对经过的IP包进行重建，将TCP/IP层的数据组合成完整的字节流，然后进行分析。这一过程包括解析应用层协议（如HTTP、HTTPS），并在此基础上进行关键字匹配和内容检查。对于加密流量，虽然内容无法直接读取，但通过对流量模式的分析，GFW可以推测出其性质。

## 进阶技术之主动探测
GFW已经启用主动探测的手段来识别Shadowsocks服务器。GFW采用被动监测与主动探测相结合的方式：其首先监测网络连接找出疑似Shadowsocks的连接，然后再把自己伪装成一个客户端，尝试对疑似Shadowsocks的服务器进行连接，从而验证自己的猜测。

GFW针对各种科学上网客户端进行主动探测已经很久了，大概从2011年开始，详情请参考[这篇文章](https://ensa.fi/active-probing/)。

Shadowsocks也成了其中一员，被精准识别，然后2019年开始shadowsocks已经基本不能使用。

![image](https://github.com/user-attachments/assets/46a4c2bd-288e-4180-b2b8-cce815fe80ad)

更多详细说明请参考[这篇文章](https://gfw.report/blog/gfw_shadowsocks/zh.html)。

## 被动流量分析与主动探测结合
关于具体是如何使用被动与主动探测技术来识别完全加密流量的，请参考这篇文章：[中国的防火长城是如何检测和封锁完全加密流量的](https://gfw.report/publications/usenixsecurity23/zh/)。

## 参考资料

- [防火长城GFW](https://zh.wikipedia.org/wiki/%E9%98%B2%E7%81%AB%E9%95%BF%E5%9F%8E)
- https://gfw.report/