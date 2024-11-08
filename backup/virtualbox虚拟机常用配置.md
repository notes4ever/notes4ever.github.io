> **virtualbox重点配置记录**

## virtualbox修改vdi的uuid
复制虚拟机的时候，一般是复制vdi，然后修改vdi，才能启动为新的虚拟机。

C:\Program Files\Oracle\VirtualBox\VBoxManage internalcommands sethduuid xx.vdi

C:\Program Files\Oracle\VirtualBox 加入环境变量后直接使用命令：

VBoxManage internalcommands sethduuid xx.vdi

修改了uuid才能操作xx.vdi（如果xx.vdi是复制的话）

## 动态磁盘vdi文件压缩瘦身

vdi文件增长会比实际的系统大小更大，virtualbox自带提供了瘦身的工具。

VBoxManage命令，默认位于：C:\Program Files\Oracle\VirtualBox 可以加入环境变量。

cmd进入vdi所在目录，对vdi文件进行以下操作：

VBoxManage modifyhd xxx.vdi --compact

![image](https://github.com/user-attachments/assets/ce52591c-0a66-4199-9181-1add3d1bf38c)

还可以考虑以下方式，一般上面的方式已经比较有效了。

理论上更好的方式是，执行以上命令之前，先进入虚拟机清理碎片。

```
cd /
sudo dd if=/dev/zero of=/free
sudo rm -rf /free
```

如果你不止一个root目录，还有home等，可以选一个剩余空间最大的盘，将 /free 换成对应的，如/home/free。

注意：这个命令可能执行很久，和磁盘大小与类型有关

## virtualbox宿主机虚拟机互通端口
都是网络转换nat的网络，虚拟机获取到的ip是一样的；虚拟机之间不能通信；虚拟机可以上网；虚拟机可以ping同宿主机；宿主机无法ping通虚拟机；但virtualbox提供了宿主机与虚拟机端口的打通。

![image](https://github.com/user-attachments/assets/362ad430-031b-4c2d-98a9-20137ec4312e)

然后宿主机利用xshell等工具就可以远程链接虚拟机了。

![image](https://github.com/user-attachments/assets/34575c10-571f-4e76-9b78-1451b43c8066)

前提是虚拟机安装打开了openssh-server。

```
sudo apt install openssh-server
sudo systemctl enable --now ssh
sudo apt install openssh-client
```

同理，可以利用这种方式把虚拟机的端口暴露给宿主机，比如web服务等。

## 参考资料

- https://blog.csdn.net/u010882234/article/details/136175399
- https://www.51cto.com/article/599836.html