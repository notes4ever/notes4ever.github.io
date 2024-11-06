![image](https://github.com/user-attachments/assets/1f8c7027-8536-4d59-8368-7b5e9cc70724)

> **关于linuxmint缓存的删除，包括apt缓存等**

## apt缓存路径
查看大小：

`sudo du -sh /var/cache/apt/archives`

删除：

`sudo apt clean`

重新查看就变小了。

## 删除不再依赖使用的内核和软件

`sudo apt  autoremove --purge
`
## 使用系统清理工具如bleachbit

`sudo apt install bleachbit
`

谨慎使用，这款软件包含有很多的缓存，如果不是特别熟悉，别乱使用。