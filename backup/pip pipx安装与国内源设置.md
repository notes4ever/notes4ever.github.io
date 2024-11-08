## pip设置清华源

`pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple`

linux与windows通用。

## linuxmint设置
linuxmint默认就能用python3命令，但不能使用python命令。

安装pip3：

sudo apt install python3-pip

可以只用python3和pip3。

## pipx安装

python12以后是推荐使用pipx。

sudo apt install pipx

pipx使用介绍：

https://zhuanlan.zhihu.com/p/637791135

更好的方式是使用虚拟环境，miniconda。