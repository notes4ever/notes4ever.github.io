![image](https://github.com/user-attachments/assets/e3ea6ef6-3bc8-4877-8e0a-704e7a87104e)

> **记录tshark常用命令**

## 查看tshark版本
tshark -v

## 查看网络接口
tshark -D

实际使用，只需要网络接口对应的序号即可。

## 不用root和sudo
sudo dpkg-reconfigure wireshark-common

选「是」，然后执行
sudo chmod +x /usr/bin/dumpcap

就可以直接以普通用户执行tshark命令了

## tshark常用命令
### 捕获过滤器，存入文件
tshark -nni 1 -f "icmp or port 443" -w packets.pcap

只能手动停止。

![image](https://github.com/user-attachments/assets/a206c4fb-6e09-4ebe-b689-9e398bc67e0c)

注意截图左下角，有显示数据包的数量。

参数解释：

- -nn 不解析ip和端口，显示原始ip和端口
- -i 序号为1的网络接口
- -f 捕获过滤器，在双引号内请遵从BPF的语法。
- -w 抓包结果存储到文件

### 指定数据包数量的抓包
tshark -nni 1 -f "icmp or port 443" -w packets.pcap -c 100

抓够100个数据包会自动停止。

### 指定文件大小的抓包
tshark -nni 1 -f "icmp or port 443" -w packets.pcap -a filesize:512

抓够512字节后会自动停止。

### 指定抓包时间
tshark -nni 1 -f "icmp or port 443" -w packets.pcap -a duration:10

抓包10秒就自动停止。

### 保存为多个大小相同文件的抓包
tshark -nni 1 -f "icmp or port 443" -w packets.pcap -b filesize:1024

单位：KB
保存的文件命名为： 
packets_00001_20240616230331.pcap 
packets_00002_20240616230353.pcap
......
只能手动停止。

### 指定抓包时长，保存成若干相等大小文件
tshark -nni 1 -f "icmp or port 443" -w packets.pcap -b filesize:1024 -a duration:600

抓包10分钟，每个文件1MB

### 读取数据包
tshark -r packets.pcap -c 50

-c 指定读取数据包的数量，不加即读取全部。

### 显示过滤，输出特定字段与分隔符
tshark -r packets.pcap -Y dns -T fields -e dns.qry.name -E separator="|"

每个字段前面都用-e参数

## tshark命令实现特定场景需求

### 找出数据包中所有dns请求域名

排序并去重示例：
tshark -r 10271418-no-doh-no-proxy.pcapng -Y dns -T fields -e dns.qry.name | sort | uniq

-Y 显示过滤 参数值可以用双引号扩起来
T fields -e 需要显示的字段 参数值可以用双引号扩起来

增加去查询的dns对应的IP：
tshark -r 10271418-no-doh-no-proxy.pcapng -Y dns -T fields -e ip.dst -e dns.qry.name | sort | uniq

这种方式无法区分具体的方向，也就无法去重。

统计总共有多少dns域名请求：
tshark -r 10271418-no-doh-no-proxy.pcapng -Y "dns" -T fields -e dns.qry.name | sort | uniq | wc -l

更多带dns查询的命令：
tshark -r test.cap -T fields -e frame.time -e ip.src -e ip.dst -e dns.qry.name -R 'udp.dstport==53 || dns'

### tshark命令找出所有tls的SNI域名

排序并去重示例：
tshark -r 10271418-no-doh-no-proxy.pcapng -Y tls.handshake.type==1 -T fields  -e tls.handshake.extensions_server_name | sort | uniq

统计tls总共有多少dns域名请求(client hello)：
tshark -r 10271418-no-doh-no-proxy.pcapng -Y "tls.handshake.type==1" -T fields  -e "tls.handshake.extensions_server_name" | sort | uniq | wc -l

追加提取目标的ip和端口：
tshark -r 10271418-no-doh-no-proxy.pcapng -Y tls.handshake.type==1 -T fields  -e ipv6.dst -e tcp.dstport -e tls.handshake.extensions_server_name -E separator="|"  | sort | uniq

以上这种写法如果同时有ipv4和ipv6就不适用，都会缺失一部分。

## 参考资料
- [tshark官方文档](https://www.wireshark.org/docs/man-pages/tshark.html)
- [tshark 大杂烩 合集](https://const.net.cn/668.html)
- [一文读懂网络报文分析神器Tshark](https://cloud.tencent.com/developer/article/2312883)
- [tshark在流量分析中的绝佳应用](https://blog.csdn.net/weixin_44288604/article/details/123807294)