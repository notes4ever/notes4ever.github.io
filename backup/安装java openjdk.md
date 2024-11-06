> **java环境 jdk的安装，以linuxmint和centos为示例。**

## linuxmint 
安装最新稳定的openjdk：

```
sudo apt update
sudo apt install default-jdk -y
```

验证：
`java --version`

或者指定openjdk版本：
```
sudo apt install openjdk-11-jdk
sudo apt install openjdk-17-jdk
```

## centos
指定版本安装java-11-openjdk：

`sudo yum install -y java-11-openjdk-devel `

## 参考

- https://blog.csdn.net/xiaochong0302/article/details/139593466
