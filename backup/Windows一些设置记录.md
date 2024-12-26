## 关闭Chrome浏览器自动更新
参考：https://blog.csdn.net/weixin_48337566/article/details/123242827

## Windows终端中使用conda
安装windows terminator，使用现代化的cmd。通过terminator启动powershell，最好将powershell先升级。执行：
```
Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy Unrestricted
```

然后执行初始化:
```
conda init --all
```

可能需要多次执行，在不同界面中。

参考：https://blog.csdn.net/qq_27386899/article/details/131273618

## windows刷新dns
```
ipconfig /flushdns
```

## Windows上GIT配置文件的位置
```
git config --list --show-origin
```

参考：https://blog.csdn.net/yetyongjin/article/details/130887293

## win10默认系统字体更改
以管理员身份运行命令提示符regedit，打开注册表，定位到`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsNT\CurrentVersion\Fonts`
找到对自己要改为的满意的字体右键——修改——复制数值数据，在找到Microsoft YaHei & Microsoft YaHei UI (TrueType)把后面的msyh.ttc改为你想要的字体。注意：只更改数值数据，数值名称不要动，改完后退出注册表，重启。

## chrome受管理托管状态
查看您的单位在您浏览器中设置的扩展程序，请点击 Chrome 菜单中的由贵单位管理。

您也可在地址栏中输入 chrome://management，然后按 Enter 键。

如果您的浏览器处于托管状态，您可从中找到贵单位设定的相关政策：在地址栏中输入 chrome://policy，然后按 Enter 键。

Windows下如何删除 Chrome的被托管 ？

Windows系统按WIN+R输入“RegEdit”运行注册表管理，删除注册表`\HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Google\`下对应的文件夹（删除整个Chrome目录）。删除后重启浏览器。

可以去掉设置菜单的“由贵单位管理”，但安全dns还是无法设置，显示受管理，设置被停用。

本质为管理员策略所控制，很可能无法根治。尤其是办公电脑中，有安全管控软件和策略。

## windows上npm配置
```
npm config set prefix "C:\Program Files\nodejs\node_global"　　//修改 npm 的全局安装模块路径
npm config set cache "C:\Program Files\nodejs\node_cache"　　　//修改 npm 的缓存路径
```

##  API测试 reqable
官网：https://reqable.com/zh-CN/download

Reqable是什么?

Reqable = Fiddler + Charles + Postman

Reqable拥有极简的设计、丰富的功能、高效的性能和桌面手机双端平台。

先进API生产力工具，新一代API调试 + API测试一站化解决方案。全平台、免登录、轻量级、高性能、无广告，让API更快更简单。基于经典的MITM中间人代理方案捕获和分析您的应用流量，自适应HTTP/HTTPS/SOCKS4/SOCKS5等多种代理协议，并全面支持HTTP2协议，助您完整还原BUG现场。支持多种过滤器，例如应用、域名、数据类型、书签以及多条件自定义搜索，帮您在茫茫数据中准确找到想要查找的内容。
