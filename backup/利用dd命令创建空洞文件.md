
![image](https://github.com/user-attachments/assets/d9042e24-b949-47db-b4bc-1fd2ba84a77b)

> **利用dd命令创建文件，以及ls和du查看文件大小的区别。**

`dd if=/dev/zero bs=4M count=10 seek=20 of=testfile
`

一块4M

count 10块

seek 跳过20块 80M是空洞大小

ls -lh  显示文件长度，包含空洞

du -h 显示实际占用大小，跳过空洞

dd 这种类比虚拟机磁盘占用，动态扩容。