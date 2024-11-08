步骤：

1、添加docker源，linuxmint可以参考：

https://www.zhihu.com/question/630722571

2、安装

sudo apt-get install docker-ce docker-ce-cli containerd.io

3、配置普通用户运维权限

```
$sudo usermod -aG docker $USER
$newgrp docker
```

其它参考，可选：

- https://zhuanlan.zhihu.com/p/651148141
- https://docs.docker.com/engine/security/rootless/ 
- https://docs.docker.com/engine/install/linux-postinstall/