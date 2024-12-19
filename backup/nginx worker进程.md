类比：Nginx服务就相当于饭店，网站用户就相当于顾客，Nginx的进程就相当于服务员，饭店要根据客户的流量及并发量来调整接待的服务人员数量，然后根据顾客量变化的监测结果及时调整到最佳的配置。

搭建服务器时，worker进程数最开始的设置可以等于CPU的核数；**高流量高并发场合也可以考虑将进程数提高至CPU核数 ×2**，具体情况要根据实际的业务来选择，因为这个参数除了要和CPU核数匹配之外，也与硬盘存储的数据及系统的负载有关，设置为CPU的核数是一个好的起始配置，这也是官方的建议。

优化绑定不同的Nginx进程到不同的CPU上：
默认就是比较平均的，可能是Nginx软件自身正在逐渐优化，使其使用多核CPU时更为均衡。

**单个进程允许的客户端最大连接数，控制连接数的参数为worker_connections**。 

worker_connections的值要根据具体服务器的性能和程序的内存使用量来指定（单个进程启动使用的内存根据程序来确定）。

在linuxmint虚拟机上，默认给定了配置是：
```
worker_processes auto;
worker_connections 768;
```

worker_connections用来设置一个worker process支持的最大并发连接数，这个连接数包括了所有连接，例如：代理服务器的 
连接、客户端的连接等，实际的并发连接数除了受worker_connections参数控制之外，还与nginx另一个参数：最大打开文件数worker_rlimit_nofile有关，Nginx总并发连接=worker数量×worker_connections。调整配置Nginx worker进程的最大打开文件数，这个控制连接数的参数为worker_rlimit_nofile，一般可以设置为优化后的ulimit -n的结果。