> **关于centos epel** 


```
sudo yum -y install epel-release
sudo yum clean all #清除本地缓存
sudo yum makecache #把源缓存到本地
```

EPEL 代表 Extra Packages for Enterprise Linux，由 Fedora Special Interest Group 创建和维护。该存储库因为 Enterprise Linux 提供一组高质量的附加软件包而闻名。EPEL 的特点是它基于 Fedora 对应版本，因此它永远不会与企业 Linux 发行版中的任何软件包发生冲突或替换。它如此受欢迎的主要原因是它捆绑的一系列功能。

- EPEL 使您能够免费获得大量的包裹。
- 它由 Fedora 小组管理，100% 开源且安全。
- 永远不会冲突或替换现有的软件包，因为它的核心使用 Fedora 对应的软件包。
- 您可以放心只获得高质量的企业级软件包。
