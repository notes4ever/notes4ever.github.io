> **记录wireshark常用命令**

## wireshark配置用户组
tcpdump需要使用root，tshark可也需要root，普通用户会报错。

可以使用root将普通用户加入wireguard用户组，或者使用chmod +s 命令。

使用root运行：
```
usermod -a -G wireshark user
```

把普通用户添加进wireshar用户组，就能实现抓包（不用root）；可能需要重启才能生效，否则还是提示权限不够。如果不能正常获取权限，使用`tshark -D `不能获得网络接口。

tcpdump不能使用这种方式。可以用chmod  u+s 来临时解决。
```
which tcpdump
sudo chmod u+s /usr/bin/tcpdump
```

使用u+s类似的还有journalct 把root权限赋给普通用户。

参考：

- https://blog.csdn.net/qimengxingyuan/article/details/80872315
- https://www.cnblogs.com/lvdongjie/p/4076056.html

## 捕获过滤
捕获过滤器应用于libpcap/WinPcap，并使用Berkeley Packet Filter(BPF)语法。这个语法被广泛用于多种数据包嗅探软件，主要因为大部分数据包嗅探软件都依赖于使用BPF的libpcap/WinPcap库。


限定词 | 说明 | 例子
-- | -- | --
Type | 指出名字或数字所代表的意义 | host、net、port
Dir | 指明传输方向是前往还是来自名字或数字 | src、dst
Proto | 限定所要匹配的协议 | Ether、ip、tcp、udp、http、ftp

示例：
```
src host 192.168.0.10 && port 80

host 2001:db8:85a3:8a2e:370:7334

!ip6
```

