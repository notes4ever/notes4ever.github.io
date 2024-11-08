![image](https://github.com/user-attachments/assets/1f7b7926-381d-4998-a57c-cb042b35d33b)

> **centos 9 国内源设置**

## centos中科大源设置

yum源目录：/etc/yum.repos.d
目录下所有的*.repo文件都移动到bak，用不到暂时备份。
只需要保留Centos.repo一个源文件即可。

文件内容：

```
[baseos]
name=CentOS Stream $releasever - BaseOS
baseurl=https://mirrors.ustc.edu.cn/centos-stream/9-stream/BaseOS/$basearch/os/
gpgcheck=1
enabled=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-centosofficial
[appstream]
name=CentOS Stream $releasever - AppStream
baseurl=https://mirrors.ustc.edu.cn/centos-stream/9-stream/AppStream/$basearch/os/
gpgcheck=1
enabled=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-centosofficial
```

更新缓存：

```
dnf clean all
dnf makecache
dnf update
```

阿里源参考：

https://www.cnblogs.com/LandWind/p/centos-stream-9-repo-set-aliyun.html

## centos9启用epel

```
sudo dnf config-manager --set-enabled crb
sudo dnf install epel-release epel-next-release
```

参考：https://www.redhat.com/sysadmin/install-epel-linux

### epel.repo

```
[epel]
name=Extra Packages for Enterprise Linux $releasever - $basearch
# It is much more secure to use the metalink, but if you wish to use a local mirror
# place its address here.
#baseurl=https://download.example/pub/epel/$releasever/Everything/$basearch/
metalink=https://mirrors.fedoraproject.org/metalink?repo=epel-$releasever&arch=$basearch&infra=$infra&content=$contentdir
enabled=1
gpgcheck=1
countme=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-$releasever

[epel-debuginfo]
name=Extra Packages for Enterprise Linux $releasever - $basearch - Debug
# It is much more secure to use the metalink, but if you wish to use a local mirror
# place its address here.
#baseurl=https://download.example/pub/epel/$releasever/Everything/$basearch/debug/
metalink=https://mirrors.fedoraproject.org/metalink?repo=epel-debug-$releasever&arch=$basearch&infra=$infra&content=$contentdir
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-$releasever
gpgcheck=1

[epel-source]
name=Extra Packages for Enterprise Linux $releasever - $basearch - Source
# It is much more secure to use the metalink, but if you wish to use a local mirror
# place its address here.
#baseurl=https://download.example/pub/epel/$releasever/Everything/source/tree/
metalink=https://mirrors.fedoraproject.org/metalink?repo=epel-source-$releasever&arch=$basearch&infra=$infra&content=$contentdir
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-$releasever
gpgcheck=1
```

### epel-next.repo

```
[epel-next]
name=Extra Packages for Enterprise Linux $releasever - Next - $basearch
# It is much more secure to use the metalink, but if you wish to use a local mirror
# place its address here.
#baseurl=https://download.example/pub/epel/next/$releasever/Everything/$basearch/
metalink=https://mirrors.fedoraproject.org/metalink?repo=epel-next-$releasever&arch=$basearch&infra=$infra&content=$contentdir
enabled=1
gpgcheck=1
countme=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-$releasever

[epel-next-debuginfo]
name=Extra Packages for Enterprise Linux $releasever - Next - $basearch - Debug
# It is much more secure to use the metalink, but if you wish to use a local mirror
# place its address here.
#baseurl=https://download.example/pub/epel/next/$releasever/Everything/$basearch/debug/
metalink=https://mirrors.fedoraproject.org/metalink?repo=epel-next-debug-$releasever&arch=$basearch&infra=$infra&content=$contentdir
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-$releasever
gpgcheck=1

[epel-next-source]
name=Extra Packages for Enterprise Linux $releasever - Next - $basearch - Source
# It is much more secure to use the metalink, but if you wish to use a local mirror
# place its address here.
#baseurl=https://download.example/pub/epel/next/$releasever/Everything/source/tree/
metalink=https://mirrors.fedoraproject.org/metalink?repo=epel-next-source-$releasever&arch=$basearch&infra=$infra&content=$contentdir
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-$releasever
gpgcheck=1
```