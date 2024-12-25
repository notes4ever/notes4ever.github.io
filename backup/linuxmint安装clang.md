linuxmint下安装，直接使用apt：
```
sudo apt update && sudo apt upgrade
sudo apt install clang
```

验证是否成功，查看版本：
```
clang --version
```

使用：
```
clang hello.c -o hello
```

参考：https://zhuanlan.zhihu.com/p/647388977