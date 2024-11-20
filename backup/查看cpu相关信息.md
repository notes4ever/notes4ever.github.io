查看cpu物理数量：
```
grep 'physical id' /proc/cpuinfo|sort|uniq|wc -l
```

查看cpu总核数：
```
grep -c processor /proc/cpuinfo
```

即等于：
```
grep processor /proc/cpuinfo|wc -l
```

使用top命令也可以知道cpu的总核数：

通过执行top命令，然后按数字1，即可显示所有的CPU核数。

查看cpu架构信息：
```
lscpu
```