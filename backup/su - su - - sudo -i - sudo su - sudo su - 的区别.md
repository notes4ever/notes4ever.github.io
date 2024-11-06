> **关于su和sudo的命令很容易记错用错，在此总结。**

## 关于su
[su username]表示切换到username用户模式，需要输入该用户密码。如果是root用户使用该命令，则不需要输入密码，因为root权限最高。

[su]只输入su后面不加账户名称时，系统默认切换到root账户，密码也为root的密码。没有时间限制。

[su -]表示申请切换root用户，需要申请root用户密码。有些Linux发行版，例如ubuntu，默认没有设置root用户的密码，所以需要我们先使用sudo passwd root设置root用户密码。

[su]和[su -]区别：

[su]只是切换了root身份，但Shell环境仍然是普通用户的Shell；而[su -]连用户和Shell环境一起切换成root身份了。只有切换了Shell环境才不会出现PATH环境变量错误，报command not found的错误。

[su]切换成root用户以后，pwd一下，发现工作目录仍然是普通用户的工作目录；而用[su -]命令切换以后，工作目录变成root的工作目录了。

用echo $PATH命令看一下su和su - 后的环境变量已经变了。

## su与sudo的区别
su密码输入的是root的密码，sudo密码输入是当前用户的密码。

[sudo su] 切换root身份，不携带当前用户环境变量。

[sudo su -]切换root身份，携带当前用户环境变量。

带-切换以后，当前目录是/root，不带-切换以后pwd是原来用户的~目录。

**sudo -i 等于 sudo su -**

## 参考资料

- https://blog.csdn.net/mutou990/article/details/107724302
- https://zhuanlan.zhihu.com/p/252345791