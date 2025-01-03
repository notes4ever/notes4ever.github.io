> **读书笔记**

## 第7章 理解Linux文件权限
对很多 Linux 发行版而言，useradd 命令默认并不创建$HOME 目录，但是-m 命令行选项会使其创建$HOME 目录。
```
useradd -m test 
```

![image](https://github.com/user-attachments/assets/445ecd14-42ef-4efb-a838-919705bdca8d)

![image](https://github.com/user-attachments/assets/a846b794-c463-40dd-9c99-8aadbb1c9b9c)

![image](https://github.com/user-attachments/assets/d8dcbde0-dcfb-4513-b91b-1d92c78be948)

![image](https://github.com/user-attachments/assets/daeae947-9c24-48b1-aa7a-99dff32dc385)

## 第8章 管理文件系统
你只能在同一个物理硬盘的可用空间范围内调整 分区大小。如果硬盘上没有地方了，就得找一个更大的硬盘，手动将已有的文件系统转移到新硬盘。 

这时候可以通过将另一块硬盘上的分区加入已有的文件系统来动态地添加存储空间。Linux逻辑卷管理器（logical volume manager，LVM）正是用来做这个的。它可以让你在无须重建整个 文件系统的情况下，轻松地管理磁盘空间。
```
sudo mkfs.ext4 /dev/vg00/lvol0 
mkdir my_LV 
sudo mount -t ext4 /dev/vg00/lvol0 my_LV
```

## 第9章 安装软件
更新：
```
apt update
apt upgrade
apt full-upgrade
```

upgrade 命令在升级过程中不会删除任何软件包。如果必须删除某个软件包才能完成升级， 可以使用full-upgrade。

卸载：
```
apt remove zsh
apt purge zsh
apt autoremove 
```

apt 的 remove 命令可以删除软件包，同时保留数据和配置文件。如果要将软件包以及相关 的数据和配置文件全部删除，那么需要使用 purge 选项。

autoremove 命令会检查所有被标记为存在依赖关系且不再被需要的软件包。