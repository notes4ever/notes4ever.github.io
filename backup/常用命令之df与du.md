> **人们经常会把df和du命令混淆，df是用于查看磁盘或分区使用情况的命令，而du命令则是用于按照指定容量单位来查看文件或目录在磁盘中的占用情况。**

## df命令
![image](https://github.com/user-attachments/assets/2cdd3204-3252-423a-bf87-92483f5632d4)

**df 的结果可能比du出来的结果更大**，就是因为删除后，还没释放空间。

## du命令
du命令来自英文词组disk usage的缩写，其功能是查看文件或目录的大小。

![image](https://github.com/user-attachments/assets/96ad1a2c-1f03-49ce-915a-00538a7ec45f)

![image](https://github.com/user-attachments/assets/f1052ed9-45b1-4e01-9ff9-1faf470be26f)

du命令，主要就是-h和-s参数的使用，还可以做一些简单的统计。

当前目录下的**文件和目录**，按照大小排序：

`du -sh * | sort -rh`

![image](https://github.com/user-attachments/assets/cf9be8d3-e820-44a3-baff-16114c1cc6fb)

查看当前目录下的所有**目录**大小：

`du -h --max-depth=1`

当前目录下，找出大小最大的20个**目录**：

```
du | \
sort -r -n | \
awk '{split("K M G",v); s=1; while($1>1024){$1/=1024; s++} print int($1)" "v[s]"\t"$2}' | \
head -n 20
```

