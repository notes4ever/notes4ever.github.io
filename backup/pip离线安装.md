以apscheduler为例：

pip download apscheduler -d /media/sf_share/apscheduler

将 /media/sf_share/apscheduler目录打包，上传到需要离线安装的机器。

在离线机器上，使用以下命令安装包：

解压出来：/home/username/apscheduler

pip install --no-index --find-links=/home/username/apscheduler  apscheduler

这里的 /home/ht/apscheduler是你存放下载的包的离线目录。

备注：

- 确保下载的包与离线环境中的 Python 版本兼容。
- 如果有多个依赖包，可以一次性下载所有依赖包，或者单独下载每个包。
- 使用 --no-index 选项告诉 pip 不在互联网上查找包。
- 使用 --find-links 指定包含下载的包的本地目录。
- 如果需要安装wheel格式的包，请确保离线环境中已经安装了wheel。