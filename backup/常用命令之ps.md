![image](https://github.com/user-attachments/assets/43b47427-f7ce-4248-af0a-79d02094ea4d)

> **ps命令与netstat类似，参数比较多，需要记住一些重要的，常用的**

## ps实践最常用法
### 查看所有进程
**ps -ef  和 ps -efl**（小写的l）

### 查看所有进程及其线程
**ps -efL** （大写的L）

### 查看某用户下的进程及其线程
**ps -u username -Lf**

### 查看特定进程的线程，结果包含进程本身
假设进程ID为1234：
**ps -mp 1234 -Lf**

### 查看特定进程的线程，结果不包含进程本身
假设进程ID为1234：
**ps  -p 1234 -Lf**

## 部分命令参数与输出结果解释
ps 命令堪比工具中的瑞士军刀，能够输出系统中运行的所有程序（进程）的大量信息。   
遗憾的是，ps有很高的复杂性：数不清的选项或许让 ps 命令成了最难掌握的命令。

﻿
![image](https://github.com/user-attachments/assets/d136a1cb-94db-46be-98c0-691e78b5a936)

![image](https://github.com/user-attachments/assets/49053f9d-d538-4f9b-b240-f89bf5bdcb2d)

**Note：** -l参数是长格式的输出，实际使用中，加不加-l，输出结果的数量也可能有略微不同。
-L则是显示进程里面的线程。

