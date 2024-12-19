Nginx的连接处理机制在不同的操作系统中会采用不同的I/O模型，在Linux下，Nginx使用**epoll**的I/O多路复用模型，在Freebsd中使用**kqueue**的I/O多路复用模型，在Solaris中使用/dev/poll方式的I/O多路复用模型，在Windows中使用的是**icop**，等等。

根据Nginx的官方文档建议，也可以不指定事件处理模型，**Nginx会自动选择最佳的事件处理模型服务**。