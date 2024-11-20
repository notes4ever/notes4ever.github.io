## perf Examples
Linux上的perf工具是一个强大的性能分析工具，它可以帮助开发者和系统管理员监控和分析系统性能。

最详尽的文档，直接查看文档说明：

https://www.brendangregg.com/perf.html

## 摘录一些用法

### 实时性能监控

```
perf top
```

这个命令会实时显示当前系统中占用CPU资源最多的函数或指令，类似于top命令，但是专注于性能事件。

### 统计程序性能

```
perf stat ls
````

这个命令会运行ls命令，并在执行前后显示这段时间内系统的性能统计数据，如CPU时间、上下文切换、缓存事件等。

### 记录性能数据

```
perf record -e cpu-cycles -g ./your_program
```

这个命令会记录./your_program运行时的CPU周期事件，并开启调用栈记录。

### 报告性能分析结果

```
perf report
```

这个命令会读取由perf record命令生成的性能数据文件，并生成易于理解的性能报告 。

### 分析特定进程的性能

```
perf stat -p <PID>
```

用-p选项指定进程ID，可以分析特定进程的性能概况 。

### 生成火焰图

```
perf record -F 99 -a -g -- sleep 60
perf script > out.perf
./stackcollapse-perf.pl out.perf > out.folded
./flamegraph.pl out.folded > flamegraph.svg
```

这一系列命令会生成一个火焰图，它可以帮助可视化程序中的性能热点 。

### 分析内核锁的性能

```
perf lock record
perf lock report
```

这两个命令分别用于记录和报告内核锁的性能，包括锁的等待时间和争用情况 。

### 分析内核内存分配

```
perf kmem record --page ls
perf kmem stat --page
```

使用perf kmem可以分析内核slab分配器的性能，包括内存分配和释放事件 。

### 自定义动态跟踪点

```
perf probe --line schedule
perf probe -a schedule:12
```

使用perf probe可以定义新的动态跟踪点，例如在schedule函数的第12行添加一个探测点 。