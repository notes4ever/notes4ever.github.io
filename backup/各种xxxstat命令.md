## dstat
dstat是一个新的性能工具，它吸收了 vmstat、iostat、ifstat 等几种工具的优点，可以同时观察系统的 CPU、磁盘 I/O、网络以及内存使用情况。

安装：

```
sudo apt/yum install dstat
```

## vmstat
vmstat 命令来自英文词组 virtual memory statistics 的缩写，其功能是监视系统资源状态。 可以使用 vmstat 查看系统中关于进程、内存、硬盘等资源的运行状态，但无法深入分析。vmstat 命令是一款轻量级的性能查看工具，不会给系统带来什么负担。

![image](https://github.com/user-attachments/assets/a2853f08-8135-4ed8-9a05-832d54ca98b7)

![image](https://github.com/user-attachments/assets/c207ae22-6e62-4ea7-8d68-2d4036c152cb)

![image](https://github.com/user-attachments/assets/bc00b882-caa0-4389-b791-0844abaf5cf5)

## iostat
监视IO设备使用情况：

![image](https://github.com/user-attachments/assets/9870c902-92b8-45ee-bb64-8990c4b69bc5)

## 综合对比
内容参考了kimi：

mpstat、pidstat、vmstat 和 iostat 都是 Linux 系统中用于监控性能的工具，它们各自关注不同的系统资源和性能指标。

- mpstat 用于监控**多核 CPU** 系统中各个 CPU 核心的性能。
- pidstat 用于监控**进程级别**的 CPU、内存、IO 和上下文切换等性能指标。
- vmstat 提供了**系统级别**的虚拟内存、进程、内存、IO 和 CPU 的综合统计信息。
- iostat 用于监控系统 **IO 设备**的性能，包括**磁盘 IO** 的统计数据。

这些工具可以单独使用，也可以组合使用，以获得更全面的系统性能视图。

### mpstat﻿ 与 ﻿vmstat﻿
关联：﻿

mpstat﻿ 是 ﻿sysstat﻿ 包中的一个工具，而 ﻿vmstat﻿ 是一个系统监控工具，它们都可以提供 CPU 使用率的信息。

区别：

﻿﻿mpstat﻿ 专注于 CPU，特别是多处理器系统中各个 CPU 核心的使用情况。它可以报告单个 CPU 核心的统计数据，也可以报告所有 CPU 核心的平均数据。

﻿﻿vmstat﻿ 提供了关于系统虚拟内存、进程、内存、IO 和 CPU 的统计信息。它更多地关注整体系统性能，而不是单个 CPU 核心。
﻿﻿pidstat﻿ 与 ﻿vmstat﻿

### pidstat 与 vmstat
关联：

pidstat 也是 sysstat 包中的一个工具，它可以提供进程级别的 CPU 和内存使用情况，而 vmstat 可以提供系统级别的内存使用情况。

区别：

pidstat 专注于进程，它可以显示特定进程或所有进程的 CPU、内存、IO 和上下文切换等详细统计信息。

vmstat 提供的是系统级别的统计信息，包括内存使用情况（如空闲、缓冲区、缓存等）、进程创建和退出、IO 统计等。

### iostat 与 vmstat
关联：

iostat 和 vmstat 都可以提供 IO 相关的统计信息。

区别：

iostat 专注于输入/输出设备负载、设备利用率和系统 IO 的统计信息。它可以报告物理和逻辑设备层面的 IO 性能数据。

vmstat 提供的是系统级别的 IO 统计信息，但它更侧重于整体的系统性能，包括内存、CPU 和 IO 的综合使用情况。