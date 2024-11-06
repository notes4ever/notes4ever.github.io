
![image](https://github.com/user-attachments/assets/f820c107-3c66-482b-8e72-0261dddf1bf7)

> **upgrade的时候禁止升级内核版本，以linuxmint为示例。**

## apt-mark hold锁定内核版本
```
sudo apt-mark hold linux-image-generic linux-headers-generic linux-firmware linux-generic
sudo apt update
sudo apt upgrade
```

## apt-mark unhold解除锁定
```
sudo apt-mark unhold linux-image-generic linux-headers-generic linux-firmware linux-generic
sudo apt update
```

## 总结
在linuxmint下，总共需要禁用四项，这样upgrade的时候就不会升级系统内核。