```
sudo yum groupinstall "Development Tools"
sudo yum install openssl-devel bzip2-devel zlib-devel readline-devel sqlite-devel xz-devel libffi-devel
```

[官网](https://www.python.org/downloads/)下载python源码：

python3124.tar.xz

解压编译：


```
tar -xf python3124.tar.xz
cd python3124
./configure --prefix=/app/usr/python3
make 
make install
```

加入环境变量：

`export PATH=/app/usr/python3/bin:$PATH`

生效：

`source ~/.bashrc `

软链接python3和pip3：

```
ln -s python3 python
ln -s pip3 pip
```