> **sudo 执行命令的时候配置免输入密码，此方式并不推荐，只适合懒人和并不知道某用户密码的场景。以centos为示例。**

通过编辑修改/etc/sudoers实现，**需要root权限**。

配置某用户sudo（全局，不安全，慎用）不用输入密码：

username    ALL=(ALL)       NOPASSWD: **ALL**

配置某用户特定权限组（特定类型的命令）不用输入密码：

username    ALL=(ALL)       NOPASSWD: **NETWORKING, SOFTWARE, SERVICES, TOOLS, PROCESSES**

配置某用户特定命令不用输入密码：

username    ALL=(ALL)	NOPASSWD: **/usr/sbin/nginx**
