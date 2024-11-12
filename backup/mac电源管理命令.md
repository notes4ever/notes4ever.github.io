电源管理设置，查看当前配置：

sudo pmset -g

禁止网络接口休眠：

sudo pmset -a sleep 0

这个命令会禁用所有网络接口的节能模式，-a 参数表示对所有电源模式生效。

恢复电源默认设置：

sudo pmset restore

系统配置文件：

/Library/Preferences/SystemConfiguration/com.apple.PowerManagement.plist

参数解释：

https://zhuanlan.zhihu.com/p/159468988