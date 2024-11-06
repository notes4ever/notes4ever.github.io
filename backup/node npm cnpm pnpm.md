> **关于node与npm的配置，以linuxmint为示例**

```
sudo apt update
sudo apt install -y nodejs
sudo apt install -y npm
```

之前nodejs安装完成后npm就自带了，现在比较新的发行版貌似是要单独安装npm才行。

查看是否正确安装：

```
node -v
npm -v
```

临时指定registry，安装cnpm与pnpm：

```
sudo npm i -g cnpm --registry=https://registry.npmmirror.com/
sudo npm i -g pnpm --registry=https://registry.npmmirror.com/
```

nrm 是一个 npm 源管理器，允许你快速地在 npm 源间切换：

`sudo npm i nrm -g --registry=https://registry.npmmirror.com/`

n是node版本管理工具：

`sudo npm i n -g --registry=https://registry.npmmirror.com/`

更新n：

`n stable`

更新node：

`n latest
`
























