> **linuxmint下一种中英文目录名切换的方式**

打开终端，在终端下输入命令：
```
sudo apt-get install xdg-user-dirs-gtk 
export LANG=en_US 
xdg-user-dirs-gtk-update
export LANG=zh_CN.UTF-8
```

更改export LANG实现切换，可能需要注销重启生效。
