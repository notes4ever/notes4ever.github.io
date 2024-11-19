#使用cookie 缓解syn flood攻击 必须设置为1
net.ipv4.tcp_syncookies = 1

#半连接的队列长度 调大容纳更多客户端syn请求 无标准 可以设置大一点 
net.ipv4.tcp_max_syn_backlog = 262144

#客户端重试syn的次数 让客户端早点放弃重试 设置1-3 如果是内网设置1-2  如果是外网2-3
net.ipv4.tcp_syn_retries = 1

#收不到client回复 服务端重试次数 早点放弃 设置1-3 如果是内网设置1-2  如果是外网2-3
net.ipv4.tcp_synack_retries = 1

#全连接队列长度 超过长度会丢弃，可以设置大点
net.core.somaxconn = 65500

#收不到对端的FIN包 尽早放弃 设置1-3 如果是内网设置1-2  如果是外网2-3
net.ipv4.tcp_fin_timeout = 1

#限制TIME_WAIT状态数量 不能设置太大 可以几千到几万之间
net.ipv4.tcp_max_tw_buckets = 6000

#复用处于TIME_WAIT状态的连接 避免本机端口不足 必须设置为1
#允许作为客户端的新连接，在安全条件下使 用 TIME_WAIT 状态下的端口
net.ipv4.tcp_tw_reuse = 1

#要想使 tcp_tw_reuse 生效，还得把 timestamps 参数设置为 1，满足安全复用的先决条件（对方也要打开 tcp_timestamps ）
net.ipv4.tcp_timestamps = 1  

#新版本已废弃 会引起意料不到的问题，比如可能会引起NAT环境下的丢包问题 设置为0
#Linux 4.12 版本后，直接取消了这一参数 它并不要求 TIME_WAIT 状态存在 60 秒，很容易导致数据错乱，不建议设置为 1
net.ipv4.tcp_tw_recycle = 0 

#所有协议类型读的最大套接字缓冲大小 单位字节 为了支撑10Gb/s 设置为16MB或更高
net.core.rmem_max = 16777216

#所有协议类型写的最大套接字缓冲大小
net.core.wmem_max = 16777216

#启用TCP接收缓冲的自动调整
net.ipv4.tcp_moderate_rcvbuf = 1 

#为TCP读缓冲设置自动调优参数
net.ipv4.tcp_rmem = 4096	87380	16777216

#为TCP写缓冲设置自动调优参数
net.ipv4.tcp_wmem = 4096	65535	16777216  

#接收缓冲区调节时，判断空闲内存的多少 数值大有利于更多地利用内存 具体多少合适，不确定
net.ipv4.tcp_mem = 94500000 915000000 927000000

#以下两个，有没有用，还不确定
net.core.wmem_default = 8388608
net.core.rmem_default = 8388608

#增加每个cpu的网络设备积压队列长度 至少比tcp_max_syn_backlog大
net.core.netdev_max_backlog = 262144 

#sack和fack能在高延时的网络中提高吞吐性能，以一定的cpu资源为代价
net.ipv4.tcp_sack = 1
net.ipv4.tcp_fack = 1  

#打开tfo功能 减少握手的rtt 需要客户端和服务端同时支持，否则回退到传统的三次握手
net.ipv4.tcp_fastopen = 3

#Linux 系统为防止孤儿连接过多，导致系统资源长期被占用，就提供了 tcp_max_orphans 参数。如果孤儿连接数量大于它，新增的孤儿连接将不再走四次挥手，而是直接发送 RST 复位报文强制关闭。具体多少合适？还不清楚 
net.ipv4.tcp_max_orphans = 3276800

#内核会定时重发 FIN 报文，重发次数默认为0表示8次 
#不止对孤儿连接有效，对所有 FIN_WAIT1 状态下的连接都有效
net.ipv4.tcp_orphan_retries = 0

#扩充窗口大小 配置设为 1，此时窗口的最大值可以达到 1GB
net.ipv4.tcp_window_scaling = 1

#作为客户端的主机 扩大客户端的端口范围
net.ipv4.ip_local_port_range = 1024    65000

#默认7200秒 在某些需要快速检测连接断开的高可用性场景中，可能需要降低 
#设置 TCP 连接在多长时间的空闲状态下开始发送 keep-alive 探测包
net.ipv4.tcp_keepalive_time = 30

#当设置为 1 时，表示启用 IP 转发功能，即允许 Linux 系统将接收到的 IP 数据包转发到其他目的地。这通常用于配置路由器或 NAT（网络地址转换）设备。如果设置为 0，则表示禁止 IP 转发，这是大多数默认的 Linux 系统设置，以避免不必要的带宽使用和潜在的安全风险。
net.ipv4.ip_forward = 0

#用于设置反向路径过滤（Reverse Path Filtering，简称 RPF）的默认行为。RPF 是一种安全特性，用于防止 IP 欺骗攻击，确保数据包的源地址是可达的，并且数据包的反向路径是正确的。大多数 Linux 发行版将此参数设置为 1，以提供额外的安全保护。但是，在某些网络配置中，如使用非对称路由或复杂的网络拓扑时，可能需要将其设置为 2 或 0。
net.ipv4.conf.default.rp_filter = 1

#默认为0 出于安全考虑，大多数现代系统默认禁用接受源路由数据包，以避免潜在的路由攻击和 IP 欺骗。
net.ipv4.conf.default.accept_source_route = 0

#SysRq 是一个内建于 Linux 内核的调试工具，它允许用户通过特定的键盘组合键发送低级系统命令，帮助诊断和修复系统问题。为了确保系统安全，可能需要完全禁用 SysRq 功能。
kernel.sysrq = 0

#当设置为 1 时，生成的核心转储文件名将会是 core.PID，其中 PID 是崩溃进程的进程 ID。这有助于区分同一程序可能产生的多个核心转储文件。
kernel.core_uses_pid = 1

#消息队列中单个消息的最大字节数 默认为65536 字节
kernel.msgmnb = 65536

#消息队列中单个消息的最大字节数 默认8192
kernel.msgmax = 65536

#用于定义单个共享内存段的最大尺寸 通常设置为物理内存的一半 在64位系统上，最大值可以设置为物理内存大小减去1字节 下面这个设置已经超过了最大值 效果是什么，还不清楚
kernel.shmmax = 68719476736

#控制着系统可以分配的共享内存段的总页数 通常设置为系统的物理内存大小除以页的大小。例如，如果系统有 16GB 的内存，那么 kernel.shmall 可以设置为 16GB * 1024 * 1024 / 4KB = 4194304 页 下面这个设置已经超过了最大值 效果是什么，还不清楚
kernel.shmall = 4294967296

#用于设置系统中可以打开的文件描述符的最大数量 并不是一个参数就能单独决定 可以设置大一点 确保本参数不是瓶颈
fs.file-max = 1024000