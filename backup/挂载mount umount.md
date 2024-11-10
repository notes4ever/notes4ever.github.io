## mount
![image](https://github.com/user-attachments/assets/5b3287bb-aa62-477a-a3de-b952e51898e4)

![image](https://github.com/user-attachments/assets/b404861d-caeb-43fa-9cc1-745c9226dd1e)

## umount
移除可移动设备时，不能直接将设备拔下，应该先卸载。

![image](https://github.com/user-attachments/assets/7c33f2aa-be13-4879-a5c0-5ee1cca7d7a8)

如果在卸载设备时，系统提示设备繁忙，无法卸载，那么通常是有进程还在访问该设备或使用该设备上的文件。这时可用 lsof 命令获得相关进程的信息，然后将进程终止。

lsof 命令的用法很简单：

lsof /path/to/device/node， 或者 lsof /path/to/mount/point。