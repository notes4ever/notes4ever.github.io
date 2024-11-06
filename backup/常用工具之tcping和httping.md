> **类比ping之基于icmp，用于探测网络层的延迟；tcping基于tcp，用于探测tcp三次握手；httping基于http/https用于测试真实的http/https数据返回延迟。**

## tcping
使用的是开源的[tcping](https://github.com/pouriyajamshidi/tcping)，使用golang实现。

![image](https://github.com/user-attachments/assets/e011966b-e729-425e-b3e4-e0e7359dcb80)

linuxmint安装：

```
wget https://github.com/pouriyajamshidi/tcping/releases/latest/download/tcping_amd64.deb -O /tmp/tcping.deb
sudo apt install -y /tmp/tcping.deb
```

或者下载后直接双击打开安装。

## httping
使用的是[httping](https://www.vanheusden.com/httping/)，官网的说明：

![image](https://github.com/user-attachments/assets/81c36f91-eda6-44e3-9c35-b9f7c98fc4e9)

读取header返回，计算真实的http延迟，包括服务器处理和网络传输。

![image](https://github.com/user-attachments/assets/382ab2ee-f2d5-4b70-9960-793312826ff7)


