## 使用termux  
官网：https://termux.dev/en/

### 在termux上安装proot-distro
```
pkg update
pkg install proot-distro
```
### 在termux上安装linux发行版
查看可用的linux发行版：

`proot-distro list`

安装发行版：
```
proot-distro install debian
proot-distro install archlinux
```

### 登录发行版
```
proot-distro login debian
proot-distro login archlinux
```

## 使用UserLAnd
官网：https://userland.tech/

直接使用图形化安装和使用，体验下来，感觉比termux更方便好用。